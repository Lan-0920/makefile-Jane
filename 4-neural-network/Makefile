# ==============================================================================
# Top-level Makefile - 主應用程式編譯設定
# ==============================================================================
CC = gcc
EXEC = cnn_demo.out
SRC = main.c

.PHONY: all debug run clean help nn_lib

# ==============================================================================
# 本地開發 (Local Build) 與 系統安裝 (System Install) 條件切換機制
# ==============================================================================
ifeq ($(USE_SYSTEM_LIB), 1)
    # 情境 2: Finalized Testing (使用系統全域路徑庫)
    # 不需要呼叫子目錄 Makefile，直接連結全域路徑的 -lnn
    CFLAGS_BASE = -Wall -Wextra
	LDFLAGS = -lnn -lm
    BUILD_DEP = 
else
    # 情境 1: Development Testing (預設：本地沙盒開發環境)
    # 需要先建置子目錄，並帶上本地 Include 路徑與 rpath 機制
    CFLAGS_BASE = -Wall -Wextra -I./nn/include
	LDFLAGS = -L./nn/lib -lnn -lm -Wl,-rpath,$(PWD)/nn/lib
    BUILD_DEP = nn_lib
endif

# Target-specific variables
all: CFLAGS = $(CFLAGS_BASE) -O2
all: $(BUILD_DEP) $(EXEC)

debug: CFLAGS = $(CFLAGS_BASE) -g -O0
debug: $(BUILD_DEP) $(EXEC)

# 連結主程式
$(EXEC): $(SRC)
	$(CC) $(CFLAGS) $(SRC) $(LDFLAGS) -o $@

# 呼叫子目錄的 Makefile 進行本地函式庫建置
nn_lib:
	$(MAKE) -C nn

# 執行可執行檔
run: all
	@echo "--- Running $(EXEC) ---"
	./$(EXEC)

# 清除所有頂層與子目錄的產出檔案
clean:
	rm -f $(EXEC)
	$(MAKE) -C nn clean

# 說明訊息
help:
	@echo "========================================================"
	@echo " Top-level Makefile"
	@echo "========================================================"
	@echo "  make 或 make all      : 本地沙盒模式建置 (開啟 -O2 最佳化)"
	@echo "  make debug            : 本地沙盒模式建置 (開啟 -g -O0 除錯)"
	@echo "  make run              : 自動編譯並執行神經網路主應用程式"
	@echo "  make clean            : 同步清除主程式與子目錄所有編譯產物"
	@echo "  使用系統全域函式庫建置範例:"
	@echo "    1. cd nn && sudo make install"
	@echo "    2. cd .. && make USE_SYSTEM_LIB=1"
	@echo "========================================================"