# Xiaomi 13 Lite (ziyi) инструкция по сборке прошивки

## Полезные ссылки

| EvolutionX                                                                                                                                                                              | LineageOS                                                                                                                                                                                                 |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Device tree](https://github.com/cupid-development/android_device_xiaomi_ziyi) · [Device Common](https://github.com/Evolution-X-Devices/device_xiaomi_sm8450-common)                    | [Device tree](https://github.com/cupid-development/android_device_xiaomi_ziyi) · [Device Common](https://github.com/cupid-development/android_device_xiaomi_sm8450-common)                                |
| [Kernel](https://github.com/Evolution-X-Devices/kernel_xiaomi_sm8450)                                                                                                                   | [Kernel](https://github.com/cupid-development/android_kernel_xiaomi_sm8450)                                                                                                                               |
| [Vendor Common](https://github.com/Evolution-X-Devices/vendor_xiaomi_sm8450-common) · [Proprietary Vendor](https://git.mainlining.org/cupid-development/proprietary_vendor_xiaomi_ziyi) | [Vendor Common](https://git.mainlining.org/cupid-development/proprietary_vendor_xiaomi_sm8450-common) · [Proprietary Vendor](https://git.mainlining.org/cupid-development/proprietary_vendor_xiaomi_ziyi) |
| [Hardware](https://github.com/Evolution-X-Devices/hardware_xiaomi)                                                                                                                      | [Hardware](https://github.com/LineageOS/android_hardware_xiaomi)                                                                                                                                          |

# Подготовка среды окружения

## Установка / обновление пакетов

```shell
sudo apt update && sudo apt upgrade -y
sudo apt install -y openjdk-11-jdk git ccache lzop pngcrush schedtool perl bc zip unzip android-sdk-platform-tools python3 python3-venv python3-pip
```

## Настройка среды окружения

```shell
export PATH=$PATH:~/android/platform-tools
export USE_CCACHE=1
export CCACHE_DIR=~/android/ccache
export ANDROID_JACK_VM_ARGS="-Xmx4g -Dfile.encoding=UTF-8 -XX:+TieredCompilation"
```
