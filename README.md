# open_data_battle
Хакатон [Open Data Battle](https://open-data-battle.geecko.com/issues) по Data Science от банка "Открытие". 

### 12 место(TOP-4%) 

## Задача:

Банк построил модель оценки вероятности дефолта (PD модель). Нужно контролировать качество этой базовой модели и оценивать ошибку, поэтому на данном хакатоне нужно:
- разработать модель прогнозирования ошибки (MPP), ошибка в данном случае  - это разница между флагом flg_90_12_add, представляющим собой реализованное событие (0 - не дефолт, 1 - дефолт) и значением PD, представляющим собой оценку от базовой модели.

Описание данных:
- в выборке 82617 наблюдений и 1908 признаков, из которых 20 категориальных;
- названия всех признаков зашифрованы, а категориальные признаки закодированы, интерпретировать их возможности нет.

Решение в файле `odb_main.csv`

Final submission в файле `odb_final_submission.csv`
## Методы:

- удаление признаков с пропусками боле 50%;
- удаление признаков с низкой информативностью - признаки, в которых более 95% значений принадлежат одному значению;
- логарифмирование целевой переменной.

Подходы, которые не улучшили качество метрики:
- Target Encoding для категориальных признаков;
- удаления выбросов, с использованием алгоритма Isolation Forest;
- генерация новых признаков;
- отбор признаков на основе feature importance;
- логарифмирование некоторых числовых признаков;
- обогащение данных статистиками из внешних источников.

## Установка:
- войти в google colab;
- `pip install -U -r requirements.txt`

## Обучение и прогноз:
- отделение последних 1000 наблюдений из выборки, так как они являются тестовой частью;
- разбиение обучающей выборки на 12 фолдов, группируя по месяцам;
- поочередное обучение 12 моделей **XGBRegressor** на подгруппах из всех месяцев, кроме одного, который используем для валидации;
- каждая модель прогнозирует тестовую выборку;
- усреднение всех прогнозов.

## Качество:
Для оценки качества используется метрика Mean Absolute Error.

Лучший результат: `MAE = 0.05843`
