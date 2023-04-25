# OS #

* Code block in markdown

  ```C
  // Hi, OS
  ```

* Linux 開機流程
  * 步驟
    * BIOS -> MBR -> GRUB -> Kernel -> Init -> Runlevel Scripts
  * 名詞
    * 基本輸入輸出系統 (Basic Input/Output System, BIOS)
    * 主開機紀錄、主引導磁區 (Master Boot Record, MBR)
    * 啟動載入程式 (GNU GRUB, GRUB)
    * 核心、內核 (Kernel)

## Table of contents ##

## 作業系統 (Operating System, OS) ##

* 說明
  * 確保 Process 可以正確執行，不會讓 Process 跟 Process 之間互相干擾，並透過 kernel mode 跟 user mode 保護硬體，並提供 high level 的 system call 讓使用者不能直接操作硬體，簡化操作，也更加有效率等。

## 基本輸入輸出系統 (Basic Input/Output System, BIOS) ##

* 說明
  * 是一段由 CPU 執行的 code，讓 CPU 可以知道怎麼初始化硬體、找到可以開機的 storage 位置、把 bootloader 載入 RAM，接著跳到 bootloader 的入口點開始執行。

## 啟動載入程式 (Bootstrap Loader, bootloader) ##

* 說明
  * 讓 CPU 知道怎麼將 OS kernel 載入 RAM，並且執行 OS kernel 的初始化

* 在 Linux 中，是使用 GNU GRUB，是一個來自 GNU 專案的啟動載入程式。GNU GRUB 的前身為使用於類 Unix 系統中的 Grand Unified Bootloader。

## 中斷 (Interrupts) ##

* 種類
  * External Interrupt (外部中斷)
    * CPU 外的週邊元件所引起的。 (I/O Complete Interrupt, I/O Device error)
  * Internal Interrupt (內部中斷)
    * 不合法的用法所引起的。 (Debug、Divide-by-zero、overflow)
  * Software Interrupt (軟體中斷)
    * 使用者程式在執行時，若需要OS 提供服務時，會藉由 System Call 來呼叫 OS 執行對應的 service routine，完成服務請求後，再將結果傳回給使用者程式。

* ISR (Interrupt Service Routine)
  * ISR 簡單來說就是中斷會跳去執行的函式,而他跟 task 或 process 不同的地方是，做 context switch 的時候 ISR 只會 PUSH 部份暫存器,而 task 或 process 會 push 所有的暫存器

* 處理流程
  1. 暫停目前 process 之執行
  2. 保存此 process 當時執行狀況
  3. OS 會根據 Interrupt ID 查尋 Interrupt vector
  4. 取得 ISR 的起始位址
  5. ISR 執行
  6. ISR 執行完成，回到原先中斷前的執行

## Process ##

## Thread ##

* 單執行緒 (single thread) 和多執行緒 (multi thread)

## Deadlock ##

## Race Condition ##

* 競爭條件（race condition）又稱為競爭危害（race hazard）
* 若是某個記憶體內的資料，會同時被兩個不同的 thread 進行存取，可以先檢查這兩個 thread 寫入同份資料時是否存在 "happens-before relation"，若不存在此關係，便存在 race condition。

## Pipeline ##

* 什麼是 pipelin ?
  * 是為了讓計算機和其它數位電子裝置能夠加速指令的通過速度 (單位時間內被執行的指令數量) 而設計的技術。

## Socket ##

* 由一個 IP 位址和一個埠號碼 (port number) 所組成。

## Mutex ##

## Semophore ##

## Paging ##