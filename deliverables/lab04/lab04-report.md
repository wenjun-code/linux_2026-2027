# Продвинутое использование git
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
  - [<span class="toc-section-number">4.2</span> Настройка
    Node.js](#настройка-nodejs)
  - [<span class="toc-section-number">4.3</span> Общепринятые
    коммиты](#общепринятые-коммиты)
  - [<span class="toc-section-number">4.4</span> Создание тестового
    репозитория](#создание-тестового-репозитория)
  - [<span class="toc-section-number">4.5</span> Практический сценарий
    использования git](#практический-сценарий-использования-git)
- [<span class="toc-section-number">5</span> Выводы](#выводы)
- [Список литературы](#список-литературы)

# Цель работы

Получение навыков правильной работы с репозиториями git: освоение
рабочего процесса Gitflow, семантического версионирования и общепринятых
коммитов.

# Задание

1.  Выполнить работу для тестового репозитория.
2.  Преобразовать рабочий репозиторий в репозиторий с git-flow и
    conventional commits.

# Теоретическое введение

Рабочий процесс Gitflow, опубликованный Винсентом Дриссеном,
предполагает строгую модель ветвления с учётом выпуска проекта: в ветке
`master` хранится официальная история релизов, ветка `develop`
предназначена для объединения всех функций, под каждую функцию отводится
отдельная ветка `feature`, для подготовки выпуска — ветка `release`, для
срочных исправлений — `hotfix`.

Семантическое версионирование задаёт формат номера версии
`MAJOR.MINOR.PATCH`, в котором мажорный номер увеличивается при
несовместимых изменениях API, минорный — при добавлении функциональности
с обратной совместимостью, патч — при исправлении ошибок.

Общепринятые коммиты (Conventional Commits) задают формат сообщения
`тип(область): описание`, где тип принимает значения `feat`, `fix`,
`docs`, `chore` и другие. Такой формат позволяет автоматически
формировать журнал изменений \[1\].

# Выполнение лабораторной работы

## Установка программного обеспечения

Установлен пакет `gitflow` из репозитория Copr и среда Node.js,
необходимая для работы утилит семантического версионирования и
общепринятых коммитов:

``` bash
sudo dnf -y copr enable elegos/gitflow
sudo dnf -y install gitflow
sudo dnf -y install nodejs24 nodejs24-npm pnpm
```

<div id="fig-tools">

<img src="image/l4-tools.png" style="width:85.0%" />

Рисунок 1: Установленные инструменты

</div>

## Настройка Node.js

Каталог с исполняемыми файлами пакетов, устанавливаемых через `pnpm`,
добавлен в переменную `PATH`:

``` bash
pnpm setup
source ~/.bashrc
```

## Общепринятые коммиты

Установлены утилиты `commitizen` (скрипт `git-cz` для форматирования
сообщений коммитов) и `standard-changelog` (формирование журнала
изменений):

``` bash
npm install -g commitizen standard-changelog
```

При установке из-за низкой скорости официального реестра использовано
зеркало `registry.npmmirror.com`.

## Создание тестового репозитория

Создан каталог `~/work/study/2026-2027/git-extended`, инициализирован
репозиторий, подготовлен файл `package.json` с настройкой `commitizen` и
метаданными пакета:

``` bash
mkdir git-extended && cd git-extended
git init -b master
```

Содержимое `package.json` приведено на
<a href="#fig-pkg" class="quarto-xref">Рисунок 2</a>; там же показаны
созданные теги и ветки.

<div id="fig-pkg">

<img src="image/l4-pkg.png" style="width:85.0%" />

Рисунок 2: Файл package.json, теги и ветки

</div>

## Практический сценарий использования git

Инициализирована структура Gitflow с параметрами по умолчанию (`-d`), в
результате созданы ветки `master` и `develop`. Затем выполнены выпуск
версии 1.0.0 и исправление 1.0.1:

``` bash
git flow init -d
git flow release start 1.0.0
git add VERSION && git commit -m 'chore(release): prepare 1.0.0'
git flow release finish -m 'chore(release): 1.0.0' 1.0.0
git flow hotfix start 1.0.1
git commit -am 'fix: correct readme'
git flow hotfix finish -m 'fix: 1.0.1' 1.0.1
```

Сообщения коммитов оформлены в соответствии с Conventional Commits:
`feat: initial project structure`, `chore(release): prepare 1.0.0`,
`fix: correct readme`.

<div id="fig-graph">

<img src="image/l4-graph.png" style="width:85.0%" />

Рисунок 3: История репозитория после выпусков

</div>

В результате в репозитории созданы два тега (`1.0.0` и `1.0.1`), ветки
`master` и `develop`, а история содержит слияния веток `release/1.0.0` и
`hotfix/1.0.1`.

# Выводы

В ходе работы освоен рабочий процесс Gitflow: инициализация структуры
веток, подготовка и завершение выпуска, выполнение срочного исправления.
Изучено семантическое версионирование и формат общепринятых коммитов.
Установлены и применены утилиты `gitflow`, `commitizen` и
`standard-changelog`. Тестовый репозиторий преобразован в репозиторий с
git-flow и conventional commits. Все задачи лабораторной работы
выполнены.

# Список литературы

<div id="refs" class="references csl-bib-body" data-entry-spacing="0">

<div id="ref-robbins_book_bash_en" class="csl-entry">

<span class="csl-left-margin">1.
</span><span class="csl-right-inline">Robbins A. Bash Pocket Reference.
O’Reilly Media, 2016. 156 с.</span>

</div>

</div>
