# SQL

Цель проекта - проанализировать базу данных.
В ней — информация о книгах, издательствах, авторах, а также пользовательские обзоры книг.

***Необходимо ответить на следующие вопросы:***
1) Сколько книг вышло после 1 января 2000 года;

2) Для каждой книги посчитайте количество обзоров и среднюю оценку;

3)  Определите издательство, которое выпустило наибольшее число книг толще 50 страниц — так вы исключите из анализа брошюры;

4) Определите автора с самой высокой средней оценкой книг — учитывайте только книги с 50 и более оценками;

5) Посчитайте среднее количество обзоров от пользователей, которые поставили больше 50 оценок.

**Описание данных:**

**Таблица `books`**

Содержит данные о книгах:

- `book_id` — идентификатор книги;
- `author_id` — идентификатор автора;
- `title` — название книги;
- `num_pages` — количество страниц;
- `publication_date` — дата публикации книги;
- `publisher_id` — идентификатор издателя.

**Таблица `authors`**

Содержит данные об авторах:

- `author_id` — идентификатор автора;
- `author` — имя автора.

**Таблица `publishers`**

Содержит данные об издательствах:

- `publisher_id` — идентификатор издательства;
- `publisher` — название издательства;

**Таблица `ratings`**

Содержит данные о пользовательских оценках книг:

- `rating_id` — идентификатор оценки;
- `book_id` — идентификатор книги;
- `username` — имя пользователя, оставившего оценку;
- `rating` — оценка книги.

**Таблица `reviews`**

Содержит данные о пользовательских обзорах на книги:

- `review_id` — идентификатор обзора;
- `book_id` — идентификатор книги;
- `username` — имя пользователя, написавшего обзор;
- `text` — текст обзора.

## Импортируем библиотеки и подключаем базу данных


```python
# импортируем библиотеки
import pandas as pd
from sqlalchemy import create_engine
# устанавливаем параметры
db_config = {'user': 'praktikum_student', # имя пользователя
 'pwd': 'Sdf4$2;d-d30pp', # пароль
 'host': 'rc1b-wcoijxj3yxfsf3fs.mdb.yandexcloud.net',
 'port': 6432, # порт подключения
 'db': 'data-analyst-final-project-db'} # название базы данных
connection_string = 'postgresql://{}:{}@{}:{}/{}'.format(db_config['user'],
 db_config['pwd'],
 db_config['host'],
 db_config['port'],
 db_config['db'])
# сохраняем коннектор
engine = create_engine(connection_string, connect_args={'sslmode':'require'})
```

## Исследуем таблицы


```python
tables = [
        'books',
        'authors',
        'publishers',
         'ratings',
         'reviews'
        ]

for i in tables:
    query = ''' SELECT * FROM '''
    query = query + i
    display(i.upper())
    display(pd.io.sql.read_sql(query, con = engine).head())
```


    'BOOKS'



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
      <th>book_id</th>
      <th>author_id</th>
      <th>title</th>
      <th>num_pages</th>
      <th>publication_date</th>
      <th>publisher_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1</td>
      <td>546</td>
      <td>'Salem's Lot</td>
      <td>594</td>
      <td>2005-11-01</td>
      <td>93</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2</td>
      <td>465</td>
      <td>1 000 Places to See Before You Die</td>
      <td>992</td>
      <td>2003-05-22</td>
      <td>336</td>
    </tr>
    <tr>
      <td>2</td>
      <td>3</td>
      <td>407</td>
      <td>13 Little Blue Envelopes (Little Blue Envelope...</td>
      <td>322</td>
      <td>2010-12-21</td>
      <td>135</td>
    </tr>
    <tr>
      <td>3</td>
      <td>4</td>
      <td>82</td>
      <td>1491: New Revelations of the Americas Before C...</td>
      <td>541</td>
      <td>2006-10-10</td>
      <td>309</td>
    </tr>
    <tr>
      <td>4</td>
      <td>5</td>
      <td>125</td>
      <td>1776</td>
      <td>386</td>
      <td>2006-07-04</td>
      <td>268</td>
    </tr>
  </tbody>
</table>
</div>



    'AUTHORS'



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
      <th>author_id</th>
      <th>author</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1</td>
      <td>A.S. Byatt</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2</td>
      <td>Aesop/Laura Harris/Laura Gibbs</td>
    </tr>
    <tr>
      <td>2</td>
      <td>3</td>
      <td>Agatha Christie</td>
    </tr>
    <tr>
      <td>3</td>
      <td>4</td>
      <td>Alan Brennert</td>
    </tr>
    <tr>
      <td>4</td>
      <td>5</td>
      <td>Alan Moore/David   Lloyd</td>
    </tr>
  </tbody>
</table>
</div>



    'PUBLISHERS'



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
      <th>publisher_id</th>
      <th>publisher</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1</td>
      <td>Ace</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2</td>
      <td>Ace Book</td>
    </tr>
    <tr>
      <td>2</td>
      <td>3</td>
      <td>Ace Books</td>
    </tr>
    <tr>
      <td>3</td>
      <td>4</td>
      <td>Ace Hardcover</td>
    </tr>
    <tr>
      <td>4</td>
      <td>5</td>
      <td>Addison Wesley Publishing Company</td>
    </tr>
  </tbody>
</table>
</div>



    'RATINGS'



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
      <th>rating_id</th>
      <th>book_id</th>
      <th>username</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>ryanfranco</td>
      <td>4</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>grantpatricia</td>
      <td>2</td>
    </tr>
    <tr>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>brandtandrea</td>
      <td>5</td>
    </tr>
    <tr>
      <td>3</td>
      <td>4</td>
      <td>2</td>
      <td>lorichen</td>
      <td>3</td>
    </tr>
    <tr>
      <td>4</td>
      <td>5</td>
      <td>2</td>
      <td>mariokeller</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



    'REVIEWS'



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
      <th>review_id</th>
      <th>book_id</th>
      <th>username</th>
      <th>text</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>brandtandrea</td>
      <td>Mention society tell send professor analysis. ...</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>ryanfranco</td>
      <td>Foot glass pretty audience hit themselves. Amo...</td>
    </tr>
    <tr>
      <td>2</td>
      <td>3</td>
      <td>2</td>
      <td>lorichen</td>
      <td>Listen treat keep worry. Miss husband tax but ...</td>
    </tr>
    <tr>
      <td>3</td>
      <td>4</td>
      <td>3</td>
      <td>johnsonamanda</td>
      <td>Finally month interesting blue could nature cu...</td>
    </tr>
    <tr>
      <td>4</td>
      <td>5</td>
      <td>3</td>
      <td>scotttamara</td>
      <td>Nation purpose heavy give wait song will. List...</td>
    </tr>
  </tbody>
</table>
</div>


Таблицы подключаются корректно.

## Задачи

### Посчитайте, сколько книг вышло после 1 января 2000 года


```python
query = ''' SELECT COUNT(DISTINCT book_id) 
            FROM books
            WHERE publication_date >= DATE('2000-01-01')
        '''
pd.io.sql.read_sql(query, con = engine) 
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
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>821</td>
    </tr>
  </tbody>
</table>
</div>



В нашей базе 819 книг, выпущенных после 2000-01-01.

### Для каждой книги посчитайте количество обзоров и среднюю оценку


```python
query = ''' SELECT COUNT(*)
            FROM books
            LEFT JOIN (SELECT book_id, COUNT(review_id) AS count_rev FROM reviews GROUP BY book_id) AS rev
            ON books.book_id = rev.book_id
            LEFT JOIN (SELECT book_id, AVG(rating) AS avg_rating FROM ratings GROUP BY book_id) AS reti
            ON books.book_id = reti.book_id
        '''
pd.io.sql.read_sql(query, con = engine) 
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
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1000</td>
    </tr>
  </tbody>
</table>
</div>




```python
query = ''' SELECT books.book_id, books.title, rev.count_rev, reti.avg_rating
            FROM books
            LEFT JOIN (SELECT book_id, COUNT(review_id) AS count_rev FROM reviews GROUP BY book_id) AS rev
            ON books.book_id = rev.book_id
            LEFT JOIN (SELECT book_id, AVG(rating) AS avg_rating FROM ratings GROUP BY book_id) AS reti
            ON books.book_id = reti.book_id
            LIMIT 1000
        '''
pd.io.sql.read_sql(query, con = engine)
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
      <th>book_id</th>
      <th>title</th>
      <th>count_rev</th>
      <th>avg_rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>652</td>
      <td>The Body in the Library (Miss Marple  #3)</td>
      <td>2.0</td>
      <td>4.500000</td>
    </tr>
    <tr>
      <td>1</td>
      <td>273</td>
      <td>Galápagos</td>
      <td>2.0</td>
      <td>4.500000</td>
    </tr>
    <tr>
      <td>2</td>
      <td>51</td>
      <td>A Tree Grows in Brooklyn</td>
      <td>5.0</td>
      <td>4.250000</td>
    </tr>
    <tr>
      <td>3</td>
      <td>951</td>
      <td>Undaunted Courage: The Pioneering First Missio...</td>
      <td>2.0</td>
      <td>4.000000</td>
    </tr>
    <tr>
      <td>4</td>
      <td>839</td>
      <td>The Prophet</td>
      <td>4.0</td>
      <td>4.285714</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>995</td>
      <td>64</td>
      <td>Alice in Wonderland</td>
      <td>4.0</td>
      <td>4.230769</td>
    </tr>
    <tr>
      <td>996</td>
      <td>55</td>
      <td>A Woman of Substance (Emma Harte Saga #1)</td>
      <td>2.0</td>
      <td>5.000000</td>
    </tr>
    <tr>
      <td>997</td>
      <td>148</td>
      <td>Christine</td>
      <td>3.0</td>
      <td>3.428571</td>
    </tr>
    <tr>
      <td>998</td>
      <td>790</td>
      <td>The Magicians' Guild (Black Magician Trilogy  #1)</td>
      <td>2.0</td>
      <td>3.500000</td>
    </tr>
    <tr>
      <td>999</td>
      <td>828</td>
      <td>The Plot Against America</td>
      <td>2.0</td>
      <td>3.000000</td>
    </tr>
  </tbody>
</table>
<p>1000 rows × 4 columns</p>
</div>



Присоединив к таблице books через LEFT JOIN 2 подзапроса из таблиц reviews и ratings по ключам book_id, мы получили данные по количеству обзоров и средней оценке пользователей.


```python
query = ''' SELECT books.book_id, books.title, rev.count_rev, reti.avg_rating
            FROM books
            LEFT JOIN (SELECT book_id, COUNT(review_id) AS count_rev FROM reviews GROUP BY book_id) AS rev
            ON books.book_id = rev.book_id
            LEFT JOIN (SELECT book_id, AVG(rating) AS avg_rating FROM ratings GROUP BY book_id) AS reti
            ON books.book_id = reti.book_id
            WHERE count_rev IS NULL OR avg_rating IS NULL
            LIMIT 1000
        '''
pd.io.sql.read_sql(query, con = engine) 
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
      <th>book_id</th>
      <th>title</th>
      <th>count_rev</th>
      <th>avg_rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>387</td>
      <td>Leonardo's Notebooks</td>
      <td>None</td>
      <td>4.000000</td>
    </tr>
    <tr>
      <td>1</td>
      <td>191</td>
      <td>Disney's Beauty and the Beast (A Little Golden...</td>
      <td>None</td>
      <td>4.000000</td>
    </tr>
    <tr>
      <td>2</td>
      <td>672</td>
      <td>The Cat in the Hat and Other Dr. Seuss Favorites</td>
      <td>None</td>
      <td>5.000000</td>
    </tr>
    <tr>
      <td>3</td>
      <td>808</td>
      <td>The Natural Way to Draw</td>
      <td>None</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <td>4</td>
      <td>83</td>
      <td>Anne Rice's The Vampire Lestat: A Graphic Novel</td>
      <td>None</td>
      <td>3.666667</td>
    </tr>
    <tr>
      <td>5</td>
      <td>221</td>
      <td>Essential Tales and Poems</td>
      <td>None</td>
      <td>4.000000</td>
    </tr>
  </tbody>
</table>
</div>



Так же получили что 5 книг не имеют обзоров.

### Определите издательство, которое выпустило наибольшее число книг толще 50 страниц — так вы исключите из анализа брошюры


```python
query = ''' SELECT publisher, COUNT(books.publisher_id)
            FROM books
            LEFT JOIN publishers ON books.publisher_id = publishers.publisher_id
            WHERE num_pages > 50
            GROUP BY publisher
            ORDER BY count DESC
            LIMIT 10
        '''
pd.io.sql.read_sql(query, con = engine) 
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
      <th>publisher</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Penguin Books</td>
      <td>42</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Vintage</td>
      <td>31</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Grand Central Publishing</td>
      <td>25</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Penguin Classics</td>
      <td>24</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Ballantine Books</td>
      <td>19</td>
    </tr>
    <tr>
      <td>5</td>
      <td>Bantam</td>
      <td>19</td>
    </tr>
    <tr>
      <td>6</td>
      <td>Berkley</td>
      <td>17</td>
    </tr>
    <tr>
      <td>7</td>
      <td>St. Martin's Press</td>
      <td>14</td>
    </tr>
    <tr>
      <td>8</td>
      <td>Berkley Books</td>
      <td>14</td>
    </tr>
    <tr>
      <td>9</td>
      <td>William Morrow Paperbacks</td>
      <td>13</td>
    </tr>
  </tbody>
</table>
</div>



Присоединив к таблице books через LEFT JOIN таблицу publishers по ключу publisher_id, задав условие num_pages > 50 и сгрупировав таблицу по столбцу publisher, мы получили ответ : **Penguin Books - 42 книги**.

### Определите автора с самой высокой средней оценкой книг — учитывайте только книги с 50 и более оценками


```python
query = ''' SELECT authors.author,
                   books.author_id, 
                   COUNT(books.book_id) AS count_book,
                   SUM(reti.ratings_count) AS ratings_count,
                   AVG(reti.avg_rating) AS avg_rating
            FROM books
            LEFT JOIN authors ON books.author_id = authors.author_id
            LEFT JOIN (SELECT book_id, 
                              COUNT(rating) AS ratings_count,
                              AVG(rating) AS avg_rating
                       FROM ratings GROUP BY book_id) AS reti
            ON books.book_id = reti.book_id
            WHERE reti.ratings_count >= 50
            GROUP BY books.author_id, authors.author
            ORDER BY avg_rating DESC 
            LIMIT 10
        '''
pd.io.sql.read_sql(query, con = engine) 
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
      <th>author</th>
      <th>author_id</th>
      <th>count_book</th>
      <th>ratings_count</th>
      <th>avg_rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>J.K. Rowling/Mary GrandPré</td>
      <td>236</td>
      <td>4</td>
      <td>310.0</td>
      <td>4.283844</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Markus Zusak/Cao Xuân Việt Khương</td>
      <td>402</td>
      <td>1</td>
      <td>53.0</td>
      <td>4.264151</td>
    </tr>
    <tr>
      <td>2</td>
      <td>J.R.R. Tolkien</td>
      <td>240</td>
      <td>2</td>
      <td>162.0</td>
      <td>4.258446</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Louisa May Alcott</td>
      <td>376</td>
      <td>1</td>
      <td>52.0</td>
      <td>4.192308</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Rick Riordan</td>
      <td>498</td>
      <td>1</td>
      <td>62.0</td>
      <td>4.080645</td>
    </tr>
    <tr>
      <td>5</td>
      <td>William Golding</td>
      <td>621</td>
      <td>1</td>
      <td>71.0</td>
      <td>3.901408</td>
    </tr>
    <tr>
      <td>6</td>
      <td>J.D. Salinger</td>
      <td>235</td>
      <td>1</td>
      <td>86.0</td>
      <td>3.825581</td>
    </tr>
    <tr>
      <td>7</td>
      <td>Paulo Coelho/Alan R. Clarke/Özdemir İnce</td>
      <td>469</td>
      <td>1</td>
      <td>57.0</td>
      <td>3.789474</td>
    </tr>
    <tr>
      <td>8</td>
      <td>William Shakespeare/Paul Werstine/Barbara A. M...</td>
      <td>630</td>
      <td>1</td>
      <td>66.0</td>
      <td>3.787879</td>
    </tr>
    <tr>
      <td>9</td>
      <td>Dan Brown</td>
      <td>106</td>
      <td>2</td>
      <td>143.0</td>
      <td>3.754540</td>
    </tr>
  </tbody>
</table>
</div>



Несмотря на то, что "Повелитель мух" ТОПчик, ***лидер по оценкам пользователей - J.K. Rowling/Mary GrandPré (4.28)***

### Посчитайте среднее количество обзоров от пользователей, которые поставили больше 50 оценок.


```python
query = ''' SELECT username, COUNT(rating) AS rating_numbers
            FROM ratings
            GROUP BY username
            HAVING  COUNT(rating) >= 50
        '''
pd.io.sql.read_sql(query, con = engine) 
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
      <th>username</th>
      <th>rating_numbers</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>sfitzgerald</td>
      <td>55</td>
    </tr>
    <tr>
      <td>1</td>
      <td>jennifermiller</td>
      <td>53</td>
    </tr>
    <tr>
      <td>2</td>
      <td>xdavis</td>
      <td>51</td>
    </tr>
    <tr>
      <td>3</td>
      <td>paul88</td>
      <td>56</td>
    </tr>
    <tr>
      <td>4</td>
      <td>lesliegibbs</td>
      <td>50</td>
    </tr>
    <tr>
      <td>5</td>
      <td>martinadam</td>
      <td>56</td>
    </tr>
    <tr>
      <td>6</td>
      <td>vanessagardner</td>
      <td>50</td>
    </tr>
    <tr>
      <td>7</td>
      <td>richard89</td>
      <td>55</td>
    </tr>
    <tr>
      <td>8</td>
      <td>shermannatalie</td>
      <td>50</td>
    </tr>
  </tbody>
</table>
</div>



Этим запросом мы нашли пользователей которые поставили больше 50 оценок. Далее применим его в подзапросе.


```python
query = ''' SELECT username, COUNT(review_id)
            FROM reviews
            WHERE username IN (SELECT username
                                FROM ratings
                                GROUP BY username 
                                HAVING COUNT(rating) > 50)
            GROUP BY username
        '''
pd.io.sql.read_sql(query, con = engine) 
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
      <th>username</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>sfitzgerald</td>
      <td>28</td>
    </tr>
    <tr>
      <td>1</td>
      <td>jennifermiller</td>
      <td>25</td>
    </tr>
    <tr>
      <td>2</td>
      <td>xdavis</td>
      <td>18</td>
    </tr>
    <tr>
      <td>3</td>
      <td>paul88</td>
      <td>22</td>
    </tr>
    <tr>
      <td>4</td>
      <td>martinadam</td>
      <td>27</td>
    </tr>
    <tr>
      <td>5</td>
      <td>richard89</td>
      <td>26</td>
    </tr>
  </tbody>
</table>
</div>



Так мы можем видеть сколько обзоров написал каждый.


```python
query = ''' SELECT COUNT(username)/COUNT(DISTINCT username) AS good_rev_avg
            FROM reviews
            WHERE username IN (SELECT username
                                FROM ratings
                                GROUP BY username 
                                HAVING COUNT(rating) >= 50)
        '''
pd.io.sql.read_sql(query, con = engine) 
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
      <th>good_rev_avg</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>24</td>
    </tr>
  </tbody>
</table>
</div>



Получилось что в среднем пользователь поставивший более 50 оценок, пишет 24 обзора.
