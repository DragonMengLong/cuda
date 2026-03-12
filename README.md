# CUDA 入门示例项目

这是一个基于 **Visual Studio + CUDA 11.0** 的最小示例工程，用于演示：
- C/C++ 程序入口（CPU 侧）
- CUDA Kernel 的基本写法
- Host 与 Device 之间的数据拷贝与调用流程

## 项目结构

```text
cuda/
├── cuda.vcxproj           # Visual Studio 工程文件（含 CUDA 编译配置）
├── cuda.vcxproj.user      # 本机用户级工程配置
├── my_first_cuda.cu       # 当前程序主入口（main）
├── kernel.cu              # CUDA Kernel 与 addWithCuda 实现
└── x64/                   # VS 生成目录（Debug/Release 输出）
```

## 关键入口

### 1) 程序主入口（CPU）
- 文件：`my_first_cuda.cu`
- 函数：`int main(void)`
- 作用：当前默认可执行入口，输出一条 CPU 侧信息。

### 2) CUDA 计算入口（GPU）
- 文件：`kernel.cu`
- Kernel：`__global__ void addKernel(int *c, const int *a, const int *b)`
- Host 包装函数：`cudaError_t addWithCuda(...)`
- 作用：
  1. 选择 GPU (`cudaSetDevice`)
  2. 申请显存 (`cudaMalloc`)
  3. 拷贝数据到 GPU (`cudaMemcpy`)
  4. 启动 Kernel (`addKernel<<<...>>>`)
  5. 同步并拷回结果 (`cudaDeviceSynchronize` + `cudaMemcpy`)

> 注意：`kernel.cu` 中有一段 `main` 示例代码，目前是注释状态，不参与编译运行。

## 如何构建与运行

1. 使用 Visual Studio 打开工程 `cuda.vcxproj`。
2. 选择配置：`Debug|x64` 或 `Release|x64`。
3. 确保本机已安装对应 CUDA Toolkit（工程当前引用 `CUDA 11.0`）。
4. 直接构建并运行即可。

## 后续建议

- 将 `my_first_cuda.cu` 中的 `main` 改为调用 `addWithCuda`，形成完整“CPU 调用 GPU”闭环。
- 为 `addWithCuda` 增加更丰富的输入规模与错误日志。
- 如需跨平台构建，可补充 `CMakeLists.txt`。
