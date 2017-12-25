# General Notes

## Types of commands
..*Session commands : e.g. kb
..*Meta commands: e.g. .load
..*Extension commands: !analyze, !dumpheap

Prompt meaning:
0:000> means target zero and thread zero (main).
kd> kernel debugging

Break instruction information:1.Exception code address (kernel code)
							  2.Code and address for last executed instrution (application).


Session commands:
lm: list modules
x: examine symbols
ln: list near
dg: shows information about segments
u (8-bytes after eip)/uf (function)/ub (8-bytes before eip): unassembly. 
dds/dps/dqs : display words and symbols. Useful to check address agains symbol table
dda, ddp, ddu... : takes a pointer and checks what is pointed by it (e.g. string).
d : display memory. Register names can be used (e.g. dc esp)
k : displays stack backtrace. k = BasePtr StackPtr InstructionPtr can be used to reconstruct a stackframe.
ChildEBP (BasePtr) = EBP for function
RetAddr (InstructionPtr) = address of next instruction executed when function returns
StackPtr = Stack Pointer
dt : display type. Can be used to overlap an address with a known structure: dt nt!_EPROCESS @$proc (using .process). This will align the _EPROCESS struct with the @$proc address.
dv : display local variables. Useful when there are symbols.
g <address>/t (jumps into function)/gu/pc/p/wt (similar to t but does not go into function)/t : run/trace(single step)/runs to the end offunction /pc runs until next call function/step over/ trace with watch
s : search memory
dx : Takes a C++ expressiona and displays results. Windbg must implement some internal object model for this.
r : register related stuff
bp/bu/bm/ba : software breakpoint/hardware breakpoint. I can set conditional breakpoints.
~[Thread Index]s/~~[Internal Thread Id]: switches to thread given either the index or the Id. Both can be obtained writing ~. The s command only changes context and thread until another thread takes over (quantum elapses).
~<tid>n/~<tid>m - suspend/resume thread. Freeze and unfreeze are just debugger seetings. 
e, ea, eb, ed: enter values. Can be used to change memory

Extension commands:
!process (only on kernel mode debugging): used to get information about processes. 
!lmi: more information about module. Complements lm
!dh: displays header information about module.
!address: shows modules and memory. If i provide the address i get the base and permissions.
!peb: show PEB
!teb: show TEB
!gle: shows last error formatted
!error [hex_id]: used to convert hex id to human-readable string
!dreg: used to query registry
!exchain: useful to show exception list
!slist, !list: useful to show lists


Meta commands>
.lastevent: shows the last exception or event happening.
.sympath(+): changes symbol path or appends new path
.symfix <directory> (+): sets path to microsoft/appends microsoft path. an argument local directory is used to cache symbols.
.reload: reloads symbols for all or specific modules.
?: evaluate expression (e.g. ? $thread gives the address of TEB (user mode)/ETHREAD (kernel mode))


Symbols:
$ used for pseudo-registers
.reload -f
Address of PEB: ? $peb

Notes:
-when a memory dump displays ????, the address is not accessible since the memory is not mapped on the address or belongs to kernel.


Questions?
-Changing the registers
-Investigate meaning of !address fields-
-bp ?(0x00c30000 + 0xCC45) ? why won't this work
-SEH (Adv Wind De pag 191): what is 0xFFFFFFFE and CT?
-Freeze/Unfreeze thread? are they suspended? how does the debugger manage that?

URLS:
https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/reading-and-writing-memory
https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/thread-syntax
