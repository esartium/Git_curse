# Курс по гит: краткий конспект

## Основные команды для работы с git через терминал:

посмотреть, какая версия гит установлена на компьютере:

```
git --version 
```

### Основной алгоритм создания коммита:

На данный момент мы находимся в обычной папке. Она не находится под версионным контролем. 

Чтобы эта папка стала рабочей директорией, произведём следующие действия:

Создадим (инициализируем) в этой папке git-репозиторий:

```
git init 
```

Посмотреть статус файлов в рабочей директории: 

```
git status
```

Мы получим информацию: 

+ о ветке, на которой в данный момент находимся

+ о существующих на данный момент коммитах (или о том, что их нет)

+ о файлах в директории (отслеживаемых/неотслеживаемых)

Чтобы добавить неотслеживаемый файл в черновую область (то есть сделать его отслеживаемым), исапользуется команда:

```
git add название_файла.разрешение_файла
```

или, если нужно добавить все файлы, находящиеся в данной директории:

```
git add .
```

После команды git add, применив git status, мы увидим, что у нас появились изменения, ожидающие коммита.

Чтобы сделать коммит:

```
git commit -m "Параметр -m - это сообщение, связанное с коммитом, и содержащее информацию о нём, пишется в кавычках"
```

## Просмотр истории коммитов

Посмотреть все коммиты в директории можно с помощью:

```
git log
```

Другими словами, git log - это журнал изменений.

Команда git log выведет все созданные коммиты вместе с их идентификаторами, датой создания, данными создателя и сообщением.

Если ввести 

```
git checkout идентификатор_коммита
```

то мы увидим более подробную информацию о конкретном коммите.

Если создать второй коммит в данной директории, а после с помощью git checkout идентификатор_коммита переключиться на первый коммит, мы откатимся назад, в то состояние, в котором делался первый коммит, и поэтому второго файла не будет в директории. Второй коммит также не будет отображаться в git log. 

Чтобы вернуться обратно, используем git checkout не для конкретного коммита, а для ветки, на которой сейчас находимся, то есть введём:

```
git checkout master
```

## Ветвление

Сейчас мы на ветке master. Эта ветка создаётся по умолчанию с первым коммитом.

Получить информацию обо всех ветках, которые есть в проекте, можно с помощью команды:

```
git branch
```

Добавим отдельную ветку:

```
git branch имя_ветки_без_пробелов
```

Теперь эту ветку можно увидеть в списке, выводимом командой git branch.

Чтобы получить доступ ко второй ветке (переключиться на вторую ветку):

```
git checkout имя_ветки
```

Команда, чтобы создать новую ветку и сразу же на неё переключиться: 

```
git checkout -b имя_новой_ветки
```

### Слияние (объединение) веток

При слиянии двух веток у нас изначально есть:

+ Ветка 1: та, в которую должны быть добавлены изменения из другой ветки

+ Ветка 2, из которой мы переносим изменения в ветку 1.


При использовании команды слияния веток мы должны находиться на ветке 1 - той, куда переносятся изменения из ветки 2.

Переходим на ветку 1. Мы хотим слить контент из ветки 2 в ветку 1. Вводим команду:

```
git merge имя_ветки_2
```

## HEAD - указатель

По умолчанию указатель HEAD при переходе (с помощью git checkout) на какую-либо ветку ссылается (указывает) на последний находящийся на ветке коммит.

То есть, переходя на другую ветку и открывая там git log, мы получаем состояние ветки, содержащее множествол коммитов, в том числе и последний коммит, на котором установлен указатель.

### Смещённый указатель

Перемещение возможно как между ветками, так и между отдельными конкретными коммитами. 

Положение указателя на конкретном (то есть не последнем) коммите - смещённый указатель.
(Как будто коммит существует сам по себе, без принадлежности к какой-либо ветке)

Перемещаться между коммитами без привязки к веткам можно с помощью:

```
git checkout идентификатор_коммита
```

После этого указатель будет установлен на этот коммит, то есть указатель станет смещённым.

Чтобы вернуть указатель в нормальное состояние (на последний коммит), нужн с помощью git checkout перейти на конкретную ветку.

## Другие команды, связанные с ветками. Альтернатива git branch.

Checkout используется и для коммитов, и для веток; это может путать.

Switch используется для работы с ветками. Например, перейти на конкретную ветку: 

```
git switch название_ветки
```

Одновременно создать ветку и перейти на эту новую созданную веку:

```
git switch -c имя_новой_ветки
```

## Удаление данных

В данном контексте под данными подразумеваются коммиты, ветки, файлы и т.п. (Как те, которые являются частью предыдущего коммита, так и те, которые не являются, например, находятся в staging - черновой зоне, и ещё не были закоммичены) 

Их мы и будем удалять из-под версионного контроля.

### Удаление файлов

Мы на ветке master, и у нас нет изменений, которые можно было бы закоммитить.

```
git ls-files 
```

Выводит, какие файлы являются частью черновой зоны (вроде)

Мы хотим сделать так, чтобы последний коммит и черновая область в итоге оказались в одинаковом состоянии.
Если просто через список файлов в папке удалить в файл, а потом сделать git ls-files, то удалённый файл всё ещё существует в черновой области. Но мы хотим, чтобы его не было в коммите.

Мы удалили файл "вручную", просто нажали "удалить" в папке; теперь, введя git status, мы видим, что у нас есть изменение, которое ждёт коммита - удалён файл.

Чтобы подготовить это изменение для коммита, используем

```
git add имя_файла
```

или

```
git rm имя_файла
```

Оба способа в данном случае эквивалентны.

Мы выполнили одну из этих команд и после этого ещё раз ввелли git status. Теперь мы можем закоммитить это изменение (удаление). 
Команда git ls-files покажет, что этот файл больше не является частью черновой области.
Теперь введём git commit и закоммитим удаление.

### Удаление (отмена) изменений

Возьмём файл и внесём туда изменения. Нажмём "сохранить". Теперь используем команду git status. Она покажет, что у нас есть несохранённые (не добавленные в черновую зону) изменения.

Теперь мы хотим избавиться от изменений этого отслеживаемого файла, не добавленного в черновую зону.

Т.е. отменить изменения, т.е. вернуться к статусу последнего коммита (находится по указателю HEAD).

```
git checkout -- название_чего-нибудь
```

(с двумя знаками минуса) значит, что нам не нужно переключаться на другую ветку; но эти знаки можно и не использовать.

Введём:

```
git checkout имя_файла_в_который_вносили_изменения
```

Изменения пропали (выведенный командой результат говорит о том, что нам нечего коммитить). Это можно использовать для неотслеживаемых файлов, которые есть в нашей текущей ветке.

Также можно использовать команду

```
git checkout .
```

Она вернёт все файлы, которые есть в текущей ветке, к состоянию последнего коммита.

Т.о., git checkout - классический способ откатить неотслеживаемые изменения в отслеживаемых файлах.

#### Альтернатива checkout

Внесём изменения в файл и нажмём "сохранить".

git status покажет, что у нас есть изменения.

Теперь введём 

```
git restore имя_файла
```

или

```
git restore .
```

Эта команда сделает то же самое, что делала git checkout, но более явно.

Теперь у нас снова "чистый" статус: команда git status показывает, что несохранённых изменений нет.

### Удалим неотслеживаемый файл

Мы добавили файл в папку и написали в нём какой-то текст.

Этот файл не добавлен в индекс, он неотслеживаемый.

Чтобы его удалить, используем:

```
git clean то_что_мы_хотим_удалить
```

А удалить мы хотим только что добавленный неотслеживаемый файл. Для этого используем ключ -d. Этот ключ значит "удалить все неотслеживаемые папки и файлы". Если мы также добавим ключ -n, то вывведем список того, что было удалено только что:

```
git clean -dn
```

Если мы посмотрели на выведенный список и уверены, что хотим удалить эти файлы, используем опцию f - "принудительно" (говорим гиту: "удаляй и не задавай вопросов"):

```
git clean -df
```

Файл удалится.

### Отмена подготовленных изменений.

Внесёи изменения в файл и сохраним.

В выводе команды git status получим информацию о том, что у нас есть неподготовленные изменения.

Добавим их в черновую зону с помощью git add. Теперь в файле есть изменения, подготовленные(выделить это слово) для коммита.

И тут мы решаем отменить это изменение. Для этого мы должны снова сделать это изменение неподготовленным.

Это требует выполнения двух комманд. Мы не можем, как в случае неподготовленных изменений, просто вернуться к указателю HEAD, т.к. из-за введённой нами команды git add индекс уже обновлён.

Прежде чем мы сможем так сделать, нам нужно сначала вернуть состояние из последнего коммита и скопировать его в черновую зону. Это выполняется с помощью:

```
git reset имя_файла
```

Эта команда копирует последнее закоммиченное состояние файла, который находится в указателе текущей ветки.
Что это значит: если мы скопируем состояние последнего коммита, то этот файл больше не будет являться изменённым.

Применив команду git reset, видим, что в рабочей директории всё ещё есть новый контент, но теперь он не является подготовленным, и для удаления можно применить рассмотренные ранее инструменты (например, git checkout).

Другой способ:

```
git restore 
```

Это то же самое (восстановление состояния проекта), но в более явном виде.

Команды git reset и git restore в данном случае эквивалентны. 

### Удаление коммитов.

Будем делать это с помощью git reset.

Git reset помогает нам сбросить указатель наей ветки, то есть, мы можем отменить последние один/несколько коммитов.

Создадим новый коммит.

+ Мягкое применение git reset:

```
git reset --soft HEAD~1
```

Это значит "мягкий сброс", HEAD - указатель, число после тильды(~) - число коммитов, на которое нужно откатиться назад.

В итоге мы удалили только последний коммит, а изменения всё ещё находятся в рабочей директории (в подготовленном состоянии).

+ Обычное применение:

```
git reset HEAD~1
```

Поведение данной команды по умолчанию: коммит удаляется, указатель откатывается на один шаг назад, и изменения также удаляются из черновой области (изменения всё ещё есть в рабочей директории, но они не добавлены в git add).

+ Жёсткое применение:

```
git reset --hard HEAD~1
```

Жёсткий сброс удаляет коммит, удаляет изменения из рабочей директории и из черновой области.

### Удаление веток.

Команда 

```
git branch -d имя_ветки
```

позволяет удалить ветку, если её уже слили.

Команда

```
git branch -D имя_ветки
```

удаляет ветку принудительно, то есть вне зависимости от того, была она слита или нет.

### Удаление данных - применение изменений на смещённом указателе.

Мы хотим переключиться на один из предыдущих коммитов, добавить изменения, закоммитить их и добавить в ветку master.

Будем рассматривать два крайних коммита: последний и предпоследний.

Перейдём к предпоследнему с помощью команды:

```
git checkout идентификатор_коммита
```

Теперь мы перешли в режим смещённого указателя.

(В этом режиме мы через git branch можем увидеть ветку, которая хранит состояние смещённого указателя)

Добавим изменения в режиме смещённого указателя. 

Потом добавим эти изменения в индекс с помощью git add.

Закоммитим. 

(*) Теперь мы не можем вернуться на ветку master, так как у нас есть новый коммит, и он не связан с какой-либо веткой, так как ветка, содержащая положение смещённого указателя, удалится.

Чтобы вернуться, надо создать ветку, которая будет содержать данный коммит.

При попытке перейти на ветку master (*) мы получим сообщение в терминале:

```
Warning: you are leaving 1 commit behind, not connected to
any of your branches:

  0beca35 изменения в режиме смещённого указателя

If you want to keep it by creating a new branch, this may be a good time
to do so with:

 git branch имя_новой_ветки предложенный_идентификатор
```

Именно эту предложенную команду вместе с предложенным идентификатором мы должны ввести. 

Чтобы окончательно сохранить изменения, применённые в режиме смещённого указателя (вернуть их в основной проект), используем:

```
git switch master
git merge имя_ветки_которую_сливаем_с_master
```

Теперь можно (вроде) удалить нашу вспомогательную ветку, так как мы слили её с master.

## .gitignore

В проекте есть файлы, которые нужны, но не должны быть под контролем версий (например, файлы логов или файлы для настройки IDE).

Мы можем указать гиту, что нам нужны игнорировать определённые файлы/папки. Для этого создадим файл 
.gitignore

Он обычно создаётся вручную:

Создаём файл, который называется .gitignore, и просто записываем в него названия игнорируемых файлов (с расширением).

Сохраняем файл .gitignore

Теперь гит будет игнорировать указанные в .gitignore файлы при команде git status, и мы не сможем добавить их в черновую область.

Если игнорируемых файлов несколько, нужно:

+ либо добавить несколько файлов в .gitignore,

+ либо, если у нас есть несколько файлов с одинаковым расширением использовать символ *. Например:

```
\ *.log 
```

(палка не нужна, это чтобы звездочка не влияла на список)

Тогда все файлы с расширением .log будут игнорироваться.

+ Если мы хотим задать исключение (то есть нам нужно, чтобы игнорировались все файлы с определённым расширением, кроме одного из таких файлов), используем символ ! в тексте файла .gitignore

Например:

```
\ *.log
  !test.log  
```

(палка не нужна, это чтобы звездочка не влияла на список)

#### Игнорирование папки:

В .gitignore пишем:

```
название_папки/*
```

Теперь она игнорируется.

## git stash

Добавим файл, добавим его в черновую область (git add), закоммитим.

git stash позволяет хранить незакомиченные неподготовленные изменения (то есть это что-то вроде внутренней памяти); благодаря этому мы можем вернуться к последнему коммиту, а потом вернуть изменения из stash.

#### Как это работает:

#### Один stash

После последнего коммита производим какие-то изменения (не коммитим их, иначе и так окажемся в состоянии последнего коммита).

Затем вводим:

```
git stash
```

Эта команда возвращает нас к состоянию последнего коммита. Изменения, которые мы вносили после последнего коммита, хранятся в stash, и в любой момент мы можем восстановить их и продолжить с ними работать, введя:

```
git stash apply
```
#### Если больше одного stash:

Мы выполнили больше одного git stash.

Чтобы посмотреть их, используем:

```
git stash list
```

Так мы увидим эти скрытые изменения. 

Пример результата выполнения этой команды:

```
stash@{0}: WIP on master: e74c3f2 a
stash@{1}: WIP on master: 47b0da6 dfffd
```

Как видно, она выводит список отменённых изменений вместе с {индексом}. По этому индексу мы можем вернуться к определённым изменениям:

```
git stash apply индекс
```

Например:

```
git stash apply 1
```

После git stash apply изменения вернутся в статус неподготовленных, и мы сможем либо подготовить их к коммиту и закоммитить, либо отменить уже изученными способами.

#### Сообщения в stash

К скрытым изменениям можно добавить сообщение:

```
git stash push -m "Буквы"
```

Благодаря этой функции при выводе git stash list будет понятно, какие изменения лежат в каждом stash.

#### Удаление записи из списка stash

##### Удаление путём внесения в проект

Удалить запись из списка stash - внести изменения, хранящиеся в одном из stash, в сам проект, и удалить этот stash.

Это делается с помощью:

```
git stash pop индекс
```

Теперь, чтобы окончательно венуть изменения, мы можем добавить их в черновую зону и закоммитить.

##### Удаление без внесения

Чтобы просто удалить stash-запись, можно использовать:

```
git stash drop индекс
```

Запись пропадёт из списка, и в git stash list поменяются индексы оставшихся там stash-записей (встанут по порядку, подряд, начиная с нулевого).

Чтобы удалить все stash-записи из списка:

```
git stash clear
```

## git reflog

Используется, если нужно вернуть удалённые данные
(например, вернуть ошибочно удалённый коммит).

Как это работает:

#### Восстановление удалённых коммитов

Сделаем новый коммит, а потом вернёмся на один коммит назад с помощью команды:

```
git reset --hard HEAD~1
```

Теперь введём:

```
git reflog
```
Эта команда выведет нам список с описанием изменений, которые применялись в данной ветке.

(Изменения хранятся в течение 14 дней)

В этом списке есть и новый коммит, который мы удалили, вернувшись на один коммит назад. Этот коммит хранится вместе с идентификатором (7 символов).

Теперь введём:

```
git reset --hard идентификатор
```

Замечание: здесь берём второй из идентификаторов. Например, в случае вывода

```
f3e0525 (HEAD -> master) HEAD@{0}: reset: moving to HEAD~1
b714c2d HEAD@{1}: commit: nea
```

берём идентификатор b714c2d

И теперь указатель HEAD перемещается на коммит, который мы только что восстановили.

#### Восстановление веток

Создадим новую ветку и сразу перейдём на неё:

```
git checkout -b название_ветки
```

Создадим на ней новый коммит.

Теперь переключимся обратно на ветку мастер.

Введём:

```
git branch -D название_ветки
```

Мы удалили ветку.

Чтобы восстановить её, нам понадобится команда:

```
git reflog
```

В списке, выведенном данной командой, мы сможем найти коммит, который делали на удалённой ветке.

Скопируем идентификатор (опять второй).

Шаг 1. Перейдём к этому коммиту:

```
git checkout идентификатор
```

Мы оказались в режиме смещённого указателя.

Шаг 2. Сделаем новую ветку, которая будет содержать этот коммит:

Либо 

```
git checkout -b имя_ветки
```

либо

```
git switch -c имя_ветки
```

Теперь у нас две ветки: master и та, которую мы только что восстановили.

## Типы слияний

### Слияние fast-forward

fast-forward - быстрое слияниие с перемоткой вперёд.

У нас есть ветка master. Мы создаём ветку feature и продолжаем работать уже на этой новой ветке. В ветке master в это время ничего нового не происходит (то есть коммит, на момент которого мы перешли на ветку feature - на ветке master является последним).

Мы закончили работу с веткой feature и хотим слить её с веткой master. 

Используем команду git merge. После ввода этой команды git применит тип слияния fast-forward.

Fast-forward слияние работает тогда, когда на главной ветке после перехода на новую не появлялось новых изменений.

После объединения веток быстрое слияние переместит указатель на ветке master с последнего коммита (который мы сделали до перехода на новую ветку), к последнему коммиту новой ветки.

Этот тип слияния не создаёт новых коммитов. То есть дополнительный коммит слияния не создаётся.

Поэтому, чтобы откатить такое слияние, просто сдвинем указатель с помощью git reset --hard HEAD~число, где число - это количество коммитов, на которые нужно передвинуться назад, то есть количество коммитов, пришедших с другой ветки на ветку master. Тогда на этой ветке указатель снова перейдёт на коммит этой ветки, который был здесь посленим до слияния (у побочной ветки её коммиты останутся невредимыми).

Также, если мы применим команду

```
git merge --squash feature
```

(feature здесь - ветка, которую мы сливаем с master), то увидим, что такое слияние - тоже fast-forward, только данная опция добавляет на ветку master от ветки feature не все новые коммиты, которых нет на master, а только последний. Другими словами, эта опция объединяет все изменения, сделанные в feature-ветке, в один коммит - коммит слияния: она отправляет на ветку master изменения с ветки feature, и если мы их закоммитим, то получим коммит слияния, и указатель теперь указывает на последний коммит ветки master.

### Рекурсивное слияние

Возникает, когда на ветке master есть какие-то изменения, внесённые после создания feature-ветки.
То есть, когда в обеих ветках существуют параллельные изменения. Тогда при слиянии появится дополнительный коммит слияния.

Чтобы сделать рекурсивное слияние, когда на ветке master после перехода на новую ветку не происходило новых изменений до слияния, используем дополнительный флаг:

```
git merge --no-ff feature
```

Если после перехода на другую ветку на ветке master происходили какие-то изменения, то флаг -no-ff не нужен:

команда

```
git merge feature
```

приведёт к рекурсивному слиянию веток.

Если мы хотим отменить слияние, то нам не нужно откатываться на все коммиты назад, достаточо откатиться назад на один коммит - коммит слияния.

## Альтернативный подход к слиянию - rebase


