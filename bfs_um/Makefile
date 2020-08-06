include ../../common/make.config

CUDA_DIR=/usr/tce/packages/cuda/cuda-10.1.243
CALIPER_DIR=${HOME}/local/caliper/lassen-dev-gcc8

CC := $(CUDA_DIR)/bin/nvcc
CFLAGS := -gencode arch=compute_70,code=sm_70 -std=c++11
INCLUDE := -I$(CUDA_DIR)/include
INCLUDE += -I$(CALIPER_DIR)/include

LFLAGS = -L$(CALIPER_DIR)/lib64 -lcaliper-serial -lcuda

SRC = bfs.cu

EXE = bfs

release: $(SRC)
	$(CC) $(CFLAGS) $(SRC) -o $(EXE) $(INCLUDE) -L$(CUDA_LIB_DIR) $(LFLAGS)

enum: $(SRC)
	$(CC) -deviceemu $(SRC) -o $(EXE) $(INCLUDE) -L$(CUDA_LIB_DIR) $(LFLAGS)

debug: $(SRC)
	$(CC) $(CFLAGS) -g $(SRC) -o $(EXE) $(INCLUDE) -L$(CUDA_LIB_DIR) $(LFLAGS)

debugenum: $(SRC)
	$(CC) -g -deviceemu $(SRC) -o $(EXE) $(INCLUDE) -L$(CUDA_LIB_DIR) $(LFLAGS)

clean: $(SRC)
	rm -f $(EXE) $(EXE).linkinfo result.txt *~
