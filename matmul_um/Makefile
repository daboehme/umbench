
CUDA_HOME=/usr/tce/packages/cuda/cuda-11.0.2
CALIPER_DIR=${HOME}/local/caliper/lassen-gcc8-cuda11

CC := $(CUDA_HOME)/bin/nvcc
CUDA_LIB_DIR := $(CUDA_HOME)/lib64
CFLAGS := -gencode arch=compute_60,code=sm_60 -gencode arch=compute_70,code=sm_70
INCLUDE := $(CALIPER_DIR)/include
SRC := main.cu util.cu
OBJ := $(SRC:.cu=.o)
EXE=umtest

LFLAGS = -L$(CALIPER_DIR)/lib64 -lcaliper-serial -lcuda

.PHONY: test run_oversub profile trace

%.o:%.cu
	$(CC) -c $(CFLAGS) $(CPPFLAGS) -o $@ $< -I$(INCLUDE)

$(EXE): clean $(OBJ) util.h
	$(CC) $(CFLAGS) $(CPPFLAGS) -o $(EXE) $(OBJ) -L$(CUDA_LIB_DIR) $(LFLAGS)

test: $(EXE)
	env MATRIX_DIM=1024 ./$(EXE)

run_oversub: $(EXE)
	env MATRIX_DIM=40960 ./$(EXE)

trace: $(EXE)
	env MATRIX_DIM=4096  nvprof --print-pci-topology --print-nvlink-topology --unified-memory-profiling per-process-device --print-gpu-trace ./$(EXE)

profile: $(EXE)
	env MATRIX_DIM=4096  nvprof --print-pci-topology --print-nvlink-topology --unified-memory-profiling per-process-device ./$(EXE)

clean: $(SRC)
	rm -f $(EXE) *.o *~
