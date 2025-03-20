<img alt="PCB" width="1080" align="top" src="https://github.com/YGDL/FT2232_Debugger/blob/main/photo/FT2232_Debugger.png">

<img alt="PCB" width="1080" align="top" src="https://github.com/YGDL/FT2232_Debugger/blob/main/photo/组合体.png">

# FT2232HX_Debugger

使用FDTI的FT2232HQ(FT2232HL)来制作调试器。该调试器可用于JTAG、SWD、UART、SPI、IIC应用。

# 硬件

该调试器的端口提供UART、JTAG、SWD、SPI、IIC通信端口，UART由单独的端口进行提供，JTAG|SPI和SWD|IIC由另一个开关SW2进行切换，不可同时使用。调试器还提供了三档输出电平的适配，分别为1.8V、3.3V、5.0V，并且提供外置电平输入引脚用于匹配1.8V至5.0V间的任意电平。

输出端采用2.54mm的排针进行连接，所有协议接口可以使用杜邦线单独进行连接，避免频繁的拔插引线。接口提供了三组电源，分别为1.8V、3.3V、5.0V。在USB供电充足情况下，1.8V与3.3V输出端可以提供约3A电流，用于调试目标的供电。由于该调试器没有使用电气隔离，因此不可用于强电系统的调试。

状态指示灯共有5个。串行通道的数据收发指示灯有两个，为一对RX和TX的指示灯，提供给用户查看串行通道的通信状态。FT2232H USB通信状态指示灯提供给用户当前USB通信状态。电源指示灯用于用户查看3.3V电源是否正常。电平指示灯用于查看当前输出端的电平状态。

## 主要器件

- [FT2232HQ/FT2232HL](#FT2232H)

- [SN74LVC8T245](#SN74LVC8T245)

- [93LC46/93LC56](#EEPROM)

- [SCT2230TVBR/TPS563208](#POWER)

### FT2232HQ/FT2232HL<a id="FT2232H"></a>

FT2232H是FTDI的第5代USB设备芯片。FT2232H是一个USB2.0高速（480Mbit/s）至UART/FIFO 芯片。具有在多种工业标准串行或并行接口配置的能力。在FT2232的创新功能的基础上，FT2232H有两个多协议同步串行引擎（MPSSE）允许使用JTAG，I2C和SPI两个通道同时进行通信。

>FT2232H的USB协议栈设置在芯片上，无需用户对USB进行固件编程。芯片对USB 2.0高速传输（480Mb/s）和全速传输（12Mb/s）兼容。FT2232H可对USB双串行/并行端口进行多种配置，内部双多协议串行同步引擎（MPSSE）简化同步串行协议（USB转JTAG、IIC、SPI或Bit-Bang）的设计。
>
>FT2232H可同时配置串行/并行端口，即MPSSE可配置双独立UART或FIFO端口。对于**串行端口**，具有独立的波特率(Baud Rate)发生器。当配置为异步串行UART接口，支持7或8位数据，1或2位停止位，和奇/偶/标志/空间/无奇偶校验，并且具有全硬件交握调制和解调器接口信号。RS232/RS422/RS485串口传输数据速率高达12Mbps，RS232数据速率限制由外部电平转换器决定，同时也可使用TXDEN引脚做为RS485串行应用自动传输的启用控制。在每个通道具有TX和RX LED驱动信号的选项。
>
>在**并行端口**配置下，USB到并行FIFO的数据传输速率高达10MB/s。而单通道同步FIFO模式传输高达40MB/s。对于FT245B方式FIFO接口，具有双向数据总线和简单的4线交握接口。增强的Bit-Bang传输模式能够配置RD#和WR#控制信号。
>
>FT2232H内部集成了+1.8V LDO稳压器、POR功能和芯片时脉倍频PLL(12MHz–480MHz)。还具有可配置的I/O驱动强度(4,8,12或16mA)和压摆率(Slew Rate)。工作模式的配置和USB描述符通过SPI接口外接EEPROM配置。USB端点配置为批量(Bulk)数据传输模式(高速模式下为512Byte数据包)。FTDI公司提供免费虚拟COM端口(VCP)和直接(D2XX)驱动程序，解决大多数情况下的USB驱动程序开发的难度。

**[FT2232H数据手册](https://ftdichip.cn/Support/Documents/DataSheets/ICs/DS_FT2232H.pdf)**

### SN74LVC8T245<a id="SN74LVC8T245"></a>

**SN74LVC8T245**具有可配置双电源轨的8位同相总线收发器，支持双向电压电平转换。SN74LVC8T245经过优化，可在V<sub>CCA</sub>和V<sub>CCB</sub>设置为1.65V至5.5V的范围内正常运行。A端口输出电平跟随V<sub>CCA</sub>。V<sub>CCA</sub>工作电压范围为1.65V~5.5V。B端口输出电平跟随V<sub>CCB</sub>。V<sub>CCB</sub>工作电压范围为1.65V~5.5V。可用于实现1.8V、2.5V、3.3V和5.5V电压节点间的双向缓冲。

SN74LVC8T245可实现两条数据总线间的异步通信。方向控制(DIR)和输出使能(OE)的电平高低可控制为B端口输出或者A端口输出，或者将两个输出端口都置于高阻抗模式。当B端口输出被使能时，此器件将数据从A总线发送到B总线，而当A端口输出被使能时，此器件将数据从B总线发送到A总线。A端口和B端口上的输入电路一直处于使能状态并且必须施加一个高电平或低电平，从而防止过大的I<sub>CC</sub>和I<sub>CCZ</sub>。

该器件完全符合使用I<sub>off</sub>的部分断电应用的规范要求。I<sub>off</sub>电路禁用输出，从而可防止其断电时破坏性电流从该器件回流。V<sub>CC</sub>隔离特性可确保只要有任何一个 V<sub>CC</sub>输入接地(GND)，则所有输出均处于高阻抗状态。为了确保加电或断电期间的高阻抗状态，OE应通过一个上拉电阻器被连接至 V<sub>CC</sub>；该电阻器的最小值由驱动器的电流吸收能力来决定。

**[SN74LVC8T245数据手册](https://www.ti.com.cn/cn/lit/gpn/sn74lvc8t245)**

### 93C46/93C56<a id="EEPROM"></a>

FT2232H推荐使用型号为93C46、93C56、93C66，这些EEPROM通信使用串行接口，并且EEPROM需要能够配置或固定为16bits模式，并且IO电平为3.3V。部分可用EEPROM数据手册如下。

**[FM93C56A数据手册](https://www.fmsh.com/nvm/FM93C46A-56A-66A_ds_eng.pdf)**

**[M93C46数据手册](https://www.st.com/resource/en/datasheet/m93c46-w.pdf)**

**[FT93C56A数据手册](https://www.fremontmicro.com/#/eeprom/1-1)**

**[93LC56BT数据手册](https://ww1.microchip.com/downloads/aemDocuments/documents/MPD/ProductDocuments/DataSheets/93AA56X-93LC56X-93C56X-2-Kbit-Microwire-Compatible-Serial-EEPROM-Data-Sheet.pdf)**

**[93LC46B数据手册](https://ww1.microchip.com/downloads/aemDocuments/documents/MPD/ProductDocuments/DataSheets/93AA46A-B-C-93LC46A-B-C-93C46A-B-C-1-Kbit-Microwire-Compatible-Serial-EEPROM-Data-Sheet-DS20001749.pdf)**

### SCT2230TVBR/TPS563208<a id="POWER"></a>

调试器的对外提供除USB供电外的3.3V电源与1.8V电源，这两个电源轨使用集成FET的同步整流控制器。在USB供电充足下，每个电源轨可以对外提供3A的最大电流。

TPS563208最大的特点为轻载下进入强制连续模式(FCCM)。该模式下输出的纹波会降低，基本与满载情况下相当。但该模式下的轻载效率较低，但对于该应用来说，轻载下的效率无关紧要。该控制器使用了德州仪器的D-CAP2<sup>TM</sup>控制方案，能够提供优秀的快速瞬态响应。

>+ 输入电压范围 4.5V~17V
>+ 输出电压范围 0.76V~7V
>+ 开关频率 560kHz
>+ 静态关断电流 <20μA
>+ 带CBC限制
>+ 固定1ms软起动时间

**[SCT2230TVBR数据手册](https://www.silicontent.com//uploads/20241212/ee247be5b939ca168199e2cbed1557c4.pdf)**

**[TPS563208数据手册](https://www.ti.com.cn/cn/lit/gpn/tps563208)**

## Layout

Layout使用Altium进行设计。该PCB设计为4层，其层叠设计如下。

| PCB层叠 |
| :-------: |
| Top Layer |
| GND Layer |
| Power Layer |
| Buttom Layer |

Layout时极限参数如下

>+ 过孔参数&nbsp&nbsp
>+ 线径与线间距：

# 结构

结构设计软件为Solidworks，设计部分中较难的是拨动开关的部分由于开关行程较大，且需带防尘设计，因此开关帽会显得极长。

## 工艺

由于壳体采用光固化树脂打印（省钱），因此厂家工艺的最小壁厚为0.8mm。由于整体的体积都较小，所以设计较为困难。
