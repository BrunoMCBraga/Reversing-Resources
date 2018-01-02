## General Notes
* GDB instructions support a shoort representation format. Assuming **info sharedlibary**, we can write **i s**.





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

### Searching


### Allocations and Mappings



## Assembly
* disassemble [ADDRESS], [OFFSET]: disassembles between addresses.

## Breakpoints
* break \*[ADDRESS]
* delete \*[ADDRESS]
* i b: list breakpoints

## Stepping
* run: run the program
* c: continue execution
* step: step into
* next: step over



## Processes and Threads


## Misc
* start: breaks at main.
* gdb -tui [BINARY FILE PATH]: start GDB in TUI mode.
* (in GDB): ctrl+x, ctrl+a: enable TUI mode.
* set disassembly-flavor intel: changes assembly representation to INTEL's.











