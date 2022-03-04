# Emulators:

I use several hardware / emulators to ensure my OS is capable of running on a generic 32 bit x86 system. 

- [This site](https://copy.sh/v86/) is an emulator on the browser for a 32 bit x86 system. Debugging the CPU state with this emulator is not possible (at least to my knowledge), but it does give an option to get a memory dump. Getting an entire memory dump is extremely useful so I use this website quite often.

- Bochs is the second emulator I use. I actually don't really like bochs that much, mainly because it doesn't perform very well. Bochs is great for debugging, especially the cpu state. Bochs also has a pretty bad problem with the PIT, as it doesn't count properly. Its kind of hard to describe the problem, but just know that the PIT does not function properly. Surprisingly the browser emulator runs way faster than bochs. I wouldn't suggest using bochs. Instead use qemu or maybe even a virtual box.

- The last thing I like to run my kernel on is an actual 32 bit x86 computer. After testing the OS on bochs and the online emulator, I like to run it on raw metal to ensure its working properly. I've had several issues on real hardware (specific issues with the PIC) that I never would have known unless I tested it on real hardware. The computer I have has 1GB of RAM  (one of the reasons I decided not to have a higher half kernel), pentium 4, PS2 keyboard and mouse ports, and a floppy reader. You can get a PC like this on ebay, with price ranging from $50 to $120. I would suggest getting a PC and not a laptop, mainly because a PC is easily upgradable. 

# Compilers/Assemblers: 

- For the assembler, I'm using the NASM assembler. I highly suggest nasm because its easy to use, and it targets a wide variety of architectures.

- For the C compiler, I'm using a simple docker container that has a cross compiler that targets raw 32 bit machine code. [This](https://github.com/kevincharm/i686-elf-gcc-toolchain) is the link to the docker container. More about developing a kernel with C is discussed [here](/SwitchingToC.md).

# Disk creation:

- To create a preformatted FAT12 floppy disk image, I use the hdiutil command thats built into macos. It works pretty well, and I haven't had any issues with it so far.

- When writing my bootloader to the disk image, I use a command called dd, which essentially lets you copy raw bytes from one location to another. I also use this when writing the OS to a real floppy disk. 