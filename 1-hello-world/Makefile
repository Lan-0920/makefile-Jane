# 定義編譯器與編譯選項
CC = gcc
CFLAGS = -Wall

# 宣告虛擬目標 (Phony Targets)
# 避免當目錄下有同名檔案（例如剛好有名為 clean 的檔案）時導致指令失效
.PHONY: all run clean help

# 1. 預設目標 (make 或 make all)：將 hello.c 編譯出 hello.o 與 hello.out
# 這裡定義了 dependency：all 依賴於 hello.out，而 hello.out 依賴於 hello.o
all: hello.out

# 將目的檔 (.o) 連結成可執行檔 (.out)
hello.out: hello.o
	$(CC) $(CFLAGS) hello.o -o hello.out

# 將原始碼 (.c) 編譯成目的檔 (.o)，但不進行連結 (-c)
hello.o: hello.c
	$(CC) $(CFLAGS) -c hello.c -o hello.o

# 2. 執行目標 (make run)：執行編譯好的可執行檔
# 加上 all 作為 dependency，確保執行前一定是最新的編譯結果
run: all
	@echo "--- Running hello.out ---"
	./hello.out

# 3. 清除目標 (make clean)：刪除產生的目的檔與可執行檔
# -f 參數代表 force，即使檔案不存在（已被刪除）也不會報錯或中斷
clean:
	rm -f hello.o hello.out

# 4. 說明目標 (make help)：印出此 Makefile 支援的功能
help:
	@echo "========================================================"
	@echo " Makefile for Exercise 1: Hello World"
	@echo "========================================================"
	@echo " 支援的指令選項："
	@echo "  make 或 make all : 編譯產生 hello.o 與可執行檔 hello.out"
	@echo "  make run         : 自動編譯並執行 hello.out"
	@echo "  make clean       : 清除產生的 hello.o 與 hello.out "
	@echo "  make help        : 顯示此說明訊息"
	@echo "========================================================"