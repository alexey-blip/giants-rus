---
title: "Дивиденды или выкуп: сравнение двух моделей утилизации токенов"
description: "Сравнение дивидендной модели и модели выкупа токенов. Формулы доходности, метрики, гибридные модели и выбор оптимальной стратегии утилизации."
date: 2026-04-09
weight: 50
tags:
  - токеномика
  - утилизация
  - buyback
  - дивиденды
categories:
  - models
---

Как протоколы создают устойчивый спрос на токен через распределение дохода и сжигание предложения. Формулы доходности, реальные примеры и алгоритм выбора оптимальной модели.

## Что такое утилизация токенов

**Утилизация токенов** — это механизмы, создающие постоянный спрос на токен и выступающие противовесом эмиссии. Если эмиссия — это кран, из которого токены вливаются в оборот, то утилизация — это воронка, которая их поглощает.

Без механизмов утилизации любой токен с эмиссией обречён на долгосрочное снижение цены: растущее предложение при постоянном или падающем спросе давит на котировки. Именно поэтому продуманная утилизация — один из ключевых элементов токеномики.

Существуют два фундаментально различных подхода к утилизации:

- **Дивидендная модель** (revenue share) — доход протокола распределяется между держателями токенов через staking. Токены не уничтожаются, но создаётся стимул их держать.
- **Модель выкупа** (buy-back & burn) — протокол выкупает токены на открытом рынке и сжигает их, уменьшая общее предложение и создавая дефляцию.

{{< callout title="Ключевой принцип" >}}
Утилизация работает только тогда, когда протокол генерирует **реальный доход**. Распределение инфляционных наград — это не утилизация, а перекладывание из одного кармана в другой. Настоящая доходность (real yield) возникает только из комиссий и прибыли протокола.
{{< /callout >}}

### Зачем нужна утилизация

Механизмы утилизации решают несколько задач одновременно:

1. **Противовес эмиссии** — компенсация давления на цену от разблокировки токенов (вестинг, награды)
2. **Стимул к удержанию** — держатели получают доход или выигрывают от дефляции вместо того, чтобы продавать
3. **Связь с реальной экономикой** — цена токена начинает отражать доходность бизнеса, а не только спекулятивный спрос
4. **Привлечение капитала** — институциональные инвесторы предпочитают токены с понятной моделью возврата

## Дивидендная модель (revenue share)

В дивидендной модели протокол направляет часть своего дохода держателям токенов. Чтобы получать выплаты, токены необходимо застейкать — заблокировать в смарт-контракте. Это создаёт два эффекта: прямой доход для стейкеров и сокращение обращающегося предложения.

### Как работает механика

1. Протокол собирает комиссии (торговые сборы, проценты по кредитам, плата за услуги)
2. Часть комиссий направляется в пул вознаграждений (reward pool)
3. Держатели блокируют токены через staking
4. Награды распределяются пропорционально доле каждого стейкера
5. Выплаты происходят в стейблкоинах, ETH или в самом токене протокола

### Формулы расчёта доходности

{{< formula math="Dividend Yield = (Revenue × Share%) / (MCap × Staking%)" desc="Revenue — годовой доход протокола · Share% — доля, направляемая стейкерам · MCap — капитализация · Staking% — доля застейканных токенов" >}}

Если протокол зарабатывает $10M в год, направляет 50% стейкерам, капитализация составляет $100M, а застейкано 40% токенов, то:

{{< formula math="Yield = ($10M × 0.5) / ($100M × 0.4) = 12.5%" desc="Реальная доходность 12.5% годовых в стейблкоинах — без инфляционного размывания" >}}

### Примеры дивидендных моделей

**xSUSHI (SushiSwap).** Одна из первых реализаций revenue share в DeFi. Держатели токена SUSHI стейкают его и получают xSUSHI — токен, чья стоимость растёт за счёт накопления 0.05% от каждого свопа на платформе. Модель простая: застейкал — получаешь долю от комиссий.

**GMX.** Децентрализованная биржа деривативов распределяет 30% от комиссий стейкерам GMX в ETH/AVAX. Доходность напрямую зависит от торгового объёма: чем больше сделок — тем выше yield. В пиковые месяцы доходность превышала 20% годовых в ETH.

**veCRV (Curve Finance).** Держатели блокируют CRV на срок до 4 лет и получают долю от комиссий всех пулов. Чем длиннее блокировка — тем больше голосов и тем выше доля наград. Это гибридная модель, совмещающая revenue share с управлением.

{{< callout title="Преимущества дивидендной модели" >}}
**Прозрачность:** стейкеры точно видят, сколько и когда получают. **Предсказуемость:** yield привязан к реальному доходу. **Прямая мотивация:** держатели заинтересованы в росте протокола, потому что это увеличивает их дивиденды.
{{< /callout >}}

## Модель выкупа и сжигания (buy-back & burn)

В модели buy-back & burn протокол использует часть дохода для выкупа собственных токенов на открытом рынке и последующего их сжигания (отправки на нулевой адрес). Это уменьшает общее предложение, что при стабильном спросе ведёт к росту цены каждого оставшегося токена.

### Механика работы

1. Протокол накапливает доход от комиссий
2. По расписанию (ежемесячно, ежеквартально) выкупает токены на DEX или через OTC
3. Выкупленные токены отправляются на burn-адрес (0x000...dead)
4. Транзакция сжигания видна в блокчейне — прозрачность верифицируема
5. Общее предложение токена сокращается навсегда

### Формула оценки эффекта

{{< formula math="Buyback Yield = (Revenue × Buyback%) / MCap" desc="Buyback% — доля дохода, направляемая на выкуп · Показывает годовое сокращение предложения в процентах от капитализации" >}}

Buyback yield экономически эквивалентен дивидендной доходности, но вместо прямых выплат стейкерам стоимость распределяется через рост цены токена.

{{< formula math="Deflation Rate = Tokens Burned / Total Supply × 100%" desc="При постоянном burn rate и фиксированном предложении — экспоненциальная дефляция" >}}

### Примеры моделей выкупа

**BNB (Binance).** Ежеквартальный автоматический burn по формуле, привязанной к объёму торгов на бирже. Начальное предложение — 200M токенов, целевое — 100M. К 2025 году сожжено более 50M BNB. Один из самых масштабных примеров buy-back & burn в индустрии.

**MKR (MakerDAO).** Протокол использует surplus-доход (превышение комиссий над расходами) для выкупа и сжигания MKR через аукционы. В периоды высокого дохода DAO MKR может сжигаться быстрее, чем минтится в качестве наград, создавая чистую дефляцию.

**AAVE (Safety Module).** Часть дохода протокола направляется в Safety Module — страховой пул. Механика включает элементы buy-back: при определённых условиях протокол выкупает AAVE на рынке для пополнения фонда, одновременно создавая спрос.

{{< callout type="warning" title="Налоговое различие" >}}
В большинстве юрисдикций buy-back не создаёт налогооблагаемого события для держателей (пока они не продадут токены по выросшей цене). Дивиденды, напротив, могут облагаться налогом в момент получения. Это делает buy-back привлекательнее с точки зрения налоговой оптимизации.
{{< /callout >}}

## Сравнение моделей

Обе модели используют доход протокола для создания ценности токена, но делают это принципиально по-разному. Ниже — детальное сравнение по ключевым параметрам.

| Параметр | Дивиденды (revenue share) | Выкуп (buy-back & burn) |
|----------|---------------------------|-------------------------|
| **Механика** | Прямые выплаты стейкерам | Выкуп и сжигание токенов |
| **Источник дохода** | Комиссии → стейкерам | Комиссии → покупка на рынке |
| **Влияние на цену** | Косвенное (снижение sell pressure) | Прямое (рост цены через дефляцию) |
| **Прозрачность** | Высокая (видны выплаты) | Высокая (видны burn-транзакции) |
| **Налоги для держателей** | Событие при получении | Событие при продаже токена |
| **Требует staking** | Да | Нет (выигрывают все держатели) |
| **Регуляторный риск** | Высокий (securities) | Средний |
| **Привязка к доходу** | Прямая и очевидная | Непрямая (через объём burn) |
| **Поведение в даунтренде** | Yield растёт (цена падает, yield = доход/цена) | Больше токенов за ту же сумму burn |
| **Примеры** | GMX, SushiSwap, Curve | BNB, MKR, AAVE |

### Регуляторные нюансы

Дивидендная модель несёт повышенный регуляторный риск: распределение дохода пропорционально долям — один из признаков ценной бумаги (security) по тесту Хоуи. Токен с revenue share может быть классифицирован как незарегистрированная ценная бумага в США и ряде других юрисдикций.

Buy-back & burn считается менее рискованным, потому что держатели не получают прямых выплат. Однако регуляторная среда меняется, и этот вопрос требует юридической оценки для каждого конкретного проекта.

## Гибридные модели

Наиболее продвинутые протоколы комбинируют оба подхода, добавляя управление (governance) и механизмы временной блокировки. Флагманская гибридная модель — vote-escrow (ve-модель), впервые реализованная в Curve Finance.

### Vote-escrow: блокировка + управление + доход

В ve-модели держатель блокирует токены на фиксированный срок (от нескольких недель до нескольких лет) и получает взамен ve-токен. Этот ve-токен даёт три преимущества одновременно:

- **Revenue share** — доля от комиссий протокола пропорционально количеству ve-токенов
- **Голоса в управлении** — возможность влиять на распределение эмиссии (gauge voting)
- **Усиленные награды** — бонусный множитель к наградам за ликвидность (boost)

{{< formula math="veToken = Token × (Lock Time / Max Lock Time)" desc="Блокировка CRV на 4 года даёт 1:1 в veCRV · на 2 года — 0.5 veCRV за 1 CRV · количество ve-токенов линейно убывает со временем" >}}

### Curve Finance (veCRV)

Curve — эталон ve-модели. Держатели блокируют CRV на срок до 4 лет, получая veCRV. Ключевые механики:

- **Комиссии:** 50% комиссий от всех пулов распределяются между держателями veCRV
- **Gauge voting:** держатели veCRV голосуют за распределение эмиссии CRV по пулам. Это создало целую экосистему «войн за голоса» (Curve Wars), где протоколы конкурируют за направление эмиссии в свои пулы
- **Boost:** провайдеры ликвидности с veCRV получают до 2.5x к наградам

### Pendle (vePENDLE)

Pendle адаптировала ve-модель для рынка yield-токенизации. Держатели vePENDLE получают долю от комиссий торговых пулов, голосуют за направление эмиссии и получают boost к наградам за ликвидность. Дополнительно, в Pendle реализован механизм buy-back: часть дохода направляется на выкуп PENDLE на рынке перед распределением стейкерам.

{{< callout title="Почему ve-модель работает" >}}
Ve-модель создаёт **тройной замок** на токены: (1) прямой доход стимулирует блокировку, (2) управление создаёт политическую ценность токена, (3) boost делает блокировку выгодной для провайдеров ликвидности. Результат — высокая доля застейканных токенов (60–80%) и стабильная цена.
{{< /callout >}}

### Другие гибридные подходы

**Buyback + redistribution.** Протокол выкупает токены на рынке, но не сжигает, а распределяет их стейкерам. Сочетает давление покупки (buy pressure) с прямым доходом.

**Частичный burn + частичное распределение.** Доход делится: 50% на buy-back & burn, 50% на распределение стейкерам. Это диверсифицирует механики и снижает концентрацию рисков.

## Метрики эффективности

Чтобы сравнивать модели утилизации между собой и оценивать их эффективность, используются следующие метрики.

### P/E Ratio для токенов

{{< formula math="Token P/E = Fully Diluted Valuation / Annual Protocol Revenue" desc="Аналог P/E из традиционных финансов · Чем ниже — тем «дешевле» токен относительно дохода" >}}

P/E позволяет сравнивать протоколы по фундаментальной оценке. Токен с P/E = 10 торгуется «дешевле», чем токен с P/E = 100 при прочих равных. Данные по протокольным доходам доступны на Token Terminal, DeFiLlama и аналогичных агрегаторах.

| Протокол | Модель | Ориентировочный P/E | Комментарий |
|----------|--------|---------------------|-------------|
| **GMX** | Revenue share | 10–15 | Низкий P/E, высокий real yield |
| **Curve** | Ve-модель (гибрид) | 20–40 | Премия за governance-ценность |
| **BNB** | Buy-back & burn | 8–12 | Массивный объём burn |
| **AAVE** | Safety module | 30–50 | Консервативная утилизация |

### Buyback Yield

{{< formula math="Buyback Yield = Annual Buyback Volume / Market Cap × 100%" desc="Показывает процент капитализации, ежегодно «возвращаемый» через сжигание" >}}

Buyback yield — ключевая метрика для оценки моделей выкупа. При buyback yield 5% протокол ежегодно сжигает токены на сумму, равную 5% своей капитализации. Это эквивалент 5% дивидендной доходности с точки зрения возврата стоимости.

### Real Yield и отличие от инфляционного дохода

{{< formula math="Real Yield = Nominal Yield − Inflation Rate" desc="Nominal Yield — заявленный APY · Inflation Rate — годовая эмиссия / текущее предложение" >}}

Критически важно отличать реальный доход от инфляционного. Если протокол платит 20% APY стейкерам, но при этом эмитирует 25% новых токенов в год, **реальная доходность отрицательна**: −5%. Стейкеры получают больше токенов, но каждый токен теряет в стоимости из-за размывания.

{{< callout type="warning" title="Пример расчёта реальной доходности" >}}
**Заявленный APY стейкинга:** 30%
**Годовая эмиссия (инфляция):** 25%
**Реальная доходность:** 30% − 25% = **5%**

Но если выплаты происходят в самом токене протокола:
**Реальный рост позиции:** 5% (в токенах)
**Эффект на цену от размывания:** −20%
**Итого реальная доходность в USD: отрицательная**
{{< /callout >}}

## Калькулятор доходности

<div style="background:#fff;border:1px solid rgba(0,0,0,0.08);border-radius:12px;padding:28px 32px;margin:32px 0;" id="uy-calc">
  <div style="font-family:'Space Grotesk',sans-serif;font-size:0.72rem;font-weight:600;text-transform:uppercase;letter-spacing:0.12em;color:#7c3aed;margin-bottom:20px;">Дивидендная доходность + выкуп</div>
  <div style="display:grid;grid-template-columns:1fr 1fr;gap:16px 28px;margin-bottom:24px;">
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:4px;">Годовая выручка от комиссий ($) <span id="uy-val-rev" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.05);padding:2px 8px;border-radius:4px;">10.0M</span></label>
      <input type="range" id="uy-rev" min="100000" max="100000000" step="100000" value="10000000" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:rgba(0,0,0,0.08);border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:4px;">Рыночная капитализация (Mcap, $) <span id="uy-val-mcap" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.05);padding:2px 8px;border-radius:4px;">100.0M</span></label>
      <input type="range" id="uy-mcap" min="1000000" max="1000000000" step="1000000" value="100000000" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:rgba(0,0,0,0.08);border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:4px;">Доля застейканных токенов (%) <span id="uy-val-stake" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.05);padding:2px 8px;border-radius:4px;">40%</span></label>
      <input type="range" id="uy-stake" min="5" max="95" step="1" value="40" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:rgba(0,0,0,0.08);border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
    <div style="display:flex;gap:28px;">
      <div style="flex:1;">
        <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:4px;">Выручки → дивиденды (%) <span id="uy-val-divr" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.05);padding:2px 8px;border-radius:4px;">50%</span></label>
        <input type="range" id="uy-divr" min="0" max="100" step="5" value="50" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:rgba(0,0,0,0.08);border-radius:2px;outline:none;accent-color:#7c3aed;">
      </div>
      <div style="flex:1;">
        <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:4px;">Выручки → выкуп (%) <span id="uy-val-bbr" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.05);padding:2px 8px;border-radius:4px;">30%</span></label>
        <input type="range" id="uy-bbr" min="0" max="100" step="5" value="30" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:rgba(0,0,0,0.08);border-radius:2px;outline:none;accent-color:#7c3aed;">
      </div>
    </div>
  </div>
  <div id="uy-warning" style="display:none;padding:10px 14px;margin-bottom:16px;background:rgba(220,38,38,0.08);border:1px solid rgba(220,38,38,0.3);border-radius:8px;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;color:#dc2626;font-weight:500;">Сумма долей на дивиденды и выкуп превышает 100% выручки</div>
  <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:12px;margin-bottom:24px;">
    <div style="background:rgba(124,58,237,0.05);border:1px solid rgba(0,0,0,0.08);border-radius:8px;padding:16px;text-align:center;">
      <div style="font-family:'Space Grotesk',sans-serif;font-size:0.68rem;font-weight:600;text-transform:uppercase;letter-spacing:0.1em;color:#7c3aed;margin-bottom:6px;">Дивидендная доходность</div>
      <div id="uy-card-div" style="font-family:'JetBrains Mono',monospace;font-size:1.4rem;font-weight:700;color:#7c3aed;">12.5%</div>
    </div>
    <div style="background:rgba(5,150,105,0.05);border:1px solid rgba(0,0,0,0.08);border-radius:8px;padding:16px;text-align:center;">
      <div style="font-family:'Space Grotesk',sans-serif;font-size:0.68rem;font-weight:600;text-transform:uppercase;letter-spacing:0.1em;color:#059669;margin-bottom:6px;">Доходность выкупа</div>
      <div id="uy-card-bb" style="font-family:'JetBrains Mono',monospace;font-size:1.4rem;font-weight:700;color:#059669;">3.0%</div>
    </div>
    <div style="background:rgba(217,119,6,0.05);border:1px solid rgba(0,0,0,0.08);border-radius:8px;padding:16px;text-align:center;">
      <div style="font-family:'Space Grotesk',sans-serif;font-size:0.68rem;font-weight:600;text-transform:uppercase;letter-spacing:0.1em;color:#d97706;margin-bottom:6px;">Совокупная доходность</div>
      <div id="uy-card-total" style="font-family:'JetBrains Mono',monospace;font-size:1.4rem;font-weight:700;color:#d97706;">15.5%</div>
    </div>
  </div>
  <div style="background:#f8f7f4;border:1px solid rgba(0,0,0,0.08);border-radius:8px;overflow:hidden;">
    <svg id="uy-svg" viewBox="0 0 700 260" preserveAspectRatio="xMidYMid meet" style="display:block;width:100%;height:260px;"></svg>
  </div>
  <table style="margin-top:16px;width:100%;border-collapse:collapse;font-size:0.8rem;">
    <thead><tr>
      <th style="text-align:left;font-family:'Space Grotesk',sans-serif;font-weight:600;font-size:0.7rem;text-transform:uppercase;letter-spacing:0.06em;color:#7c3aed;padding:6px 10px;border-bottom:1px solid rgba(0,0,0,0.08);">Год</th>
      <th style="text-align:left;font-family:'Space Grotesk',sans-serif;font-weight:600;font-size:0.7rem;text-transform:uppercase;letter-spacing:0.06em;color:#7c3aed;padding:6px 10px;border-bottom:1px solid rgba(0,0,0,0.08);">Дивиденды ($)</th>
      <th style="text-align:left;font-family:'Space Grotesk',sans-serif;font-weight:600;font-size:0.7rem;text-transform:uppercase;letter-spacing:0.06em;color:#7c3aed;padding:6px 10px;border-bottom:1px solid rgba(0,0,0,0.08);">Выкуп ($)</th>
      <th style="text-align:left;font-family:'Space Grotesk',sans-serif;font-weight:600;font-size:0.7rem;text-transform:uppercase;letter-spacing:0.06em;color:#7c3aed;padding:6px 10px;border-bottom:1px solid rgba(0,0,0,0.08);">Дох. див.</th>
      <th style="text-align:left;font-family:'Space Grotesk',sans-serif;font-weight:600;font-size:0.7rem;text-transform:uppercase;letter-spacing:0.06em;color:#7c3aed;padding:6px 10px;border-bottom:1px solid rgba(0,0,0,0.08);">Дох. выкуп</th>
      <th style="text-align:left;font-family:'Space Grotesk',sans-serif;font-weight:600;font-size:0.7rem;text-transform:uppercase;letter-spacing:0.06em;color:#7c3aed;padding:6px 10px;border-bottom:1px solid rgba(0,0,0,0.08);">Совокупный</th>
    </tr></thead>
    <tbody id="uy-tbody"></tbody>
  </table>
</div>
<script>
(function(){
  var $=function(id){return document.getElementById(id)};
  var sl={rev:$('uy-rev'),mcap:$('uy-mcap'),stake:$('uy-stake'),divr:$('uy-divr'),bbr:$('uy-bbr')};
  var vl={rev:$('uy-val-rev'),mcap:$('uy-val-mcap'),stake:$('uy-val-stake'),divr:$('uy-val-divr'),bbr:$('uy-val-bbr')};
  var svg=$('uy-svg'),tbody=$('uy-tbody'),warning=$('uy-warning');
  var cardDiv=$('uy-card-div'),cardBB=$('uy-card-bb'),cardTotal=$('uy-card-total');
  function fmtM(v){if(v>=1e9)return(v/1e9).toFixed(1)+'B';if(v>=1e6)return(v/1e6).toFixed(1)+'M';if(v>=1e3)return(v/1e3).toFixed(0)+'K';return v.toFixed(0)}
  function fmtP(v){return(v*100).toFixed(1)+'%'}
  function fmtD(v){return'$'+(v>=1e6?(v/1e6).toFixed(2)+'M':v>=1e3?(v/1e3).toFixed(0)+'K':v.toFixed(0))}
  function render(){
    var rev=+sl.rev.value,mcap=+sl.mcap.value,stake=+sl.stake.value/100,divR=+sl.divr.value/100,bbR=+sl.bbr.value/100;
    vl.rev.textContent=fmtM(rev);vl.mcap.textContent=fmtM(mcap);vl.stake.textContent=Math.round(stake*100)+'%';vl.divr.textContent=Math.round(divR*100)+'%';vl.bbr.textContent=Math.round(bbR*100)+'%';
    if(divR+bbR>1){warning.style.display='block'}else{warning.style.display='none'}
    var divYield=(rev*divR)/(mcap*stake),bbYield=(rev*bbR)/mcap,combinedYield=divYield+bbYield;
    cardDiv.textContent=fmtP(divYield);cardBB.textContent=fmtP(bbYield);cardTotal.textContent=fmtP(combinedYield);
    var W=700,H=260,pd={t:30,r:30,b:50,l:60},pw=W-pd.l-pd.r,ph=H-pd.t-pd.b;
    var accent='#7c3aed',green='#059669',orange='#d97706',muted='#8a8a9a';
    var bars=[{label:'Дивиденды',value:divYield,color:accent},{label:'Выкуп',value:bbYield,color:green},{label:'Совокупный',value:combinedYield,color:orange}];
    var maxVal=Math.max(combinedYield,0.01)*1.2,barW=pw/bars.length*0.4,gap=(pw-barW*bars.length)/(bars.length+1);
    var h='';
    for(var g=0;g<=4;g++){var gy=pd.t+(g/4)*ph;h+='<line x1="'+pd.l+'" y1="'+gy+'" x2="'+(W-pd.r)+'" y2="'+gy+'" stroke="'+muted+'" stroke-opacity="0.15" stroke-dasharray="4,4"/>';h+='<text x="'+(pd.l-8)+'" y="'+(gy+4)+'" text-anchor="end" fill="'+muted+'" font-size="10" font-family="JetBrains Mono,monospace">'+fmtP(maxVal*(1-g/4))+'</text>'}
    for(var i=0;i<bars.length;i++){var bx=pd.l+gap+(barW+gap)*i,bh=Math.max((bars[i].value/maxVal)*ph,2),by=pd.t+ph-bh;h+='<rect x="'+bx+'" y="'+by+'" width="'+barW+'" height="'+bh+'" rx="4" fill="'+bars[i].color+'" opacity="0.8"/>';h+='<text x="'+(bx+barW/2)+'" y="'+(by-8)+'" text-anchor="middle" fill="'+bars[i].color+'" font-size="12" font-weight="600" font-family="JetBrains Mono,monospace">'+fmtP(bars[i].value)+'</text>';h+='<text x="'+(bx+barW/2)+'" y="'+(H-12)+'" text-anchor="middle" fill="'+muted+'" font-size="11" font-family="Space Grotesk,sans-serif">'+bars[i].label+'</text>'}
    svg.innerHTML=h;
    var rows='',curRev=rev,curMcap=mcap;
    for(var y=1;y<=5;y++){var dAmt=curRev*divR,bAmt=curRev*bbR,dy=(curRev*divR)/(curMcap*stake),by2=(curRev*bbR)/curMcap,cy=dy+by2;rows+='<tr><td style="padding:5px 10px;font-family:JetBrains Mono,monospace;font-size:0.78rem;color:#4a4a5a;border-bottom:1px solid rgba(0,0,0,0.06)">'+y+'</td><td style="padding:5px 10px;font-family:JetBrains Mono,monospace;font-size:0.78rem;color:#4a4a5a;border-bottom:1px solid rgba(0,0,0,0.06)">'+fmtD(dAmt)+'</td><td style="padding:5px 10px;font-family:JetBrains Mono,monospace;font-size:0.78rem;color:#4a4a5a;border-bottom:1px solid rgba(0,0,0,0.06)">'+fmtD(bAmt)+'</td><td style="padding:5px 10px;font-family:JetBrains Mono,monospace;font-size:0.78rem;color:'+accent+';border-bottom:1px solid rgba(0,0,0,0.06)">'+fmtP(dy)+'</td><td style="padding:5px 10px;font-family:JetBrains Mono,monospace;font-size:0.78rem;color:'+green+';border-bottom:1px solid rgba(0,0,0,0.06)">'+fmtP(by2)+'</td><td style="padding:5px 10px;font-family:JetBrains Mono,monospace;font-size:0.78rem;font-weight:600;color:'+orange+';border-bottom:1px solid rgba(0,0,0,0.06)">'+fmtP(cy)+'</td></tr>';curRev*=1.1;curMcap*=(1+by2)}
    tbody.innerHTML=rows;
  }
  Object.values(sl).forEach(function(s){s.addEventListener('input',render)});render();
})();
</script>

## Ошибки и подводные камни

Модели утилизации кажутся простыми, но на практике проекты совершают одни и те же ошибки, которые обесценивают токен несмотря на формально работающую механику.

### Инфляционные награды под видом дохода

Самая распространённая ошибка — маскировка инфляционной эмиссии под yield. Протокол заявляет «30% APY на staking», но эти 30% выплачиваются из вновь эмитированных токенов, а не из дохода. Стейкеры получают больше токенов, но цена каждого токена снижается из-за разбавления.

{{< callout type="warning" title="Красные флаги" >}}
**APY > 100%** почти всегда означает инфляционные награды. **Выплаты в собственном токене** без привязки к доходу — классический признак Понци-механики. **Отсутствие данных о доходе** протокола при высоких наградах — повод для тщательной проверки.
{{< /callout >}}

### Неустойчивый APY

Некоторые протоколы устанавливают фиксированный APY в начале работы, пока реальный доход не покрывает обещанные выплаты. Разница финансируется из казначейства или новой эмиссии. Когда казначейство истощается, APY резко падает, а вместе с ним и цена токена.

Устойчивая модель привязывает выплаты к фактическому доходу: доход растёт — растут и дивиденды; падает — падают. Честный подход, даже если доходность ниже.

### Buy-back без прозрачности

Протокол объявляет о программе buy-back, но не публикует адреса, транзакции и расписание. Без верифицируемого burn нет гарантии, что выкуп действительно происходит. Лучшая практика — публичный burn-адрес и регулярные отчёты с ссылками на транзакции в блокчейне.

### Регуляторный риск securities

Тест Хоуи определяет ценные бумаги по четырём признакам: (1) вложение денег, (2) в общее предприятие, (3) с ожиданием прибыли, (4) исключительно за счёт усилий других. Токен с revenue share попадает под все четыре пункта, если стейкеры пассивно получают доход от работы команды протокола.

Способы минимизации этого риска:

- **Децентрализация управления** — передача ключевых решений DAO снижает зависимость от «усилий других»
- **Активное участие** — ve-модель требует от держателей голосования, что добавляет элемент участия
- **Buy-back вместо дивидендов** — непрямое распределение стоимости считается менее рискованным
- **Юридическая структура** — обёртка через Foundation или DAO с чёткой документацией

### Концентрация стейка

Если 80% стейка принадлежит 5 кошелькам (часто — инсайдерам), то revenue share фактически возвращает доход основателям. Для рядовых держателей yield размывается. Здоровый протокол демонстрирует широкое распределение стейка с Gini-коэффициентом ниже 0.7.

## Как выбрать модель

Выбор модели утилизации зависит от типа протокола, регуляторной среды, стадии развития и предпочтений сообщества. Ниже — структурированная схема принятия решения.

### Определяющие факторы

| Фактор | Дивиденды (revenue share) | Выкуп (buy-back & burn) | Гибрид (ve-модель) |
|--------|---------------------------|-------------------------|---------------------|
| **Стабильный доход** | Высокий и стабильный | Любой уровень | Высокий и стабильный |
| **Регуляторный контекст** | Вне юрисдикции SEC | Любой | DAO-управление |
| **Целевая аудитория** | Пассивные инвесторы | Спекулянты, держатели | Активные участники |
| **Сложность реализации** | Средняя | Низкая | Высокая |
| **Нужно governance** | Не обязательно | Не обязательно | Да, обязательно |

### Рекомендации по типу протокола

**DEX / торговая платформа** — гибридная ve-модель. Торговые комиссии создают стабильный доход, управление распределением ликвидности добавляет ценность токену. Примеры: Curve, Pendle, Velodrome.

**Кредитный протокол** — buy-back & burn или Safety Module. Процентный доход предсказуем, но регуляторные риски высоки — прямые дивиденды опасны. Примеры: AAVE, MakerDAO.

**Инфраструктурный проект (L1/L2, оракулы)** — staking с элементами revenue share. Комиссии сети распределяются между валидаторами, создавая натуральный спрос на токен для участия в консенсусе.

**Биржевой токен** — buy-back & burn. Централизованные биржи не могут полностью децентрализовать управление. Buy-back прост в реализации и не несёт рисков securities при наличии utility. Примеры: BNB.

**Ранняя стадия (мало дохода)** — отложенная утилизация. Не стоит обещать yield, если дохода ещё нет. Лучше заложить механику в смарт-контракт, но активировать её, когда протокол начнёт стабильно зарабатывать.

{{< callout title="Итоговый алгоритм" >}}
1. Определите источник и объём дохода протокола.
2. Оцените регуляторные ограничения для вашей юрисдикции.
3. Определите целевое поведение держателей (пассивный доход или активное участие).
4. Выберите базовую модель (дивиденды / выкуп / гибрид).
5. Рассчитайте прогнозный yield через калькулятор выше.
6. Проверьте устойчивость при падении дохода на 50–70%.
{{< /callout >}}

### Дерево решений

```text
Доход протокола стабильный?
├── Да → Нужно governance?
│   ├── Да → Ve-модель (гибрид)
│   └── Нет → Юрисдикция рискованная?
│       ├── Да → Buy-back & burn
│       └── Нет → Revenue share (дивиденды)
└── Нет → Доход растёт?
    ├── Да → Buy-back (масштабируется с доходом)
    └── Нет → Отложить утилизацию до стабилизации
```

{{< cta title="Нужна модель утилизации для вашего токена?" text="Спроектируем механику спроса, рассчитаем доходность для стейкеров и подготовим финмодель." link="https://t.me/karanyuk" button="Обсудить проект →" >}}

---

## Связанные материалы

- [5 моделей предложения токенов]({{< relref "token-supply-models" >}}) — как токены появляются в системе и как распределяются между участниками
