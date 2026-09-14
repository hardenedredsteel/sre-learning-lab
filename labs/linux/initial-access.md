# Первичная настройка доступа

Дата: 2026-09-14

## Исходное состояние

- ОС: Ubuntu 24.04
- Пользователь по умолчанию: root
- Учебный пользователь: alex
- Административная группа: sudo
- Доступ: SSH Ed25519
- Сервер: VPS Professional NVMe (NL)

## Выполненные действия

- Создан пользователь `alex`.
- Пользователь добавлен в группу `sudo`.
- Создан `/home/alex/.ssh`.
- Добавлен публичный SSH-ключ.
- Проверен вход под `alex`.
- Проверено выполнение `sudo`.
- Настроена защита SSH.
- Настроен UFW.

## Проверка

```bash
whoami
id
sudo whoami
```

Результат:

```text
alex
uid=1000(alex) gid=1000(alex) groups=1000(alex),27(sudo),100(users)
root
```

## Возникшая ошибка

При создании файла была допущена опечатка:

```text
authorizes_keys
```

SSH ожидает:

```text
authorized_keys
```

Ошибка исправлена переименованием файла и настройкой владельца и прав:

```bash
mv authorizes_keys authorized_keys
chown alex:alex authorized_keys
chmod 600 authorized_keys
```
