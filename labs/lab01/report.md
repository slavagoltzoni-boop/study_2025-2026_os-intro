# Лабораторная работа №1
## Установка и настройка операционной системы Linux

**Автор:** vsgoljcov  
**Дата:** 2026

---

# Цель работы

Приобретение навыков установки и первичной настройки операционной системы Linux (дистрибутив Fedora) в среде виртуализации VirtualBox, а также освоение базовых команд для работы в терминале и подготовки документации.

---

# Выполнение работы

## 1. Создание виртуальной машины

В программе VirtualBox была создана новая виртуальная машина со следующими параметрами:

- **Имя:** `vsgoljcov`
- **Тип ОС:** Linux
- **Версия:** Fedora (64-bit)
- **Оперативная память:** 4096 МБ
- **Жёсткий диск:** 50 ГБ, динамический
- **Сеть:** NAT

![Создание виртуальной машины](images/vm-creation.png)

## 2. Установка операционной системы

Запущена установка Fedora с ISO-образа. В процессе установки были заданы:

- язык интерфейса: русский
- раскладка клавиатуры: English (US), Russian
- имя пользователя: `vsgoljcov`
- имя хоста: `vsgoljcov`
- пароль пользователя

![Приветственный экран установщика](images/installer-welcome.png)

![Создание пользователя](images/user-creation.png)

После завершения установки ISO-образ был отключён в настройках VirtualBox, выполнена перезагрузка.

## 3. Первоначальная настройка системы

После входа в систему открыт терминал (Win+Enter). Получены права суперпользователя:

```bash
sudo -i
3.1. Отключение проблемного репозитория
Чтобы избежать ошибок при установке пакетов, репозиторий fedora-cisco-openh264 был отключён:

bash
mv /etc/yum.repos.d/fedora-cisco-openh264.repo /etc/yum.repos.d/fedora-cisco-openh264.repo.bak
3.2. Установка необходимых пакетов
Установлены средства разработки и программы для комфортной работы:

bash
dnf -y group install development-tools
dnf -y install tmux mc kitty dnf-automatic
https://images/dnf-install.png

3.3. Настройка автоматического обновления
Включён таймер автоматического обновления:

bash
systemctl enable --now dnf-automatic.timer
systemctl status dnf-automatic.timer
https://images/dnf-automatic-status.png

3.4. Отключение SELinux
Для учебных целей SELinux переведён в режим permissive. В файле /etc/selinux/config параметр SELINUX=enforcing заменён на SELINUX=permissive.

bash
nano /etc/selinux/config
https://images/selinux-config.png

3.5. Обновление системы и перезагрузка
bash
dnf -y update
systemctl reboot
4. Установка ПО для документации
4.1. Установка pandoc
bash
dnf -y install pandoc
4.2. Установка TeX Live
Установлен полный дистрибутив LaTeX:

bash
dnf -y install texlive-scheme-full
4.3. Установка pandoc-crossref
Так как пакет отсутствует в репозиториях, он установлен вручную:

bash
cd /tmp
wget https://github.com/lierdakil/pandoc-crossref/releases/download/v0.3.23a/pandoc-crossref-Linux-X64.tar.xz
tar -xf pandoc-crossref-Linux-X64.tar.xz
mv pandoc-crossref /usr/local/bin/
chmod +x /usr/local/bin/pandoc-crossref
4.4. Проверка версий
bash
pandoc --version | head -n 1
pandoc-crossref --version | head -n 1
https://images/pandoc-versions.png

5. Анализ загрузки системы (команда dmesg)
Для получения информации о системе использована команда dmesg. Результаты представлены ниже.

5.1. Версия ядра Linux
bash
dmesg | grep -i "linux version"
Результат:

text
[    0.000000] Linux version 6.17.1-300.fc43.x86_64 ...
5.2. Частота процессора
bash
dmesg | grep -i "detected.*mhz"
Результат:

text
[    0.000011] tsc: Detected 2419.200 MHz processor
5.3. Модель процессора (CPU0)
bash
dmesg | grep -i "cpu0"
Результат:

text
[    0.252124] smpboot: CPU0: 11th Gen Intel(R) Core(TM) i5-1135G7 @ 2.40GHz
5.4. Объём доступной оперативной памяти
bash
grep MemTotal /proc/meminfo
Результат:

text
MemTotal:        3945872 kB
5.5. Тип обнаруженного гипервизора
bash
dmesg | grep -i "hypervisor detected"
Результат:

text
[    0.000000] Hypervisor detected: KVM
5.6. Тип файловой системы корневого раздела
bash
findmnt / -o FSTYPE,SOURCE,TARGET
Результат:

text
FSTYPE SOURCE           TARGET
btrfs  /dev/sda3[/root] /
5.7. Последовательность монтирования файловых систем
bash
dmesg | grep -i "mount" | tail -20
Результат: (приведены последние строки вывода)

text
[    6.538839] EXT4-fs (sda2): mounted filesystem ...
https://images/dmesg.png

Выводы
В ходе выполнения лабораторной работы была установлена и настроена операционная система Fedora Linux в среде VirtualBox. Выполнены все необходимые шаги: установка средств разработки, настройка автоматического обновления, отключение SELinux, установка pandoc и TeX Live. Проведён анализ загрузки системы с помощью команды dmesg, получена информация об аппаратном обеспечении виртуальной машины.

Контрольные вопросы
1. Какую информацию содержит учётная запись пользователя?
Учётная запись пользователя в Linux содержит:

имя пользователя (login);

UID (идентификатор пользователя);

GID (идентификатор основной группы);

домашний каталог (например, /home/vsgoljcov);

командную оболочку (shell), например /bin/bash;

зашифрованный пароль (хранится в /etc/shadow);

комментарий (обычно полное имя пользователя).

Пример просмотра информации:

bash
id
cat /etc/passwd | grep vsgoljcov
2. Команды терминала с примерами
Действие	Команда	Пример
Справка по команде	man, --help	man ls, ls --help
Перемещение по ФС	cd	cd /home, cd .., cd ~
Просмотр содержимого	ls	ls -la
Определение объёма	du, df	du -sh /home, df -h
Создание/удаление	touch, mkdir, rm	touch file.txt, mkdir dir, rm -r dir
Права доступа	chmod, chown	chmod 755 script.sh, chown user:group file
История команд	history	history, !!, !100
3. Что такое файловая система? Примеры
Файловая система — это способ организации и хранения данных на диске. Примеры:

ext4 — стандартная для Linux, журналируемая, поддерживает большие файлы и тома.

XFS — высокопроизводительная, хороша для больших файлов.

btrfs — современная, поддерживает снапшоты, сжатие, проверку целостности.

FAT32 — старая, совместимая со всеми ОС, ограничение 4 ГБ на файл.

NTFS — стандартная для Windows, журналируемая, поддерживает права доступа.

4. Как посмотреть подмонтированные файловые системы?
bash
mount
findmnt
df -hT
cat /proc/mounts
5. Как удалить зависший процесс?
Найти PID процесса:

bash
ps aux | grep имя_процесса
Отправить сигнал завершения:

bash
kill PID
Если не завершается:

bash
kill -9 PID
Завершить все процессы с именем:

bash
pkill имя_процесса
text
