# RiveraOS

An Operating System designed by Wave to decide the next Windows.

### Q. How to download it?
A. Go to the Releases and download `master.zip`. Then go inside it and do:

```sh
make              # build the kernel (requires i686-elf-gcc + nasm)
make disk         # create a 64 MiB persistent disk image (first time only)
make run          # launch in QEMU
```

Or run without a disk image (no file storage):

```sh
make run-no-disk
```

### Q. What do I need to build it?
A. You need:

| Tool | Where to get it |
|------|----------------|
| `i686-elf-gcc` | https://github.com/lordmilko/i686-elf-tools — cross-compiler for bare-metal x86 |
| `nasm` | https://nasm.us — Netwide Assembler |
| `make` | https://gnuwin32.sourceforge.net/packages/make.htm — GNU Make (Windows) or `apt install build-essential` (Linux) |
| `qemu-system-i386` | https://www.qemu.org — full-system emulator |
| `xorriso` (optional) | https://www.gnu.org/software/xorriso — needed only if you want ISO boot instead of `-kernel` |

**Windows users**: After installing the cross-compiler, add its `bin/` folder to your `PATH` so `i686-elf-gcc` is available in PowerShell/CMD.

### Q. What can it do so far?
A. Phase 1 features:

- **Boot**: GRUB Multiboot v1, 32-bit protected mode, GDT/IDT/PIC
- **Display**: VGA text mode 80×25 with colour, `printf` support
- **Keyboard**: PS/2 US QWERTY, Shift/Ctrl/CapsLock, arrow keys, Home/End
- **Storage**: ATA PIO (primary master, LBA28), 64 MiB persistent disk
- **Filesystem**: RiverFS v2 with directories (`ls`, `mkdir`, `cd`, `pwd`)
- **Shell**: Interactive command line with command history (Up/Down arrows), custom prompt `$ [/path/] ?`
- **POST screen**: Styled hardware-inventory display on boot

Commands: `help`, `echo`, `clear`, `ls`, `mkdir`, `cd`, `pwd`, `cat`, `write`, `append`, `rm`, `info`, `reboot`, `shutdown`

### Q. How do I use the persistent storage?
A. The file `persist.img` is a raw 64 MiB disk. Files and directories you create are saved there across reboots. Attach it with:

```sh
qemu-system-i386 -kernel build/kernel.elf -drive file=persist.img,format=raw,if=ide
```

(Note: use `-drive file=...,format=raw` — do **not** use `-hda` on QEMU 10.x, it blocks writes past block 0.)

### Q. How do I inspect the disk image from Windows?
A. Use the included Python tool:

```sh
python scripts/riverfs_tool.py persist.img ls
python scripts/riverfs_tool.py persist.img cat myfile
python scripts/riverfs_tool.py persist.img extract myfile
```

### Q. Can I contribute?
A. This is a learning OS — fork it, experiment, break things, fix them. Pull requests welcome.
