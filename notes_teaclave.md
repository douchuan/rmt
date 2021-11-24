
teaclave编译用xargo，不是通过cargo（cargo过于上层,无法控制编译选项）

### TODO

- 2-3 本SGX的专业书籍，覆盖入门->中级->高级（底层实现原理，类似Intel SGX Explained这样的论文）
- 大量文章，像Rust社区所做的，针对各种库写的各种book
- API文档
- 顺滑的开发环境，IntellJ plugin for SGX (创建项目，编译，运行)
- support dev in Windows & Mac & Linux (各种发行版)
- 完善的模拟器支持
- 各种宣传，举办各种比赛

### cmake

项目用cmake组织，example用makefile组织

[cmake tutorial](https://cmake.org/cmake/help/latest/guide/tutorial/index.html)

[learning cmake](https://github.com/Akagi201/learning-cmake/)

### edl



### teaclave-sgx-sdk

sgx_ucrypto, sgx_tcrypto 区别

u: untrust
t: trust

### teaclave-sgx-sdk, localattestation

### teaclave-sgx-sdk, remoteattestation