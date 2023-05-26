# C Language #

```C
/* Hi, this is C Language page. */
```

## Table of contents ##

* [定義 (definition)](#定義-definition)
* [基本題](#基本題)
* [Bitwise](#bitwise)
* [Data Type](#data-type)
* [Data Structure](#data-structure)
* [Loops](#loops)
* [I/O](#io)
* [Keyword](#keyword)
* [Pointer](#pointer)
* [函式指標 (function pointer)](#函式指標-function-pointer)
* [Preprocessor](#preprocessor)
* [String](#string)
* [C Standard Library](#c-standard-library)
* [Algorithm](#algorithm)

## 定義 (definition) ##

### ASCII 字符集 ###

* 在 ascii 中，包含 128 個字符，其中前 32 個，即 0~31 (0x00~0x1F) ，為不可見字符，這些字符叫做控制字符。
* ascii 中，第 127 個字符對應的是 delete ，也是不可見的。
* 這些字符皆對應著一個特殊的控制空能的字符，簡稱功能字符或功能碼 (function code) 。
* 此類字符，對應不同的功能，發揮一定的控制作用，所以也稱為控制字符。

  | 十進制 (dec) | 十六進制 (hex) | 控制字符 | 轉義字符 | 說明 | Ctrl+字母 |
  | --- | --- | --- | --- | --- | --- |
  | 0 | 0x00 | %NUL |  | Null character (空字符) | @ (Shift + 2) |
  | 8 | 0x08 | BS | b | Backspace (退格) | H |
  | 10 | 0x0A | LF | n | Line feed (換行鍵) | J |
  | 13 | 0x0D | CR | r | Carriage return (回車鍵) | M |
  | 20 | 0x14 | DC4 |  | Device Control 4 (設備控制 4) | T |
  | 24 | 0x18 | CAN |  | Cancel (取消) | X |
  | 32 | 0x20 | SP |  | White space | [Space] * |
  | 127 | 0x7F | DEL |  | Delete (刪除) | ?* |

### 變數範圍和生命周期 ###

* 區域變數 (loacl variable): local 變數僅活在該函式內，存放位置在 stack 或 heap 記憶體中。
* 靜態變數 (static variable)
  * 在編譯時，就已配置固定的記憶體空間，使靜態變數的值得以保存。
  * 再次呼叫該函數時，會將靜態變數存在記憶體空間中的值取出來用。
  * 作用
    1. 在函數本體內，一個被宣告為靜態的變數，在這一函數被呼叫過程中，其值不變。
    2. 在一個 block 中，一個被宣告為靜態的變數，可以被 block 內的所有函數存取，但不能被 block 外的其他函數存取。是一個本地的全局變量。
    3. 在一個 block 中，一個被宣告為靜態的函數，可以被 block 內的其他函數呼叫，但不能在 block 外被呼叫。
* 全域變數 (global variable): 所有區段皆可使用此變數。

### 複雜度 ###

* 時間複雜度 (Time Complexity)
  * Big-O
    * f(n) = Ο(g(n))
  * 排序: n (常數) < log n < n < n log n < n^2 < 2^n < n!
    1. 常數時間 (Constant Time) => O(1)，例如：陣列讀取
    2. 對數時間、次線性時間 (Logarithmic Time) => O(log n)，例如：二分搜尋
    3. 線性時間 (Linear Time) => O(n)，例如：簡易搜尋
    4. 線性乘對數時間 (Quasilinear Time) => O(n log n)，例如：合併排序
    5. 次方時間 (Quadratic Time) => O(n^2)，例如：選擇排序
    6. 指數時間 (Exponential Time) => O(2^n)，例如：費式數列
    7. 階乘時間 (Factorial Time) => O(n!)
* 空間複雜度 (Space Complexity)

### 傳值呼叫、傳址呼叫、參考呼叫 ###

* 傳值呼叫 (call by value)
  * 複製記憶體。副程式與主程式的變數是存放在個別的記憶體中。

  ```C
  int callByValue(int a, int b){...}
  ```

* 傳址呼叫 (call by address, call by pointer)
  * 共用記憶體。副程式與主程式的變數是使用同一個記憶體。

  ```C
  int callByAddress(int *a, int *b){...}
  ```

* 參考呼叫 (call by reference): 只能用在 C++

  ```C++
  int callByReference(int &a, int &b){...}
    ```

### 編譯與執行的過程 ###

* source code (.c, .h) -> Preprocessor (.c) -> Compiler (.s) -> Assembler (.o) -> Linker (.exe) -> Loader  -> CPU
  1. 原始碼 (source code): 副檔名為 .c，標頭檔副檔名為 .h。
  2. [Build] 預處理器 (Preprocessor): 將須引入的檔案或是 macro 展開。
  3. [Build] 編譯器 (Compiler): C 語言常見的編譯器有 GCC、Clang，在此 GCC 為廣義的編譯器，可以達到 build 的所有功能。狹義的編譯器，主要就是將 C 語言轉成組合語言，產出 .s, .asm 檔。
  4. [Build] 組譯器 (Assembler): 將低階語言所寫的程式翻譯成目的檔，為 .o 檔。
  5. [Build] 連結器 (Linker): 將多個目的檔或靜態函式庫 (Static library, .a, .lib) 合併成一個可執行檔 (.exe, .out) 或函式庫的工具。
  6. [Run] 載入器 (Loader): 是作業系統的一部份，用於把程式和動態函式庫 (Shared library, .so, .dll) 的指令載入到記憶體 (RAM) 中等待 CPU 執行，當載入完成之後，作業系統會將控制權交給載入的程式碼，讓它開始運作。
  7. [Run] CPU: 對載入的指令進行運算或儲存等操作。
* .a 檔與 .exe 檔的差別
  * .exe 檔中，有包含 main () 的 source code 。

### 偵錯 (Debug) ###

* 語法錯誤 (Syntax error): 不符合語言的規定，將編譯後所指出的錯誤修正，再重新編譯即可。
* 語意錯誤 (Semantic error): 程式本身的語法沒有問題，但邏輯上可能有瑕疵，造成非預期性的結果。
* 記憶體區段錯誤 (Segmentation fault, 縮寫 segfault): 也稱存取權限衝突 (access violation)，是一種程式錯誤。

### 識別字及關鍵字 ###

* 識別字 (Identifier): 使用者自訂的變數與函數名稱。
* 關鍵字 (Keyword): 編譯程式本身使用的識別字。

### struct 與 class 的差別 ###

### 最佳化方法種類 ###

* 硬體
  1. 增加 cache : 在 CPU 和記憶體中間增加 cache，解決 CPU 和記憶體之間執行速率差異過大的問題。
* 軟體
  1. 編譯器最佳化 : 在編譯連結時由編譯器進行優化，會調整程式碼的執行順序或者刪掉一些無用的語句。將記憶體中讀取的資料存到 cache 或 暫存器中。
  2. 程式設計最佳化 : 編寫程式碼時，對程式碼的邏輯順序進行合理安排，提升效率。

## 基本題 ##

* 列印輸出

  ```C
  int i;
  i = (1, 2, 3);
  printf("i =%d\n", i); // i =3
  ```

* 列印輸出

  ```C
  int first = 50, second = 60, third;
  third = first /* Will this comment work? */ + second;
  printf("%d /* And this? */ \n", third);
  ```

* 不同資料型態的零值比較

  ```C
  /* bool */
  if (flag) {...}
  if (!flag) {...}

  /* float */
  #define EPSILON 0.00001 // EPSILON 小於 10的-6次
  if ((x >= - EPSILON) && (x <= EPSILON)) {...}

  /* pointer */
  if (p == NULL) {...}
  if (p != NULL) {...}
  ```

## Bitwise ##

* 補數運算

  ```C
  printf("%x\n", 0); // 0
  printf("%x\n", 1); // 1
  printf("%x\n", 2); // 2
  printf("%x\n", -1); // ffffffff
  printf("%x\n", -2); // fffffffe
  printf("%x\n", -1 << 1); // fffffffe
  ```

  ```C
  unsigned int zero = 0;

  /* 1's complement of zero */
  unsigned int compzero = 0xFFFF; // 在 16 bits 的處理器是正確的，但其他會出錯
  unsigned int compzero = ~0; // 正確的作法
  ```

* 給定一個整型變量 a，寫兩段程式碼，第一個設置 a 的 bit 3，第二個清除 a 的 bit 3

  ```C
  #define BIT3 (0x1 << 3)
  static int a;
  void set_bit3(void) {  a |= BIT3; }
  void clear_bit3(void) {  a &= ~BIT3; }
  ```

* 基本運算

    ```C
    unsigned long v1 = 0x00001111;
    unsigned long v2 = 0x00000202;
    unsigned long v;

    v = v1 & (~v2);
    printf("num_c=%x\n", v); // 1111

    v = v | v2;
    printf("num_c=%x\n", v); // 1313
    ```

* mask 方法做 bitwise 操作

    ```C
    a = a | 7    // 最右側 3 位設為 1，其餘不變。
    a = a & (~7) // 最右側 3 位設為 0，其餘不變。
    a = a ^ 7    // 最右側 3 位執行 NOT operator，其餘不變。
    ```

* 辨別 N 是不是 2 的冪次

  ```C
  int isPowerOf2(int N){
      while (N > 1){
          if ((N%2) == 0) {
              N /= 2;
              if (N == 1) {
                  return 1;
              }
          } else {
              break;
          }
      }
      return 0;
  }
  ```

  ```C
  int isPowerOf2(int N){
      return N>0 && (N&(N-1)) == 0;
  }
  ```

* 給一個 unsigned short, 問換算成16進制後四個值是否相同? 若是回傳1,否則回傳0

    ```C
    #include <stdint.h>

    int isHexEqaul(uint16_t input){
        uint16_t hex[4];
        uint8_t is_eqaul;

        hex[0] = (input & 0xF000) >> 12;
        hex[1] = (input & 0x0F00) >> 8;
        hex[2] = (input & 0x00F0) >> 4;
        hex[3] = input & 0x000F;

        is_eqaul = ((hex[0] == hex[1]) && (hex[1] == hex[2]) && (hex[2] == hex[3]));
        return is_eqaul;
    }
    ```

* 求一個數的最高位1在第幾位

    ```C
    #include <stdint.h>
    void searchHighestBit(uint8_t N)
    {
        uint8_t len = sizeof(N) * 8;
        uint8_t mask = 0x80;
        if (N == 0)
        {
            printf("N is 0");
        }

        while (N != 0)
        {
            if ((N & mask) != 0)
            {
                printf("hightest bit is %d th bit\n", len - 1);
                break;
            }
            mask = mask >> 1;
            len--;
        }
    }
    ```

* 求 fun(456) + fun(123) + fun(789) = ?

    ```C
    // ans = 4 + 6 + 5 = 15
    int fun(int x){
        int count = 0 ;
        while(x){
            count++ ;
            x = x & (x-1) ;
        }
        return count ;
    }
    ```

* 當表達式中，存在有符號類型和無符號類型時所有的操作數都自動轉換為無符號類型 (unsigned)，因
此 -20 變成了一個非常大的正整數。

    ```C
    void foo(void)
    {
        unsigned int a = 6;
        int b = -20;
        (a+b > 6) ? puts("> 6") : puts("<= 6");
    }
    ```

* 在一個數值中計算幾個 bit 為 1

  ```C
  int countBitOne(int x){
    int count = 0;
    while (x){
        count++;
        x = x & (x - 1);
    }
    return count;
  }
  ```

* 將一個數值的奇偶 bit 互換

  ```C
  x = (x&0xaaaaaaaa)>>1 | (x&0x55555555)<<1;
  ```

* bit 清除後三碼

  ```C
  x &= (~0)<<3;
  ```

## Data Type ##

### CPU type ###

* x86_64, x64, AMD64 皆可以代表為 64-bits CPU。

### 基本資料型態 (Primitive Data Types) ###

|  | 32-bits CPU (x86) | 64-bits CPU (x86_64) | format str | #include <stdint.h> |
| --- | --- | --- | --- | --- |
|  | Size(bytes) | Size(bytes) |  | Typedef |
| _Bool | 1 | 1 | %i、%d |  |
| char | 1 | 1 | %c | uint8_t |
| short (int) | 2 | 2 | %hi、%hd | uint16_t |
| int | 2 | 4 | %i、%d | uint32_t |
| long (int) | 4 | 8 | %li、%ld | uint64_t |
| long long | 4 | 8 | %lli、%lld |  |
| float | 4 | 4 | %f、%e、%g |  |
| double | 8 | 8 | %lf、%e、%g |  |
| long double | 12 | 16 | %lf、%le、%lg |  |
| size_t | 4 | 8 |  |  |
| pointer | 4 | 8 | %p |  |
| string | 4 | 8 | %s |  |

* int 考題

```C
#include <limits.h>

printf("The minimum value of INT = %x", INT_MIN); // 0x80000000
printf("The maximum value of INT = %x", INT_MAX); // 0x7fffffff
```

### 複合資料型態 (Compound Data Types) ###

* 結構 (struct)
  * 產生一種新的資料型態，每個成員變數都配置一段空間
  * 在 C 和 C++ 中，struct 用來包裝不同型態的資料。
  * 在 C++ 中，如果要針對內部成員做複雜處理的時候，都使用 class。
  * 位元欄位 (Bit fields)
  * 指定、取址、使用結構成員運算子 '.' 跟使用結構指標運算子 '->'
* 聯合 (union)
  * 產生一種新的資料型態，共用一段記憶體空間，所需的記憶體空間大小由最大的成員變數覺得
  * 指定、取址、使用結構成員運算子 '.' 跟使用結構指標運算子 '->'
* 列舉 (enum)
  * 一組由識別字所代表的整數常數
  * 除非特別指定，不然都是由0開始，接下來遞增1

### Union ###

* union 考題

  ```C
  union StateMachine {
      char character;
      int number;
      char *str;     //32-bits: 4     64-bits: 8
  };
  int main(void) {
      union StateMachine machine;
      machine.number = 1;
      printf("sizeof: %lu\n", sizeof(machine));
      printf("number: %d\n", machine.number);
      return 0;
  }
  ```

## Data Structure ##

* array

* Linked List
  * 說明
    * 是一種線性序列結構，使用指針指向下一個位址，充分利用記憶體空間。
  * 若存放串列元素的記憶體是循序的，則此串列稱為循序串列 (sequential list)；若存放串列元素的記憶體並不連續，而必須以指標將他們鏈結起來，則稱為鏈結串列 (linked list)。
  * 終端節點已是最後一個節點，所以將第二個成員，亦即指標設成 NULL。
  * 常用的操作
    * 建立 create
    * 列印 print、走訪 traverse
    * 插入 insert
    * 刪除 delete
    * 搜尋 search
    * 釋放 free

* 堆疊 Stack
  * 說明
    * 後進先出 (Last In First Out, LIFO)
    * 堆疊頂部 (top), 堆疊底部 (bottom), 加入資料 (push), 移除資料 (pop)

* 佇列、隊列 Queue
  * 說明
    * 先進先出 (First-In-First-Out, FIFO)
    * 佇列首 (front), 佇列尾 (rear), 加入資料 (enqueue), 移除資料 (dequeue)
  * 種類
    * 環形佇列
    * 雙向佇列
    * 優先佇列

* 堆積 Heap

* Tree

  * 種類
    * 二元樹 (Binary Tree)
    * 二元搜尋樹 (Binary Search Tree, BST)
    * 平衡樹 (Self-balanced Binary Search Tree, height-balance binary search tree)

* Graph

* Hash table

### Linked List ###

* Sorted Liked List - Insertion

## Loops ##

* 遞迴函數 (Recursive function)
  * 函數本身呼叫自己
  * 舉例
    * 階層函數 (factorial function)
    * 河內塔 (hanoi tower)
    * 費式數列 (fibonacci)
    * 二元數搜尋法 (binary search)
    * 最大公因數 (highest common factor，hcf)
    * 十進位轉換成二進位
  * 優點
    * 使程式碼變得簡潔。
  * 缺點
    * 占用記憶體，存放未執行完畢的部分。
    * 當遞迴函數的層數很大時，就必須要有較大的記憶體。
  * 注意事項
    * 一定要有終止條件，使函數得以返回上層呼叫的地方。
    * 並非以遞迴的方式所撰寫的函數執行起來就比較有效率。

* 無窮迴圈 (Infinite loops)

    ```C
    while(1){...}
    ```

    ```C
    for(;;){...}
    ```

* 費氏數列 (Fibonacci Sequence)

    ```C
    // 1, 1, 2, 3, 5, 8, 13, 21, 34, 55
    int fib(int n){
        if ((n==1)|| (n==2))
            return 1;
        return (fib(n-1)+fib(n-2));
    }
    ```

* 最大公因數、遞迴寫法

    ```C
    // x > y
    int gcd(int x, int y){               
        if (y == 0) // 餘 0，除數 x 即為最大公因數
            return x;
        else
            return gcd(y, x % y); // 前一步驟的除數為被除數，餘數為除數
    }
    ```

* 尋找質數 (find prine number)

## I/O ##

* 輸出函數 printf()
  * printf with modifiers
    | modifiers | func | example |
    | --- | --- | --- |
    | 數字 | 3個欄位寬度 | %3d |
    | 數字 | 6個欄位寬度，小數點佔2個 | %6.2f |
    | - | 靠左對齊 | %-3d |
    | + | 將數值的正負號顯示出來 | %+3d |
    | 空白 | 數值為正時，留一格空白；為負時，顯示負號 | % 6f |
    | 0 | 將固定欄位長度的數值前空白處填上0。(與負號同時使用時，此功能無效) | % 6f |

* 輸入函數 scanf()
  * 使用 scanf() 函數應注意的事項
    * 在 Dos 和 Windows 中，輸入結束後，按下 enter ，scanf() 將接收到歸位字元 (carriage return, \r) ，便會判定輸入完畢，換行字元 (line feed, \n) 將留在輸入緩衝區 (buffer) 中。下一次 scanf() 讀取字元時，跳過不可列印字元即可，亦即在 %c 前面加一個空白。

    * 處理 buffer 中的不可列印字元
      1. scanf 中，在 %c 前面加上一個空格，跳過不可列印字元。

          ```C
          scanf(" %c", &ch);
          ```

      2. 吸收空格這個字元

          ```C
          getchar();
          ```

* 輸入函數 get
  * gets(): 允許字串裡有空白字元，語法比 scanf() 更簡潔。 (須按下 enter)
  * getchar(): (須按下 enter)
  * getch(): 字元不會回應在螢幕上。 (不須按下 enter)
  * getche(): e 代表 echo ，字元被接收後，將立刻回應到螢幕上。 (不須按下 enter)

* 輸出函數 put
  * putchar()
  * puts(): 輸出字串後會自動換行，所以使用的頻率較 printf() 低。

* Terminal 中，處理 signal
  * 終止正在執行的程式 (ctrl - c): ascii 碼為 3 。
    * 按下 ctrl - c 時， terminal 會發送了一個 SIGINT (中斷訊號) 給 Shell ， Shell 再把 SIGINT 轉發給 ping process ，最後 ping process 收到後就會自己停掉。
  * 暫停正在執行的程式 (ctrl - z): ascii 碼為 26 。

    ```C
    #include <conio.h>

    char ch
    while(ch != 3){
        ch = getch();
        printf("ASCII of ch=%d\n", ch);
    }
    printf("已按了 ctrl-c ...\n");
    ```

## Keyword ##

### static ###

* 說明
  1. 在函數本體內(in Function Block)，一個被宣告為 static 的變數，其值存在記憶體中，即使函數執行完畢，static 變數也不會消失。
  2. 在一個 Block ( ie. {...} ) 內 (但在函數體外)，一個被宣告為 static 的變數，可以被 Block 內所有的函數存取，但不能被 Block 外的其它函數存取。它是一個本地的全局變量。
  3. 在 Block 內，一個被聲明為 static 的函數，只可被這一 Block 內的其它函數呼叫。也就是這個函數被限制在宣告它的Block的本地範圍內使用。

### const ###

* 說明
  * const 代表只可以讀取不能寫入，使用 const 有以下好處：
    1. 提升程式碼可讀性
    2. 使編譯器保護那些不希望被改變的參數
    3. 給優化器一些附加的資訊

### volatile (易揮發的) ###

* 說明
  * 被 volatile 修飾的變數代表它可能會被不預期的更新，因此告知編譯器不對它涉及的地方做最佳化，並在每次操作它的時候都讀取該變數實體位址上最新的值，而不是讀取暫存器的值。
  * 常見的應用
    1. 修飾中斷處理程式中 (ISR) 中可能被修改的全域變數
    2. 修飾多執行緒 (multi-threaded) 的全域變數
    3. 設備的硬體暫存器 (如狀態暫存器)

### inline 函式 ###

* 說明
  * inline 可以將修飾的函式設為行內函式，即像巨集 (define) 一樣將該函式展開編譯，用來加速執行速度。
  * inline 函示只能建議編譯器，建議不一定被採納，例如遞迴函式無法在呼叫點展開，數千行的函式也不適合在呼叫點展開，如果編譯器拒絕將函式展開，會視為一般函式進行編譯，inline 的建議會被忽略。

* inline 和 #define 的差別
  1. inline 函數只對參數進行一次計算，避免了部分巨集易產生的錯誤。
  2. inline 函數的參數類型被檢查，並進行必要的型態轉換。
  3. 巨集定義盡量不使用於複雜的函數
  4. 用 inline 後編譯器不一定會實作，僅為建議。

### sizeof ###

* 說明
  * sizeof 運算子是一個一元運算子，不是函數。
  * sizeof 不會在執行時計算變數或型別的值，而是在編譯時，所有的 sizeof 都被具體的值替換。
  * 運算之結果的型別為 size_t ，其實就是無號整數 (long long unsigned int) ，在 printf() 中，需使用 %llu 。
  * 查詢常數、變數、指針或資料型態所占位元組

* 原則
  * 對齊：每一成員需對齊爲後一成員類型的倍數
  * 補齊：最終大小補齊爲成員類型最大值的倍數

    ```C
    // sizeof(struct A) = 16 bytes
    struct A{
        int a;
        short b;
        int c;
        char d;
    };
    // sizeof(struct B) = 12 bytes
    struct B{
        int a;
        short b;
        char c;
        int d;
    };
    // sizeof(struct C) = 12 bytes
    struct C{
        uint8_t a[3];
        uint32_t b[2];
    };
    ```

## Pointer ##

* 在 printf() 以 16 進位 (hex) 顯示，格式碼需使用 %p 。

  ```C
  int a = 1;
  int *ptr = &a;
  printf("address of a is %p\n", ptr);
  printf("value of a is %d\n", *ptr);
  printf("address of ptr is %p\n", &ptr);
  ```

* 用變數 a 給出下面的定義

  ```C
  const int a; // a 是一個常數型整數 (const int)
  int const a; // a 是一個 const int
  const int *a; // a 是一個 pointer，指向 const int 變數
  int const *a; // a 是一個 pointer，指向 const int 變數
  int * const a; // a 是一個常數型指標 (const pointer)，指向 int 變數
  int const * const a ; // a 是一個 const pointer，指向 const int 變數
  ```

  ```C
  /* 秘訣 : 由後往前念 */
  int a; // 一個整型數
  int *a; // 一個指向整數的指標
  int **a; // 一個指向指標的指標，它指向的指標是指向一個整型數
  int a[10]; // 一個有10個整數型的陣列
  int *a[10]; // 一個有10個指標的陣列，該指標是指向一個整數型的
  int (*a)[10]; // 一個指向有10個整數型陣列的指標
  int (*a)(int); // 一個指向函數的指標，該函數有一個整數型參數並返回一個整數
  int (*a[10])(int); // 一個有10個指標的陣列，該指標指向一個函數，該函數有一個整數型參數並返回一個整數
  ```

* 輸出是什麼?

  ```C
  char *ptr;
  if ((ptr = (char *)malloc(0)) == NULL)
      puts("Got a null pointer");
  else  puts("Got a valid pointer");
  ```

* 要求設定一個絕對位址為 0x67a9 的整數型變數的值為 0xaa55

  ```C
  int *ptr;
  ptr = (int *)0x67a9; // 將一個整數型強製轉型 (typecast) 為一指標是合法的
  *ptr = 0xaa55;
  ```

* 雙重指標 - 指向指標的指標
  * 利用雙重指標來表示二為陣列的元素
  * 利用指標表示陣列 arr\[m][n] 元素的位址  ，可寫成 \*(arr+m)+n
  * 利用指標表示陣列 arr\[m][n] 元素  ，可寫成 \*(*(arr+m)+n)
  * arr\[0][0] 的值若以指標來表示，可以寫成 **num 。

* 運算子優先序
  * 遞增遞減 (++, --) > 取值 (*) > 算術 (+, -)
    * \*ptr++ 相當於 *(ptr++)
    * \*ptr-- 相當於 *(ptr--)
    * \*ptr+1 相當於 (*ptr)+1
    * \*ptr-1 相當於 (*ptr)-1

* 指標變數建立字串 的走訪

  ```C
  while(*ptr != '\0'){ // while 走訪
    ...
  }

  for(int i = 0; *(ptr + i) != '\0'; i++){ // for 走訪
    ...
  }
  ```

* 指標函數列印數值

  ```C
  int B = 2;
  void func(int *p){p = &B;}
  int main(){
      int A = 1, C = 3;
      int *ptrA = &A;
      func(ptrA);
      printf("%d\n", *ptrA); // 1
      return 0;
  }
  ```

    ```C
  int B = 2;
  void func(int *p){p = &B;}
  int main(){
      int A = 1, C = 3;
      int *ptrA = &A;
      func(ptrA);
      printf("%d\n", *ptrA); // 2
      return 0;
  }
  ```

* 透過函式記憶體配置

  ```C
  #include <stdio.h>
  #include <stdlib.h>
  void getMemory(char** s)
  {
      *s = (char*)malloc(sizeof(char));
      printf("s = %p\n", s);
      printf("*s = %p\n", *s);
  }
  int main()
  {
      char* ch = NULL;
      getMemory(&ch);
      printf("&ch = %p\n", &ch);
      printf("ch = %p\n", ch);
      return 0;
  }
  ```

* 指標考題

  ```C
  char s[]="0113256";
  char *p=s;
  printf("%c",*p++);
  printf("%c",*(p++));
  printf("%c",(*p)++);
  printf("%c",*++p);
  printf("%c",*(++p));
  printf("%c",++*p);
  printf("%c",++(*p));
  printf("\n%s",s);
  ```

* 指標陣列考題

  ```C
  int a[] ={1,2,3,4,5,6,7,9};
  int *ptr = (int*) (&a+1);
  printf("%d\n", &a);
  printf("%d\n", &a+1);
  printf("%d\n", ptr);
  printf("%d\n", *(ptr));
  printf("%d\n", ptr-1);
  printf("%d\n", *(ptr-1));
  printf("%d\n", ptr-2);
  printf("%d\n", *(ptr-2));
  printf("%d\n", *a);
  printf("%d\n", *a+7);
  printf("%d\n", *(a+7));
  ```

* 陣列指標型態考題

  ```C
  int arr[] = {10,20,30,40,50};
  int *ptr1 = arr;
  int *ptr2 = arr+5;
  printf("%d\n", (ptr2-ptr1));               //5
  printf("%d\n", (char*)ptr2 - (char*)ptr1); //20
  ```

* 多重陣列考題

  ```C
  char *str[ ][2] =
    {"professor","Justin","teacher","Momor","student","Caterpillar"};
  char str2[3][10] = {"professor", "Justin", "etc"};
  printf("%s\n", str[1][1]); //Momor
  printf("%c\n",str2[1][1]); //u
  ```

  ```C
  double data[3][5] = {{1,3,4,5,10},{7,8,9,10,11},{2,12,6,15,14}};
  cout<<*(data+1)[1]<<endl;   // 2  = data[2][0]
  cout<<*((data+1)[1])<<endl; // 2  = data[2][0]
  cout<<(*(data+1))[1]<<endl; // 8  = data[1][1]
  ```

## 函式指標 (function pointer) ##

* 指向 function 的指標，每個 function 的啟始記憶體位置，即為 function 的名稱。
* C 語言中，不論是 variable、array、struct、或是 function(一段程式碼)，都有所屬的起始記憶體位置。
* 函式指標的參數資料型態與引入參數個數皆須匹配

* 範例

  ```C
  int doAdd(int a, int b) { return a + b; }
  int doMinus(int a, int b) { return a - b; }

  int main(void) {
    // 宣告 function pointer，注意所設定的參數數量與型態
    int (*my_func_ptr)(int, int);

    my_func_ptr = doAdd; // function pointer 指向 doAdd
    printf("fp 指向 doAdd => %d\n", (*my_func_ptr)(5, 3));    //結果：8

    my_func_ptr = doMinus; // function pointer 指向 doMinus
    printf("fp 指向 doMinus => %d\n", (*my_func_ptr)(5, 3));  //結果：2 

    return 0;
  }
  ```

* function pointer 結合 typedef

  ```C
  // 等於將 int (*)(int, int) 簡化成 MathMethod
  typedef int (*MathMethod)(int, int); 

  int Mul(int a, int b){return a*b;}
  int Divide(int a, int b){return a/b;}
  int Minus(int a, int b){return a-b;}
  int Add(int a, int b){return a+b;}

  int Calc(int x, int y, MathMethod Opt){
      return Opt(x, y);
  } 
  
  int main(){
      int a = 8, b = 7;
      printf("a x b = %d\n", Calc(a, b, Mul)); 
      printf("a / b = %d\n", Calc(a, b, Divide));
      printf("a + b = %d\n", Calc(a, b, Add));
      printf("a - b = %d\n", Calc(a, b, Minus));
  }
  ```

* function pointer array 用法

  ```C
  #include <stdint.h>
  #include <stdbool.h>

  void func_run() { printf("start\r\n"); }
  void func_stop() { printf("stop\r\n"); }
  void func_exit() { printf("exit\r\n"); }

  static void (*command[])(void) = {func_run, func_stop, func_exit};

  int OnStateChange(uint8_t state){
      if (state >= 3)
      {
          printf("Wrong state!\n");
          return false;
      }

      command[state]();
      return 0;
  }
  ```

## Preprocessor ##

* 種類
  * #include : 引入標頭檔
  * #define : 定義符號常數、定義巨集 (marco ，大陸翻譯:宏定義) ，不可以加分號
  * 條件編譯
    * #if, #elif, #define
    * #ifdef, #ifndef, #if defined(), #if !defined()
    * #else
    * #endif
  * #undef : 刪除巨集定義
  * #error

* marco : 簡單的函數
  1. 有引數，無引數
  2. trade-off: 相較於函數，占用記憶體較多，但程式的執行流程不用移轉，因而程式執行的速度較快。

* 用預處理指令 #define 聲明一個常數，用以表示1年中有多少秒 (忽略閏年問題)

  ```C
  #define SECONDS_PER_YEAR (60 * 60 * 24 * 365)UL
  ```

* 寫一個“標準”巨集 MIN ，這個巨集輸入兩個參數並返回較小的一個

  ```C
  #define MIN(A, B) ((A) <= (B) ? (A) : (B))
  ```

* 引入防護和條件編譯 (Include guard)

  ```C
  #ifdef MYHEADER
  #define MYHEADER
      // part A
  #else
      // part B
  #endif
  ```

## String ##

* C 語言中，字串使用""括住，在宣告時，則會自動在字串的尾端幫你補上'\0'。

  ```C
  char ch1 = 'a';
  printf("sizeof(ch1) is %llu.\n", sizeof(ch1)); // 1
  char ch2[] = "a";
  printf("sizeof(ch2) is %llu.\n", sizeof(ch2)); // 2
  ```

* C 語言中，在字元陣列 (char array) 的結尾加上字串結束字元 '\0' ，即為字串。

  ```C
  char str[] = {'s','t','r','i','n','g','\0'};
  printf("%s\n", str); // 使用 %s ，輸出 string
  ```

* string.h 的函式庫
  * strlen( s ) : 返回的型別也是 size_t ，不包括 '\0' 或 Null 。

    ```C
    #inlcude <string.h>

    char s1[] = "hello";
    printf("strlen(s1) is %llu\n", strlen(s1)); // 5
    char s2[] = {'h','e','l','l','o','\0'};
    printf("strlen(s2) is %llu\n", strlen(s2)); // 5
    char *s3 = "hello";
    printf("strlen(s3) is %llu\n", strlen(s3)); // 5
    ```

  * strcpy( s1, s2 ) :
  * strcmp( s1, s2 ) :

* 字串反轉 string reverse

  ```C
  void swap(char* a, char* b){
    if(a==b)return;
    *a^=*b;
    *b^=*a;
    *a^=*b;
  }
  void reverseString(char* s, int sSize){    
      int i=0,j=sSize-1;
      while(i<j){
          swap(s+i,s+j);
          i++;j--;
      }
      return;
  }
  ```

## C Standard Library ##

* C 標準函式庫，英文縮寫 libc

  * ANSI C 共包括15個標頭檔
    * assert.h、ctype.h、errno.h、float.h、limits.h、locale.h、math.h、setjmp.h、signal.h、stdarg.h、tddef.h、stdio.h、stdlib.h、string.h 和 time.h

  * Normative Addendum 1 (NA1) 批准了3個標頭檔增加到C標準函式庫中
    * iso646.h、wchar.h 和 wctype.h

  * C99 標準增加了6個標頭檔
    * complex.h、fenv.h、inttypes.h、stdbool.h、stdint.h 和 tgmath.h

  * C11標準中又新增了5個標頭檔
    * stdalign.h、stdatomic.h、stdnoreturn.h、threads.h 和 uchar.h

* 記憶體複製

    ```C
    void *memcpy( void *dest, const void *src, size_t count );
    ```

* 字串複製

  ```C
  const char *str1 = "abc\0def";
  char str2[16] = {0};
  char str3[16] = {0};

  strcpy(str2, str1);
  memcpy(str3, str1, sizeof(str3)); // 8
  printf("str2 = %s\n", str2);    // str2 = abc
  printf("str3 = %c\n", str3[5]); // str3 = e
  ```

## Algorithm ##

### 排序 (Sort) ###

* Quick Sort

  ```C
  void quickSort(int* nums, int head, int tail){
      if(head>=tail){return;}
      int comp=nums[head];
      int i=head,j=tail;
      while(i<j){
          while(nums[j]>=comp && i<j){j--;}
          nums[i]=nums[j];
          nums[j]=comp;
          while(nums[i]<=comp && i<j){i++;}
          nums[j]=nums[i];
          nums[i]=comp;
      }
      quickSort(nums,head,i-1);
      quickSort(nums,j+1,tail);
  }
  ```

* Merge Sort

### 搜尋 (Search) ###

* 線性搜尋 (Linear Search)

* 二元搜尋演算法 (Binary Search Algorithm)

  ```C
  int binsearch(int *arr, int searchNum, int arrSize)
  {
      int left = 0, right = n;
      while (left <= right)
      {
          int mid = (left + right) / 2;
          if (arr[mid] > k)
          { // 往左半部找
              right = mid - 1;
          }
          else if (arr[mid] < k)
          { // 往右半部找
              left = mid + 1;
          }
          else
              return mid;
      }
      return -1; // not found
  }
  ```

### Other ###

* 高斯演算法 (Gauss Algorithm)

  ```C
  // Time Complexity :O(n)
  int add(int N){
    int i, ans = 0;
    for (int i = 0; i < N; i++){
        ans += i;
    }
    printf("ans =%d", ans);
    return ans;
  }

  // Time Complexity :O(1)
  int gaussianAdd(int N){
    int ans = 0;
    ans = (1 + N) * N / 2;
    printf("ans of gaussianAdd() =%d\n", ans);
    return ans;
  }
  ```

* swap
  * 使用額外一個變數
  * 加減法運算
  * 位元運算

    ```C
    void swap(char* a, char* b){
        if(a==b)return;
        *a^=*b;
        *b^=*a;
        *a^=*b;
    }
    ```

  * 使用 define

* test
