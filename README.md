# Шпаргалка по Git

## Что такое Git

Git — это **система контроля версий**, которая помогает отслеживать *изменения* в проекте. Этот инструмент можно использовать как для *индивидуальной*, так и для *командной* работы.

## Ключевые команды Git

### Инициализировать репозиторий (git init)

Чтобы Git начал отслеживать изменения в проекте, папку с файлами этого проекта нужно сделать __Git-репозиторием__ (от англ. *repository* — _«хранилище»_).

```bash
$ cd ~/dev/first-project # перейти в нужную папку
$ git init # создать репозиторий 
```

### "Разгитить" папку (rm -rf .git)

Для этого нужно *удалить* скрытую подпапку `.git`.

```bash
$ cd <папка с репозиторием> # перейти в папку
$ rm -rf .git # удалить подпапку .git 
```

* ключ `-r` (от англ. *recursive* — «*рекурсивно*») позволяет удалять папки вместе с их содержимым;

* ключ `-f` (от англ. *force* — «*заставить*») избавит вас от вопросов вроде «Вы точно хотите удалить этот файл? А этот? И этот тоже?».

### Проверить состояние репозитория (git status)

```bash
$ cd <папка с репозиторием> # перейти в папку
$ git status # показать текущее состояние репозитория 
```

### Подготовить файлы к сохранению (git add)

Данная команда не сохраняет, а *запоминает* текущее содержимое (контент) файла. Она делает файлы *отслеживаемыми* и *готовыми* к сохранению.

```bash
$ git add <файл> # подготовить к сохранению конкретный файл
$ git add --all # подготовить к сохранению все файлы в репозитории
$ git add . # добавить в репозиторий текущую папку со всеми файлами
```

### Выполнить коммит (git commit -m)

**Коммит** (по названию команды git commit) — это по сути список файлов с их контентом. Коммит гарантирует, что изменения будут сохранены в истории и при необходимости к ним можно будет «откатиться».

```bash
$ git commit -m 'Мой первый коммит!' # сделать коммит
```

Ключ `-m` (от англ. *message* — «*сообщение*») присваивает коммиту сообщение.

### Просмотреть историю коммитов (git log)

```bash
$ git log # вывести список коммитов
```

Данная команда выводит коммиты в обратном хронологическом порядке — последние коммиты оказываются первыми сверху.

## Что такое GitHub

**GitHub** — платформа для хранения IT-проектов и совместной работы над ними с использованием Git.

### Чем различаются Git и GitHub

**Git** и **GitHub** — это два *разных* проекта, которые развиваются независимо друг от друга. 

**GitHub**:
1. *Платформа* для размещения удалённых репозиториев, которая работает с Git и упрощает командное взаимодействие.
2. Принадлежит компании Microsoft.

**Git**:
1. *Консольный инструмент* для работы с локальными и удалёнными репозиториямии.
2. Разрабатывается как проект с открытым исходным кодом.

## Синхронизация репозиториев

### Что такое SSH-ключ

Когда компьютеры обмениваются данными в сети, они следуют **сетевым протоколам** (англ. *network protocols*) — *правилам* обмена данными между компьютерами.

**SSH** (от англ. *Secure Shell Protocol*) — один из наиболее распространённых сетевых протоколов, обеспечивающий *безопасный* обмен данными в сети. Использует *пару ключей*: 
* Приватный ключ (англ. *private key*) хранится только на вашем компьютере и не должен передаваться кому-либо ещё. Он используется для расшифровки данных.
* Публичный ключ (англ. *public key*) доступен всем и используется для шифрования данных. Они могут быть расшифрованы парным приватным ключом.

### Проверка наличия SSH-ключа

Обычно SSH-ключи находятся в директории `.ssh/`. 

```bash
$ cd ~ # перейти в домашнюю директорию
$ ls -la .ssh/ # вывести список созданных ключей 
``` 

### Сгенерировать SSH-ключ

```bash
$ ssh-keygen -t ed25519 -C "электронная почта, к которой привязан ваш аккаунт на GitHub" # с использованием алгоритма шифрования ed25519
$ ssh-keygen -t rsa -b 4096 -C "электронная почта, к которой привязан ваш аккаунт на GitHub" # с использованием алгоритма шифрования rsa
```

### Привязать SSH-ключ к GitHub

1. Скопируйте содержимое файла с публичным ключом в буфер обмена.

```bash
$ clip < ~/.ssh/id_rsa.pub # скопировать содержимое ключа в буфер обмена для rsa
$ clip < ~/.ssh/id_ed25519.pub # скопировать содержимое ключа в буфер обмена для ed25519
```

2. Перейдите на GitHub и выберите пункт `Settings` (англ. «*настройки*») в меню аккаунта.
3. В меню слева нажмите на пункт `SSH and GPG keys`.
4. В открывшейся вкладке выберите `New SSH key` (англ. «*новый SSH-ключ*»).
5. В поле `Title` (англ. «*заголовок*») напишите название ключа. Например, Personal key (англ. «*личный ключ*»).
6. В поле `Key type` (англ. «*тип ключа*») должно быть Authentication Key (англ. «*ключ аутентификации*»).
7. В поле `Key` скопируйте ваш ключ из буфера обмена.
8. Нажмите на кнопку `Add SSH key` (англ. «*добавить SSH-ключ*»).
9. Проверьте правильность ключа с помощью следующей команды.

```bash
$ ssh -T git@github.com 
```

### Привязать удалённый репозиторий к локальному (git remote add)

1. Перейдите на страницу удалённого репозитория, выберите тип SSH и скопируйте URL. Кнопка справа позволит сделать это мгновенно.
2. Откройте консоль, перейдите в каталог локального репозитория и введите команду `git remote add` (от англ. *remote* — «*удалённый*» и *add* — «*добавить*»).

```bash
$ cd ~/dev/first-project
$ git remote add origin git@github.com:%ИМЯ_АККАУНТА%/first-project.git
```

### Убедиться, что репозитории связаны (git remote -v)

```bash
$ git remote -v
origin    git@github.com:%ИМЯ_АККАУНТА%/%ИМЯ-ПРОЕКТА%.git (fetch)
origin    git@github.com:%ИМЯ_АККАУНТА%/%ИМЯ-ПРОЕКТА%.git (push) 
```

В выводе должны быть две последние строчки, аналогичные тем, что показаны выше.

Флаг `-v` — короткая форма флага `--verbose` (англ. «*подробный*»). Он позволяет показать больше информации в выводе.

### Отправить изменения на удалённый репозиторий (git push)

Загрузить содержимое локального репозитория на GitHub:

```bash
$ git push -u origin main # Если команда приведёт к ошибке, попробуйте 
                          # заменить main на master.
```

В первый раз эту команду нужно вызвать с флагом `-u` и параметрами `origin` (имя удалённого репозитория) и `main` или `master` (название текущей ветки). Флаг `-u` свяжет *локальную* ветку с одноимённой *удалённой*. 

В дальнейшем при работе с удалённым репозиторием флаг `-u` можно опустить и писать просто `git push`.

#### Ветки

Каждый коммит сохраняет актуальное состояние файлов. Сами же коммиты хранятся в **ветках** (англ. *branch*).

Если **коммит** — это *снимок* состояния файлов, то **ветка** — *временная шкала*, на которой расположены эти снимки. Ветка всегда начинается от одного из коммитов.

В репозитории может существовать сразу *несколько* веток — параллельных историй изменений. Также они могут соединяться друг с другом.

Самая *первая* ветка в репозитории появляется автоматически и называется `main` (англ. «*основная*») или `master`. Её имя нужно указывать при отправке коммитов на удалённый репозиторий или при получении их из него.

## Навигация по коммитам

### Что такое хеш и хеширование

**Хеширование** (от англ. *hash*, «*рубить*», «*крошить*», «*мешанина*») — это способ преобразовать набор данных и получить их «*отпечаток*» (англ. *fingerprint*).

**Информация о коммите** — это набор данных: когда был сделан коммит, содержимое файлов в репозитории на момент коммита и ссылка на предыдущий, или родительский (англ. *parent*), коммит.

Git *хеширует* (*преобразует*) информацию о коммите с помощью алгоритма *SHA-1* (от англ. *Secure Hash Algorithm* — «*безопасный алгоритм хеширования*») и получает для каждого коммита свой уникальный **хеш** — результат хеширования.

Обычно **хеш** — это короткая (40 символов в случае SHA-1) строка, которая состоит из цифр 0—9 и латинских букв A—F (неважно, заглавных или строчных). Она обладает следующими свойствами:
* если хеш получить дважды для одного и того же набора входных данных, то результат будет гарантированно одинаковый;
* если хоть что-то в исходных данных поменяется (хотя бы один символ), то хеш тоже изменится (причём сильно).

Git хранит таблицу соответствий `хеш → информация о коммите`. Если вы знаете хеш, вы можете узнать всё остальное: автора и дату коммита и содержимое закоммиченных файлов. Можно сказать, что **хеш** — основной *идентификатор* коммита.

Все хеши и таблицу `хеш → информация о коммите` Git сохраняет в служебные файлы. Они находятся в скрытой папке `.git` в репозитории проекта.

### Лог или описание коммита

После вызова `git log` появляется список коммитов с их *описанием*.

Элементы, из которых состоит описание:
* строка из цифр и латинских букв после слова **commit** — это *хеш* коммита;
* **Author** — имя автора и его электронная почта;
* **Date** — дата и время создания коммита;
* в конце находится сообщение коммита.

### Получить сокращённый лог (git log --oneline)

Получить **сокращённый лог** можно с помощью команды `git log` с флагом `--oneline` (англ. «*одной строкой*»). В терминале появятся только первые несколько символов хеша каждого коммита и их комментарии.

Сокращённый лог *полезен*, если в репозитории уже *много* коммитов — например, сотни или тысячи. В этом случае можно *быстро найти* нужный по описанию.

**Сокращённый хеш** (то есть первые несколько символов полного) можно использовать точно так же, как и полный. Для этого команда `git log --oneline` автоматически подбирает такую длину сокращённых хешей, чтобы они были уникальными в пределах репозитория и Git всегда мог понять, о каком коммите идёт речь.

### Что такое HEAD

**Файл** `HEAD` (англ. «*голова*», «*головной*») — один из *служебных* файлов папки `.git`. Он указывает на коммит, который сделан *последним* (то есть на *самый новый*).

**Внутри** `HEAD` — ссылка на служебный файл: `refs/heads/master` (или `refs/heads/main` в зависимости от названия ветки). Если заглянуть в этот файл, можно увидеть *хеш последнего коммита*.

Когда вы делаете коммит, Git обновляет `refs/heads/master` — записывает в него хеш последнего коммита. Получается, что `HEAD` тоже обновляется, так как ссылается на `refs/heads/master`.

Многие команды Git принимают в качестве параметра хеш коммита. Если нужно передать последний коммит, то вместо его хеша можно просто написать слово `HEAD` — Git поймёт, что вы имели в виду последний коммит.

## Статусы файлов в Git

### Untracked (англ. «неотслеживаемый»)

Git «видит», что такой файл существует, но не следит за изменениями в нём. У `untracked`-файла нет предыдущих версий, зафиксированных в коммитах или через команду `git add`.

### Staged (англ. «подготовленный»)

После выполнения команды `git add` файл попадает в **staging area** (от англ. *stage* — «*сцена*», «*этап [процесса]*» и *area* — «*область*»), то есть в список файлов, которые войдут в коммит. В этот момент файл находится в состоянии `staged`.

Команда `git add` добавляет в staging area только текущее содержимое файла. Если вы, например, сделаете `git add file.txt`, а затем измените `file.txt`, то новое содержимое файла не будет находиться в staging.

### Tracked (англ. «отслеживаемый»)

Состояние `tracked` — это противоположность `untracked`. Оно довольно широкое по смыслу: в него попадают файлы, которые уже были зафиксированы с помощью `git commit`, а также файлы, которые были добавлены в staging area командой `git add`. То есть все файлы, в которых Git так или иначе отслеживает изменения.

### Modified (англ. «изменённый»)

Состояние `modified` означает, что Git сравнил содержимое файла с последней сохранённой версией и нашёл отличия. Например, файл был закоммичен и после этого изменён или добавлен в staging area и после этого изменён.

### Типичный жизненный цикл файла в Git

1. Файл только что создали. Git ещё не отслеживает содержимое этого файла. Состояние: `untracked`.
2. Файл добавили в staging area с помощью `git add`. Состояние: `staged` (+ `tracked`).
* Возможно, изменили файл ещё раз. Состояния: `staged`, `modified` (+ `tracked`).
* Обратите внимание: `staged` и `modified` у одного файла, но у разных его версий.
* Ещё раз выполнили `git add`. Состояние: `staged` (+ `tracked`).
3. Сделали коммит с помощью `git commit`. Состояние: `tracked`.
4. Изменили файл. Состояние: `modified` (+ `tracked`).
5. Снова добавили в staging area с помощью `git add`. Состояния: `staged` (+ `tracked`).
6. Сделали коммит. Состояния: `tracked`.
7. Повторили пункты 4−7 много-много раз.

* `untracked` **--** `git add` **-->** `staged` + `tracked`
* `staged` **--** *modifications* **-->** `modified` + `tracked`
* `modified` **--** `git add` **-->** `staged` + `tracked`
* `staged` **--** `git commit -m` **-->** `tracked`
* `tracked` **--** *modifications* **-->** `modified` + `tracked`
* `modified` **--** `git add` **-->** `staged` + `tracked`
* `staged` **--** `git commit -m` **-->** `tracked`

## Оформление сообщений к коммитам

В выводе команды `git log --oneline` умещается максимум 72 первых символа сообщения.

### Рекомендации

#### Общие рекомендации:

* сообщение коммита легко читается;
* оно информативное;
* все сообщения оформлены в одном стиле.

#### Инфинитив и императив

Для сообщений на *русском* языке часто рекомендуют использовать **инфинитивы**:
* `Добавить тесты для PipkaService`
* `Исправить ошибку #123`

Для сообщений на *английском* рекомендуется использовать **повелительное наклонение** (англ. *imperative*):
* `Use library mega_lib_300`
* `Fix exit button`

Эти рекомендации сложились исторически, и им следуют многие проекты.

### Подходы к оформлению сообщений коммитов

#### Корпоративный

В корпоративном стиле в начале сообщения обычно указывают Jira-ID (идентификатор задачи), а после — текст сообщения.

```bash
$ git commit -m "LGS-239: Дополнить список пасхалок новыми числами"
```

#### Conventional Commits

Стандарт **Conventional Commits** (англ. «*соглашение о коммитах*») отличается качественной документацией и подробной проработкой. Он подходит для репозиториев с исходным кодом программ и предлагает такой формат коммита: `<type>: <сообщение>`.

`type` — это тип изменений, например:
* `feat` (сокращение от англ. *feature*) — для новой функциональности;
* `fix` (от англ. «*исправить*», «*устранить*») — для исправленных ошибок.

```bash
$ git commit -m "feat: добавить подсчёт суммы заказов за неделю"
```

#### GitHub-стиль

GitHub можно использовать не только для *хранения файлов* проекта, но и для **ведения списка задач** (англ. *issue*) этого проекта. Если коммит «закрывает» или «решает» какую-то задачу, то в его сообщении удобно указывать ссылку на неё. Для этого в любом месте сообщения нужно указать `#<номер задачи>`.

```bash
$ git commit -m "Исправить #334, добавить график температуры" 
```

В таком случае GitHub свяжет коммит и задачу.