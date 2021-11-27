
teaclave编译用xargo，不是通过cargo（cargo过于上层,无法控制编译选项）

### TODO

- 2-3 本SGX的专业书籍，覆盖入门->中级->高级（底层实现原理，类似Intel SGX Explained这样的论文）
- 大量文章，像Rust社区所做的，针对各种库写的各种book
- API文档
- 顺滑的开发环境，IntellJ plugin for SGX (创建项目，编译，运行)
- support dev in Windows & Mac & Linux (各种发行版)
- 完善的模拟器支持
- 各种宣传，举办各种比赛

把零散的文档集中化，降低学习成本和付出的找资料的时间：通过Intel SGX文档和linux-sgx代码，来学习SGX原理和开发，能否做到从teaclave官网就直接学会。

剖析local attestation, remote attestation demos，并形成文档。

### cmake

项目用cmake组织，example用makefile组织

[cmake tutorial](https://cmake.org/cmake/help/latest/guide/tutorial/index.html)

[learning cmake](https://github.com/Akagi201/learning-cmake/)

### edl



### teaclave-sgx-sdk

sgx_ucrypto, sgx_tcrypto 区别

u: untrust
t: trust

### hello-rust

```make
$(Signed_RustEnclave_Name): $(RustEnclave_Name)
	mkdir -p bin
	@$(SGX_ENCLAVE_SIGNER) sign -key enclave/Enclave_private.pem -enclave $(RustEnclave_Name) -out $@ -config enclave/Enclave.config.xml
	@echo "SIGN =>  $@"
```
等价于

```shell
sgx_sign sign -key enclave/Enclave_private.pem -enclave enclave/enclave.so -out bin/enclave.signed.so -config enclave/Enclave.config.xml
```

对so签名, 用到三个文件 Enclave_private.pem, enclave.so, Enclave.config.xml

todo: Enclave_private.pem 生成
todo: enclave/Enclave_t.c enclave/Enclave_t.h app/Enclave_u.c app/Enclave_u.h
todo: edl


### http-req

### teaclave-sgx-sdk, localattestation

### teaclave-sgx-sdk, remoteattestation


### [build.rs](https://doc.rust-lang.org/cargo/reference/build-scripts.html)

Build scripts communicate with Cargo by printing to stdout. Cargo will interpret each line that starts with cargo: as an instruction that will influence compilation of the package. All other lines are ignored.

The output of the script is hidden from the terminal during normal compilation. If you would like to see the output directly in your terminal, invoke Cargo as "very verbose" with the -vv flag. This only happens when the build script is run. If Cargo determines nothing has changed, it will not re-run the script, see change detection below for more.

All the lines printed to stdout by a build script are written to a file like target/debug/build/&lt;pkg&gt;/output (the precise location may depend on your configuration). The stderr output is also saved in that same directory.


```shell
root@8d2c41a7fa6e:~/sgx# cat samplecode/hello-rust/app/target/release/build/app-45ba18665ab83486/output
cargo:rustc-link-search=native=../lib
cargo:rustc-link-lib=static=Enclave_u
cargo:rustc-link-search=native=/opt/sgxsdk/lib64
cargo:rustc-link-lib=dylib=sgx_urts_sim
```

### [ffi](https://doc.rust-lang.org/nomicon/ffi.html)

todo: ffi

### [rustc](https://doc.rust-lang.org/rustc/what-is-rustc.html)

todo: rustc

