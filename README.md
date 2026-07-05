# RiveraOS

An Operating System designed by Wave (Adrish Maity, a.k.a. Average) to decide the next Windows.

### Q. How to download it?
A. Go to the Releases and download `master.zip`. Then go inside it and do:

```sh
make              # build the kernel (requires i686-elf-gcc + nasm)
make disk         # create a 64 MiB persistent disk image (first time only)
make run          # launch in QEMU with persistent storage
```

Or run without a disk image (no file storage):

```sh
make run-no-disk
```

### Q. What do I need to build it?
A.

| Tool | Where to get it |
|------|----------------|
| `i686-elf-gcc` | https://github.com/lordmilko/i686-elf-tools — cross-compiler for bare-metal x86 |
| `nasm` | https://nasm.us — Netwide Assembler |
| `make` | https://gnuwin32.sourceforge.net/packages/make.htm — GNU Make (Windows) or `apt install build-essential` (Linux) |
| `qemu-system-i386` | https://www.qemu.org — full-system emulator |

**Windows users**: After installing the cross-compiler, add its `bin/` folder to your `PATH` so `i686-elf-gcc` is available in PowerShell/CMD.

### Q. How do I build a bootable ISO?

**On Linux / WSL:**

```sh
# Install GRUB tools + xorriso
sudo apt install grub-pc-bin xorriso

# Build the ISO
make iso

# Boot it in QEMU
qemu-system-i386 -cdrom rivera.iso -drive file=persist.img,format=raw,if=ide
```

This runs `grub-mkimage` to create a GRUB core image with the `multiboot` and `normal` modules, then `xorriso` bundles it into a bootable ISO with El Torito no-emulation boot.

**On Windows (without Linux/WSL):**

`make iso` requires `grub-mkimage` and `xorriso`, which are Linux tools. On Windows, use QEMU's `-kernel` method instead:

```sh
make run
```

That boots the kernel directly with no ISO needed, which is faster for development.

### Q. What can it do so far?
A. Phase 1 features:

- **Boot**: GRUB Multiboot v1, 32-bit protected mode, GDT/IDT/PIC
- **POST screen**: Styled hardware-inventory display on boot — Press any key to continue
- **Display**: VGA text mode 80×25 with colour, `printf` support, **blinking hardware cursor**
- **Keyboard**: PS/2 US QWERTY, Shift/Ctrl/CapsLock, arrow keys (incl. extended 0xE0 prefix), Home, End
- **Shell**: Interactive command-line with **Left/Right arrow cursor navigation** (insert in middle of line), **Up/Down command history** (16 entries), **custom prompt** `$ [/path/] ?`
- **Storage**: ATA PIO (primary master, LBA28), 64 MiB persistent disk
- **Filesystem**: RiverFS v2 with directories (`ls`, `mkdir`, `cd`, `pwd`)

| Command | What it does |
|---------|-------------|
| `help` | Show all commands |
| `echo` | Print text (quote-aware) |
| `clear` | Clear screen |
| `ls [path/]` | List directory |
| `mkdir <path/` | Create directory |
| `cd <path>` | Change directory |
| `pwd` | Print current directory |
| `cat <path>` | Display file content |
| `write <path> <text>` | Write file |
| `append <path> <text>` | Append to file |
| `rm <path>` | Delete file |
| `info` | System information |
| `reboot` | Reboot the machine |
| `shutdown` | Halt the CPU |

### Q. How do I use the persistent storage?
A. The file `persist.img` is a raw 64 MiB disk. Files and directories you create are saved there across reboots. Attach it with:

```sh
qemu-system-i386 -kernel build/kernel.elf -drive file=persist.img,format=raw,if=ide
```

**IMPORTANT**: Use `-drive file=...,format=raw` — do **not** use `-hda` on QEMU 10.x. `-hda` auto-detects format and silently blocks writes past block 0.

### Q. How do I inspect the disk image from Windows?
A. Use the included Python tool:

```sh
python scripts/riverfs_tool.py persist.img ls
python scripts/riverfs_tool.py persist.img cat myfile
python scripts/riverfs_tool.py persist.img extract myfile
```

### Q. Can I contribute?
A. This is a learning OS — fork it, experiment, break things, fix them. Pull requests welcome.

---

Made with ❤️ by Adrish Maity (Average) — Class 7.
