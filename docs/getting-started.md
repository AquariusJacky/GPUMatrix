# Using MatLib Library in Your Project

## Prerequisites

- CUDA Toolkit (11.0 or higher)
- C++11 compatible compiler (g++)
- MatLib library installed
- NVIDIA GPU with compute capability 3.0 or higher

## Project Setup

1. **Create Project Structure**
```bash
mkdir myproject
cd myproject
touch main.cpp
```

2-1. **Compile**
```bash
g++ -std=c++11 your_program.cpp -I/path/to/matlib/include -L/path/to/matlib/lib -lmatlib -lcudart
```

2-2. **Configure Makefile** \
Or alternatively, create a Makefile

```bash
# Create and modify Makefile
vim Makefile
```

In Makefile
````makefile
# Compiler settings
CXX := g++
NVCC := nvcc

# Change these paths to match your MatLib installation
MATLIB_PATH := /path/to/installation
CUDA_PATH := /usr/local/cuda

# Compiler flags
CXX_FLAGS := -O3 -std=c++11 -Wall

# Include and library paths
INCLUDES := -I$(MATLIB_PATH)/include
LIB_PATHS := -L$(MATLIB_PATH)/build/release/lib -L$(CUDA_PATH)/lib64
LIBS := -lmatlib -lcudart

# Source files and target
SOURCES := $(wildcard *.cpp)
TARGET := myprogram
BUILD_DIR := build

all: $(BUILD_DIR)/$(TARGET)

$(BUILD_DIR):
	@mkdir -p $(BUILD_DIR)

$(BUILD_DIR)/$(TARGET): $(SOURCES) | $(BUILD_DIR)
	$(CXX) $(CXX_FLAGS) $(INCLUDES) $^ -o $@ $(LIB_PATHS) $(LIBS)

run: $(BUILD_DIR)/$(TARGET)
	@LD_LIBRARY_PATH=$(MATLIB_PATH)/build/release/lib:$(CUDA_PATH)/lib64 ./$(BUILD_DIR)/$(TARGET)

clean:
	rm -rf $(BUILD_DIR)

.PHONY: all run clean
````

## Building Your Project

```bash
# Build the program
make

# Run the program
make run

# Clean build files
make clean
```

## Project Structure
```
myproject/
├── Makefile
├── main.cpp
└── build/
    └── myprogram
```

## Troubleshooting

### Library Not Found
```bash
# Add to ~/.bashrc or run in terminal
export LD_LIBRARY_PATH=/path/to/matlib/lib:$LD_LIBRARY_PATH
```

### CUDA Installation Check
```bash
# Check CUDA compiler
nvcc --version

# Check GPU status
nvidia-smi
```

### Include Files Not Found
- Verify `MATLIB_PATH` in Makefile
- Check if headers exist in `$(MATLIB_PATH)/include/matlib/`

## Next Steps

See the MatLib library documentation for:
- API Reference
- Code Examples
- Performance Tips
- Advanced Features

## Reference
- Fastor is a high performance tensor (fixed multi-dimensional array) library for modern C++.
  https://github.com/romeric/Fastor

## To Do list
- Square Matrix power
- Reduction
- Scan
- histogram
- Sparse Matrix (SpMV, CSR, etc.)