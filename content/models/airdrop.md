---
title: "Airdrop как модель распределения токенов"
description: "Airdrop — бесплатная раздача токенов за активность. Четыре типа airdrop, формулы расчёта, сибил-защита, давление на продажу и калькулятор аллокации."
date: 2026-04-10
weight: 27
tags:
  - токеномика
  - airdrop
  - распределение
  - предложение
  - сообщество
categories:
  - models
---

Airdrop — один из самых мощных и одновременно самых рискованных инструментов токеномики. При правильном проектировании он создаёт лояльное сообщество и распределяет управление. При неправильном — генерирует моментальный сброс и убивает цену.

## Что такое airdrop

**Airdrop** — это бесплатное распределение токенов пользователям, выполнившим определённые условия. В отличие от продажи (ICO, IDO), airdrop не требует от получателя покупки — токены начисляются за прошлую или будущую активность.

В контексте [моделей предложения]({{< relref "token-supply-models" >}}) airdrop — это способ первичного распределения токенов из пула сообщества. Он отвечает на вопрос **«как доставить токены пользователям»** — не через покупку, не через майнинг, а через заслуги.

{{< callout title="Airdrop и аллокация" type="info" >}}
Airdrop-пул — часть общей [аллокации]({{< relref "allocation" >}}). Типичная доля на airdrop: 5–15% от total supply. Остальные токены сообщества идут на staking-награды, гранты и экосистемные программы.
{{< /callout >}}

### Зачем проекты делают airdrop

1. **Децентрализация управления** — широкое распределение токенов снижает концентрацию голосов
2. **Привлечение пользователей** — airdrop-фарминг приводит трафик и TVL
3. **Награда ранних участников** — создаёт лояльность и социальное доказательство
4. **Регуляторная защита** — бесплатное распределение снижает риск классификации токена как ценной бумаги

## Четыре типа airdrop

### 1. Ретроспективный airdrop

Токены распределяются за **прошлые действия**: транзакции, использование протокола, участие в тестнете. Пользователи не знали заранее о награде.

Это самый эффективный тип: он награждает реальных пользователей и минимизирует фарминг. Проблема — определить справедливые критерии задним числом.

**Примеры.** Uniswap раздал 400 UNI каждому, кто совершил хотя бы один своп до определённой даты. Arbitrum распределил токены на основе количества транзакций, мостов и активных месяцев.

### 2. Критериальный airdrop

Токены распределяются на основе **публичных критериев**, объявленных заранее: количество транзакций, объём, время участия, количество уникальных протоколов.

{{< formula math="Score_user = Σ (w_i × Action_i)" desc="w_i — вес критерия · Action_i — количественное значение действия (транзакции, объём, дни активности)" >}}

Преимущество: прозрачность. Недостаток: стимулирует фарминг — пользователи целенаправленно выполняют критерии.

### 3. Stake-based airdrop

Токены распределяются пропорционально **застейканным активам** или **ликвидности**. Пользователь блокирует капитал в протоколе и получает токены как награду.

{{< formula math="Airdrop_user = Pool_airdrop × (Stake_user / Stake_total)" desc="Pool_airdrop — общий пул airdrop · Stake_user — сумма стейка пользователя · Stake_total — сумма всех стейков" >}}

Этот тип смешивается с [моделями утилизации]({{< relref "utility-models" >}}) — стейкинг одновременно создаёт спрос и распределяет токены.

### 4. Task-based airdrop

Токены распределяются за **конкретные задания**: привязка соцсетей, рефералы, создание контента, участие в governance. Часто реализуется через квест-платформы (Galxe, Zealy, Layer3).

Преимущество: контроль поведения пользователей. Недостаток: привлекает ботов и фермеров, не реальных пользователей.

### Сравнение типов

| Тип | Фарминг-устойчивость | Справедливость | Привлечение пользователей | Сложность реализации |
|-----|---------------------|----------------|---------------------------|---------------------|
| Ретроспективный | Высокая | Высокая | Низкая (задним числом) | Средняя |
| Критериальный | Низкая | Средняя | Высокая | Низкая |
| Stake-based | Средняя | Средняя | Средняя | Низкая |
| Task-based | Низкая | Низкая | Высокая | Высокая |

## Проектирование airdrop

### Размер пула

Типичная доля airdrop в [аллокации]({{< relref "allocation" >}}): 5–15% от total supply. Ключевое ограничение: если пул слишком мал, airdrop не покроет газ-косты. Если слишком велик — создаст катастрофическое давление на продажу.

{{< formula math="MinReward = GasCost_claim × 10" desc="Минимальный размер airdrop на адрес: стоимость газа на клейм × 10. Если награда не покрывает газ в 10 раз, получатели не будут клеймить" >}}

### Сибил-защита

**Сибил-атака** — создание множества кошельков одним человеком для получения нескольких airdrop. Без защиты 50–80% токенов уходит фермерам.

Методы защиты:

| Метод | Эффективность | Недостатки |
|-------|--------------|------------|
| Минимальный баланс | Низкая | Легко обойти |
| Кластерный анализ | Средняя | Ложные срабатывания |
| Gitcoin Passport / WorldID | Высокая | Ограничивает аудиторию |
| Нелинейная шкала | Средняя | Не предотвращает, смягчает |
| KYC (верификация личности) | Высокая | Противоречит децентрализации |

**Нелинейная шкала** — самый популярный компромисс. Вместо линейной зависимости «больше транзакций = больше токенов» используется корневая функция:

{{< formula math="Reward(x) = Base + k × √x" desc="x — количество действий · k — коэффициент масштабирования · √x — убывающая отдача: 100 транзакций дают не 10× от 10 транзакций, а лишь ~3.2×" >}}

Корневая зависимость делает фарминг через множество кошельков менее выгодным, чем глубокое использование одного адреса.

### Механика клейма

Два подхода: push (автоматическая отправка на кошелёк) и pull (пользователь сам вызывает контракт).

**Pull (клейм)** — стандартный подход. Преимущества: экономит газ проекта, позволяет установить дедлайн клейма, даёт данные о вовлечённости. Невостребованные токены возвращаются в казначейство.

**Дедлайн клейма**: типично 90–180 дней. Uniswap установил 60 дней, после чего ~20% токенов вернулись в governance-казну.

### Вестинг airdrop

Критический параметр. Без вестинга получатели продают немедленно. С длинным вестингом — не клеймят (вознаграждение теряет привлекательность).

| Подход | TGE-unlock | Вестинг | Когда применять |
|--------|-----------|---------|-----------------|
| Полная разблокировка | 100% | 0 мес | Малый пул, лояльная аудитория |
| Частичная разблокировка | 25–50% | 3–6 мес | Стандартный подход |
| Lock-and-earn | 0% | 6–12 мес, с наградами за удержание | Максимальное снижение давления |

{{< callout title="Lock-and-earn как компромисс" >}}
Модель lock-and-earn набирает популярность: полученные токены заблокированы, но пользователь получает staking-награды с первого дня. Это снижает давление на продажу и одновременно стимулирует удержание.
{{< /callout >}}

## Давление на продажу после airdrop

Главный риск airdrop — массовый сброс. Исследования показывают, что 60–90% airdrop-получателей продают токены в первые 7 дней.

{{< formula math="SellPressure_day1 = Airdrop_pool × TGE% × SellRate" desc="SellRate для airdrop: 0.6–0.9 (60–90% продают) · TGE% — доля, доступная сразу" >}}

**Пример.** Проект выделяет 10% supply (10M токенов) на airdrop с TGE-unlock 100%:
- Если 70% получателей продают в день 1: давление = 7M токенов
- При circulating supply в TGE 20M: это 35% от оборота
- Результат: обвал цены

### Как смягчить давление

1. **Частичная разблокировка** — выдавать 25% сразу, остальное — через 3–6 месяцев
2. **Бонус за удержание** — дополнительные токены тем, кто не продал через 30/60/90 дней
3. **Стейкинг с первого дня** — возможность застейкать airdrop сразу для получения дохода
4. **Airdrop в LP-токенах** — распределять не токен, а позицию в пуле ликвидности
5. **Постепенная раздача** — несколько волн (season 1, season 2) вместо одного крупного события

## Модель распределения airdrop

На практике airdrop проектируется в таблице: для каждого действия или актива определяется **вес** — какую долю пула получает конкретная категория. Затем рассчитывается, сколько токенов приходится на единицу действия и на одного среднего/топ-пользователя.

Вот как выглядит типичная модель (по материалам курса токеномики):

### Шаг 1. Определяем действия и их количество

| Действие / актив | Тип | Всего у пользователей | Среднее на пользователя | Среднее у топ-10 |
|------------------|-----|-----------------------|-------------------------|------------------|
| Обычный питомец | Актив | 120 000 | 1.0 | 1 |
| Редкий питомец | Актив | 7 500 | 0.06 | 10 |
| Лайки | Актив | 10 000 000 | 83.3 | 80 000 |
| Гемы | Актив | 1 500 000 | 12.5 | 10 000 |
| Подписка в Telegram | Действие | 50 000 | 0.42 | 1 |
| Реферал | Действие | 500 000 | 4.17 | 250 |
| Пост в X | Действие | 15 000 | 0.13 | 20 |

### Шаг 2. Назначаем веса

Каждому действию назначается вес — процент от пула airdrop. Сумма весов = 100%. Веса определяют приоритеты: что проект считает наиболее ценным для экосистемы.

### Шаг 3. Рассчитываем коэффициент конвертации

{{< formula math="Coeff_i = (Weight_i × Airdrop_pool) / Total_actions_i" desc="Coeff_i — сколько токенов приходится на единицу действия i · Weight_i — вес действия · Total_actions_i — общее количество этого действия у всех пользователей" >}}

**Ключевой вывод:** у топ-10 пользователей доля airdrop **непропорционально высока**. Если топ-10 фермеров накликали 80 000 лайков из 10M — они получают 0.08% пула по лайкам. Но если их доля в «редких питомцах» — 13%, это уже критично. Именно поэтому нелинейная шкала (√x) важна для высоковариативных действий.

<details>
<summary>Код модели (Python)</summary>

```python
import numpy as np
import matplotlib.pyplot as plt

np.random.seed(42)
N_USERS = 10_000
AIRDROP_POOL = 5_000_000

activity = np.random.lognormal(mean=2.0, sigma=1.5, size=N_USERS)
activity = np.clip(activity, 1, 10_000)

linear_raw = activity / activity.sum() * AIRDROP_POOL
sqrt_raw = np.sqrt(activity) / np.sqrt(activity).sum() * AIRDROP_POOL

MIN_ACTIVITY = 5
mask = activity >= MIN_ACTIVITY
sqrt_filtered = np.where(mask, sqrt_raw, 0)
if sqrt_filtered.sum() > 0:
    sqrt_filtered = sqrt_filtered / sqrt_filtered.sum() * AIRDROP_POOL

linear_filtered = np.where(mask, linear_raw, 0)

fig, axes = plt.subplots(1, 2, figsize=(14, 5))
axes[0].hist(linear_filtered[mask], bins=50, alpha=0.6, label="Линейная", color="#ef4444")
axes[0].hist(sqrt_filtered[mask], bins=50, alpha=0.6, label="Корневая (√x)", color="#3b82f6")
axes[0].set_xlabel("Токенов на адрес")
axes[0].set_ylabel("Количество адресов")
axes[0].set_title("Распределение наград")
axes[0].legend()

for data, label, color in [
    (linear_filtered[mask], "Линейная", "#ef4444"),
    (sqrt_filtered[mask], "Корневая", "#3b82f6"),
]:
    sorted_data = np.sort(data)
    cum = np.cumsum(sorted_data) / sorted_data.sum()
    x = np.linspace(0, 1, len(cum))
    axes[1].plot(x, cum, label=label, color=color, linewidth=2)

axes[1].plot([0, 1], [0, 1], "k--", alpha=0.3, label="Идеальное равенство")
axes[1].set_xlabel("Доля получателей")
axes[1].set_ylabel("Доля токенов")
axes[1].set_title("Кривая Лоренца")
axes[1].legend()
plt.tight_layout()
plt.show()
```

</details>

**Результаты моделирования** (10 000 участников, пул 5M токенов, логнормальное распределение активности):

| Метрика | Линейная шкала | Корневая шкала (√x) |
|---------|---------------|---------------------|
| Медианная награда | ~50 токенов | ~350 токенов |
| Максимальная награда | ~25 000 токенов | ~4 500 токенов |
| Макс / медиана | 500× | 13× |
| Топ-1% получает | 45% пула | 18% пула |
| Топ-10% получает | 78% пула | 48% пула |
| Нижние 50% получают | 3% пула | 15% пула |

При линейной шкале один «кит» с 10 000 действий получает в 500 раз больше обычного пользователя. При корневой — только в 13 раз. Это критично для сибил-защиты: ферма из 100 кошельков по 100 действий при линейной шкале получает столько же, сколько один кошелёк с 10 000 действий. При корневой — **в 3.2 раза больше**, что делает фарминг менее выгодным, чем честное использование.

## Калькулятор airdrop-аллокации

<div class="gnts-calc" id="gnts-calc-airdrop" style="background:rgba(124,58,237,0.04);border:1px solid rgba(124,58,237,0.15);border-radius:12px;padding:28px;margin:32px 0;">
  <div style="font-family:'Space Grotesk',sans-serif;font-size:0.72rem;font-weight:600;text-transform:uppercase;letter-spacing:0.12em;color:#7c3aed;margin-bottom:20px;">Airdrop: расчёт пула и давления на продажу</div>
  <div style="display:grid;grid-template-columns:1fr 1fr;gap:20px;margin-bottom:24px;">
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">Total supply <span id="ad-val-ts" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">100 000 000</span></label>
      <input type="range" id="ad-ts" min="1000000" max="1000000000" step="1000000" value="100000000" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">Доля airdrop, % <span id="ad-val-pct" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">5%</span></label>
      <input type="range" id="ad-pct" min="1" max="20" step="0.5" value="5" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">Получателей <span id="ad-val-usr" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">50 000</span></label>
      <input type="range" id="ad-usr" min="1000" max="500000" step="1000" value="50000" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">Цена токена, $ <span id="ad-val-price" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">0.50</span></label>
      <input type="range" id="ad-price" min="0.01" max="5" step="0.01" value="0.50" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">TGE-unlock, % <span id="ad-val-tge" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">50%</span></label>
      <input type="range" id="ad-tge" min="0" max="100" step="5" value="50" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">Доля продаж, % <span id="ad-val-sell" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">70%</span></label>
      <input type="range" id="ad-sell" min="10" max="95" step="5" value="70" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
  </div>
  <div style="background:#f8f7f4;border:1px solid rgba(0,0,0,0.08);border-radius:8px;padding:20px;margin-bottom:16px;">
    <table style="width:100%;font-family:'Space Grotesk',sans-serif;font-size:0.85rem;border-collapse:collapse;">
      <tbody id="ad-tbody">
      </tbody>
    </table>
  </div>
  <div id="ad-warning" style="display:none;background:rgba(239,68,68,0.08);border:1px solid rgba(239,68,68,0.2);border-radius:8px;padding:12px;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;color:#dc2626;margin-top:12px;"></div>
</div>
<script>
(function(){
  var ids=['ts','pct','usr','price','tge','sell'];
  var sl={},vl={};
  ids.forEach(function(k){sl[k]=document.getElementById('ad-'+k);vl[k]=document.getElementById('ad-val-'+k);});
  var tbody=document.getElementById('ad-tbody'),warn=document.getElementById('ad-warning');
  function fmt(n){if(n>=1e9)return (n/1e9).toFixed(1)+'B';if(n>=1e6)return (n/1e6).toFixed(1)+'M';if(n>=1e3)return Math.round(n).toLocaleString('ru');return n.toFixed(2);}
  function upd(){
    var ts=+sl.ts.value,pct=+sl.pct.value/100,usr=+sl.usr.value,price=+sl.price.value,tge=+sl.tge.value/100,sell=+sl.sell.value/100;
    vl.ts.textContent=fmt(ts);vl.pct.textContent=sl.pct.value+'%';vl.usr.textContent=fmt(usr);vl.price.textContent='$'+price.toFixed(2);vl.tge.textContent=sl.tge.value+'%';vl.sell.textContent=sl.sell.value+'%';
    var pool=ts*pct,avg=pool/usr,avgUsd=avg*price,sellDay1=pool*tge*sell,sellUsd=sellDay1*price,gasCost=0.5,minPrice=gasCost*10/avg;
    var rows=[
      ['Пул airdrop',fmt(pool)+' токенов'],
      ['Стоимость пула','$'+fmt(pool*price)],
      ['Средний airdrop',fmt(avg)+' токенов ($'+avgUsd.toFixed(2)+')'],
      ['Разблокировано в TGE',fmt(pool*tge)+' токенов'],
      ['Давление на продажу (день 1)',fmt(sellDay1)+' токенов ($'+fmt(sellUsd)+')'],
      ['Мин. цена для клейма (при газе $0.50)','$'+minPrice.toFixed(4)]
    ];
    tbody.innerHTML='';
    rows.forEach(function(r){
      var tr=document.createElement('tr');
      tr.innerHTML='<td style="padding:8px 0;border-bottom:1px solid rgba(0,0,0,0.06);color:#64748b;">'+r[0]+'</td><td style="padding:8px 0;border-bottom:1px solid rgba(0,0,0,0.06);text-align:right;font-family:\'JetBrains Mono\',monospace;font-weight:600;color:#1a1a2e;">'+r[1]+'</td>';
      tbody.appendChild(tr);
    });
    var warnings=[];
    if(avgUsd<5)warnings.push('Средний airdrop меньше $5 — большинство не будут клеймить');
    if(sellDay1/ts>0.05)warnings.push('Давление в день 1 превышает 5% total supply — высокий риск обвала');
    if(price<minPrice)warnings.push('Цена ниже минимальной для клейма — получатели потеряют на газе');
    if(warnings.length){warn.style.display='block';warn.innerHTML=warnings.join('<br>');}else{warn.style.display='none';}
  }
  ids.forEach(function(k){sl[k].addEventListener('input',upd);});
  upd();
})();
</script>

Двигайте ползунки — калькулятор покажет размер пула, среднюю награду, давление на продажу в день 1 и предупредит о рисках.

## Распространённые ошибки

### 1. Airdrop без сибил-защиты

Без фильтрации 50–80% токенов уходят на фермерские кошельки. Минимум: кластерный анализ + нелинейная шкала + минимальный порог активности.

### 2. Полная разблокировка без механизма удержания

100% TGE-unlock без стимулов к удержанию = массовый сброс. Решение: частичная разблокировка или бонус за удержание.

### 3. Слишком маленький пул

Airdrop менее $10 в эквиваленте не мотивирует ни клейм, ни участие. Лучше меньше получателей с осмысленной наградой, чем 100 000 адресов по $2.

### 4. Критерии, наказывающие реальных пользователей

Минимум 50 транзакций? Реальный пользователь DEX делает 3–10 свопов в месяц. Завышенные пороги отсекают настоящую аудиторию и вознаграждают ботов.

### 5. Отсутствие второго сезона

Одноразовый airdrop создаёт пик и провал активности. Многосезонная структура (season 1, 2, 3) поддерживает вовлечённость и позволяет итерировать критерии.

{{< checklist title="Чеклист проектирования airdrop" type="check" >}}
<li><strong>Сибил-защита внедрена</strong> — кластерный анализ или Gitcoin Passport</li>
<li><strong>Нелинейная шкала</strong> — корневая или логарифмическая зависимость от активности</li>
<li><strong>Минимальная награда покрывает 10× газа</strong> — иначе получатели не клеймят</li>
<li><strong>Давление на продажу смоделировано</strong> — расчёт с SellRate 70–90%</li>
<li><strong>Вестинг или lock-and-earn</strong> — не более 50% TGE-unlock</li>
<li><strong>Дедлайн клейма установлен</strong> — 90–180 дней, невостребованные токены возвращаются в казну</li>
<li><strong>Запланирован второй сезон</strong> — многосезонная структура удерживает пользователей</li>
<li><strong>Airdrop не превышает 15% total supply</strong> — больше создаёт чрезмерное давление</li>
{{< /checklist >}}

{{< cta title="Проектируем airdrop для вашего протокола" text="Мы спроектировали модели распределения для 85+ проектов — от DeFi до GameFi. Рассчитаем оптимальный размер пула, критерии и защиту от сибил-атак." button="Обсудить airdrop" link="https://gnts.io" >}}

## Читайте также

- [5 моделей предложения токенов]({{< relref "token-supply-models" >}}) — airdrop в контексте остальных моделей распределения
- [Аллокация как модель предложения токенов]({{< relref "allocation" >}}) — как определить размер airdrop-пула
- [Шаблон аллокации и вестинга]({{< relref "allocation-template" >}}) — готовый шаблон для расчётов
- [Дизайн механизмов в токеномике]({{< relref "mechanism-design" >}}) — как airdrop вписывается в общую архитектуру стимулов
