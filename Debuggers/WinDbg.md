# General Notes

## Types of commands

* Session commands : e.g. kb
* Meta commands: e.g. .load
* Extension commands: !analyze, !dumpheap

## Prompt meaning

* 0:000> means target zero and thread zero (main).
* kd> kernel debugging

## Memory Interpretation
* When a memory dump displays question marks, the address is not accessible since the memory is not mapped on the address or belongs to kernel.
*< dead beef

## Address ranges

## Evaluating Expressions
?: evaluate expression (e.g. ? $thread gives the address of TEB (user mode)/ETHREAD (kernel mode))


# Commands

## Help
* ?: command help
* .help: meta-command help

## Getting addresses for relevant structures
* ? $peb: prints address of PEB
* !peb: show PEB
* ? $teb: same for TEB
* !teb: show TEB
* ? $proc: same for _EPROCESS


## Reckon
* lm: list modules
* !lmi [module_name]: more information about module. 
* !dh [address]: displays header information about module at address.
* x [module_name]![symbol_name]: search for symbol_name on module with module_name. symbol_name can be a regex such as \**word*\*. The modules can be found using lm.  
* ln [address] (list near): lists symbols at or close to the address.  
* dg: shows information about segment selectors (e.g. dg ds)
* dt [module_name]![type] [address] (display type): Can be used to overlap an address with a known structure (e.g. dt nt!_EPROCESS @$proc (using .process). This will align the _EPROCESS struct with the @$proc address).
* dv [pattern]: displays local variables with a given pattern. Useful when symbols are present. 
* !address [address]: memory map. Address is optional but if provided prints a summary about the address (e.g. range, permissions).


## Memory reading/manipulation
* dds/dps/dqs [address_range]: display words and symbols referenced within the provided address_range.
* d/da/db... [address_range] : displays memory content within range. Typical usage: da (ascii strings), du (unicode strings) 
* dda/ddp/ddu [address_range]: symilar to the previous one but it will expect the memory to have a pointer to data. The pointer will be displayed and so will be the data.
* ? poi([address]): roughly speaking, reads a pointer from the address. Say you know a given address contains a pointer to something. You could use dd or simply this. This could be useful to combine with another command like *u* (e.g. u poi(address)). **NOTE: the address does not have to have a pointer there. It can be anything** 
* s [address_range]  [pattern]: search memory for pattern.
* e/ea/eb/ed [address] [data]: modify memory (e.g. eza [address] "Hello World!" will write a null-terminated ASCII string to [address]).
* .writemem [file_path] [address_range]: dumps memory within address_range to a file.

## Assembly
* u(b) [address/address_range]: disassembles a couple of instructions starting at the providded address. The b dictates that it should start disassembling back instead of forth.
* u [start_address] [end_address]: disassembles instructions between the provided addresses
* uf [address/address_range]: disassembly function. It parses the assembly and adds clickable links when you have branches. 

## Stack (~thread_number allows you to choose the thread)
* k (and variants) : displays stack backtrace. The stack trace includes the base pointer for the stack frame (i.e. ebp for each function), the return address, and function names.

## Registers (~thread_number allows you to choose the thread)
* r: registers information.
* r [register]=[value]: sets register value.
* r [register]: print register value

## Breakpoints  (~thread_number allows you to choose the thread)
* bp [address]: software breakpoint at address
* bu [module_name]![symbol_name] (unresolved breakpoint): breakpoint is not associated with memory but with a symbol that, when loaded and resolved will hold an address. Useful when we know a binary will load a dll and call a function later on.
* bm [module_name]![symbol_name]: similar to bu but assumes the symbol is resolved and the module loaded.
* ba [address] (break on access): sets data/hardware breakpoint.     
* bl: list breakpoint ids and addresses.
* bc [breakpoint_id]: deletes breakpoint with given id. The id can be obtained from bl. bc \* will delete everything.

## Stepping (~thread_number allows you to choose the thread)
* g [start_address] [break_address] (go): simple go. With arguments we can dictate where it starts and where it ends.  
* gu: go until return is hit.
* t: step into, i.e., functions are iterated and not skipped as with the next command.
* p: single step as t but routines are skipped.
* pc:executes until a call is reached.
* wt: trace and watch changes. Good to understand what happens during the execution of the function.

**Note: Both breakpoints and stepping commands can be combined with WinDbg commands. You can set a break point after a function responsible for dynamic resolution of library functions as such: bp [function_address] "ln eax". This will print the symbol at the resolved address verytime the function is executed.**

## Threads
Threads can be identified as such:
* ~.: current thread.
* ~#: thread that caused exception or debug event.
* ~*: all threads.
* ~[ordinal]: thread with given ordinal. 
* \~\~[TID]: thread with given id.

**Notes: 
  - These identifiers can be used standalone or prepended to others like breakpoints and stepping to identify which thread should be afected.
  - s can be preprended to the last two identifers to switch contexts. This is useful if you want to inspect the state of other threads.
**

## Configuring symbols:
1. On WinDbg go to File->Symbol File Path...
2. Paste srv*c:\MyServerSymbols*https://msdl.microsoft.com/download/symbols. The \*c:\MyServerSymbols\* represents the path were the downloaded symbols will be saved.
3. Use the extension command \*.reload -f\* to force all the symbols to be loaded. If the binary dynamically loads dlls later and if you suspect the symbols are not being identified, use the command again.

## Misc
* .lastevent: shows the last exception or event happening.
* !gle: shows last error formatted. Useful when an API error is returned and there is a need to translate that error into something human-readable.
* !error [error_value]: converts error hex code to human-readable string.
* !dreg [registry_key]: query registry.
* !exchain: prints current exception chain.
* !slist, !list: list iteration.



Questions?
-Investigate meaning of !address fields-
-bp ?(0x00c30000 + 0xCC45) ? why won't this work
-SEH (Adv Wind De pag 191): what is 0xFFFFFFFE and CT?
-Freeze/Unfreeze thread? are they suspended? how does the debugger manage that?

URLS:
https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/reading-and-writing-memory
https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/thread-syntax
https://stackoverflow.com/questions/127386/in-visual-studio-c-what-are-the-memory-allocation-representations (find list for these)
