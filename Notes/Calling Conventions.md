
# IA-32
## CDECL
- Arguments: pushed on stack in inverse order
- Cleaner: caller
- Return value: EAX
- Non-volatile registers (saved by callee): EBP, ESP, EBX, ESI, EDI

## STDCALL
- Arguments: pushed on stack in inverse order
- Cleaner: calleee
- Return value: EAX
- Non-volatile registers (saved by callee): EBP, ESP, EBX, ESI, EDI

## FASTCALL
- Arguments: ECX, EDX and then stack
- Cleaner: callee
- Return value: EAX
- Non-volatile registers (saved by callee): EBP, ESP, EBX, ESI, EDI

## THISCALL (GCC)
- Arguments: pushed on stack in inverse order. The **this** is pushed last being the first parameter of every function.
- Cleaner: caller
- Return value: EAX
- Non-volatile registers (saved by callee): EBP, ESP, EBX, ESI, EDI

## THISCALL (Microsoft)
- Arguments: this passed on ECX and then stack
- Cleaner: callee typically but for functions with variable number of arguments, the caller does it.
- Return value: EAX
- Non-volatile registers (saved by callee): EBP, ESP, EBX, ESI, EDI



# x86-64

## Microsoft x64 calling convention:
- Arguments: RCX/XMM0, RDX/XMM1, R8/XMM2, R9/XMM3 and then stack. 32 bytes of shadow space must be allocated by the caller. This is used by compilers to put a copy of the arguments passed on registers on the stack to facilitate debugging.
- Cleaner: caller 
- Return value: RAX for 64 bits or less, XMM0 for floats
- Non-volatile registers (saved by callee): RBX, RBP, RDI, RSI, RSP, R12, R13, R14, R15


## System V AMD64 ABI
- Arguments: RDI, RSI, RDX, RCX, R8, R9, XMM0–7. 
- Cleaner: caller 
- Return value: RAX for up to 64 bits, RAX, RDX for up to 128 bits, XMM0 and XMM1 for floats 
- Non-volatile registers (saved by callee): RBX, RBP, RSP, R12, R13, R14, R15



**Note: In general, the registers EAX, ECX and EDX should be saved by the caller.**
