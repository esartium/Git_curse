# Курс по гит: краткий конспект

## Основные команды для работы с git через терминал:

посмотреть, какая версия гит установлена на компьютере:

git --version 

### Основной алгоритм создания коммита:

На данный момент мы находимся в обычной папке. Она не находится под версионным контролем. 

Чтобы эта папка стала рабочей директорией, произведём следующие действия:

Создадим (инициализируем) в этой папке git-репозиторий:

git init 

Посмотреть статус файлов в рабочей директории: 

git status

Мы получим информацию: 

+ о ветке, на которой в данный момент находимся

+ о существующих на данный момент коммитах (или о том, что их нет)

+ о файлах в директории (отслеживаемых/неотслеживаемых)

Чтобы добавить неотслеживаемый файл в черновую область (то есть сделать его отслеживаемым), исапользуется команда:

git add название_файла.разрешение_файла

или, если нужно добавить все файлы, находящиеся в данной директории:

git add .

После команды git add, применив git status, мы увидим, что у нас появились изменения, ожидающие коммита.

Чтобы сделать коммит:

git commit -m "Параметр -m - это сообщение, связанное с коммитом, и содержащее информацию о нём, пишется в кавычках"

## Просмотр истории коммитов

Посмотреть все коммиты в директории можно с помощью:

git log

Другими словами, git log - это журнал изменений.

Команда git log выведет все созданные коммиты вместе с их идентификаторами, датой создания, данными создателя и сообщением.

Если ввести 
git checkout идентификатор_коммита,
то мы увидим более подробную информацию о конкретном коммите.

Если создать второй коммит в данной директории, а после с помощью git checkout идентификатор_коммита переключиться на первый коммит, мы откатимся назад, в то состояние, в котором делался первый коммит, и поэтому второго файла не будет в директории. Второй коммит также не будет отображаться в git log. 

Чтобы вернуться обратно, используем git checkout не для конкретного коммита, а для ветки, на которой сейчас находимся, то есть введём:

git checkout master

## Ветвление

Сейчас мы на ветке master. Эта ветка создаётся по умолчанию с первым коммитом.

Получить информацию обо всех ветках, которые есть в проекте, можно с помощью команды:

git branch

Добавим отдельную ветку:

git branch имя_ветки_без_пробелов

Теперь эту ветку можно увидеть в списке, выводимом командой git branch.

Чтобы получить доступ ко второй ветке (переключиться на вторую ветку):

git checkout имя_ветки

Команда, чтобы создать новую ветку и сразу же на неё переключиться: 

git checkout -b имя_новой_ветки

### Слияние (объединение) веток

При слиянии двух веток у нас изначально есть:

+ Ветка 1: та, в которую должны быть добавлены изменения из другой ветки

+ Ветка 2, из которой мы переносим изменения в ветку 1.

При использовании команды слияния веток мы должны находиться на ветке 1 - той, куда переносятся изменения из ветки 2.

Переходим на ветку 1. Мы хотим слить контент из ветки 2 в ветку 1. Вводим команду:

git merge имя_ветки_2

## HEAD - указатель

По умолчанию указатель HEAD при переходе (с помощью git checkout) на какую-либо ветку ссылается (указывает) на последний находящийся на ветке коммит.

То есть, переходя на другую ветку и открывая там git log, мы получаем состояние ветки, содержащее множествол коммитов, в том числе и последний коммит, на котором установлен указатель.

### Смещённый указатель

Перемещение возможно как между ветками, так и между отдельными конкретными коммитами. 

Положение указателя на конкретном (то есть не последнем) коммите - смещённый указатель.
(Как будто коммит существует сам по себе, без принадлежности к какой-либо ветке)

Перемещаться между коммитами без привязки к веткам можно с помощью:

git checkout идентификатор_коммита

После этого указатель будет установлен на этот коммит, то есть указатель станет смещённым.

Чтобы вернуть указатель в нормальное состояние (на последний коммит), нужн с помощью git checkout перейти на конкретную ветку.

## Другие команды, связанные с ветками. Альтернатива git branch.

Checkout используется и для коммитов, и для веток; это может путать.

Switch используется для работы с ветками. Например, перейти на конкретную ветку: 

git switch название_ветки

Одновременно создать ветку и перейти на эту новую созданную веку:

 git switch -c имя_новой_ветки

## Удаление данных

В данном контексте под данными подразумеваются коммиты, ветки, файлы и т.п. (Как те, которые являются частью предыдущего коммита, так и те, которые не являются, например, находятся в staging - черновой зоне, и ещё не были закоммичены) 

Их мы и будем удалять из-под версионного контроля.

### Удаление файлов

Мы на ветке master, и у нас нет изменений, которые можно было бы закоммитить.

git ls-files 

Выводит, какие файлы являются частью черновой зоны (вроде)

Мы хотим сделать так, чтобы последний коммит и черновая область в итоге оказались в одинаковом состоянии.
Если просто через список файлов в папке удалить в файл, а потом сделать git ls-files, то удалённый файл всё ещё существует в черновой области. Но мы хотим, чтобы его не было в коммите.

Мы удалили файл "вручную", просто нажали "удалить" в папке; теперь, введя git status, мы видим, что у нас есть изменение, которое ждёт коммита - удалён файл.

Чтобы подготовить это изменение для коммита, используем

git add имя_файла

или

git rm имя_файла

Оба способа в данном случае эквивалентны.

Мы выполнили одну из этих команд и после этого ещё раз ввелли git status. Теперь мы можем закоммитить это изменение (удаление). 
Команда git ls-files покажет, что этот файл больше не является частью черновой области.
Теперь введём git commit и закоммитим удаление.

### Удаление (отмена) изменений

Возьмём файл и внесём туда изменения. Нажмём "сохранить". Теперь используем команду git status. Она покажет, что у нас есть несохранённые (не добавленные в черновую зону) изменения.

Теперь мы хотим избавиться от изменений этого отслеживаемого файла, не добавленного в черновую зону.

Т.е. отменить изменения, т.е. вернуться к статусу последнего коммита (находится по указателю HEAD).

git checkout -- название_чего-нибудь

(с двумя знаками минуса) значит, что нам не нужно переключаться на другую ветку; но эти знаки можно и не использовать.

Введём:

git checkout имя_файла_в_который_вносили_изменения

Изменения пропали (выведенный командой результат говорит о том, что нам нечего коммитить). Это можно использовать для неотслеживаемых файлов, которые есть в нашей текущей ветке.

Также можно использовать команду

git checkout .

Она вернёт все файлы, которые есть в текущей ветке, к состоянию последнего коммита.

Т.о., git checkout - классический способ откатить неотслеживаемые изменения в отслеживаемых файлах.

#### Альтернатива checkout

Внесём изменения в файл и нажмём "сохранить".

git status покажет, что у нас есть изменения.

Теперь введём 

git restore имя_файла

или

git restore .

Эта команда сделает то же самое, что делала git checkout, но более явно.

Теперь у нас снова "чистый" статус: команда git status показывает, что несохранённых изменений нет.

### Удалим неотслеживаемый файл

Мы добавили файл в папку и написали в нём какой-то текст.

Этот файл не добавлен в индекс, он неотслеживаемый.

Чтобы его удалить, используем:

git clean то_что_мы_хотим_удалить

А удалить мы хотим только что добавленный неотслеживаемый файл. Для этого используем ключ -d. Этот ключ значит "удалить все неотслеживаемые папки и файлы". Если мы также добавим ключ -n, то вывведем список того, что было удалено только что:

git clean -dn

Если мы посмотрели на выведенный список и уверены, что хотим удалить эти файлы, используем опцию f - "принудительно" (говорим гиту: "удаляй и не задавай вопросов"):

git clean -df

Файл удалится.

### Отмена подготовленных изменений.








