# CONTRIBUTING

Короткий гайд для командної роботи без конфліктів у LaTeX.

## Головний принцип

Для `Preliminary Design Description` діє правило:

- **1 PR = 1 subsection file**

Це мінімізує merge-конфлікти і пришвидшує рев'ю.

## Де писати контент

### Design section

Основний файл:

- `sections/preliminary_design_description.tex`

Він тільки підключає підсекції через `\input{...}`.

Контент пишеться в:

- `sections/design/<chapter>/<section>/<subsection>/<file>.tex`

Кожен файл у `sections/design/` відповідає **одному** найнижчому пункту структури (leaf subsection).

Приклад реального шляху:

- `sections/design/2_rover_design_description/2_3_communication_subsystem/2_3_5_link_performance_analysis.tex`

### Test plan

Для тест-плану залишаємо командні файли:

- `teams/<team>/tests.tex`

Ці файли визначають макроси `\XXXtestrows` і збираються у master-таблицю.

## Tiny contract на початку subsection-файлу

На початку кожного файлу `sections/design/*.tex` є міні-контракт:

- `Teams`
- `Requirements`
- `Status`

Не видаляйте цей блок. Оновлюйте поля по мірі прогресу.

## Правила для PR

### Обов'язково

1. Один PR змінює **один** файл у `sections/design/` (допустимі дрібні супутні правки типу опечатки в тому ж subsection).
2. Назва PR:
   - `DES <subsection-id>: short title`
   - приклад: `DES 5.5: link performance analysis`
3. У description PR вкажи:
   - які `REQ-*` закриті
   - які крос-посилання додані/оновлені
   - що ще лишилось (якщо є)
   - додай скріншот скопільованої секції, щоб було зручно рев'ювити твою секцію
4. Новий PR створюємо як **Draft**. Після перевірки тімлід переводить його з Draft у Ready for review.

#### Приклад description / коментаря до PR (Markdown)

Скопіюй шаблон нижче в опис PR або в перший коментар:

```md
## What Was Done
- Updated subsection: `sections/design/<chapter>/<section>/<subsection>/<file>.tex`
- Added/updated: <short summary of changes>

## Closed Requirements
- REQ-XXX-001
- REQ-XXX-002

## Cross-references
- Added/updated references to: `Section X.Y`, `Table Z`, `Figure N`

## Remaining Work
- [ ] <item 1>
- [ ] <item 2>

## Section Screenshot
![Compiled subsection screenshot](<insert_link_or_drag_and_drop_image_here>)
```

### Стандарт назви гілки

- `des/<subsection-id>-<short-title>`
- приклад: `des/5.5-link-performance-analysis`

## Рекомендований git workflow

Це нормальний workflow для цього репозиторію: спочатку вносиш усі зміни локально, потім створюєш нову гілку, комітиш **тільки свій файл**, пушиш і повертаєшся в `main`.

```bash
# 0) Оновити main
git checkout main
git pull

# 1) Після того як зробив зміни у своєму subsection-файлі
git checkout -b des/<subsection-id>-<short-title>

# 2) Додати лише змінений файл
git add sections/design/<chapter>/<section>/<subsection>/<file>.tex

# 3) Коміт
git commit -m "DES <subsection-id>: short title"

# 4) Перший пуш нової гілки з прив'язкою remote
git push -u origin des/<subsection-id>_<short-title>

# 5) Створити PR на GitHub і одразу позначити Draft

# 6) Повернутись у main
git checkout main
```

Якщо після змін потрібен ще один PR, створюй **нову гілку** від актуального `main` і повторюй кроки.

## Якщо прийшли правки в Draft PR

Якщо в твоєму Draft PR попросили зміни, **не створюй новий PR**.
Працюй у тій самій гілці, яка прив'язана до цього PR:

```bash
# 1) Перейти на гілку цього PR
git checkout des/<subsection-id>-<short-title>

# 2) Внести правки у свій файл

# 3) Додати змінений файл
git add sections/design/<chapter>/<section>/<subsection>/<file>.tex

# 4) Новий коміт з правками
git commit -m "DES <subsection-id>: address review comments"

# 5) Пуш у ту саму гілку
git push
```

Після `git push` цей самий PR на GitHub оновлюється **автоматично** (нові коміти додаються в нього).

Коли всі правки враховані, тімлід переводить PR з Draft у Ready for review.

### Не робимо

- Не змішуємо кілька subsection в одному PR.
- Не переносимо великий контент між чужими subsection-файлами.
- Не редагуємо `sections/preliminary_design_description.tex`, якщо просто додаємо текст у subsection.

## Мінімальний чекліст перед пушем

1. Заповнені `Teams`, `Requirements` і `Status` у tiny contract.
2. Немає заглушок виду `[Add content for this subsection.]` у твоєму файлі.
3. Є хоча б одна явна згадка `REQ-*` у тексті підсекції (або таблиці/рисунку).
4. Документ компілюється локально.

## Компіляція

Компілюй з кореня репозиторію:

```bash
latexmk -pdf main.tex
latexmk -c
```

## Де зберігати фігури

- Командні фігури: `teams/<team>/figures/`
- Загальні фігури: `figures/`

Вставка прикладу:

```latex
\begin{figure}[H]
    \centering
    \includegraphics[width=0.9\linewidth]{teams/arm/figures/arm_cad}
    \caption{Robotic arm overview}
    \label{fig:arm_cad}
\end{figure}
```

## Часті помилки

| Помилка | Як правильно |
|---|---|
| Один PR змінює 3-4 subsection файли | Тримай PR у межах одного `sections/design/<file>.tex` |
| Залишені заглушки `[Add content ...]` | Замінити на реальний текст перед merge |
| Пишеш контент у `sections/preliminary_design_description.tex` | Писати в конкретний `sections/design/*.tex` |

## Питання

Пишіть у командний чат або відкривайте issue в репозиторії.
