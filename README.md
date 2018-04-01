# BH1750

该库针对BH1750数字光线传感器的arduino库。
BH1750传感器使用IIC接口，仅需两根信号线即可与arduino通信。
常见的BH1750模块如下图

![GY-30 Module image](resources/gy30-module.jpg)


## 简介

The BH1750 has six different measurement modes which are divided in two groups;
continuous and one-time measurements. In continuous mode the sensor
continuously measures lightness value. In one-time mode, the sensor makes only
one measurement and then goes into Power Down mode.

Each mode has three different precisions:

  - Low Resolution Mode - (4 lx precision, 16ms measurement time)
  - High Resolution Mode - (1 lx precision, 120ms measurement time)
  - High Resolution Mode 2 - (0.5 lx precision, 120ms measurement time)

By default, this library uses Continuous High Resolution Mode, but you can
change this to a different mode by passing the mode argument to
BH1750.begin().

When the One-Time mode is used your sensor will go into Power Down mode when
it completes the measurement and you've read it. When the sensor is powered up
again it returns to the default mode which means it needs to be reconfigured
back into One-Time mode. This library has been implemented to automatically
reconfigure the sensor when you next attempt a measurement so you should not
have to worry about such low level details.

The datasheet for the BH1750 chip can be obtained [here](http://www.elechouse.com/elechouse/images/product/Digital%20light%20Sensor/bh1750fvi-e.pdf)


## 安装
点击 "Clone or download" -> "Download ZIP" 按钮.

  - **(Arduino 1.5.x以上版本)** 打开Arduino
    IDE, 点击 `项目 -> 加载库 -> 添加 .ZIP 库 ` 然后选择下载好的zip文件。

## 例子

An example using the BH1750 library in conjunction with the GY-30 board
(which contains the BH1750 component) is presented below. The example
code uses the BH1750 library in the default continuous high precision
mode when making light measurements.

### 连线

连线说明:

  - VCC -> 3V3 or 5V
  - GND -> GND
  - SCL -> SCL (A5 on Arduino Nano, Uno, Leonardo, etc or 21 on Mega and Due, on esp8266 free selectable)
  - SDA -> SDA (A4 on Arduino Nano, Uno, Leonardo, etc or 20 on Mega and Due, on esp8266 free selectable)
  - ADD -> NC/GND or VCC (see below)

The ADD pin is used to set the sensor I2C address. By default (if ADD voltage
less than 0.7 * VCC) the sensor address will be 0x23. If it has voltage
greater or equal to 0.7VCC voltage (e.g. you've connected it to VCC) the
sensor address will be 0x5C.
模块与arduino的连接图如下
![Example wiring diagram image](resources/wiring-diagram-gy30-module.png)

*The image above was created using [Fritzing](http://fritzing.org/home/) and
the GY-30 module was obtained from [here](http://omnigatherum.ca/wp/?p=6)*.

### 程序
将以下程序上传到arduino.

``` c++
#include <Wire.h>
#include <BH1750.h>

BH1750 lightMeter;

void setup(){

  Serial.begin(9600);

  // Initialize the I2C bus (BH1750 library doesn't do this automatically)
  // On esp8266 devices you can select SCL and SDA pins using Wire.begin(D4, D3);
  Wire.begin();

  lightMeter.begin();
  Serial.println(F("BH1750 Test"));

}

void loop() {

  uint16_t lux = lightMeter.readLightLevel();
  Serial.print("Light: ");
  Serial.print(lux);
  Serial.println(" lx");
  delay(1000);

}
```

### 输出
将传感器移到光线较强的地方，光强值将会增加。
```
BH1750 Test
Light: 70 lx
Light: 70 lx
Light: 59 lx
Light: 328 lx
Light: 333 lx
Light: 335 lx
Light: 332 lx
```
在examples目录下还有更多例子。