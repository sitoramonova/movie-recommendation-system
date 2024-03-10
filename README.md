# movie-recommendation-system
![Image alt](https://github.com/sitoramonova/movie-recommendation-system/blob/main/%D0%B4%D0%B8%20%D0%BA%D0%B0%D0%BF%D1%80%D0%B8%D0%BE.png)

Этот проект предоставляет помощь киноманам, которые иногда сталкиваются с трудностями при выборе фильма для просмотра. Он предлагает удобную и простую в использовании функцию, которая позволяет пользователю ввести название своего любимого фильма, а затем автоматически генерирует список из 30 названий фильмов, которые должны понравиться этому пользователю.
Учитывается несколько ключевых факторов: «жанры», «ключевые слова», «слоган», «актеры», «режиссер». Благодаря этому у пользователя появляется возможность обнаружить новые фильмы, которые соответствуют его вкусам и предпочтениям. <br/>

Здесь используется датасет из 4803 фильмов который можно скачать по ссылке: https://drive.google.com/file/d/1cCkwiVv4mgfl20ntgY3n4yApcWqqZQe6/view<br/>

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

Реализация алгоритма вычисления косинуса между векторными представлениями взята из библиотеки SciPy.
