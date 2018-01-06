## General Notes
* GDB instructions support a shoort representation format. Assuming **info sharedlibary**, we can write **i s**.
* If a thread id is not specified on **break**, all threads are subject to the breakpoints.




## Reckon
* info sharedlibrary: show loaded modules.
* info threads: show threads

## Registers
* i r: print registers
* set $[REGISTER NAME]=[VALUE]

## Memory Reading/Manipulation


### Writing
* dump [FORMAT] memory [FILE_PATH] [START_ADDRESS] [END_ADDRESS]: dumps memory range on file using the given format. The typical format is binary.
* set {char[STRING SIZE]}[ADDRESS]="[STRING]": writes the string to the address. 

### Printing
* x/nfu [ADDRESS]: prints memory. n dictates the number of units, f is the format character, u is the unit (e.g. b, h, w, g). As an example, **x/1sb [ADDRESS]** prints string at address.
* print \*(char\*\*)[ADDRESS]: prints string at address.
* How to print unicode strings????? (wchar_t)
*  poi?
### Searching


### Allocations and Mappings



## Assembly
* disassemble [ADDRESS], [OFFSET]: disassembles between addresses.

## Breakpoints
* break \*[ADDRESS]
* delete breakpoints [BREAKPOINT NUMBER]
* delete breakpoints: deletes all breakpoints
* i b: list breakpoints
* watch/rwatch/awatch \*[ADDRESS]: data breakpoints for write/read/both.

## Stepping
* run: run the program
* c: continue execution
* si: step into using instruction granularity. step can be used to step using source code lines as granularity.
* ni: step over using instruction granularity. next can be used to step using source code lines as granularity.



## Processes and Threads
* set scheduler-locking on: full locking (no thread except the current thread may run)
* thread [THREAD ID]: switch to thread.

## Misc
* start: breaks at main.
* gdb -tui [BINARY FILE PATH]: start GDB in TUI mode.
* (in GDB): ctrl+x, ctrl+a: enable TUI mode.
* set disassembly-flavor intel: changes assembly representation to INTEL's.
* info frame: information about current stack frame.










