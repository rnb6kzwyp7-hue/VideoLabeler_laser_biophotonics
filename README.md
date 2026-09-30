# VideoLabeler

> Этот публичный репозиторий содержит руководство и готовые установщики. Исходный код находится в отдельном закрытом репозитории. Разделы о разработке и сборке предназначены для участников проекта с доступом к исходному коду.

VideoLabeler — настольное приложение для синхронной работы с видео, экспериментальными данными и покадровой разметкой.

Приложение предназначено для задач, в которых видеозапись эксперимента необходимо связать с временным рядом или спектральными измерениями, после чего размечать отдельные кадры, анализировать экспериментальные данные и сохранять результат для дальнейшей обработки.

В текущей версии поддерживаются два профиля работы:

- **Tissue Sensor** — работа с временными сигналами;
- **CRC** — работа со спектральными измерениями.

Интерфейс приложения — на английском языке. Документация в этом README — на русском.

---

## Основные возможности

VideoLabeler объединяет в одном интерфейсе:

- просмотр видео;
- покадровую навигацию;
- отображение экспериментальных данных;
- синхронизацию времени видео и экспериментального сигнала;
- двунаправленную навигацию между видео и сигналом;
- ручную покадровую разметку;
- настраиваемые labels;
- горячие клавиши;
- таблицу созданных аннотаций;
- экспорт и загрузку annotations CSV;
- сохранение и повторное открытие processed CSV;
- сохранение аннотаций вне диапазона processed data через sidecar CSV;
- встроенные и пользовательские метрики;
- временные графики Tissue Sensor;
- RAW- и OD-спектры CRC;
- временные ряды CRC-метрик.

Поддерживаемые видеоформаты:

- `.mp4`
- `.avi`
- `.mov`
- `.mkv`

---

# Установка готового приложения

Скачайте установщик для своей системы из [выпуска v97aaebf](https://github.com/rnb6kzwyp7-hue/VideoLabeler_laser_biophotonics/releases/tag/v97aaebf):

| Система | Файл |
| --- | --- |
| macOS, Apple silicon | [DMG](https://github.com/rnb6kzwyp7-hue/VideoLabeler_laser_biophotonics/releases/download/v97aaebf/VideoLabeler-macOS-arm64-97aaebf.dmg) |
| Windows 10, x64 | [EXE](https://github.com/rnb6kzwyp7-hue/VideoLabeler_laser_biophotonics/releases/download/v97aaebf/VideoLabeler-Windows-x64-97aaebf-Setup.exe) |

Windows 11 пока отдельно не проверялась. На Mac с процессором Intel сборка не проверялась.

Для готовых сборок Python и исходный код пользователю не нужны. При запуске приложения выберите профиль **Tissue Sensor** или **CRC**.

## macOS (Apple silicon)

1. Откройте `VideoLabeler-macOS-arm64-97aaebf.dmg`.
2. В открывшемся окне перетащите `VideoLabeler.app` на ярлык **Applications** («Программы»).
3. Извлеките образ и запускайте VideoLabeler из «Программ», Launchpad или поиска Spotlight.

Установка не создаёт папку или ярлык на рабочем столе. Если ярлык нужен, пользователь может добавить его сам. Сборка `arm64` предназначена для Mac с процессором Apple; сборка для Intel Mac пока не подготовлена.

DMG проверен локально после копирования приложения из образа. Он пока не подписан Developer ID и не нотариализован Apple: на другом Mac система может ограничить его запуск.

## Windows (x64)

Скачайте `VideoLabeler-Windows-x64-97aaebf-Setup.exe` по ссылке выше, запустите и пройдите шаги установки. Он устанавливает приложение для текущего пользователя без прав администратора, создаёт ярлык **VideoLabeler** прямо на рабочем столе и добавляет его в меню «Пуск». Открывайте приложение через любой из этих ярлыков.

## Пользовательские настройки

В готовых сборках `labels.json`, `shortcuts.json` и `metrics.json` хранятся отдельно от программы:

- macOS: `~/Library/Application Support/VideoLabeler/`;
- Windows: `%APPDATA%\VideoLabeler\`.

При удалении приложения пользовательские данные не удаляются автоматически. Экспортированные CSV сохраняются в местах, выбранных пользователем.

---

# Запуск и выбор профиля

Для запуска из исходного кода выполните в корневой папке проекта:

```bash
python main.py
```

При запуске необходимо выбрать профиль:

- **Tissue Sensor**
- **CRC**

Профиль определяет формат экспериментальных данных, способ загрузки, доступные измерительные каналы, вид визуализации и тип пользовательских метрик.

---

# Главное окно

Главное окно объединяет несколько функциональных областей:

- видео;
- labels;
- annotations;
- экспериментальную визуализацию;
- меню File;
- меню Settings;
- элементы синхронизации и навигации.

Компоновка ориентирована на работу с видео как с основной временной шкалой, а экспериментальные данные используются как синхронизированный источник дополнительной информации.

## Меню File

Через меню File выполняются основные операции с данными. В зависимости от выбранного профиля доступны действия для:

- открытия видео;
- загрузки экспериментальных данных;
- сохранения processed CSV;
- открытия processed CSV;
- загрузки annotations CSV;
- экспорта annotations CSV;
- выхода из приложения.

## Меню Settings

Через Settings доступны:

- **Labels** — редактирование labels;
- **Metrics** — управление пользовательскими метриками;
- **Synchronization** — настройка связи между видео и экспериментальным сигналом.

---

# Видео и навигация

Видео является основной временной шкалой при разметке.

После загрузки видео приложение позволяет:

- воспроизводить видео;
- останавливать воспроизведение;
- переходить между кадрами;
- менять текущую позицию;
- использовать текущий кадр для создания аннотации;
- синхронно перемещать курсор экспериментальных данных.

Для воспроизведения используется Qt Multimedia. `QMediaPlayer` используется как основной источник текущего времени во время проигрывания.

---

# Labels

Label — это класс разметки, который можно назначить конкретному кадру видео.

Каждый label имеет:

- `id`;
- `name`;
- `shortcut`.

Labels сохраняются в:

```text
labels.json
```

Редактор открывается через:

```text
Settings → Labels
```

В редакторе можно:

- добавлять labels;
- удалять labels;
- изменять имя;
- изменять `id`;
- изменять shortcut.

`id` хранится как строка. При изменении `id` существующего label текущие аннотации мигрируют на новый `id`.

---

# Аннотации

Аннотация связывает конкретный кадр видео с label:

```text
frame → label_id
```

Аннотации можно:

- создавать;
- изменять;
- удалять;
- загружать из CSV;
- экспортировать в CSV.

Формат annotations CSV:

```text
frame,label_id
```

Пример:

```csv
frame,label_id
120,1
245,2
380,1
```

При загрузке существующих annotations приложение проверяет структуру файла и используемые `label_id`.

---

# Синхронизация видео и сигнала

VideoLabeler поддерживает двунаправленную синхронизацию между временем экспериментального сигнала и временем видео.

Используется линейная модель:

```text
video_time = a * signal_time + b
```

где:

- `signal_time` — время экспериментального сигнала;
- `video_time` — соответствующее время видео;
- `a` — масштаб времени;
- `b` — временное смещение.

Синхронизацию можно задать:

- по одной точке;
- по двум точкам;
- с ручным масштабом.

При использовании двух точек приложение рассчитывает смещение и масштаб времени.

После настройки синхронизации:

- изменение позиции видео перемещает курсор экспериментальных данных;
- клик или выбор времени на графике может переместить видео;
- CRC Spectrum и CRC Metric используют общий текущий signal time.

---

# Профиль Tissue Sensor

Tissue Sensor предназначен для работы с временными экспериментальными измерениями.

## Загрузка исходных данных

Loader работает с файлами Tissue Sensor вида:

```text
00*.CSV
```

Основные особенности загрузки:

- разделитель `;`;
- первые две строки пропускаются;
- поддерживается десятичная запятая;
- `Timestamp` используется для построения временной шкалы;
- выполняется коррекция переполнения timestamp;
- `Result_3` переименовывается в `Result`.

Время рассчитывается как:

```text
seconds = Timestamp / 1e4
time_ms = seconds * 1000
```

## Raw-каналы

На raw-графике могут использоваться:

- `PD1`
- `PD2`
- `PD3`
- `PD4`
- `Pulse`
- `Out`
- `Out_2`
- `Shutdown`

Также может присутствовать `Result`.

## График Tissue Sensor

Основной график показывает выбранный временной ряд.

Можно:

- выбрать измерительный канал;
- выбрать пользовательскую метрику;
- задать Start;
- задать End;
- нажать Apply;
- вернуть Full Range.

Текущий момент времени отмечается вертикальным курсором.

## Основные processed-каналы

В профиле используются:

- `PD1`
- `PD2`
- `PD3`
- `PD4`
- `Pulse`
- `475/550`
- `550/575`
- `475/575`

---

# Processed CSV

Для Tissue Sensor можно сформировать processed CSV, объединяющий экспериментальные данные, синхронизацию, номера кадров и разметку.

Processed CSV имеет фиксированную схему из 16 колонок:

```text
PD1
PD2
PD3
PD4
Pulse
Result
C1
C2
seconds
seconds_video
frame
date
true_label
475/550
550/575
475/575
```

Количество строк соответствует количеству исходных экспериментальных измерений.

## Основные поля

- `seconds` — время экспериментального сигнала;
- `seconds_video` — соответствующее время видео;
- `frame` — соответствующий кадр;
- `true_label` — label для соответствующего кадра;
- `475/550`, `550/575`, `475/575` — рассчитываемые отношения каналов.

---

# Аннотации вне диапазона processed data

Processed CSV содержит только строки, соответствующие экспериментальным измерениям. Поэтому часть размеченных кадров видео может не иметь строки в processed CSV.

Чтобы такие annotations не терялись, рядом сохраняется sidecar-файл.

Если основной файл называется:

```text
experiment.csv
```

то sidecar называется:

```text
experiment.annotations.csv
```

Формат sidecar:

```text
frame,label_id
```

При повторном открытии processed CSV:

- если sidecar существует, annotations загружаются из него;
- если sidecar отсутствует, приложение пытается восстановить annotations из `frame` и `true_label`.

Так сохраняется обратная совместимость со старыми processed CSV.

---

# Восстановление синхронизации

Processed CSV хранит:

- `seconds`;
- `seconds_video`.

При повторном открытии приложение восстанавливает линейную синхронизацию по первой и последней валидным точкам.

---

# Профиль CRC

CRC предназначен для работы со спектральными измерениями.

Поддерживаемые исходные форматы:

- `.hdf`
- `.zip`

При загрузке CRC необходимо выбрать:

1. папку экспериментальных данных;
2. Dark reference;
3. Light reference.

Reference-файлы поддерживаются в форматах:

- `.csv`
- `.zip`

Все записи сортируются по времени и формируют общую временную последовательность.

---

# CRC HDF

HDF loader использует, в частности:

```text
data/axis0
data/block0_values
timestamp/values
```

Timestamp интерпретируется как время в наносекундах.

Также могут использоваться settings, metadata и reference-related данные.

---

# CRC ZIP

ZIP loader работает с parquet-данными внутри архива.

Используются данные типа:

- data;
- timestamp;
- comment;
- settings.

Для чтения parquet используется PyArrow через pandas.

---

# Reference spectra

Dark и Light reference могут быть загружены из CSV или ZIP.

Для reference spectrum применяется вычитание среднего значения в диапазоне около 300 нм через логику `subtract_300`.

---

# CRC Spectrum Mode

В режиме **Spectrum** одновременно отображаются два графика одинакового размера:

- RAW spectrum;
- OD spectrum.

Они относятся к одному и тому же текущему spectral record.

## RAW spectrum

RAW показывает исходную интенсивность:

```text
X → wavelength
Y → intensity
```

## OD spectrum

OD рассчитывается на основе текущего измерения и выбранных Dark/Light references.

## Навигация по спектрам

При изменении текущего spectrum:

- обновляется RAW;
- обновляется OD;
- обновляется signal time;
- синхронно перемещается видео.

CRC View также содержит выбор Section для навигации по спектральным данным.

## Wavelength range

Для RAW и OD используется общий диапазон длин волн.

Можно задать:

- Start;
- End;
- Apply;
- Full Range.

Диапазон сохраняется при переключении между records и режимами. Изменение диапазона является только настройкой отображения и не запускает пересчёт метрик.

---

# CRC Metric Mode

В режиме **Metric** отображается временной ряд выбранной CRC-метрики.

Можно:

- выбрать метрику;
- задать Start;
- задать End;
- нажать Apply;
- вернуть Full Range;
- перемещать текущий signal time;
- синхронно перемещать видео.

Изменение viewport не перемещает видео само по себе. Текущий cursor может находиться вне видимой области выбранного временного диапазона.

---

# Встроенная CRC-метрика

В профиль CRC входит встроенная метрика:

```text
OD_coef71
```

Она рассчитывается для каждого спектрального измерения.

Используемая формула:

```python
def new_calc_dye71(od):
    _500 = od.loc[495:505].mean()
    _570 = od.loc[560:580].mean()
    _600 = od.loc[590:610].mean()
    return (_600 - _500) / (_570 - _500)
```

---

# Пользовательские метрики

VideoLabeler поддерживает пользовательские Python-метрики.

Редактор открывается через:

```text
Settings → Metrics
```

Метрики являются profile-specific.

## Tissue Sensor metric

Тип:

```text
table
```

Функция получает весь DataFrame:

```python
def calculate(df):
    ...
```

Она должна вернуть числовой `pandas.Series` той же длины и с тем же индексом, что и исходный DataFrame.

Пример:

```python
def calculate(df):
    return df["PD1"] / df["PD2"]
```

## CRC spectrum metric

Тип:

```text
spectrum
```

Функция вызывается для одной строки CRC DataFrame:

```python
def calculate(row):
    ...
```

Она должна вернуть одно числовое значение.

Пример:

```python
def calculate(row):
    return row["OD"].mean()
```

## Доступные объекты

В пользовательском коде доступны:

- `pd` — pandas;
- `np` — NumPy.

## Проверка кода

Metrics Settings позволяет протестировать код до сохранения.

Ошибки синтаксиса, несовпадение длины результата, неправильный индекс или неподходящий тип результата показываются пользователю.

## Безопасность

> **Важно:** custom metrics являются trusted Python code и не выполняются в sandbox. Используйте только код, которому доверяете.

## Хранение

Пользовательские метрики сохраняются локально в:

```text
metrics.json
```

Built-in metrics в этот файл не записываются. `metrics.json` игнорируется Git.

---

# Типичный workflow — Tissue Sensor

1. Запустить VideoLabeler.
2. Выбрать **Tissue Sensor**.
3. Открыть видео.
4. Загрузить папку экспериментальных данных.
5. Настроить Synchronization.
6. Проверить соответствие видео и сигнала.
7. Настроить Labels.
8. Выполнить покадровую разметку.
9. При необходимости создать custom metrics.
10. Выбрать нужный канал или метрику на графике.
11. Сохранить processed CSV.
12. При необходимости отдельно экспортировать annotations CSV.
13. Позже открыть processed CSV и продолжить работу.

---

# Типичный workflow — CRC

1. Запустить VideoLabeler.
2. Выбрать **CRC**.
3. Открыть видео.
4. Выбрать папку CRC-измерений.
5. Выбрать Dark reference.
6. Выбрать Light reference.
7. Настроить Synchronization.
8. Просматривать RAW и OD spectra.
9. Перемещаться между spectrum records.
10. При необходимости ограничить wavelength range.
11. Переключиться в Metric mode.
12. Выбрать `OD_coef71` или custom metric.
13. При необходимости ограничить time range.
14. Выполнять покадровую разметку видео.

---

# Файлы пользователя

Указанные ниже файлы в готовой сборке находятся в каталоге пользовательских настроек, описанном в разделе установки. При запуске из исходного кода они находятся в корне проекта.

## `labels.json`

Содержит список labels и может изменяться через интерфейс.

## `shortcuts.json`

Содержит настройки shortcuts.

## `metrics.json`

Содержит пользовательские custom metrics. Создаётся и изменяется приложением и не хранится в Git.

## `sample.mp4`

Локальный тестовый видеофайл для `tests/test_video_player.py`.

Файл не входит в репозиторий и добавлен в `.gitignore`. Если он отсутствует, соответствующий video test пропускается.

---

# Установка для разработки

Текущая конфигурация проверена с Python 3.13 на macOS и Python 3.12 на Windows.

Создание virtual environment:

```bash
python -m venv .venv
```

macOS / Linux:

```bash
source .venv/bin/activate
```

Windows:

```powershell
.venv\Scripts\activate
```

---

# Runtime dependencies

Установка:

```bash
python -m pip install -r requirements.txt
```

Текущий runtime-набор:

```text
PySide6==6.11.1
opencv-python==4.12.0.88
numpy==2.2.6
pandas==2.3.2
pyqtgraph==0.14.0
h5py==3.16.0
pyarrow==25.0.1
```

---

# Development dependencies

Установка:

```bash
python -m pip install -r requirements-dev.txt
```

`requirements-dev.txt` также устанавливает runtime dependencies.

Основные development dependencies:

```text
pytest==9.1.1
pyinstaller==6.22.3
```

---

# Проверка зависимостей

```bash
python -m pip check
```

Перед release рекомендуется повторять эту проверку в новом чистом virtual environment.

---

# Запуск тестов

Полный suite:

```bash
pytest -q
```

На контрольной точке `97aaebf` полный прогон на macOS завершился результатом `104 passed`. Некоторые тесты с локальным видео могут пропускаться, если `sample.mp4` отсутствует.

Основные области покрытия:

- AnnotationManager;
- LabelManager;
- SynchronizationModel;
- ProcessedDataBuilder;
- ProcessedDataLoader;
- ExportManager;
- MetricManager;
- MetricExecutor;
- VideoPlayer.

## VideoPlayer test

`tests/test_video_player.py` может использовать локальный файл:

```text
sample.mp4
```

Если файл отсутствует, тест пропускается.

---

# Структура проекта

```text
VideoLabeler/
├── main.py
├── VideoLabeler.spec
├── VideoLabeler-Windows.iss
├── requirements.txt
├── requirements-dev.txt
├── resources/
│   ├── default_labels.json
│   ├── default_shortcuts.json
│   └── icons/
├── src/
│   ├── app.py
│   ├── controller.py
│   ├── main_window.py
│   ├── settings.py
│   ├── task_profiles.py
│   ├── theme.py
│   ├── models/
│   │   ├── label.py
│   │   ├── metric.py
│   │   └── synchronization.py
│   ├── services/
│   │   ├── annotation_manager.py
│   │   ├── crc_experimental_data.py
│   │   ├── experimental_data.py
│   │   ├── export_manager.py
│   │   ├── label_manager.py
│   │   ├── metric_executor.py
│   │   ├── metric_manager.py
│   │   ├── processed_data.py
│   │   ├── processed_data_loader.py
│   │   └── video_player.py
│   └── widgets/
│       ├── annotations_table.py
│       ├── crc_view.py
│       ├── experimental_plot.py
│       ├── labels_panel.py
│       ├── labels_settings_dialog.py
│       ├── metrics_settings_dialog.py
│       ├── spectrum_plot.py
│       ├── synchronization_dialog.py
│       └── video_widget.py
└── tests/
```

Локальные `labels.json`, `shortcuts.json`, `metrics.json`, `sample.mp4`, `.venv/`, `build/` и `dist/` не входят в исходный release-архив.

---

# Основные компоненты

## `main.py`

Точка входа приложения. Создаёт `VideoLabelerApp` и запускает его.

## `src/app.py`

Основной orchestration-слой. Связывает MainWindow, Controller, сервисы, task profile, synchronization, metrics и save/load workflows.

## `src/controller.py`

Управляет видео, текущим кадром, annotations и взаимодействием между UI и данными.

## `src/main_window.py`

Содержит главное окно, меню, основные панели, video UI и profile-specific экспериментальные представления.

## `src/task_profiles.py`

Определяет поддерживаемые профили Tissue Sensor и CRC.

## `src/models/`

Содержит модели Label, MetricDefinition и SynchronizationModel.

## `src/services/`

Содержит основную data/business logic: annotations, labels, loaders, processed data, metrics, export и video frame reading.

## `src/widgets/`

Содержит UI-компоненты: video widget, experimental plot, CRC view, spectrum plot, labels panel, annotations table и dialogs.

---

# Производительность

Приложение рассчитано на работу с крупными экспериментальными наборами данных.

В ходе оптимизации были реализованы:

- lazy synchronization time map;
- отсутствие полного списка времени для каждого кадра;
- использование компактных pandas dtypes там, где это безопасно;
- category dtype для подходящих колонок;
- уменьшение количества полных DataFrame copies;
- оптимизация восстановления synchronization;
- shallow copy при обновлении `true_label`;
- lazy custom metric calculation;
- хранение только текущего результата custom metric;
- повторная загрузка эксперимента без накопления старых объектов.

Эти оптимизации особенно важны для длительных Tissue Sensor экспериментов и CRC-наборов с большим количеством спектров.

---

# Release-проверка

Перед release рекомендуется выполнить:

```bash
python -m pip check
pytest -q
```

Также рекомендуется:

1. создать новое чистое virtual environment;
2. установить `requirements-dev.txt`;
3. запустить полный test suite;
4. запустить `python main.py`;
5. проверить Tissue Sensor;
6. проверить CRC;
7. проверить видео;
8. проверить custom metrics;
9. проверить synchronization;
10. проверить save/load processed data.

---

# Сборка приложений

Сборку PyInstaller выполняют на той же ОС, для которой нужен результат:

```bash
python -m PyInstaller --noconfirm --clean VideoLabeler.spec
```

На macOS команда создаёт `dist/VideoLabeler.app`, на Windows — папку `dist\VideoLabeler` с `VideoLabeler.exe` и зависимостями. Сборка на macOS arm64 и папочная сборка на Windows x64 проверены; установленная из `.dmg` копия также запускалась вне `dist`.

Для создания Windows-установщика после сборки папки установите Inno Setup и скомпилируйте `VideoLabeler-Windows.iss` на Windows. Результат появится в `dist\installer`. Установщик копирует весь bundle, создаёт ярлык на рабочем столе и запись в меню «Пуск».

Windows-установщик собран и проверен на Windows 10. DMG проверен на Mac с Apple silicon. Перед широким распространением нужно дополнительно проверить Windows 11 и другой Mac, а также решить вопрос цифровой подписи установщиков.

---

# Текущее состояние

Основная функциональность VideoLabeler реализована.

Поддерживаются:

- Tissue Sensor;
- CRC;
- video playback;
- frame annotations;
- configurable labels;
- keyboard shortcuts;
- bidirectional synchronization;
- processed data;
- processed annotations sidecar;
- RAW CRC spectra;
- OD CRC spectra;
- CRC metric time series;
- built-in metric `OD_coef71`;
- custom table metrics;
- custom spectrum metrics;
- persistence custom metrics.

Готовые установщики для Windows x64 и macOS arm64 опубликованы в выпуске `v97aaebf`. Сборка Windows прошла установку и запуск на Windows 10; DMG проверен на Mac с Apple silicon. Код сборок соответствует контрольной точке `97aaebf`; последующие изменения документации и сценария установщика внесены отдельно.
