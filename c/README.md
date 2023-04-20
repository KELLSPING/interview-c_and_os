# C Language #

* Code block in markdown

    ```C
    // Hi
    ```

## Table of contents ##

* [Noun Definition](#noun-definition)
* [Basic](#basic)
* [Bitwise](#bitwise)
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
    * Big O
    * 排序: 1 (常數) < log n < n < n log n < n^2 < 2^n < n!
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

    ```C
    int callByValue(int a, int b){...}
    ```

  * 傳址呼叫 (call by address, call by pointer)

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

* Error

  * 語法錯誤 (Syntax error): 不符合語言的規定，將編譯後所指出的錯誤修正，再重新編譯即可。
  * 語意錯誤 (Semantic error): 程式本身的語法沒有問題，但邏輯上可能有瑕疵，造成非預期性的結果。
  * 記憶體區段錯誤 (Segmentation fault, 縮寫 segfault): 也稱存取權限衝突 (access violation)，是一種程式錯誤。

## Basic ##

* 列印輸出

    ```C
    int i;
    i = (1, 2, 3);
    printf("i  = %d\n", i);
    ```

* 列印輸出

    ```C
    int first = 50, second = 60, third;
    third = first /* Will this comment work? */ + second;
    printf("%d /* And this? */ \n", third);
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

* 給定一個整型變量a，寫兩段程式碼，第一個設置a的bit 3，第二個清除a 的bit 3

    ```C
    #define BIT3 (0x1 << 3)
    static int a;
    void set_bit3(void) {  a = BIT3; }
    void clear_bit3(void) {  a &= ~BIT3; }
    ```

* 基本運算

    ```C
    unsigned long num_a = 0x00001111;
    unsigned long num_b = 0x00000202;
    unsigned long num_c;

    num_c = num_a & (~num_b);
    num_c = num_c | num_b;

    printf("%lx", num_c); // 00001313
    ```

* mask 方法做 bitwise 操作

    ```C
    a = a | 7    // 最右側 3 位設為 1，其餘不變。
    a = a & (~7) // 最右側 3 位設為 0，其餘不變。
    a = a ^ 7    // 最右側 3 位執行 NOT operator，其餘不變。
    ```

* 辨別 N 是不是 2 的冪次

    ```C
    int function(int N){
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

## Data Structure ##

* unsigned int

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

* array

* struct

  * 產生一種新的資料型態，每個成員變數都配置一段空間
  * 在 C 和 C++ 中，struct 用來包裝不同型態的資料。
  * 在 C++ 中，如果要針對內部成員做複雜處理的時候，都使用 class。
  * 位元欄位 (Bit fields)
  * 指定、取址、使用結構成員運算子'.'跟使用結構指標運算子'->'

* union

  * 產生一種新的資料型態，共用一段記憶體空間，所需的記憶體空間大小由最大的成員變數覺得
  * 指定、取址、使用結構成員運算子'.'跟使用結構指標運算子'->'

* enum

  * 一組由識別字所代表的整數常數
  * 除非特別指定，不然都是由0開始，接下來遞增1

* Linked List

* Stack

* Queue

* Heap

* Tree

  * 種類
    * 二元樹 (Binary Tree)
    * 二元搜尋樹 (Binary Search Tree, BST)
    * 平衡樹 (Self-balanced Binary Search Tree, height-balance binary search tree)

* Graph

* Hash table

## Loops ##

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

  * local 變數 : local 變數僅活在該函式內，存放位置在 stack 或 heap 記憶體中。
  * static 變數 : static 變數生命周期 (life time) 跟程式一樣長，而範圍 (scope) 則維持不變，即在宣告的函式之外仍無法存取 static 變數。
  * global 變數 : 所有區段皆可使用此變數。

## Pointer ##

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

## Preprocessor ##

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

## C Standard Library ##

* C 標準函式庫，英文縮寫 libc

  * ANSI C 共包括15個標頭檔
    * assert.h、ctype.h、errno.h、float.h、limits.h、locale.h、math.h、setjmp.h、signal.h、stdarg.h、tddef.h、stdio.h、stdlib.h、string.h 和 time.h

  * Normative Addendum 1 (NA1) 批准了3個標頭檔增加到C標準函式庫中
    * iso646.h、wchar.h 和 wctype.h

  * C99 標準增加了6個標頭檔
    * complex.h、fenv.h、inttypes.h、stdbool.h、stdint.h 和 tgmath.h

  * C11標準中又新增了5個標頭檔
    * stdalign.h、stdatomic.h、stdnoreturn.h、threads.h和uchar.h

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

* 
