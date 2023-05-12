---
title: "Пошук малих груп"
teaching: 40
exercises: 15
questions:
- "Модульне програмування: поєднання функцій разом"
- "Як перевірити деяку гіпотезу для всіх груп заданого порядку"
objectives:
- "Використовуючи Small Groups Library"
- "Розробка системи функцій, яка сумісна"
keypoints:
- "Організуйте код у функції."
- "Створюйте малі групи одну за одною замість того, щоб створювати їх величезний список."
- "Використання `SmallGroupsInformation` може допомогти зменшити простір пошуку."
- "GAP не є чарівним інструментом: теоретичні знання можуть допомогти набагато більше, ніж підхід грубої сили."
---

У цьому розділі ми хочемо відкрити деякі нетривіальні групи з цікавою
властивістю, а саме, що середній порядок їхніх елементів є цілим числом.

Дистрибутив GAP включає низку бібліотек даних (див. огляд
[here](http://www.gap-system.org/Datalib/datalib.html)). Однією з них є
[Small Groups Library](http://www.gap-system.org/Packages/sgl.html) Ганса Ульріха Беше, Беттіни Айк та Імона О'Брайена.

Ця бібліотека надає різні утиліти для визначення того, яка інформація
там зберігається, і надсилання запитів для пошуку груп із потрібними
властивостями. Ключовими функціями є `SmallGroup`, `AllSmallGroups`,
`NrSmallGroups`, `SmallGroupsInformation` та `IdGroup`. Наприклад:

~~~
gap> NrSmallGroups(64);
267
gap> SmallGroupsInformation(64);

  There are 267 groups of order 64.
  They are sorted by their ranks.
     1 is cyclic.
     2 - 54 have rank 2.
     55 - 191 have rank 3.
     192 - 259 have rank 4.
     260 - 266 have rank 5.
     267 is elementary abelian.

  For the selection functions the values of the following attributes
  are precomputed and stored:
     IsAbelian, PClassPGroup, RankPGroup, FrattinifactorSize and
     FrattinifactorId.

  This size belongs to layer 2 of the SmallGroups library.
  IdSmallGroup is available for this size.

gap> G:=SmallGroup(64,2);
<pc group of size 64 with 6 generators>
gap> AllSmallGroups(Size,64,NilpotencyClassOfGroup,5);
[ <pc group of size 64 with 6 generators>, <pc group of size 64 with 6 generators>,
  <pc group of size 64 with 6 generators> ]
gap> List(last,IdGroup);
[ [ 64, 52 ], [ 64, 53 ], [ 64, 54 ] ]
~~~
{: .output}

Ми хотіли б використати нашу власну функцію тестування, яку ми створимо тут,
використовуючи вбудовану нотацію (доступну для функцій з одним аргументом):

~~~
TestOneGroup := G -> IsInt( AvgOrdOfGroup(G) );
~~~
{: .source}

Тепер спробуйте, наприклад,

~~~
List([TrivialGroup(),Group((1,2))],TestOneGroup);
~~~
{: .source}

~~~
[ true, false ]
~~~
{: .output}

~~~
gap> AllSmallGroups(Size,24,TestOneGroup,true);
~~~
{: .source}

~~~
[ ]
~~~
{: .output}

> ## Тут починається модульне програмування
>
> Чому повернення логічних значень є хорошим проєктним рішенням для таких функцій, 
> замість того, щоб просто друкувати інформацію чи повертати рядок, наприклад `"YES"`?
{: .callout}

Це простий приклад функції, яка перевіряє всі групи заданого порядку.
Він створює одну групу за раз, перевіряє потрібну властивість і повертається, щойно 
буде виявлено приклад. В іншому випадку він повертає `fail`, який є особливим типом
 логічної змінної в GAP.

~~~
TestOneOrderEasy := function(n)
local i;
for i in [1..NrSmallGroups(n)] do
  if TestOneGroup( SmallGroup( n, i ) ) then
    return [n,i];
  fi;
od;
return fail;
end;
~~~
{: .source}

Наприклад,

~~~
TestOneOrderEasy(1);
~~~
{: .source}

~~~
[ 1, 1 ]
~~~
{: .output}

~~~
TestOneOrderEasy(24);
~~~
{: .source}

~~~
fail
~~~
{: .output}

`AllSmallGroups` не вистачає пам'яті - що робити?
>
> *Використовуйте ітерацію над `[1..NrSmallGroups(n)]`, як показано у функції вище
> * Використовуйте `IdsOfAllSmallGroups`, яка приймає ті самі аргументи, що й `AllSmallGroups`, 
> але повертає ідентифікатори замість груп.
{: .callout}

Ітерація над `[1..NrSmallGroups(n)]` дає вам більше гнучкості, якщо вам потрібно
більше контролювати хід обчислення. Наприклад, наступна версія
нашої функції тестування друкує додаткову інформацію про номер
групи, що тестується. Він також надає функцію тестування як аргумент (чому
ви думаєте, що це краще?)

~~~
TestOneOrder := function(f,n)
local i, G;
for i in [1..NrSmallGroups(n)] do
  Print(n, ":", i, "/", NrSmallGroups(n), "\r");
  G := SmallGroup( n, i );
  if f(G) then
    Print("\
");
    return [n,i];
  fi;
od;
Print("\
");
return fail;
end;
~~~{: .source}

Наприклад,

~~~
TestOneOrder(TestOneGroup,64);
~~~
{: .source}

відобразить змінний лічильник під час обчислення, а потім поверне `fail`:

~~~
64:267/267
fail
~~~
{: .output}

Наступним кроком є інтеграція `TestOneOrder` у функцію, яка перевірить
усі порядки від 2 до `n` і зупиниться, щойно знайде приклад
групи, середній порядок елемента якого є цілим числом:

~~~
TestAllOrders:=function(f,n)
local i, res;
for i in [2..n] do
  res:=TestOneOrder(f,i);
  if res <> fail then
    return res;
  fi;
od;
return fail;
end;
~~~
{: .source}

Ми бачимо, що існує така група порядку 105:

~~~
TestAllOrders(TestOneGroup,128);
~~~
{: .source}

~~~
2:1/1
3:1/1
4:2/2
5:1/1
6:2/2
7:1/1
8:5/5
...
...
...
100:16/16
101:1/1
102:4/4
103:1/1
104:14/14
105:1/2
[ 105, 1 ]
~~~
{: .output}

Щоб дослідити її далі, ми можемо отримати її `StructureDescription` (див.
[тут](http://www.gap-system.org/Manuals/doc/ref/chap39.html#X87BF1B887C91CA2E)
для пояснення нотації, яку використано):

~~~
G:=SmallGroup(105,1); AvgOrdOfGroup(G); StructureDescription(G);
~~~
{: .source}

~~~
<pc group of size 105 with 3 generators>
17
"C5 x (C7 : C3)"
~~~
{: .output}

а потім перетворити її на скінченно представлену групу, щоб побачити її твірні та співвідношення.

~~~
H:=SimplifiedFpGroup(Image(IsomorphismFpGroup(G)));
RelatorsOfFpGroup(H);
~~~
{: .source}

~~~
<fp group on the generators [ F1, F2, F3 ]>
[ F1^3, F2^-1*F1^-1*F2*F1, F3^-1*F2^-1*F3*F2, F3^-1*F1^-1*F3*F1*F3^-1, F2^5,
  F3^7 ]
~~~
{: .output}

Тепер ми хочемо спробувати більші групи, починаючи з порядку 106 (ми перевіряємо,
що інша група порядку 105 не має такої властивості)

~~~
List(AllSmallGroups(105),AvgOrdOfGroup);
~~~
{: .source}

~~~
[ 17, 301/5 ]
~~~
{: .output}

З невеликими змінами ми додаємо додатковий аргумент, який визначає порядок,
з якого потрібно починати:

~~~
TestRangeOfOrders:=function(f,n1,n2)
local n, res;
for n in [n1..n2] do
  res:=TestOneOrder(f,n);
  if res <> fail then
    return res;
  fi;
od;
return fail;
end;
~~~
{: .source}

Але тепер ми викликаємо

~~~
TestRangeOfOrders(TestOneGroup,106,256);
~~~
{: .source}

і виявляємо, що тестування 2328 груп порядку 128 і додатково 56092
груп порядку 256 вже займає занадто багато часу.

> ## Не панікувати!
>
> Ви можете перервати GAP, натиснувши Ctrl-C один раз. Після цього GAP увійде
> в цикл розриву, позначений підказкою розриву `brk>`. Ви можете вийти з нього,
> ввівши `quit;` (стережіться натискання Ctrl-C двічі протягом секунди – це
> повністю завершить сеанс GAP).
{: .callout}

Це ще одна ситуація, коли теоретичні знання допомагають набагато більше,
ніж підхід грубої сили. Якщо група є _p_-групою, то порядок кожного
 класу спряженості нетотожного елемента групи ділиться на _p_;
 отже, середній порядок елемента групи може не бути цілим числом. Тому
_p_-групи можна виключити з розрахунку. Отже, нова версія коду є

~~~
TestRangeOfOrders:=function(f,n1,n2)
local n, res;
for n in [n1..n2] do
  if not IsPrimePowerInt(n) then
     res:=TestOneOrder(f,n);
     if res <> fail then
       return res;
     fi;
   fi;
od;
return fail;
end;
~~~
{: .source}

і використовуючи його, ми можемо виявити групу порядку 357 з тією ж властивістю:

~~~
gap> TestRangeOfOrders(TestOneGroup,106,512);
~~~
{: .source}

~~~
106:2/2
108:45/45
...
350:10/10
351:14/14
352:195/195
354:4/4
355:2/2
356:5/5
357:1/2
[ 357, 1 ]
~~~
{: .output}

Наступна функція демонструє ще більшу гнучкість: вона є варіативною, тобто
вона може приймати два або більше аргументів, перші два з яких будуть призначені змінним `f` і `n`, а решта буде доступна в список `r`
(це позначено `...` після `r`). Перший аргумент —
функція перевірки, другий — порядок перевірки, третій і четвертий —
номери першої та останньої груп цього порядку, які потрібно
перевірити. За замовчуванням останні два дорівнюють 1 і `NrSmallGroups(n)`
відповідно. Ця функція також показує, як перевірити вхідні дані
та створювати зручні повідомлення про помилки у випадку неприпустимих аргументів.

Крім того, ця функція демонструє, як використовувати повідомлення `Info`,
які можна вмикати та вимикати, встановивши відповідний рівень `Info`. Потреба,
яку ми тут розглядаємо, полягає в тому, щоб мати можливість перемикати рівні багатослівності
виводу без підходу, схильного до помилок, проходження коду та коментування
інструкцій `Print`. Це досягається шляхом створення інформаційного класу:

~~~
gap> InfoSmallGroupsSearch := NewInfoClass("InfoSmallGroupsSearch");
~~~
{: .source}

~~~
InfoSmallGroupsSearch
~~~
{: .output}

Тепер замість `Print("something");` можна використовувати
`Info( InfoSmallGroupsSearch, infolevel, "something");`,
де `infolevel` є додатним цілим числом, що визначає рівень докладності.
Цей рівень можна змінити на `n` за допомогою команди
`SetInfoLevel( InfoSmallGroupsSearch, n);`. Перегляньте фактичні виклики `Info` у
коді нижче:

~~~
TestOneOrderVariadic := function(f,n,r...)
local n1, n2, i;

if not Length(r) in [0..2] then
  Error("The number of arguments must be 2,3 or 4\
" );
fi;

if not IsFunction( f ) then
  Error("The first argument must be a function\
" );
fi;

if not IsPosInt( n ) then
  Error("The second argument must be a positive integer\
" );
fi;

if IsBound(r[1]) then
  n1:=r[1];
  if not n1 in [1..NrSmallGroups(n)] then
    Error("The 3rd argument, if present, must belong to ", [1..NrSmallGroups(n)], "\
" );
  fi;
else
  n1:=1;
fi;

if IsBound(r[2]) then
  n2:=r[2];
  if not n2 in [1..NrSmallGroups(n)] then
    Error("The 4th argument, if present, must belong to ", [1..NrSmallGroups(n)], "\
" );
  elif n2 < n1 then
    Error("The 4th argument, if present, must be greater or equal to the 3rd \
" );
  fi;
else
  n2:=NrSmallGroups(n);
fi;

Info( InfoSmallGroupsSearch, 1,
      "Checking groups ", n1, " ... ", n2, " of order ", n );
for i in [n1..n2] do
  if InfoLevel( InfoSmallGroupsSearch ) > 1 then
    Print(i, "/", NrSmallGroups(n), "\r");
  fi;
  if f(SmallGroup(n,i)) then
    Info( InfoSmallGroupsSearch, 1,
          "Discovered counterexample: SmallGroup( ", n, ", ", i, " )" );
    return [n,i];
  fi;
od;
Info( InfoSmallGroupsSearch, 1,
      "Search completed - no counterexample discovered" );
return fail;
end;
~~~{: .source}

У наступному прикладі показано, як тепер можна керувати
виводом, перемикаючи рівень інформації для `InfoSmallGroupsSearch`:

~~~
gap> TestOneOrderVariadic(IsIntegerAverageOrder,24);
fail
gap> SetInfoLevel( InfoSmallGroupsSearch, 1 );
gap> TestOneOrderVariadic(IsIntegerAverageOrder,24);
#I  Перевірка груп 1 ... 15 порядку 24
#I  Пошук завершено - контрприкладів не знайдено
fail
gap> TestOneOrderVariadic(IsIntegerAverageOrder,357);
#I  Перевірка груп 1 ... 2 порядку 357
#I  Виявлений контрприклад: SmallGroup( 357, 1 )
[ 357, 1 ]
gap> SetInfoLevel( InfoSmallGroupsSearch, 0);
gap> TestOneOrderVariadic(IsIntegerAverageOrder,357);
[ 357, 1 ]
~~~
{: .output}

Звичайно, тепер це вносить деякі ускладнення для тестового файлу,
 який порівнює фактичний вихід із еталонним виходом. Щоб вирішити
цю проблему, ми вирішимо запустити тести на інформаційному рівні 0, щоб заблокувати
всі додаткові виходи. Оскільки тести могли бути запущені в
сеансі GAP з іншим рівнем інформації, ми запам’ятаємо цей рівень інформації,
щоб відновити його після тесту:

~~~
# Знаходжeння груп із цілим середнім порядком
gap> INFO_SSS:=InfoLevel(InfoSmallGroupsSearch);;
gap> SetInfoLevel( InfoSmallGroupsSearch, 0);
gap> res:=[];;
gap> for n in [1..360] do
>      if not IsPrimePowerInt(n) then
>        t := TestOneOrderVariadic( IsIntegerAverageOrder,n,1,NrSmallGroups(n) );
>        if t <> fail then
>          Add(res,t);
>        fi;
>      fi;
>    od;
gap> res;
[ [ 1, 1 ], [ 105, 1 ], [ 357, 1 ] ]
gap> SetInfoLevel( InfoSmallGroupsSearch, INFO_SSS);
~~~
{: .output}

> ## Чи містить бібліотека Small Groups Library іншу групу з цією властивістю?
>
> * Що Ви можете сказати про порядок груп із цією властивістю?
>
> * Чи можете Ви оцінити, скільки часу може зайняти перевірка всіх 408641062 груп порядку 1536?
>
> * Скільки груп порядку не вище 2000 Ви могли б перевірити,
> за винятком _p_-груп і груп порядку 1536?
>
> * Чи можете Ви знайти іншу групу з цією властивістю в бібліотеці Small Groups Library
 > (порядок не дорівнює 1536)?
{: .challenge}

