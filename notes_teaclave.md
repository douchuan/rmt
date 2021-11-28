
teaclave编译用xargo，不是通过cargo（cargo过于上层,无法控制编译选项）

### TODO

- 2-3 本SGX的专业书籍，覆盖入门->中级->高级（底层实现原理，类似Intel SGX Explained这样的论文）
- 大量文章，像Rust社区所做的，针对各种库写的各种book
- API文档
- IDE? IntellJ plugin for SGX (创建项目，编译，运行)
- support dev in Windows & Mac & Linux (各种发行版)

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

### protobuf

### prost-protobuf

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


### sgx_tstd, println!

- sgx_tstd/src/macros.rs: println!
- sgx_tstd/src/io/stdio.rs: _print, pub fn stdout()
- sgx_tstd/src/sys/stdio.rs: Stdout
- sgx_tstd/src/sys/fd.rs: FileDesc
- sgx_libc/src/linux/x86_64/ocall.rs, write (与ocall的分界线)

todo: more about supporting concepts

- sgx_tstd/src/io/lazy.rs, LazyStatic, SgxThreadMutex, SgxReentrantMutex

### sgx_tstd, panic!

### sgx_trts

Trusted Runtime System

The Intel(R) SGX trusted runtime system (tRTS) is a key component of the Intel(R) 
Software Guard Extensions SDK.

It provides the enclave entry point logic as well as other functions to be used 
by enclave developers.

ascii.rs, cpu_feature.rs, enclave.rs, macros.rs, memeq.rs, trts.rs, 
c_str.rs, cpuid.rs, memchr.rs, oom.rs, veh.rs

- cpuid.rs, As the CPUID instruction is executed by an OCALL, the results should not be trusted. Code should verify the results and perform a threat evaluation to determine the impact on trusted code if the results were spoofed.
- enclave.rs: global_data_t, thread_data_t, SgxGlobalData, SgxThreadData, SgxThreadPolicy 
- memeq.rs: Comparing buffer contents in constant time
- trts.rs: rsgx_read_rand, rsgx_read_rand function is used to generate a random number inside the enclave
- trts.rs: rsgx_data_is_within_enclave, ...
- veh.rs: register/unregister exception handler

### untrusted: fs, path, time


### [ffi](https://doc.rust-lang.org/nomicon/ffi.html)

todo: ffi

### [rustc](https://doc.rust-lang.org/rustc/what-is-rustc.html)

todo: rustc