Intel® Software Guard Extensions (Intel® SGX) Developer Guide


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


