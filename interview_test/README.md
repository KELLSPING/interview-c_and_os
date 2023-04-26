# Interview Test #

```C
/* Hi, this is Interview Test page. */
```


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
