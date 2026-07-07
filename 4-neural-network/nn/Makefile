# ==============================================================================
# nn/Makefile - 函式庫編譯設定
# ==============================================================================
CC = gcc
CFLAGS = -Wall -Wextra -O2 -fPIC -I./include
AR = ar
ARFLAGS = rcs

# 路徑定義
SRC_DIR = src/nn
INC_DIR = include/nn
LIB_DIR = lib
OBJ_DIR = obj

# 原始碼與目標檔
SRCS = $(SRC_DIR)/tensor.c $(SRC_DIR)/operator.c
OBJS = $(OBJ_DIR)/tensor.o $(OBJ_DIR)/operator.o

# 安裝路徑 (預設為系統全域路徑)
PREFIX = /usr/local
INSTALL_INC = $(PREFIX)/include/nn
INSTALL_LIB = $(PREFIX)/lib

STATIC_LIB = $(LIB_DIR)/libnn.a
DYNAMIC_LIB = $(LIB_DIR)/libnn.so

.PHONY: all static dynamic clean install help

# 預設同時建置靜態與動態函式庫
all: static dynamic

# 建立靜態函式庫 (.a)
static: $(STATIC_LIB)

$(STATIC_LIB): $(OBJS)
	@mkdir -p $(LIB_DIR)
	$(AR) $(ARFLAGS) $@ $^
	

# 建立動態函式庫 (.so)
dynamic: $(DYNAMIC_LIB)

$(DYNAMIC_LIB): $(OBJS)
	@mkdir -p $(LIB_DIR)
	$(CC) -shared -o $@ $^ -lm

# 編譯個別的物件檔案 (.o)
$(OBJ_DIR)/%.o: $(SRC_DIR)/%.c
	@mkdir -p $(OBJ_DIR)
	$(CC) $(CFLAGS) -c $< -o $@

# 系統安裝 (System Install)
install: all
	@echo "Installing headers to $(INSTALL_INC)..."
	@mkdir -p $(INSTALL_INC)
	cp -f include/nn/*.h $(INSTALL_INC)/
	@echo "Installing libraries to $(INSTALL_LIB)..."
	@mkdir -p $(INSTALL_LIB)
	cp -f $(DYNAMIC_LIB) $(INSTALL_LIB)/
	@echo "Installation completed. (Remember to run ldconfig if needed)"

# 清除編譯產物
clean:
	rm -rf $(OBJ_DIR) $(LIB_DIR)

# 說明訊息
help:
	@echo "========================================================"
	@echo " Library Makefile (nn/Makefile)"
	@echo "========================================================"
	@echo "  make all     : 建置靜態庫與動態庫"
	@echo "  make static  : 僅建置靜態庫 (lib/libnn.a)"
	@echo "  make dynamic : 僅建置動態庫 (lib/libnn.so)"
	@echo "  make install : 將標頭檔與動態庫安裝至系統全域路徑"
	@echo "  make clean   : 清除所有物件檔案與函式庫"
	@echo "========================================================"