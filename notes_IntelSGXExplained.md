
全景式的文档，包括计算机体系结构(booting, Cache, Address translation, privilege level switching)，
Security Background，TEE相关的产品(TPM, TT, SGX...)，SGX programming model...




使用场景：

For example, a cloud service that performs image pro- cessing on confidential medical images could be imple- mented by having users upload encrypted images. The users would send the encryption keys to software running inside an enclave. The enclave would contain the code for decrypting images, the image processing algorithm, and the code for encrypting the results. The code that receives the uploaded encrypted images and stores them would be left outside the enclave.

Hypervisor 作用:

The Intel architecture was designed to support running multiple application software instances, called processes. An operating system (§ 2.3), allocates the computer’s re- sources to the running processes. Server computers, espe- cially in cloud environments, may run multiple operating system instances at the same time. This is accomplished by having a hypervisor (§ 2.3) partition the computer’s re- sources between the operating system instances running on the computer.

virtualization 让系统软件的复杂度实现可控

System software uses virtualization techniques to isolate each piece of software that it manages (process or operating system) from the rest of the software running on the computer. This isolation is a key tool for keeping software complexity at manageable levels, as it allows application and OS developers to focus on their software, and ignore the interactions with other software that may run on the computer.

address translation & software privilege levels 对virtualization的意义

A key component of virtualization is address translation (§ 2.5), which is used to give software the impression that it owns all the memory on the computer. Address translation provides isolation that prevents a piece of buggy or malicious software from directly damaging other software, by modifying its memory contents.

The other key component of virtualization is the software privilege levels (§ 2.3) enforced by the CPU. Hardware privilege separation ensures that a piece of buggy or malicious software cannot damage other software indirectly, by interfering with the system software managing it.

MSR
The Model-Specific Register (MSR) space consists of 232 MSRs, which are used to configure the CPU’s operation. The MSR space was initially intended for the use of CPU model-specific firmware, but some MSRs have been promoted to architectural MSR status, making their semantics a part of the Intel architecture. For example, architectural MSR 0x10 holds a high-resolution monotonically increasing timestamp counter.

Address Translation and Virtualization(hypervisor, EPT)

When a hypervisor is active, the page tables set up by an operating system map between virtual addresses and guest-physical addresses in a guest-physical address space. The hypervisor multiplexes the computer’s DRAM between the operating systems’ guest-physical address spaces via the second layer of address translations, which uses extended page tables (EPT) to map guest-physical addresses to physical addresses.

The EPT uses the same data structure as the page tables, so the process of translating guest-physical ad- dresses to physical addresses follows the same steps as IA-32e address translation. The main difference is that the physical address of the data structure’s root node is stored in the extended page table pointer (EPTP) field in the Virtual Machine Control Structure (VMCS) for the guest OS. Figure 15 illustrates the address translation process in the presence of hardware virtualization.


Handle Exception
When a hardware exception occurs, the execution state may be corrupted, and the current stack cannot be relied on. Therefore, the CPU first uses the handler’s IDT entry to set up a known good stack. SS is loaded with a null descriptor, and RSP is set to the IST value to which the IDT entry points. After switching to a reliable stack, the CPU pushes the snapshot in Table 4 on the stack, then loads the IDT entry’s values into the CS and RIP registers, which trigger the execution of the exception handler.

After the exception handler completes, it uses the IRET (interrupt return) instruction to load the registers from the on-stack snapshot and switch back to ring 3.

The Intel architecture gives the fault handler complete control over the execution context of the software that in- curred the fault. This privilege is necessary for handlers (e.g., #GP) that must perform context switches (§ 2.6) as a consequence of terminating a thread that encoun- tered a bug. It follows that all fault handlers must be trusted to not leak or tamper with the information in an application’s execution context.


The Motherboard

The components we care about are connected by the following buses: the Quick-Path Interconnect (QPI [91]), a network of point-to-point links that connect processors, the double data rate (DDR) bus that connects a CPU to DRAM, the Direct Media Interface (DMI) bus that connects a CPU to the PCH, the Peripheral Component Interconnect Express (PCIe) bus that connects a CPU to peripherals such as a Network Interface Card (NIC), and the Serial Programming Interface (SPI) used by the PCH to communicate with the flash memory.

The PCIe bus is an extended, point-to-point version of the PCI standard, which provides a method for any peripheral connected to the bus to perform Direct Memory Access (DMA), transferring data to and from DRAM without involving an execution core and spending CPU cycles. The PCI standard includes a configuration mechanism that assigns a range of DRAM to each peripheral, but makes no provisions for restricting a peripheral’s DRAM accesses to its assigned range.

The Processor Die

Security extensions to the Intel architecture, such as Trusted Execution Technology (TXT) [70] and Software Guard Extensions (SGX) [14, 139], rely on the fact that the processor die includes the memory and I/O controller, and thus can prevent any device from accessing protected memory areas via Direct Memory Access (DMA) transfers. § 2.11.3 takes a deeper look at the uncore organization and at the machinery used to prevent unauthorized DMA transfers.

Out-of-Order and Speculative Execution

CPU cores can execute instructions orders of magnitude faster than DRAM can read data. Computer architects attempt to bridge this gap by using hyper-threading (§ 2.9.3), out-of-order and speculative(推理) execution, and caching, which is described in § 2.11. In CPUs that use out-of-order execution, the order in which the CPU carries out a program’s instructions (execution order) is not necessarily the same as the order in which the in- structions would be executed by a sequential evaluation system (program order)


Peripherals use bus-specific protocols to signal inter- rupts. For example, PCIe relies on Message Signaled Interrupts (MSI), which are memory writes issued to specially designed memory addresses. The bus-specific interrupt signals are received by the I/O Advanced Programmable Interrupt Controller (IOAPIC) in the PCH

The IOAPIC routes interrupt signals to one or more Local Advanced Programmable Interrupt Controllers (LAPICs).

Platform Initialization (Booting)
The UEFI Standard

The firmware in recent computers with Intel processors implements the Platform Initialization (PI) process in the Unified Extensible Firmware Interface (UEFI) specification

The computer powers up, reboots, or resumes from sleep in the Security phase (SEC). The SEC implementation is responsible for establishing a temporary memory store and loading the next stage of the firmware into it. As the first piece of software that executes on the computer, the SEC implementation is the system’s root of trust, and performs the first steps towards establishing the system’s desired security properties.


PEI

SEC is followed by the Pre-EFI Initialization phase (PEI), which initializes the computer’s DRAM, copies itself from the temporary memory store into DRAM, and tears down the temporary storage. When the computer is powering up or rebooting, the PEI implementation is also responsible for initializing all the nonvolatile storage units that contain UEFI firmware and loading the next stage of the firmware into DRAM.

microcode

Intel patents [110, 138] describing Software Guard Extensions (SGX) disclose that SGX is entirely implemented in microcode, except for the memory encryption engine. 

The use of microcode assists can be measured using the Precise Event Based Sampling (PEBS) feature in recent Intel processors. PEBS provides counters for the number of micro-ops coming from MSROM, including complex instructions and assists, counters for the numbers of assists associated with some micro-op classes (SSE and AVX stores and transitions), and a counter for assists generated by all other micro-ops.


