# W25Q64
STM32F103C8T6_W25Q64
![image](https://github.com/user-attachments/assets/4cc24f25-bf99-4fb8-9444-61a558bbc26f)
1. 整个存储器被划分为block，block划分为sector，sector划分为page
   1block=64kByte，1sector=4kByte，1page=256bytes
2. 图中的Status register是用来说明，当前状态，比如忙状态，写使能，写保护
3. 地址位是24位，即3bytes，前2bytes进入到图中Page Address counter，最后1bytes进入到图中Byte Address counter来找到对应是哪一个byte
4. 图中256-Byte page buffer本质就是ram，为什么需要这个呢：因为SPI发数据是很快的，但是flash是非易失寄存器，他的读写很慢，我需要把SPI数据先全部写到ram里面。等我把数据写完之后，ram再慢慢把数据写到flash里面。所以每次写完，ram都会回复给status register一个busy的信号表示他忙，在把数据传到flash里面。
![image](https://github.com/user-attachments/assets/38b58258-8dc6-45b3-b446-2756c2d5fbcd)
1. 写入比较麻烦，读取比较简单。因为写入要对flash里面产生改变得到掉电不丢失的结果，读比较简单只要看一下状态就可以了。每次写之前都要写使能，等于是玩手机前密码要解锁。写完之后，写使能寄存器会自己把自己改成写失能，等于是顺手关门了。
2. 根据第二条规定，举例来说：比如我要把0xAA=10101010改写为0x55=01010101，那么只能改写为0x00000000
3. 所以写之前必须要擦除，写之前需要擦除，擦除后所有的位变为1.（ram是不需要擦除这一步骤的）
4. 最小的擦除单元是1个扇区，1扇区=4KBytes=4096Bytes。如果你怕擦除时候会擦掉其他的data，就需要把原来的整个扇区数据读出来，改好要改的那个byte再写回去。
5. 写入的时候最多只能写256bytes，如果写多了就会覆盖掉最开始写的那个。
![image](https://github.com/user-attachments/assets/31e61275-3d91-4deb-8fed-b10eb2572e9e)

注意数据手册里面的写时间。扇区写时间，块写时间等

SPI的代码部分，传输数据流程如下：上升沿移入数据，下降沿移出数据
SS下降沿--->移出第一个数据--->SCK上升沿--->移入一个数据--->SCK下降沿--->移出第二个数据
![image](https://github.com/user-attachments/assets/d6a6f41c-08aa-454e-bc7d-4c9700b17786)


