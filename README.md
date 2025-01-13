# Xiaomi 13 Lite (ziyi) инструкция по сборке прошивки

![Xiaomi 13 Lite](banner.webp)

# Полезные ссылки 📌

| LineageOS                                                                                                                                                                                                 |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Device tree](https://github.com/cupid-development/android_device_xiaomi_ziyi) · [Device Common](https://github.com/cupid-development/android_device_xiaomi_sm8450-common)                                |
| [Kernel](https://github.com/cupid-development/android_kernel_xiaomi_sm8450)                                                                                                                               |
| [Vendor Common](https://git.mainlining.org/cupid-development/proprietary_vendor_xiaomi_sm8450-common) · [Proprietary Vendor](https://git.mainlining.org/cupid-development/proprietary_vendor_xiaomi_ziyi) |
| [Hardware](https://github.com/LineageOS/android_hardware_xiaomi)                                                                                                                                          |

# Системные требования 💻

* Дистрибутив Ubuntu 22.04.5 LTS (можно на WSL v2, но нежелательно)
* Оперативной памяти МИНИМУМ 16 GB (если у вас мало оперативной памяти, то рекомендуется использовать swap!)
* Потребуется не менее 300 ГБ свободного места на SSD / NVME
* Желательно стабильное подключение к интернету
* Самое главное: терпение и стальные нервы

# Подготовка 🔧

## Установка / обновление пакетов

```shell
sudo apt update && sudo apt upgrade
```

```shell
sudo apt install bc bison build-essential ccache curl flex g++-multilib gcc-multilib git git-lfs gnupg gperf imagemagick lib32ncurses5-dev lib32readline-dev lib32z1-dev liblz4-tool libncurses5 libncurses5-dev libsdl1.2-dev libssl-dev libwxgtk3.0-gtk3-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip unzip zlib1g-dev
```

## Настройка среды окружения

### Установка platform-tools

```shell
cd ~/
```

```shell
wget https://dl.google.com/android/repository/platform-tools-latest-linux.zip
```

```shell
unzip platform-tools-latest-linux.zip -d ~
```

### Добавление platform-tools в среду окружения

```shell
nano ~/.profile
```

Добавляем в самый конец файла:

```shell
# add Android SDK platform tools to path
if [ -d "$HOME/platform-tools" ] ; then
  PATH="$HOME/platform-tools:$PATH"
fi
```

> [!NOTE]
> * Навигация в файле **на стрелочки**
> * Чтобы сохранить нажмите **Ctrl+O** и **Enter**
> * Чтобы выйти **Ctrl+X**

После выхода, выполните данную комманду (она обновляет настройки среды окружения):

```shell
source ~/.profile
```

## Создание необходимых каталогов

```shell
mkdir -p ~/bin
```

```shell
mkdir -p ~/android/lineage
```

## Установка утилиты repo

```shell
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
```

```shell
chmod a+x ~/bin/repo
```

```shell
export PATH="~/bin:$PATH"
```

## Настройка Git

```shell
git config --global user.email "you@example.com"
```

```shell
git config --global user.name "Your Name"
```

```shell
git lfs install
```

```shell
git config --global trailer.changeid.key "Change-Id"
```

> [!IMPORTANT]
> * Там где **you@example.com** укажите адрес своей электронной почты
> * Там где **Your Name** ваше имя

## Настройка переменных окружения

```shell
export USE_CCACHE=1
```

```shell
export CCACHE_EXEC=/usr/bin/ccache
```

Далее выполните эту команду:

```shell
nano ~/.bashrc
```

Добавляем в самый конец файла:

```shell
# ccache size
ccache -M 50G
```

# Инициализация репозитория / загрузка исходного кода 📥

> [!NOTE]
> Актуальные команды всегда можно найти в manifest / build репозитории вашей прошивки.

## Выбираем каталог

```shell
cd ~/android/lineage
```

## Инициализируем репозиторий

В нашем случае это LineageOS.

```shell
repo init -u https://github.com/LineageOS/android.git -b lineage-22.1 --git-lfs --no-clone-bundle
```

## Выкачиваем исходный код прошивки

```shell
repo sync
```

> [!IMPORTANT]
> * Данный процесс может занять около 1-2 часов, зависит от скорости вашего интернета
> * В случае появления ошибок, не беспокойтесь, по окончанию процесса сделайте езё раз синхронизацию, и он докачает
    нужные файлы
> * В случае успешной синхронизации, в консоли будет надпись: **repo sync has finished successfully.**

# Добавление device-side репозиториев 📚

> [!NOTE]
> Если какой-то репозиторий клонируется с ошибкой, то пробуйте ещё раз

## Выбираем каталог

```shell
cd ~/android/lineage
```

## Добавление device tree (дерево устройства)

```shell
git clone https://github.com/cupid-development/android_device_xiaomi_ziyi.git -b lineage-22.1 --depth 1 device/xiaomi/ziyi
```

## Добавление ядра

```shell
git clone https://github.com/cupid-development/android_kernel_xiaomi_sm8450.git -b lineage-22.1 --depth 1 kernel/xiaomi/sm8450
```

## Добавление Device Common

```shell
git clone https://github.com/cupid-development/android_device_xiaomi_sm8450-common.git -b lineage-22.1 --depth 1 device/xiaomi/sm8450-common
```

## Добавление Vendor Common

```shell
git clone https://git.mainlining.org/cupid-development/proprietary_vendor_xiaomi_sm8450-common.git -b lineage-22.1 --depth 1 vendor/xiaomi/sm8450-common
```

## Добавление Proprietary Vendor (вендор устройства)

```shell
git clone https://git.mainlining.org/cupid-development/proprietary_vendor_xiaomi_ziyi.git -b lineage-22.1 --depth 1 vendor/xiaomi/ziyi
```

## Добавление Hardware

```shell
git clone https://github.com/LineageOS/android_hardware_xiaomi.git -b lineage-22.1 --depth 1 hardware/xiaomi
```

# Сборка прошивки 📦

## Выбираем каталог

```shell
cd ~/android/lineage
```

## Инициализируем среду окружения для сборки прошивки

```shell
source build/envsetup.sh
```

## Начинаем...

```shell
croot
```

```shell
brunch ziyi
```

> [!NOTE]
> * Примерное время сборки 3-4 часа, зависит от мощности вашего компьютера
> * В случае появления warning, deprecated или notice ошибок, не беспокойтесь, это нормально
