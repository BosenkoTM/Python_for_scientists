# Анализ временных рядов в проекте Python

Временной ряд — это набор данных, записанных в разные временные интервалы. Анализ временного ряда означает анализ данных временного ряда с использованием различных статистических инструментов и методов. В этом проекте анализируется набор данных временного ряда **Parking Birmingham**, загруженный из репозитория машинного обучения UCI.

================================================================================

## Содержание

1. Введение во временные ряды

2. Типы данных

3. Терминология временных рядов

4. Импорт пакетов Python

5. Импорт набора данных

6. Описание набора данных

7. Анализ разведочных данных

8. Индексирование с данными временных рядов

9. Повторная выборка данных временных рядов

10. Обработка пропущенных значений в данных временных рядов

11. Визуализация данных временных рядов

12. Сезонная декомпозиция с данными временных рядов

13. Модель временных рядов ARIMA

14. Выбор параметров для модели временных рядов ARIMA

15. Подгонка модели временных рядов ARIMA

16. Создание и визуализация прогнозов

17. Заключение

18. Ссылки

================================================================================

## 1. Введение во временные ряды

Данные временных рядов — это набор данных или наблюдений, записанных через разные или регулярные интервалы времени. В общем случае временные ряды — это последовательность набора данных, которые получены через равные интервалы времени. Частота записанных значений может быть выражена через часы, ежедный, еженедельный, ежемесячный, ежеквартальный или ежегодный интервал времени.

Анализ временных рядов охватывает статистические методы анализа данных временных рядов. Эти методы позволяют извлекать значимую статистику, закономерности и другие характеристики данных. Временные ряды визуализируются с помощью линейных диаграмм. Таким образом, анализ временных рядов включает понимание неотъемлемых аспектов работы с временными рядами, чтобы  создавать точные прогнозы.

Приложения временных рядов используются в статистике, финансах или бизнес-приложениях. Очень распространенным примером временных рядов является ежедневное значение закрытия фондового индекса, такого как NASDAQ или Dow Jones. Другие распространенные приложения временных рядов — прогнозирование продаж и спроса, прогнозирование погоды, эконометрика, обработка сигналов, распознавание образов и прогнозирование землетрясений.

В этом проекте проводится анализ временных рядов набора данных **Parking Birmingham**, загруженного из репозитория машинного обучения UCI.

================================================================================

## 2. Типы данных

Как указано выше, анализ временных рядов — это статистический анализ данных временных рядов. Данные временных рядов означают, что данные записаны в разные периоды времени или интервалы. Данные временных рядов могут быть трех типов:-

1. **Данные временных рядов** — наблюдения значений переменной, записанные в разные моменты времени, называются данными временных рядов.

2. **Данные перекрестного сечения** — это данные одной или нескольких переменных, записанные в один и тот же момент времени.

3. **Объединенные данные** — это комбинация данных временных рядов и данных перекрестного сечения.

================================================================================

## 3. Терминология временных рядов

Во временных рядах существуют различные термины и концепции, которые следует знать.

1. **Зависимость** — относится к ассоциации двух наблюдений одной и той же переменной в предыдущие периоды времени.

2. **Стационарность** — показывает среднее значение ряда, которое остается постоянным в течение периода времени. Если прошлые эффекты накапливаются, а значения увеличиваются до бесконечности, то стационарность не соблюдается.

3. **Различение** — различение используется для того, чтобы сделать ряд стационарным и контролировать автокорреляции. В анализе временных рядов могут быть некоторые случаи, когда не требуется различение, а быстро расходящиеся ряды могут давать неверные оценки.

4. **Спецификация** — может включать проверку линейных или нелинейных связей зависимых переменных с использованием моделей временных рядов, таких как модели ARIMA.

5. **Экспоненциальное сглаживание** - Экспоненциальное сглаживание в анализе временных рядов предсказывает значение следующего периода на основе прошлого и текущего значения. Оно включает в себя усреднение данных таким образом, что аномальные компоненты каждого отдельного временного интервала или значения или наблюдения взаимосокращаются. Метод экспоненциального сглаживания используется для прогнозирования краткосрочного прогноза.

6. **Подгонка кривой** - Регрессия подгонки кривой в анализе временных рядов используется, когда данные находятся в нелинейной зависимости.

7. **ARIMA** - ARIMA означает Auto Regressive Integrated Moving Average.
================================================================================

## 4. Импорт пакетов Python

Для анализа временных рядов понадобятся пакеты Python — numpy, pandas, matplotlib и seaborn. 

Если требуется, чтобы изображения были построены в  Jupyter Notebook, мы должны добавить магическую команду IPython %matplotlib в наш код.

Кроме этого, используем параметры построения графиков Seaborn по умолчанию с `sns.set()`.

Итак, код выглядит следующим образом:

`import numpy as np`

`import pandas as pd`

`import matplotlib.pyplot as plt`

`import seaborn as sns`

`%matplotlib inline`

`sns.set()`

================================================================================

## 5. Импорт набора данных

Импорт данных, которые будет использоваться в этом проекте с помощью функции pandas `read_csv()`.

`data = 'datasets/dataset.csv'`

`df = pd.read_csv(data)`

================================================================================

## 6. Dataset description

Используем `Parking Birmingham Data Set` для этого проекта. Набор данных - парковка автомобилей в городе Бирмингем в Великобритании.
Данные показывают уровень загруженности (с 8:00 до 16:30) автомобилей с 2016/10/04 по 2016/12/19. Набор данных содержит 35717 экземпляров и 4 атрибута.

Набор данных можно найти по следующему адресу:-

https://archive.ics.uci.edu/ml/datasets/Parking+Birmingham

================================================================================

## 7. Исследовательский анализ данных

Проверим размерность датафрейма с помощью метода **shape()**.

`df.shape`

В наборе данных 35717 строк и 4 столбца.

Далее проверим первые пять строк набора данных с помощью метода **head()**.


`df.head()`

Столбец `LastUpdated` содержит дату и время, объединенные в один столбец временной метки. Нам нужно разделить его на два отдельных столбца.

Теперь я воспользуюсь методом **info()**, чтобы просмотреть кратную информацию по датафрейму.

`df.info()`

Видно, что столбец `LastUpdated` имеет тип данных object. Требуется преобразовать его в формат datatime. Можно использовать метод pandas **to_datetime()**. Он даст два столбца `Date` и `Time` с разделенными датами.

преобразуем столбец `LastUpdated` в формат `datetime`

`df['LastUpdated'] = pd.to_datetime(df['LastUpdated'])`

еще раз просмотрите сводку по dataframe

`df.info()`

Теперь мы видим, что столбец `LastUpdated` имеет тип данных `datetime`. Разобьем этот столбец `LastUpdated` на два отдельных столбца `Date` и `Time`.

`df['Date'] = df['LastUpdated'].dt.date`

`df['Time'] = df['LastUpdated'].dt.time`

Я подтвержу, что столбец «LastUpdated» теперь разделен на два отдельных столбца, просмотрев первые десять строк набора данных.

еще раз просмотрите первые десять строк набора данных

`df.head(10)`

Теперь удалим лишние столбцы из набора данных временного ряда.


`cols = ['SystemCodeNumber', 'Capacity', 'LastUpdated', 'Time']`

`df.drop(cols, axis=1, inplace=True)`

Проверить типы данных столбцов.

`df.dtypes`

Видно, что столбец `Date` имеет тип данных `object`. Он должен иметь формат `datetime`. Преобразуем тип данных столбца `Date` из типа данных `object` в формат `datetime`. Метод `to_datetime()` библиотеки Pandas позволяет  преобразовать тип данных object в формат `datetime` библиотеки Python.

`df['Date']=pd.to_datetime(df['Date'])`

еще раз проверьте тип данных df dataframe

`df.dtypes`

Теперь видим, что тип данных столбца `Date` — `datetime`.

================================================================================

## 8. Индексирование с данными временных рядов

При работе с данными временных рядов в Python всегда необходимо устанавливать даты в качестве индекса. Поэтому определим столбец `Date` в качестве индекса фрейма данных.

`df.set_index('Date', inplace=True)`

`df.index`

Поле `dtype=datetime[ns]` подтверждает, что индекс состоит из объекта `datestamp`. `length=35717` предполагает, что  есть 35717 меток с датой. Но параметр `freq=None` предполагает, что частота для метки даты не указана.

================================================================================

## 9. Повторная выборка данных временных рядов

Если более внимательно рассмотреть данные, увидим, что существуют разные временные промежутки в течение одного дня. С этим типом данных может быть сложно работать. Поэтому  преобразуем этот набор данных в более удобный.

Используем функцию `resample()` из pandas dataframe, которая в основном используется для  временных рядов. Она позволяет группировать временные ряды в блоки (1 день или 1 месяц), применять функцию к каждой группе (среднее) и создавать повторно выбранные данные.

`y=df['Occupancy'].resample('D').mean()`


`y.head(10)`


Здесь термин `D` означает, что группируем данные в блоки по дням и вычисляем среднее значение в течении одного дня.


================================================================================


## 10. Handling missing values in time series data


Now, I will check for missing values in the time series data. The following command will help me to do that.


`y.isnull().sum()`


The above command shows that there are 4 days with missing values in the time series.


I will fill in the missing values using the pandas `fillna()` command. I will use the `method=bfill` argument to fill in the missing values. It will fill in the missing values with the values in the forward index.


`y.fillna(method='bfill', inplace=True)`


Now, I will again check for missing values in the time series.


`y.isnull().sum()`


The above command shows that there are no missing values in the time series.


================================================================================


## 11. Visualizing the time series data


Visualizing the time series data is an important step in time series analysis. It will help us to visualize several important things 
as follows:-


-**seasonality** - does the time series data display seasonality or periodic pattern?


-**trend** - does the time series data display a consistent upwards or downwards slope?


-**noise** - are there any outliers or missing values that are not consistent with the time series data?


The visualization helps to answer these questions.


Visualize the time series data


`y.plot(figsize=(15, 6))`


`plt.show()`


The plot reveals some interesting pattern in the time series. It has a seasonality pattern but no increasing or decreasing trend. 
The pattern reveals that the `Occupancy` has increased in December month. May be it is due to Christmas celebrations in December.


================================================================================


## 12. Seasonal decomposition with time series data


There is another method to visualize the time series data. This method is called **time-series decomposition**. It allows us to decompose the time series into three distinct components - trend, seasonality and noise.


Python provides a `statsmodels` module which provides tools and techniques for statistical analysis and modeling. This `statsmodels` module provides a `seasonal_decompose` function to perform seasonal decomposition.


Seasonal decomposition returns a figure of relatively small size. So the first two lines of code chunk ensures that the output figure is large enough for us to visualize. We can perform seasonal decomposition in Python with the following lines of code:-


`from pylab import rcParams`


`rcParams['figure.figsize'] = 16, 8`


`decomposition = sm.tsa.seasonal_decompose(y, model='additive')`


`fig = decomposition.plot()`


`plt.show()`


Time series decomposition makes it easy to visualize the data in clear manner. It helps us to identify variation in the time series. 
The above plot shows the upwards trend in time series. It can be used to understand the structure of the time series. The time series decomposition is important because many forecasting methods are built upon this concept of structured decomposition to produce forecasts.


================================================================================


## 13. The ARIMA Time Series Model


One of the most common methods used in time series forecasting is known as the **ARIMA model**. ARIMA stands for **AutoRegressive Integrated Moving Average**. It is a generalization of an AutoRegressive Moving Average (ARMA) model. These models are fitted to time series data to better understand the data or to predict future points in the series called **forecasting**. 


The AR part of ARIMA indicates that the evolving variable of interest is regressed on prior values. The MA part indicates that the regression error is actually a linear combination of error terms. The I (for "integrated") indicates that the data values have been replaced with the difference between their values and the previous values (and this differencing process may have been performed more than once). The purpose of these features is to make the model fit the data as well as possible.


There are three distinct integers (p, d, q) that are used to parametrize ARIMA models. So, ARIMA models are denoted with the notation ARIMA(p, d, q). These three parameters account for **seasonality**, **trend** and **noise** in timeseries datasets.


- **p** is the auto-regressive part of the model. It allows us to incorporate the effect of past values into our model. 


- **d** is the integrated part of the model. This includes terms in the model that incorporate the amount of differencing to apply to the time series. 


- **q** is the moving average part of the model. This allows us to set the error of our model as a linear combination of the error values observed at previous time points in the past.


Non-seasonal ARIMA models are generally denoted by `ARIMA(p,d,q)` where parameters p, d and q are non-negative integers. `p` is the order (number of time lags) of the autoregressive model, `d` is the degree of differencing (the number of times the data have had past values subtracted), and `q` is the order of the moving-average model. 


Seasonal ARIMA models are usually denoted by `ARIMA(p,d,q)(P,D,Q)s`, where `s` refers to the number of periods in each season, and the uppercase P,D,Q refer to the autoregressive, differencing and moving average terms for the seasonal part of the ARIMA model. The term `s` refers to the periodicity of the time series.


================================================================================


## 14.	Parameter Selection for the ARIMA Time Series Model


Now, I will fit the time series data with a seasonal ARIMA model. I have to find the optimal parameter values for our `ARIMA(p,d,q)(P,D,Q)s` time series model. The python code below will help us to find the optimal parameter values for our model. 


The following code will use a grid search to iteratively explore different combinations of parameters. For each combination of parameters, we fit a new seasonal ARIMA model with the `SARIMAX()` function from the statsmodels module and assess its overall quality. The optimal set of parameters will be the one that yields the best performance.


Define the p, d and q parameters to take any value between 0 and 2


`p = d = q = range(0, 2)`


Generate all different combinations of p, q and q triplets


`pdq = list(itertools.product(p, d, q))`


Generate all different combinations of seasonal p, q and q triplets


`seasonal_pdq = [(x[0], x[1], x[2], 4) for x in list(itertools.product(p, d, q))]`


`print('Examples of parameter combinations for Seasonal ARIMA are as follows:-')`


`print('SARIMAX: {} x {}'.format(pdq[1], seasonal_pdq[1]))`


`print('SARIMAX: {} x {}'.format(pdq[1], seasonal_pdq[2]))`


`print('SARIMAX: {} x {}'.format(pdq[2], seasonal_pdq[3]))`


`print('SARIMAX: {} x {}'.format(pdq[2], seasonal_pdq[4]))`



### Grid Search or Hyperparameter Optimization


The above sets of triplets of parameters can now be used to automate the process of training and evaluating ARIMA models on 
different combinations of parameters. In Statistics and Machine Learning, this process is known as **grid search (or hyperparameter optimization)** for model selection.


The statistical models fitted with different parameters can be ranked and compared against each other based on their `AIC` value. `AIC` which stands for `Akaike Information Criterion` value is conveniently returned with ARIMA models fitted using statsmodels. It measures how well a model fits the data while taking into account the overall complexity of the model. A model that fits the data very well while using lots of features will be assigned a larger AIC score than a model that uses fewer features to achieve the same goodness-of-fit. Therefore, we are interested in finding the model that yields the lowest `AIC` value.


The following code snippet iterates through combinations of parameters and uses the `SARIMAX` function from statsmodels to fit the corresponding Seasonal ARIMA model. Here, the order argument specifies the (p, d, q) parameters, while the seasonal_order argument specifies the (P, D, Q, S) seasonal component of the Seasonal ARIMA model. After fitting each SARIMAX()model, the code prints out its respective AIC score.


The code output suggests that SARIMAX(1, 1, 1)x(0, 1, 1, 4) provides the lowest AIC value of 767.8663. So, we should consider this to be the optimal option out of all the models considered.


================================================================================


## 15.Fitting an ARIMA Time Series Model


I have identified the optimal set of parameters that produces the best fit model. Now, I will fit these optimal parameter values 
into a new `SARIMAX` model.


`model = sm.tsa.statespace.SARIMAX(y,`


                               `order=(1, 1, 1),`
                                
                            
                                `seasonal_order=(0, 1, 1, 4),`
                                
                                
                                `enforce_stationarity=False,`
                                
                                
                                `enforce_invertibility=False)`
                                

`results = model.fit()`


`print(results.summary().tables[1])`


The above summary table displays significant amount of information. The coef column shows the weight or importance of each feature 
and how each one impacts the time series. The P>|z| column shows us the significance of each feature weight.


Now, I will run model diagnostics to detect any unusual behaviour. It is important to run model diagnostics to ensure that none of 
the assumptions made by the model have been violated. The `plot_diagnostics` object generates model diagnostics.


`results.plot_diagnostics(figsize=(15, 12))`


`plt.show()`


We should always check that the residuals of the model are uncorrelated and normally distributed with zero-mean. If the seasonal ARIMA model does not satisfy these properties, then the model can be further improved.


In this case, the model diagnostics suggests that the model residuals are not normally distributed based on the following observations:-


- In the top right plot, we can see that the red KDE line does not follow with the N(0,1) line. This shows that the residuals are not normally distributed. 


- The qq-plot on the bottom left shows that the ordered distribution of residuals (blue dots) follows the linear trend of the samples taken from a standard normal distribution with N(0, 1). This is a strong indication that the residuals are not normally distributed.


- The residuals over time (top left plot) don't display any obvious seasonality and appear to be white noise. This is confirmed by the autocorrelation (i.e. correlogram) plot on the bottom right. It shows that the time series residuals have low correlation with lagged versions of itself.



So, we can conclude that our model does not produce a satisfactory fit to the time series data. We can change some parameters of our seasonal ARIMA model to improve the model fit. The grid search only considered a restricted set of parameter combinations. We may find better models if we widened the grid search.


Although, the model does not produce a satisfactory fit to the data, but I will use the same model to illustrate the process of validating and producing the forecasts for demonstration purposes.


================================================================================


## 16.	Producing and Visualizing the Forecasts


Now, I will show how to use this time series model to forecast future values. The `get_forecast()` attribute of the time series 
object can compute forecasted values for a specified number of steps ahead.


Get forecast 100 steps ahead in future


`pred_uc = results.get_forecast(steps=100)`


Get confidence intervals of forecasts


`pred_ci = pred_uc.conf_int()`


`ax = y.plot(label='observed', figsize=(20, 15))`


`pred_uc.predicted_mean.plot(ax=ax, label='forecast')`


`ax.fill_between(pred_ci.index`,

                `pred_ci.iloc[:, 0],`
                
                `pred_ci.iloc[:, 1], color='k', alpha=.25)`
                

`ax.set_xlabel('Date')`


`ax.set_ylabel('Occupancy')`


`plt.legend()`


`plt.show()`


The forecast values and associated confidence intervals can now be used to further understand the time series and understand it.


================================================================================


## 17.	Conclusion


In this project, I implement a seasonal ARIMA time series model in Python to predict Occupancy rates of car parks in 
Parking Birmingham Data Set. The forecasts show that the time series model is expected to continue increasing at a steady pace. 
As we forecast further into the future, we become less confident in our values.


================================================================================


## 18. References


The ideas and concepts in this project are taken from the following websites:-


1.	https://en.wikipedia.org/wiki/Time_series


2.	https://www.statisticssolutions.com/time-series-analysis/


3.	https://en.wikipedia.org/wiki/Autoregressive_integrated_moving_average


4.	https://www.digitalocean.com/community/tutorials/a-guide-to-time-series-forecasting-with-arima-in-python-3


5.	https://www.digitalocean.com/community/tutorials/a-guide-to-time-series-visualization-with-python-3






