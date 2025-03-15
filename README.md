# 改进版3.7V锂电池充电模块
## 有什么用？
折腾电路的时候，经常用的是一个通过USB给3.7V 18650锂电池或者电池包形式的锂电池（聚合物电池）充电，同时带有电池保护以及“边充边放”功能（就是如果插着USB给电池充电的时候，负载由USB供电，如果拔下USB，负载由电池供电），并且支持MCU下载，这样一个简单模块，在淘宝上各种找，就是找不到好用的，可见到的是这么几类：
1. 充电管理+升压电路：直接可用来做充电宝。
2. 充电管理+电池保护：接锂电池USB充电
3. 支持太阳能板的充电管理，无电池保护：接锂电池和太阳能板

这些模块的设计者不太像是电路爱好者，设计都比较诡异，并且无法扩展，我碰到的痛点是：
1. 没有边充边放设计：淘宝上买的锂电池充电模块都没有边充边放的设计，这个基本是必备功能
2. 电池保护功能是鸡肋：模块自带了电池保护功能，适合18650锂电，但是如果用聚合物锂电池，这些电池自带保护电路。。。
3. 不引出USB的D+、D-：“通过USB下载MCU”基本是个必备功能，如果需要外挂一个USB转串口的模块得各种焊飞线，USB口的飞线一不小心就焊坏
4. 尺寸和面包板不兼容（最XX的！）：插脚间距不是100mil整数倍，横竖插不到面包板上或者焊接到万能板上。。。
5. 不支持电池过热保护：TP4056可以通过NTC提供电池过热保护功能，但是淘宝货都没有引出这个脚甚至直接接地。。。

实在忍不了。故此鼓捣一个更符合我习惯的模块。

## 怎么用？
1. 如果电池不带保护电路，焊接R6，如果带保护电路，焊接R7（同时不用焊接U2、U3、R4、R5、C3）
2. 电池正负极分别焊接B+和B-（如果电池带保护电路也一样）
3. O+、O-是输出（可接升压模块做充电宝），O-也同时是模块的Gnd
4. D+、D-是USB数据线（可接USB-串口转换）
5. I-、I+是外置输入，可以外挂其他5V输出的模块例如太阳能板管理
6. TP是过热保护，替换R8为合适的电阻并使用TP连接电池的NTC，具体请参考TP4056的数据手册

## 关键参数
1. 输入：5V/1A Max
2. 输出：电池电压或者USB电压-0.7V/1A Max

# Improved 3.7V Lithium Battery Charging Module
## What is it for?
When tinkering with circuits, I often need a simple module that:
- Charges 3.7V 18650 lithium batteries or polymer battery packs via USB
- Provides battery protection
- Supports "charging while discharging" (USB powers the load when connected, battery powers the load when USB is disconnected)
- Allows MCU firmware downloads

Surprisingly, no satisfactory modules could be found on Taobao. Existing modules fall into three categories:
1. Charging management + boost circuit: Directly usable as power banks
2. Charging management + battery protection: For USB charging of lithium batteries
3. Solar panel charging management (no battery protection): For lithium batteries and solar panels

These modules seem designed by non-enthusiasts with odd implementations and poor expandability. Key pain points:
1. No charging-while-discharging functionality: Essential feature missing in all Taobao modules
2. Redundant battery protection: Built-in protection is redundant for polymer batteries with existing protection circuits
3. Missing USB D+/D- pins: Requires messy soldering for MCU programming via USB
4. Breadboard incompatible (most annoying!): Pin spacing not in 100mil multiples
5. No thermal protection: TP4056 supports NTC-based thermal protection, but Taobao modules either ground this pin or ignore it

This frustration led me to develop a module better suited to my needs.

## How to use?
1. Battery protection:
   - Solder R6 for unprotected batteries
   - Solder R7 for protected batteries (omit U2, U3, R4, R5, C3)
2. Battery connection: Connect to B+/B- terminals (same for protected batteries)
3. Output: Use O+/O- (O- shares module ground)
4. USB interface: D+/D- for USB-to-serial conversion
5. External input: Connect 5V sources (e.g. solar panels) to I+/I-
6. Thermal protection: Replace R8 with appropriate resistor and connect TP to battery NTC (refer to TP4056 datasheet)

## Key Specifications
1. Input: 5V/1A Max
2. Output: Battery voltage or (USB voltage - 0.7V)/1A Max
