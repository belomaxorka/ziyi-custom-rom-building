# Xiaomi 13 Lite (ziyi) инструкция по сборке прошивки

В данном руководстве мы собирем прошивку EvolutionX.

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

# Подготовка среды окружения

## Установка / обновление пакетов

```shell
sudo apt update && sudo apt upgrade
sudo apt install zip unzip bc
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
cd ~/
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
> * Навигация в файле на стрелочки
> * Чтобы сохранить нажмите Ctrl+O и Enter
> * Чтобы выйти Ctrl+X

## Создание необходимых каталогов

```shell
mkdir ~/android
cd ~/android
```
