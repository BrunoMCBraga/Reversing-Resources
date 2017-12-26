# Rabin
* rabin2 -z(z) binary: strings. A single z prints only data section.
* rabin2 -e binary: entry point
* rabin2 -s binary: show symbols
* rabin2 -i binary: imports

# R2

## Starting
* r2 [file]: opens file (similar to IDA)
* r2 -w [file_path]: opens file in write mode. Useful for patching.
* r2 -d [binary_path]: start debugging.
* r2 -p [project_name]: loads previously saved project. This is similar to IDA loading idb.

## Static Analysis
* s [symbol_name]/[func_name]/[address]: seek to address. Seek does not mean run to or set EIP at even when you are debugging. It means that other operations that depend on current address will act on the new addres (e.g. pd can be used after seek to disassemble at the address you sought to).
* afn [new_name] [address/current_name]: modify name for function.
* axt [address]: find references to this address.
* afu [address]: defines function starting on current address up to address. This is useful if Radare is not spotting a function. 
* Ps [project_name]: save current project. Projects seem to be similar to IDA idbs. If you add comments, rename functions, create functions, you should see those when loading the project again. The project keeps track of the path for the binary so you can query symbols, entrypoint, etc when the project is loaded.


## Reckon
* aaaa: analyse binary. Useful to see references and leverange function graphs.
* dmm: list modules
* ieq: shows entry point.
* is: shows symbols.
* ii: shows imports.
* dm: memory map.

## Stack

## Registers
* dr [register]=[new_value]: sets register value.
* dr [register]: shows value of register.

## Memory Reading/Manipulation
**Notes: 
    - For memory dumps you can use variants of y (yank) or w commands. The y is more cumbersome but more flexible. It leverages the concept of a yank buffer (or clipboard) which means that in order to write to a file, you need to yank the data to the buffer, and then dump the clipboard to a file.  
    - When you must pass the path for a file as an argument, you can assume that the current directory is the location of the analysed binary. 
    - If addresses are not provided, the current offset is assumed.
**

* y [size] @[address]: read bytes from memory at address to buffer.
* yfa [file_path] copy: copies the full file to the buffer.
* yy [address]: dumps whatever is on the buffer to the memory address.
* ytf [file_path]: dump buffer data to file. 
* yf [size] [offset_within_file] [file_name]. reads size bytes of data starting at offset_within_file to the buffer. If size is -1, everything is read.

* wz [string] @[address]: write null-terminated ASCII string to address.
* ww [string] @[address]: write Unicode string to address.
* wx [hex] @[address]: writes hex to address.
* wf [file_path]: writes the content of file to current offset.
* wtf [file_path] [size]: writes the content of memory starting at the current offset to a file.

* px @[address]: prints hex dump starting at address.
* ps @[address]: prints string at address.

* /x [hex_string]: searches for hex string.
* /w [string]: search for Unicode string.
* / [string]: searches for ASCII string.

* dm [address] [size]: allocates size bytes of memory starting at address. If address is -1, it is allocated anywhere.

* o: list opened files.
* o/on [file_path] [address]: opens and maps file in memory. o is raw while on can be used for binaries. 


## Assembly
* pd/D @address-> disassemble N opcodes/bytes


## Breakpoints
* db->set breakpoint


## Stepping
* dc -> go
* ds->single step (into) or just s on VV
* dt->step over
* dcu main-> goes to main


## Threads

## Misc
* dls->clear screen
* [command] ~[expression]: greps output of command for expression
* [command] ~something: grips for something on command output

ood?


## Visual Mode
* VV->Visual mode
* p on visual mode->change views. If used on Graph, it alternates between displays. 
* VV @ address/funct name (from is): graph of function at
* d->rename current function
* :-> opens command prompt. Then i can use something like: CC comment @address
* _ string->can be used to search for functions and symbols…
* s [address] and then on visual mode df to create function










