<img alt="PCB" width="1080" align="top" src="https://github.com/YGDL/FT2232_Debugger/blob/main/photo/FT2232_Debugger.png">

# FT2232HX_Debugger

使用FDTI的FT2232HQ(FT2232HL)来制作调试器。该调试器可用于JTAG、SWD、UART、SPI、IIC应用。

# 硬件

该调试器的端口提供UART、JTAG、SWD、SPI、IIC通信端口，UART由单独的端口进行提供，JTAG|SPI和SWD|IIC由另一个开关SW2进行切换，不可同时使用。调试器还提供了三档输出电平的适配，分别为1.8V、3.3V、5.0V，并且提供外置电平输入引脚用于匹配1.8V至5.0V间的任意电平。

输出端采用2.54mm的排针进行连接，所有协议接口可以使用杜邦线单独进行连接，避免频繁的拔插引线。接口提供了三组电源，分别为1.8V、3.3V、5.0V。在USB供电充足情况下，1.8V与3.3V输出端可以提供约3A电流，用于调试目标的供电。由于该调试器没有使用电气隔离，因此不可用于强电系统的调试。

状态指示灯共有7个。两个通道的数据收发指示灯共有四个，每个通道具有一对RX和TX的指示灯，提供给用户查看通信状态。FT2232 USB通信状态指示灯提供给用户当前USB通信状态。电源指示灯用于用户查看3.3V电源是否正常。电平指示灯用于查看当前输出端的电平状态。

## 主要器件
- [FT2232HQ(FT2232HL)](###FT2232)
- [STC](###STC)
- [93C]
- [SN74LVC8T245]


### FT2232

XXXXX

### STC

XXX
