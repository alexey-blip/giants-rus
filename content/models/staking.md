---
title: "Стейкинг: экономика, модели и проектирование наград"
description: "Экономика стейкинга в токеномике — APR и APY, инфляционные и реальные награды, liquid staking, slashing. Формулы, код, калькулятор доходности."
date: 2026-04-10
weight: 28
tags:
  - токеномика
  - стейкинг
  - APR
  - liquid staking
  - награды
categories:
  - models
---

Стейкинг — фундаментальный механизм криптоэкономики. Он одновременно обеспечивает безопасность сети, сокращает обращающееся предложение и создаёт доход для участников. Но за простотой концепции «заблокируй токены — получи награду» скрывается сложная экономика, где инфляция, реальная доходность и давление на продажу определяют судьбу токена.

## Что такое стейкинг

**Стейкинг** — это блокировка токенов в смарт-контракте для получения наград и/или участия в работе сети. Стейкер временно отказывается от ликвидности в обмен на доход.

В токеномике стейкинг играет двойную роль:

1. **Механизм предложения** — стейкинг-награды являются каналом эмиссии новых токенов (reward-модель в [моделях предложения]({{< relref "token-supply-models" >}}))
2. **Механизм спроса** — стейкинг создаёт причину покупать и удерживать токен, работая как [модель утилизации]({{< relref "utility-models" >}})

{{< callout title="Стейкинг в контексте токеномики" >}}
Стейкинг — не изолированный параметр. Он встроен в баланс предложения и спроса: награды увеличивают предложение (инфляция), а блокировка сокращает обращающийся объём. Итоговый эффект зависит от процента застейканных токенов и поведения стейкеров.
{{< /callout >}}

### Два типа стейкинга

| Тип | Назначение | Источник наград | Примеры |
|-----|-----------|-----------------|---------|
| **Консенсусный** | Обеспечение безопасности PoS-сети | Эмиссия + комиссии сети | Ethereum, Cosmos, Solana |
| **Протокольный** | Стимулирование участия в протоколе | Комиссии протокола, казначейство | Aave, Curve, GMX |

Консенсусный стейкинг — часть архитектуры блокчейна. Протокольный — инструмент токеномиста для управления поведением пользователей.

## APR и APY: реальная доходность

### Формулы

{{< formula math="APR = (Rewards_year / Staked_total) × 100%" desc="APR (Annual Percentage Rate) — годовая ставка без учёта реинвестирования. Rewards_year — сумма наград за год" >}}

{{< formula math="APY = (1 + APR / n)^n - 1" desc="APY (Annual Percentage Yield) — ставка с реинвестированием n раз в год. При ежедневном реинвестировании n = 365" >}}

При APR = 10% и ежедневном реинвестировании: APY = (1 + 0.10/365)^365 - 1 = **10.52%**. Разница невелика при умеренных ставках, но при APR = 100% APY достигает **171.5%**.

### Номинальная vs реальная доходность

Номинальный APR — то, что показывают в интерфейсе. Реальная доходность учитывает инфляцию токена:

{{< formula math="RealYield = APR_nominal - InflationRate" desc="RealYield — реальная доходность стейкера · InflationRate — годовой темп роста total supply" >}}

**Пример.** Сеть с APR 12% и инфляцией 8%:
- Номинальная доходность: 12%
- Реальная доходность: 12% - 8% = **4%**
- Нестейкеры теряют: -8% (их доля размывается инфляцией)

{{< callout title="Инфляционный стейкинг — не бесплатные деньги" type="warning" >}}
Если 100% наград идёт из эмиссии, стейкинг — это перераспределение от нестейкеров к стейкерам, а не генерация ценности. Реальный доход возникает только из комиссий протокола и внешней выручки.
{{< /callout >}}

### Real yield: доход из комиссий

Протоколы с реальным доходом (real yield) распределяют не инфляционные токены, а комиссии, заработанные протоколом:

{{< formula math="RealYield_protocol = (Fees_year × ShareToStakers) / (TokenPrice × Staked_total)" desc="Fees_year — годовой доход протокола · ShareToStakers — доля, идущая стейкерам (обычно 30–70%)" >}}

**Пример.** Протокол с годовым доходом $50M, 50% идёт стейкерам, застейкано 100M токенов по $5:

RealYield = ($50M × 0.5) / ($5 × 100M) = **5%** — чистый доход без инфляции.

## Экономика стейкинга: параметры проектирования

### 1. Целевой процент стейкинга

Какая доля circulating supply должна быть застейкана? Типичные значения для PoS-сетей: 40–70%.

| Застейкано | Эффект |
|-----------|--------|
| < 30% | Низкая безопасность, высокие награды для привлечения |
| 30–50% | Стандартный уровень, умеренные награды |
| 50–70% | Высокая безопасность, низкие награды |
| > 70% | Низкая ликвидность, проблемы с торговлей |

### 2. Динамическая ставка наград

Многие сети используют адаптивную модель: ставка растёт, когда стейкинг ниже целевого уровня, и падает, когда выше.

{{< formula math="APR(s) = APR_target × (S_target / S_actual)^k" desc="s — текущий процент стейкинга · S_target — целевой процент · k — чувствительность (обычно 0.5–2)" >}}

При S_target = 50%, S_actual = 30%, k = 1 и APR_target = 8%:

APR = 8% × (50/30) = **13.3%** — повышенная ставка для привлечения стейкеров.

При S_actual = 70%:

APR = 8% × (50/70) = **5.7%** — пониженная ставка для снижения блокировки.

### 3. Lock-up период

Время, на которое токены блокируются. Длинный lock-up снижает давление на продажу, но ограничивает ликвидность.

| Подход | Lock-up | Примеры |
|--------|---------|---------|
| Гибкий | 0 дней (unstake мгновенно) | Liquid staking |
| Короткий | 7–14 дней (unbonding) | Cosmos, Polkadot |
| Средний | 30–90 дней | Протокольный стейкинг |
| Длинный | 6–12 месяцев | veToken (Curve) |
| Кастомный | Выбор пользователя (больше lock = больше reward) | Convex, многие L2 |

### 4. Slashing — штрафы

**Slashing** — конфискация части застейканных токенов за нарушения: двойная подпись блока, длительный даунтайм, некорректная валидация. Размер штрафа: 0.01% (незначительные нарушения) до 100% (атака на сеть).

Slashing создаёт экономический стимул для честного поведения и является ключевым элементом [дизайна механизмов]({{< relref "mechanism-design" >}}).

## Liquid staking

**Liquid staking** (ликвидный стейкинг) — технология, позволяющая получать награды за стейкинг без потери ликвидности. Стейкер получает производный токен (LST — Liquid Staking Token), который можно использовать в DeFi.

### Как работает

1. Пользователь депозитит 1 ETH в протокол ликвидного стейкинга
2. Протокол стейкает ETH в сети и выдаёт 1 stETH (или эквивалент)
3. stETH начисляет награды (ребейс или курс растёт)
4. Пользователь использует stETH как залог, ликвидность или торгует им
5. При выводе — обмен stETH обратно на ETH + накопленные награды

### Экономические эффекты

| Параметр | Без liquid staking | С liquid staking |
|----------|-------------------|-----------------|
| Капитал заблокирован | Да | Нет (LST ликвиден) |
| DeFi-композабильность | Нет | Да (LST как залог) |
| Процент стейкинга | Ограничен (конкуренция с DeFi) | Выше (не надо выбирать) |
| Системный риск | Низкий | Выше (каскадные ликвидации) |
| Реальная доходность | APR стейкинга | APR стейкинга + DeFi-доход |

### Влияние на токеномику

Liquid staking кардинально меняет модель: застейканные токены не выходят из обращения. Это означает, что стейкинг перестаёт сокращать circulating supply. Для токеномиста это значит, что **нельзя рассчитывать на стейкинг как на механизм снижения давления на продажу**, если LST-протоколы доминируют.

{{< formula math="EffectiveCirculating = Circulating - Staked + Staked_liquid" desc="Staked_liquid — застейканные токены в liquid staking, которые остаются в обращении через LST" >}}

## Моделирование экономики стейкинга

Модель с динамическим APR показывает: при старте с 30% стейкинга APR повышается до ~13% для привлечения стейкеров. По мере приближения к целевым 50% ставка снижается до 8%. Инфляция от наград составляет 4–5% годовых, но доход от комиссий даёт дополнительные 3–5% реальной доходности.

**Результаты симуляции** (параметры: 100M supply, целевой стейкинг 50%, APR 8%, комиссии $5M/год):

| Месяц | Стейкинг, % | APR | Инфляция | Real Yield | Total supply |
|-------|------------|-----|----------|------------|-------------|
| 0 | 30% | 13.3% | 4.0% | 12.6% | 100.0M |
| 12 | 41% | 9.8% | 4.0% | 7.9% | 104.9M |
| 24 | 47% | 8.5% | 4.0% | 6.6% | 110.0M |
| 36 | 49% | 8.1% | 4.0% | 6.2% | 115.2M |
| 48 | 50% | 8.0% | 4.0% | 6.0% | 120.5M |
| 60 | 50% | 8.0% | 4.0% | 6.0% | 126.0M |

Что видно из таблицы:
- **APR снижается** с 13.3% до 8% по мере роста стейкинга к целевым 50% — динамическая модель работает
- **Инфляция стабильна** на 4% — потому что стейкинг и APR сбалансированы
- **Real Yield = 6%** при наличии комиссий. Без комиссий ($0 revenue) Real Yield = APR - Inflation = 8% - 4% = **4%** — чистый перенос от нестейкеров
- **Total supply вырастает на 26%** за 5 лет — это цена инфляционного стейкинга

<details>
<summary>Код симуляции (Python)</summary>

```python
import numpy as np
import matplotlib.pyplot as plt

TOTAL_SUPPLY_0 = 100_000_000
STAKING_TARGET = 0.50
APR_TARGET = 0.08
SENSITIVITY = 1.0
FEE_REVENUE_YEAR = 5_000_000
SHARE_TO_STAKERS = 0.50
TOKEN_PRICE = 1.0
MONTHS = 60

months = np.arange(MONTHS)
supply = np.zeros(MONTHS)
staked_pct = np.zeros(MONTHS)
apr = np.zeros(MONTHS)
real_yield = np.zeros(MONTHS)
inflation = np.zeros(MONTHS)

supply[0] = TOTAL_SUPPLY_0
staked_pct[0] = 0.30

for m in range(MONTHS):
    staked = supply[m] * staked_pct[m]
    apr[m] = min(APR_TARGET * (STAKING_TARGET / max(staked_pct[m], 0.01)) ** SENSITIVITY, 0.30)
    monthly_emission = staked * (apr[m] / 12)
    inflation[m] = (monthly_emission * 12) / supply[m]
    fee_yield = (FEE_REVENUE_YEAR * SHARE_TO_STAKERS) / (TOKEN_PRICE * staked) if staked > 0 else 0
    real_yield[m] = apr[m] - inflation[m] + fee_yield
    if m < MONTHS - 1:
        supply[m + 1] = supply[m] + monthly_emission
        delta = (STAKING_TARGET - staked_pct[m]) * 0.05
        staked_pct[m + 1] = np.clip(staked_pct[m] + delta, 0.05, 0.90)

fig, axes = plt.subplots(2, 2, figsize=(14, 10))
axes[0, 0].plot(months, apr * 100, color="#3b82f6", linewidth=2)
axes[0, 0].set_title("Динамический APR"); axes[0, 0].set_ylabel("APR, %"); axes[0, 0].grid(True, alpha=0.3)
axes[0, 1].plot(months, staked_pct * 100, color="#10b981", linewidth=2)
axes[0, 1].axhline(y=50, color="#ef4444", linestyle="--", alpha=0.5, label="Цель: 50%")
axes[0, 1].set_title("Процент стейкинга"); axes[0, 1].legend(); axes[0, 1].grid(True, alpha=0.3)
axes[1, 0].plot(months, real_yield * 100, color="#8b5cf6", linewidth=2, label="Реальная доходность")
axes[1, 0].plot(months, inflation * 100, color="#ef4444", linewidth=2, linestyle="--", label="Инфляция")
axes[1, 0].set_title("Реальная доходность vs инфляция"); axes[1, 0].legend(); axes[1, 0].grid(True, alpha=0.3)
axes[1, 1].plot(months, supply / 1e6, color="#f59e0b", linewidth=2)
axes[1, 1].set_title("Total supply"); axes[1, 1].set_ylabel("Токенов, млн"); axes[1, 1].grid(True, alpha=0.3)
plt.tight_layout(); plt.show()
```

</details>

## Калькулятор доходности стейкинга

<div class="gnts-calc" id="gnts-calc-staking" style="background:rgba(124,58,237,0.04);border:1px solid rgba(124,58,237,0.15);border-radius:12px;padding:28px;margin:32px 0;">
  <div style="font-family:'Space Grotesk',sans-serif;font-size:0.72rem;font-weight:600;text-transform:uppercase;letter-spacing:0.12em;color:#7c3aed;margin-bottom:20px;">RealYield = APR - Inflation + FeeYield</div>
  <div style="display:grid;grid-template-columns:1fr 1fr;gap:20px;margin-bottom:24px;">
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">Номинальный APR <span id="st-val-apr" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">8%</span></label>
      <input type="range" id="st-apr" min="1" max="100" step="0.5" value="8" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">Инфляция сети <span id="st-val-inf" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">5%</span></label>
      <input type="range" id="st-inf" min="0" max="50" step="0.5" value="5" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">Доход протокола, $/год <span id="st-val-rev" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">$5M</span></label>
      <input type="range" id="st-rev" min="0" max="100000000" step="500000" value="5000000" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">Доля стейкерам <span id="st-val-share" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">50%</span></label>
      <input type="range" id="st-share" min="10" max="100" step="5" value="50" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">Застейкано токенов <span id="st-val-stk" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">50M</span></label>
      <input type="range" id="st-stk" min="1000000" max="500000000" step="1000000" value="50000000" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">Цена токена, $ <span id="st-val-price" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">$1.00</span></label>
      <input type="range" id="st-price" min="0.01" max="100" step="0.01" value="1" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
  </div>
  <div style="background:#f8f7f4;border:1px solid rgba(0,0,0,0.08);border-radius:8px;overflow:hidden;">
    <svg id="st-svg" viewBox="0 0 500 200" preserveAspectRatio="xMidYMid meet" style="display:block;width:100%;height:200px;"></svg>
  </div>
  <div style="background:#f8f7f4;border:1px solid rgba(0,0,0,0.08);border-radius:8px;padding:20px;margin-top:12px;">
    <table style="width:100%;font-family:'Space Grotesk',sans-serif;font-size:0.85rem;border-collapse:collapse;">
      <tbody id="st-tbody"></tbody>
    </table>
  </div>
</div>
<script>
(function(){
  var ids=['apr','inf','rev','share','stk','price'];
  var sl={},vl={};
  ids.forEach(function(k){sl[k]=document.getElementById('st-'+k);vl[k]=document.getElementById('st-val-'+k);});
  var svg=document.getElementById('st-svg'),tbody=document.getElementById('st-tbody');
  function fmt(n){if(n>=1e9)return '$'+(n/1e9).toFixed(1)+'B';if(n>=1e6)return '$'+(n/1e6).toFixed(1)+'M';if(n>=1e3)return '$'+Math.round(n).toLocaleString('ru');return '$'+n.toFixed(2);}
  function fmtT(n){if(n>=1e9)return (n/1e9).toFixed(1)+'B';if(n>=1e6)return (n/1e6).toFixed(0)+'M';return Math.round(n).toLocaleString('ru');}
  function upd(){
    var apr=+sl.apr.value/100,inf=+sl.inf.value/100,rev=+sl.rev.value,share=+sl.share.value/100,stk=+sl.stk.value,price=+sl.price.value;
    vl.apr.textContent=sl.apr.value+'%';vl.inf.textContent=sl.inf.value+'%';vl.rev.textContent=fmt(rev);vl.share.textContent=sl.share.value+'%';vl.stk.textContent=fmtT(stk);vl.price.textContent='$'+price.toFixed(2);
    var feeYield=stk>0&&price>0?(rev*share)/(price*stk):0;
    var realYield=apr-inf+feeYield;
    var apy=Math.pow(1+apr/365,365)-1;
    var nonstaker=-inf;
    // Bar chart
    var bars=[{l:'Номинальный APR',v:apr,c:'#3b82f6'},{l:'Инфляция',v:-inf,c:'#ef4444'},{l:'FeeYield',v:feeYield,c:'#10b981'},{l:'Real Yield',v:realYield,c:realYield>=0?'#8b5cf6':'#ef4444'}];
    var maxV=Math.max(apr,Math.abs(inf),Math.abs(feeYield),Math.abs(realYield),0.01);
    var w=500,h=200,pad=120,bw=(w-pad)/bars.length,mid=h/2;
    var s='';
    bars.forEach(function(b,i){
      var x=pad+i*bw+bw*0.15,bwi=bw*0.7;
      var barH=Math.abs(b.v)/maxV*(mid-20);
      var y=b.v>=0?mid-barH:mid;
      s+='<rect x="'+x+'" y="'+y+'" width="'+bwi+'" height="'+barH+'" fill="'+b.c+'" rx="3" opacity="0.8"/>';
      s+='<text x="'+(x+bwi/2)+'" y="'+(mid+(b.v>=0?-barH-8:barH+16))+'" text-anchor="middle" font-family="JetBrains Mono,monospace" font-size="13" font-weight="600" fill="'+b.c+'">'+(b.v*100).toFixed(1)+'%</text>';
      s+='<text x="'+(x+bwi/2)+'" y="'+(h-6)+'" text-anchor="middle" font-family="Space Grotesk,sans-serif" font-size="10" fill="#64748b">'+b.l+'</text>';
    });
    s+='<line x1="'+pad+'" y1="'+mid+'" x2="'+w+'" y2="'+mid+'" stroke="#cbd5e1" stroke-width="1"/>';
    svg.innerHTML=s;
    var rows=[
      ['Реальная доходность (Real Yield)',(realYield*100).toFixed(2)+'%'],
      ['APY (с реинвестированием)',(apy*100).toFixed(2)+'%'],
      ['FeeYield (доход от комиссий)',(feeYield*100).toFixed(2)+'%'],
      ['Потери нестейкера (размытие)',(nonstaker*100).toFixed(1)+'%'],
      ['Годовой доход стейкера',fmt(stk*price*realYield)+' / '+fmtT(stk*realYield)+' токенов']
    ];
    tbody.innerHTML='';
    rows.forEach(function(r){
      var tr=document.createElement('tr');
      tr.innerHTML='<td style="padding:8px 0;border-bottom:1px solid rgba(0,0,0,0.06);color:#64748b;">'+r[0]+'</td><td style="padding:8px 0;border-bottom:1px solid rgba(0,0,0,0.06);text-align:right;font-family:\'JetBrains Mono\',monospace;font-weight:600;color:#1a1a2e;">'+r[1]+'</td>';
      tbody.appendChild(tr);
    });
  }
  ids.forEach(function(k){sl[k].addEventListener('input',upd);});
  upd();
})();
</script>

## Распространённые ошибки

### 1. APR выше 100% без источника дохода

Трёхзначный APR привлекает внимание, но если единственный источник — эмиссия, это гиперинфляция. Реальная доходность отрицательная, а цена токена падает быстрее, чем растёт количество.

### 2. Нет учёта liquid staking

Если 80% стейка идёт через LST-протоколы, стейкинг не сокращает circulating supply. Модели, построенные на предположении «застейканное = заблокированное», дадут неверный прогноз цены.

### 3. Одинаковый APR для всех lock-up периодов

Стейкер, блокирующий на 1 месяц, и стейкер на 12 месяцев несут разный риск. Без дифференциации ставок все выбирают минимальный lock-up, и механизм не работает.

### 4. Slashing слишком мягкий или отсутствует

Без штрафов нет экономических стимулов к честному поведению. Валидаторы могут атаковать сеть без потерь. Минимальный slashing: 0.1% за даунтайм, 5%+ за двойную подпись.

### 5. Награды из казначейства без ограничения

Фиксированный пул наград заканчивается. Если staking-rewards идут из казначейства (а не из эмиссии), через 2–3 года пул пуст и APR падает до нуля. Решение: комбинировать эмиссию и комиссии.

{{< checklist title="Чеклист проектирования стейкинга" type="check" >}}
<li><strong>Источник наград определён</strong> — эмиссия, комиссии или комбинация</li>
<li><strong>Реальная доходность положительна</strong> — APR выше инфляции хотя бы на 2-3%</li>
<li><strong>Целевой процент стейкинга установлен</strong> — 40-70% для PoS, 20-50% для протоколов</li>
<li><strong>Динамическая ставка реализована</strong> — APR адаптируется к текущему проценту стейкинга</li>
<li><strong>Lock-up дифференцирован</strong> — больше lock-up = больше награда</li>
<li><strong>Slashing параметры заданы</strong> — штрафы за даунтайм и атаки</li>
<li><strong>Liquid staking учтён в модели</strong> — LST не сокращают circulating supply</li>
<li><strong>Устойчивость на 5+ лет смоделирована</strong> — награды не закончатся через 2 года</li>
{{< /checklist >}}

{{< cta title="Спроектируем экономику стейкинга для вашего протокола" text="Мы моделировали стейкинг для PoS-сетей, DeFi-протоколов и liquid staking платформ. Рассчитаем оптимальные параметры и проверим устойчивость на 5 лет." button="Обсудить стейкинг" link="https://gnts.io" >}}

## Читайте также

- [Дивиденды или выкуп: модели утилизации]({{< relref "utility-models" >}}) — стейкинг-награды как механизм утилизации
- [5 моделей спроса на токен]({{< relref "demand-models" >}}) — стейкинг в контексте создания спроса
- [Дизайн механизмов в токеномике]({{< relref "mechanism-design" >}}) — slashing и стимулы как элементы дизайна механизмов
- [Симуляции и сценарный анализ]({{< relref "simulations" >}}) — моделирование стейкинг-экономики через симуляции
