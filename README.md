# Preliminary Report — ERC 2026

## General

Завдання звіту - максимально коротко, скорочуючи текст до мінімуму, за допомогою обрахунків, схем, фото та 3д моделей описати дизайн нашого марсохода та дрона,  а також - обгрунтувати виконання нами всіх вимог.

Для цього в нас є 25 сторінок, не враховуючи титулки, змісту, та додатків. Тому текст має бути короткий, фото, схеми та таблиці мають бути акуратними та змістовними, щоб зекономити місце. 
Великі схеми варто винести у відділ додатків.

Пріоритет правил: якщо між документами є конфлікт, використовуємо актуальні правила з `REPORT_UPDATES.md`.


---

## Report updates !МЕГА ВАЖЛИВО!
[REPORT_UPDATES.md](REPORT_UPDATES.md)

Коротко по актуальних правилах:
- Не додавайте `REQ-*` у текст subsection-файлів; примітки по вимогах додаються централізовано.
- Пишіть коротко: булети замість довгих абзаців.
- Використовуйте спрощені назви компонентів; якщо модель не визначена, ставте `TBD`.
- Мінімізуйте опис куплених деталей без інженерної аналітики.


## Diagram standarts

**Всі стандарти** описані у [DIAGRAM_STANDARDS.md](DIAGRAM_STANDARDS.md)

## Conribution rules

**Всі правила для контрибуції** описані у [CONTRIBUTING.md](CONTRIBUTING.md)

# !Обов'язково перечитай правила!

## Quick start

Installation of tools:
```bash
# Full bundle (~4 GB) (recommended to avoid issues):
sudo apt install texlive-full latexmk

# Required package for SI units formatting:
sudo apt install texlive-science
```

Compile PDF and clean auxiliary files:
```bash
latexmk -pdf main.tex && latexmk -c
```

---

## Структура репозиторію

```
.
├── main.tex                    # Точка входу — тільки \include, нічого більше
├── preamble.tex                # Всі \usepackage, стилі, кольори — не чіпай без потреби
│
├── sections/                   # Головні розділи звіту (структура фіксована ERC)
│   ├── cover.tex               # Титульна сторінка
│   ├── matrix_of_compliance.tex       # Розділ 1 — Matrix of Compliance
│   ├── preliminary_test_plan.tex      # Розділ 2 — Test Plan
│   ├── preliminary_design_description.tex  # Розділ 3 — Design Description
│   ├── rio_analysis.tex               # Розділ 4 — RIO Analysis
│   └── project_budget.tex             # Розділ 5 — Project Budget
│
├── teams/                      # Технічний контент кожної підкоманди
│   ├── arm/
│   │   └── tests.tex           # Тести специфічні для маніпулятора
│   ├── drone/
│   │   ├── tests.tex
│   ├── electronics/
│   │   ├── design.tex          # Схеми живлення, електроніка
│   │   └── tests.tex
│   ├── ground_station/
│   │   └── tests.tex
│   ├── navigation/
│   │   └── tests.tex
│   ├── science/
│   │   └── tests.tex
│   └── suspension/
│       └── tests.tex
│
├── tables/                     # Великі таблиці винесені окремо
│   ├── compliance_summary.tex  # Зведена таблиця відповідності вимогам (Розділ 1)
│   ├── rio_table.tex           # Таблиця ризиків RIO (Розділ 4)
│   └── test_plan.tex           # Таблиця тестів (Розділ 2)
│
├── figures/                    # Всі графічні матеріали
│   ├── tikz/                   # TikZ діаграми як окремі .tex файли
│   │   └── *.tex               # \input{figures/tikz/назва.tex}
│   ├── photos/                 # Фото ровера, деталей, прототипів (.jpg, .png)
│   │   └── *.jpg / *.png
│   └── cad/                    # PDF-експорти з CAD або EasyEDA
│       └── *.pdf
│
└── appendix/
    └── requirements.tex        # Appendix 3 — повна таблиця вимог ERC
```

---

## Посилання на розділи та одиниці вимірювання

### Як робити посилання на розділи

Всім розділам, підрозділам, підпідрозділам та параграфам **додані label** для легкого посилання.

**КонвенціяLabel's:**
- `\label{sec:назва}` — для розділів (**sections**)
- `\label{subsec:назва}` — для підрозділів (**subsections**)
- `\label{subsubsec:назва}` — для підпідрозділів (**subsubsections**)
- `\label{par:назва}` — для параграфів (**paragraphs**)
- `\label{fig:назва}` — для картинок (**figures**)

**Як посилатися на розділ:**
```latex
% Всередину тексту
Додаткова інформація про цю систему доступна в (\autoref{par:autonomous_navigation_collision_avoidance}).

% На початку речення
\autoref{subsubsec:communication_subsystem} детально описує архітектуру комунікацій.

% На табличні елементи
Параметри наведені в табл. \ref{tab:power_budget}.
```

---

### Як описувати одиниці вимірювання

Для правильного форматування **одиниць вимірювання** використовуємо пакет **`siunitx`**.

**Синтаксис базові приклади:**

```latex
% Простий розмір з одиницею
\qty{60}{\fps}                    % → 60 fps
\qty{10}{\metre}                  % → 10 m
\qty{20}{\fps}                    % → 20 fps

% Діапазон 
\qtyrange{0.3}{20}{\metre}        % → 0.3 m to 20 m
\qtyrange{0.2}{2.5}{\metre}       % → 0.2 m to 2.5 m

% Кут
\ang{360}                         % → 360°
\ang{110}~$\times$~\ang{70}       % → 110° × 70°

% Множинність (resolution, size)
\numproduct{1280 x 720}           % → 1280 × 720
\numproduct{100 x 100}            % → 100 × 100

% Частота
\qty{400}{\hertz}                 % → 400 Hz
\qty{10}{\hertz}                  % → 10 Hz
```

**Кастомні одиниці:**
Якщо потреба в новій одиниці (яка не у стандартному пакеті), додай у `preamble.tex`:
```latex
\DeclareSIUnit{\fps}{fps}    % Приклад: fps (frames per second)
```

---

## Куди що класти — коротко

> **Примітка**: Проектний контент (design descriptions) організований у ієрархічній структурі
> `sections/design/[chapter]/[section]/[subsection]/` 
> Дивись файл [sections/preliminary_design_description.tex](sections/preliminary_design_description.tex)
> для повного переліку розділів і шляхів для включення файлів.

### Я пишу контент для розділу дизайну

1. Знайди відповідний розділ і підрозділ у ієрархії `sections/design/`
   - Наприклад, схема живлення: `sections/design/2_rover_design_description/2_2_electrical_power_subsystem/`
2. Схему намалюй в **EasyEDA**, експортуй як **PDF** → поклади в `figures/cad/power_system.pdf`
3. Текстовий опис пиши у відповідному `.tex` файлі розділу
4. Вставляй схему так:
   ```latex
   \begin{figure}[h]
     \centering
     \includegraphics[width=\linewidth]{figures/cad/power_system}
     \caption{Power distribution system}
     \label{fig:power}
   \end{figure}
   ```

---

### Я маю TikZ діаграму (комунікацій, архітектури, тощо)

1. TikZ-код діаграми поклади окремим файлом: **`figures/tikz/diagram_name.tex`**
2. Вставляй у відповідний розділ дизайну через:
   ```latex
   \begin{figure}[h]
     \centering
     \input{figures/tikz/diagram_name}
     \caption{Diagram description}
     \label{fig:diagram-label}
   \end{figure}
   ```
3. Текстовий опис і біографія діаграми — в відповідному `.tex` файлі у `sections/design/` ієрархії
   - Наприклад, комунікаційної архітектури: `sections/design/2_rover_design_description/2_3_communication_subsystem/`

---

### Я беру участь в інших розділах (RIO, Budget, тощо)

- Вміст розділів **RIO Analysis**, **Project Budget**, **Matrix of Compliance** та **Test Plan** залишаються у відповідних файлах у `sections/`
- Таблиці винесені в окремі файли: `tables/rio_table.tex`, `tables/test_plan.tex`, `tables/compliance_summary.tex`
- Методологія і пояснення — у відповідних розділах

---

### Я додаю фото прототипу

Поклади файл у **`figures/photos/`** і вставляй так:
```latex
\includegraphics[width=0.8\linewidth]{figures/photos/rover_prototype}
```
Розширення (`.jpg`, `.png`) вказувати не потрібно.

---

## Типи діаграм — який інструмент

| Тип діаграми | Інструмент | Куди класти |
|---|---|---|
| Схеми комунікацій, блок-діаграми, PBS | TikZ (в LaTeX) | `figures/tikz/*.tex` |
| Електричні схеми | EasyEDA → export PDF | `figures/cad/*.pdf` |
| Механічні креслення, CAD | SolidWorks / FreeCAD → export PDF | `figures/cad/*.pdf` |
| Фото, скріншоти | PNG / JPG | `figures/photos/` |

> **TikZ діаграми** завжди як окремий `.tex` файл через `\input{}` — не вставляй TikZ-код
> прямо в текст розділу, це ускладнює git diff і злиття змін.

---

## Вимоги ERC 2026

| Параметр | Значення |
|---|---|
| Формат | A4, searchable PDF |
| Максимум сторінок | 30 (25 сторінок на всі розділи + 5 сторінок на appendices, але без cover, TOC) |
| Мова | Англійська |
| Мінімальний шрифт | 10 pt |
| Поля | ≥ 2.54 cm (1 inch) з усіх сторін |
| Назва файлу | `<TeamName>_PreliminaryReport_ERC2026.pdf` |
| Appendices | Опціонально: тільки додаткова інформація, яка не вмістилася в основному тексті (великі креслення, таблиці). **Важливо**: appendices входять у 30-сторінковий ліміт |


---

## Чеклист перед здачею

- [ ] Заповнені метадані в `main.tex` (`\teamname` і т.д.)
- [ ] Всі `[placeholder]` замінені реальним контентом
- [ ] У підсекціях немає зайвих `REQ-*` згадок у тексті
- [ ] Довгі абзаци перероблені в короткі булети там, де це можливо
- [ ] Назви моделей у тексті замінені на спрощені; для невизначених позицій вказано `TBD`
- [ ] Документ компілюється без помилок (`latexmk -pdf main.tex`)
- [ ] Перевірена кількість сторінок (максимум 25)
- [ ] Перевірені поля і розмір шрифту
- [ ] MoC `.xlsx` файл вбудований у PDF через PDF-редактор (наприклад [Foxit](https://www.foxit.com/))
- [ ] Файл названий правильно: `<TeamName>_PreliminaryReport_ERC2026.pdf`
- [ ] Окремо здана Preliminary RF Form (без неї — дискваліфікація)

