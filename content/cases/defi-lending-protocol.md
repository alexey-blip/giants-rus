---
title: "Кейс: как кредитный протокол утилизировал избыток токена через субординацию"
description: "Разбор токеномики краудлендинговой платформы: bonding curve, трёхуровневая субординация и три механизма сжигания для утилизации кэшбек-токенов."
date: 2026-04-10
draft: false
weight: 10
tags:
  - кейс
  - defi
  - lending
  - bonding-curve
  - утилизация
---

## Задача: куда девать токены, розданные кредиторам

Краудлендинговая платформа для малого и среднего бизнеса. Модель: платформа соединяет кредиторов (физлица, средний чек 50-20 000 USDC) и заёмщиков (компании, займы 500K-1M USDC траншами по 100-200K). Покрытие залогом — 30-50%.

Чтобы привлечь кредиторов, платформа выдаёт кэшбек в токенах за каждый депозит. 65% всего supply (6.5 млн из 10 млн токенов) выделено на эти вознаграждения. Проблема очевидна: если кредиторы просто продают полученные токены, возникает постоянное давление на цену. Кэшбек обесценивается → стимул пропадает → кредиторы уходят → платформа теряет ликвидность.

Задача для токеномиста: **создать механизм утилизации токенов**, который заставляет кредиторов тратить полученный кэшбек внутри платформы, а не продавать на рынке.

## Кейс: bonding curve + субординация + сжигание

Решение строится на трёх компонентах, каждый из которых снижает давление на продажу.

### Компонент 1: Bonding curve вместо фиксированной эмиссии

Токены не распределяются по графику — они минтятся через bonding curve только при реальной активности кредиторов. Цена минта растёт по степенной функции:

{{< formula math="P_mint = P₀ · ((β + minted) / β)^α" desc="P₀ = 1.00 USDC, α = 0.45, β = 10 000. Степенная функция с затухающим ростом — ранние кредиторы получают токены дешевле" >}}

Средняя цена минта P_average используется как внутренний оракул для конвертации платежей:

{{< formula math="P̄ = P₀ · β / ((α+1) · L) · [((β + L) / β)^(α+1) − 1]" desc="L = количество выпущенных токенов. P̄ всегда ниже текущей P_mint, что делает оплату токенами выгоднее" >}}

### Компонент 2: Трёхуровневая субординация — главный поглотитель токенов

Каждый кредитор получает CT-токены (confirmation tokens), подтверждающие его позицию в кредитном пуле. По умолчанию все позиции имеют приоритет **Middle**. Кредитор может:

- **Повысить до High** — заплатив токены. При дефолте заёмщика High-позиции погашаются в последнюю очередь (максимальная защита)
- **Понизить до Low** — бесплатно, получив выплату из фонда повышений. Low-позиции несут убытки первыми

Это и есть ключевой механизм утилизации: кредиторы тратят полученный кэшбек на защиту своих же депозитов.

Цена повышения растёт линейно по мере заполнения High-корзины (максимум 1/3 от всех позиций):

{{< formula math="P(u) = C_nom · (a + b · u)" desc="a = 0.01, b = 0.15, u = степень заполнения High-корзины (0…1), C_nom = номинал CT-токена (например, 10 USDC)" >}}

| Заполнение High | Цена повышения (% от номинала) | При номинале 10 USDC |
|----------------|-------------------------------|---------------------|
| 1% | 1.14% | 1.14 USDC |
| 12% | 2.74% | 2.74 USDC |
| 28% | 5.14% | 5.14 USDC |
| 55% | 9.24% | 9.24 USDC |
| 66% | 10.89% | 10.89 USDC |

Выплаты Low-кредиторам взвешены по времени до погашения — чем дольше до конца, тем больше выплата:

{{< formula math="w_i = (1 + k · r)^(d_i / 365)" desc="k = 3.0 (усилитель), r = APR = 25%, d_i = дней до погашения" >}}

При дефолте убытки поглощаются в порядке: Low → Middle → High.

### Компонент 3: Механизмы сжигания и дополнительного спроса

**Субординационное сжигание.** Пример: кредиторы потратили 100 токенов на повышение приоритета до High. Платформа берёт 20% комиссии (20 токенов — сжигаются). Оставшиеся 80 токенов передаются тем, кто согласился понизить приоритет до Low. Если желающих понизиться меньше, чем повыситься — невостребованный остаток тоже сжигается.

**Квартальный buyback & burn** — 10% чистой прибыли платформы направляется на выкуп токенов с рынка и сжигание.

**Оплата KYB/Due Diligence** — заёмщики оплачивают проверку (~22 000 USDC эквивалент) со скидкой при оплате токенами. При 5-10 новых проектах в месяц это 110-220K USDC дополнительного спроса.

**Штрафы за просрочку** — пеня 2-4x от базовой ставки (например, при базовой 30% APR и множителе 3x — штраф 90% APR). Оплачивается в USDC или токенах через P_average.

## Калькулятор: bonding curve и кэшбек

<style>
.bc-calc { background:#fff; border:1px solid rgba(0,0,0,0.08); border-radius:12px; padding:32px; margin:36px 0; }
.bc-calc-title { font-family:'Space Grotesk',sans-serif; font-size:0.72rem; font-weight:600; text-transform:uppercase; letter-spacing:0.12em; color:#7c3aed; margin-bottom:20px; }
.bc-calc-row { margin-bottom:16px; }
.bc-calc-label { display:flex; justify-content:space-between; font-family:'Space Grotesk',sans-serif; font-size:0.82rem; font-weight:500; color:#1a1a2e; margin-bottom:4px; }
.bc-calc-val { font-family:'JetBrains Mono',monospace; font-size:0.8rem; color:#7c3aed; background:rgba(124,58,237,0.05); padding:2px 8px; border-radius:4px; }
.bc-calc-slider { width:100%; height:4px; -webkit-appearance:none; appearance:none; background:rgba(0,0,0,0.08); border-radius:2px; outline:none; accent-color:#7c3aed; }
.bc-calc-slider::-webkit-slider-thumb { -webkit-appearance:none; width:16px; height:16px; border-radius:50%; background:#7c3aed; cursor:pointer; }
.bc-calc-result { background:#f8f7f4; border-radius:8px; padding:20px; margin-top:20px; }
.bc-calc-result-block { padding:12px 0; border-bottom:1px solid rgba(0,0,0,0.06); }
.bc-calc-result-block:last-child { border-bottom:none; padding-bottom:0; }
.bc-calc-result-title { font-family:'Space Grotesk',sans-serif; font-size:0.75rem; font-weight:600; text-transform:uppercase; letter-spacing:0.08em; color:#7c3aed; margin-bottom:6px; }
.bc-calc-result-line { font-family:'Space Grotesk',sans-serif; font-size:0.88rem; color:#1a1a2e; line-height:1.7; }
.bc-calc-result-line strong { font-family:'JetBrains Mono',monospace; font-size:0.85rem; color:#1a1a2e; }
.bc-calc-result-highlight { font-family:'JetBrains Mono',monospace; font-size:1.1rem; font-weight:700; color:#7c3aed; }
</style>

<div class="bc-calc">
  <div class="bc-calc-title">Калькулятор bonding curve и кэшбека</div>

  <div class="bc-calc-row">
    <label class="bc-calc-label">Сумма депозита (USDC) <span class="bc-calc-val" id="calc-deposit-val">10 000</span></label>
    <input type="range" id="calc-deposit" class="bc-calc-slider" min="100" max="100000" value="10000" step="100">
  </div>
  <div class="bc-calc-row">
    <label class="bc-calc-label">Выпущено токенов <span class="bc-calc-val" id="calc-minted-val">500 000</span></label>
    <input type="range" id="calc-minted" class="bc-calc-slider" min="1000" max="7000000" value="500000" step="10000">
  </div>
  <div class="bc-calc-row">
    <label class="bc-calc-label">Коэффициент кэшбека (LLC) <span class="bc-calc-val" id="calc-llc-val">5%</span></label>
    <input type="range" id="calc-llc" class="bc-calc-slider" min="1" max="15" value="5" step="1">
  </div>
  <div class="bc-calc-row">
    <label class="bc-calc-label">Заполнение High-корзины <span class="bc-calc-val" id="calc-fill-val">28%</span></label>
    <input type="range" id="calc-fill" class="bc-calc-slider" min="1" max="95" value="28" step="1">
  </div>

  <div class="bc-calc-result" id="calc-results"></div>
</div>

<script>
(function(){
  var P0=1,alpha=0.45,beta=10000;
  function pMint(m){return P0*Math.pow((beta+m)/beta,alpha)}
  function pAvg(m){return m===0?P0:P0*beta/((alpha+1)*m)*(Math.pow((beta+m)/beta,alpha+1)-1)}
  function f(n){return n.toLocaleString('ru-RU',{maximumFractionDigits:2})}
  function fi(n){return n.toLocaleString('ru-RU',{maximumFractionDigits:0})}

  function update(){
    var dep=+document.getElementById('calc-deposit').value;
    var mint=+document.getElementById('calc-minted').value;
    var llc=+document.getElementById('calc-llc').value/100;
    var fill=+document.getElementById('calc-fill').value/100;

    document.getElementById('calc-deposit-val').textContent=fi(dep);
    document.getElementById('calc-minted-val').textContent=fi(mint);
    document.getElementById('calc-llc-val').textContent=Math.round(llc*100)+'%';
    document.getElementById('calc-fill-val').textContent=Math.round(fill*100)+'%';

    var price=pMint(mint),avg=pAvg(mint);
    var userCb=0.80*dep*llc/price,affCb=0.20*dep*llc/price;
    var subPrice=10*(0.01+0.15*fill),ctCount=dep/10;
    var totalSub=subPrice*ctCount/avg;
    var pct=totalSub/userCb*100;

    document.getElementById('calc-results').innerHTML=
      '<div class="bc-calc-result-block">'+
        '<div class="bc-calc-result-title">Bonding curve</div>'+
        '<div class="bc-calc-result-line">P_mint: <strong>'+f(price)+' USDC</strong> · P_average: <strong>'+f(avg)+' USDC</strong></div>'+
      '</div>'+
      '<div class="bc-calc-result-block">'+
        '<div class="bc-calc-result-title">Кэшбек за депозит '+fi(dep)+' USDC</div>'+
        '<div class="bc-calc-result-line">Кредитору: <strong>'+f(userCb)+' токенов</strong> (≈'+fi(userCb*price)+' USDC по цене минта)</div>'+
        '<div class="bc-calc-result-line">Аффилиату: <strong>'+f(affCb)+' токенов</strong></div>'+
      '</div>'+
      '<div class="bc-calc-result-block">'+
        '<div class="bc-calc-result-title">Повышение '+fi(ctCount)+' CT до High (заполнение '+Math.round(fill*100)+'%)</div>'+
        '<div class="bc-calc-result-line">Цена за 1 CT: <strong>'+f(subPrice)+' USDC</strong> = '+f(subPrice/avg)+' токенов</div>'+
        '<div class="bc-calc-result-line">Итого: <strong>'+fi(subPrice*ctCount)+' USDC = '+fi(totalSub)+' токенов</strong></div>'+
        '<div class="bc-calc-result-line" style="margin-top:8px">Стоимость защиты: <span class="bc-calc-result-highlight">'+fi(pct)+'%</span> от кэшбека</div>'+
      '</div>';
  }

  document.querySelectorAll('.bc-calc .bc-calc-slider').forEach(function(s){s.addEventListener('input',update)});
  update();
})();
</script>

## Как это помогло: анализ утилизации

### Воронка утилизации токенов

Схема потоков токенов в системе:

```
Bonding Curve (минт)
    │
    ├─── 80% → Кредитор получает кэшбек
    │         │
    │         ├─── Повышает приоритет до High → УТИЛИЗАЦИЯ
    │         ├─── Продаёт на рынке → ДАВЛЕНИЕ НА ЦЕНУ
    │         └─── Держит (уровни лояльности) → УДЕРЖАНИЕ
    │
    └─── 20% → Аффилиат / Платформа
                │
                └─── Продаёт или тратит → РЫНОК

    Субординация (100 токенов потрачено):
    ├─── 20% комиссия платформы → СЖИГАНИЕ (20 токенов)
    └─── 80% → выплата Low-кредиторам (80 токенов)
         └─── невостребованный остаток → СЖИГАНИЕ

    KYB/DD: ~22K USDC за проверку заёмщика → СПРОС НА ТОКЕН
    Штрафы: 2-4x от базовой ставки → СПРОС НА ТОКЕН
    Buyback: 10% чистой прибыли → СЖИГАНИЕ
```

### Количественная оценка

При стабильной работе платформы (допустим 10 пулов по 500K USDC, LLC = 5%):

| Метрика | Значение |
|---------|---------|
| Депозиты в месяц | 5 000 000 USDC |
| Кэшбек (5% от депозитов) | 250 000 USDC эквивалент в токенах |
| Утилизация через субординацию (при 30% кредиторов повышают) | ~75 000 USDC эквивалент |
| Оплата KYB/DD токенами (5-10 проектов × ~22K) | ~110 000-220 000 USDC эквивалент |
| Штрафы за просрочку (при 5% дефолтных траншей) | ~15 000-25 000 USDC эквивалент |
| Buyback & burn (10% от ~150K прибыли) | ~15 000 USDC эквивалент |
| **Итого утилизация** | **~215 000-335 000 USDC → 86-134% от эмиссии** |

При активном потоке заёмщиков спрос на токен через KYB/DD может превышать эмиссию кэшбека. Но это зависит от темпа привлечения новых проектов. Если приток заёмщиков замедляется, утилизация падает до ~90K USDC (субординация + buyback) — 36% от эмиссии. Модель работает, пока:

1. **Bonding curve замедляет эмиссию** — при 4 млн выпущенных токенов цена минта ~15 USDC, за тот же депозит кредитор получает в 3 раза меньше токенов
2. **Реальные дефолты повышают спрос на High** — если в первый год 2-3 пула уходят в дефолт, Low-кредиторы теряют деньги, и остальные начинают массово повышать приоритет
3. **Buyback привязан к реальной прибыли** — если платформа генерирует 3% от объёма кредитов (150K+ USDC/мес), сжигание увеличивается пропорционально

### Слабые места модели

{{< checklist title="Риски, которые мы выявили" type="curve" >}}
<li><strong>Холодный старт субординации</strong> — пока нет дефолтов, кредиторы не видят смысла повышать приоритет. Первые 6-12 месяцев утилизация через субординацию может быть близка к нулю</li>
<li><strong>Bonding curve работает только в одну сторону</strong> — нет механизма burn при возврате токенов в контракт. Если кредиторы массово продают, цена на рынке может упасть ниже P_average, создавая арбитраж</li>
<li><strong>Аффилиатная доля (20%) — чистое давление на продажу</strong> — аффилиаты не участвуют в кредитовании и не имеют стимула утилизировать токены через субординацию</li>
<li><strong>КYB/DD — разовый спрос</strong> — заёмщик платит за проверку один раз (~22K USDC). При 5-10 новых проектах в месяц это 110-220K USDC спроса, но этот канал не масштабируется с ростом TVL</li>
{{< /checklist >}}

### Итоговые выводы

{{< checklist title="Что сработало в этой модели" type="check" >}}
<li><strong>Субординация — органический спрос</strong> — кредиторы тратят токены не потому что им обещали APY, а потому что защищают свои деньги. Это самый устойчивый тип утилизации</li>
<li><strong>Bonding curve + алгоритмический кэшбек = саморегулирующаяся эмиссия</strong> — при росте активности цена минта растёт, кэшбек в токенах уменьшается. Не нужно вручную корректировать параметры</li>
<li><strong>Тройное сжигание перекрывает buyback</strong> — субординационный burn генерируется автоматически, без затрат на выкуп с рынка. Buyback — только дополнительный канал</li>
<li><strong>CT-токены как внутренний рынок</strong> — P2P-торговля позициями создаёт ещё один слой ликвидности и комиссий, не требуя внешнего DEX</li>
{{< /checklist >}}

Главный урок: в кредитном протоколе утилизация токена должна быть привязана к управлению рисками, а не к стейкингу или геймификации. Кредитор тратит токен, потому что без повышения приоритета он рискует потерять депозит — это фундаментально сильнее, чем «застейкай и получи APY».

{{< cta title="Разработаем токеномику для вашего DeFi-протокола" text="Мы проектируем модели утилизации, bonding curves и механизмы управления рисками для кредитных платформ, DEX и других DeFi-протоколов." button="Обсудить проект" link="https://gnts.io" >}}

## Читайте также

- [Bonding Curve: модель предложения в токеномике]({{< relref "models/bonding-curve" >}})
- [5 моделей предложения токенов]({{< relref "models/token-supply-models" >}})
- [Дивиденды или выкуп: сравнение двух моделей утилизации токенов]({{< relref "models/utility-models" >}})
- [Аллокация как модель предложения токенов]({{< relref "models/allocation" >}})
