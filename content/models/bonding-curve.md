---
title: "Bonding Curve: модель предложения в токеномике"
description: "Что такое bonding curve, формулы минтинга, типы кривых предложения, примеры реализации в DeFi и программах лояльности. Подбор параметров A, B, C."
date: 2026-04-09
weight: 30
tags:
  - токеномика
  - bonding curve
  - ценообразование
categories:
  - models
---

## Что такое bonding curve

**Bonding curve** (кривая предложения) — это математическая функция, которая динамически определяет цену токена на основе его совокупного предложения. Чем больше токенов наминчено, тем выше цена следующего.

Ключевой принцип прост: **чем раньше — тем дешевле**. Ранние участники получают токены по более низкой цене, что создаёт естественный стимул для раннего участия в проекте.

В отличие от традиционных моделей (фиксированная цена, аукцион, листинг на бирже), bonding curve обеспечивает прямую связь между спросом и предложением без посредников: смарт-контракт автоматически минтит токены при покупке и может сжигать при продаже.

{{< callout title="Почему это важно" >}}
Bonding curve считается **самой честной и понятной моделью минтинга** со стороны покупателя: цена определяется алгоритмом, а не решениями команды. Каждый участник видит, по какой цене минтят токены до и после него.
{{< /callout >}}

## Место bonding curve среди моделей предложения

Модели предложения токенов отвечают на вопрос: **как и за что стейкхолдеры получают токены**. Основные четыре модели:

| Модель | Логика | Пример |
|--------|--------|--------|
| **Bonding Curve** | Минтинг по формуле P = f(supply) — цена растёт с предложением | pump.fun, Loyalty |
| **Airdrop** | Раздача за целевые действия | Uniswap, Optimism |
| **Reward** | Вознаграждение за действия (P2E, PoW, PoS) | Bitcoin, Notcoin |
| **DEX / CEX** | Покупка через пул ликвидности или ордербук | Uniswap V2, Bybit |

{{< callout type="info" title="Вестинг — не модель предложения" >}}
Вестинг — это механизм контроля скорости разблокировки уже выделенных токенов. Он комбинируется с любой моделью (airdrop + вестинг, reward + вестинг), но не является самостоятельной моделью предложения. Подробнее — в статье [5 моделей предложения токенов]({{< relref "token-supply-models" >}}).
{{< /callout >}}

Ключевое отличие bonding curve от других моделей: эмиссия динамически адаптируется к реальному спросу. Пришло 1 000 или 100 000 пользователей — аллокация разная, цена каждого токена отражает текущее число участников.

## Математика: формула минтинга

Базовая формула bonding curve определяет **цену минтинга очередного токена** как функцию от совокупного предложения:

{{< formula math="P(T) = A + B · T^C" desc="P — цена минтинга · T — общее число наминченных токенов · A, B, C — коэффициенты" >}}

### Что означают параметры

- **A** — начальная (минимальная) цена токена. Даже при нулевом предложении цена не равна нулю.
- **B** — коэффициент масштаба. Определяет, насколько быстро растёт цена с увеличением supply.
- **C** — показатель степени (крутизна кривой). При C=1 рост линейный, при C=2 квадратичный, при C=0.5 — корневой.

{{< callout type="warning" title="Пример" >}}
При формуле `P = 0.10 + 0.00001 · T^1.5` цена первого токена составит $0.10. После минтинга 10 000 токенов цена вырастет до $10.10. Подбор параметров — ключевая задача токеномиста.
{{< /callout >}}

### Средневзвешенная цена

При использовании bonding curve в платёжных системах (loyalty, payments) часто применяется **средневзвешенная историческая цена**:

{{< formula math="Pp(T) = A + B · T^C / (C + 1)" desc="Нечувствительна к мгновенным скачкам спроса · Упрощает расчёты на фронтенде" >}}

## Типы кривых

| Тип | Формула | Свойство | Применение |
|-----|---------|----------|------------|
| **Линейная** | P = A + B·T | Равномерный рост | Простые модели, loyalty |
| **Полиномиальная** | P = A + B·T^C | Ускоряющийся рост | Стандарт для token sale |
| **Экспоненциальная** | P = A·e^(B·T) | Агрессивный рост | Scarce assets, NFT |
| **Логарифмическая** | P = A + B·ln(T) | Быстрый старт, стабилизация | Массовые продукты |
| **Step function** | Pi при T∈[Ti, Ti+1) | Ступенчатый рост | pump.fun (Solana) |

Платформа **pump.fun** на Solana использует step function — цена растёт ступенями. Когда кривая достигает порога, ликвидность автоматически переносится на DEX (Raydium), и дальше цену определяет рынок.

## Применения

### Token Launch / Fundraising

Bonding curve используется для честного распределения токенов на ранней стадии проекта. Инвесторы минтят токены по формуле — ранние получают лучшую цену без привилегированных раундов.

Пример: предпродажа 5% токенов инвесторам по bonding curve со стартовой ценой $0.10 и целевой ценой $0.25 через 6 месяцев.

### DeFi и AMM

AMM вроде Uniswap используют вариацию bonding curve для ценообразования: формула `x · y = k` задаёт кривую, по которой цена меняется при каждой сделке.

### Программы лояльности

Один из самых перспективных кейсов — **Bonding Curve Loyalty**: кэшбек в токенах для e-commerce.

1. Покупатель платит в USDC (Shopify, Stripe, Coinbase Commerce)
2. Часть суммы идёт в bonding curve контракт
3. Смарт-контракт минтит токены по текущей цене кривой
4. Токены можно потратить на скидки или продать обратно (burn)

{{< formula math="ΔT = Sc · kt / Pm(Ttotal)" desc="Sc — сумма кэшбека · kt — процент в токенах · Pm — цена минтинга" >}}

## Преимущества и ограничения

### Преимущества

{{< checklist type="check" >}}
<li><strong>Прозрачность</strong> — цена определяется алгоритмом, любой может проверить</li>
<li><strong>Непрерывная ликвидность</strong> — токен можно купить в любой момент</li>
<li><strong>Стимул ранних участников</strong> — вознаграждение за раннее участие без привилегий</li>
<li><strong>Связь цены со спросом</strong> — эмиссия адаптируется к числу участников</li>
<li><strong>Автономность</strong> — работает без маркет-мейкера и посредника</li>
{{< /checklist >}}

### Ограничения

- **Сложность реализации** — требует грамотного смарт-контракта
- **Нет price discovery** — цена алгоритмическая, а не рыночная
- **Ликвидность при burn** — казначейство должно быть обеспечено
- **Расхождение с DEX** — при листинге цена может отличаться от кривой

{{< callout title="Ключевой вывод" >}}
Bonding curve — модель **предпродажи и минтинга**, а не финальный рынок. Большинство проектов используют кривую на раннем этапе, затем переходят к пулу ликвидности на DEX.
{{< /callout >}}

## Практика: подбор параметров

Задача токеномиста — подобрать A, B и C под бизнес-ограничения:

```
Условия:
- Начальная цена: $0.10
- Через 3 мес: $0.20 – $0.40
- Через 12 мес: $0.60 – $1.00

Входные данные:
- Пользователи: 1,000 → рост 500-2,000/мес
- Средний чек: $100-$130
- Кэшбек: 5-10%
```

Подход к решению:

1. Прогнозируем кумулятивный объём средств через bonding curve по месяцам
2. Подставляем в формулу и решаем систему неравенств
3. Проверяем параметры интерактивно ниже, визуализируем в Desmos
4. Тестируем чувствительность: что если пользователей в 2x больше/меньше?

## Калькулятор bonding curve

<div class="gnts-calc" id="gnts-calc-bc" style="background:rgba(124,58,237,0.04);border:1px solid rgba(124,58,237,0.15);border-radius:12px;padding:28px;margin:32px 0;">
  <div style="font-family:'Space Grotesk',sans-serif;font-size:0.72rem;font-weight:600;text-transform:uppercase;letter-spacing:0.12em;color:#7c3aed;margin-bottom:20px;">P(T) = A + B · T^C</div>
  <div style="display:grid;grid-template-columns:1fr 1fr;gap:20px;margin-bottom:24px;">
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">Начальная цена (A) <span id="bc-val-a" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">0.10</span></label>
      <input type="range" id="bc-a" min="0.01" max="1" step="0.01" value="0.10" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">Масштаб (B) <span id="bc-val-b" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">0.00001</span></label>
      <input type="range" id="bc-b" min="0.000001" max="0.001" step="0.000001" value="0.00001" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">Крутизна (C) <span id="bc-val-c" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">1.50</span></label>
      <input type="range" id="bc-c" min="0.5" max="3" step="0.1" value="1.5" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
    <div>
      <label style="display:flex;justify-content:space-between;font-family:'Space Grotesk',sans-serif;font-size:0.82rem;font-weight:500;color:#1a1a2e;margin-bottom:6px;">Макс. supply <span id="bc-val-s" style="font-family:'JetBrains Mono',monospace;font-size:0.8rem;color:#7c3aed;background:rgba(124,58,237,0.08);padding:2px 8px;border-radius:4px;">50 000</span></label>
      <input type="range" id="bc-s" min="1000" max="500000" step="1000" value="50000" style="width:100%;height:4px;-webkit-appearance:none;appearance:none;background:#e5e7eb;border-radius:2px;outline:none;accent-color:#7c3aed;">
    </div>
  </div>
  <div style="background:#f8f7f4;border:1px solid rgba(0,0,0,0.08);border-radius:8px;overflow:hidden;margin-bottom:16px;">
    <svg id="bc-svg" viewBox="0 0 500 280" preserveAspectRatio="xMidYMid meet" style="display:block;width:100%;height:280px;"></svg>
  </div>
  <table style="width:100%;border-collapse:collapse;font-size:0.8rem;">
    <thead><tr>
      <th style="text-align:left;font-family:'Space Grotesk',sans-serif;font-weight:600;font-size:0.7rem;text-transform:uppercase;letter-spacing:0.06em;color:#8a8a9a;padding:6px 10px;border-bottom:1px solid rgba(0,0,0,0.08);">Supply</th>
      <th style="text-align:left;font-family:'Space Grotesk',sans-serif;font-weight:600;font-size:0.7rem;text-transform:uppercase;letter-spacing:0.06em;color:#8a8a9a;padding:6px 10px;border-bottom:1px solid rgba(0,0,0,0.08);">Цена токена</th>
      <th style="text-align:left;font-family:'Space Grotesk',sans-serif;font-weight:600;font-size:0.7rem;text-transform:uppercase;letter-spacing:0.06em;color:#8a8a9a;padding:6px 10px;border-bottom:1px solid rgba(0,0,0,0.08);">Совокупная стоимость</th>
    </tr></thead>
    <tbody id="bc-tbody"></tbody>
  </table>
</div>
<script>
(function(){
  var sl={a:document.getElementById('bc-a'),b:document.getElementById('bc-b'),c:document.getElementById('bc-c'),s:document.getElementById('bc-s')};
  var vl={a:document.getElementById('bc-val-a'),b:document.getElementById('bc-val-b'),c:document.getElementById('bc-val-c'),s:document.getElementById('bc-val-s')};
  var svg=document.getElementById('bc-svg'),tbody=document.getElementById('bc-tbody');
  function P(t,a,b,c){return a+b*Math.pow(t,c)}
  function tc(t,a,b,c){var n=200,h=t/n,s=0;for(var i=0;i<n;i++){var t0=i*h,t1=(i+1)*h;s+=(P(t0,a,b,c)+P(t1,a,b,c))*h/2}return s}
  function fmt(v){if(v>=1e6)return(v/1e6).toFixed(2)+'M';if(v>=1e3)return(v/1e3).toFixed(1)+'K';if(v>=1)return v.toFixed(2);if(v>=0.001)return v.toFixed(4);return v.toExponential(2)}
  function render(){
    var a=+sl.a.value,b=+sl.b.value,c=+sl.c.value,maxS=+sl.s.value;
    vl.a.textContent=a.toFixed(2);
    vl.b.textContent=b<0.0001?b.toExponential(1):b.toFixed(6);
    vl.c.textContent=c.toFixed(2);
    vl.s.textContent=maxS.toLocaleString('ru-RU');
    var W=500,H=280,pd={t:20,r:20,b:40,l:60};
    var pw=W-pd.l-pd.r,ph=H-pd.t-pd.b;
    var pts=[],maxP=0;
    for(var i=0;i<=100;i++){var t=(i/100)*maxS;var p=P(t,a,b,c);pts.push({t:t,p:p});if(p>maxP)maxP=p}
    maxP=maxP*1.1||1;
    var polyPts=pts.map(function(pt){return(pd.l+(pt.t/maxS)*pw)+','+(pd.t+ph-(pt.p/maxP)*ph)}).join(' ');
    var h='';
    for(var g=0;g<=4;g++){
      var gy=pd.t+(g/4)*ph;
      h+='<line x1="'+pd.l+'" y1="'+gy+'" x2="'+(W-pd.r)+'" y2="'+gy+'" stroke="#8a8a9a" stroke-opacity="0.15" stroke-dasharray="4,4"/>';
      h+='<text x="'+(pd.l-8)+'" y="'+(gy+4)+'" text-anchor="end" fill="#8a8a9a" font-size="10" font-family="JetBrains Mono,monospace">$'+fmt(maxP*(1-g/4))+'</text>';
    }
    for(var g=0;g<=4;g++){
      var gx=pd.l+(g/4)*pw;
      h+='<text x="'+gx+'" y="'+(H-8)+'" text-anchor="middle" fill="#8a8a9a" font-size="10" font-family="JetBrains Mono,monospace">'+fmt(maxS*(g/4))+'</text>';
    }
    h+='<polygon points="'+pd.l+','+(pd.t+ph)+' '+polyPts+' '+(pd.l+pw)+','+(pd.t+ph)+'" fill="#7c3aed" opacity="0.08"/>';
    h+='<polyline points="'+polyPts+'" fill="none" stroke="#7c3aed" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/>';
    svg.innerHTML=h;
    var rows=[0,0.1,0.25,0.5,0.75,1].map(function(f){
      var t=Math.round(f*maxS);
      return '<tr><td style="padding:5px 10px;font-family:JetBrains Mono,monospace;font-size:0.78rem;color:#4a4a5a;border-bottom:1px solid rgba(0,0,0,0.06)">'+t.toLocaleString('ru-RU')+'</td><td style="padding:5px 10px;font-family:JetBrains Mono,monospace;font-size:0.78rem;color:#4a4a5a;border-bottom:1px solid rgba(0,0,0,0.06)">$'+fmt(P(t,a,b,c))+'</td><td style="padding:5px 10px;font-family:JetBrains Mono,monospace;font-size:0.78rem;color:#4a4a5a;border-bottom:1px solid rgba(0,0,0,0.06)">$'+fmt(tc(t,a,b,c))+'</td></tr>';
    });
    tbody.innerHTML=rows.join('');
  }
  Object.values(sl).forEach(function(s){s.addEventListener('input',render)});
  render();
})();
</script>

## Примеры реализации

### Кредитный DeFi-протокол: bonding curve с α-параметром

DeFi-протокол кредитования использует другую форму bonding curve с α-параметром:

{{< formula math="Pmint($L) = P(0) × ($Lminted)^α" desc="P(0) — базовая цена (1 USDT) · $Lminted — совокупное число наминченных токенов · α — параметр крутизны" >}}

Расчёт стоимости минтинга N новых токенов (интегральная формула):

{{< formula math="Cost(Δ$L, $L₀) = P(0)/(α+1) × [($L₀+Δ$L)^(α+1) − ($L₀)^(α+1)]" desc="Показывает экспоненциальный рост стоимости: первый токен = 1 USDT, тысячный = 25 USDT" >}}

{{< case title="Кредитный DeFi-протокол: Proof-of-Loan с дефляционной моделью" tags="DeFi Lending, Bonding Curve, Proof-of-Loan" >}}
10 строк таблиц с прогрессией цены, 4 формулы (минтинг, стоимость, обратный расчёт, кумулятивная), анализ конкурентов (Goldfinch, Clearpool, Atlendis).
{{< /case >}}

### pump.fun (Solana)

Step function bonding curve. Токен минтится по ступенчатой цене. При достижении порога ликвидность автоматически переносится на Raydium (DEX). Стандарт для запуска мемкоинов 2024-2025.

### Bonding Curve Loyalty (кэшбек в токенах)

Кэшбек в токенах для e-commerce. Интеграция с Shopify, Stripe, Coinbase Commerce. Параметры A, B, C настраиваются мерчантом через админку. Buy-back с дисконтом обеспечивает обратную ликвидность.

### Bancor / Continuous Token Model

Одна из первых реализаций bonding curve в блокчейне (2017). Токены минтятся и сжигаются автоматически через reserve pool с заданным коэффициентом резервирования.

{{< cta title="Нужна bonding curve для вашего проекта?" text="Подберём параметры кривой, построим модель в Google Sheets с симуляциями и подготовим ТЗ для разработчиков." link="https://t.me/karanyuk" button="Обсудить проект →" >}}

---

## Связанные материалы

- [5 моделей предложения токенов]({{< relref "token-supply-models" >}}) — обзор всех моделей предложения, включая вестинг, airdrop и reward
<!-- TODO: добавить ссылку на allocation когда страница появится -->
