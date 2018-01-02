## General Notes






## Reckon


## Registers
i r: print registers
set $[REGISTER NAME]=[VALUE]

## Memory Reading/Manipulation


### Writing


### Printing
* px @[address]: prints hex dump starting at address.
* ps @[address]: prints string at address.

### Searching


### Allocations and Mappings



## Assembly
disassemble [ADDRESS], [OFFSET]: disassembles between addresses.

## Breakpoints
break \*[ADDRESS]
delete \*[ADDRESS]
i b: list breakpoints

## Stepping
run: run the program
c: continue execution
step: step into
next: step over



## Processes and Threads


## Misc
start: breaks at main.
gdb -tui [BINARY FILE PATH]: start GDB in TUI mode.
(in GDB): ctrl+x, ctrl+a: enable TUI mode.












