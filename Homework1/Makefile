
CUR_DIR := .

INC_DIR := $(CUR_DIR)/inc
SRC_DIR := $(CUR_DIR)/src
OBJ_DIR := $(CUR_DIR)/obj
BIN_DIR := $(CUR_DIR)/bin

PROGRAM_STATIC := main_static
PROGRAM_SHARED := main_shared
LIB_NAME := libstrutils

STATIC_LIB := ./lib/static
SHARED_LIB := ./lib/shared


static: $(SRC_DIR)/bstrutils.c $(SRC_DIR)/main.c
	gcc -c $(SRC_DIR)/bstrutils.c -o $(OBJ_DIR)/bstrutils.o -I $(INC_DIR)
	gcc -c $(SRC_DIR)/main.c -o $(OBJ_DIR)/main.o -I $(INC_DIR)
	ar rcs $(STATIC_LIB)/$(LIB_NAME).a $(OBJ_DIR)/bstrutils.o
	gcc $(OBJ_DIR)/main.o -L $(STATIC_LIB) -l strutils -o $(BIN_DIR)/$(PROGRAM_STATIC)


shared: $(SRC_DIR)/bstrutils.c $(SRC_DIR)/main.c
	gcc -c $(SRC_DIR)/bstrutils.c -o $(OBJ_DIR)/bstrutils.o -I $(INC_DIR) -fPIC
	gcc -c $(SRC_DIR)/main.c -o $(OBJ_DIR)/main.o -I $(INC_DIR) -fPIC
	gcc -shared -fPIC $(OBJ_DIR)/bstrutils.o -o $(SHARED_LIB)/$(LIB_NAME).so
	gcc $(OBJ_DIR)/main.o -L $(SHARED_LIB) -l strutils -o $(BIN_DIR)/$(PROGRAM_SHARED)	


all: static shared

clean: 
	rm -rf $(OBJ_DIR)/*.o
	rm -rf $(STATIC_LIB)/*.a
	rm -rf $(SHARED_LIB)/*.so
	rm -rf $(BIN_DIR)/*