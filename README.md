# 旗標科技修改版 ulab

注意：這是旗標科技修改的版本, 使用在《[Flag's 創客‧自造者工作坊 用 Python 學 AIoT 智慧聯網](https://www.flag.com.tw/books/product/FM623A)》產品中, 客製化 MicroPython 的韌體。

## 編譯客製韌體程序

基本編譯程序請依照〈[如何編譯 ESP32 的 MicroPython 韌體](https://hackmd.io/ELwKGJYTSq--7MRaOOuGyQ?view)〉一文, 但需要注意以下事項：

1. MicroPython 請檢出到 b16990425 雜湊碼的提交狀態：

```
git clone https://github.com/micropython/micropython.git
cd micropyton
git checkout b16990425
```

2. [esp-idf 工具鏈](https://hackmd.io/ELwKGJYTSq--7MRaOOuGyQ?view#%E6%BA%96%E5%82%99-ESP-IDF-%E5%B7%A5%E5%85%B7%E9%8F%88)請依照上述文章採用 143d26aa49df524e10fb8e41a71d12e731b9b71d 雜湊碼的 3.3 版。
3. 請依照〈[加入使用者自訂模組–以 ulab 為例](https://hackmd.io/ELwKGJYTSq--7MRaOOuGyQ?view#%E5%8A%A0%E5%85%A5%E4%BD%BF%E7%94%A8%E8%80%85%E8%87%AA%E8%A8%82%E6%A8%A1%E7%B5%84%E2%80%93%E4%BB%A5-ulab-%E7%82%BA%E4%BE%8B)〉修改 ports/esp32/mpconfigport.h 檔, 啟用 ulab 模組。
4. 請在 ports/esp32/modules/ 下加入此套件所需的各 Python 模組檔案, 這些模組可在 [FM623A 的下載檔案](https://github.com/FlagTech/Python_AIoT_FM623A)中找到：
  ```
  ports/esp32/modules/BlynkLib.py
  ports/esp32/modules/keras_lite.py
  ports/esp32/modules/mpu6050.py
  ports/esp32/modules/umqtt/
  ports/esp32/modules/urequests.py  
  ```
以下是 [ulab](https://github.com/v923z/micropython-ulab) 原始說明。

# micropython-ulab

ulab is a numpy-like array manipulation library for micropython. 
The module is written in C, defines compact containers for numerical 
data, and is fast. 

Documentation can be found under https://micropython-ulab.readthedocs.io/en/latest/
The source for the manual is in https://github.com/v923z/micropython-ulab/blob/master/docs/ulab-manual.ipynb,
while developer help is in https://github.com/v923z/micropython-ulab/blob/master/docs/ulab.ipynb.

# Firmware

Firmware for pyboard.v.1.1, and PYBD_SF6 is updated once in a while, and can be downloaded 
from https://github.com/v923z/micropython-ulab/releases.

## Compiling

If you want to try the latest version of `ulab`, or your hardware is 
different to pyboard.v.1.1, or PYBD_SF6, the firmware can be compiled 
from the source by following these steps:

First, you have to clone the micropython repository by running 

```
git clone https://github.com/micropython/micropython.git
```
on the command line. This will create a new repository with the name `micropython`. Staying there, clone the `ulab` repository with 

```
git clone https://github.com/v923z/micropython-ulab.git ulab
```

Then you have to include `ulab` in the compilation process by editing `mpconfigport.h` of the directory of the port for which you want to compile, so, still on the command line, navigate to `micropython/ports/unix`, or `micropython/ports/stm32`, or whichever port is your favourite, and edit the `mpconfigport.h` file there. All you have to do is add a single line at the end: 

```
#define MODULE_ULAB_ENABLED (1)
```

This line will inform the compiler that you want `ulab` in the resulting firmware. If you don't have the cross-compiler installed, your might want to do that now, for instance on Linux by executing 

```
sudo apt-get install gcc-arm-none-eabi
```
If that was successful, you can try to run the make command in the port's directory as 
```
make BOARD=PYBV11 USER_C_MODULES=../../../ulab all
```
which will prepare the firmware for pyboard.v.11. Similarly, 
```
make BOARD=PYBD_SF6 USER_C_MODULES=../../../ulab all
```
will compile for the SF6 member of the PYBD series. Provided that you managed to compile the firmware, you would upload that by running
either
```
dfu-util --alt 0 -D firmware.dfu
```
or 
```
python pydfu.py -u firmware.dfu
```

In case you got stuck somewhere in the process, a bit more detailed instructions can be found under https://github.com/micropython/micropython/wiki/Getting-Started, and https://github.com/micropython/micropython/wiki/Pyboard-Firmware-Update.
