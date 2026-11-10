# Annual Project Plan: Benchmark and Meta-Model for Preliminary Assessment of Textual Data Utility in Time-Series Forecasting

## 1. Working Topic

**Preliminary assessment of the applicability of textual data to time-series forecasting tasks before training a multimodal model.**

The working goal of the project is to investigate whether, before training a full multimodal `TS + Text` model, it is possible to estimate whether the available text contains an additional predictive signal for a specific time-series forecasting task.

The project consists of two interconnected parts:

1. **Benchmark environment** — a set of tasks, a protocol for creating a unified evaluation target, data splitting rules, leakage control, metrics, and comparison criteria.
2. **Meta-model / utility estimator** — a model that estimates the expected utility of text before training a multimodal forecasting model, based on the characteristics of the time series, text, and forecasting task.

The first-year project should be self-contained: there should be a minimally viable benchmark, a set of baseline methods, and an initial model for estimating text utility. At the same time, the architectural component is intentionally left open for extension, so that in the second year the utility estimation method can be improved and the work can move toward more advanced architectures for integrating text and time series.

---

# 2. Research Formulation

## 2.1. Problem

In a typical multimodal forecasting pipeline, the usefulness of text is determined post hoc:

1. a time series is selected;
2. a text source is selected;
3. a time-series-only model is built;
4. a `TS + Text` model is built;
5. the models are trained;
6. the usefulness of text is determined from the change in forecasting quality.

This approach does not answer the following question:

> **Can we determine, before training a multimodal model, whether a given text source is worth using for a particular time series and forecasting task?**

The project considers an earlier stage of multimodal forecasting:

> **Can we robustly determine, before model training, whether there is an additional predictive relationship between a specific text source and a time series?**

The focus is not merely on topical similarity between the text and the time series, but on whether the text contains information related to the future dynamics of the series beyond what is already contained in its own history.

The main research task is to identify indicators of such a relationship and to test whether the applicability of textual data to a forecasting task can be estimated in advance from the properties of the time series, the text, their temporal interaction, and the forecasting horizon.

---

## 2.2. Main Hypothesis

**H1. The usefulness of text for a forecasting task is neither completely random nor specific to a single model. It is partially determined by observable properties of the time series, the text stream, the task parameters, and their interaction.**

Implication:

> If such structure exists, it can be modeled and used for preliminary estimation of text utility on new tasks.

---

## 2.3. Main Research Question

> **Can the expected additional predictive value of text for a new time-series forecasting task be estimated using only information available before training a multimodal model?**

---

## 2.4. Additional Research Questions

1. Which properties of a time series are associated with the subsequent usefulness of text?
2. Which properties of the text stream are associated with the subsequent usefulness of text?
3. How important are the temporal and semantic relationships between text and the time series?
4. How does text utility change depending on forecasting horizon and time-series frequency?
5. To what extent does text informativeness transfer across different time series and datasets?
6. To what extent does the realized gain depend on the architecture of the forecasting model?
7. Can we distinguish between:
   - the absence of useful signal in the text;
   - the presence of useful signal that a specific model failed to exploit?
8. Can a preliminary estimate of text utility be used to reduce the cost of developing and training multimodal models?

---

# 3. What Counts as the Benchmark

The benchmark in this project is **not a single model**, but a combination of:

- a set of tasks: `Time Series + Text + Specific Forecasting Task`;
- rules for constructing a reference estimate of text utility;
- a unified temporal evaluation protocol;
- data leakage control;
- rules for splitting data into training, validation, and test sets;
- evaluation metrics;
- simple baseline methods for comparison;
- tests of transferability to new data and tasks;
- a protocol for evaluating new models that predict text utility.

The benchmark should make it possible to compare different methods that solve the same task:

> **For a new forecasting task, predict text utility before training a full multimodal model.**

---

# 4. What Counts as the Meta-Model

The meta-model is **a method for solving the task defined by the benchmark**.

Its input contains information available before full training:

- the time series or its representation;
- the text stream or its representation;
- task parameters;
- if necessary, general characteristics of the intended architecture.

Its output may include:

- an estimate of expected text utility, for example probabilistic or another suitable form;
- the probability that text will improve forecasting quality;
- an uncertainty estimate.

The architecture should emerge from the first experiments and data analysis. We do not want to fix it in advance; instead, architectural choices should be guided by what is observed during the analysis of the problem itself.

---

# Checkpoint 1. Start and Planning

## Goal

Define the research formulation and the boundaries of the first-year project.

## Tasks

- Clarify what is meant by text applicability and text utility.
- Select an initial domain and available data for the first hypothesis test.
- Define how the reference estimate of text utility will be formed.
- Identify the main risks: data leakage, dependence on a specific model, and insufficient transferability.
- Create the repository and describe the project formulation and plan.

## Result

A fixed problem formulation, an initial dataset, and an experimental plan.

---

# Checkpoint 2. Exploratory Data Analysis and Viability Check

## Goal

Determine whether it makes sense to scale the idea into a full benchmark.

The following question must be answered:

> **Is there an observable and predictable signal linking the properties of a “time series + text” pair to the subsequent usefulness of the text?**

The study should first be conducted within one well-understood domain. If no stable structure can be found even in this restricted setting, scaling the idea to multiple domains would not be justified.

## Tasks

- Study the main properties of the time series and text stream.
- Analyze the temporal relationship between text and subsequent time-series dynamics.
- Obtain an initial reference estimate of text utility on a limited set of tasks.
- Test whether simple data characteristics can predict this utility better than a naive approach.
- Test the stability of the result on unseen time series or time periods.

## Result

A conclusion about the viability of the main hypothesis and a decision on whether to scale, refine, or narrow the formulation.

---

# Checkpoint 3. First ML Models and Minimal Benchmark

## Goal

Move from the micro-study to the first reproducible version of the task.

## Tasks

- Formalize the reference estimate of text utility.
- Define the structure of one benchmark task: a time series, associated text, and forecasting setup.
- Define correct data splitting without temporal or content leakage.
- Implement simple baseline methods for preliminary estimation of text utility.
- Define evaluation metrics and assess the contribution of different information groups: the time series, the text, and their interaction.

## Result

**Benchmark v1**: a pilot set of tasks, reference values, evaluation rules, metrics, and first baseline results.

---

# Checkpoint 4. ML Service

## Goal

Implement a demonstration interface for the current evaluation method.

## Tasks

Create a service that accepts:

- a time series;
- textual data;
- forecasting task parameters;

and returns a preliminary estimate of text applicability.

The service should demonstrate the central idea of the project:

> **The estimate is produced before training a full “time series + text” model.**

## Result

A working API and a reproducible process for obtaining the estimate.

---

# Mid-Year Project Defense

By the end of the first half of the research year, the following should be demonstrated:

- the research problem formulation;
- results of the hypothesis viability test;
- the first version of the benchmark;
- the first methods for estimating text applicability;
- the main limitations identified;
- the scaling plan for the second semester.

The main question for the mid-year defense is:

> **Is there sufficient empirical evidence to consider preliminary text applicability estimation a feasible task?**

---

# Checkpoint 5. Benchmark Expansion and Model Improvement

## Goal

Move from a pilot formulation to a more general version of the task, provided that the previous stages confirm its feasibility.

## Tasks

- Increase the diversity of time series, texts, horizons, and frequencies.
- If the signal is sufficiently strong, add new datasets or a second domain.
- Test the stability of text utility estimation across different tasks.
- Investigate how strongly the result depends on the forecasting architecture being used.
- Move from simple features to richer data representations if justified by the first-semester results.
- Test transferability to unseen time series and datasets.

## Result

**Benchmark v2** and an improved version of the preliminary utility estimation model.

---

# Checkpoint 6. Neural Meta-Model

## Goal

Develop a neural method for estimating text applicability.

## Formulation

The model should jointly account for:

- the structure of the time series;
- the content of textual data;
- forecasting task parameters;
- the interaction between text and the time series.

The specific architecture is not fixed in advance and should be selected based on the results of the previous stages.

The main question of this stage is:

> **Does a learned joint representation of text and time series provide a more stable and transferable estimate than simpler methods?**

## Result

The first version of a neural deep-learning meta-model, comparison with baseline approaches, and error analysis.

---

# Checkpoint 7. Final Improvements

## Goal

Produce a complete and reproducible research result.

## Tasks

- Set up experiment reproducibility and logging.
- Fix the final data splits and evaluation protocol.
- Test robustness to noisy, irrelevant, and outdated text.
- Conduct the final transferability evaluation.
- Analyze dependence on domain and forecasting architecture.
- Prepare the final version of the benchmark, meta-model, and research report.

## Result

- **Benchmark v3**;
- the first complete version of the meta-model;
- a reproducible set of experiments;
- the service;
- material for the final defense and a potential research paper.

---

# Final Research Question

> **Can we determine in advance when text is potentially useful for a time-series forecasting task?**

Further work may focus on improving the meta-model, conducting a deeper analysis of the dependence of text utility on forecasting architecture, or developing models that use the obtained utility estimate directly when integrating text into time-series forecasting.




# Годовой план проекта: benchmark и meta-model для предварительной оценки полезности текстовых данных в задачах прогнозирования временных рядов

## 1. Рабочая тема

**Предварительная оценка применимости текстовых данных к задачам прогнозирования временных рядов до обучения мультимодальной модели.**

Рабочая цель проекта- исследовать, можно ли заранее, до обучения полноценной мультимодальной модели TS + Текст, оценить, содержит ли доступный текст дополнительный предсказательный сигнал для конкретной задачи прогнозирования временного ряда

Проект состоит из двух взаимосвязанных частей:

1. **Benchmark-среда**-набор задач, протокол формирования единого источника оценивания  , правила разделения данных, контроль утечек, метрики и критерии сравнения
2. **Meta-model / utility estimator**- модель, которая по характеристикам временного ряда, текста и постановки задачи прогнозирования оценивает ожидаемую полезность текста до обучения мультимодальной forecasting-модели

В первый год задача должна быть завершённой сама по себе(так как он нашей команды, да и от всех команд этого требуют должен существовать минимально работоспособный бэнчмарк, набор baseline методов и собственная модель первичной оценки полезности текста. При этом архитектурная часть намеренно остаётся расширяемой, чтобы во второй год можно было улучшать способ оценки полезности и переходить к более сложной архитектуре интеграции текста и временных рядов

---

# 2. Исследовательская постановка

## 2.1. Проблема

В типичном мультимодальном пайплайне прогнозирования полезность текста определяется постфактум:

1. выбирается временной ряд
2. выбирается текстовый источник
3. строится чистая модель временных рядов 
4. строится TS+текст модель
5. модели обучаются
6. по изменению качества определяется, помог текст или нет

Такой подход не отвечает на вопрос:

> **Можно ли ещё до обучения мультимодальной модели понять, имеет ли смысл использовать данный текстовый источник для данного временного ряда и конкретной задачи прогнозирования?**

Проект рассматривает более ранний этап мультимодального прогнозирования **можно ли до обучения модели устойчиво определить, существует ли между конкретным текстовым источником и временным рядом дополнительная предсказательная связь**

Речь идёт не просто о тематической близости текста и ряда, а о наличии в тексте информации, связанной с будущей динамикой ряда сверх того, что уже содержится в его истории

Основная исследовательская задача- найти признаки такой связи и проверить, можно ли по свойствам временного ряда, текста, их временного взаимодействия и горизонта прогнозирования заранее оценивать применимость текстовых данных к конкретной forecasting-задаче

---

## 2.2. Основная гипотеза

**H1. Полезность текста для forecasting-задачи не является полностью случайной или специфичной для одной конкретной модели. Она частично определяется наблюдаемыми свойствами временного ряда, текстового потока, параметров задачи и их взаимодействия.**

Следствие:

> Если такая структура существует, её можно моделировать и использовать для предварительной оценки text utility на новых задачах

---

## 2.3. Основной исследовательский вопрос

> **Можно ли по данным, доступным до обучения мультимодальной модели, оценить ожидаемую дополнительную предсказательную ценность текста для новой задачи прогнозирования временного ряда?**

---

## 2.4. Дополнительные исследовательские вопросы

1. Какие свойства временного ряда связаны с последующей полезностью текста?
2. Какие свойства текстового потока связаны с последующей полезностью текста?
3. Насколько важна временная и семантическая связь между текстом и рядом?
4. Как меняется полезность текста в зависимости от горизонта прогнозирования и частоты ряда?
5. Насколько вообще информативность текста переносится между различными временными рядами и датасетами?
6. Насколько фактически реализуемый выигрыш зависит от архитектуры модели(прогностической)?
7. Можно ли отделить:
   - отсутствие полезного сигнала в тексте;
   - наличие сигнала, который конкретная модель не смогла использовать?
8. Можно ли использовать предварительную оценку полезности текста , которую мы получим , для удешевления разработки и обучения мультимодальных моделей?

---

# 3. Что считается benchmark

Benchmark в проекте - **не одна модель**, а совокупность:

- набора задач `Time Series + Текст + Конкретная задача`
- правила формирования эталонной оценки полезности текста
- единого temporal evaluation protocol
- контроль утечек данных
- правила разделения данных на обучающую, валидационную и тестовую выборки
- метрики оценки качества
- простые базовые методы для сравнения
- набор тестов на переносимость результатов на новые данные и задач
- протокол оценки новых моделей, предсказывающих полезность текста

В бенчмарке должна существовать возможность сравнивать различные методы, которые решают одну задачу, как мы уже и сказали выше 

> **по новой задаче прогнозирования предсказать текстовую полезность до обучения полной мультимодальной модели**

---

# 4. Что считается meta-model

Meta-model -это **метод решения задачи внутри бэнчмарка**

На вход ей передаётся информация, доступная до полноценного обучения:

- временной ряд или его представление
- текстовый поток или его представление
- параметры задачи
- при необходимости-общие характеристики предполагаемой архитектуры

На выходе:

- оценка ожидаемой полезности текста(будто вероятностная или какая-либо иная);
- или вероятность того, что текст даст улучшение прогнозирования(в целом тоже самое что мы имел в виду буквально в пункте выше);
- или оценка неопределённости.

Архитектура должна появиться как результат первых экспериментов и анализа данных.Мы не хотим фиксировать ее , а хотим мыслить в контексте того, что увидим при анализе самой задачи 

---

# Чекпойнт 1. Старт и планирование

## Цель

Зафиксировать исследовательскую постановку и границы проекта первого года.

## Задачи

- Уточнить, что понимается под применимостью и полезностью текста
- Выбрать стартовый домен и доступные данные для первичной проверки гипотезы
- Определить, как будет формироваться эталонная оценка полезности текста
- Зафиксировать основные риски: утечки данных, зависимость результата от конкретной модели и недостаточная переносимость
- Создать репозиторий и описать постановку и план проекта

## Результат

Зафиксированная постановка задачи, стартовый набор данных и план экспериментов

---

# Чекпойнт 2. Разведочный анализ данных и проверка жизнеспособности идеи

## Цель

Проверить, имеет ли смысл масштабировать идею до полноценного бенчмарка

Необходимо ответить на вопрос:

> **Есть ли наблюдаемый и предсказуемый сигнал, связывающий свойства пары «временной ряд + текст» с последующей полезностью текста?**

Для этого исследование сначала проводится внутри одного хорошо понятного домена. Если устойчивую структуру нельзя обнаружить даже в таком ограниченном случае, масштабирование на множество доменов будет не нужно

## Задачи

- Исследовать основные свойства временных рядов и текстового потока
- Проанализировать временную связь текста с последующей динамикой ряда
- Получить первичную эталонную оценку полезности текста на ограниченном наборе задач
- Проверить, можно ли по простым характеристикам данных предсказывать эту полезность лучше наивного подхода
- Проверить устойчивость результата на новых рядах или временных периодах

## Результат

Вывод о жизнеспособности основной гипотезы и решение о дальнейшем масштабировании, уточнении или сужении постановки

---

# Чекпойнт 3. Первые ML модели и минимальный бенчмарк

## Цель

Перейти от микро-исследования к первой воспроизводимой версии задачи.

## Задачи

- Формализовать эталонную оценку полезности текста.
- Зафиксировать структуру одной задачи бенчмарка: временной ряд, связанный текст и постановка прогнозирования
- Определить корректное разделение данных без временных и содержательных утечек
- Реализовать простые базовые методы предварительной оценки полезности текста
- Определить метрики качества и проверить вклад разных групп информации: ряда, текста и их взаимодействия 

## Результат

**Бенчмарк v1**: пилотный набор задач, эталонные значения, правила оценки, метрики и первые базовые результаты

---

# Чекпойнт 4. ML сервис

## Цель

Реализовать демонстрационный интерфейс текущего метода оценки.

## Задачи

Создать сервис, который принимает:

- временной ряд
- текстовые данные
- параметры задачи прогнозирования

и возвращает предварительную оценку применимости текста

Сервис должен демонстрировать основную идею проекта: **оценка выполняется до обучения полноценной модели «временной ряд + текст»**.

## Результат

Работающий API и воспроизводимый процесс получения оценки

---

# Промежуточная защита

К концу первого полугода исследования необходимо показать:

- постановку исследовательской задачи
- результаты проверки жизнеспособности гипотезы
- первую версию бенчмарка
- первые методы оценки применимости текста
- основные выявленные ограничения
- план масштабирования на второй семестр

Главный вопрос промежуточной защиты:

> **Есть ли эмпирические основания считать предварительную оценку применимости текста реализуемой задачей?**

---

# Чекпойнт 5. Расширение бенчмарка и улучшение модели

## Цель

Перейти от пилотной постановки к более общей версии задачи(если прошлый пункты окажутся реализуемым конечно :) )

## Задачи

- Увеличить разнообразие временных рядов, текстов, горизонтов и частот
- При наличии достаточного сигнала добавить новые наборы данных или второй домен
- Проверить устойчивость оценки полезности текста между различными задачами
- Исследовать, насколько результат зависит от используемой прогнозной архитектуры
- Перейти от простых признаков к более содержательным представлениям данных, если это оправдано результатами первого семестра
- Проверить переносимость оценки на невиденные ряды и наборы данных

## Результат

**Бенчмарк v2** и улучшенная версия модели предварительной оценки

---

# Чекпойнт 6. Нейронная метамодель

## Цель

Разработать нейросетевой метод оценки применимости текста

## Постановка

Модель должна совместно учитывать:

- структуру временного ряда
- содержание текстовых данных
- параметры задачи прогнозирования
- взаимодействие между текстом и рядом

Конкретная архитектура заранее не фиксируется и выбирается на основе результатов предыдущих этапов

Основной вопрос этапа:

> **Даёт ли обучаемое совместное представление текста и временного ряда более устойчивую и переносимую оценку, чем простые методы?**

## Результат

Первая версия нейронной DL метамодели, её сравнение с базовыми подходами и анализ ошибок

---

# Чекпойнт 7. Финальные доработки

## Цель

Сформировать законченный и воспроизводимый исследовательский результат

## Задачи

- Настроить воспроизводимость и логирование экспериментов
- Зафиксировать итоговые разделения данных и протокол оценки
- Проверить устойчивость результата к шумным, нерелевантным и устаревшим текстам
- Провести итоговую оценку переносимости модели
- Проанализировать зависимость результата от домена и прогнозной архитектуры
- Подготовить итоговую версию бенчмарка, метамодели и исследовательского отчёта

## Результат

- **Бенчмарк v3**;
- первая законченная версия метамодели
- воспроизводимый набор экспериментов
- сервис
- материал для итоговой защиты и потенциальной статьи
---



> **Можно ли заранее определить, когда текст потенциально полезен для задачи прогнозирования временного ряда?**

Дальнейшая работа может быть направлена на улучшение метамодели, более глубокое исследование зависимости полезности текста от архитектуры или разработку моделей, которые используют полученную оценку непосредственно при интеграции текста в прогнозирование временных рядов

