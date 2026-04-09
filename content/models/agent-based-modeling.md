---
title: "Агентное моделирование в токеномике"
description: "Когда табличная модель не справляется: агентное моделирование для стресс-тестирования токеномики. Код на Python, три рабочих примера, сравнение подходов."
date: 2026-04-09
weight: 20
tags:
  - токеномика
  - моделирование
  - симуляция
  - агентное моделирование
  - cadCAD
  - python
categories:
  - Модели
authors:
  - Алексей Каранюк
---

## Проблема: таблица считает средние, а система ломается на крайностях

Табличная модель токеномики работает с агрегатами: общий supply, средняя цена, суммарный стейкинг. Она не моделирует поведение отдельных участников. А именно поведение участников определяет, выживет система или нет.

Агентное моделирование (agent-based modeling, ABM) строит систему снизу вверх: каждый участник — отдельный агент со своим балансом, стратегией и порогами принятия решений. Модель запускается на сотни прогонов, и результат — не один прогноз, а распределение возможных исходов.

Дальше — три рабочих примера на Python, которые можно запустить в Google Colab.

## Пример 1. Симуляция AMM-пула

Простейший случай: пул Uniswap V2 с инвариантом k = x × y. На каждом шаге случайный агент делает своп — добавляет один из токенов в пул. Вопрос: как ведёт себя цена на 1000 шагах?

```python
import numpy as np
import matplotlib.pyplot as plt

x_initial = 1000  # начальный резерв токена X
y_initial = 1000  # начальный резерв токена Y
num_periods = 1000

x = np.zeros(num_periods)
y = np.zeros(num_periods)
price = np.zeros(num_periods)
k = np.zeros(num_periods)

x[0], y[0] = x_initial, y_initial
price[0] = x[0] / y[0]
k[0] = x[0] * y[0]

np.random.seed(42)

for t in range(1, num_periods):
    if np.random.choice([True, False]):
        # агент продаёт X → пул получает X, отдаёт Y
        dx = np.random.normal(0, 100)
        x[t] = x[t-1] + dx
        y[t] = k[0] / x[t]  # инвариант k = x * y
    else:
        # агент продаёт Y → пул получает Y, отдаёт X
        dy = np.random.binomial(1, 0.5) * np.random.normal(0, 100)
        y[t] = y[t-1] + dy
        x[t] = k[0] / y[t]

    price[t] = x[t] / y[t]
    k[t] = x[t] * y[t]
```

Результат — цена блуждает случайно, но инвариант k сохраняется:

```python
fig, axes = plt.subplots(2, 1, figsize=(12, 8))

axes[0].plot(price, linewidth=0.8)
axes[0].set_ylabel('Цена (x/y)')
axes[0].set_title('Цена токена: 1000 шагов случайных свопов')
axes[0].axhline(y=1, color='red', linestyle='--', alpha=0.5, label='начальная цена')
axes[0].legend()
axes[0].grid(True, alpha=0.3)

axes[1].plot(x, label='Резерв X', linewidth=0.8)
axes[1].plot(y, label='Резерв Y', linewidth=0.8)
axes[1].set_ylabel('Количество токенов')
axes[1].set_xlabel('Период')
axes[1].set_title('Динамика резервов пула')
axes[1].legend()
axes[1].grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

{{< callout title="Что это показывает" >}}
Табличная модель дала бы вам одну линию: «цена растёт на 2% в месяц». Симуляция показывает реальность — случайное блуждание с проскальзыванием. На 1000 шагах цена может уйти в 5x от начальной и вернуться обратно. Это принципиально другая картина для проектирования ликвидности.
{{< /callout >}}

## Пример 2. Сравнение двух формул AMM

Не все AMM одинаковые. Сравним поведение стандартного Uniswap V2 (k = x × y) с полиномиальным инвариантом (x³y + y³x = k) на одних и тех же случайных данных:

```python
num_periods = 1000
x1, y1 = np.zeros(num_periods), np.zeros(num_periods)  # Uniswap: k = xy
x2, y2 = np.zeros(num_periods), np.zeros(num_periods)  # Полиномиальный: x³y + y³x = k

x1[0] = y1[0] = x2[0] = y2[0] = 1000
k_uni = x1[0] * y1[0]  # 1_000_000
k_poly = x2[0]**3 * y2[0] + y2[0]**3 * x2[0]  # 2_000_000_000_000

np.random.seed(42)

for t in range(1, num_periods):
    change_x = np.random.choice([True, False])

    if change_x:
        dx = np.random.normal(0, 100)
        # AMM1: k = x * y  →  y = k / x
        x1[t] = x1[t-1] + dx
        y1[t] = k_uni / x1[t]
        # AMM2: x³y + y³x = k  →  решаем кубическое уравнение
        x2[t] = x2[t-1] + dx
        # y³ * x2[t] + y * x2[t]³ = k_poly
        # Это кубическое уравнение относительно y, решаем численно
        coeffs = [x2[t], 0, x2[t]**3, -k_poly]
        roots = np.roots(coeffs)
        real_positive = [r.real for r in roots if np.isreal(r) and r.real > 0]
        y2[t] = min(real_positive) if real_positive else y2[t-1]
    else:
        dy = np.random.binomial(1, 0.5) * np.random.normal(0, 100)
        y1[t] = y1[t-1] + dy
        x1[t] = k_uni / y1[t]
        y2[t] = y2[t-1] + dy
        coeffs = [y2[t], 0, y2[t]**3, -k_poly]
        roots = np.roots(coeffs)
        real_positive = [r.real for r in roots if np.isreal(r) and r.real > 0]
        x2[t] = min(real_positive) if real_positive else x2[t-1]

price1 = x1 / y1
price2 = x2 / y2

plt.figure(figsize=(12, 5))
plt.plot(price1, label='Uniswap V2: k = x·y', linewidth=0.8, alpha=0.8)
plt.plot(price2, label='Полиномиальный: x³y + y³x = k', linewidth=0.8, alpha=0.8)
plt.ylabel('Цена (x/y)')
plt.xlabel('Период')
plt.title('Сравнение AMM: одинаковые свопы, разные инварианты')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

Полиномиальный инвариант сглаживает колебания цены вблизи равновесия (x ≈ y), но усиливает их на экстремумах. Это важно для проектирования стейблкоин-пулов — Curve использует похожий подход.

## Пример 3. Стейкинг и банк-ран

Ключевой пример: почему ABM нужен для систем со стейкингом. 200 агентов стейкают токены. У каждого свой порог APR, ниже которого он выходит. Когда крупный участник выходит — APR падает → следующая волна выходов.

```python
import numpy as np
import matplotlib.pyplot as plt

np.random.seed(42)

# --- Параметры системы ---
num_agents = 200
num_steps = 365          # дней
total_supply = 10_000_000
daily_rewards = 2_740    # токенов в день (~10% годовых при полном стейке)

# --- Генерация агентов ---
# Распределение балансов по степенному закону (как в реальных сетях)
raw = np.random.pareto(a=1.5, size=num_agents) + 1
balances = (raw / raw.sum() * total_supply * 0.6).astype(int)  # 60% supply стейкается

# Порог APR, ниже которого агент выходит (от 3% до 15%)
apr_thresholds = np.random.uniform(0.03, 0.15, size=num_agents)

# Задержка реакции: не все выходят мгновенно (1–14 дней)
reaction_delay = np.random.randint(1, 15, size=num_agents)

# Состояние: True = стейкает
is_staking = np.ones(num_agents, dtype=bool)
days_below_threshold = np.zeros(num_agents, dtype=int)

# --- Запись метрик ---
history_staked = np.zeros(num_steps)
history_apr = np.zeros(num_steps)
history_num_stakers = np.zeros(num_steps)

# --- Симуляция ---
for day in range(num_steps):
    staked_total = balances[is_staking].sum()
    if staked_total == 0:
        break

    current_apr = (daily_rewards * 365) / staked_total

    history_staked[day] = staked_total
    history_apr[day] = current_apr
    history_num_stakers[day] = is_staking.sum()

    # Каждый агент проверяет: APR ниже моего порога?
    for i in range(num_agents):
        if not is_staking[i]:
            continue
        if current_apr < apr_thresholds[i]:
            days_below_threshold[i] += 1
            if days_below_threshold[i] >= reaction_delay[i]:
                is_staking[i] = False  # выход из стейкинга
        else:
            days_below_threshold[i] = 0

# --- Визуализация ---
fig, axes = plt.subplots(3, 1, figsize=(12, 10), sharex=True)

axes[0].plot(history_apr[:day] * 100, linewidth=1, color='#2ecc71')
axes[0].axhline(y=10, color='red', linestyle='--', alpha=0.5, label='начальный APR')
axes[0].set_ylabel('APR, %')
axes[0].set_title('Каскадный выход из стейкинга (банк-ран)')
axes[0].legend()
axes[0].grid(True, alpha=0.3)

axes[1].plot(history_staked[:day] / 1e6, linewidth=1, color='#3498db')
axes[1].set_ylabel('Стейк, млн токенов')
axes[1].grid(True, alpha=0.3)

axes[2].plot(history_num_stakers[:day], linewidth=1, color='#e74c3c')
axes[2].set_ylabel('Количество стейкеров')
axes[2].set_xlabel('День')
axes[2].grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# --- Статистика ---
print(f"Начальный стейк: {balances.sum():,} токенов ({balances.sum()/total_supply:.0%} supply)")
print(f"Начальный APR: {(daily_rewards * 365) / balances.sum() * 100:.1f}%")
print(f"Финальный стейк: {history_staked[day-1]:,.0f} токенов")
print(f"Финальный APR: {history_apr[day-1] * 100:.1f}%")
print(f"Выбыло стейкеров: {num_agents - int(history_num_stakers[day-1])}")
```

### Что здесь происходит

Ключевые параметры модели:

| Параметр | Значение | Зачем |
|----------|----------|-------|
| `num_agents` | 200 | Количество стейкеров |
| `daily_rewards` | 2 740 токенов | Фиксированная эмиссия (~10% годовых при полном стейке) |
| `a=1.5` (Парето) | Степенной закон | Распределение балансов: несколько китов + много мелких |
| `apr_thresholds` | 3–15% | У каждого агента свой минимальный APR |
| `reaction_delay` | 1–14 дней | Не все реагируют мгновенно |

Механизм банк-рана:

1. Начальный APR ~10% — все довольны
2. Агенты с порогом >10% начинают выходить (их мало, но они есть)
3. Стейк уменьшается → APR растёт (rewards / меньший стейк = выше APR)
4. Система стабилизируется на новом равновесии

Но если добавить **внешний шок** — падение цены токена, при котором киты выходят первыми:

```python
# Добавляем шок на 90-й день: топ-5 стейкеров выходят
shock_day = 90
top5 = np.argsort(balances)[-5:]

for day in range(num_steps):
    # ... основной цикл тот же ...

    # Шок: киты выходят
    if day == shock_day:
        for whale in top5:
            is_staking[whale] = False

    # ... остальная логика ...
```

Теперь каскад: киты уходят → APR для остальных растёт, но если цена падает, то реальная доходность в долларах снижается → следующие агенты уходят → давление на продажу → цена падает ещё → самоусиливающийся цикл.

Табличная модель показала бы: «при выходе 20% стейкеров APR вырастет до 12,5%». Агентная модель показывает: **кто именно** выходит, **когда** и **что это запускает**.

## Пример 4. Многоагентная оптимизация портфеля

Более сложный случай: агенты с разным уровнем богатства оптимизируют портфель из трёх токенов через AMM. Каждый агент максимизирует свою функцию полезности Кобба-Дугласа.

```python
from scipy.optimize import minimize

class Agent:
    def __init__(self, richness):
        self.richness = richness  # 0..1, степень богатства
        # портфель: [доллары, токен A, токен B]
        self.portfolio = np.array([0.0, 0.0, 0.0])
        # коэффициенты полезности (у каждого агента свои)
        self.utility_weights = np.random.gamma(1.5, 1.0, size=3)
        self.utility_weights = np.clip(self.utility_weights, 0.1, 10.0)

    def income(self):
        """Доход за период: богатые получают больше"""
        base = np.array([100_000, 1_000_000, 1.0])
        noise = np.random.uniform(0.95, 1.05, size=3)
        return self.richness * base * noise

    def utility(self, portfolio):
        """Функция полезности Кобба-Дугласа"""
        return sum(w * np.log(max(p, 1e-10))
                   for w, p in zip(self.utility_weights, portfolio))

class AMM:
    def __init__(self, reserves):
        self.reserves = np.array(reserves, dtype=float)
        self.k = np.prod(self.reserves)  # x * y * z = k

    def swap(self, token_in, token_out, amount_in):
        """Своп через CPMM: сколько token_out получим за amount_in token_in"""
        new_in = self.reserves[token_in] + amount_in
        new_out = self.k / (new_in * self.reserves[3 - token_in - token_out])
        amount_out = self.reserves[token_out] - new_out

        if amount_out <= 0 or new_out <= 0:
            return 0

        self.reserves[token_in] = new_in
        self.reserves[token_out] = new_out
        return amount_out

    def price(self, token_a, token_b):
        return self.reserves[token_b] / self.reserves[token_a]

# --- Запуск ---
num_epochs = 30
agents = []
amm = AMM([1_000_000, 10_000_000, 100_000])  # доллары, токен A, токен B

prices_01 = []  # цена токена A в долларах
prices_02 = []  # цена токена B в долларах

for epoch in range(num_epochs):
    # Новый агент каждую эпоху
    richness = np.random.lognormal(-2.3, 1.0)
    richness = min(richness, 1.0)
    agents.append(Agent(richness))

    # Все агенты получают доход
    for agent in agents:
        agent.portfolio += agent.income()

    # Агенты торгуют на AMM (оптимизируют полезность)
    for agent in agents:
        best_utility = agent.utility(agent.portfolio)

        for token_in in range(3):
            for token_out in range(3):
                if token_in == token_out:
                    continue
                if agent.portfolio[token_in] < 1:
                    continue

                # Сколько продать, чтобы максимизировать полезность?
                max_sell = agent.portfolio[token_in] * 0.5  # не более 50% позиции

                def neg_utility(amount):
                    test_portfolio = agent.portfolio.copy()
                    test_portfolio[token_in] -= amount[0]
                    received = amount[0] * amm.reserves[token_out] / (amm.reserves[token_in] + amount[0])
                    test_portfolio[token_out] += received
                    return -agent.utility(test_portfolio)

                result = minimize(neg_utility, [max_sell * 0.1],
                                  bounds=[(0, max_sell)], method='L-BFGS-B')

                if result.success and -result.fun > best_utility:
                    optimal_amount = result.x[0]
                    received = amm.swap(token_in, token_out, optimal_amount)
                    if received > 0:
                        agent.portfolio[token_in] -= optimal_amount
                        agent.portfolio[token_out] += received
                        best_utility = agent.utility(agent.portfolio)

    prices_01.append(amm.price(0, 1))
    prices_02.append(amm.price(0, 2))

# --- Визуализация ---
fig, ax = plt.subplots(figsize=(12, 5))
ax.plot(prices_01, label='Токен A / доллар', linewidth=1.5)
ax.plot(prices_02, label='Токен B / доллар', linewidth=1.5)
ax.set_xlabel('Эпоха')
ax.set_ylabel('Цена')
ax.set_title('Динамика цен: 30 агентов с разным богатством торгуют на AMM')
ax.legend()
ax.grid(True, alpha=0.3)
plt.show()
```

Каждый агент решает задачу оптимизации: при каких свопах его полезность максимальна? Результат — эмерджентное ценообразование, которое невозможно получить из формулы.

## Табличная модель vs агентная симуляция

| Параметр | Табличная модель | Агентное моделирование |
|----------|------------------|----------------------|
| Единица анализа | Совокупные показатели | Отдельные участники |
| Результат | Один сценарий | Распределение исходов |
| Каскадные эффекты | Не видны | Ядро модели |
| Сложность | Часы | Дни |
| Когда нужна | Allocation, вестинг, unit-экономика | Стейкинг, governance, AMM, многосторонние рынки |

## Инструменты

**cadCAD** — Python-фреймворк от BlockScience. Формализует модель как цепочку «состояние → политика → механизм обновления». Использовался для моделирования Ethereum 2.0, Filecoin, Ocean Protocol.

**radCAD** — упрощённый API поверх cadCAD. Быстрый старт, меньше шаблонного кода.

**TokenSPICE** — симулятор от Ocean Protocol. Работает с реальными EVM-контрактами через форк блокчейна. Тестирует не абстракцию, а конкретный Solidity-код.

**Чистый Python** (NumPy + SciPy) — для моделей, где важна гибкость. Все примеры в этой статье написаны без внешних фреймворков.

{{< checklist title="Чек-лист: нужна ли агентная модель" >}}
- В системе есть участники с конкурирующими интересами
- Действия одного участника влияют на условия для других
- Есть механизмы с обратной связью (стейкинг, AMM, bonding curve)
- Важно понять устойчивость к экстремальным сценариям
- Распределение позиций сильно неравномерное (есть киты)
- Система включает governance с голосованием
{{< /checklist >}}

3+ пунктов — агентная модель окупит вложения. 0–1 — начните с [табличной модели](/models/token-supply-models/).

{{< cta title="Нужна агентная модель для вашего проекта?" text="Мы проектируем токеномику и проверяем её симуляциями — от табличных моделей до агентного моделирования" button="Обсудить проект" link="https://t.me/karanyuk" >}}
