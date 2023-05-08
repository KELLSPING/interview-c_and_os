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

## Hon Hai ##

* 基本數據型態佔據多少 bytes
  * 考慮 32 bits 或 64 bits

* 假設要從 32 位元寬的 register 讀取/寫入，宣告一個適當的變量來存取中間值。

  ```C
  #include <stdint.h>
  uint32_t reg_value;
  ```

* 宣告是否有效? 並且代表甚麼意思
  * const (char)* c1
    * 有效
    * c1 是一個"指向字元常量的指標"。指針的指向可以變，指向的內容不可以變。
  * (char) const * c2
    * 有效
    * c2 是一個"指向常量字元的指標"。指針的指向可以變，指向的內容不可以變。
  * (char*) const c3
    * 有效
    * c3 是一個"指向字元的常量指標"。指針的指向不可以變，指向的內容可以變。
  * (char*) c4 const
    * 無效
    * 語法錯誤，應該將 const 放置於識別字前面。

* 宣告一個新的 structure，裡面包含一個整數

  ```C
  struct my_struct {
    int my_integer;
  };
  ```

* 給定一個指向整數的指針，該整數屬於您在前面問題中定義的結構類型變量。寫一個宏定義來找到包含該整數的變量。

  ```C
  struct my_struct example;
  int *p = &example.my_integer;
  ....
  ```

* 寫一個函數，接受一個 32-bit 的整數，並回傳該正數除以 256 的餘數。

  ```C
  #include <stdint.h>
  uint32_t remainder_of_mod_256(uint32_t input){
      // return input % 256; // method 1
      return input & 0xFF; // method 2
  }
  ```

* 請解釋什麼是循環緩衝區 (circular buffer)?

* 如何判斷循環緩衝區是空還是滿？

* 一個單項鏈結串列形成了一個環形。假設你知道 head 節點，寫一個 function 去檢測環形中 head 節點
