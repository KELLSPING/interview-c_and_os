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

## Garmin ##

* 哪一行編譯會錯誤

```C
// foo 是一個指向字元常量的指標，指標的指向可以變，但指向的內容不可以變
const char *foo = "wow";  
foo = "top";
foo[0] = 1; // error: assignment of read-only location '*foo'
```

* In C language, when a global variable may be modified by an exception handler, it should be declared as ?
  * volatile

* In C++, 下列問題請回答 true 或 false

    ```C
    int ia[] = {0,1,1,2,3,5,8,13,21};

    ia[1] == *(ia+1)
    &ia[2] == ia+2
    (&ia[2] - &ia[0]) == 2
    ```

  * 1 == 1, true
  * 89bffdc8 == 89bffdc8, true
  * 2 == 2, true

* 更正程式碼

  ```C
  void AddData(void *data, unsigned int value, int index){
      data[index] = value;
  }

  int main(void){
      unsigned int arr[20];
      AddData(arr, 10, 5);
      return 0;
  }
  ```

  * warning: dereferencing 'void *' pointer
  * 將 void \*data 更改為 unsigned int *data

* 更正程式碼

  ```C
  void swqp(int *a, int *b){
    int *temp;
    temp = a;
    a = b;
    b = temp;
  }
  ```

  ```C
  // 更正後
  void swap(int *a, int *b){
    int temp;
    temp = *a;
    *a = *b;
    *b = temp;
  }
  ```

* 輸出顯示

  ```C
  typedef union {
      int i;
      unsigned char ch[3];
  }Student;

  int main(void){
      Student student;
      student.i = 0x1420;
      printf("%d\n", sizeof(student));
      printf("%d\n", student.ch[0]);
      printf("%d\n", student.ch[1]);
      printf("%d\n", student.ch[2]);
      return 0;
  }
  ```

  * 4, 32 (ascii SP), 20 (ascii DC4), 0 (ascii NUL)

* 回傳值是多少

  ```C
  int fun(int x){
    int count = 0;
    while (x){
      count++;
      x = x & (x - 1);
    }
    return count;
  }
  ```

  * if input x is 51
    * 4
  * if input x is 1025
    * 2

* 請說明"靜態記憶體配置"與"動態記憶體配置"的差異

* Write a function to reverse digits of an integer - int reverse(int x), the answer should return 0 when the reversed integer overflows.

  * input x=1234, return 4321

  ```C
  #include <limits.h>
  int reverse(int n){
    int reversed = 0;
    if (n > INT_MAX){
        return 0;
    }
    while (n != 0){
        int remainder = n % 10;
        reversed = reversed * 10 + remainder;
        n /= 10;
    }
    return reversed;
  }
  ```

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
