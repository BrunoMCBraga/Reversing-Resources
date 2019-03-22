# General Notes

## Types of Commands

* Session commands : e.g. kb
* Meta commands: e.g. .load
* Extension commands: !analyze, !dumpheap

## Prompt Meaning

* 0:000>: means target zero and thread zero.
* kd>: kernel debugging

## Memory Interpretation
* When a memory dump displays question marks, the address is not accessible since the memory is not mapped on the address or belongs to kernel.
* Memory is typically initialised with standard values:
  * 0xABABABAB : Used by Microsoft's HeapAlloc() to mark "no man's land" guard bytes after allocated heap memory
  * 0xABADCAFE : A startup to this value to initialize all free memory to catch errant pointers
  * 0xBAADF00D : Used by Microsoft's LocalAlloc(LMEM_FIXED) to mark uninitialised allocated heap memory
  * 0xBADCAB1E : Error Code returned to the Microsoft eVC debugger when connection is severed to the debugger
  * 0xBEEFCACE : Used by Microsoft .NET as a magic number in resource files
  * 0xCCCCCCCC : Used by Microsoft's C++ debugging runtime library to mark uninitialised stack memory
  * 0xCDCDCDCD : Used by Microsoft's C++ debugging runtime library to mark uninitialised heap memory
  * 0xDDDDDDDD : Used by Microsoft's C++ debugging heap to mark freed heap memory
  * 0xDEADDEAD : A Microsoft Windows STOP Error code used when the user manually initiates the crash.
  * 0xFDFDFDFD : Used by Microsoft's C++ debugging heap to mark "no man's land" guard bytes before and after allocated heap memory
  * 0xFEEEFEEE : Used by Microsoft's HeapFree() to mark freed heap memory

More cases can be found under the "Magic debug values" section on: https://en.wikipedia.org/wiki/Magic_number_(programming).

## Address Ranges
There is a whole page dedicated to this topic here: https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/address-and-address-range-syntax. However, i tend to rely on:
* Explicit: u [first_address] [last_address]-> disassembles whatever is between the aforementioned addresses.
* Implicit: dd [address] L[number_of_units]-> prints number_of_units dwords starting at address. If i used db, you would get the same number of units but bytes, not words (dd understands dwords while db understands bytes).
**Note: Sometimes the L will not work for ranges that are too large. Use L?**

## Evaluating Expressions
WinDbg is capable of interpreting expressions as well as interpreting data in memory as C++ objects:
* https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/---evaluate-expression-
* https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/----evaluate-c---expression-
* https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/evaluating-expressions

Some examples:
* ? eip: prints eip
* ? poi(esp): prints a dword from the top of the stack
* ?? @@c++(sizeof(0x0)): prints size of data. In this case, it is an integer with four bytes.

? means evaluate expression while ?? @@c++ means evaluate C++ expression. It is possible to evaluate MASM expressions as well. 

# Commands

## Help
* ?: command help
* .help: meta-command help

## Getting Addresses for Relevant Structures
* ? $peb: prints address of PEB
* !peb: shows PEB
* ? $teb: same for TEB
* !teb: shows TEB
* ? $proc: same for _EPROCESS

## Reckon
* lm: list modules
* !lmi [module_name]: more information about module. 
* !dh [address]/[module_name]: displays header information about module at address or with name. Useful to find entrypoint.
* x [module_name]![symbol_name]: search for symbol_name on module with module_name. symbol_name can be a regex such as \**word*\*. The modules can be found using lm. Also, this search is for functions. Check dt. 
* ln [address] (list near): lists symbols at or close to the address.  
* dg: shows information about segment selectors (e.g. dg ds)
* dt [module_name]![type] [address] (display type): can be used to overlap an address with a known structure (e.g. dt nt!_EPROCESS @$proc (using .process). This will align the _EPROCESS struct with the @$proc address). dt can also be used to enumerate structures using a search syntax similar to x.
* dv [pattern]: displays local variables with a given pattern. Useful when symbols are present. 
* !address [address]: memory map. Address is optional but if provided prints a summary about the address (e.g. range, permissions).

## Stack (~thread_number allows you to choose the thread)
* k (and variants) : displays stack backtrace. The stack trace includes the base pointer for the stack frame (i.e. ebp for each function), the return address, and function names.

## Registers (~thread_number allows you to choose the thread)
* r: registers information.
* r [register]=[value]: sets register value.
* r [register]: print register value

## Memory Reading/Manipulation
* dds/dps/dqs [address_range]: display words and symbols referenced within the provided address_range.
* d/da/db... [address_range] : displays memory content within range. Typical usage: da (ascii strings), du (unicode strings) 
* dda/ddp/ddu [address_range]: symilar to the previous one but it will expect the memory to have a pointer to data. The pointer will be displayed and so will be the data.
* ? poi([address]): roughly speaking, reads a pointer from the address. Say you know a given address contains a pointer to something. You could use dd or simply this. This could be useful to combine with another command like *u* (e.g. u poi(address)). **NOTE: the address does not have to have a pointer there. It can be anything** 
* s [address_range]  [pattern]: search memory for pattern.
* e/ea/eb/ed [address] [data]: modify memory (e.g. eza [address] "Hello World!" will write a null-terminated ASCII string to [address]).
* .writemem [file_path] [address_range]: dumps memory within address_range to a file.
* .readmem [file_path] [address_range]: reads file and dumps content into address.
* .dvalloc [size]: leverages VirtualAllocEx to allocate extra memory.
* s [STARTING_ADDRESS] L?[LENGTH]/[END_ADDRESS] [PATTERN]: search for pattern in memory. PATTERN can be a sequence of bytes.

## Assembly
* u(b) [address/address_range]: disassembles a couple of instructions starting at the providded address. The b dictates that it should start disassembling back instead of forth.
* u [start_address] [end_address]: disassembles instructions between the provided addresses
* uf [address/address_range/symbol_name]: disassembly function. It parses the assembly and adds clickable links when you have branches. 

## Breakpoints  (~thread_number allows you to choose the thread)
* bp [address] "[COMMAND]": software breakpoint at address and executed command
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
* wt [start_address] [end_address]: trace and watch changes. Good to understand what happens during the execution of the function.

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

## Configuring Symbols:
1. On WinDbg go to File->Symbol File Path...
2. Paste srv*c:\MyServerSymbols\*https://msdl.microsoft.com/download/symbols. The \*c:\MyServerSymbols\* represents the path were the downloaded symbols will be saved.
3. Use the extension command **.reload -f** to force all the symbols to be loaded. If the binary dynamically loads dlls later and if you suspect the symbols are not being identified, use the command again.

## Misc
* .lastevent: shows the last exception or event happening.
* !gle: shows last error formatted. Useful when an API error is returned and there is a need to translate that error into something human-readable.
* !error [error_value]: converts error hex code to human-readable string.
* !dreg [registry_key]: query registry.
* !exchain: prints current exception chain.
* !slist, !list: list iteration.


## Kernel Debugging
!idt : dump IDT

## .NET Debugging (.NET 4.0)
### Preparation
1. Breaking on clrjit load: sxe ld:clrjit
2. Run with g
3. .cordll -ve -u -l (.loadby sos clr if something fails) 

At this point, the necessary modules are loaded.

### Help
* !SOS.help: shows SOS commands
* !SOS.help ![COMMAND]: shows inofrmation about commands (e.g. !SOS.help !bpmd)

### Assemblies, Modules, Methods
* !dumpdomain: shows information about loaded assemblies and module descriptors.
* !dumpassembly [ASSEMBLY_DESCRIPTOR]: does not add more information when compared to previous command but it is more targetted to assembly.
* !dumpmodule -mt [MODULE_DESCRIPTOR]: based on [MODULE_DESCRIPTOR] obtained from previous command. Lists types defined and referenced.
* !dumpmt -md [METHOD_TABLE]: Based on the descriptors under MT you can list methods. From those you can get the descriptors. 


### Landing on main and inspecting IL
1. !clrstack: if you ran g once, you will likely see at the top of the stack: ... [MODULE].Main(System.String[]). Copy [MODULE].Main(System.String[]).
2. !name2ee [MODULE].Main(System.String[]): This will show you some fields like: Module, Assembly, MethodDesc and a message: "Not JITTED yet. Use !bpmd -md [MethodDesc] to break on run". This means the method is not compiled to native code yet.
3. !bpmd -md [MethodDesc]: Sets a breakpoint based on method descriptor.
4. g: runs until the breakpoint is hit

**Note: After i wrote the instructions above i inspected the help docs using !sos.help for !bpmd and found:***

```
This brings up a good question: "I want to set a breakpoint on the main
method of my application. How can I do this?"

  1) If you know the full path to SOS, use this command and skip to step 6
       .load <the full path to sos.dll>

  2) If you don't know the full path to sos, its usually next to clr.dll
     You can wait for clr to load and then find it.
     Start the debugger and type: 
       sxe -c "" clrn
  3) g
  4) You'll get the following notification from the debugger:
     "CLR notification: module 'mscorlib' loaded"
  5) Now you can load SOS. Type
       .loadby sos clr

  6) Add the breakpoint with command such as:
       !bpmd myapp.exe MyApp.Main
  7) g
  8) You will stop at the start of MyApp.Main. If you type "bl" you will 
     see the breakpoint listed.

```

Now we inspect IL:
1. !ip2md [INSTRUCTION_POINTER]: converts instruction pointer to method descriptor. Not needed if you still have it from previous commands.
2. !dumpil [METHOD_DESCRIPTOR]: dumps il for method based on descriptor.  
Optionally, you can run !u [ASSEMBLY_ADDRESS] and you will see the disassembly that is more verbose (e.g. string references are dereferenced so you can see them, .NET methods are shown like System.Console.WriteLine(System.String))


### Once inside function
* !clrstack -a: adds parameters and locals to output of !clrstack. 
* !dumpobj /d [PARAMETER_DESCRIPTOR]: prints information about object, e.g. the parameter.

Assuming an array:
1. !dumparray /d [ARRAY_DESCRIPTOR]: where [ARRAY_DESCRIPTOR] is obtained from !dumpobj. This will show you for each index the descriptor for the object.
2. !dumpobj /d [OBJECT_DESCRIPTOR]: where [OBJECT_DESCRIPTOR] can be obtained from the previous command. This command can be used to dump content of objects (e.g. string).

### Navigating code
Bsides the usual t, g, etc, you can try to move with "managed breakpoint". Once you run !dumpil you will see offsets on the left of each IL line. You can then use !bpmd as so:
* !bpmd [MODULE_NAME] [METHOD_NAME] [IL_OFFSET]: where module name can be obtained from lm, method name can be simply [TYPE].[METHOD] (no argument signature needed) and il offset is not e.g. IL_001d bu 001d.

Based on this, by inspecting methods using WinDbg and DNSpy, you can then select where to land and speed up the analysis.

### Inspect Threads, Objects
* !threads: shows managed threads.
* !do [ADDRESS]: inspect object at address (i.e. prints fields) 

### QUESTIONS
- Breaking on constructor??

# Resources
* http://windbg.info/doc/1-common-cmds.html#14_tracing
* https://github.com/lowleveldesign/debug-recipes/blob/master/debugging-using-windbg/sos.help.txt
* https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-managed-code

