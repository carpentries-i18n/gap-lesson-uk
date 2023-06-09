---
title: "Перша сесія з GAP"
teaching: 30
exercises: 10
questions:
- "Робота з командним рядком GAP"
objectives:
- "Поради та підказки, які заощадять час"
- "Використання довідкової системи GAP"
- "Базові об'єкти та конструкції в мові GAP"
keypoints:
- "Пам’ятайте, що GAP чутливий до регістру!"
- "Не панікуйте, якщо побачите `Error, Variable: 'FuncName' must have a value`."
- "Звертайте увагу на імена змінних і функцій."
- "Використовуйте редагування командного рядка."
- "Використовуйте автозаповнення замість повного введення імен функцій і змінних."
- "Використовуйте `?` та `??`, щоб переглянути сторінки довідки."
- "Встановіть HTML в якості стандартного формату довідки за допомогою `SetHelpViewer`."
- "Використовуйте функцію `LogTo`, щоб зберегти всі введення та виведення GAP у текстовий файл."
- "Якщо обчислення триває занадто довго, натисніть <Control>-C, щоб перервати його."
- "Прочитайте «Перше заняття з GAP» у підручнику з GAP."
---

Якщо GAP встановлено правильно, ви повинні мати можливість його запустити. Як саме
це зробити, залежатиме від вашої операційної системи та способу встановлення
GAP. Після запуску, GAP виведе на екран свій *банер*, який відображає інформацію про версію
системи та завантажені компоненти, а потім запрошення командного рядка
`gap>`, наприклад:

~~~
 ┌───────┐   GAP 4.9.2 of 04-Jul-2018
 │  GAP  │   https://www.gap-system.org
 └───────┘   Architecture: x86_64-apple-darwin16.7.0-default64
 Configuration:  gmp 6.1.2, readline
 Loading the library and packages ...
 Packages:   AClib 1.3, Alnuth 3.1.0, AtlasRep 1.5.1, AutPGrp 1.9, 
             Browse 1.8.8, CRISP 1.4.4, Cryst 4.1.17, CrystCat 1.1.8, 
             CTblLib 1.2.2, FactInt 1.6.2, FGA 1.4.0, GAPDoc 1.6.1, IO 4.5.1, 
             IRREDSOL 1.4, LAGUNA 3.9.0, Polenta 1.3.8, Polycyclic 2.14, 
             PrimGrp 3.3.1, RadiRoot 2.8, ResClasses 4.7.1, SmallGrp 1.3, 
             Sophus 1.24, SpinSym 1.5, TomLib 1.2.6, TransGrp 2.0.2, 
             utils 0.54
 Try '??help' for help. See also '?copyright', '?cite' and '?authors'
gap> 
~~~
{: .output}

Щоб вийти з GAP, введіть `quit;` у командному рядку GAP. Пам’ятайте, що всі команди GAP,
включно з цією, мають закінчуватися крапкою з комою! Потренуйтеся вводити
`quit;`, щоб вийти з GAP, а потім починати новий сеанс GAP. Перш ніж продовжити, ви
можливо забажаєте ввести наступну команду, щоб відображати запрошення GAP та команди, введені користувачем
у різних кольорах:

~~~
 ColorPrompt(true);
~~~
{: .source}

Найпростіший шлях розпочати роботу з GAP - це використовувати GAP як калькулятор:

~~~
( 1 + 2^32 ) / (1 - 2*3*107 );
~~~
{: .source}

~~~
-6700417
~~~
{: .output}

Якщо ви хочете записати те, що ви робили під час сеансу GAP, щоб ви могли переглянути це
 пізніше, ви можете ввімкнути ведення журналу за допомогою функції `LogTo`, як наведено далі.

~~~
LogTo("gap-intro.log");
~~~
{: .source}

Це створить файл `gap-intro.log` у поточному каталозі, який
міститиме всі подальші вхідні та вихідні дані, які з’являтимуться у вашому терміналі.
Щоб припинити ведення журналу, ви можете викликати `LogTo` без аргументів, як у `LogTo();`,
або залишити GAP. Зауважте, що `LogTo` очищає файл перед запуском, якщо він
уже існує!

Може бути корисним залишити кілька коментарів у файлі журналу на випадок,
якщо ви повернетеся до нього в майбутньому. Коментар у GAP починається з символу `#` і
продовжується до кінця рядка. Ви можете ввести наступне після
підказки GAP:

~~~
# Урок Software Carpentry GAP
~~~
{: .source}

тоді після натискання клавіші Return GAP відобразить нову підказку, але коментар
буде записаний у файл журналу.

Файл журналу записує всю взаємодію з GAP, яка відбувається після виклику
`LogTo`, але не раніше. Ми можемо повторити наші обчислення вище,
якщо ми також хочемо їх записати. Замість того, щоб вводити їх повторно, ми будемо використовувати клавіші зі стрілками вгору та вниз
для прокручування *історії командного рядка*. Повторюйте це, доки знову не побачите
формулу, потім натисніть Return (розташування курсору в командному
рядку не має значення):

~~~
( 1 + 2^32 ) / (1 - 2*3*107 );
~~~
{: .source}

~~~
-6700417
~~~
{: .output}

Ви також можете редагувати команди, що вже існують. Натисніть клавішу «Вгору» ще раз, а потім використовуйте
клавіші зі стрілками вліво та вправо, Delete або Backspace, щоб відредагувати їх та замінити 32 на 64 (інші корисні комбінації клавіш —
Ctrl-A та Ctrl-E, щоб перемістити курсор на початок і кінець
рядку, відповідно). Тепер натисніть клавішу Return (у будь-якій позиції
курсору в командному рядку):

~~~
( 1 + 2^64 ) / (1 - 2*3*107 );
~~~
{: .source}

~~~
-18446744073709551617/641
~~~
{: .output}

Корисно знати, що якщо історія командного рядка велика, можна
виконати частковий пошук, ввівши початкову частину команди, а потім використовуючи
клавіші зі стрілками вгору та вниз, щоб прокрутити лише ті рядки, які починаються
з введених символів.

Якщо ви бажаєте зберегти значення для подальшого використання, ви можете присвоїти йому ім'я
за допомогою `:=`

~~~
universe := 6*7;
~~~
{: .source}

> ## `:=`, `=` і `<>`
>
> * В інших мовах Ви можете бути більш знайомі з використанням `=`, щоб присвоювати значення
> змінним, але GAP використовує `:=`.
>
> * GAP використовує `=` для порівняння, якщо дві об'єкти однакові (де інші мови можуть
>  використовувати `==`).
>
> * Нарешті, GAP використовує `<>`, щоб перевірити, чи два об'єкти не рівні (замість `!=`, що
>  ви могли бачити раніше).
{: .callout}

Пробільні символи (тобто пробіли, табуляції та символи переводу рядка) не мають значення в GAP,
за винятком випадків, коли вони знаходяться всередині рядка. Наприклад, попереднє введеня
можна ввести без пробілів:

~~~
(1+2^64)/(1-2*3*107);
~~~
{: .source}

~~~
-18446744073709551617/641
~~~
{: .output}

Пробіли часто використовуються для форматування більш складних команд
для кращої читабельності. Наприклад, наступне введення, яке створює матрицю 3×3:

~~~
m:=[[1,2,3],[4,5,6],[7,8,9]];
~~~
{: .source}

~~~
[ [ 1, 2, 3 ], [ 4, 5, 6 ], [ 7, 8, 9 ] ]
~~~
{: .output}

Замість цього ми можемо записати нашу матрицю в 3 рядки. У цьому випадку замість повної підказки
`gap>` відображатиметься часткова підказка `>`, доки користувач не завершить
введення крапкою з комою:

~~~
gap> m:=[[ 1, 2, 3 ],
>        [ 4, 5, 6 ],
>        [ 7, 8, 9 ]];
~~~
{: .source}

~~~
[ [ 1, 2, 3 ], [ 4, 5, 6 ], [ 7, 8, 9 ] ]
~~~
{: .output}

Ви можете використовувати `Display` для красивого друку змінних, включаючи цю матрицю:

~~~
Display(m);
~~~
{: .source}

~~~
[ [  1,  2,  3 ],
  [  4,  5,  6 ],
  [  7,  8,  9 ] ]
~~~
{: .output}

Загалом функції GAP, як наприклад `LogTo` і `Display`, викликаються за допомогою дужок,
які містять (можливо, порожній) список аргументів.

> ## Функції також є об’єктами GAP
>
> Перевірте, що станеться, якщо ви забудете додати дужки,
> наприклад, введіть `LogTo;` і `Factorial;`
> Пізніше ми пояснимо відмінності в цих результатах.
{: .callout}

Нижче наведені декілька прикладів виклику інших функцій GAP:

~~~
Factorial(100);
~~~
{: .source}

~~~
93326215443944152681699238856266700490715968264381621468\
59296389521759999322991560894146397615651828625369792082\
7223758251185210916864000000000000000000000000
~~~
{: .output}

(точна ширина виводу залежатиме від налаштувань вашого терміналу),

~~~
Determinant(m);
~~~
{: .source}

~~~
0
~~~
{: .output}

та

~~~
Factors(2^64-1);
~~~
{: .source}

~~~
[ 3, 5, 17, 257, 641, 65537, 6700417 ]
~~~
{: .output}

Функції можна комбінувати різними способами та
використовувати як аргументи інших функцій, наприклад,
функція `Filtered` приймає список і функцію, повертаючи
всі елементи списку, які задовольняють функцію.
`IsEvenInt` ("Is Even Integer" з англ. "чи є ціле число парним"), як не дивно, перевіряє, чи є ціле число парним!

~~~
Filtered( [2,9,6,3,4,5], IsEvenInt);
~~~
{: .source}

~~~
[ 2, 6, 4 ]
~~~
{: .output}


Корисною функцією інтерфейсу командного рядка GAP, яка економить час, є заповнення
ідентифікаторів під час натискання клавіші Tab. Наприклад, введіть `Fib`, а потім
натисніть клавішу Tab, щоб завершити введення `Fibonacci`:

~~~
Fibonacci(100);
~~~
{: .source}

~~~
354224848179261915075
~~~
{: .output}

У випадку, якщо унікальне доповнення неможливе, GAP спробує виконати
часткове доповнення, а натискання клавіші Tab вдруге відобразить усі можливі
доповнення ідентифікатора. Спробуйте, наприклад, ввести `GroupHomomorphismByImages`
або `NaturalHomomorphismByNormalSubgroup` за допомогою доповнення.

Сподіваємось, те, як функції називаються в GAP, допоможе вам запам’ятовувати або навіть вгадувати назви
бібліотечних функцій. Якщо назва змінної складається з кількох слів,
то перша літера кожного слова пишеться з великої літери (пам’ятайте, що GAP чутливий до регістру!).
Подальші відомості про правила іменування, які використовуються в GAP,
задокументовані в посібнику GAP [тут](http://www.gap-system.org/Manuals/doc/ref/chap5.html#X81F732457F7BC851).
Функції з назвами `У_ВЕРХНЬОМУ_РЕГІСТРІ` є внутрішніми функціями, не призначеними
для загального використання. Використовуйте їх з особливою обережністю!

Важливо пам’ятати, що GAP чутливий до регістру. Наприклад, наступне
введення викликає помилку:

~~~
factorial(100);
~~~
{: .source}

~~~
Error, Variable: 'factorial' must have a value
not in any function at line 14 of *stdin*
~~~
{: .error}

тому що назва функції бібліотеки GAP – `Factorial`. Використання малих літер
замість великих або навпаки також впливає на доповнення назви.

Тепер давайте розглянемо таку задачу: для скінченної групи _G_ обчислити
середній порядок її елементів (тобто суму порядків її елементів, поділену
на порядок групи). З чого почати?

Введіть `?Group`, і ви побачите всі записи довідкової системи, що починаються з `Group`:

~~~
┌──────────────────────────────────────────────────────────────────────────────┐
│   Choose an entry to view, 'q' for none (or use ?<nr> later):                │
│[1]    AutoDoc (not loaded): @Group                                           │
│[2]    loops (not loaded): group                                              │
│[3]    polycyclic: Group                                                      │
│[4]    RCWA (not loaded): Group                                               │
│[5]    Tutorial: Groups and Homomorphisms                                     │
│[6]    Tutorial: Group Homomorphisms by Images                                │
│[7]    Tutorial: group general mapping                                        │
│[8]    Tutorial: GroupHomomorphismByImages vs. GroupGeneralMappingByImages    │
│[9]    Tutorial: group general mapping single-valued                          │
│[10]   Tutorial: group general mapping total                                  │
│[11]   Reference: Groups                                                      │
│[12]   Reference: Group Elements                                              │
│[13]   Reference: Group Properties                                            │
│[14]   Reference: Group Homomorphisms                                         │
│[15]   Reference: GroupHomomorphismByFunction                                 │
│[16]   Reference: Group Automorphisms                                         │
│[17]   Reference: Groups of Automorphisms                                     │
│[18]   Reference: Group Actions                                               │
│[19]   Reference: Group Products                                              │
│[20]   Reference: Group Libraries                                             │
│ > > >                                                                        │
└─────────────── [ <Up>/<Down> select, <Return> show, q quit ] ────────────────┘
~~~
{: .output}

Ви можете використовувати клавіші зі стрілками для переміщення вгору та вниз по списку, а також відкривати сторінки довідки,
натискаючи клавішу Return. Для цієї вправи спочатку відкрийте `Tutorial: Groups and Homomorphisms`.
Зверніть увагу на навігаційні інструкції внизу екрана. Перегляньте
перші дві сторінки, потім натисніть `q`, щоб повернутися до меню вибору. Далі перейдіть до елементу
`Reference: Groups` і відкрийте його. На перших двох сторінках ви знайдете
функцію `Group` та згадку `Order`.

Посібник GAP доступний у кількох форматах: текстовий зручний для перегляду в терміналі,
PDF зручний для друку, а HTML (особливо з підтримкою MathJax)
дуже ефективний для перегляду за допомогою браузера. Якщо ви
використовуєте GAP на віддаленій машині, це (імовірно) не працюватиме. (див.
`?WriteGapIniFile` про те, як зробити це налаштування постійним):

~~~
SetHelpViewer("browser");
~~~
{: .source}

Після цього викличте довідку знову та побачите різницю!

Давайте тепер скопіюємо наступні вхідні дані з першого прикладу глави довідкового посібника GAP
про групи. У ньому показано, як створювати перестановки та присвоювати значення
змінним. Це `Reference: Groups`. Ви можете вибрати його, ввівши `?11`, де
ви заміните `11` на те значення, яке з’явиться перед `Reference: Groups` на вашому комп'ютері.

Якщо ви переглядаєте документацію GAP у терміналі, вам може бути корисно
відкрити дві копії GAP, одну для читання документації та одну для написання коду!

Цей посібник показує, як перестановки в GAP записуються в циклічній нотації, а також
показує загальні функції, які використовуються з групами. Крім того, у деяких місцях використовуються дві крапки з комою
в кінці рядка. Це не дозволить GAP показувати результат обчислення.

~~~
a:=(1,2,3);;b:=(2,3,4);;
~~~
{: .source}

Далі, нехай `G` - група, утворена за допомогою `a` та `b`:

~~~
G:=Group(a,b);
~~~
{: .source}

~~~
Group([ (1,2,3), (2,3,4) ])
~~~
{: .output}

Ми можемо дослідити деякі властивості групи `G` та її породжувачів:

~~~
Size(G); IsAbelian(G); StructureDescription(G); Order(a);
~~~
{: .source}

~~~
12
false
"A4"
3
~~~
{: .output}

Нашим наступним завданням є з'ясувати, як отримати список елементів `G` та їх порядок.
Введіть `?elements` і перегляньте список тем довідки. Після перевірки запис у Tutorial не здається актуальним, але  є запис у
довідковому посібнику (Reference). Тут також пояснюється різниця між використанням `AsSSortedList`
і `AsList`. Отже, це список елементів `G`:

~~~
AsList(G);
~~~
{: .source}

~~~
[ (), (2,3,4), (2,4,3), (1,2)(3,4), (1,2,3), (1,2,4), (1,3,2), (1,3,4),
  (1,3)(2,4), (1,4,2), (1,4,3), (1,4)(2,3) ]
~~~
{: .output}

Повернений об’єкт є _списком_. Ми хотіли б призначити його змінній
для дослідження та повторного використання. Ми забули це зробити, коли обчислювали. Звичайно,
ми можемо використовувати історію командного рядка, щоб відновити останню команду, відредагувати
 її та викликати знову. Але замість цього ми будемо використовувати `last`, яка є спеціальною змінною,
що містить останній результат, повернутий GAP:

~~~
elts:=last;
~~~
{: .source}

~~~
[ (), (2,3,4), (2,4,3), (1,2)(3,4), (1,2,3), (1,2,4), (1,3,2), (1,3,4),
  (1,3)(2,4), (1,4,2), (1,4,3), (1,4)(2,3) ]
~~~
{: .output}

Це список. Списки в GAP індексуються від 1.
Наступні команди (сподіваємося!) не потребують пояснень:

~~~
gap> elts[1]; elts[3]; Length(elts);
~~~
{: .source}

~~~
()
(2,4,3)
12
~~~
{: .output}

> ## Списки — це більше, ніж масиви
>
> * Може містити дірки або бути порожнім
>
> * Може динамічно змінювати їх довжину (за допомогою `Add`, `Append` або прямого призначення)
>
> * Не обов’язково містить об’єкти одного типу
>
>
> * Дивіться більше в [GAP Tutorial: Lists and Records](http://www.gap-system.org/Manuals/doc/tut/chap3.html)
{: .callout}

Багато функцій у GAP посилаються на `Множини`. Множина у GAP — це лише список,
який не має повторень, жодних дірок і елементів у порядку зростання. Ось кілька прикладів:

~~~
gap> IsSet([1,3,5]); IsSet([1,5,3]); IsSet([1,3,3]);
~~~
{: .source}

~~~
true
false
false
~~~
{: .output}

Тепер давайте розглянемо цікаве обчислення - середній порядок елементів
групи `G`. Існує багато різних способів зробити це, ми розглянемо деякі з них
тут.

Цикл `for` у GAP дозволяє щось робити для кожного члена колекції.
Загальна форма циклу "for" така:

~~~
for val in collection do
  <something with val>
od;
~~~
{: .source}

Наприклад, ми можемо знайти середній порядок нашої групи `G`.

~~~
s:=0;;
for g in elts do
  s := s + Order(g);
od;
s/Length(elts);
~~~
{: .source}

~~~
31/12
~~~
{: .output}

Насправді, ми можемо просто перебирати елементи `G` (загалом GAP
дозволить вам перебирати більшість типів об’єктів).  Нам потрібно перейти на використання `Size`
замість `Length`, оскільки групи не мають довжини!

~~~
s:=0;;
for g in G do
  s := s + Order(g);
od;
s/Size(G);
~~~
{: .source}

~~~
31/12
~~~
{: .output}

Існують і інші способи зациклювання. Наприклад, натомість ми можемо перейти до діапазону цілих чиселі
прийняти `elts` як масив:

~~~
s:=0;;
for i in [ 1 .. Length(elts) ] do
  s := s + Order( elts[i] );
od;
s/Length(elts);
~~~
{: .source}

~~~
31/12
~~~
{: .output}

Однак часто існують більш компактні способи виконання завдань. Ось дуже
короткий шлях:

~~~
Sum( List( elts, Order ) ) / Length( elts );
~~~
{: .source}

~~~
31/12
~~~
{: .output}

Давайте розберемо останню частину:

* `Order` знаходить порядок однієї перестановки.
* `List(L,F)` створює новий список, де функція `F` застосовується
до кожного члена списку `L`.
* `Sum(L)` додає члени списку `L`.

> ## Який підхід найкращий?
>
> Порівняйте ці підходи. Якому із них ви віддасте перевагу?
{: .callout}


GAP має дуже корисні інструменти для роботи зі списками. Зараз ми покажемо ще кілька прикладів.

Іноді GAP не має тієї функції, яка нам потрібна.
Наприклад, `NrMovedPoints` дає кількість переміщених точок перестановки,
але що, якщо ми хочемо знайти всі перестановки, які пересувають 4 точки? Ось тут
і з’являється позначення GAP зі стрілками. `g -> e` створює нову функцію, яка отримує один аргумент `g`
і повертає значення виразу `e`. Ось деякі приклади:

* знаходження всіх елементів `G` без фіксованих точок:

~~~
Filtered( elts, g -> NrMovedPoints(g) = 4 );
~~~
{: .source}

~~~
[ (1,2)(3,4), (1,3)(2,4), (1,4)(2,3) ]
~~~
{: .output}

* знаходження перестановки в `G`, яка спрягає (1,2) та (2,3)

~~~
First( elts, g -> (1,2)^g = (2,3) );
~~~
{: .source}

~~~
(1,2,3)
~~~
{: .output}

Давайте перевіримо це (пам’ятайте, що в GAP перестановки множаться зліва направо!):

~~~
(1,2,3)^-1*(1,2)*(1,2,3)=(2,3);
~~~
{: .source}

~~~
true
~~~
{: .output}

* перевірка, чи всі елементи `G` пересувають точку 1 у 2:

~~~
ForAll( elts, g -> 1^g <> 2 );
~~~
{: .source}

~~~
false
~~~
{: .output}

* перевірка того, чи є елемент у `G`, який переміщує рівно дві точки:

~~~
ForAny( elts, g -> NrMovedPoints(g) = 2 );
~~~
{: .source}

~~~
false
~~~
{: .output}

> ## Використовуйте операції зі списком, щоб вибрати з `elts` стабілізатор точки 2 і централізатор перестановки (1,2)
>
> * `Filtered( elts, g -> 2^g = 2 );`
>
> * `Filtered( elts, g -> (1,2)^g = (1,2) );`
{: .challenge}

