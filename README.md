Задание 2

Систему сплитования я выделил через города и интервалы между квантилями ratio, предварительно добавив в наш датафрейм колонку ratio (numerator/denuminator).

Почему? 
Поскольку города это важная особенность, и у каждого региона есть свои характерные черты (уровень зарплат, плотность, население, процент рабочего населения и тд)
На интервалы также важно разделить, потому что не объективно сравнивать между собой точки, у которых абсолютно разное соотношение numerator/denuminator. Решением стало разбивка на интервалы между квантилями (0-25, 25-50 и тд)


Основная задача на данном этапе — разработать и проверить систему сплитования, чтобы в дальнейшем можно было провести A/B-тестирование. 
Если честно, я не понял, что означает “чтобы можно было провести АВ  со 2 апреля 2024.”, поскольку “2024-03-31” это максимальная дата в наших данных. Если речь о том, чтобы выкатить изменения 2 апреля и это никак не связано с моим заданием, то едем дальше.

Здесь должен быть ipynb файл, но у меня не получилось его вставить. Прикрепил его отдельно.


Статистический тест.
Для выбора теста нужно проверить нормальность распределения, распределение получилось не очень нормальным (было проверено визуально через гистограмму и через qq plot)
Я также попробовал взять логарифм, поскольку очень много было выбросов нулевых значений, я подумал, что выкидывать их нельзя и попытался взять логарифм + 1 от каждого значения, но почему то все равно на гистограмме было достаточно выбросов слева = 0, скорее всего я что-то не понял.

В таком случае я решил использовать тест Манна-Уитни, потому что для данных с большим количеством нулевых значений обычно предпочтительнее использовать непараметрические тесты, поскольку они не требуют предположения о нормальности распределения данных. 
Тест Манна-Уитни особенно полезен, когда распределение данных сильно отличается от нормального.
По результатам теста, который не показал статистически значимых различий между группами (U-статистика: 387902015.0
P-значение: 0.811715744156799
Нет статистически значимых различий между выборками), можно сделать вывод, что система сплитования работает корректно. Это означает, что группы были разделены таким образом, что они статистически идентичны по ключевым параметрам, что является хорошим основанием для проведения A/B-теста.
Также признаюсь честно, размер конечного датафрейма ab_test_groups 
(55739 rows × 9 columns), а изначального (55770 rows × 6 columns) 
Очень долго искал, куда делось 31 значение, но так и не нашел.




A/B тест
Далее команда внедрила какие-либо изменения и ab тест начался.

У меня не было вводных данных, о каких изменениях идет речь, что мы за сайт, какой у нас трафик и тд. Поэтому, я могу сказать только общую информацию.
В зависимости от того насколько  большой эффект от изменений мы ожидаем, результаты могут проявиться быстрее. Для малозаметных изменений потребуется больше времени, чтобы увидеть различия.
Также скорость набора данных зависит от трафика дарксторов. Чем выше трафик, тем быстрее можно будет собрать необходимый объем данных.
Стабильность метрик: Важно, чтобы метрики, на которые мы нацелены,  были стабильны и не подвержены сезонным или внешним колебаниям, которые могут исказить результаты. 
Очень аккуратно нужно подходить к отслеживанию например в октябре, когда на момент начала эксперимента погода благоприятная, но в ноябре начинает выпадать снег. Это способствует увеличению времени доставки заказа. Люди в целом меньше хотят выходить на улицу в плохую погоду и тд. Очень много важных факторов. Поэтому нужно выбрать максимально однородный сезон, если так можно выразиться. (например середина лета или зимы, когда не происходит резких изменений в климате) 
Безусловно аномалии всегда возможны. При столкновении с ними, стоит их учитывать и продлить или наоборот прекратить тестирование.
В среднем, A/B-тесты могут длиться от нескольких дней до нескольких недель. Однако, для получения наиболее точных и надёжных результатов, рекомендуется проводить тестирование минимум 2-4 недели. Таким образом мы уберем случайные колебания и сможем увидеть реальное влияние изменений.
Знаю, что существует формула выборка / трафик = количество дней. 




