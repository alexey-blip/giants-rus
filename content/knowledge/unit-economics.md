---
title: "Юнит-экономика токен-проекта: метрики, формулы и моделирование"
description: "Как рассчитать юнит-экономику криптопроекта — LTV, CAC, доходность на токен, скорость обращения. Формулы, Python-код, калькулятор и типичные ошибки."
date: 2026-04-10
weight: 15
tags:
  - токеномика
  - юнит-экономика
  - LTV
  - CAC
  - velocity
categories:
  - knowledge
---

Токеномика без юнит-экономики — это аллокация процентов, за которыми нет денег. Юнит-экономика отвечает на вопрос: **сколько стоит привлечь одного пользователя, сколько он приносит и как токен влияет на эти числа**. Без этого расчёта любые модели предложения и спроса остаются абстракцией.

## Что такое юнит-экономика в контексте токенов

**Юнит-экономика** — расчёт доходности на единицу (пользователя, транзакцию, подписку). В токен-проектах добавляется второй слой: помимо фиатной выручки, существует токен-экономика — эмиссия, стимулы и сток токенов, которые влияют на P&L.

В традиционном бизнесе формула проста: если LTV > CAC, бизнес масштабируется. В криптопроекте LTV включает доход от использования **и** влияние токена (субсидии, эмиссия, сжигание), а CAC может оплачиваться токенами (airdrop, реферальные программы, ликвидити-майнинг).

### Зачем считать юнит-экономику

1. **Проверка устойчивости** — окупается ли привлечение пользователей без бесконечной эмиссии?
2. **Калибровка эмиссии** — сколько токенов тратить на стимулы, чтобы не уйти в гиперинфляцию?
3. **Оценка токена** — какая фундаментальная цена токена при заданных метриках использования?
4. **Привлечение инвесторов** — инвесторы вкладывают в метрики, а не в обещания

### Почему в web3 это сложнее, чем в традиционном бизнесе

В классическом SaaS юнит-экономика линейна: привлёк пользователя → он платит подписку → окупился через N месяцев. В токен-проекте добавляется нелинейный фактор: **токен — это по сути кредиторская задолженность проекта**, привязанная к утилизации и рыночной цене.

Это означает, что юнит-экономика токен-проекта **принципиально не может быть точной**:

- **CAC зависит от цены токена** — а цена зависит от спроса, который зависит от количества пользователей, которых вы привлекаете за этот CAC. Циклическая зависимость
- **LTV зависит от утилизации токена** — если пользователи перестанут стейкать или burn-механизм изменится, ценность токена и LTV изменятся
- **Рыночная цена влияет на всё** — при росте цены токена растёт и «стоимость» стимулов (CAC), и «ценность» пользовательского вклада (LTV). Эти эффекты не компенсируют друг друга

{{< callout title="Юнит-экономика как ориентир, не как истина" type="info" >}}
Токен-проект не может рассчитать юнит-экономику с точностью SaaS. Но может определить **порядок величин и направление**: растёт LTV/CAC или падает? Сходится экономика при трёх сценариях цены или нет? Именно это и нужно инвесторам и команде.
{{< /callout >}}

## Ключевые метрики

### CAC — стоимость привлечения пользователя

{{< formula math="CAC = (MarketingSpend + TokenIncentives × P_token) / NewUsers" desc="MarketingSpend — фиатные расходы на маркетинг · TokenIncentives — токены на привлечение · P_token — рыночная цена токена" >}}

В криптопроектах CAC часто включает токен-составляющую: [airdrop]({{< relref "models/airdrop" >}}), реферальные награды, ликвидити-майнинг. Эти расходы реальны — они создают давление на продажу.

**Критический момент: CAC зависит от P_token.** В формуле выше P_token — текущая рыночная цена токена. Это значит, что CAC **не является константой**: при росте цены токена в 5 раз расходы на стимулы вырастают пропорционально (если количество раздаваемых токенов фиксировано). Проект, который раздаёт 100K токенов/мес, тратит $50K при цене $0.50 и $500K при цене $5.00 — за тех же самых пользователей.

**Пример.** Протокол тратит $50K/мес на маркетинг и раздаёт 100 000 токенов в месяц. Привлечено 2 000 новых пользователей:

| P_token | Стоимость токенов | CAC | Комментарий |
|---------|-------------------|-----|-------------|
| $0.10 | $10K | $30 | Дёшево привлекать |
| $0.50 | $50K | $50 | Приемлемо |
| $2.00 | $200K | $125 | Дорого |
| $5.00 | $500K | $275 | Нерентабельно |

Именно поэтому многие проекты переходят с фиксированного количества токенов на фиксированный долларовый бюджет стимулов — чтобы CAC не зависел от рыночной цены.

{{< callout title="Токен-субсидии — это реальные расходы" type="warning" >}}
Раздача токенов «бесплатна» только если токен ничего не стоит. Каждый розданный токен — это давление на продажу, которое размывает стоимость для всех держателей. Включайте токен-стимулы в CAC по рыночной цене — и моделируйте минимум три сценария цены.
{{< /callout >}}

### ARPU — средний доход на пользователя

{{< formula math="ARPU_month = Revenue_month / ActiveUsers" desc="Revenue_month — фиатный доход протокола · ActiveUsers — активные пользователи за месяц (MAU)" >}}

В DeFi-протоколах доход складывается из:
- Комиссии за свопы (DEX)
- Проценты по кредитам (лендинг)
- Комиссии за мосты (кроссчейн)
- Плата за хранение (децентрализованные хранилища)
- Подписки (premium-функции)

### LTV — пожизненная ценность пользователя

{{< formula math="LTV = ARPU_month × AvgLifespan_months" desc="AvgLifespan — средний срок жизни пользователя в протоколе (в месяцах)" >}}

Для подписочных моделей: LTV = ARPU / ChurnRate. Для транзакционных: LTV = ARPU × среднее количество активных месяцев.

**Пример.** ARPU = $5/мес, средний пользователь активен 8 месяцев:

LTV = $5 × 8 = **$40**

При CAC = $50 юнит-экономика **отрицательная**: проект теряет $10 на каждом пользователе.

### LTV/CAC — коэффициент окупаемости

{{< formula math="LTV/CAC > 3 — здоровая юнит-экономика" desc="LTV/CAC < 1 — убыточность · 1–3 — зона риска · > 3 — масштабируемый бизнес" >}}

| LTV/CAC | Интерпретация |
|---------|--------------|
| < 1.0 | Убыточно — каждый пользователь стоит больше, чем приносит |
| 1.0–2.0 | Рискованно — работает при идеальном удержании |
| 2.0–3.0 | Приемлемо — но мало запаса на ошибку |
| 3.0–5.0 | Здорово — стандарт для масштабирования |
| > 5.0 | Отлично — или недоинвестирование в рост |

### Velocity — скорость обращения токена

{{< formula math="V = TransactionVolume / MarketCap" desc="V — velocity (скорость обращения) · TransactionVolume — объём транзакций за период · MarketCap — рыночная капитализация" >}}

Velocity показывает, как быстро токены переходят из рук в руки. Высокая velocity (> 10) означает, что пользователи не держат токен — покупают и тут же тратят. Это снижает фундаментальную цену.

{{< formula math="P_fundamental = PQ / (V × S)" desc="Уравнение обмена Фишера, адаптированное для токеномики · P — цена единицы услуги · Q — объём услуг · V — velocity · S — circulating supply" >}}

**Пример.** Протокол обрабатывает транзакции на $100M/год (PQ), velocity = 20, circulating supply = 50M:

P_fundamental = $100M / (20 × 50M) = **$0.10 за токен**

Если снизить velocity до 5 (через стейкинг, lock-up, [модели утилизации]({{< relref "utility-models" >}})):

P_fundamental = $100M / (5 × 50M) = **$0.40** — рост в 4 раза.

{{< callout title="Velocity — главный враг цены" type="info" >}}
Механизмы снижения velocity (стейкинг, lock-up, burn при использовании) критически важны для поддержания цены. Без них токен становится транзитным средством — его покупают и сразу продают, не создавая устойчивого спроса.
{{< /callout >}}

## Юнит-экономика с учётом токена

### Токен-субсидированная модель

Большинство криптопроектов на ранней стадии субсидируют пользователей: дают токены за использование (ликвидити-майнинг, награды, [airdrop]({{< relref "models/airdrop" >}})). Это создаёт искусственно высокий ARPU для пользователей — и отрицательную экономику для протокола.

{{< formula math="NetRevenue = FiatRevenue - TokenSubsidies × P_token" desc="Чистый доход = фиатная выручка минус стоимость розданных токенов" >}}

**Пример.** DEX генерирует $2M/мес комиссий и раздаёт 500K токенов/мес по $3:

NetRevenue = $2M - 500K × $3 = **$500K** (чистый доход)

Если цена токена вырастет до $5: NetRevenue = $2M - 500K × $5 = **-$500K** (убыток)

### Точка безубыточности

{{< formula math="Users_breakeven = TotalCosts / (ARPU_month - TokenCost_per_user)" desc="Количество пользователей, при котором доход покрывает расходы и токен-субсидии" >}}

### Модель погашения долга

Токен-субсидии на раннем этапе — это «долг» перед будущими держателями. Каждый розданный токен создаёт потенциальное давление на продажу. Здоровая экономика предполагает, что:

1. Субсидии привлекают пользователей (фаза роста)
2. Пользователи генерируют доход (фаза монетизации)
3. Доход покрывает давление от продажи розданных токенов (фаза устойчивости)

## Моделирование юнит-экономики

Симуляция на 36 месяцев показывает: при росте цены токена на 2%/мес расходы на стимулы к 36-му месяцу удваиваются, а CAC растёт с $75 до $125+. При этом LTV остаётся $62.50 (зависит от ARPU и churn, не от цены токена). В итоге LTV/CAC ухудшается даже при росте MAU.

**Ключевые выводы:**
- Без дохода от комиссий точка безубыточности не достигается за 36 месяцев
- При фиксированном количестве раздаваемых токенов рост цены **ухудшает** юнит-экономику
- Переход на фиксированный долларовый бюджет стимулов стабилизирует CAC

<details>
<summary>Код симуляции (Python)</summary>

```python
import numpy as np
import matplotlib.pyplot as plt

MONTHS = 36
MARKETING_SPEND = 50_000
TOKEN_INCENTIVES = 100_000
TOKEN_PRICE_0 = 1.0
NEW_USERS_MONTH = 2_000
CHURN_RATE = 0.08
ARPU = 5.0
PRICE_GROWTH = 0.02

months = np.arange(MONTHS)
active_users = np.zeros(MONTHS)
revenue = np.zeros(MONTHS)
token_cost = np.zeros(MONTHS)
net_revenue = np.zeros(MONTHS)
cumulative_pnl = np.zeros(MONTHS)
cac = np.zeros(MONTHS)
ltv = np.zeros(MONTHS)

for m in range(MONTHS):
    token_price = TOKEN_PRICE_0 * (1 + PRICE_GROWTH) ** m
    if m == 0:
        active_users[m] = NEW_USERS_MONTH
    else:
        active_users[m] = active_users[m - 1] * (1 - CHURN_RATE) + NEW_USERS_MONTH
    revenue[m] = active_users[m] * ARPU
    token_cost[m] = TOKEN_INCENTIVES * token_price
    total_cost = MARKETING_SPEND + token_cost[m]
    net_revenue[m] = revenue[m] - total_cost
    cumulative_pnl[m] = (cumulative_pnl[m - 1] if m > 0 else 0) + net_revenue[m]
    cac[m] = total_cost / NEW_USERS_MONTH
    ltv[m] = ARPU * (1 / CHURN_RATE)

fig, axes = plt.subplots(2, 2, figsize=(14, 10))
axes[0, 0].plot(months, active_users, color="#3b82f6", linewidth=2)
axes[0, 0].set_title("MAU"); axes[0, 0].grid(True, alpha=0.3)
axes[0, 1].plot(months, revenue / 1000, color="#10b981", linewidth=2, label="Доход")
axes[0, 1].plot(months, (np.full(MONTHS, MARKETING_SPEND) + token_cost) / 1000, color="#ef4444", linewidth=2, label="Расходы")
axes[0, 1].set_title("Доход vs расходы"); axes[0, 1].legend(); axes[0, 1].grid(True, alpha=0.3)
axes[1, 0].fill_between(months, cumulative_pnl / 1000, where=cumulative_pnl >= 0, alpha=0.3, color="#10b981")
axes[1, 0].fill_between(months, cumulative_pnl / 1000, where=cumulative_pnl < 0, alpha=0.3, color="#ef4444")
axes[1, 0].plot(months, cumulative_pnl / 1000, color="#1e293b", linewidth=2)
axes[1, 0].set_title("Кумулятивный P&L"); axes[1, 0].grid(True, alpha=0.3)
axes[1, 1].plot(months, ltv / cac, color="#8b5cf6", linewidth=2)
axes[1, 1].axhline(y=3.0, color="#10b981", linestyle="--", alpha=0.5, label="3x")
axes[1, 1].axhline(y=1.0, color="#ef4444", linestyle="--", alpha=0.5, label="1x")
axes[1, 1].set_title("LTV / CAC"); axes[1, 1].legend(); axes[1, 1].grid(True, alpha=0.3)
plt.tight_layout(); plt.show()
```

</details>

## Калькулятор юнит-экономики

<div class="gnts-calc" id="gnts-calc-ue" style="background:rgba(124,58,237,0.04);border:1px solid rgba(124,58,237,0.15);border-radius:12px;padding:28px;margin:32px 0;">
  <div style="font-family:'Space Grotesk',sans-serif;font-size:0.72rem;font-weight:600;text-transform:uppercase;letter-spacing:0.12em;color:#7c3aed;margin-bottom:20px;">LTV / CAC = (ARPU / Churn) / ((Marketing + Tokens × P) / NewUsers)</div>
  <div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:16px;margin-bottom:24px;">
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">Маркетинг, $/мес <span id="ue-val-mkt" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">$50K</span></label>
      <input type="range" id="ue-mkt" min="0" max="500000" step="5000" value="50000" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">Токенов/мес <span id="ue-val-tok" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">100K</span></label>
      <input type="range" id="ue-tok" min="0" max="1000000" step="10000" value="100000" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">Цена токена, $ <span id="ue-val-price" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">$1.00</span></label>
      <input type="range" id="ue-price" min="0.01" max="10" step="0.01" value="1" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">Новых юзеров/мес <span id="ue-val-usr" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">2 000</span></label>
      <input type="range" id="ue-usr" min="100" max="50000" step="100" value="2000" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">ARPU, $/мес <span id="ue-val-arpu" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">$5.00</span></label>
      <input type="range" id="ue-arpu" min="0.1" max="100" step="0.1" value="5" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">Отток юзеров, %/мес <span id="ue-val-churn" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">8%</span></label>
      <input type="range" id="ue-churn" min="1" max="30" step="0.5" value="8" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
  </div>
  <div style="background:#f8f7f4;border:1px solid rgba(0,0,0,0.08);border-radius:8px;overflow:hidden;">
    <svg id="ue-svg" viewBox="0 0 500 180" preserveAspectRatio="xMidYMid meet" style="display:block;width:100%;height:180px;"></svg>
  </div>
  <div style="background:#f8f7f4;border:1px solid rgba(0,0,0,0.08);border-radius:8px;padding:20px;margin-top:12px;">
    <table style="width:100%;font-family:'Space Grotesk',sans-serif;font-size:0.85rem;border-collapse:collapse;">
      <tbody id="ue-tbody"></tbody>
    </table>
  </div>
  <div id="ue-warning" style="display:none;background:rgba(239,68,68,0.08);border:1px solid rgba(239,68,68,0.2);border-radius:8px;padding:12px;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;color:#dc2626;margin-top:12px;"></div>
</div>
<script>
(function(){
  var ids=['mkt','tok','price','usr','arpu','churn'];
  var sl={},vl={};
  ids.forEach(function(k){sl[k]=document.getElementById('ue-'+k);vl[k]=document.getElementById('ue-val-'+k);});
  var svg=document.getElementById('ue-svg'),tbody=document.getElementById('ue-tbody'),warn=document.getElementById('ue-warning');
  function fmt(n){if(Math.abs(n)>=1e6)return '$'+(n/1e6).toFixed(1)+'M';if(Math.abs(n)>=1e3)return '$'+(n/1e3).toFixed(1)+'K';return '$'+n.toFixed(0);}
  function fmtT(n){if(n>=1e6)return (n/1e6).toFixed(0)+'M';if(n>=1e3)return (n/1e3).toFixed(0)+'K';return n.toFixed(0);}
  function upd(){
    var mkt=+sl.mkt.value,tok=+sl.tok.value,price=+sl.price.value,usr=+sl.usr.value,arpu=+sl.arpu.value,churn=+sl.churn.value/100;
    vl.mkt.textContent=fmt(mkt);vl.tok.textContent=fmtT(tok);vl.price.textContent='$'+price.toFixed(2);vl.usr.textContent=usr.toLocaleString('ru');vl.arpu.textContent='$'+arpu.toFixed(2);vl.churn.textContent=sl.churn.value+'%';
    var tokenCost=tok*price;var totalCost=mkt+tokenCost;var cac=usr>0?totalCost/usr:0;
    var lifespan=churn>0?1/churn:999;var ltv=arpu*lifespan;
    var ltvcac=cac>0?ltv/cac:0;
    // Bar chart: CAC breakdown vs LTV
    var w=500,h=180,pad=60;
    var maxVal=Math.max(cac,ltv,1);
    var s='';
    // CAC bar (stacked: marketing + token)
    var mktPart=usr>0?mkt/usr:0;var tokPart=usr>0?tokenCost/usr:0;
    var bh=40,y1=50,y2=110;
    var mktW=mktPart/maxVal*(w-pad-80);var tokW=tokPart/maxVal*(w-pad-80);
    var ltvW=ltv/maxVal*(w-pad-80);
    s+='<text x="'+pad+'" y="'+(y1-8)+'" font-family="Space Grotesk,sans-serif" font-size="11" fill="#64748b">CAC: $'+cac.toFixed(0)+'</text>';
    s+='<rect x="'+pad+'" y="'+y1+'" width="'+mktW+'" height="'+bh+'" fill="#3b82f6" rx="4" opacity="0.8"/>';
    s+='<rect x="'+(pad+mktW)+'" y="'+y1+'" width="'+tokW+'" height="'+bh+'" fill="#f59e0b" rx="0" opacity="0.8"/>';
    if(mktW>50)s+='<text x="'+(pad+mktW/2)+'" y="'+(y1+bh/2+5)+'" text-anchor="middle" font-family="JetBrains Mono,monospace" font-size="10" fill="white">Маркетинг $'+mktPart.toFixed(0)+'</text>';
    if(tokW>50)s+='<text x="'+(pad+mktW+tokW/2)+'" y="'+(y1+bh/2+5)+'" text-anchor="middle" font-family="JetBrains Mono,monospace" font-size="10" fill="white">Токены $'+tokPart.toFixed(0)+'</text>';
    s+='<text x="'+pad+'" y="'+(y2-8)+'" font-family="Space Grotesk,sans-serif" font-size="11" fill="#64748b">LTV: $'+ltv.toFixed(0)+'</text>';
    var ltvColor=ltv>=cac?'#10b981':'#ef4444';
    s+='<rect x="'+pad+'" y="'+y2+'" width="'+ltvW+'" height="'+bh+'" fill="'+ltvColor+'" rx="4" opacity="0.8"/>';
    if(ltvW>50)s+='<text x="'+(pad+ltvW/2)+'" y="'+(y2+bh/2+5)+'" text-anchor="middle" font-family="JetBrains Mono,monospace" font-size="11" fill="white">$'+ltv.toFixed(0)+' ('+lifespan.toFixed(1)+' мес)</text>';
    svg.innerHTML=s;
    var rows=[
      ['CAC (фиат + токены)','$'+cac.toFixed(2)],
      ['   в т.ч. токен-составляющая','$'+tokPart.toFixed(2)+' ('+(cac>0?(tokPart/cac*100).toFixed(0):0)+'% CAC)'],
      ['LTV (ARPU × срок жизни)','$'+ltv.toFixed(2)+' ('+lifespan.toFixed(1)+' мес)'],
      ['LTV / CAC',ltvcac.toFixed(2)+'×'],
      ['Месячный чистый доход на юзера','$'+(arpu-cac/lifespan).toFixed(2)]
    ];
    tbody.innerHTML='';
    rows.forEach(function(r){
      var tr=document.createElement('tr');
      tr.innerHTML='<td style="padding:6px 0;border-bottom:1px solid rgba(0,0,0,0.06);color:#64748b;">'+r[0]+'</td><td style="padding:6px 0;border-bottom:1px solid rgba(0,0,0,0.06);text-align:right;font-family:\'JetBrains Mono\',monospace;font-weight:600;color:#1a1a2e;">'+r[1]+'</td>';
      tbody.appendChild(tr);
    });
    var warnings=[];
    if(ltvcac<1)warnings.push('LTV/CAC < 1 — юнит-экономика отрицательная, проект теряет деньги на каждом пользователе');
    if(ltvcac>=1&&ltvcac<3)warnings.push('LTV/CAC < 3 — зона риска, мало запаса на ошибку');
    if(tokPart/cac>0.7)warnings.push('Токен-составляющая > 70% CAC — при росте цены токена CAC резко вырастет');
    if(warnings.length){warn.style.display='block';warn.innerHTML=warnings.join('<br>');}else{warn.style.display='none';}
  }
  ids.forEach(function(k){sl[k].addEventListener('input',upd);});
  upd();
})();
</script>

Двигайте ползунки и наблюдайте, как меняется соотношение LTV/CAC. Обратите внимание: при увеличении цены токена CAC растёт (жёлтая часть полоски), а LTV остаётся прежним.

## Оценка стоимости токена через юнит-экономику

### Метод дисконтированных потоков

{{< formula math="TokenValue = Σ (NetCashFlow_t / (1 + r)^t) / TotalSupply" desc="r — ставка дисконтирования (20–40% для крипто) · NetCashFlow — чистый денежный поток протокола" >}}

### Метод velocity

{{< formula math="P_token = PQ / (V × CirculatingSupply)" desc="P — цена единицы услуги · Q — количество транзакций · V — velocity · Уравнение обмена Фишера" >}}

### Метод P/E для токенов

{{< formula math="P_token = (AnnualRevenue × P/E_ratio) / CirculatingSupply" desc="P/E_ratio для крипто: 10–30 (зависит от роста). AnnualRevenue — годовой доход протокола" >}}

**Пример.** Протокол с $10M годового дохода, P/E = 20, circulating supply = 50M:

P_token = ($10M × 20) / 50M = **$4.00**

## Распространённые ошибки

### 1. Игнорирование токен-субсидий в расходах

«Airdrop ничего не стоит» — ложь. Каждый розданный токен — это давление на продажу, размывающее стоимость для держателей. Всегда включайте токен-стимулы в CAC по текущей рыночной цене.

### 2. LTV без учёта оттока

LTV = ARPU × 100 месяцев? Только если отток 1%. При реальном churn 8–15% средний срок жизни пользователя — 7–12 месяцев. Завышенный LTV маскирует убыточность.

### 3. ARPU на основе активных пользователей без фильтрации ботов

В криптопроектах 30–70% «пользователей» — боты, фермеры и дублированные адреса. ARPU на «реальных» пользователях может быть в 3–5 раз ниже заявленного.

### 4. Модель без сценариев цены токена

При росте цены токена в 5 раз расходы на стимулы растут пропорционально (если стимулы в токенах с фиксированным количеством). Моделируйте три сценария: текущая цена, рост в 3 раза, падение в 3 раза.

### 5. Выручка из токен-продаж как «доход»

Продажа токенов из казначейства — это не выручка, а привлечение капитала. Реальный доход — это комиссии от использования протокола.

{{< checklist title="Чеклист юнит-экономики токен-проекта" type="check" >}}
<li><strong>CAC рассчитан с учётом токен-субсидий</strong> — фиат + токены по рыночной цене</li>
<li><strong>LTV рассчитан с реальным churn</strong> — не оптимистичные 100 месяцев, а измеренные данные</li>
<li><strong>LTV/CAC > 3</strong> — или план по достижению этого уровня</li>
<li><strong>Velocity токена оценена</strong> — и есть механизмы её снижения</li>
<li><strong>Точка безубыточности определена</strong> — при каком MAU протокол выходит в плюс</li>
<li><strong>Три сценария цены токена смоделированы</strong> — база, рост, падение</li>
<li><strong>Доход от комиссий отделён от токен-продаж</strong> — это разные вещи</li>
<li><strong>Боты отфильтрованы из MAU</strong> — реальные пользователи, не кошельки</li>
{{< /checklist >}}

{{< cta title="Построим юнит-экономику вашего токен-проекта" text="Мы рассчитали юнит-экономику для 85+ проектов — от DeFi до GameFi. Проверим устойчивость вашей модели и найдём точку безубыточности." button="Рассчитать экономику" link="https://t.me/karanyuk" >}}

## Читайте также

- [5 моделей спроса на токен]({{< relref "demand-models" >}}) — как модели спроса влияют на ARPU и velocity
- [Дивиденды или выкуп: модели утилизации]({{< relref "utility-models" >}}) — механизмы снижения velocity через стейкинг и burn
- [Процесс создания токеномики]({{< relref "tokenomics-process" >}}) — как юнит-экономика вписывается в общий процесс проектирования
- [Симуляции и сценарный анализ]({{< relref "simulations" >}}) — моделирование юнит-экономики через симуляции
