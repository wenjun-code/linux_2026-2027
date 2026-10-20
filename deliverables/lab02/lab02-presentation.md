# Лабораторная работа № 2
Чжан Вэньцзюнь
2026-10-20

# Информация

## Докладчик

- Чжан Вэньцзюнь
- зачётная книжка № 1132259051
- Российский университет дружбы народов им. П. Лумумбы

## Цель работы

- Изучить идеологию и применение средств контроля версий
- Освоить умения по работе с git
- Создать ключи SSH и PGP
- Настроить автоматическую подпись коммитов
- Создать репозиторий курса и рабочее пространство

# Выполнение работы

## Установка программного обеспечения

``` bash
sudo dnf -y install git gh
```

- `git` — система контроля версий
- `gh` — интерфейс командной строки GitHub

<img src="image/s1-install.png" style="width:70.0%" />

## Базовая настройка git

``` bash
git config --global user.name 'Чжан Вэньцзюнь'
git config --global user.email wenjun-code@users.noreply.github.com
git config --global core.quotepath false
git config --global init.defaultBranch master
git config --global core.autocrlf input
git config --global core.safecrlf warn
```

<img src="image/s2-config.png" style="width:70.0%" />

## Создание ключей SSH

- RSA 4096 бит: `ssh-keygen -t rsa -b 4096`
- Ed25519: `ssh-keygen -t ed25519`
- Ключи размещены в каталоге `~/.ssh/`

<img src="image/s3-sshkey.png" style="width:70.0%" />

## Создание ключа PGP

- Алгоритм: RSA 4096 бит, назначение `sign`, срок действия — бессрочно
- Отпечаток: `24299B5D6A1430222C7F881072F4C76DCD8F1C14`

``` bash
gpg --batch --pinentry-mode loopback --passphrase '' \
    --quick-gen-key 'Чжан Вэньцзюнь <wenjun-code@users.noreply.github.com>' rsa4096 sign 0
```

<img src="image/s4-gpg.png" style="width:70.0%" />

## Настройка и проверка подписи

``` bash
git config --global user.signingkey 24299B5D6A1430222C7F881072F4C76DCD8F1C14
git config --global commit.gpgsign true
git commit -S -m 'test: signed commit'
git log --show-signature -1
```

<img src="image/s6-sign.png" style="width:70.0%" />

## Рабочий каталог и репозиторий

``` bash
mkdir -p ~/work/study/2026-2027
cd ~/work/study/2026-2027
git clone https://github.com/wenjun-code/linux_2026-2027
```

<img src="image/s5-clone.png" style="width:70.0%" />

# Заключение

## Выводы

- Освоена базовая настройка git
- Созданы ключи SSH (RSA и Ed25519) и ключ PGP
- Настроена автоматическая подпись коммитов
- Подпись коммита проверена и признана действительной
- Репозиторий курса склонирован в рабочий каталог

## Спасибо за внимание!

Вопросы?
