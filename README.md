# W25Q64
STM32F103C8T6_W25Q64
![image](https://github.com/user-attachments/assets/4cc24f25-bf99-4fb8-9444-61a558bbc26f)
1. 整个存储器被划分为block，block划分为sector，sector划分为page
   1block=64kByte，1sector=4kByte，1page=256bits
2. 图中的Status register是用来说明，当前状态，比如忙，比如写保护
3. 地址位是24位，即3bytes，前2bytes进入到图中Page Address counter，最后1bytes进入到图中Byte Address counter来找到对应是哪一个byte
4. 图中256-Byte page buffer本质就是ram，为什么需要这个呢：因为SPI发数据是很快的，但是flash是非易失寄存器，他的读写很慢，我需要把SPI数据先全部写到ram里面。等我把数据写完之后，ram再慢慢把数据写到flash里面。所以每次写完，ram都会回复给status register一个busy的信号表示他忙。
