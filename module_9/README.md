# Проект 9. Конкурс «Alice» - идентификация пользователя в Интернете по последовательности переходов по сайтам.   
##  Open ML Course: Линейные модели (Весна 2022)   
### ODS Open ML Course  
![https://img.shields.io/badge/Python-3.8.5-blue](https://img.shields.io/badge/Python-3.8.5-blue)

## Оглавление  
[1. Описание проекта](https://github.com/alex-sokolov2011/skillfactory_rds/blob/master/module_9/README.md#Описание-проекта)  
[2. Постановка задачи](https://github.com/alex-sokolov2011/skillfactory_rds/blob/master/module_9/README.md#Постановка-задачи)  
[3. Краткая информация о данных](https://github.com/alex-sokolov2011/skillfactory_rds/blob/master/module_9/README.md#Краткая-информация-о-данных)  
[4. Этапы работы над проектом](https://github.com/alex-sokolov2011/skillfactory_rds/blob/master/module_9/README.md#Этапы-работы-над-проектом)  
[5. Результат](https://github.com/alex-sokolov2011/skillfactory_rds/blob/master/module_9/README.md#Результат)  

### Описание проекта  
В этом [конкурсе](https://ods.ai/tracks/linear-models-spring22/competitions/alice) нужно определить конкретного пользователя сети Интернет по сессии посещения веб-сайтов. В каждой сессии может быть от 1 до 10 сайтов – количество сайтов ограничено длиной сессии. Почему конкурс называется «Alice»?  

Во-первых, так назвали конкурс его создатели. Да, конкурс уже достаточно давно используется в курсе mlcourse.ai его создателем Юрой Кашницким @Yorko. Он любезно предоставил данные для перезапуска конкурса на платформе ODS, чтобы курсы, такие как «Линейные модели» и «Открытый курс машинного обучения», могли переиспользовать конкурс.  

Во-вторых, Alice – это «обычное имя», например, в криптографии говорят Alice вместо «Пользователь А» и Bob вместо «Пользователь Б». Все равно как мы назовем пользователя, которого мы ищем, или «target = 1», поэтому давайте именно его, вернее, её назовём Alice. Будем искать Alice!  

Данные для конкурса собраны с прокси-серверов Университета Блеза Паскаля и взяты из [статьи](http://ceur-ws.org/Vol-1703/paper12.pdf), которая описывает методы поиска Alice. Если хочется посмотреть на все научные подходы к решению, есть [книга](http://www.charuaggarwal.net/freqbook.pdf). Современные материалы ищите в сети по словам "Traversal Pattern Mining" и "Sequential Pattern Mining".  

А мы будем решать задачу методами машинного обучения как задачу классификации.  
:arrow_up:[к оглавлению](https://github.com/alex-sokolov2011/skillfactory_rds/blob/master/module_9/README.md#Оглавление)

### Постановка задачи
Для каждой сессии нужно предсказать, принадлежит ли сессия Alice (метка «1»), или нет (метка «0»).  

**Условия соревнования:**  
[Ссылка соревнование](https://ods.ai/competitions/alice)  

- в день можно отправить 3 решения.
- целевая метрика – ROC AUC. Пример ее описания [тут](https://habrahabr.ru/post/228963/).
- на лидерборде рейтинг участников будет рассчитываться по подвыборке ответов из тестовых данных. Конкурс заканчивается 05.06.2022 23:59:59 МСК. В это время закроется возможность отправки решений. Окончательная оценка решений пройдет по оставшейся части ответов из тестовых данных. Таким образом, ваш рейтинг может поменяться, если вы переобучились. Лучшие 15 решений должны будут предоставить ноутбуки с решениями для проверки и формирования окончательного списка победителей.  

### Краткая информация о данных

В обучающей выборке train.csv:  
- site_i – это индексы посещенных сайтов (расшифровка дана в pickle-файле со словарем site_dic.pkl)  
- time_i – время посещения сайтов site_i  
  
В проверочной выборке есть целевой признак target – факт того, что сессия принадлежит Alice.  
Я создал [клон данных на каггл](https://www.kaggle.com/datasets/sokolovaleks/open-ml-course-linear-models-spring22), чтобы участникам удобнее было стартовать на kaggle  
:arrow_up:[к оглавлению](https://github.com/alex-sokolov2011/skillfactory_rds/blob/master/module_9/README.md#Оглавление)

### Этапы работы над проектом  

- в этом конкурсе я принял участие, чтобы получше разобраться с Линейными моделями. К конкурсу я подошел уже имея годовой опыт в продуктовом DS. Но в основном я использовал бустинги и LAMA. Линейные модели использовал только для стекинга моделей в LAMA. Линейки быстры на проде и хорошо интерпретируемы, поэтому я решил в них подразобраться получше и на сдачу построить несколько пайплайнов для быстрого моделирования и анализа с помощью линеек.  
- в ходе EDA выяснил как привести датасет к датасету похожего [соревнования](https://www.kaggle.com/competitions/catch-me-if-you-can-intruder-detection-through-webpage-session-tracking2) и далее использовал наработки по EDA оттуда. В результате создал вводный [ноутбук](https://www.kaggle.com/code/sokolovaleks/linearmodels-openmlcourse-spr22-hw-plus-eda) с EDA для участников [чата](https://t.me/c/1716413804/2079) по сореву для ориентации в датасете.  
- использовал только LogReg sklear и хотел поиспользвать инструменты, которые раньше не использовал.  
- не использовал в этом решении Feature Important и Feature Permutation, потому что использовал их в Альфабатле 2.0, да и в работе использую их часто поэтому выбрал SequentialFeatureSelector, чтобы попробовать новые методы отбора.  
- для кросс-валидации использовал TimeSeriesSplit и StratifiedKFold (средние, дисперсию и геометрическое среднее). Какой-то стабильной системы для верификации не сложилось, поэтому валидировался об ЛБ и уже дальше смотрел на статистику по всем ключевым метрикам CV.  
- использовал TfidfVectorizer на (1,5) и 50000. Предварительно обработал поле названий сайтов - удалил знаки препинания и т.п.  
- сгенерировал около 60 новых признаков. Дальше я сформировал три множества признаков (так чтобы в них были нескореллированные признаки). Прогнал их через SequentialFeatureSelector. Каждый чанк обрабатывался примерно 1.5 часа. Дальше из них сделал пересечение. Убрал пересечение из трех множеств и прогнал еще раз. Сформировал второе по значимости пересечение. Потом проводил эксперименты добавляя и удаляя признаки по одному.  
  
:arrow_up:[к оглавлению](https://github.com/alex-sokolov2011/skillfactory_rds/blob/master/module_9/README.md#Оглавление)

### Результат:  
score на ods = 0,966855 [LB Private](https://ods.ai/competitions/alice/leaderboard/private) (0,972716 [LB Public](https://ods.ai/tracks/linear-models-spring22/competitions/alice/leaderboard) (-0.005 - переобучился)) 11 место на Private из 410 участников  
score на kaggle = 0.97008 . 29 место из 5000 участников. [LB Public](https://www.kaggle.com/competitions/catch-me-if-you-can-intruder-detection-through-webpage-session-tracking2/leaderboard)

Из-за моей проф деформации (работал раньше учителем математики старших классов) по ходу сорева подготовил материалы, которые будут полезны для новичков и по согласованию с оргом частями подавал их в чат, чтобы участники сорева могли шаг за шагом продвигаться в решении задачи и повышать скор:
1. [Домашка + EDA для формирования признаков](https://www.kaggle.com/code/sokolovaleks/linearmodels-openmlcourse-spr22-hw-plus-eda)
2. [Оригинальный бейзлайн](https://www.kaggle.com/code/sokolovaleks/linearmodels-openmlcourse-spr22-baseline)
3. [Бейзлайн 2 с примерами добавления фич](https://www.kaggle.com/code/sokolovaleks/linearmodels-openmlcourse-spr22-baseline-2)
4. [Бейзлайн 3 с CV](https://www.kaggle.com/code/sokolovaleks/linearmodels-openmlcourse-spr22-baseline-3-withcv)
5. [Бейзлайн 4 с GreadSearch](https://www.kaggle.com/code/sokolovaleks/linearmodels-openmlcourse-spr22-baseline4-gsearch)

Также пособирал из чата разные идеи/вопросы на разбор может пригодятся ([тут](https://docs.google.com/spreadsheets/d/11BzQ4wrKjaS4nXmHoWHw2pyBql2urV3Barirln1cL1M/edit?usp=sharing)). Думал будут добавлять вопросики, но тема как-то не пошла.  
Решение не привожу по просьбе организаторов соревнования (если будут вопросы пишите)

К сожалению не хватило времени, чтобы:
- попробовать Feature Selection на l1 saga  
  
:arrow_up:[к оглавлению](https://github.com/alex-sokolov2011/skillfactory_rds/blob/master/module_9/README.md#Оглавление)

Если информация по этому проекту покажется вам интересной или полезной, то я буду очень вам благодарен, если отметите репозиторий и профиль ⭐️⭐️⭐️-дами