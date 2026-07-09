# Lab 2: Build Systems and Tools - C/C++ Compilation Automation


## 📂 專案架構與目錄說明

```text
makefile-jane/
├── README.md               # 本說明文件
├── 1-hello-world/          # Exercise 1: 基礎語法與 Dependency 連鎖
│   ├── Makefile
│   └── hello.c
├── 2-simple-math/          # Exercise 2: Wildcard 與 Pattern Rules 的運用
│   ├── Makefile
│   ├── area.c
│   ├── log.c
│   └── sqrt.c
├── 3-linked-list/          # Exercise 3: Target-specific 與隱式規則變數
│   ├── Makefile
│   ├── list.c
│   ├── list.h
│   └── main.c
└── 4-neural-network/       # Exercise 4: 複合型專案、動態/靜態庫與環境
    ├── Makefile            # 頂層主應用程式 Makefile
    ├── main.c              # 使用神經網路庫的主程式 (CNN 模擬)
    └── nn/                 # 神經網路核心函式庫
        ├── Makefile        # 函式庫獨立 Makefile
        ├── include/nn/     # 標頭檔 (.h)
        └── src/nn/         # 原始碼 (.c)
```

## Exercise 1: Hello World
功能：指令前加上 @ 隱藏冗餘輸出，以及 rm -f 的防錯清除機制。

執行指令：
```
cd 1-hello-world
make          # 編譯產生 hello.o 與可執行檔 hello.out
make run      # 自動編譯並執行程式
make clean    # 清除產生的目的檔與執行檔
make help     # 印出支援的指令與說明
```
## Exercise 2: Simple Math

所有數學運算（如 sqrt, log10）均正確於連結階段尾端加上 -lm 以供 Linker 正確解析。

執行指令：
```
cd 2-simple-math
make all      # 一鍵編譯目錄下所有的數學工具程式 (.out)
make area     # 編譯並執行計算圓面積程式
make sqrt     # 編譯並執行計算開平方根程式
make log      # 編譯並執行計算常用對數程式
make clean    # 清除所有產生的執行檔
```

## Exercise 3: Linked List
使用 Target-specific Variable Values 對不同的目標給予不同的編譯最佳化選項。

執行指令：
```
cd 3-linked-list
make all      # 動態追加 -O2 最佳化編譯並建置發布版程式
make debug    # 動態追加 -g -O0 包含除錯資訊並關閉最佳化，供 GDB 使用
make run      # 編譯並自動執行單向鏈結串列測試
make clean    # 清除所有物件檔案 (.o) 與可執行檔
```
## Exercise 4: Neural Network Library 

- 核心設計機制（雙模切換）：
    - Development Testing：預設模式。
    頂層 Makefile 會自動呼叫子目錄 nn/Makefile 於本地編譯出 libnn.so，並搭配 -Wl,-rpath 將執行期查找路徑燒進二進位檔。
    - 全域部署測試（Finalized Testing）：將動態庫部署至標準系統路徑下（如 /usr/local/lib/ 與 /usr/local/include/nn/），使其成為作業系統環境的一部分。
    
執行與驗證步驟：
```
cd 4-neural-network

# === 測試 1:  (預設) ===
make clean
make run        # 自動於本地建置、連結並在本地載入動態庫執行

# === 測試 2: 模擬發布至系統全域環境 ===
cd nn
sudo make install   # 將函式庫與標頭檔複製到系統全域路徑
sudo ldconfig       # 更新系統動態連結器快取

cd ..
make clean          # 清除本地編譯產物
make USE_SYSTEM_LIB=1      # 帶入變數，強迫 Linker 直接面向全域路徑連結
make run USE_SYSTEM_LIB=1  # 驗證在沒有本地庫檔案下，主程式依然可載入全域動態庫
```