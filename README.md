# RiveraOS
RiveraOS, the FUTURE of Operating Systems as Windows slowly declines.

Prerequisites in case you wanna build yourself manually:

make:
gnu-grub 2.12: ```sh winget install ezwinports.make```
i686-elf-gcc: https://github.com/lordmilko/i686-elf-tools
lld: ```sh winget install LLVM.LLVM```

Go to PowerShell 7 and type: `make clean && make disk && make apps && make all && make iso`
Then to run it: `make run-iso or make run-iso-serial`

First run RiveraOS Setup menuentry, then after installing and rebooting, select RiveraOS.

Thanks for you kind attention to this!
