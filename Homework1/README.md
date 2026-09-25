# Homework1 - Thư viện strutils

## Các biến thường dùng trong Makefile

```makefile
# Thư mục chứa file header.
INC_DIR := ./inc

# Thư mục chứa mã nguồn C.
SRC_DIR := ./src

# Thư mục chứa file object.
OBJ_DIR := ./obj

# Thư mục riêng cho object của static library.
OBJ_STATIC := $(OBJ_DIR)/static

# Thư mục riêng cho object của shared library.
OBJ_SHARED := $(OBJ_DIR)/shared

# Thư mục chứa các file thực thi.
BIN_DIR := ./bin

# Tên chương trình dùng static library.
PROGRAM_STATIC := main_static

# Tên chương trình dùng shared library.
PROGRAM_SHARED := main_shared

# Tên thư viện, không viết thêm tiền tố lib.
# Makefile sẽ tự tạo libstrutils.a và libstrutils.so.
LIB_NAME := strutils

# Thư mục chứa static library.
STATIC_LIB := ./lib/static

# Thư mục chứa shared library.
SHARED_LIB := ./lib/shared
```

## Target static

```makefile
# Target static phụ thuộc vào hai file mã nguồn.
static: $(SRC_DIR)/bstrutils.c $(SRC_DIR)/main.c

	# Biên dịch bstrutils.c thành file object.
	# -c: chỉ biên dịch, chưa liên kết.
	# -o: chỉ định tên file đầu ra.
	# -I: chỉ định thư mục chứa file header.
	gcc -c $(SRC_DIR)/bstrutils.c \
		-o $(OBJ_STATIC)/bstrutils.o \
		-I$(INC_DIR)

	# Tạo static library từ file object.
	# ar: công cụ tạo thư viện tĩnh.
	# rcs: thêm object và tạo thư viện nếu chưa tồn tại.
	ar rcs $(STATIC_LIB)/lib$(LIB_NAME).a \
		$(OBJ_STATIC)/bstrutils.o

	# Biên dịch main.c và liên kết với static library.
	# -L: chỉ định thư mục tìm thư viện.
	# -l: chọn thư viện cần liên kết.
	# -o: chỉ định tên chương trình đầu ra.
	gcc $(SRC_DIR)/main.c \
		-L$(STATIC_LIB) \
		-l$(LIB_NAME) \
		-o $(BIN_DIR)/$(PROGRAM_STATIC) \
		-I$(INC_DIR)
```

Nếu `LIB_NAME := strutils` thì:

```makefile
-l$(LIB_NAME)
```

sẽ tương đương với:

```bash
-lstrutils
```

Trình liên kết sẽ tự tìm file:

```text
libstrutils.a
```

## Target shared

```makefile
# Target shared phụ thuộc vào hai file mã nguồn.
shared: $(SRC_DIR)/bstrutils.c $(SRC_DIR)/main.c

	# Biên dịch bstrutils.c thành object cho shared library.
	# -fPIC: tạo mã máy có thể đặt ở vị trí bất kỳ trong bộ nhớ.
	gcc -c -fPIC $(SRC_DIR)/bstrutils.c \
		-o $(OBJ_SHARED)/bstrutils.o \
		-I$(INC_DIR)

	# Tạo shared library từ file object.
	# -shared: yêu cầu gcc tạo thư viện động .so.
	gcc -shared $(OBJ_SHARED)/bstrutils.o \
		-o $(SHARED_LIB)/lib$(LIB_NAME).so

	# Biên dịch main.c và liên kết với shared library.
	# -L: chỉ định thư mục tìm shared library.
	# -l: chọn thư viện strutils.
	# -o: chỉ định tên chương trình đầu ra.
	gcc $(SRC_DIR)/main.c \
		-L$(SHARED_LIB) \
		-l$(LIB_NAME) \
		-o $(BIN_DIR)/$(PROGRAM_SHARED) \
		-I$(INC_DIR)
```

## Chạy chương trình

Chạy chương trình dùng static library:

```bash
./bin/main_static
```

Chạy chương trình dùng shared library:

```bash
LD_LIBRARY_PATH=./lib/shared ./bin/main_shared
```

`LD_LIBRARY_PATH` cho Linux biết nơi cần tìm file `libstrutils.so` khi chạy chương trình.

## Ý nghĩa các tùy chọn

- `-Iinc`: tìm file header trong thư mục `inc`.
- `-Llib/static`: tìm thư viện trong thư mục `lib/static`.
- `-Llib/shared`: tìm thư viện trong thư mục `lib/shared`.
- `-lstrutils`: liên kết với `libstrutils.a` hoặc `libstrutils.so`.
- `-fPIC`: tạo object phù hợp cho shared library.
- `-shared`: tạo shared library `.so`.
- `ar rcs`: tạo static library `.a`.
