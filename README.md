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