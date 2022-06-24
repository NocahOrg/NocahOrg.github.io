

# The Basic Idea:

My OS's bootloader is not an UEFI bootloader. Since UEFI can emulate Legacy bios's, my bootloader is a Legacy Bootloader (more detail [here](https://linuxhint.com/difference-between-uefi-and-legacy/#:~:text=UEFI%20runs%20in%2032%2Dbit,systems%20(OS)%20as%20applications.) about UEFI vs Legacy).When the computer is booted, the bios and other firmware is executed (more detail [here](https://manybutfinite.com/post/how-computers-boot-up/)). After the firmware checks the hardware, the firmware then scans each disk (or CD / floppy / USB / etc...) to check if they are bootable. Once the bios hits a bootable medium, it will load the first sector from that disk into the RAM address 0x7c00, then start executing that code. This first sector is referd to as the Boot Loader. The boot loader for MIDOS is a 2 staged bootloader, meaning that the first stage (which is located on the first sector of the disk) is only meant to load the second stage, and then switch execution to the second stage. The reason for this is because the bios loads the first sector of the disk, but each sector is only 512 bytes. Because each sector is only 512 bytes, that means the stage 1 boot loader can only be 512 bytes. As you could imagine, 512 bytes is very small. To get around this size limitation, all we have Stage 1 do is load Stage 2 (stage 2 being much larger than 512 bytes). Stage 2 takes care of setting up the hardware, and loading the kernel.  

# Stage 1:

### Creating a bootable disk image:

In order to make a disk bootable, a specific magic value is needed at the end of the first sector of the disk. When the bios recognizes this magic number, it will load the first sector off that disk in to RAM (the first sector being the stage 1 bootoader), then switch execution from the bios to the stage 1 bootloader. Below shows a very basic bootloader that clears the interrupts and halts the cpu.

```
bits	16 				; 16 bit real mode	

org 	0x7c00 				; we are loaded by bios to 0x7c00	

cli 					; clear interrupts
hlt 					; halt cpu

times 510 - ($-$$) db 0  		; pad the rest of the sector with 0
dw  	0xaa55 				; magic number 
``` 

The 0xaa55 is the magic number that the bios will recognize. The command to compile this is shown below.

```
nasm -f bin Stage1.asm -o Stage1.bin
```

The Image below shows the output in machine code.

![bootloaderbin](/Images/bootloaderbin.png)

As you can see, the "cli" and "hlt" have been compiled to its corresponding machine codes. The ``` times 510 - ($-$$) db 0 ``` has padded the rest of the sector with 0. Finally, the magic number is located at the end of the sector.  

Now that we have this bin, we need to put this on the first sector of a disk. To do this, I used the dd command to write this bin to the first sector of a virutal floppy disk image. Below shows the dd command to do this. "a.dmg" is the virtual floppy disk image.

```
dd if=Stage1.bin of=a.dmg conv=notrunc;
```
When booting from this virtual floppy disk in Bochs, the x86 emulator successfully identifys this drive as bootable. Apon execution of the bootloader, the emulator is halted immediately by the "cli" and "hlt" instructions. As you could imagine, this is a pretty terrible bootloader, as it isn't loading anything at all. 

### Creating the bootloader:

The stage 1 bootloader is very basic. All it does is parse the disk to load the stage 2 bootloader. After loading stage 2, it will switch execution to stage 2. In order to store stage 2 on a disk, the disk must be formatted with a specific file system. For my operating system, I decided to use the FAT12 file system. I do plan on creating my own file system later on, but for now I'm just using the FAT12 standard. Since I'm on a mac, I used the hdiutil to create a FAT12 formatted floppy disk. The commands below shows how to create a virtul floppy image formated with the FAT12 file system.

```
mkdir MiDOS
hdiutil create -size 1440k -fs "MS-DOS FAT12" -layout NONE -srcfolder MiDOS -format UDRW -ov .././disks/a.dmg
rm -r MiDOS
``` 
Now that I had this FAT12 virtual floppy disk, all I needed to do was copy the stage 2 bins into the root of this disk image. Because I haven't actually created stage 2 yet, I made a stage 2 stub. This stub was just used to test the functionality of the stage 1 bootloader. The code below shows the stub for stage 2.

```
org 0x0500		; we are loaded at linear address 0x500
 
bits 	16		; we are still in real mode

cli
hlt
```

The section that explains the code for how stage 1 parses the FAT12 formated disk is [here](../FS/FAT12.md).


# Stage 2: