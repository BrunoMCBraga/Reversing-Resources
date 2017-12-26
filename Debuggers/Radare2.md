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

## Stack

## Registers
* dr [register]=[new_value]: sets register value.
* dr [register]: shows value of register.

## Memory Reading/Manipulation
* y [size] @address->read bytes from memory to buffer
* yea [file] [copy]
* yy address->dumps whatever is on the buffer to memory
* ytf file_name: dump clipboard to file. The file will be on the same folder as the binary.
* w
* px @address-> hex dump with ascii
* w ‘string’ @address-> write string to address
* /x0x4444 ->search for hex string
* /w hello -> search for word string?
* ps @address-> print string at address
* o file_name->maps file in memory
* yfa file_name copy: copies all data to memory. Then yy can be used.
* yf [size] [start_within_file] [file_name]. If size is -1, everything is copied.




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










