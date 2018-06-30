## What is the IDT and how to locate it?
- The IDT (Interrupt Descriptor Table) is an array with 256 8-byte entries with information to locate interrupt handlers. This array is indexed using interrupt or exception identifiers. The IDT can be located using the IDT register, a 48-bit register (i.e. 6 bytes) where the top 4 bytes contain the base of the IDT and the lower 2 bytes store the table limit. Each processor/core contain a different IDT and, therefore, the IDTR differs. The IDT can also be obtained from the Processor Control Region (PCR) which is a structure associated with each processor.

## What are the Processor Control Region and Processor Control Region Block?
The second is inside the first one. PCR contains information like the base of the IDT, CPU state (e.g. stack pointers), current IRQL. PCRB contains information about the CPU (e.g. type, model, current thread, next thread to run).  
