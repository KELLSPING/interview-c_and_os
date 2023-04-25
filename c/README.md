# C Language #

```C
/* Hi, this is C Language page. */
```

## Table of contents ##

* [Noun Definition](#noun-definition)
* [Basic](#basic)
* [Bitwise](#bitwise)
* [Data Type](#data-type)
* [Data Structure](#data-structure)
* [Loops](#loops)
* [I/O](#io)
* [Keyword](#keyword)
* [Pointer](#pointer)
* [Preprocessor](#preprocessor)
* [String](#string)
* [C Standard Library](#c-standard-library)
* [Algorithm](#algorithm)

## Noun Definition ##

* 複雜度
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

* 傳值呼叫、傳址呼叫、參考呼叫
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

* 編譯與執行的過程
  * source code (.c, .h) -> Preprocessor (.c) -> Compiler (.s) -> Assembler (.o) -> Linker (.exe) -> Loader  -> CPU
    1. 原始碼 (source code): 副檔名為 .c，標頭檔副檔名為 .h。
    2. [Build] 預處理器 (Preprocessor): 將須引入的檔案或是 macro 展開。
    3. [Build] 編譯器 (Compiler): C 語言常見的編譯器有 GCC、Clang，在此 GCC 為廣義的編譯器，可以達到 build 的所有功能。狹義的編譯器，主要就是將 C 語言轉成組合語言，產出 .s, .asm 檔。
    4. [Build] 組譯器 (Assembler): 將低階語言所寫的程式翻譯成目的檔，為 .o 檔。
    5. [Build] 連結器 (Linker): 將多個目的檔或靜態函式庫 (Static library, .a, .lib) 合併成一個可執行檔 (.exe, .out) 或函式庫的工具。
    6. [Run] 載入器 (Loader): 是作業系統的一部份，用於把程式和動態函式庫 (Shared library, .so, .dll) 的指令載入到記憶體 (RAM) 中等待 CPU 執行，當載入完成之後，作業系統會將控制權交給載入的程式碼，讓它開始運作。
    7. [Run] CPU: 對載入的指令進行運算或儲存等操作。

* 偵錯 (Debug)
  * 語法錯誤 (Syntax error): 不符合語言的規定，將編譯後所指出的錯誤修正，再重新編譯即可。
  * 語意錯誤 (Semantic error): 程式本身的語法沒有問題，但邏輯上可能有瑕疵，造成非預期性的結果。
  * 記憶體區段錯誤 (Segmentation fault, 縮寫 segfault): 也稱存取權限衝突 (access violation)，是一種程式錯誤。

* 識別字及關鍵字
  * 識別字 (Identifier): 使用者自訂的變數與函數名稱。
  * 關鍵字 (Keyword): 編譯程式本身使用的識別字。

## Basic ##

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
    unsigned long num_a = 0x00001111;
    unsigned long num_b = 0x00000202;
    unsigned long num_c;

    num_c = num_a & (~num_b);
    printf("num_c=%x\n", num_c); // 1111

    num_c = num_c | num_b;
    printf("num_c=%x\n", num_c); // 1313
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

## Data Type ##

* x86_64, x64, AMD64 皆可以代表為 64-bits CPU。

* 基本資料型態 (Primitive Data Types)
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

* 複合資料型態 (Compound Data Types)
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

* 關鍵字 static 的作用是什麼?

  1. 在函數本體內(in Function Block)，一個被宣告為靜態的變數，在這一函數被呼叫過程中維持其值不變。
  2. 在一個Block(ie. {...} )內 (但在函數體外)，一個被宣告為靜態的變數可以被Block內所有的函數存取， 但不能被Block外的其它函數存取。它是一個本地的全局變量。
  3. 在Block內，一個被聲明為靜態的函數，只可被這一Block內的其它函數呼叫。也就是這個函數被限制在宣告它的Block的本地範圍內使用。

* 關鍵字 const 有什麼含意？

  * const 代表只可以讀取不能寫入，使用 const 有以下好處：
    1. 提升程式碼可讀性
    2. 使編譯器保護那些不希望被改變的參數
    3. 給優化器一些附加的資訊

    ```C
    const int a; // a 是一個常數型整數 (const int)
    int const a; // a 是一個 const int
    const int *a; // a 是一個 pointer，指向 const int 變數
    int const *a; // a 是一個 pointer，指向 const int 變數
    int * const a; // a 是一個常數型指標 (const pointer)，指向 int 變數
    int const * a const; // a 是一個 const pointer，指向 const int 變數
    ```

* 關鍵字 volatile 有什麼含意？

* 變數範圍和生命周期
  * 區域變數 (loacl variable): local 變數僅活在該函式內，存放位置在 stack 或 heap 記憶體中。
  * 靜態變數 (static variable)
    * 在編譯時，就已配置固定的記憶體空間，使靜態變數的值得以保存。
    * 再次呼叫該函數時，會將靜態變數存在記憶體空間中的值取出來用。
    * 作用
      1. 在函數本體內，一個被宣告為靜態的變數，在這一函數被呼叫過程中，其值不變。
      2. 在一個 block 中，一個被宣告為靜態的變數，可以被 block 內的所有函數存取，但不能被 block 外的其他函數存取。是一個本地的全局變量。
      3. 在一個 block 中，一個被宣告為靜態的函數，可以被 block 內的其他函數呼叫，但不能在 block 外被呼叫。
  * 全域變數 (global variable): 所有區段皆可使用此變數。

* 關鍵字 sizeof
  * 查詢常數、變數、指針或資料型態所占位元組
  * struct
    * 對齊原則：每一成員需對齊爲後一成員類型的倍數
    * 補齊原則：最終大小補齊爲成員類型最大值的倍數

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

* sizeof 運算子是一個一元運算子。運算之結果的型別為 size_t ，其實就是無號整數(long long unsigned int)，在 printf() 中，需使用 %llu 。

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

* 線性搜尋 (Linear Search)

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

* swap
  * 使用額外一個變數
  * 加減法運算
  * 位元運算
  * 使用 define
