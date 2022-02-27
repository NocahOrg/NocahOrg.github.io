# MIDOS

This is a single tasked 32 bit OS I've been working on for the past few months.

The features of this operating system will include:

- A basic shell with commands (mkdir, create, sysinfo, assemble, cd, etc...)
- A basic text editor to create assembly programs
- an assembler to assemble programs
- A system library that allows the user to collect input and output data on to the screen
- ability to store files to Floppy through the FAT12 File system
- ability to install OS on to a HDD on the system from floppy (this sort of mimics the "try ubuntu or install ubuntu" feature)

The github for this project can be found [here](https://github.com/micahwagner/My_OS).

# Prerequisites:
Below is a list of prerequisite knowledge needed before building a 32-bit x86 operating system

- [This](https://www.youtube.com/watch?v=LnzuMJLZRdU&list=PLowKtXNTBypFbtuVMUVXNR0z1mu7dp7eH) link leads to a playlist created by ben eater. In this series, Ben Eater creates a 6502 computer system. The reason I recommend this is because it gives a great base understanding of how the cpu interacts with the rest of the components (ram, rom, io, etc..).
- [This](https://8bitworkshop.com/v3.8.0/?platform=vcs&file=examples%2Fhello.a) link leads to an online IDE that allows you to code in 6502 assembly for an atari, and many other old computer systems. This is a great source for learning how to code in assembly.
- [This](https://skilldrick.github.io/easy6502/) link helped me understand 6502 assembly. it goes through all the basics of 6502 assembly. highly recommend for learning 6502 assembly.
- [This](https://www.youtube.com/watch?v=ojHSzW3zVNU&list=PLZlHzKk21aImqCiV71iE2I1dUE5LNejQk) link isn't necessary, but it gives a great understanding of how cpu's work internally. 
- [This](https://www.cs.virginia.edu/~evans/cs216/guides/x86.html) is a great source for learning x86 assembly
- Lastly, learning C is necessary to program an operating system. I suggest watching some youtube tutorials on C.

# Sections:

- [Tools](Dev/bootloaderTools.md)
- [Bootloader](/bootloader/bootloader.md)
- [The File System](/FS/FAT12.md)
- [Switching to C](Dev/SwitchingToC.md)
- [The Kernel](/kernel/kernel.md)
