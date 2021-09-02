# Создаем коннекцию к базе


```python
# импортируем библиотеки
import pandas as pd
from sqlalchemy import create_engine

db_config = {'user': 'praktikum_student', # имя пользователя
            'pwd': 'Sdf4$2;d-d30pp', # пароль
            'host': 'rc1b-wcoijxj3yxfsf3fs.mdb.yandexcloud.net',
            'port': 6432, # порт подключения
            'db': 'data-analyst-zen-project-db'} # название базы данных

connection_string = 'postgresql://{}:{}@{}:{}/{}'.format(db_config['user'],
                                                db_config['pwd'],
                                                db_config['host'],
                                                db_config['port'],
                                                db_config['db'])

engine = create_engine(connection_string)

query = '''
            SELECT * FROM dash_visits
        '''

dash_visits = pd.io.sql.read_sql(query, con = engine)
```

Коннекция к базе хранится в переменной engine.


```python
dash_visits.to_csv('dash_visits.csv', sep='\t', encoding='utf-8', index=False)
```


```python
dash_visits.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>record_id</th>
      <th>item_topic</th>
      <th>source_topic</th>
      <th>age_segment</th>
      <th>dt</th>
      <th>visits</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1040597</td>
      <td>Деньги</td>
      <td>Авто</td>
      <td>18-25</td>
      <td>2019-09-24 18:32:00</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1040598</td>
      <td>Деньги</td>
      <td>Авто</td>
      <td>18-25</td>
      <td>2019-09-24 18:35:00</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1040599</td>
      <td>Деньги</td>
      <td>Авто</td>
      <td>18-25</td>
      <td>2019-09-24 18:54:00</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1040600</td>
      <td>Деньги</td>
      <td>Авто</td>
      <td>18-25</td>
      <td>2019-09-24 18:55:00</td>
      <td>17</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1040601</td>
      <td>Деньги</td>
      <td>Авто</td>
      <td>18-25</td>
      <td>2019-09-24 18:56:00</td>
      <td>27</td>
    </tr>
  </tbody>
</table>
</div>




```python
dash_visits.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 30745 entries, 0 to 30744
    Data columns (total 6 columns):
     #   Column        Non-Null Count  Dtype         
    ---  ------        --------------  -----         
     0   record_id     30745 non-null  int64         
     1   item_topic    30745 non-null  object        
     2   source_topic  30745 non-null  object        
     3   age_segment   30745 non-null  object        
     4   dt            30745 non-null  datetime64[ns]
     5   visits        30745 non-null  int64         
    dtypes: datetime64[ns](1), int64(2), object(3)
    memory usage: 1.4+ MB



```python
dash_visits.duplicated().value_counts()
```




    False    30745
    dtype: int64




```python
for i in dash_visits.columns:
    print(i.upper())
    print(dash_visits[i].describe())
    print()
    print(dash_visits[i].value_counts())
    print('------------------------------')
```

    RECORD_ID
    count    3.074500e+04
    mean     1.055969e+06
    std      8.875461e+03
    min      1.040597e+06
    25%      1.048283e+06
    50%      1.055969e+06
    75%      1.063655e+06
    max      1.071341e+06
    Name: record_id, dtype: float64
    
    1048576    1
    1064947    1
    1057457    1
    1059504    1
    1045163    1
              ..
    1060143    1
    1058094    1
    1064237    1
    1062188    1
    1050623    1
    Name: record_id, Length: 30745, dtype: int64
    ------------------------------
    ITEM_TOPIC
    count         30745
    unique           25
    top       Отношения
    freq           1536
    Name: item_topic, dtype: object
    
    Отношения             1536
    Интересные факты      1535
    Наука                 1505
    Подборки              1456
    Полезные советы       1424
    Общество              1422
    Россия                1385
    История               1363
    Семья                 1287
    Путешествия           1247
    Деньги                1234
    Женщины               1230
    Дети                  1229
    Туризм                1206
    Здоровье              1203
    Красота               1193
    Культура              1160
    Юмор                  1129
    Искусство             1119
    Рассказы              1109
    Психология            1056
    Скандалы              1023
    Знаменитости           976
    Женская психология     914
    Шоу                    804
    Name: item_topic, dtype: int64
    ------------------------------
    SOURCE_TOPIC
    count                  30745
    unique                    26
    top       Семейные отношения
    freq                    1822
    Name: source_topic, dtype: object
    
    Семейные отношения    1822
    Россия                1687
    Знаменитости          1650
    Полезные советы       1578
    Путешествия           1563
    Кино                  1505
    Дети                  1459
    История               1437
    Семья                 1405
    Одежда                1379
    Здоровье              1243
    Искусство             1228
    Авто                  1077
    Психология            1055
    Сад и дача            1036
    Политика              1024
    Спорт                 1007
    Сделай сам             995
    Ремонт                 985
    Деньги                 973
    Еда                    912
    Интерьеры              809
    Строительство          758
    Музыка                 750
    Технологии             741
    Финансы                667
    Name: source_topic, dtype: int64
    ------------------------------
    AGE_SEGMENT
    count     30745
    unique        6
    top       18-25
    freq       7056
    Name: age_segment, dtype: object
    
    18-25    7056
    26-30    5875
    31-35    5552
    36-40    5105
    41-45    3903
    45+      3254
    Name: age_segment, dtype: int64
    ------------------------------
    DT
    count                   30745
    unique                     17
    top       2019-09-24 18:58:00
    freq                     3383
    first     2019-09-24 18:28:00
    last      2019-09-24 19:00:00
    Name: dt, dtype: object
    
    2019-09-24 18:58:00    3383
    2019-09-24 18:57:00    3342
    2019-09-24 18:56:00    3325
    2019-09-24 18:59:00    3317
    2019-09-24 18:55:00    3088
    2019-09-24 19:00:00    2729
    2019-09-24 18:54:00    2551
    2019-09-24 18:30:00    1261
    2019-09-24 18:32:00    1257
    2019-09-24 18:31:00    1253
    2019-09-24 18:53:00    1107
    2019-09-24 18:29:00    1031
    2019-09-24 18:33:00    1007
    2019-09-24 18:52:00     719
    2019-09-24 18:28:00     615
    2019-09-24 18:34:00     576
    2019-09-24 18:35:00     184
    Name: dt, dtype: int64
    ------------------------------
    VISITS
    count    30745.000000
    mean        10.089673
    std         19.727601
    min          1.000000
    25%          1.000000
    50%          3.000000
    75%         10.000000
    max        371.000000
    Name: visits, dtype: float64
    
    1      8999
    2      4304
    3      2637
    4      1879
    5      1359
           ... 
    168       1
    231       1
    135       1
    246       1
    207       1
    Name: visits, Length: 212, dtype: int64
    ------------------------------


    <ipython-input-6-722aa352a41e>:3: FutureWarning: Treating datetime data as categorical rather than numeric in `.describe` is deprecated and will be removed in a future version of pandas. Specify `datetime_is_numeric=True` to silence this warning and adopt the future behavior now.
      print(dash_visits[i].describe())



```python
dash_visits.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>record_id</th>
      <th>item_topic</th>
      <th>source_topic</th>
      <th>age_segment</th>
      <th>dt</th>
      <th>visits</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1040597</td>
      <td>Деньги</td>
      <td>Авто</td>
      <td>18-25</td>
      <td>2019-09-24 18:32:00</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1040598</td>
      <td>Деньги</td>
      <td>Авто</td>
      <td>18-25</td>
      <td>2019-09-24 18:35:00</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1040599</td>
      <td>Деньги</td>
      <td>Авто</td>
      <td>18-25</td>
      <td>2019-09-24 18:54:00</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1040600</td>
      <td>Деньги</td>
      <td>Авто</td>
      <td>18-25</td>
      <td>2019-09-24 18:55:00</td>
      <td>17</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1040601</td>
      <td>Деньги</td>
      <td>Авто</td>
      <td>18-25</td>
      <td>2019-09-24 18:56:00</td>
      <td>27</td>
    </tr>
  </tbody>
</table>
</div>




```python
dash_visits.groupby('item_topic').agg({'visits':'sum'})\
.sort_values(by = 'visits', ascending = False).head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>visits</th>
    </tr>
    <tr>
      <th>item_topic</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Наука</th>
      <td>21736</td>
    </tr>
    <tr>
      <th>Отношения</th>
      <td>20666</td>
    </tr>
    <tr>
      <th>Интересные факты</th>
      <td>19942</td>
    </tr>
    <tr>
      <th>Общество</th>
      <td>19640</td>
    </tr>
    <tr>
      <th>Подборки</th>
      <td>17772</td>
    </tr>
    <tr>
      <th>Россия</th>
      <td>16966</td>
    </tr>
    <tr>
      <th>Полезные советы</th>
      <td>15435</td>
    </tr>
    <tr>
      <th>История</th>
      <td>15389</td>
    </tr>
    <tr>
      <th>Семья</th>
      <td>11897</td>
    </tr>
    <tr>
      <th>Женщины</th>
      <td>11499</td>
    </tr>
  </tbody>
</table>
</div>




```python
dash_visits['visits'].sum()
```




    310207




```python
dash_visits.groupby('source_topic').agg({'record_id':'nunique'})\
.sort_values(by = 'record_id', ascending = False).head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>record_id</th>
    </tr>
    <tr>
      <th>source_topic</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Семейные отношения</th>
      <td>1822</td>
    </tr>
    <tr>
      <th>Россия</th>
      <td>1687</td>
    </tr>
    <tr>
      <th>Знаменитости</th>
      <td>1650</td>
    </tr>
    <tr>
      <th>Полезные советы</th>
      <td>1578</td>
    </tr>
    <tr>
      <th>Путешествия</th>
      <td>1563</td>
    </tr>
    <tr>
      <th>Кино</th>
      <td>1505</td>
    </tr>
    <tr>
      <th>Дети</th>
      <td>1459</td>
    </tr>
    <tr>
      <th>История</th>
      <td>1437</td>
    </tr>
    <tr>
      <th>Семья</th>
      <td>1405</td>
    </tr>
    <tr>
      <th>Одежда</th>
      <td>1379</td>
    </tr>
  </tbody>
</table>
</div>




```python
for i in dash_visits['source_topic'].unique():
    print(i.upper())
    print(dash_visits[dash_visits['source_topic'] == i].groupby('item_topic').agg({'visits':'sum'})\
         .sort_values(by='visits', ascending = False).head(3))
    print('*****************************')
    print()
```

    АВТО
                      visits
    item_topic              
    Россия              1885
    Наука               1606
    Интересные факты    1254
    *****************************
    
    ДЕНЬГИ
                     visits
    item_topic             
    Полезные советы     916
    Семья               458
    Рассказы            454
    *****************************
    
    ДЕТИ
                visits
    item_topic        
    Психология    1233
    История       1047
    Общество      1007
    *****************************
    
    ЕДА
                visits
    item_topic        
    Семья         1236
    Подборки       871
    Дети           675
    *****************************
    
    ЗДОРОВЬЕ
                      visits
    item_topic              
    Интересные факты    2090
    Полезные советы     1346
    Общество            1181
    *****************************
    
    ЗНАМЕНИТОСТИ
                visits
    item_topic        
    Отношения     2040
    Скандалы      1992
    Россия        1579
    *****************************
    
    ИНТЕРЬЕРЫ
                     visits
    item_topic             
    Отношения           862
    Полезные советы     427
    Семья               321
    *****************************
    
    ИСКУССТВО
                      visits
    item_topic              
    Интересные факты     697
    История              695
    Культура             682
    *****************************
    
    ИСТОРИЯ
                      visits
    item_topic              
    Интересные факты    1273
    Общество            1116
    Россия              1104
    *****************************
    
    КИНО
                visits
    item_topic        
    Наука         3279
    Шоу           2201
    Культура      1543
    *****************************
    
    МУЗЫКА
                      visits
    item_topic              
    Наука                403
    Интересные факты     325
    Россия               324
    *****************************
    
    ОДЕЖДА
                     visits
    item_topic             
    Подборки           1612
    Отношения          1428
    Полезные советы     891
    *****************************
    
    ПОЛЕЗНЫЕ СОВЕТЫ
                visits
    item_topic        
    Подборки      2795
    Отношения     2716
    Здоровье      2335
    *****************************
    
    ПОЛИТИКА
                visits
    item_topic        
    Общество      1209
    Деньги         949
    Отношения      830
    *****************************
    
    ПСИХОЛОГИЯ
                        visits
    item_topic                
    Общество               919
    Женская психология     463
    Интересные факты       449
    *****************************
    
    ПУТЕШЕСТВИЯ
                     visits
    item_topic             
    Рассказы           4587
    История            2643
    Полезные советы    2088
    *****************************
    
    РЕМОНТ
                     visits
    item_topic             
    Отношения           510
    Полезные советы     485
    Подборки            479
    *****************************
    
    РОССИЯ
                      visits
    item_topic              
    Общество            3471
    Россия              2847
    Интересные факты    2567
    *****************************
    
    САД И ДАЧА
                      visits
    item_topic              
    Отношения            944
    Интересные факты     825
    Дети                 633
    *****************************
    
    СДЕЛАЙ САМ
                     visits
    item_topic             
    Туризм              688
    Полезные советы     679
    Здоровье            548
    *****************************
    
    СЕМЕЙНЫЕ ОТНОШЕНИЯ
                        visits
    item_topic                
    Общество              2727
    Женщины               2270
    Женская психология    2073
    *****************************
    
    СЕМЬЯ
                visits
    item_topic        
    Общество      1416
    Семья         1131
    Женщины        988
    *****************************
    
    СПОРТ
                  visits
    item_topic          
    Россия           703
    Скандалы         597
    Знаменитости     433
    *****************************
    
    СТРОИТЕЛЬСТВО
                visits
    item_topic        
    Отношения      513
    Подборки       361
    Семья          351
    *****************************
    
    ТЕХНОЛОГИИ
                visits
    item_topic        
    Наука          749
    Подборки       440
    Россия         316
    *****************************
    
    ФИНАНСЫ
                visits
    item_topic        
    Деньги         415
    Общество       406
    Шоу            354
    *****************************
    



```python
dash_visits.groupby(['source_topic', 'item_topic']).agg({'visits':'sum'})\
.sort_values(by='visits', ascending = False).head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>visits</th>
    </tr>
    <tr>
      <th>source_topic</th>
      <th>item_topic</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Путешествия</th>
      <th>Рассказы</th>
      <td>4587</td>
    </tr>
    <tr>
      <th>Россия</th>
      <th>Общество</th>
      <td>3471</td>
    </tr>
    <tr>
      <th>Кино</th>
      <th>Наука</th>
      <td>3279</td>
    </tr>
    <tr>
      <th>Россия</th>
      <th>Россия</th>
      <td>2847</td>
    </tr>
    <tr>
      <th>Полезные советы</th>
      <th>Подборки</th>
      <td>2795</td>
    </tr>
    <tr>
      <th>Семейные отношения</th>
      <th>Общество</th>
      <td>2727</td>
    </tr>
    <tr>
      <th>Полезные советы</th>
      <th>Отношения</th>
      <td>2716</td>
    </tr>
    <tr>
      <th>Путешествия</th>
      <th>История</th>
      <td>2643</td>
    </tr>
    <tr>
      <th>Россия</th>
      <th>Интересные факты</th>
      <td>2567</td>
    </tr>
    <tr>
      <th>Полезные советы</th>
      <th>Здоровье</th>
      <td>2335</td>
    </tr>
  </tbody>
</table>
</div>



# Ссылка на дашборд

https://public.tableau.com/app/profile/kostya2797/viz/Yandex_Zendashboard/Dashboard1
