## Описание проекта

Проект по определению рыночной стоимости недвижимости на основе открытого набора данных `NYC Housing Data 2003-2019` от департамента финансов Нью-Йорка. В проекте также произведена оценка потенциального экономического эффекта. Датасет содержит информацию о ценах на жилье в Нью-Йорке на основе данных за последние 17 лет.

Ссылка на датасет: https://www.kaggle.com/kaijiechen/nyc-housing-data-20032019

### Постановка бизнес задачи

Допустим у нас есть платформа, которая является базой объявлений об аренде и продаже недвижимости. Одним из вариантов получения прибыли является риелторская коммиссия с суммы продажи или аренды. В этом случае целесообразно разработать модель, которая сможет определить рыночную стоимость продажи или аренды интересующей недвижимости. Определение стоимости защитит платформу и ее пользователей от недобросовестных клиентов, которые заинтересованы в занижении или завышении цены. Предсказание стоимости само по себе также можно использовать как коммерческую услугу.

### Математическая постановки задачи

Такая бизнес-цель требует построения модели регрессионного анализа. Так как цены на жилье меняются из года в год, для построения поддержания работоспособности модели необходимо обучать ее на актульных данных. Оптимальной метрикой качества для данной задачи, на мой взгляд, является RMSE, стремящееся к нулю. Для интерпретации результатов работы модели воспользуемся метрикой MAE.

## Валидация данных

Произведена предобработка данных по всем признакам кроме целевого. Затем данные были разбиты на две случайные выборки: основная и отложенная - для оценки экономической выгоды. Перед построением ML-модели также произведена предобработка целевого признака основной выборки. В свою очередь, эти данные были разбиты на тренировочную и тестовую выборки.
## Baseline модель

В качестве `Baseline` было взято среднее значение цены недвижимости для каждого `BOROUGH`, `ZIP CODE` и годa продажи.

## ML-модель

В качестве модели машинного обучения была выбрана модель `CatBoostRegressor`. Данная модель работает с категориальными признаками без дополнительного кодирования и масштабирования. При построении модели использовалась кросс-валидация на `5 K-folds` с подсчетом `RMSE` на каждом. Подбор гиппер-параметров производился по минимальному среднему по всем фолдам значению метрики `RMSE`. Затем готовая модель применена к тестовой выборке.

## Оценка экономического эффекта

При оценке экономического эффекта был сделан ряд допущений. Платформа берет комиссию в случае успешной сделки, допустим, в размере `0.1 %` от суммы сделки (то есть цены за недвижимость). В датасете представлены исторические данные о совершенных сделках, поэтому в случае, когда мы даем предсказание о цене на объекты недвижимости, которые в том числе не будут проданы, необходимо определить долю проданных объектов среди всех представленных на рынке. Допустим, на платформе за прогнозируемый период удается реализовать `70 %` объявлений.
