# Interview Test #

```C
/* Hi, this is Interview Test page. */
```

## Realtek ##

* 位元操作

* 指標特性

* Big-Endian 與 Little-Endian

* 二元樹反轉

* memory leak

* call by value, call by reference

## ASolid ##

* Bitwise

* data structure

## Innolux ##

* 位元運算

  * 輸入 4 bytes data ，如果最高位元為 A 和次高位元為 B ，則返回 data 的最低位元和次低位元；如果最高位元不等於 A ，或次高位元不等於 B ，則返回 0xFFFF。

    ```C
    #include <stdint.h>

    #define CHECKSUM 0xA
    #define DISPLAYID 0xB

    uint32_t func(uint32_t input)
    {
        if (((input & 0xF000) >> 12) == CHECKSUM)
        {
            if (((input & 0x0F00) >> 8) == DISPLAYID)
            {
                return input & 0x00FF;
            }
        }
        return 0xFFFF;
    }
    ```

## Gemtek ##

* 指標操作
* bitwise (AND/OR)
* 字串做反向
* Linux
* Network

## CC2541 OSAL ##

* 作業系統層抽象層 (operating system abstraction layer, OSAL)

* 一種支援多任務運行的系統資源分配機制。但還不能算是真正的操作系統。

## RTOS 與 Linux 的區別 ##
