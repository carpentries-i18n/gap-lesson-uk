---
layout: page
title: Налаштування
permalink: /setup/
---

## Windows

На [сторінці завантажень GAP](http://www.gap-system.org/Releases/),
завантажте інсталятор `.exe` і двічі клацніть файл, щоб запустити його.
Коли вас запитають про шлях інсталяції, зверніть увагу, що він
не повинен містити пробілів. Наприклад, ви можете встановити GAP 4.X.Y у `C:\gap-4.X.Y`
(за замовчуванням), `D:\gap-4.X.Y` чи `C:\Math\GAP\gap-4.X.Y`, але ви не повинні
інсталювати його в такий каталог, як `C:\Program Files\gap-4.X.Y` чи
`C:\Users\alice\My Documents\gap-4.X.Y`.

## OS X

В OS X Вам потрібно інсталювати GAP із джерела, як описано
на [сторінці завантажень GAP](http://www.gap-system.org/Releases/).
Завантажте один із архівів, наданих там, розпакуйте його та запустіть
`./configure && make` у розпакованому каталозі. Потім перейдіть
до підкаталогу `pkg` і викличте `../bin/BuildPackages.sh`, щоб запустити
сценарій, який створить більшість пакетів, які потребують компіляції
(за умови наявності достатньої кількості бібліотек, заголовків і інструментів).

Крім того, ви також можете встановити GAP за допомогою [Homebrew](http://brew.sh/).
Після встановлення Homebrew дотримуйтесь інструкцій для
[GAP Homebrew tap](https://github.com/gap-system/homebrew-gap).

## Linux

У Linux вам потрібно встановити GAP із джерела, як описано на
[сторінці завантажень GAP](http://www.gap-system.org/Releases/).
Завантажте один із наданих там архівів, розпакуйте його та запустіть
`./configure && make` у розпакованому каталозі. Потім перейдіть
до підкаталогу `pkg` і викличте `../bin/BuildPackages.sh`, щоб запустити
сценарій, який створить більшість пакетів, які потребують компіляції
(за умови наявності достатньої кількості бібліотек, заголовків і інструментів).
