# movie-recommendation-system

Этот проект предоставляет помощь киноманам, которые иногда сталкиваются с трудностями при выборе фильма для просмотра. Он предлагает удобную и простую в использовании функцию, которая позволяет пользователю ввести название своего любимого фильма, а затем автоматически генерирует список из 30 названий фильмов, которые должны понравиться этому пользователю.
Учитывается несколько ключевых факторов: «жанры», «ключевые слова», «слоган», «актеры», «режиссер». Благодаря этому у пользователя появляется возможность обнаружить новые фильмы, которые соответствуют его вкусам и предпочтениям. <br/>

Здесь используется датасет из 4803 фильмов который можно скачать по ссылке: https://drive.google.com/file/d/1cCkwiVv4mgfl20ntgY3n4yApcWqqZQe6/view<br/>
В нем содержатся следующие данные о фильмах: index, budget, genres, homepage, id, keywords, original_languag,e original_title, overview, popularity, production_companies, production_countries, release_date, revenue, 
runtime, spoken_languages, status, tagline, title, vote_average, vote_count, cast, crew director. <br/>

В работе выбирается 5 колонок из этого датасета: 'genres','keywords','tagline','cast','director', комбинируем значения этих колонок для каждого фильма, тем самым, получаем масси из строк, где каждая строка - это название фильма, его жанр, ключевые слова, слоган, актеры, режиссер, записанные через пробел.<br/>

Далее, преобразуем текстовых данные( массив combined_features) в векторы с помощью TfidfVectorizer().<br/>

Теперь, получив значение feature_vectors необходимо оценить их сходства, исходя из этого мы получим на вход название фильма, а на выход - список фильмов, которые похожи на данный.
Существует несколько видов сравнения: <br/>
## 1. Наибольшая общая подпоследовательность
1. Наибольшая общая подпоследовательность (LCS, longest common subsequence) - набор подстрок, входящих в обе строки в одинаковом
порядке. Мы разрешаем подпоследовательности состоять только их целых слов. Для получения меры сходства из наибольшей
общей подпоследовательности достаточно взять отношение удвоенного
количества слов в ней к сумме слов в обеих строках:<br/>
$similarityLCS(s1, s2) = \frac{2 \cdot |LCS(s1, s2)} {|s1| + |s2|}$<br/>
В данной работе для нахождения наибольшей общей подпоследовательности использована реализация алгоритма из библиотеки difflib<br/>
## 2. Косинусное расстояние<br/>
Косинусное расстояние (COS, cosine similarity) измеряет сходство
между текстами как косинус угла между их векторными представлениями <br/>
$similarityCOS(s1, s2) = \frac{\overline{s1} \cdot \overline{s2} }{||\overline{s1}|| \cdot ||\overline{s2}||}$ <br/>

Реализация алгоритма вычисления косинуса между векторными представлениями взята из библиотеки sklearn.metrics.pairwise.<br/>
## 3. Locality-Sensitive Hashing <br/>
Locality-sensitive hashing (LSH — вероятностный метод понижения размерности многомерных данных. Основная идея состоит в таком подборе хеш-функций для некоторых измерений, чтобы похожие объекты с высокой степенью вероятности попадали в одну корзину. Один из способов борьбы с «проклятием размерности» при поиске и анализе многомерных данных, которое заключается в том, что при росте размерности исходных данных поиск по индексу ведёт себя хуже, чем последовательный просмотр. Метод позволяет строить структуру для быстрого приближённого (вероятностного) поиска n-мерных векторов, «похожих» на искомый шаблон.

LSH является одним из наиболее популярных на сегодняшний день приближённых алгоритмов поиска ближайших соседей (Approximate Nearest Neighbor, ANN). LSH в этом подходе отображает множество точек в высокоразмерном пространстве в множество ячеек, т. е. в хеш-таблицу. В отличие от традиционных хешей, LSH обладает свойством чувствительности к местоположению (locality-sensitive hash), благодаря чему способен помещать соседние точки в одну и ту же ячейку.<br/>
## 4.Расстояние Левенштейна <br/>
Расстояние Левенштейна (LEV, Levenshtein distance) — это минимальное количество вставок, удалений и замен символов, необходимых
для преобразования одной строки в другую . Для преобразования
расстояния в меру сходства надо нормализовать его максимально возможным значением — максимумом длин двух строк — и вычесть из 1.
Это же значение можно получить из отношения длины наибольшей
общей подпоследовательности к максимуму длин строк <br/>
$similarityLEV (s1, s2) = 1 − \frac {LEV (s1, s2)} {max(|s1|, |s2|)} = \frac{LCS(s1, s2)}{max(|s1|, |s2|)}$ <br/>

Для вычисления расстояния Левенштейна используется алгоритм поиска наибольшей общей подпоследовательности из
библиотеки difflib.<br/>
### Наш выбор
В нашем случае, мы сравниваем данные с помощью cosine similarity. Получаем массив , сортируем его и выводим значения, т.е названия фильмов, которые больше всего похожи на данный фильм, учитывая наш запрос. <br/>
![Image alt](https://github.com/sitoramonova/movie-recommendation-system/blob/main/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-03-11%20%D0%B2%2003.04.25.png)

### Ранжирование фильмов:
Использование дополнительных признаков: Mожем использовать дополнительные признаки для расчета весов или значимости каждого фильма. Например, можно учитывать рейтинг фильма, количество оценок, год выпуска и другие метаданные, чтобы сделать рекомендации более релевантными.
Теперь, давайте немного изменим наш запрос. Не будем учитывать режиссеров фильма, но учтем рейтинг и дату выхода. и посмотрим, как изменится вывод.<br/> 
![Image alt](https://github.com/sitoramonova/movie-recommendation-system/blob/main/m_merged.png) <br/>

Сравним наш результат с результат рекомендационной системы Кинопоиска. Вот, фильмы, рекомендованные пользователям, которым понравился фильм «Поймай меня, если сможешь»: <br/>
The Wolf of Wall Street<br/>
Public Enemies<br/>
The Departed<br/>
The Talented Mr. Ripley<br/>
White Collar<br/>
Blow<br/>
I Love You Phillip Morris<br/>
The Terminal<br/>
The Aviator,<br/>
The Thomas Crown Affair,<br/>
Ocean's Eleven<br/>
Matchstick Men<br/>
Now You See Me<br/>
American Made,<br/>
Confessions of a Dangerous Mind,<br/>
Plein soleil,<br/>
Совпали следующие фильмы: "The Wolf of Wall Street", "The Departed", "The Aviator"<br/>
А сравнивая с выводом, где учитывался режисер, совпадения: "The Departed", "The Aviator", "American Hustle", "Gangs of New York". <br/>
Если же учитывать и режисера, и рейтинг с датой выходы, совпадения следующие: "The Departed", "The Wolf of Wall Street", "The Aviator", "Saving Private Ryan".


UPDATE
Изменила dataset из 4803  на dataset из 45463 фильмов
он был скачен из kaggle, преобразован в более удобный формат

в коде были изменены названия ячеек из датасета, которые были выбраны для реализации рекомендационной системы
были выбраны 'genres', 'vote_average', 'popularity', 'production_companies', 'release_date', 'vote_average', 'imdb_id'

новый вывод программы:

1 . Moll Flanders
2 . The Fortunes and Misfortunes of Moll Flanders
3 . The Human Experience
4 . The Blue Veil
5 . The Telephone Book
6 . The Wooden Man's Bride
7 . The Gift
8 . The Thief Lord
9 . Storm Over Asia
10 . Twixt
11 . Charlotte Gray
12 . Films to Keep You Awake: The Christmas Tale
13 . American Winter
14 . The Best Years of Our Lives
15 . Project: Shadowchaser
16 . Christmas in Connecticut
17 . Only Yesterday
18 . Morning Star
19 . Tommy's Honour
20 . Places in the Heart
21 . Plan 9
22 . The Man from Snowy River
23 . The Reader
24 . María Candelaria
25 . Homeland (Iraq Year Zero)
26 . The Crown Jewels
27 . Stalingrad
28 . ...ing
29 . The Chase

## Вывод.
Наша рекомендационная система, основанная на методе cosine similarity, с новым датасетом, не имеет совпадений с рекомендациям КиноПоиска.
Это может быть связано с тем, что датасет слишком большой и данные в ячейках не совсем подходят для векторизации.


