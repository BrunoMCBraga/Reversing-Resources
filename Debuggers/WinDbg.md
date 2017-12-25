# General Notes

## Types of commands

* Session commands : e.g. kb
* Meta commands: e.g. .load
* Extension commands: !analyze, !dumpheap

## Prompt meaning

* 0:000> means target zero and thread zero (main).
* kd> kernel debugging

## Address ranges

## Evaluating Expressions


# Commands

## Getting addresses for relevant structures
* ? $peb: prints address of PEB
* ? $teb: same for TEB
* ? $proc: same for _EPROCESS

## Reckon
* lm: list modules
* x [module_name]![symbol_name]: search for symbol_name on module with module_name. symbol_name can be a regex such as \**word*\*. The modules can be found using lm.  
* ln [address] (list near): lists symbols at or close to the address.  
* dg [fs/gs/ds]: shows information about segment selectors
* dt [module_name]![type] [address] (display type): Can be used to overlap an address with a known structure (e.g. dt nt!_EPROCESS @$proc (using .process). This will align the _EPROCESS struct with the @$proc address).


## Memory Reading/Manipulation
* dds/dps/dqs [address_range]: display words and symbols referenced within the provided address_range.
* d/da/db... [address_range] : displays memory content within range. Typical usage: da (ascii strings), du (unicode strings) 
* dda/ddp/ddu [address_range]: symilar to the previous one but it will expect the memory to have a pointer to data. The pointer will be displayed and so will be the data.
* ? poi([address]): roughly speaking, reads a pointer from the address. Say you know a given address contains a pointer to something. You could use dd or simply this. This could be useful to combine with another command like *u* (e.g. u poi(address)). *NOTE:the address does not have to have a pointer there. It can be anything* 

## Assembly
u(b) [address/address_range]: disassembles a couple of instructions starting at the providded address. The b dictates that it should start disassembling back instead of forth.
u [start_address] [end_address]: disassembles instructions between the provided addresses
uf [address/address_range]: disassembly function. It parses the assembly and adds clickable links when you have branches. 

## Stack
k (and variants) : displays stack backtrace. The stack trace includes the base pointer for the stack frame (i.e. ebp for each function), the return address, and function names.



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
https://stackoverflow.com/questions/127386/in-visual-studio-c-what-are-the-memory-allocation-representations (find list for these)
