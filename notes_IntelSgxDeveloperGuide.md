Intel® Software Guard Extensions (Intel® SGX) Developer Guide

非常好的SGX材料，从程序员角度讲述如何写enclave程序（原理及安全问题），论述精准，取材得当，视野高。
是学习SGX必读材料。

前期需要了解：Intel EPID

This document provides examples of many programming constructs and principles based on a hypothetical generic runtime system. The elements of this runtime system include the following:

Untrusted Run-Time System (uRTS) – code that executes outside of the Intel SGX enclave environment and performs functions such as:

- Loading and managing an enclave.
- Making calls to an enclave and receiving calls from within an enclave.

Trusted Run-Time System(tRTS) – code that executes within an Intel SGX enclave environment and performs functions such as:

- Receiving calls into the enclave and making calls outside of an enclave.
-  Managing the enclave itself.
- Standard C/C++ libraries and run-time environment.

Edge Routines – functions that may run outside the enclave (untrusted edge routines) or inside the enclave (trusted edge routines) and serve to bind a call from the application with a function inside the enclave or a call from the enclave with a function in the application.

3rd Party Libraries – for the purpose of this document,this is any library that has been tailored to work inside the Intel SGX enclave environment.


Enclave Measurement: A single 256-bit hash that identifies the code and initial data to be placed inside the enclave, the expected order and position in which they are to be placed, and security properties of those pages. A change in any of these variables will result in 
a different measurement.When the enclave code/data pages are placed inside the EPC, the CPU calculates the enclave measurement and sotres this value in the MRENCLAVE register.

Local Attestation Example中，A和B，如何相互发现？communication path是一个PIPE，还是socket？

The application B retrieves the MRENCLAVE value from the enclave certificate for enclave B.
系统提供的机制吗？还是需要每个Enclave独自实现？

The SDK includes a tool for signing enclaves, called sgx_sign



# Data Center Attestation Service

DCAP: Data Center Attestation Service

替换IAS（Intel Attestation Service）

This directory includes a reference implementation of data center attestation
service using
[Intel SGX Data Center Attestation Primitives](https://software.intel.com/en-us/blogs/2019/05/21/intel-sgx-datacenter-attestation-primitives) (DCAP),
which allows third-parties to create their own attestation infrastructure for
the datacenter and cloud. Compared to Intel Attestation Service (IAS), DCAP
Attestation Service is for environment where internet services is not accessible
and entities who are unwilling to outsource trust decisions to third-parties
(like Intel's IAS).

By default, Intel Attestation Service (IAS) will be used for attestation in
Teaclave. To use DCAP instead of IAS, you have to first build Teaclave with DCAP
enabled (by appending `-DDCAP=ON` option to `cmake`) and deploy in
infrastructure with DCAP supported.

The Intel's [DCAP Installation Guide](https://download.01.org/intel-sgx/sgx-dcap/1.3.1/linux/docs/Intel_SGX_DCAP_Linux_SW_Installation_Guide.pdf)
contains instructions to install essential dependencies for developers. Also,
you need to prepare environment in your infrastructure before deploying a
DCAP-enabled application.



### [Web-Based Training](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sgx-web-based-training.html)

Before an application can use Intel SGX, four conditions have to be met:

- The CPU in the system must support the Intel SGX instructions.
- The system BIOS must support Intel SGX.
- Intel SGX must be enabled in the BIOS, and
- The Intel SGX Platform Software, or PSW, must be installed on that system.


Enclaves are considered trusted because they cannot be modified after they have been built. If an enclave is modified by a malicious user or software, it will be detected by the CPU and it won’t load.


The fundamental protection provided by Intel SGX is that an enclave’s secrets can only be accessed by the code that is inside the enclave. The only way to execute that code is through the interface functions that the enclave developer has created. The CPU enforces this restriction, and even privileged users can’t circumvent it

Each enclave defines one or more Enclave Calls, or ECALLS, which are the entry points into the enclave from the untrusted application. An enclave may also have Outside Calls, or OCALLS, which allow enclave functions to call out to the untrusted application and then return to the enclave. These ECALLs and OCALLs make up an enclave’s interface, which is defined in the encave's EDL file. EDL stands for Enclave Definition Language. An EDL file loosely resembles a C-style header file with function prototypes, and in fact shares much of that syntax.

EDL: Enclave Definition Language

```
enclave {

	trusted {
		public void store_secret([in, string] char *msg);
		public int print_hash([out] sgx_status_t *error); 
	};
	untrusted {
		void o_print_hash([in] unsigned char hash[32]); 
	};

}; 
```

Data Sealing

Enclaves are essentially stateless: they are destroyed when the system goes to sleep, when the application exits, and of course when the application explicitly destroys them. When an enclave is destroyed, all of its contents are lost.

To preserve the information that’s stored in an enclave, you must explicitly send it outside the enclave to untrusted memory. Intel SGX provides a capability called data sealing which encrypts enclave data in the enclave using an encryption key that is derived from the CPU. This encrypted data block, also called the sealed data, can only be decrypted, or unsealed, on the same system (and, typically, in the exact same enclave) where it was created. The encryption itself provides assurances of confidentiality, integrity, and authenticity on the data.

There is an important caveat with data sealing that can have significant security implications: enclaves do not authenticate the untrusted application. You must not assume that only your application will load your enclave, or that your ECALLs will be invoked in a specific order. Anyone can load your enclave and execute its ECALLs in any order they choose. Your enclave’s API must not allow the sealing and unsealing capability to leak secrets, or grant unauthorized access to them.

Key points:

- Enclaves are essentially stateless: they are destroyed when the system goes to sleep, when the application exits, and of course when the application explicitly destroys them
- the sealed data, can only be decrypted, or unsealed, on the same system 
- Anyone can load your enclave and execute its ECALLs in any order they choose


### [Strengthen Enclave Trust with Attestation](https://www.intel.com/content/www/us/en/developer/tools/software-guard-extensions/attestation-services.html)






