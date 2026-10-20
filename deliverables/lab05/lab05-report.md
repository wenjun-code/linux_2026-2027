# Настройка рабочей среды
Чжан Вэньцзюнь
2026-10-20

- [<span class="toc-section-number">1</span> Цель работы](#цель-работы)
- [<span class="toc-section-number">2</span> Задание](#задание)
- [<span class="toc-section-number">3</span> Теоретическое
  введение](#теоретическое-введение)
- [<span class="toc-section-number">4</span> Выполнение лабораторной
  работы](#выполнение-лабораторной-работы)
  - [<span class="toc-section-number">4.1</span> Установка программного
    обеспечения](#установка-программного-обеспечения)
  - [<span class="toc-section-number">4.2</span> Инициализация хранилища
    паролей](#инициализация-хранилища-паролей)
  - [<span class="toc-section-number">4.3</span> Синхронизация с
    git](#синхронизация-с-git)
  - [<span class="toc-section-number">4.4</span> Добавление и просмотр
    пароля](#добавление-и-просмотр-пароля)
  - [<span class="toc-section-number">4.5</span> Настройка
    chezmoi](#настройка-chezmoi)
- [<span class="toc-section-number">5</span> Выводы](#выводы)
- [Список литературы](#список-литературы)

# Цель работы

Освоение инструментов настройки рабочей среды: менеджера паролей `pass`
и системы управления файлами конфигурации `chezmoi`.

# Задание

1.  Установить и настроить менеджер паролей `pass`.
2.  Организовать синхронизацию хранилища паролей через git.
3.  Установить и настроить `chezmoi` для управления файлами
    конфигурации.
4.  Освоить основные операции `chezmoi`.

# Теоретическое введение

Менеджер паролей `pass` построен в идеологии Unix: данные хранятся в
файловой системе в виде каталогов и файлов, каждый из которых зашифрован
GPG-ключом. Структура базы паролей может быть произвольной, однако при
использовании дополнительного программного обеспечения семантика
закладывается в имена файлов: например, `example.com`,
`example.com/user.pgp`, `user@example.com.pgp`, `example.com:22.pgp`.

Программа `chezmoi` управляет файлами конфигурации («dotfiles») в
домашнем каталоге: исходные версии хранятся в отдельном каталоге
`~/.local/share/chezmoi`, поддерживается шаблонизация и синхронизация
через git, что позволяет одной командой развернуть окружение на новой
машине \[1\].

# Выполнение лабораторной работы

## Установка программного обеспечения

Установлены менеджер паролей `pass` с расширением `pass-otp`, его
альтернативная реализация `gopass`, а также `chezmoi`:

``` bash
sudo dnf -y install pass pass-otp gopass chezmoi
```

## Инициализация хранилища паролей

Для шифрования хранилища создан отдельный GPG-ключ с назначением `encr`
(ключ, созданный в лабораторной работе № 2, предназначен только для
подписи и не может использоваться для шифрования):

``` bash
gpg --batch --pinentry-mode loopback --passphrase '' \
    --quick-gen-key 'Чжан Вэньцзюнь (pass encr) <wenjun-code@users.noreply.github.com>' rsa4096 encr 0
pass init 977D93C1037F05B2BFCD64183B360DE800EA0D60
```

Хранилище размещено в каталоге `~/.password-store`.

## Синхронизация с git

В хранилище инициализирован git-репозиторий; изменения, вносимые через
`pass`, фиксируются автоматически:

``` bash
pass git init
pass git log --oneline
```

Файл `~/.password-store/.gitattributes` настроен на использование `gpg`
для показа различий зашифрованных файлов.

## Добавление и просмотр пароля

Пароль добавлен в хранилище и проверен обратным чтением:

``` bash
pass insert -m example.com/user
pass show example.com/user
```

<div id="fig-pass">

<img src="image/lab05-pass.png" style="width:85.0%" />

Рисунок 1: Работа с хранилищем паролей pass

</div>

Пароль хранится в зашифрованном виде в файле
`~/.password-store/example.com/user.gpg`; содержимое каталога и история
git показаны на <a href="#fig-pass" class="quarto-xref">Рисунок 1</a>.

## Настройка chezmoi

Инициализировано хранилище конфигураций, в управление добавлен файл
`~/.bashrc`:

``` bash
chezmoi init
chezmoi add ~/.bashrc
chezmoi managed
chezmoi status
chezmoi source-path ~/.bashrc
```

<div id="fig-chezmoi">

<img src="image/lab05-chezmoi.png" style="width:85.0%" />

Рисунок 2: Управление файлами конфигурации с помощью chezmoi

</div>

Команда `chezmoi managed` показывает список управляемых файлов
(`.bashrc`), `chezmoi status` — расхождения между исходным каталогом и
домашним каталогом (расхождений нет), `chezmoi source-path` — путь к
исходной версии файла в каталоге `~/.local/share/chezmoi`.

Каталог `~/.local/share/chezmoi` является git-репозиторием, что
позволяет синхронизировать конфигурацию между машинами и разворачивать
её одной командой `chezmoi apply`.

# Выводы

В ходе работы установлены и настроены менеджер паролей `pass` и система
управления файлами конфигурации `chezmoi`. Создан отдельный GPG-ключ для
шифрования хранилища паролей, хранилище подключено к git для
синхронизации, проверены операции добавления и чтения пароля. В
управление `chezmoi` добавлен файл `.bashrc`, освоены команды `init`,
`add`, `managed`, `status` и `source-path`. Поставленные задачи
выполнены.

# Список литературы

<div id="refs" class="references csl-bib-body" data-entry-spacing="0">

<div id="ref-newham_book_learning-bash_en" class="csl-entry">

<span class="csl-left-margin">1.
</span><span class="csl-right-inline">Newham C. [Learning the bash
Shell: Unix Shell
Programming](http://www.amazon.com/Learning-bash-Shell-Programming-Nutshell/dp/0596009658).
O’Reilly Media, 2005. 354 с.</span>

</div>

</div>
