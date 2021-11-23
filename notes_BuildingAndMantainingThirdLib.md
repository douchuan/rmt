[Building and Maintaining a Third-Party Library Supply Chain for Productive and Secure SGX Enclave Development](https://arxiv.org/abs/2005.04367)

在Rust SGX生态中，第三方库面临的问题：

- 第三方库的选择标准
- 如何patching and updates
- 随着项目演进和开发便利性考虑，必然有更多的第三方库被采纳，如何更省人力的维护

Key points：

- Rust STD 与 Rust SGX STD关系and why   
- Rust SGX 已支持的常用库(159个)
- Rust SGX选择第三方库的原则
- disable multi-threading
- how patches and updates (Patch crawler, merger, Expert Review)
- Version, Rust SGX only offer the latest version
- Open sources depending on Rust SGX
