Towards Memory Safe Enclave Programming with Rust-SGX



为什么用Rust，而不用Java，python，javascript（基于VM，有GC，可攻击面太大，效率问题, Runtime占用大）......

Rust SGX基于Intel SGX SDK, 及为什么基于SGX-SDK而没有完全用Rust开发。

- SGX-SDK code base很庞大(40w行, C/C++ 27w行, 汇编 2w行)
- SGX-SDK 加密算法效率原因用汇编实现（2w行），Intel native AES-NI 用硬件实现
- SGX-SDK 对 sealing and remote attestation 的完善支持
- Intel对SGX-SDK的支持

Rust SGX的架构 (SGX-SDK, Rust Binding, Trusted Runtime, supported libs)

RAII-style for SgxSha256

Rust 与SGX SDK间的绑定层设计原则

类型系统的证明（不懂，skip）

实现：

an SGX program typically has two components: trusted component running inside of the SGX enclave and the untrusted component that interacts with the enclaves. Accordingly, the implementation of Rust-SGX SDK also has two parts: a trusted part and an untrusted part.

Rust标准库 和 SGX SDK

The trusted part can be further broken down into two major components in Rust-SGX SDK. The first component is the Rust standard library ported into the SGX execution environment. The second component, which depends on the Rust standard library, is the Rust wrappers for Intel SGX SDK APIs. They can be further broken down into several smaller components, including the SGX runtime and platform service, in-SGX cryptography, trusted key exchange, and protected file system, etc.

Rust 标准库需要改造，才能用于SGX：threading, synchronization, and exception handling.

thread: current Rust-SGX does not allow the use of thread local storage when the enclave is
configured with the unbound TCS policy.

mutex: 重新实现标准库的sys::Mutex

Exception handling: 重新实现 panic


Static Data Init

On most Unix-like platforms, static data in a dynamic library is initialized when the library is loaded by dlopen. For SGX, this procedure is different. Static data in an enclave is initialized when the first ECALL happens. The trusted runtime service of SGX has implemented a mechanism to initialize C/C++ static objects defined in an enclave, which are placed in a special ELF section called .init_array.

Rust的初始化方式是懒加载的，同时需要注意多线程问题，这部分需要改造：

Rust has its own static data initialization process, whose im- plementation is platform specific. We have to port this process into SGX, using the low-level static initialization functionalities provided by the trusted SGX runtime service. In particular, Rust adopts a lazy initialization scheme, meaning static data will not be fully initialized until the first time it is accessed. Therefore, each static variable has to be associated with an initialization state. Since Rust-SGX supports concurrency, we need to make sure this lazy initialization is thread safe, using the mutex primitives we ported into SGX.

[Intel SGX SDK 与 Rust SGX性能对比](https://github.com/mesalock-linux/rust-sgx-benchmark)

TLS with SGX remote attestation

与另一个Rust版本的SGX实现Fortanix Rust EDP，性能对比，完胜！原因：
The reason that Rust-SGX has better performance than that of Fortanix is due to the design of these two systems. First, Fortanix Rust EDP SDK depends on Rust’s libstd, which does not use any optimization features for SGX. Secondly, Fortanix Rust EDP SDK replaces ECALL/OCALL designed by Intel with a usercall.

High-performance Scientific Computation， Rust-SGX 好学，易用，用户友好

Machine Learning

todo：
用Rust-SGX(teaclave)开发的应用
用SGX-SDK开发的应用

Language Support for SGX:

- RPython, a typed Python dialect
- GOTEE, a research project porting the Go into SGX

Rust-SGX 与 Fortanix Rust EDP 对比：

Since the first release of Rust-SGX in 2017, our methodology of building secure enclaves using memory safe languages has inspired similar efforts. The most related one is the Fortanix Rust Enclave Development Platform (Fortanix Rust EDP) [5] released in 2019. Fortanix EDP invented its own application binary interface with the SGX hardware. Fortanix Rust EDP has a slightly different engineering focus compared with Rust-SGX. Specifically, Rust- SGX focused more on memory safety and compatibility with trusted execution environment, whereas Fortanix Rust EDP engaged more in memory safety and compatibility with Rust’s standard library.

Rust-SGX安全原因不支持Rust STD中的environment/timing/network

Some portions of the Rust’s standard library cannot be securely implemented in SGX, such as environment variable, timing, and networking

