# Xiaomi 13 Lite (ziyi) инструкция по сборке прошивки

В данном руководстве мы собирем прошивку EvolutionX A15.

## Полезные ссылки

| EvolutionX                                                                                                                                                                              | LineageOS                                                                                                                                                                                                 |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Device tree](https://github.com/cupid-development/android_device_xiaomi_ziyi) · [Device Common](https://github.com/Evolution-X-Devices/device_xiaomi_sm8450-common)                    | [Device tree](https://github.com/cupid-development/android_device_xiaomi_ziyi) · [Device Common](https://github.com/cupid-development/android_device_xiaomi_sm8450-common)                                |
| [Kernel](https://github.com/Evolution-X-Devices/kernel_xiaomi_sm8450)                                                                                                                   | [Kernel](https://github.com/cupid-development/android_kernel_xiaomi_sm8450)                                                                                                                               |
| [Vendor Common](https://github.com/Evolution-X-Devices/vendor_xiaomi_sm8450-common) · [Proprietary Vendor](https://git.mainlining.org/cupid-development/proprietary_vendor_xiaomi_ziyi) | [Vendor Common](https://git.mainlining.org/cupid-development/proprietary_vendor_xiaomi_sm8450-common) · [Proprietary Vendor](https://git.mainlining.org/cupid-development/proprietary_vendor_xiaomi_ziyi) |
| [Hardware](https://github.com/Evolution-X-Devices/hardware_xiaomi)                                                                                                                      | [Hardware](https://github.com/LineageOS/android_hardware_xiaomi)                                                                                                                                          |

# Системные требования

* Дистрибутив Ubuntu 24
* Оперативной памяти МИНИМУМ 16 GB
* Потребуется не менее 300 ГБ свободного места
* Желательно стабильное подключение к интернету
* Самое главное: терпение и стальные нервы

# Подготовка

## Установка / обновление пакетов

```shell
sudo apt update && sudo apt upgrade
sudo apt install unzip bc
bc bison build-essential ccache curl flex g++-multilib gcc-multilib git git-lfs gnupg gperf imagemagick lib32readline-dev lib32z1-dev libelf-dev liblz4-tool libsdl1.2-dev libssl-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev
```

## Настройка среды окружения

### Установка platform-tools

```shell
cd ~/
wget https://dl.google.com/android/repository/platform-tools-latest-linux.zip
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
mkdir -p ~/android
```

## Установка утилиты repo

```shell
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
```

## Настройка Git

```shell
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
git lfs install
git config --global trailer.changeid.key "Change-Id"
```

> [!IMPORTANT]
> * Там где **you@example.com** укажите адрес своей электронной почты
> * Там где **Your Name** ваш ник

## Настройка переменных окружения

```shell
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
```

Далее выполните эту команду:

```shell
nano ~/.bashrc
```

Вставьте в файле (или замените) данный параметр:

```shell
ccache -M 50G
```

# Инициализация репозитория / загрузка исходного кода

> [!NOTE]
> * Актуальные команды всегда можно найти в manifest-репозитории вашей прошивки.
> * В нашем случае это [EvolutionX Manifest](https://github.com/Evolution-X/manifest)

## Выбираем ранее созданную нами папку android

```shell
cd ~/android
```

## Инициализируем репозиторий

В нашем случае это EvolutionX.

```shell
repo init -u https://github.com/Evolution-X/manifest -b vic --git-lfs
```

## Выкачиваем исходный код прошивки

```shell
repo sync -c -j$(nproc --all) --force-sync --no-clone-bundle --no-tags
```

> [!IMPORTANT]
> * Данный процесс может занять около 1-2 часов, зависит от скорости вашего интернета
> * В случае появления ошибок, не беспокойтесь, по окончанию процесса сделайте езё раз синхронизацию, и он докачает
    нужные файлы
> * В случае успешной синхронизации, в консоли будет надпись: **repo sync has finished successfully.**

# Добавление device-side репозиториев

## Выбираем каталог

```shell
cd ~/android
```

## Добавление device tree (дерево устройства)

```shell
git clone https://github.com/cupid-development/android_device_xiaomi_ziyi.git -b lineage-22.1 --depth 1 device/xiaomi/ziyi
```

## Добавление ядра

```shell
git clone https://github.com/Evolution-X-Devices/kernel_xiaomi_sm8450.git -b vic --depth 1 kernel/xiaomi/sm8450
```

## Добавление Device Common

```shell
git clone https://github.com/Evolution-X-Devices/device_xiaomi_sm8450-common.git -b vic --depth 1 device/xiaomi/sm8450-common
```

## Добавление Vendor Common

```shell
git clone https://github.com/Evolution-X-Devices/vendor_xiaomi_sm8450-common.git -b vic --depth 1 vendor/xiaomi/sm8450-common
```

## Добавление Proprietary Vendor (вендор устройства)

```shell
git clone https://git.mainlining.org/cupid-development/proprietary_vendor_xiaomi_ziyi.git -b lineage-22.1 --depth 1 vendor/xiaomi/ziyi
```

## Добавление Hardware

```shell
git clone https://github.com/Evolution-X-Devices/hardware_xiaomi.git -b vic --depth 1 hardware/xiaomi
```
