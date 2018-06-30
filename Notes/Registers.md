* Which control registers exist and what are the usages?
- CR0: controls things like paging, mode (protected vs. real)
- CR2: containes the address that caused a page fault
- CR3: upper 20 bits address the first table used as a starting point to translate virtual into physical addresses
- CR4: controls PAE, Virtual Machine Extensions, etc

* Which debug resisters exist and what are the usages?
- DR0-DR3:
- DR6:
- DR7:
Add information here later!!!!

* What are model-specific registers?
These registers are CPU-specific and are indexed using a 32-bit integer and read/written through RDMSR/WRMSR instructins. They are only accessible on ring 0. 
SYSENTER transfers execution to the address stored in MSR 0x176 (IA32_SYSENTER_EIP) which is set by the OS and contains the handler for system calls. 
