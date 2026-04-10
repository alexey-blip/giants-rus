---
title: "Скорость обращения токена: почему платёжные токены теряют стоимость"
description: "Парадокс скорости обращения: чем активнее используется токен, тем ниже его цена. Уравнение обмена MV=PQ, механизмы удержания (стейкинг, сжигание, блокировка), калькулятор и Python-симуляция."
date: 2026-04-10
weight: 16
tags:
  - токеномика
  - скорость обращения
  - спрос
  - утилизация
  - стейкинг
categories:
  - models
---

Вот парадокс: проект выпускает токен для оплаты услуг. Пользователи растут, транзакции множатся — а цена токена стоит на месте или падает. Это не баг. Это **проблема скорости обращения** (velocity problem) — фундаментальная проблема всех платёжных токенов, которая объясняется одной формулой из XVIII века.

## Что такое скорость обращения

**Скорость обращения** (velocity) — сколько раз токен меняет владельца за период. Если 1 токен за год участвовал в 10 транзакциях, его скорость обращения = 10.

{{< formula math="V = Объём транзакций / Средняя капитализация" desc="V — скорость обращения · Объём транзакций — суммарный объём за период · Средняя капитализация — средняя рыночная капитализация за тот же период" >}}

Скорость обращения отражает **время удержания**: чем быстрее токен проходит цепочку «купил → использовал → продал», тем выше V. Bitcoin с V ~5 держат месяцами. Утилити-токен с V ~50 проскакивает кошелёк за часы.

### Почему это проблема

Высокая скорость обращения означает, что пользователю **невыгодно держать** токен. Он покупает токен в момент оплаты и тут же тратит. Продавец услуги получает токен и тут же продаёт за стейблкоин. Никто не держит — значит, нет давления на покупку, нет дефицита, нет роста цены.

{{< callout type="warning" title="Парадокс платёжных токенов" >}}
Чем эффективнее токен выполняет свою платёжную функцию, тем выше его скорость обращения и тем ниже обоснованная цена. Идеальное средство платежа — бесполезный актив для хранения стоимости.
{{< /callout >}}

## Уравнение обмена MV = PQ

В основе лежит уравнение Фишера, адаптированное для токеномики:

{{< formula math="M · V = P · Q" desc="M — капитализация токена · V — скорость обращения · P — средняя цена единицы услуги · Q — количество оплаченных услуг за период" >}}

Перепишем для оценки фундаментальной капитализации:

{{< formula math="M = PQ / V" desc="M — обоснованная рыночная капитализация · PQ — годовой объём транзакций в долларах · V — скорость обращения" >}}

### Что это значит на практике

Допустим, протокол обрабатывает $50M транзакций в год. При разной скорости обращения получаем разную обоснованную капитализацию:

| Объём транзакций (PQ) | Скорость (V) | Капитализация (M) | Цена при 100M токенов |
|---|---|---|---|
| $50M | 5 | $10M | $0.10 |
| $50M | 10 | $5M | $0.05 |
| $50M | 20 | $2.5M | $0.025 |
| $50M | 50 | $1M | $0.01 |

При одинаковом объёме транзакций рост V с 5 до 50 **снижает обоснованную капитализацию в 10 раз**. Это и есть проблема скорости обращения: токен может обслуживать огромный оборот, но стоить копейки.

### Исторический контекст

Уравнение обмена (MV = PQ) было сформулировано экономистом Ирвингом Фишером в 1911 году для анализа денежной массы. Кайл Самани из Multicoin Capital в 2017 году адаптировал его для криптовалют и показал, что большинство утилити-токенов обречены на низкую оценку из-за высокой скорости обращения.

## Механизмы удержания: как замедлить обращение

**Механизм удержания** (velocity sink) — инструмент, который заставляет или мотивирует держать токен дольше. Чем больше токенов заблокировано, тем меньше в свободном обращении, тем ниже эффективная скорость.

{{< formula math="V_эфф = Объём транзакций / (Капитализация - Заблокированные)" desc="Заблокированные — стоимость заблокированных токенов (стейкинг, governance, залоги) · При высоком объёме блокировки даже большой V не давит на цену" >}}

### Типы механизмов удержания

| Механизм | Как работает | Сила | Пример |
|---|---|---|---|
| **Стейкинг (PoS)** | Валидаторы блокируют токен для участия в консенсусе | Сильный — экономический риск (slashing) | ETH (32 ETH, ~23% предложения залочено) |
| **Сжигание** | Часть комиссий уничтожается навсегда | Постоянный — сокращает предложение | EIP-1559 (ETH базовая комиссия) |
| **Блокировка для голосования** | Токены блокируются для участия в управлении | Средний — зависит от активности DAO | veCRV (до 4 лет блокировки) |
| **Залог** | Токен используется как обеспечение кредита или участия | Сильный — разблокировка требует возврата | FIL (залог майнеров на 540 дней) |
| **Распределение дохода** | Держатели получают долю дохода протокола | Средний — чем выше доходность, тем ниже скорость | GMX (30% комиссий стейкерам) |
| **Длинные контракты** | Токены заблокированы на срок оказания услуги | Средний — привязан к бизнес-логике | Filecoin (контракт на хранение) |

{{< callout title="Принцип эффективного удержания" >}}
Механизм работает, только если **удержание выгоднее продажи**. Стейкинг без реального дохода (награды из эмиссии) — не удержание, а перекладывание инфляции. Настоящее удержание привязано к внешнему доходу протокола.
{{< /callout >}}

### Формула влияния удержания на цену

Комбинируя механизмы, протокол может существенно снизить эффективную скорость обращения:

{{< formula math="P = PQ / (V · S · (1 - L))" desc="P — фундаментальная цена · S — общее предложение · L — доля заблокированных токенов (0..1) · При L = 0.5 и V = 20 эффект тот же, что при V = 10 без блокировки" >}}

## Калькулятор скорости обращения

<style>
.vc-calc { background:#fff; border:1px solid rgba(0,0,0,0.08); border-radius:12px; padding:28px; margin:36px 0; }
.vc-title { font-family:'Space Grotesk',sans-serif; font-size:0.72rem; font-weight:600; text-transform:uppercase; letter-spacing:0.12em; color:#7c3aed; margin-bottom:20px; }
.vc-grid { display:grid; grid-template-columns:1fr 1fr; gap:16px; margin-bottom:20px; }
@media(max-width:700px){.vc-grid{grid-template-columns:1fr;}}
.vc-slider-label { display:flex; justify-content:space-between; font-family:'Space Grotesk',sans-serif; font-size:0.82rem; font-weight:500; color:#1a1a2e; margin-bottom:4px; }
.vc-slider-value { font-family:'JetBrains Mono',monospace; font-size:0.8rem; color:#7c3aed; background:rgba(124,58,237,0.05); padding:2px 8px; border-radius:4px; }
.vc-slider { width:100%; height:4px; -webkit-appearance:none; appearance:none; background:rgba(0,0,0,0.08); border-radius:2px; outline:none; accent-color:#7c3aed; }
.vc-slider::-webkit-slider-thumb { -webkit-appearance:none; width:16px; height:16px; border-radius:50%; background:#7c3aed; cursor:pointer; }
.vc-results { display:grid; grid-template-columns:repeat(5,1fr); gap:8px; margin-bottom:20px; }
@media(max-width:700px){.vc-results{grid-template-columns:repeat(3,1fr);}}
.vc-metric { background:#f8f7f4; border:1px solid rgba(0,0,0,0.06); border-radius:8px; padding:10px 8px; text-align:center; }
.vc-metric-label { font-family:'Space Grotesk',sans-serif; font-size:0.65rem; font-weight:600; text-transform:uppercase; letter-spacing:0.06em; color:#8a8a9a; margin-bottom:4px; }
.vc-metric-num { font-family:'JetBrains Mono',monospace; font-size:1rem; font-weight:700; color:#1a1a2e; }
.vc-metric-num.accent { color:#7c3aed; }
.vc-chart-wrap { background:#f8f7f4; border:1px solid rgba(0,0,0,0.08); border-radius:8px; overflow:hidden; }
.vc-legend { display:flex; gap:16px; margin-top:10px; font-family:'Space Grotesk',sans-serif; font-size:0.78rem; color:#4a4a5a; }
.vc-legend-dot { display:inline-block; width:12px; height:3px; border-radius:2px; margin-right:4px; vertical-align:middle; }
</style>

<div class="vc-calc" id="vc-calculator">
  <div class="vc-title">Калькулятор фундаментальной цены токена</div>
  <div class="vc-grid">
    <div>
      <div class="vc-slider-label">Годовой объём транзакций (PQ), $M <span class="vc-slider-value" id="vc-pq-val">50</span></div>
      <input type="range" id="vc-pq" class="vc-slider" min="1" max="500" step="1" value="50">
    </div>
    <div>
      <div class="vc-slider-label">Скорость обращения (V) <span class="vc-slider-value" id="vc-v-val">20</span></div>
      <input type="range" id="vc-v" class="vc-slider" min="1" max="100" step="1" value="20">
    </div>
    <div>
      <div class="vc-slider-label">Общее предложение, M токенов <span class="vc-slider-value" id="vc-s-val">100</span></div>
      <input type="range" id="vc-s" class="vc-slider" min="1" max="1000" step="1" value="100">
    </div>
    <div>
      <div class="vc-slider-label">Доля заблокированных, % <span class="vc-slider-value" id="vc-l-val">30</span></div>
      <input type="range" id="vc-l" class="vc-slider" min="0" max="90" step="1" value="30">
    </div>
  </div>
  <div class="vc-results">
    <div class="vc-metric"><div class="vc-metric-label">Кап-ция без удержания</div><div class="vc-metric-num" id="vc-mcap0">$2.5M</div></div>
    <div class="vc-metric"><div class="vc-metric-label">Кап-ция с удержанием</div><div class="vc-metric-num" id="vc-mcap1">$3.6M</div></div>
    <div class="vc-metric"><div class="vc-metric-label">Цена без удержания</div><div class="vc-metric-num" id="vc-p0">$0.025</div></div>
    <div class="vc-metric"><div class="vc-metric-label">Цена с удержанием</div><div class="vc-metric-num" id="vc-p1">$0.036</div></div>
    <div class="vc-metric"><div class="vc-metric-label">Рост цены</div><div class="vc-metric-num accent" id="vc-boost">+43%</div></div>
  </div>
  <div class="vc-chart-wrap">
    <svg id="vc-svg" viewBox="0 0 560 260" preserveAspectRatio="xMidYMid meet" style="display:block;width:100%;height:260px;"></svg>
  </div>
  <div class="vc-legend">
    <span><span class="vc-legend-dot" style="background:#8a8a9a;"></span>Без удержания</span>
    <span><span class="vc-legend-dot" style="background:#7c3aed;height:3px;"></span>С удержанием</span>
  </div>
</div>

<script>
(function(){
  var $=function(id){return document.getElementById(id)};
  var sl={pq:$('vc-pq'),v:$('vc-v'),s:$('vc-s'),l:$('vc-l')};
  function fmt(n){if(n>=1e9)return'$'+(n/1e9).toFixed(1)+'B';if(n>=1e6)return'$'+(n/1e6).toFixed(1)+'M';if(n>=1e3)return'$'+(n/1e3).toFixed(1)+'K';return'$'+n.toFixed(2)}
  function fP(n){if(n>=1)return'$'+n.toFixed(2);if(n>=0.01)return'$'+n.toFixed(4);return'$'+n.toFixed(6)}
  function render(){
    var pq=+sl.pq.value*1e6,v=+sl.v.value,s=+sl.s.value*1e6,lk=+sl.l.value/100;
    $('vc-pq-val').textContent=sl.pq.value;$('vc-v-val').textContent=sl.v.value;
    $('vc-s-val').textContent=sl.s.value;$('vc-l-val').textContent=sl.l.value;
    var m0=pq/v,m1=pq/(v*(1-lk)),p0=m0/s,p1=m1/s,boost=((p1/p0)-1)*100;
    $('vc-mcap0').textContent=fmt(m0);$('vc-mcap1').textContent=fmt(m1);
    $('vc-p0').textContent=fP(p0);$('vc-p1').textContent=fP(p1);
    $('vc-boost').textContent='+'+boost.toFixed(0)+'%';
    // SVG chart
    var W=560,H=260,pd={t:16,r:16,b:32,l:56},pw=W-pd.l-pd.r,ph=H-pd.t-pd.b;
    var pts0=[],pts1=[],maxP=0;
    for(var vi=1;vi<=60;vi++){var pr0=(pq/vi)/s,pr1=(pq/(vi*(1-lk)))/s;pts0.push(pr0);pts1.push(pr1);if(pr1>maxP)maxP=pr1;if(pr0>maxP)maxP=pr0}
    maxP*=1.1;
    var svg='';
    // grid
    for(var gi=0;gi<=4;gi++){var gy=pd.t+(gi/4)*ph;svg+='<line x1="'+pd.l+'" y1="'+gy+'" x2="'+(W-pd.r)+'" y2="'+gy+'" stroke="#8a8a9a" stroke-opacity="0.12" stroke-dasharray="4,4"/>';svg+='<text x="'+(pd.l-6)+'" y="'+(gy+4)+'" text-anchor="end" fill="#8a8a9a" font-size="9" font-family="JetBrains Mono,monospace">'+fP(maxP*(1-gi/4))+'</text>'}
    for(var xi=0;xi<=6;xi++){var xv=xi*10;if(xv===0)xv=1;var gx=pd.l+((xv-1)/59)*pw;svg+='<text x="'+gx+'" y="'+(H-6)+'" text-anchor="middle" fill="#8a8a9a" font-size="9" font-family="JetBrains Mono,monospace">'+xv+'</text>'}
    svg+='<text x="'+(pd.l+pw/2)+'" y="'+(H)+'" text-anchor="middle" fill="#8a8a9a" font-size="9" font-family="Space Grotesk,sans-serif">Скорость обращения (V)</text>';
    // raw line (dashed)
    var d0='M';for(var i=0;i<60;i++){var x=pd.l+(i/59)*pw,y=pd.t+ph-(pts0[i]/maxP)*ph;d0+=(i?'L':'')+(x.toFixed(1)+','+y.toFixed(1))}
    svg+='<path d="'+d0+'" fill="none" stroke="#8a8a9a" stroke-width="2" stroke-dasharray="6,4"/>';
    // sink line
    var d1='M';for(var j=0;j<60;j++){var x2=pd.l+(j/59)*pw,y2=pd.t+ph-(pts1[j]/maxP)*ph;d1+=(j?'L':'')+(x2.toFixed(1)+','+y2.toFixed(1))}
    svg+='<path d="'+d1+'" fill="none" stroke="#7c3aed" stroke-width="2.5"/>';
    // area between
    var area='M';for(var k=0;k<60;k++){var ax=pd.l+(k/59)*pw,ay=pd.t+ph-(pts1[k]/maxP)*ph;area+=(k?'L':'')+(ax.toFixed(1)+','+ay.toFixed(1))}
    for(var k2=59;k2>=0;k2--){var ax2=pd.l+(k2/59)*pw,ay2=pd.t+ph-(pts0[k2]/maxP)*ph;area+='L'+(ax2.toFixed(1)+','+ay2.toFixed(1))}
    area+='Z';
    svg+='<path d="'+area+'" fill="#7c3aed" opacity="0.08"/>';
    $('vc-svg').innerHTML=svg;
  }
  Object.values(sl).forEach(function(s){s.addEventListener('input',render)});
  render();
})();
</script>

Двигайте ползунки, чтобы увидеть, как скорость обращения и доля заблокированных токенов влияют на фундаментальную цену. Заливка между кривыми показывает выигрыш от механизмов удержания.

## Симуляция: как удержание меняет цену со временем

Рассмотрим модельный протокол с растущим числом транзакций. Смоделируем 24 месяца работы и покажем, как разные комбинации механизмов влияют на фундаментальную цену.

**Параметры симуляции:**
- Начальный месячный объём: $2M, рост +5% в месяц
- Общее предложение: 100M токенов
- Скорость обращения: 20
- Три сценария: без удержания, стейкинг (30% заблокировано), стейкинг + сжигание (30% + 0.5% предложения сжигается в месяц)

| Месяц | PQ (годовой) | Без удержания | Стейкинг 30% | Стейкинг + сжигание |
|---|---|---|---|---|
| 1 | $24.0M | $0.012 | $0.017 | $0.017 |
| 6 | $30.6M | $0.015 | $0.022 | $0.023 |
| 12 | $39.1M | $0.020 | $0.028 | $0.031 |
| 18 | $49.8M | $0.025 | $0.036 | $0.042 |
| 24 | $63.6M | $0.032 | $0.045 | $0.057 |
| **Рост за 24 мес** | **+165%** | **+165%** | **+165%** | **+238%** |

Стейкинг одинаково масштабирует цену на +43% (множитель 1/(1-0.3)). Но сжигание **накапливается**: к 24-му месяцу предложение сократилось на ~11%, что даёт дополнительный рост.

<details>
<summary>Python-код симуляции</summary>

```python
import pandas as pd

months = 24
initial_monthly_pq = 2_000_000  # $2M/мес
growth_rate = 0.05               # +5% в месяц
total_supply = 100_000_000       # 100M
velocity = 20
staking_pct = 0.30               # 30% заблокировано
burn_rate = 0.005                 # 0.5% предложения сжигается в месяц

rows = []
supply_current = total_supply

for m in range(1, months + 1):
    monthly_pq = initial_monthly_pq * (1 + growth_rate) ** (m - 1)
    annual_pq = monthly_pq * 12

    # Без удержания
    mcap_raw = annual_pq / velocity
    price_raw = mcap_raw / total_supply

    # Стейкинг
    mcap_stake = annual_pq / (velocity * (1 - staking_pct))
    price_stake = mcap_stake / total_supply

    # Стейкинг + сжигание
    supply_current -= supply_current * burn_rate
    mcap_burn = annual_pq / (velocity * (1 - staking_pct))
    price_burn = mcap_burn / supply_current

    rows.append({
        'month': m,
        'annual_pq': annual_pq,
        'price_raw': price_raw,
        'price_stake': price_stake,
        'price_burn': price_burn,
        'supply_after_burn': supply_current
    })

df = pd.DataFrame(rows)
print(df[['month', 'annual_pq', 'price_raw', 'price_stake', 'price_burn']].to_string(index=False))
```

</details>

## Типичные ошибки

{{< checklist title="Чего избегать при проектировании механизмов удержания" type="check" >}}
<li><strong>Стейкинг без реального дохода.</strong> Если награды приходят из эмиссии, а не из внешнего дохода протокола — это не удержание, а перераспределение инфляции. Награда за удержание из воздуха не создаёт спрос.</li>
<li><strong>Сжигание без потока комиссий.</strong> Если сжигать токены из казны, а не из комиссий — это временная мера, которая закончится вместе с казной. Устойчивое сжигание привязано к объёму транзакций.</li>
<li><strong>Путать скорость обращения и ликвидность.</strong> Низкая скорость обращения не означает низкую ликвидность. ETH имеет умеренную скорость (~5), но огромную ликвидность. Токен с V = 100 и нулевой глубиной стакана — плохая комбинация.</li>
<li><strong>Игнорировать спекулятивный спрос.</strong> Модель MV=PQ описывает фундаментальную оценку. В реальности цена может быть в 10-100 раз выше из-за спекуляции — но и в 10 раз ниже, когда хайп проходит.</li>
<li><strong>Создавать искусственные блокировки.</strong> Принудительная блокировка без выгоды раздражает пользователей и увеличивает давление продажи после разблокировки. Удержание должно быть экономически рациональным выбором, а не тюрьмой.</li>
{{< /checklist >}}

## Кейс: уроки TON

Блокчейн TON — наглядная иллюстрация проблемы скорости обращения в масштабе. Данные из разбора динамики сети за 2024-2025 годы показывают классический дисбаланс.

### Цифры

| Метрика | 2024 | 2025 | Изменение |
|---|---|---|---|
| Аккаунтов в сети | 108M | 165M | +53% |
| Транзакций в день | 5.6M | 1.7M | -70% |
| Комиссии (TON/день) | 16 133 | 5 179 | -68% |
| Минтинг (TON/день) | 69 979 | 88 254 | +26% |

Аккаунтов стало больше, но транзакции и комиссии упали. При этом эмиссия выросла на 26%. Токены генерируются значительно быстрее, чем создаётся спрос на их использование.

### Анализ через MV = PQ

Для TON в 2025 году: ежедневные комиссии ~5 179 TON, что при цене ~$3 даёт годовой PQ около $5.7M. При скорости обращения ~15 и предложении 5.1B:

{{< formula math="P_фунд = $5.7M / (15 · 5.1B) ≈ $0.000075" desc="Фундаментальная оценка TON по модели MV=PQ в тысячи раз ниже рыночной цены · Разница покрывается спекулятивным спросом и ожиданиями роста экосистемы" >}}

Это не значит, что TON «переоценён» в традиционном смысле — рыночная цена включает ожидания будущего роста транзакционного объёма и развития экосистемы. Но модель показывает, что **текущий фундаментальный спрос** недостаточен для поддержания цены без спекулятивной составляющей.

{{< callout type="info" title="Что мог бы сделать TON" >}}
Увеличить фундаментальную оценку можно через: (1) рост DeFi-активности и комиссий, (2) введение сжигания по типу EIP-1559, (3) увеличение доли стейкинга с реальным slashing, (4) блокировку для участия в управлении экосистемой.
{{< /callout >}}

## Продвинутый анализ: комбинирование механизмов

Наиболее устойчивые проекты комбинируют несколько механизмов удержания. Вот как это работает у лидеров рынка:

| Проект | Стейкинг | Сжигание | Управление | Распред. дохода | Эфф. V |
|---|---|---|---|---|---|
| **Ethereum** | 23% в PoS | EIP-1559 | Нет | Нет | ~5 |
| **Curve** | Нет | Нет | veCRV до 4 лет | 50% комиссий | ~3 |
| **GMX** | Есть | Нет | Нет | 30% комиссий в ETH | ~4 |
| **Filecoin** | Залог 540 дней | Нет | Нет | Нет | ~8 |
| **BNB** | Нет | Квартальное | Нет | Скидка на комиссии | ~12 |

Ethereum комбинирует стейкинг (сильная блокировка) и сжигание (постоянное сокращение предложения) — в результате эффективная скорость одна из самых низких на рынке. Curve достигает ещё более низкой скорости через экстремально длинные блокировки (до 4 лет) в обмен на распределение дохода.

### Формула комбинированного эффекта

{{< formula math="P = PQ / (V · S₀ · (1 - L) · (1 - B)^t)" desc="S₀ — начальное предложение · L — доля заблокированных · B — месячная ставка сжигания · t — число месяцев · Механизмы мультипликативно усиливают друг друга" >}}

---

## Читайте также

- [5 моделей спроса на токен]({{< relref "demand-models" >}}) — обзор всех моделей утилизации
- [Стейкинг в токеномике]({{< relref "staking" >}}) — детальный разбор стейкинга как механизма удержания
- [Дивиденды или выкуп]({{< relref "utility-models" >}}) — сравнение стратегий распределения дохода
- [Дизайн механизмов]({{< relref "mechanism-design" >}}) — как интегрировать предложение и спрос
- [5 моделей предложения токенов]({{< relref "token-supply-models" >}}) — вторая сторона уравнения: откуда берутся токены
- [AMM и ликвидность]({{< relref "amm" >}}) — рыночная модель, которая определяет реальную цену

{{< cta title="Проблема скорости обращения в вашем проекте?" text="Если единственная функция вашего токена — оплата услуг, проблема скорости обращения неизбежна. Спроектируем комбинацию механизмов удержания, которая привяжет цену токена к реальному доходу протокола." button="Обсудить проект →" >}}
