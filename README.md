# Домашнее задание к занятию "`Название занятия`" - `Фамилия и имя студента`

---

## Задание 1 СУБД

```
Крупная строительная компания, которая также занимается проектированием и девелопментом, решила создать правильную архитектуру для работы с данными. Ниже представлены задачи, которые необходимо решить для каждой предметной области.

Какие типы СУБД, на ваш взгляд, лучше всего подойдут для решения этих задач и почему?
```

```
1.1. Бюджетирование проектов с дальнейшим формированием финансовых аналитических отчётов и прогнозирования рисков. СУБД должна гарантировать целостность и чёткую структуру данных.
1.1.* Хеширование стало занимать длительно время, какое API можно использовать для ускорения работы?
```

Рекомендуемая СУБД: Реляционная СУБД (например, PostgreSQL или Microsoft SQL Server).
Обоснование: Реляционные СУБД обеспечивают целостность данных и строгую структуру, что критично для финансовых отчетов и прогнозирования рисков. Они поддерживают транзакции и позволяют использовать сложные запросы для анализа данных.
API для ускорения хеширования: Можно рассмотреть использование API для работы с кэшированием, например, Redis или Memcached. Эти инструменты могут значительно ускорить доступ к часто запрашиваемым данным.

```
1.2. Под каждый девелоперский проект создаётся отдельный лендинг, и все данные по лидам стекаются в CRM к маркетологам и менеджерам по продажам. Какой тип СУБД лучше использовать для лендингов и для CRM? СУБД должны быть гибкими и быстрыми.
1.2.* Можно ли эту задачу закрыть одной СУБД? И если да, то какой именно СУБД и какой реализацией?
```

Рекомендуемая СУБД: NoSQL СУБД (например, MongoDB) для лендингов и реляционная СУБД (например, PostgreSQL) для CRM.
Обоснование: NoSQL базы данных обеспечивают гибкость в структуре данных, что полезно для лендингов с изменяющимися требованиями. Реляционная СУБД подходит для CRM благодаря своей способности поддерживать сложные связи между данными.
Можно ли использовать одну СУБД? Да, можно использовать MongoDB как единую СУБД для обеих задач, так как она может хранить как структурированные, так и неструктурированные данные.

```
1.3. Отдел контроля качества решил создать базу по корпоративным нормам и правилам, обучающему материалу и так далее, сформированную согласно структуре компании. СУБД должна иметь простую и понятную структуру.
1.3.* Можно ли под эту задачу использовать уже существующую СУБД из задач выше и если да, то как лучше это реализовать?
```

Рекомендуемая СУБД: Реляционная СУБД (например, PostgreSQL).
Обоснование: Простота структуры реляционной базы данных позволяет легко организовать информацию о корпоративных нормах и правилах.
Можно ли использовать существующую СУБД? Да, можно использовать ту же реляционную базу данных из задачи 1.1. Для этого можно создать отдельную схему или таблицы в рамках одной базы данных.

```
1.4. Департамент логистики нуждается в решении задач по быстрому формированию маршрутов доставки материалов по объектам и распределению курьеров по маршрутам с доставкой документов. СУБД должна уметь быстро работать со связями.
1.4.* Можно ли к этой СУБД подключить отдел закупок или для них лучше сформировать свою СУБД в связке с СУБД логистов?
```

Рекомендуемая СУБД: Графовая СУБД (например, Neo4j) или реляционная СУБД с поддержкой пространственных данных (например, PostgreSQL с расширением PostGIS).
Обоснование: Графовые базы данных отлично подходят для работы со связями между объектами (маршрутами и курьерами). Реляционные базы также могут быть использованы при наличии пространственных запросов.
Можно ли подключить отдел закупок? Да, можно интегрировать отдел закупок в ту же систему логистики при использовании реляционной базы данных. Если используется графовая база данных, то лучше создать отдельную базу для закупок с возможностью интеграции через API.


`1.5.* Можно ли все перечисленные выше задачи решить, используя одну СУБД? Если да, то какую именно?`

Рекомендуемая СУБД: PostgreSQL.
Обоснование: PostgreSQL является мощной реляционной базой данных с поддержкой JSON-данных и расширений (таких как PostGIS), что позволяет ей справляться с различными типами задач: от финансового учета до управления логистикой и хранения норм и правил.
Таким образом, используя одну универсальную реляционную базу данных (например, PostgreSQL), можно решить все перечисленные задачи с учетом их специфики и требований к структуре данных.

---

## Задание 2 Транзакции

```
2.1. Пользователь пополняет баланс счёта телефона, распишите пошагово, какие действия должны произойти для того, чтобы транзакция завершилась успешно. Ориентируйтесь на шесть действий.
2.1.* Какие действия должны произойти, если пополнение счёта телефона происходило бы через автоплатёж?
```

### 2.1. Пошаговые действия для пополнения баланса счёта телефона
1. Идентификация пользователя: Пользователь вводит свои учетные данные (номер телефона, пароль и/или код подтверждения) для доступа к системе пополнения баланса.
2. Выбор способа пополнения: Пользователь выбирает способ пополнения (например, банковская карта, электронный кошелёк, наличные через терминал и т.д.).
3. Ввод суммы пополнения: Пользователь вводит сумму, на которую он хочет пополнить баланс своего телефона.
4. Проверка доступности средств: Система проверяет, достаточно ли средств на выбранном способе оплаты (например, на банковской карте или в электронном кошельке) для завершения транзакции.
5. Обработка транзакции: Если средства доступны, система инициирует транзакцию, отправляя запрос на списание суммы с выбранного способа оплаты и зачисление её на баланс телефона.
6. Подтверждение успешного завершения: После успешного завершения транзакции система уведомляет пользователя о том, что баланс его телефона был успешно пополнен, и отображает обновлённый баланс.

### 2.1.* Действия при автоплатеже
1. Настройка автоплатежа: Пользователь заранее настраивает автоплатёж в системе, указывая сумму и периодичность (например, ежемесячно) пополнения баланса.
2. Идентификация пользователя: Система автоматически идентифицирует пользователя по его учетной записи и привязанным данным (например, номеру телефона).
3. Проверка доступности средств: В день выполнения автоплатежа система проверяет наличие достаточных средств на привязанном способе оплаты (например, банковской карте или счёте).
4. Обработка транзакции: Если средства доступны, система инициирует автоматическую транзакцию для списания указанной суммы с привязанного способа оплаты и зачисления её на баланс телефона.
5. Подтверждение успешного завершения: После успешного завершения транзакции система автоматически уведомляет пользователя о том, что баланс его телефона был успешно пополнен.
6. Запись в историю транзакций: Система сохраняет информацию о проведённом автоплатеже в истории транзакций пользователя для дальнейшего просмотра и анализа.

Таким образом, процесс автоплатежа более автоматизирован и требует меньше действий со стороны пользователя по сравнению с обычным пополнением баланса.


---

## Задание 3 SQL vs NoSQL

### 3.1. Напишите пять преимуществ SQL-систем по отношению к NoSQL.

1. Структурированность данных: SQL-системы используют фиксированную схему данных, что обеспечивает строгую структуру и целостность данных. Это позволяет легко управлять и поддерживать данные, а также гарантирует, что все данные соответствуют заданным правилам.
2. Поддержка транзакций: SQL-системы обеспечивают поддержку ACID-транзакций (Atomicity, Consistency, Isolation, Durability), что гарантирует надежность и целостность операций с данными. Это особенно важно для приложений, где критично важна точность и согласованность данных.
3. Сложные запросы: SQL предоставляет мощный язык запросов, который позволяет выполнять сложные операции с данными, включая объединения (JOIN), подзапросы и агрегации. Это делает SQL-системы более подходящими для аналитических задач и отчетности.
4. Стандартизация: SQL является стандартным языком для работы с реляционными базами данных, что делает его широко распространенным и понятным для разработчиков. Это упрощает обучение и интеграцию различных систем.
5. Инструменты и экосистема: Реляционные базы данных имеют богатую экосистему инструментов для управления, мониторинга и анализа данных, а также множество библиотек и фреймворков для интеграции с различными языками программирования.

### 3.1.* Какие, на ваш взгляд, преимущества у NewSQL систем перед SQL и NoSQL.

1. Скорость обработки: NewSQL системы предлагают высокую производительность при обработке транзакций благодаря использованию современных архитектур (например, распределенных систем), что позволяет им обрабатывать большое количество запросов в реальном времени.
2. Гибкость схемы: Хотя NewSQL системы сохраняют реляционную модель данных, они часто предлагают большую гибкость в управлении схемами по сравнению с традиционными SQL-системами, позволяя адаптироваться к изменяющимся требованиям бизнеса.
3. Поддержка масштабируемости: NewSQL системы разработаны с учетом горизонтальной масштабируемости, что позволяет им эффективно работать в распределенных средах и обрабатывать большие объемы данных без потери производительности.
4. Совмещение преимуществ SQL и NoSQL: NewSQL системы объединяют лучшие черты реляционных баз данных (например, поддержку ACID-транзакций) с возможностями NoSQL систем (например, масштабируемостью и высокой доступностью), что делает их универсальным решением для различных типов приложений.
5. Современные технологии: NewSQL системы часто используют современные технологии хранения данных (например, in-memory хранилища) и оптимизированные алгоритмы обработки запросов, что позволяет достигать высокой производительности при работе с большими объемами данных.

Таким образом, NewSQL системы представляют собой эволюцию традиционных реляционных баз данных, предлагая преимущества как SQL, так и NoSQL решений в одном пакете.



## Задание 4 Кластеры

```
Необходимо производить большое количество вычислений при работе с огромным количеством данных, под эту задачу выделено 1000 машин.
На основе какого критерия будете выбирать тип СУБД и какая модель распределённых вычислений здесь справится лучше всего и почему?
```


При выборе типа СУБД для работы с огромным количеством данных и большим числом вычислений на 1000 машинах, следует учитывать несколько ключевых критериев:

### Критерии выбора СУБД

#### 1. Объем и структура данных:

   1. Если данные структурированы и требуют строгой схемы, лучше выбрать реляционную СУБД (например, PostgreSQL или MySQL).
   2. Если данные неструктурированные или полуструктурированные (например, JSON, XML), то NoSQL базы данных (например, MongoDB, Cassandra) могут быть более подходящими.
#### 2. Требования к производительности:

   1. Для высокопроизводительных операций с данными (например, OLAP-запросы) лучше использовать системы, оптимизированные для аналитики (например, ClickHouse).
   2. Если важна скорость обработки транзакций (OLTP), то стоит рассмотреть NewSQL решения (например, Google Spanner).
#### 3. Масштабируемость:

- Необходимо учитывать возможность горизонтального масштабирования. NoSQL базы данных обычно предлагают лучшую масштабируемость по сравнению с традиционными реляционными СУБД.
#### 4. Поддержка распределенных вычислений:

- Выбор СУБД должен поддерживать распределенные вычисления и быть способным эффективно работать в кластерной среде.
#### 5. Поддержка транзакций:

- Если важна поддержка ACID-транзакций, стоит рассмотреть реляционные или NewSQL базы данных.

#### 6. Сообщество и поддержка:

- Наличие активного сообщества и хорошей документации может значительно упростить процесс разработки и поддержки системы.


### Модель распределённых вычислений
Для обработки больших объемов данных на 1000 машинах наиболее подходящей моделью распределенных вычислений будет MapReduce или его современные аналоги, такие как Apache Spark.


### Почему MapReduce или Apache Spark?

1. Эффективная обработка больших объемов данных:

MapReduce позволяет разбивать задачи на более мелкие подзадачи, которые могут выполняться параллельно на множестве машин. Это особенно полезно при работе с большими наборами данных.

2. Гибкость в обработке данных:

Apache Spark предоставляет более гибкие API для обработки данных в реальном времени и пакетной обработки по сравнению с традиционным MapReduce. Он также поддерживает различные форматы данных и источники.

3. Поддержка различных типов вычислений:

Spark поддерживает не только MapReduce операции, но и SQL-запросы через Spark SQL, машинное обучение через MLlib и графовые вычисления через GraphX.

4. Интерактивные запросы:

В отличие от традиционного MapReduce, который ориентирован на пакетную обработку, Spark позволяет выполнять интерактивные запросы к данным, что может быть полезно для анализа в реальном времени.

5. Оптимизация производительности:

Spark использует in-memory обработку данных, что значительно ускоряет выполнение задач по сравнению с дисковой обработкой в традиционных системах.


Таким образом, выбор типа СУБД будет зависеть от структуры и объема данных, требований к производительности и транзакциям, а модель распределенных вычислений должна обеспечивать эффективную обработку больших объемов данных с возможностью параллельного выполнения задач на множестве машин.