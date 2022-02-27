

# The Basic Idea:

My OS's bootloader is not an UEFI bootloader. Since UEFI can emulate Legacy bios's, my bootloader is a Legacy Bootloader (more detail [here](https://linuxhint.com/difference-between-uefi-and-legacy/#:~:text=UEFI%20runs%20in%2032%2Dbit,systems%20(OS)%20as%20applications.) about UEFI vs Legacy).When the compuer is booted, the bios and other firmware is executed (more detail [here](https://manybutfinite.com/post/how-computers-boot-up/)). After the firmware checks the hardware, the firmware then scans each disk (or CD / floppy / USB / etc...) to check if they are bootable. Once the bios hits a bootable medium, it will load the first sector from that disk into the RAM address 0x7c00, then start executing that code. This first sector is referd to as the Boot Loader. The boot loader for MIDOS is a 2 staged bootloader, meaning that the first stage (which is located on the first sector of the disk) is only meant to load the second stage, and then switch execution to the second stage. The reason for this is because the bios loads the first sector of the disk, but each sector is only 512 bytes. Because each sector is only 512 bytes, that means the stage 1 boot loader can only be 512 bytes. As you could imagine, 512 bytes is very small. To get around this size limitation, all we have Stage 1 do is load Stage 2 (stage 2 being much larger than 512 bytes). Stage 2 takes care of setting up the hardware, and loading the kernel.  

# Stage 1:

In order to make a disk bootable, a specific magic value is needed at the end of the first sector of the disk. When the bios recognizes this magic number, it will then boot that disk. Below shows a very basic bootloader that clears the interrupts and halts the cpu.

```
bits	16 				; 16 bit real mode	

org 	0x7c00 				; we are loaded by bios to 0x7c00	

cli 					; clear interrupts
hlt 					; halt cpu

times 510 - ($-$$) db 0  ; pad the rest of the sector with 0
dw  	0xaa55 			; magic number to tell bios that this disk is bootable
``` 

The 0xaa55 is the magic number that the bios will recognize. The command to compile this is shown below.

```
nasm -f bin Stage1.asm -o Stage1.bin
```

The Image below shows the output in machine code.

# Stage 2: