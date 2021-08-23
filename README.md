# 旗標科技修改版 ulab

注意：這是旗標科技修改的版本, 使用在《[Flag's 創客‧自造者工作坊 用 Python 學 AIoT 智慧聯網](https://www.flag.com.tw/books/product/FM623A)》產品中, 客製化 MicroPython 的韌體。

## 編譯客製韌體程序

基本編譯程序請依照〈[旗標 FM623A 客製化韌體的編譯程序](https://hackmd.io/@flagmaker/BklDL0lbt)〉一文

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
