# Rabin
* rabin2 -z(z) binary: strings. A single z prints only data section.
* rabin2 -e binary: entry point
* rabin2 -s binary: show symbols
* rabin2 -i binary: imports

# R2

## General Notes
* If addresses/offsets are not provided for most instructions, the current offset is assumed.
* The prompt **[address]>** indicates where you are. If you are doing static analysis, everytime you use **s (seek)** this address will change to the sought address. On debug mode, using the seek command will not change the EIP for the current offset and the EIP may be the same but not always. 

### Internal Configuration Variables
Radare has a huge list of configurable variables. The following commands may prove useful:
* e??: list all variables.
* e??[expression]: search for configurations starting with expression (e.g. e?? dbg will show all the variables within dbg).
* e [variable]=[value]: sets variable (e.g. **e dbg.forks=true** will stop execution if fork is called). 

## Starting
* r2 [file_path]: opens file (similar to IDA)
* r2 -w [file_path]: opens file in write mode. Useful for patching.
* r2 -d [binary_path]: start debugging.
* r2 -p [project_name]: loads previously saved project. This is similar to IDA loading idb.

## Static Analysis
* s [symbol_name]/[func_name]/[address]: seek to address. Seek does not mean run to or set EIP at even when you are debugging. It means that other operations that depend on current address will act on the new addres (e.g. pd can be used after seek to disassemble at the address you sought to).
* afn [new_name] [address/current_name]: modify name for function.
* axt [address]: find references to this address.
* afu [address]: defines function starting on current address up to address. This is useful if Radare is not spotting a function. 
* Ps [project_name]: save current project. Projects seem to be similar to IDA idbs. If you add comments, rename functions, create functions, you should see those when loading the project again. The project keeps track of the path for the binary so you can query symbols, entrypoint, etc when the project is loaded.
* CC [string] @[address]: adds comment at address.


## Reckon
* aaaa: performs deep analysis of the binary. Useful to later see references and leverage function graphs.
* dmm: list modules
* ieq: shows entry point.
* is: shows symbols.
* ii: shows imports.
* dm: memory map.
* dd: file descriptors.

## Stack

## Registers
* dr [register]=[new_value]: sets register value.
* dr [register]: shows value of register.

## Memory Reading/Manipulation
**Notes: 
    - For memory dumps you can use variants of y (yank) or w commands. The y is more cumbersome but more flexible. It leverages the concept of a yank buffer (or clipboard) which means that in order to write to a file, you need to yank the data to the buffer, and then dump the clipboard to a file.  
    - When you must pass the path for a file as an argument, you can assume that the current directory is the location of the analysed binary.**

### Yanking 
* y [size] @[address]: read bytes from memory at address to buffer.
* yfa [file_path] copy: copies the full file to the buffer.
* yy [address]: dumps whatever is on the buffer to the memory address.
* ytf [file_path]: dump buffer data to file. 
* yf [size] [offset_within_file] [file_name]. reads size bytes of data starting at offset_within_file to the buffer. If size is -1, everything is read.

### Writing
* wz [string] @[address]: write null-terminated ASCII string to address.
* ww [string] @[address]: write Unicode string to address.
* wx [hex] @[address]: writes hex to address.
* wf [file_path]: writes the content of file to current offset.
* wtf [file_path] [size]: writes the content of memory starting at the current offset to a file.

### Printing
* px @[address]: prints hex dump starting at address.
* ps @[address]: prints string at address.

### Searching
* /x [hex_string]: searches for hex string.
* /w [string]: search for Unicode string.
* / [string]: searches for ASCII string.

### Allocations and Mappings
* dm [address] [size]: allocates size bytes of memory starting at address. If address is -1, it is allocated anywhere.
* o: list opened files.
* o/on [file_path] [address]: opens and maps file in memory. o is raw while on can be used for binaries. 


## Assembly
* pd/D [-][size] @[address]: disassemble size instructions/bytes. The minus can be used to print backwards from address.
* pdfs @[address]: prints information about function (section, symbols, calls, jumps).

## Breakpoints
* db: list software breakpoints.
* db [address/symbol_name]: set software breakpoint.
* db -[address/symbol_name]: remove software breakpoint.
* dbi: list breakpoint indexex.
* dbid [index]: disable breakpoint by index.
* dbc [address/symbol_name] [cmd]/dbic [index]: run command at breakpoint address/index.
* dbw [addr] [rwx]: sets hardware breakpoint at address. 
* drx: list hardware breakpoints.
* drx [index] [address] [length] [rwx]: changes defined breakpoint. The indexes are obtained based on the CPU register used. The length can be 1,2,4 or 8 bytes.


## Stepping
* dc: go
* ds: single step (will step into functions) or just s on VV
* dso: single step (will skip function calls)
* dt: step over
* dcu/dsu [address/synbol]: runs to/steps to run.
* dtc [address] ([start_address] - [end_address]): trace range os instructions or just traces to address.
* (visual mode) s/S: step/step-over.

## Processes and Threads
* dp: list pid and children.
* dpt: list threads.
* dpt=[thread_id]: attach to thread.

## Misc
* dls: clear screen
* [command] ~[expression]: greps output of command for expression.
* [command] ~something: greps for something on command output.
* [command] | less: self-explanatory.
* ood: reopen file in debug mode.


## Visual Mode
* VV: visual mode (graph mode to be more precise). If **q** is pressed, it will show other non-graph views. If the key is pressed again it returns to command mode.
* p: change views. If used on graph mode, it alternates between display types. 
* VV @[address/symbol]: graph of function at.
* d: rename current function
* : :opens command prompt. Then a command like **is** can be used.
* C: toggle colours.









