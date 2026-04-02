# Стандарти діаграм і схем

> **Область застосування:** цей документ визначає які стандарти, інструменти
> та workflow використовує кожна команда при створенні діаграм, схем і
> технічних креслень для звіту ERC 2026 та всієї подальшої документації.
>
> **Навіщо:** однакові діаграми роблять звіт професійним, допомагають журі
> оцінювати дизайн, і дозволяють членам команди читати роботу одне одного.

---

## Швидка довідка — який стандарт для якої команди

| Команда | Тип діаграми | Стандарт | Інструмент |
|---|---|---|---|
| **Ground Station** | Схеми комунікацій, мережеві блок-діаграми | ECSS-E-ST-10-06C | TikZ |
| **Electronics** | Електричні схеми | IEC 60617 / IEEE 315 | EasyEDA → export PDF |
| **Electronics** | Блок-діаграми живлення | ECSS-E-ST-20C | TikZ |
| **Arm** | Механічні креслення, CAD-види | ISO 128 | CAD → export PDF |
| **Arm** | Кінематика / блок-діаграми керування | ECSS-E-ST-10-06C | TikZ |
| **Suspension** | Механічні креслення, CAD-види | ISO 128 | CAD → export PDF |
| **Suspension** | Результати FEA | — | Скріншот з CAD → PDF |
| **Navigation** | Архітектура ПЗ, ROS node graphs | ECSS-E-ST-10-06C | TikZ |
| **Navigation** | Розміщення сенсорів на ровері | ISO 128 | CAD → export PDF |
| **Science** | Блок-діаграми інструментів | ECSS-E-ST-10-06C | TikZ |
| **Science** | Схеми потоку зразків | ECSS-E-ST-10-06C | TikZ |
| **Drone** | Механічні креслення | ISO 128 | CAD → export PDF |
| **Drone** | Авіоніка / схеми проводки | IEC 60617 / ECSS-E-ST-10-06C | EasyEDA або TikZ |
| **Всі команди** | Product Tree / PBS | ECSS-M-ST-10C Rev.1 | TikZ (`forest` пакет) |
| **Всі команди** | RIO теплова карта | ECSS-M-ST-80C | TikZ (кольорова таблиця) |

---

## Нова структура файлів (важливо)

У проєкті більше немає командних `design.tex` для основного опису.

Тепер контент вставляється у файли:

- `sections/design/<subsection>.tex`

Приклади:

- `sections/design/2_3_5_link_performance_analysis.tex`
- `sections/design/2_4_3_rover_camera_suite.tex`
- `sections/design/5_3_communications_ground_control.tex`

`sections/preliminary_design_description.tex` лише збирає ці підсекції через `\input{...}`.

---

## Узгодження з REPORT_UPDATES (контент навколо діаграм)

Щоб діаграми та текст працювали як єдине ціле:

- Не дублюйте `REQ-*` у тілі subsection-тексту; примітки про відповідність вимогам додаються централізовано.
- Пояснення до діаграм мають бути короткими, переважно bullet-форматом.
- У підписах і тексті використовуйте спрощені назви компонентів; якщо модель ще не вибрана, ставте `TBD`.
- Для куплених вузлів залишайте тільки параметри, що впливають на інженерні рішення (маса, потужність, інтерфейси, надійність), без маркетингового опису.

---

## Стандарти — детально

### ECSS-E-ST-10-06C — Технічна документація та інтерфейси
**Використовують:** всі команди для блок-діаграм, схем комунікацій, архітектури ПЗ.

Основний стандарт для інженерних діаграм у космічних / rover проєктах. Визначає:
- Нотацію блоків і стрілок
- Формат документів керування інтерфейсами (ICD)
- Консистентне підписування: кожен блок має назву, кожна стрілка — напрямок і протокол

**Основні правила для наших діаграм:**
- Кожен блок має мати підпис (назва + ключовий параметр, напр. `Jetson Orin / ROS2`)
- Кожна стрілка показує напрямок (`→` або `↔`) і тип протоколу / сигналу
- Пов'язані блоки групуються у пунктирний або суцільний прямокутник із заголовком секції
- Мінімальний шрифт усередині діаграми — 7 pt (бажано 10 pt), той самий шрифт що в документі

**Повний текст:** https://ecss.nl/standard/ecss-e-st-10-06c-technical-and-interface-documentation/

---

### IEC 60617 / IEEE 315 — Символи електричних схем
**Використовують:** Electronics, Drone (схеми проводки).

Визначає стандартні символи для резисторів, конденсаторів, MOSFET, op-amp, роз'ємів тощо.

**Основні правила:**
- Використовувати лише стандартні символи IEC — без власних вигаданих символів
- Кожен компонент отримує позначення (R1, C3, U2, J1…)
- Кожна шина / дріт іменується якщо несе іменований сигнал
- Шини живлення підписані з напругою: `+12V`, `+5V`, `GND`
- EasyEDA використовує символи IEC 60617 за замовчуванням ✓

**Довідник символів:** https://std.iec.ch/iec60617

---

### ISO 128 — Технічна документація продукту (креслення)
**Використовують:** Arm, Suspension, Drone для CAD-креслень.

Визначає типи ліній, проєкції, простановку розмірів для механічних креслень.

**Основні правила для наших експортів:**
- Використовувати ортогональну проєкцію (спереду / зверху / збоку) або ізометрію — підписувати який вид
- Включати основний напис (назва деталі, масштаб, одиниці в мм, автор, дата)
- Проставляти розміри критичних елементів відповідно до вимог ERC (макс. розмір, деталі що впливають на масу)
- Експортувати з CAD як **векторний PDF**, а не растровий PNG

**Більшість CAD-інструментів (SolidWorks, FreeCAD, Fusion 360) застосовують ISO 128 автоматично.**

---

### ECSS-M-ST-10C Rev.1 — Структура розбивки продукту (PBS)
**Використовують:** всі команди для Product Tree / PBS діаграм.

Визначає ієрархічну нотацію для систем, підсистем та компонентів.

**Рівні ієрархії для нашого ровера:**
```
Рівень 0 — Система     (UCU Mars Rover)
Рівень 1 — Підсистема  (Шасі, Електроніка, Рука, Наука, Навігація, GS, Дрон)
Рівень 2 — Вузол       (Rocker-bogie, Мотори приводу, Плата живлення…)
Рівень 3 — Компонент   (Мотор × 6, BMS, Jetson Orin…)
```

**Повний текст:** https://ecss.nl/standard/ecss-m-st-10c-rev-1-project-planning-and-implementation/

---

### ECSS-M-ST-80C — Управління ризиками
**Використовують:** таблиця RIO і теплова карта в `sections/rio_analysis.tex`.

Визначає матрицю 5×5 ймовірність/серйозність, розрахунок індексу ризику та стратегії реагування
— вже реалізовано в шаблоні звіту.

---

## Правила візуального стилю (для всіх TikZ-діаграм)

Ці правила роблять всі діаграми однаковими по всьому звіту.

### Кольори
```latex
% Визначено один раз у preamble.tex — вже є там
\definecolor{ercblue}{RGB}{0,70,127}       % заголовки секцій, основні блоки
\definecolor{templateblue}{RGB}{0,112,192} % другорядний акцент
```

Використовувати тільки ці кольори в діаграмах:
- **Заливка блоку:** білий (`fill=white`) з чорною рамкою (`draw=black`)
- **Головний / ключовий вузол:** світло-синя заливка `fill=ercblue!15, draw=ercblue`
- **Рамка секції:** світло-сіра заливка `fill=black!5, draw=black, dashed`
- **Стрілки:** чорні (`draw=black`)
- Без веселки кольорів — деякі члени журі друкують звіт чорно-білим

### Шрифти всередині TikZ
```latex
font=\small        % основний підпис (≈10pt)
font=\scriptsize   % підзаголовок / анотація протоколу (≈8pt)
font=\tiny         % анотації на стрілках (≈6pt, використовувати рідко)
```

### Товщина ліній
```latex
line width=0.5pt   % звичайні стрілки і рамки блоків
line width=0.8pt   % рамки секцій / груп
line width=1.2pt   % основний / виділений вузол
```

### Стилі стрілок — використовувати ці іменовані стилі
Скопіюй на початок своєї TikZ-діаграми:

```latex
\tikzset{
  % Стандартна направлена стрілка
  arr/.style={->, >=Stealth, line width=0.5pt, draw=black},
  % Двонаправлений зв'язок
  bidi/.style={<->, >=Stealth, line width=0.9pt, draw=black},
  % Бездротовий / RF зв'язок (пунктир)
  dlink/.style={->, >=Stealth, line width=0.5pt, dashed, draw=black},
  % Тимчасовий / передзмагальний зв'язок (точковий)
  dotlink/.style={<->, >=Stealth, line width=0.9pt, dotted, draw=black},
  % Провідний Ethernet (жирний)
  ethl/.style={->, >=Stealth, line width=0.9pt, draw=black},
}
```

---

## Workflow: створення діаграми

### TikZ блок-діаграми (всі команди)

```
1. Скопіюй стартовий сніпет нижче у teams/<твоя_команда>/figures/<назва>.tex
2. Малюй діаграму в тому файлі (НЕ inline у `sections/preliminary_design_description.tex`)
3. Вставляй у відповідний файл `sections/design/<subsection>.tex` через:

      \begin{figure}[H]
        \centering
        \resizebox{\linewidth}{!}{\input{teams/<команда>/figures/<назва>}}
        \caption{Підпис діаграми}
        \label{fig:твій_label}
      \end{figure}

4. Компілюй з кореня репо: latexmk -pdf main.tex
5. Перевір: чи читається діаграма на аркуші A4?
   Якщо текст занадто дрібний → спрощуй або розбивай на дві діаграми
6. Commit: git add teams/<команда>/figures/<назва>.tex && git commit
```

### EasyEDA електричні схеми (Electronics, Drone)

```
1. Намалюй схему в EasyEDA за символами IEC 60617
2. File → Export → PDF  (НЕ PNG — PDF зберігає векторну якість)
3. Збережи як: teams/electronics/figures/<назва>.pdf
4. Вставляй у відповідний `sections/design/<subsection>.tex` через `\includegraphics` (як будь-яке фото)
5. Переконайся що схема має основний напис (назва, ревізія, дата)
6. Commit: git add teams/electronics/figures/<назва>.pdf && git commit
```

### CAD креслення (Arm, Suspension, Drone)

```
1. Підготуй вид у CAD (SolidWorks / FreeCAD / Fusion 360)
2. Експортуй як PDF (аркуш креслення, не скріншот 3D-вікна)
3. Збережи як: teams/<команда>/figures/<назва>.pdf
4. Вставляй у відповідний `sections/design/<subsection>.tex` через `\includegraphics`
5. Переконайся що основний напис читається на аркуші A4
6. Commit: git add teams/<команда>/figures/<назва>.pdf && git commit
```

---

## GitHub workflow для діаграм

Кожна зміна діаграми — окремий commit з зрозумілим повідомленням:

```bash
# Нова діаграма
git add teams/ground_station/figures/gs_communication.tex
git commit -m "feat(gs): add communication schema TikZ diagram"

# Оновлення існуючої
git add teams/electronics/figures/power_arch.pdf
git commit -m "fix(electronics): update power architecture after BMS change"

# Формат повідомлень:
# feat(<команда>): <що додано>
# fix(<команда>):  <що виправлено>
# refactor(<команда>): <що реструктуровано>
```

**Якщо діаграма велика і в процесі** — роби в окремій гілці:
```bash
git checkout -b feat/gs-communication-diagram
# ... малюєш діаграму ...
git push origin feat/gs-communication-diagram
# Відкривай Pull Request на GitHub коли готово
```

**Що НЕ комітити:**
```
# Додай до .gitignore якщо ще немає:
*.aux *.log *.out *.toc *.fls *.fdb_latexmk *.synctex.gz
main.pdf   # компілюється локально, не потрібен у репо
```

---

## Стартовий TikZ сніпет

Скопіюй у `teams/<твоя_команда>/figures/<назва_діаграми>.tex`.
Рядки з `\documentclass` — тільки для окремого тестування, в репо їх видаляй.

```latex
% Тільки для standalone-тесту — ВИДАЛИ при вставці в репо:
% \documentclass[border=5pt]{standalone}
% \usepackage{tikz}
% \usetikzlibrary{arrows.meta,positioning,fit,calc,backgrounds}
% \definecolor{ercblue}{RGB}{0,70,127}
% \begin{document}

\tikzset{
  blk/.style={
    rectangle, draw=black, fill=white, line width=0.5pt,
    minimum width=4.0cm, minimum height=0.9cm,
    align=center, font=\small
  },
  blkhi/.style={
    blk, line width=1.2pt, fill=ercblue!15, draw=ercblue
  },
  arr/.style={->, >=Stealth, line width=0.5pt, draw=black},
  bidi/.style={<->, >=Stealth, line width=0.9pt, draw=black},
  dlink/.style={->, >=Stealth, line width=0.5pt, dashed, draw=black},
  ethl/.style={->, >=Stealth, line width=0.9pt, draw=black},
}

\begin{tikzpicture}[node distance=1.4cm]

  %% === ВУЗЛИ ===
  \node[blkhi] (main)                     {Головний комп'ютер\\{\scriptsize Jetson Orin · ROS2}};
  \node[blk]   (sensor) [below of=main]   {Сенсор\\{\scriptsize USB 3.0}};
  \node[blk]   (output) [right=2cm of main]{Вивід\\{\scriptsize ETH RJ45}};

  %% === СТРІЛКИ ===
  \draw[arr]  (sensor) -- (main)    node[midway, right, font=\tiny] {дані};
  \draw[ethl] (main)   -- (output)  node[midway, above, font=\tiny] {ETH};

  %% === РАМКА СЕКЦІЇ ===
  \begin{scope}[on background layer]
    \node[
      draw=black, fill=black!5, dashed, line width=0.8pt,
      inner sep=8pt, rounded corners=3pt,
      fit=(main)(sensor),
      label={[font=\small\bfseries]above:ROVER}
    ] {};
  \end{scope}

\end{tikzpicture}

% \end{document}
```

**Необхідні пакети** — додай у `preamble.tex` якщо ще немає:
```latex
\usepackage{tikz}
\usetikzlibrary{arrows.meta, positioning, fit, calc, backgrounds}
\usepackage[edges]{forest}   % тільки для Product Tree
```

---

## Сніпет Product Tree (всі команди)

```latex
% teams/<команда>/figures/pbs_rover.tex
% Потребує: \usepackage[edges]{forest}

\begin{forest}
  for tree={
    draw, rounded corners=2pt,
    align=center, font=\small,
    edge={->, >=Stealth, line width=0.5pt},
    l sep=1.2cm, s sep=0.4cm,
    where level=0{fill=ercblue!20, line width=1pt}{
      where level=1{fill=ercblue!10}{fill=white}
    },
  }
  [UCU Space Robotics\\{\scriptsize Рівень 0 — Система}
    [Шасі\\{\scriptsize Р1}
      [Рама\\{\scriptsize Р2}]
      [Колеса $\times$6\\{\scriptsize Р2}]
    ]
    [Електроніка\\{\scriptsize Р1}
      [Jetson Orin\\{\scriptsize Р2}]
      [Плата живлення\\{\scriptsize Р2}]
    ]
    [Рука\\{\scriptsize Р1}
      [Суглоби $\times$6\\{\scriptsize Р2}]
      [Захоплювач\\{\scriptsize Р2}]
    ]
  ]
\end{forest}
```

---

## Корисні посилання

| Ресурс | Посилання |
|---|---|
| ECSS стандарти (безкоштовне завантаження) | https://ecss.nl/standards/ |
| TikZ & PGF мануал | https://tikz.dev |
| CircuiTikZ мануал (електричні схеми) | https://ctan.org/pkg/circuitikz |
| `forest` пакет (дерева / PBS) | https://ctan.org/pkg/forest |
| EasyEDA (електричні схеми) | https://easyeda.com |
| Браузер символів IEC 60617 | https://std.iec.ch/iec60617 |
| GitHub репо команди | https://github.com/<org>/<repo> |
| GitHub Markdown guide | https://docs.github.com/en/get-started/writing-on-github |

> **Заміни** `https://github.com/<org>/<repo>` на реальне посилання на ваше репо.

---

## Чеклист перед комітом діаграми

- [ ] Файл лежить у `teams/<команда>/figures/` — не inline в `sections/preliminary_design_description.tex`
- [ ] Кожен блок має підпис (назва + ключовий параметр)
- [ ] Кожна стрілка має напрямок і підпис протоколу / сигналу
- [ ] Шрифт читається на аркуші A4 (мінімум `\scriptsize` ≈ 8pt)
- [ ] Кольори відповідають правилам стилю (білий / ercblue, без веселки)
- [ ] EasyEDA експортований як PDF, а не PNG
- [ ] CAD — аркуш креслення з основним написом, не скріншот 3D-вікна
- [ ] У `sections/design/<subsection>.tex` є `\caption{}` і `\label{}` для цієї фігури
- [ ] Діаграма компілюється без помилок (`latexmk -pdf main.tex` з кореня репо)
- [ ] Commit message відповідає формату: `feat(<команда>): <опис>`