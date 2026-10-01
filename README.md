# RiveraOS
RiveraOS, the FUTURE of Operating Systems as Windows slowly declines.

Prerequisites in case you wanna build yourself manually:

MSVC: Visual Studio Build Tools 2022

QEMU : www.qemu.org

xorriso: https://github.com/PeyTy/xorriso-exe-for-windows

rust (as of 0.0.4): https://rust-lang.org/tools/install/

rust-src (after getting rust): `rustup component add rust-src`

make: ```winget install ezwinports.make```

gnu-grub 2.12: https://ftp.gnu.org/gnu/grub/grub-2.12-for-windows.zip

i686-elf-gcc: https://github.com/lordmilko/i686-elf-tools

lld: ```winget install LLVM.LLVM```

Go to PowerShell 7 and type: `make clean && make disk && make apps && make all && make iso`
Then to run it: `make run-iso or make run-iso-serial`

First run RiveraOS Setup menuentry, then after installing and rebooting, select RiveraOS.

Thanks for you kind attention to this!

Note: As of 0.0.4, the hard coded paths problem has been fixed and before 0.0.4, you'd have to manually edit all of the hard coded paths!

Here is a preview:

<img width="812" height="618" alt="image" src="https://github.com/user-attachments/assets/dd93228b-fb62-4bf4-b295-22ea7ae54df0" />

