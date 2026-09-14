# Baseline сервера

## Назначение

Исходная фотография VPS перед началом практики по Linux, сетям, Docker,
Ansible и troubleshooting.

Baseline позволяет сравнивать состояние сервера до и после изменений.

- Дата фиксации: 2026-09-14
- Hostname: `moderkiller01.datacheap.ru`
- ОС: Ubuntu 24.04.5 LTS
- Ядро: `6.8.0-111-generic`
- Архитектура: `x86_64`
- Виртуализация: KVM
- Гипервизор: KVM
- Публичный IPv4: `144.48.10.27`
- Сетевой интерфейс: `ens3`

> Публичный IP здесь указан намеренно как часть учебной документации.
> Секреты, приватные ключи, токены и пароли в репозиторий не добавляются.

## Ресурсы

| Ресурс | Значение |
|---|---|
| CPU | 8 vCPU |
| CPU model | Intel Xeon Gold 6238R |
| RAM | 7.8 GiB |
| Swap | 511 MiB |
| Диск | 120 GiB |
| Корневая ФС | `/dev/vda2` |
| Размер `/` | 119 GiB |
| Использовано `/` | 3.7 GiB |
| Свободно `/` | 109 GiB |
| NUMA | 1 node |

## CPU

```text
Architecture:        x86_64
CPU(s):              8
On-line CPU(s):      0-7
Thread(s) per core:  1
Core(s) per socket:  8
Socket(s):           1
Virtualization:      VT-x
Hypervisor vendor:   KVM
```

## Память

```text
              total        used        free      shared  buff/cache   available
Mem:           7.8Gi       565Mi       5.1Gi       4.9Mi       2.4Gi       7.2Gi
Swap:          511Mi          0B       511Mi
```

## Диски и файловые системы

### Block devices

```text
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sr0     11:0    1 1024M  0 rom
vda    253:0    0  120G  0 disk
├─vda1 253:1    0    1M  0 part
└─vda2 253:2    0  120G  0 part /
```

### Filesystems

```text
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           794M  1.1M  793M   1% /run
/dev/vda2       119G  3.7G  109G   4% /
tmpfs           3.9G     0  3.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           794M   12K  794M   1% /run/user/0
```

## Сеть

### Интерфейсы

```text
lo      UNKNOWN  127.0.0.1/8 ::1/128
ens3    UP       144.48.10.27/24
                 fe80::5054:ff:fee5:d995/64
```

### Маршруты

```text
default via 144.48.10.1 dev ens3 onlink
144.48.10.0/24 dev ens3 proto kernel scope link src 144.48.10.27
```

### Краткая интерпретация

- `ens3` — основной сетевой интерфейс VPS.
- Адрес сервера в сети: `144.48.10.27/24`.
- Локальная подсеть: `144.48.10.0/24`.
- Шлюз по умолчанию: `144.48.10.1`.
- Весь трафик, для которого нет более специфичного маршрута, отправляется
  через default route.
- Флаг `onlink` означает, что шлюз считается доступным непосредственно через
  интерфейс, даже если ядро не выводит это из обычной проверки маршрута.

## Важные наблюдения

1. Сервер виртуальный: работает внутри KVM.
2. Доступно 8 vCPU и около 8 GiB RAM — достаточно для учебного Docker- и
   Ansible-стенда.
3. Корневая файловая система занимает весь диск.
4. LVM сейчас не используется: в выводе `lsblk` отсутствуют `lvm`-тома.
5. Swap есть, но небольшой — около 512 MiB.
6. Сетевой интерфейс поднят, IPv4-адрес назначен, default route присутствует.
7. IPv6-адрес на интерфейсе сейчас link-local (`fe80::/64`), глобальный IPv6
   адрес не указан.

## Команды, которыми получен baseline

```bash
cat /etc/os-release
uname -a
lscpu
free -h
lsblk
df -h
ip -br addr
ip route
```

## Команды для повторной проверки

```bash
hostnamectl
uptime
nproc
free -h
swapon --show
lsblk -f
df -hT
ip -br addr
ip route
ss -tulpn
systemctl --failed
```

## Что проверить после первичной настройки

- создан ли отдельный пользователь с `sudo`;
- запрещён ли SSH-вход root;
- отключена ли SSH-аутентификация по паролю;
- настроен ли firewall;
- установлен ли unattended-upgrades;
- какие TCP-порты слушают сервисы;
- установлен ли Docker;
- работает ли Ansible-подключение;
- появилась ли резервная копия конфигураций.
