---
title: "Аллокация как модель предложения токенов"
description: "Как распределить токены между командой, инвесторами и сообществом. Графики вестинга, cliff, TGE, типичные ошибки и калькулятор разблокировки."
date: 2026-04-09
weight: 20
tags:
  - токеномика
  - аллокация
  - распределение токенов
  - вестинг
categories:
  - models
---

## Что такое аллокация токенов

**Аллокация токенов** — это распределение долей общего предложения (total supply) между различными группами стейкхолдеров проекта. Если bonding curve отвечает на вопрос «по какой цене», то аллокация отвечает на вопрос **«кому и сколько»**.

Аллокация определяет баланс интересов между теми, кто строит продукт (команда), теми, кто финансирует (инвесторы), и теми, кто использует (сообщество). Неправильная аллокация убивает проекты чаще, чем плохой код.

Проект выпускает, допустим, 100 000 000 токенов. Их нужно распределить так, чтобы:

- У команды была мотивация работать долгосрочно, но не возможность сбросить токены в первый день
- У инвесторов была адекватная доходность без доминирования над остальными
- У сообщества были токены для участия в управлении и экосистемных активностях
- На рынке была достаточная ликвидность с первого дня торгов

{{< callout type="info" title="Аллокация и другие модели предложения" >}}
Аллокация не существует в вакууме. Каждая группа получает токены через свою **модель предложения**: команда — через вестинг, сообщество — через airdrop или staking-награды. Аллокация задаёт «сколько», а модель предложения — «как и когда».
{{< /callout >}}

Аллокация фиксируется до запуска и не может быть изменена после развёртывания смарт-контрактов. В отличие от динамических моделей (bonding curve, airdrop по метрикам), аллокация — это **статический план**, задающий верхние границы для каждой группы.

### Ключевые термины

| Термин | Определение |
|--------|-------------|
| **Total supply** | Максимальное количество токенов, которое когда-либо будет существовать |
| **Circulating supply** | Количество токенов, свободно обращающихся на рынке в данный момент |
| **Аллокация** | Процентное распределение total supply между группами стейкхолдеров |
| **Вестинг** | График постепенной разблокировки токенов во времени |
| **TGE** | Token Generation Event — момент создания токенов и начала торгов |
| **Cliff** | Период после TGE, в течение которого токены заблокированы полностью |


## Кто получает токены

Стандартная аллокация включает от пяти до восьми групп. Каждая группа имеет свою логику и типичный диапазон долей.

### Команда и основатели

Типичная доля: **15–20%** от total supply. Эти токены всегда идут с длинным вестингом (3–4 года) и cliff-периодом (6–12 месяцев). Доля команды не должна превышать 20% — более высокие значения вызывают обоснованные подозрения у инвесторов.

### Инвесторы (seed, private, strategic)

Типичная совокупная доля всех раундов: **15–25%**.

- **Seed-раунд** (3–8%): самые ранние инвесторы, максимальный дисконт, самый длинный вестинг
- **Private-раунд** (5–12%): институциональные инвесторы, средний дисконт
- **Strategic-раунд** (3–7%): партнёры экосистемы, часто с привязкой к KPI

### Сообщество и экосистема

Типичная совокупная доля: **30–45%**. Чем больше доля сообщества, тем более децентрализованным считается проект.

### Казначейство, ликвидность, советники

**Казначейство** (10–20%) — резерв для долгосрочного развития, управляемый через multisig или DAO. **Ликвидность** (5–10%) — токены для DEX и CEX, без которых торговля невозможна. **Советники** (2–5%) — консультанты с вестингом не короче командного.

### Сводная таблица типичных аллокаций

| Группа | Типичная доля | Cliff | Вестинг | TGE-разблокировка |
|--------|---------------|-------|---------|-------------------|
| **Команда** | 15–20% | 6–12 мес | 24–48 мес | 0% |
| **Инвесторы (seed)** | 3–8% | 6–12 мес | 18–36 мес | 0–5% |
| **Инвесторы (private)** | 5–12% | 3–6 мес | 12–24 мес | 0–10% |
| **Сообщество / airdrop** | 10–20% | 0 мес | 0–12 мес | 50–100% |
| **Staking-награды** | 10–20% | 0 мес | 48–96 мес | по мере эмиссии |
| **Казначейство** | 10–20% | 0–6 мес | 24–60 мес | 0–5% |
| **Ликвидность** | 5–10% | 0 мес | 0 мес | 100% |
| **Советники** | 2–5% | 6–12 мес | 24–36 мес | 0% |

{{< callout type="info" title="Правило проверки" >}}
Сумма всех аллокаций **всегда равна 100%**. Если при планировании сумма превышает 100%, нужно урезать одну или несколько групп. Если меньше 100% — остаток обычно добавляется в казначейство.
{{< /callout >}}


## Cliff и вестинг: как работает разблокировка

Аллокация определяет «сколько», а **вестинг** — «когда». Без вестинга аллокация бессмысленна: если все токены разблокированы сразу, ничто не мешает инсайдерам продать всё в первый день.

### Анатомия вестинг-графика

Стандартный вестинг-график состоит из трёх фаз:

1. **TGE-разблокировка** — процент токенов, доступных сразу в момент запуска (0–25%)
2. **Cliff-период** — время после TGE, в течение которого новых разблокировок нет (0–12 месяцев)
3. **Линейный вестинг** — равномерная разблокировка оставшихся токенов (6–48 месяцев)

{{< formula math="U(t) = A_total × TGE% + A_remaining × min(1, max(0, (t − cliff) / vesting))" desc="U(t) — разблокировано на момент t · A_total — аллокация группы · A_remaining = A_total × (1 − TGE%) · cliff, vesting — в месяцах" >}}

### Пример расчёта

Seed-инвестор получает 5 000 000 токенов (5% от 100M) с условиями: TGE = 5%, cliff = 6 месяцев, вестинг = 24 месяца.

```
Месяц 0 (TGE):    250,000
Месяц 1-6 (cliff): 250,000 (без изменений)
Месяц 7:          447,917
Месяц 18:         2,625,000
Месяц 30:         5,000,000 (100%)
```

### Типы вестинга

| Тип | Описание | Применение |
|-----|----------|------------|
| **Линейный** | Равномерная разблокировка каждый период | Команда, инвесторы |
| **Ступенчатый** | Разблокировка квартальными блоками | Советники, стратегические партнёры |
| **По вехам** | Привязка к достижению KPI | Гранты, экосистемный фонд |
| **Экспоненциальный** | Медленный старт, ускорение к концу | Staking-награды |

### Совокупный график разблокировки

Самый важный выходной документ аллокации — **совокупный график разблокировки**. Он показывает, какой процент total supply будет в обращении в каждый момент времени. Идеальный график имеет плавную S-образную кривую. Резкие ступеньки — плохой знак: они создают «cliff-события», когда на рынок одновременно попадает большой объём токенов.


## TGE: первичная разблокировка

**TGE** (Token Generation Event) — момент создания токенов и начала торгов. Процент токенов, разблокированных при TGE, влияет на начальную капитализацию, давление на продажу и ликвидность.

{{< formula math="MC_TGE = P_listing × CS_TGE" desc="MC — рыночная капитализация · P_listing — цена листинга · CS_TGE — circulating supply при TGE" >}}

### Типичные диапазоны TGE-разблокировки

| Группа | Типичный TGE% | Обоснование |
|--------|---------------|-------------|
| **Команда** | 0% | Демонстрация долгосрочной заинтересованности |
| **Seed-инвесторы** | 0–5% | Минимальная ликвидность для хеджирования |
| **Private-инвесторы** | 5–10% | Компенсация за меньший дисконт |
| **Public sale** | 20–50% | Розничные инвесторы ожидают быструю ликвидность |
| **Airdrop** | 50–100% | Основная функция — быстрое распределение |
| **Ликвидность** | 100% | Необходима с первого дня торгов |

{{< callout type="info" title="Золотое правило MC/FDV" >}}
Здоровый диапазон MC/FDV при TGE: **10–25%**. Ниже 10% — рынок боится будущего разводнения. Выше 30% — высокое начальное давление на продажу.
{{< /callout >}}

Самые опасные моменты после TGE — окончание cliff-периодов команды и крупных инвесторов. Решение — **смещать cliff-периоды**: seed — 6 месяцев, private — 9, команда — 12.


## Типы аллокационных моделей

Выбор модели зависит от стадии проекта, регуляторной среды и стратегии привлечения капитала.

| Параметр | Fair Launch | Private Sale | ICO / Public Sale | Launchpad |
|----------|-------------|--------------|-------------------|-----------|
| **Доля инвесторов** | 0% | 20–35% | 30–60% | 5–15% |
| **Доля команды** | 0–10% | 15–20% | 10–20% | 15–20% |
| **Доля сообщества** | 60–100% | 30–45% | 20–40% | 40–55% |
| **TGE circulating** | 80–100% | 5–15% | 30–60% | 10–20% |
| **Пример** | Bitcoin, мемкоины | Большинство L1/L2 | EOS, Tezos (2017-18) | Binance Launchpad |

**Fair launch** — все токены через открытые механизмы, без предварительных раундов. Максимальная децентрализация, но проект не получает финансирования. **Private sale** — самая распространённая модель для инфраструктурных проектов: длинные вестинги, низкий TGE circulating. **Launchpad** — гибрид: продажа через платформу-посредник с встроенной аудиторией.

{{< callout type="info" title="Тренд 2024–2026" >}}
Рынок смещается к более «честным» распределениям: больше airdrop-аллокации, меньше инвесторских долей, быстрее вестинг. Проекты с FDV/MC выше 10x при запуске теряют доверие сообщества.
{{< /callout >}}


## Частые ошибки аллокации

{{< callout type="error" title="Ошибка 1: Слишком большая доля команды" >}}
Аллокация команды выше 25% вызывает справедливый вопрос: «Для кого создаётся этот токен?» Крупные инвесторы откажутся участвовать, сообщество не поверит в децентрализацию.
{{< /callout >}}

{{< callout type="error" title="Ошибка 2: Отсутствие cliff-периода" >}}
Команда или инвесторы без cliff получают возможность продавать с первого дня. Правило: **cliff команды >= 6 месяцев**, cliff инвесторов >= 3 месяцев. Без исключений.
{{< /callout >}}

{{< callout type="warning" title="Ошибка 3: Синхронные cliff-события" >}}
Если cliff команды, seed и private заканчивается в один месяц, на рынок одновременно попадает 15–25% total supply. Решение — сдвигать cliff-периоды: seed через 6 месяцев, private через 9, команда через 12.
{{< /callout >}}

{{< callout type="warning" title="Ошибка 4: Забыть про ликвидность" >}}
Минимум 5% total supply должно быть выделено на обеспечение начальной ликвидности на DEX. Без этого проскальзывание составит 5–10% даже на мелких ордерах.
{{< /callout >}}

### Чек-лист проверки аллокации

{{< checklist type="check" >}}
<li>Сумма всех аллокаций = 100%</li>
<li>Доля команды <= 20%</li>
<li>Cliff команды >= 6 месяцев</li>
<li>Вестинг команды >= вестинг инвесторов</li>
<li>TGE circulating 10–25% (если не fair launch)</li>
<li>Ликвидность >= 5%</li>
<li>Cliff-события не совпадают (разброс >= 3 месяцев)</li>
<li>Максимальная разблокировка за месяц <= 5% total supply</li>
<li>Сообщество + экосистема >= 30%</li>
{{< /checklist >}}


## Инструменты планирования

Каждая цифра аллокации должна быть смоделирована, визуализирована и проверена на устойчивость.

### Google Sheets

Большинство аллокаций моделируется в Google Sheets. Это основной рабочий инструмент: прозрачный для инвесторов и команды, легко расширяемый. Мы подготовили [готовый шаблон модели аллокации и вестинга](/models/allocation-template/) — откройте, сделайте копию и подставьте свои числа.

### Агрегаторы разблокировок

Для бенчмаркинга полезен [Token Unlocks](https://token.unlocks.app/) — агрегатор графиков разблокировки существующих проектов. Можно посмотреть, как устроена аллокация конкурентов, и сравнить параметры.

### Визуализация: что показать инвесторам

Минимальный набор для инвесторской презентации:

1. **Круговая диаграмма аллокации** — доли групп в % от total supply
2. **Stacked area chart** — совокупный circulating supply по месяцам
3. **Гистограмма ежемесячных разблокировок** — показывает пики давления на продажу

## Калькулятор вестинг-графика

Настройте аллокацию и параметры вестинга для каждой группы. Калькулятор строит совокупный график разблокировки и предупреждает, если сумма аллокаций превышает 100%.

<style>
.calc-section { background:#fff; border:1px solid rgba(0,0,0,0.08); border-radius:12px; padding:32px; margin:36px 0; }
.calc-title { font-family:'Space Grotesk',sans-serif; font-size:0.72rem; font-weight:600; text-transform:uppercase; letter-spacing:0.12em; color:#7c3aed; margin-bottom:20px; }
.calc-slider-label { display:flex; justify-content:space-between; font-family:'Space Grotesk',sans-serif; font-size:0.82rem; font-weight:500; color:#1a1a2e; margin-bottom:4px; }
.calc-slider-value { font-family:'JetBrains Mono',monospace; font-size:0.8rem; color:#7c3aed; background:rgba(124,58,237,0.05); padding:2px 8px; border-radius:4px; }
.calc-slider { width:100%; height:4px; -webkit-appearance:none; appearance:none; background:rgba(0,0,0,0.08); border-radius:2px; outline:none; accent-color:#7c3aed; }
.calc-slider::-webkit-slider-thumb { -webkit-appearance:none; width:16px; height:16px; border-radius:50%; background:#7c3aed; cursor:pointer; }
.calc-alloc-bar { height:12px; border-radius:6px; overflow:hidden; display:flex; margin:8px 0 4px; }
.calc-alloc-bar div { height:100%; transition:width 0.3s; }
.calc-alloc-remaining { font-family:'Space Grotesk',sans-serif; font-size:0.75rem; color:#8a8a9a; margin-bottom:8px; }
.calc-alloc-remaining.over { color:#dc2626; font-weight:600; }
.calc-warning { display:none; background:rgba(220,38,38,0.08); border:1px solid #dc2626; border-radius:8px; padding:12px 16px; margin:12px 0; font-family:'Space Grotesk',sans-serif; font-size:0.85rem; font-weight:600; color:#dc2626; text-align:center; }
.calc-warning.visible { display:block; }
.calc-charts-overlay { position:relative; }
.calc-charts-overlay.blocked::after { content:''; position:absolute; inset:0; background:rgba(248,247,244,0.7); border-radius:8px; z-index:2; }
.calc-group-panel { border:1px solid rgba(0,0,0,0.08); border-radius:8px; margin-bottom:8px; overflow:hidden; }
.calc-group-header { display:flex; align-items:center; justify-content:space-between; padding:10px 14px; cursor:pointer; background:#f8f7f4; user-select:none; transition:background 0.2s; }
.calc-group-header:hover { background:rgba(139,92,246,0.08); }
.calc-group-header-left { display:flex; align-items:center; gap:8px; font-family:'Space Grotesk',sans-serif; font-size:0.85rem; font-weight:600; color:#1a1a2e; }
.calc-group-dot { width:10px; height:10px; border-radius:2px; display:inline-block; }
.calc-group-header-right { font-family:'JetBrains Mono',monospace; font-size:0.78rem; color:#7c3aed; }
.calc-group-arrow { margin-left:8px; font-size:0.7rem; color:#8a8a9a; transition:transform 0.2s; }
.calc-group-panel.open .calc-group-arrow { transform:rotate(180deg); }
.calc-group-body { display:none; padding:12px 14px 14px; background:#fff; border-top:1px solid rgba(0,0,0,0.08); }
.calc-group-panel.open .calc-group-body { display:block; }
.calc-group-row { display:grid; grid-template-columns:1fr 1fr; gap:10px; margin-top:8px; }
.alloc-charts-grid { display:grid; grid-template-columns:300px 1fr; gap:32px; align-items:start; }
@media (max-width:900px) { .alloc-charts-grid { grid-template-columns:1fr; } }
</style>

<div class="calc-section" id="alloc-calculator">
  <div class="calc-title">Калькулятор вестинг-графика</div>
  <div style="margin-bottom:20px;">
    <label class="calc-slider-label">Общее предложение (Total Supply) <span class="calc-slider-value" id="val-supply">100 000 000</span></label>
    <input type="range" id="sl-supply" class="calc-slider" min="1000000" max="10000000000" step="1000000" value="100000000">
  </div>
  <div id="group-panels"></div>
  <div class="calc-alloc-bar" id="alloc-bar"></div>
  <div class="calc-alloc-remaining" id="alloc-remaining">Распределено: 100%</div>
  <div class="calc-warning" id="alloc-warning"></div>
  <div class="calc-charts-overlay" id="calc-charts">
    <div class="alloc-charts-grid">
      <div>
        <div style="background:#f8f7f4;border:1px solid rgba(0,0,0,0.08);border-radius:8px;overflow:hidden;">
          <svg id="alloc-pie" viewBox="0 0 300 300" style="display:block;width:100%;height:300px;"></svg>
        </div>
        <div id="alloc-legend" style="margin-top:12px;font-family:'Space Grotesk',sans-serif;font-size:0.78rem;display:flex;flex-wrap:wrap;gap:8px 16px;"></div>
      </div>
      <div>
        <div style="background:#f8f7f4;border:1px solid rgba(0,0,0,0.08);border-radius:8px;overflow:hidden;">
          <svg id="alloc-chart" viewBox="0 0 520 300" preserveAspectRatio="xMidYMid meet" style="display:block;width:100%;height:300px;"></svg>
        </div>
        <table style="margin-top:16px;width:100%;border-collapse:collapse;font-size:0.8rem;">
          <thead><tr>
            <th style="text-align:left;font-family:'Space Grotesk',sans-serif;font-weight:600;font-size:0.7rem;text-transform:uppercase;letter-spacing:0.06em;color:#7c3aed;padding:6px 10px;border-bottom:1px solid rgba(0,0,0,0.08);">Месяц</th>
            <th style="text-align:left;font-family:'Space Grotesk',sans-serif;font-weight:600;font-size:0.7rem;text-transform:uppercase;letter-spacing:0.06em;color:#7c3aed;padding:6px 10px;border-bottom:1px solid rgba(0,0,0,0.08);">Разблокировано</th>
            <th style="text-align:left;font-family:'Space Grotesk',sans-serif;font-weight:600;font-size:0.7rem;text-transform:uppercase;letter-spacing:0.06em;color:#7c3aed;padding:6px 10px;border-bottom:1px solid rgba(0,0,0,0.08);">% от Supply</th>
            <th style="text-align:left;font-family:'Space Grotesk',sans-serif;font-weight:600;font-size:0.7rem;text-transform:uppercase;letter-spacing:0.06em;color:#7c3aed;padding:6px 10px;border-bottom:1px solid rgba(0,0,0,0.08);">За месяц</th>
          </tr></thead>
          <tbody id="alloc-tbody"></tbody>
        </table>
      </div>
    </div>
  </div>
</div>
<script>
(function(){
  var $=function(id){return document.getElementById(id)};
  var groups=[
    {key:'team',name:'Команда',color:'#7c3aed',pct:18,tge:0,cliff:12,vest:48},
    {key:'inv',name:'Инвесторы',color:'#2563eb',pct:15,tge:5,cliff:6,vest:24},
    {key:'comm',name:'Сообщество',color:'#059669',pct:30,tge:60,cliff:0,vest:12},
    {key:'tres',name:'Казначейство',color:'#d97706',pct:15,tge:0,cliff:3,vest:36},
    {key:'liq',name:'Ликвидность',color:'#dc2626',pct:8,tge:100,cliff:0,vest:1},
    {key:'stak',name:'Staking',color:'#0891b2',pct:10,tge:0,cliff:0,vest:54},
    {key:'adv',name:'Советники',color:'#a855f7',pct:4,tge:0,cliff:9,vest:36}
  ];
  var panelsEl=$('group-panels');
  var panelHTML='';
  groups.forEach(function(g){
    panelHTML+='<div class="calc-group-panel" id="panel-'+g.key+'">'+
      '<div class="calc-group-header" onclick="this.parentElement.classList.toggle(\'open\')">'+
        '<span class="calc-group-header-left"><span class="calc-group-dot" style="background:'+g.color+'"></span>'+g.name+'</span>'+
        '<span class="calc-group-header-right"><span id="val-'+g.key+'">'+g.pct+'</span>%<span class="calc-group-arrow">&#9660;</span></span>'+
      '</div>'+
      '<div class="calc-group-body">'+
        '<div><label class="calc-slider-label">Доля (%) <span class="calc-slider-value" id="valsl-'+g.key+'">'+g.pct+'</span></label>'+
        '<input type="range" id="sl-'+g.key+'" class="calc-slider" min="0" max="50" step="1" value="'+g.pct+'"></div>'+
        '<div class="calc-group-row">'+
          '<div><label class="calc-slider-label">TGE (%) <span class="calc-slider-value" id="val-tge-'+g.key+'">'+g.tge+'</span></label>'+
          '<input type="range" id="sl-tge-'+g.key+'" class="calc-slider" min="0" max="100" step="1" value="'+g.tge+'"></div>'+
          '<div><label class="calc-slider-label">Cliff (мес) <span class="calc-slider-value" id="val-cliff-'+g.key+'">'+g.cliff+'</span></label>'+
          '<input type="range" id="sl-cliff-'+g.key+'" class="calc-slider" min="0" max="24" step="1" value="'+g.cliff+'"></div>'+
        '</div>'+
        '<div class="calc-group-row">'+
          '<div><label class="calc-slider-label">Вестинг (мес) <span class="calc-slider-value" id="val-vest-'+g.key+'">'+g.vest+'</span></label>'+
          '<input type="range" id="sl-vest-'+g.key+'" class="calc-slider" min="1" max="72" step="1" value="'+g.vest+'"></div>'+
          '<div></div>'+
        '</div>'+
      '</div>'+
    '</div>';
  });
  panelsEl.innerHTML=panelHTML;
  var pieSvg=$('alloc-pie'),chartSvg=$('alloc-chart'),tbody=$('alloc-tbody'),legend=$('alloc-legend'),bar=$('alloc-bar'),remaining=$('alloc-remaining'),warning=$('alloc-warning'),chartsWrap=$('calc-charts');
  function fmt(v){if(v>=1e9)return(v/1e9).toFixed(1)+'B';if(v>=1e6)return(v/1e6).toFixed(1)+'M';if(v>=1e3)return(v/1e3).toFixed(0)+'K';return v.toFixed(0)}
  function render(){
    var supply=+$('sl-supply').value;
    $('val-supply').textContent=(+supply).toLocaleString('ru-RU');
    var total=0,gData=[];
    groups.forEach(function(g){
      var pct=+$('sl-'+g.key).value,tge=+$('sl-tge-'+g.key).value,cliff=+$('sl-cliff-'+g.key).value,vest=+$('sl-vest-'+g.key).value;
      $('val-'+g.key).textContent=pct;$('valsl-'+g.key).textContent=pct;
      $('val-tge-'+g.key).textContent=tge;$('val-cliff-'+g.key).textContent=cliff;$('val-vest-'+g.key).textContent=vest;
      total+=pct;
      gData.push({key:g.key,name:g.name,color:g.color,pct:pct,tge:tge/100,cliff:cliff,vest:Math.max(1,vest)});
    });
    var barHtml='';gData.forEach(function(g){if(g.pct>0)barHtml+='<div style="width:'+g.pct+'%;background:'+g.color+';"></div>';});
    bar.innerHTML=barHtml;
    remaining.textContent='Распределено: '+total+'%';
    remaining.className='calc-alloc-remaining'+(total!==100?' over':'');
    if(total<100)remaining.textContent+=' (осталось '+(100-total)+'%)';
    if(total>100)remaining.textContent+=' (превышение на '+(total-100)+'%)';
    if(total>100){warning.textContent='\u26A0\uFE0F Сумма аллокаций: '+total+'% — превышает 100%!';warning.classList.add('visible');chartsWrap.classList.add('blocked')}
    else{warning.classList.remove('visible');chartsWrap.classList.remove('blocked')}
    var legHtml='';gData.forEach(function(g){if(g.pct>0)legHtml+='<span style="display:flex;align-items:center;gap:4px;"><span style="width:10px;height:10px;border-radius:2px;background:'+g.color+';display:inline-block;"></span>'+g.name+' '+g.pct+'%</span>';});
    legend.innerHTML=legHtml;
    var cx=150,cy=150,r=120,pieHtml='',angle=-Math.PI/2,pieTotal=Math.max(total,1);
    gData.forEach(function(g){if(g.pct<=0)return;var frac=g.pct/pieTotal,da=frac*2*Math.PI;var x1=cx+r*Math.cos(angle),y1=cy+r*Math.sin(angle),x2=cx+r*Math.cos(angle+da),y2=cy+r*Math.sin(angle+da),large=da>Math.PI?1:0;pieHtml+='<path d="M'+cx+','+cy+' L'+x1+','+y1+' A'+r+','+r+' 0 '+large+',1 '+x2+','+y2+' Z" fill="'+g.color+'" opacity="0.85"/>';var mid=angle+da/2,lx=cx+(r*0.65)*Math.cos(mid),ly=cy+(r*0.65)*Math.sin(mid);if(frac>0.05)pieHtml+='<text x="'+lx+'" y="'+(ly+4)+'" text-anchor="middle" fill="#fff" font-size="11" font-family="Space Grotesk,sans-serif" font-weight="600">'+g.pct+'%</text>';angle+=da;});
    pieSvg.innerHTML=pieHtml;
    var maxMonth=48;gData.forEach(function(g){var end=g.cliff+g.vest+3;if(end>maxMonth)maxMonth=Math.min(end,72)});
    var monthData=[],prevTotal=0;
    for(var m=0;m<=maxMonth;m++){var row={total:0,groups:{},delta:0};gData.forEach(function(g){var alloc=(g.pct/100)*supply;if(alloc<=0){row.groups[g.key]=0;return}var tgeAmt=alloc*g.tge,restAmt=alloc-tgeAmt,unlocked=tgeAmt;if(m>g.cliff&&g.vest>0)unlocked+=Math.min(restAmt,restAmt*(m-g.cliff)/g.vest);unlocked=Math.min(unlocked,alloc);row.groups[g.key]=unlocked;row.total+=unlocked});row.delta=row.total-prevTotal;prevTotal=row.total;monthData.push(row)}
    var W=520,H=300,pd={t:20,r:20,b:40,l:65},pw=W-pd.l-pd.r,ph=H-pd.t-pd.b,maxVal=supply,ch='';
    for(var gi2=0;gi2<=4;gi2++){var gy=pd.t+(gi2/4)*ph;ch+='<line x1="'+pd.l+'" y1="'+gy+'" x2="'+(W-pd.r)+'" y2="'+gy+'" stroke="#8a8a9a" stroke-opacity="0.15" stroke-dasharray="4,4"/>';ch+='<text x="'+(pd.l-8)+'" y="'+(gy+4)+'" text-anchor="end" fill="#8a8a9a" font-size="9" font-family="JetBrains Mono,monospace">'+fmt(maxVal*(1-gi2/4))+'</text>'}
    for(var gi3=0;gi3<=4;gi3++){var gx=pd.l+(gi3/4)*pw;ch+='<text x="'+gx+'" y="'+(H-8)+'" text-anchor="middle" fill="#8a8a9a" font-size="9" font-family="JetBrains Mono,monospace">'+Math.round(maxMonth*gi3/4)+'м</text>'}
    var gKeys=gData.map(function(g){return g.key}).filter(function(k){return gData.filter(function(g2){return g2.key===k})[0].pct>0});
    for(var gi=gKeys.length-1;gi>=0;gi--){var pts='',basePts='';for(var m2=0;m2<=maxMonth;m2++){var x=pd.l+(m2/maxMonth)*pw,cumAbove=0;for(var j=0;j<=gi;j++)cumAbove+=monthData[m2].groups[gKeys[j]];var y=pd.t+ph-(cumAbove/maxVal)*ph;pts+=x+','+y+' '}for(var m3=maxMonth;m3>=0;m3--){var x2=pd.l+(m3/maxMonth)*pw,cumBelow=0;for(var j2=0;j2<gi;j2++)cumBelow+=monthData[m3].groups[gKeys[j2]];var y2=pd.t+ph-(cumBelow/maxVal)*ph;basePts+=x2+','+y2+' '}var gColor=gData.filter(function(g){return g.key===gKeys[gi]})[0].color;ch+='<polygon points="'+pts+basePts+'" fill="'+gColor+'" opacity="0.6"/>'}
    chartSvg.innerHTML=ch;
    var milestones=[0,3,6,12,18,24,36,48,60].filter(function(m){return m<=maxMonth});
    var tRows=milestones.map(function(m){var d=monthData[m],pct2=(d.total/supply*100).toFixed(1),delta=m>0?d.total-monthData[Math.max(0,m-1)].total:d.total;return '<tr><td style="padding:5px 10px;font-family:JetBrains Mono,monospace;font-size:0.78rem;color:#4a4a5a;border-bottom:1px solid rgba(0,0,0,0.06)">'+m+'</td><td style="padding:5px 10px;font-family:JetBrains Mono,monospace;font-size:0.78rem;color:#4a4a5a;border-bottom:1px solid rgba(0,0,0,0.06)">'+fmt(d.total)+'</td><td style="padding:5px 10px;font-family:JetBrains Mono,monospace;font-size:0.78rem;color:#4a4a5a;border-bottom:1px solid rgba(0,0,0,0.06)">'+pct2+'%</td><td style="padding:5px 10px;font-family:JetBrains Mono,monospace;font-size:0.78rem;color:#4a4a5a;border-bottom:1px solid rgba(0,0,0,0.06)">+'+fmt(Math.max(0,delta))+'</td></tr>'});
    tbody.innerHTML=tRows.join('');
  }
  document.querySelectorAll('.calc-section .calc-slider').forEach(function(s){s.addEventListener('input',render)});
  render();
})();
</script>


## Примеры аллокаций

Два анонимизированных паттерна, основанных на реальных проектах разного типа.

### Паттерн 1: Инфраструктурный L1-протокол

Венчурное финансирование $30M+, total supply 1B токенов.

| Группа | Доля | Cliff | Вестинг | TGE% |
|--------|------|-------|---------|------|
| **Команда** | 18% | 12 мес | 48 мес | 0% |
| **Инвесторы (seed + private + strategic)** | 20% | 3–6 мес | 12–24 мес | 0–10% |
| **Staking** | 25% | 0 | 72 мес | эмиссия |
| **Экосистемный фонд** | 15% | 0 | 48 мес | 2% |
| **Казначейство** | 10% | 6 мес | 36 мес | 0% |
| **Ликвидность + airdrop** | 9% | 0 | 0 | 100% |
| **Советники** | 3% | 12 мес | 36 мес | 0% |

**TGE circulating ~10%.** Характерная черта: большая доля staking (25%) с длинной эмиссией — токены приходят в обращение через механизм, требующий блокировки.

### Паттерн 2: DeFi-протокол с airdrop-фокусом

Один private-раунд $5M, total supply 100M токенов.

| Группа | Доля | Cliff | Вестинг | TGE% |
|--------|------|-------|---------|------|
| **Команда** | 15% | 6 мес | 36 мес | 0% |
| **Инвесторы** | 12% | 3 мес | 18 мес | 10% |
| **Airdrop** | 15% | 0 | 6 мес | 50% |
| **Ликвидность** | 10% | 0 | 0 | 100% |
| **Награды протокола** | 20% | 0 | 36 мес | 0% |
| **Казначейство + гранты** | 23% | 3 мес | 24 мес | 0% |
| **Советники** | 5% | 6 мес | 24 мес | 0% |

**TGE circulating ~19%.** Крупный airdrop (15%) с быстрой разблокировкой создаёт «взрывной» старт, стимулируя активность в первые недели.

{{< callout type="success" title="Общий принцип" >}}
Чем больше доля инвесторов, тем длиннее вестинг и ниже TGE-разблокировка. Доли сообщества, наоборот, разблокируются быстрее — цель в немедленном вовлечении пользователей.
{{< /callout >}}


---

## Связанные материалы

- [5 моделей предложения токенов]({{< relref "token-supply-models.md" >}}) — обзор всех моделей предложения, включая bonding curve, airdrop и reward
- [Что такое токеномика]({{< relref "/basics/what-is-tokenomics.md" >}}) — базовые понятия и структура токеномики проекта


{{< cta title="Нужна модель аллокации для вашего проекта?" text="Спроектируем распределение токенов, графики вестинга и подготовим модель в Google Sheets." >}}
