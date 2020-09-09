# ZyXOS
The OS for ZyX-20x4

## Booting

- When the computer is turned off, [BIOS](https://github.com/Z20x4/ZyX-20x4/tree/master/Firmware/bios) is stored in the ROM of controller
- After the computer is turned on, BIOS is loaded to its RAM
- BIOS loads boot loader from the computer's ROM
- Boot loader loads and starts the OS
  - **#TODO: should be more detailed here**


## Syscalls

The main way of process communication with the operating system is syscalls.

#### What happens when the system is called in ZyXOS:
- The current process tries to make a system call, which means, to access the memory, it is not allowed
- When that  forbidden address is tried to be accessed, MMU raises an NMI for the processor
- The NMI handler (stored in 0066h) can recognize that address as a location of syscall handler
- The OS enters the kernel mode
- The program counter is set to that address, and the system call is performed
- The OS enters user mode
- Process continues execution



#### Implemented syscalls

The ZyXOS will have all 5 types of syscalls: 
- Process control
- File management
- Device management
- Information maintenance
- Communication

#### TODO: Description of each syscall, present in the ZyXOS


## Memory management

ZyXOS works with an external [Memory Management Unit](https://github.com/Z20x4/FPGA), but also keeps the copy of MMU mapping in RAM.
Each process is associated with own pages mapping table with such properties:
- Some fixed pages are mapped 1 to 1 that are not accessible from user mode (e.g. first pages will contain syscalls handlers, BIOS functions and so on.)
- When the process starts, most of the pages (except stack and .text section) are not commited until they are needed 


## Filesystem

The ZyXOS will use the FAT16 file system, with memory controller made in Arduino.
