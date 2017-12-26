# Rabin
* rabin2 -zz binary: strings. A single z prints only data section.
* rabin2 -e binary: entry point
* rabin2 -qs binary: show symbols
* rabin2 -qi binary: imports

# R2

## Starting
* r2 [file]: opens file (similar like IDA)
* r2 -w [file_path]: opens file in write mode. Useful for patching.
* r2 -d binary->start debuggingVV
* r2 -p [project_name]

## Static Analysis
* s funct_name->seek to address
* afn name [offset/current_name]: modify name for function
* Ps project_name->save as project name
* r2 - to open with nothing. Then, Po project_name
* axt [addre]->find references to this address
* afu [address]->defines function starting on current address up to add


## Reckon
* aaaa: analyse binary. Useful to see references and leverange function graphs.
dmm->list modules
* ieq: shows entry point
* is: shows symbols 

## Stack

## Registers
* dr reg=value-> set register value
* dr reg...

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


## Visual Mode
* VV->Visual mode
* p on visual mode->change views. If used on Graph, it alternates between displays. 
* VV @ address/funct name (from is): graph of function at
* d->rename current function
* :-> opens command prompt. Then i can use something like: CC comment @address
* _ string->can be used to search for functions and symbols…
* s [address] and then on visual mode df to create function











