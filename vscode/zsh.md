[dev post](https://dev.to/zinox9/installing-zsh-on-windows-37em)

## Cygwin

Think of it as an OS. It provides a POSIX C runtime built on top of Windows so you can compile most Unix software to run on top of it. It comes with GCC, and to some extent, you can call the Win32 API from within Cygwin, although I'm not sure that is meant to happen or work at all.

## apt-cyg
apt-cyg is a command-line installer for Cygwin which cooperates with Cygwin Setup and uses the same repository. The syntax is similar to apt-get. Usage examples:

    "apt-cyg install " to install packages
    "apt-cyg remove " to remove packages
    "apt-cyg update" to update setup.ini
    "apt-cyg show" to show installed packages
    "apt-cyg find " to find packages matching patterns
    "apt-cyg describe " to describe packages matching patterns
    "apt-cyg packageof " to locate parent packages

## gdb
A debugger is a program that runs other programs, allowing the user to exercise control over these programs, and to examine variables when problems arise.

GNU Debugger, which is also called gdb, is the most popular debugger for UNIX systems to debug C and C++ programs.

GNU Debugger helps you in getting information about the following:

- If a core dump happened, then what statement or expression did the program crash on?

- If an error occurs while executing a function, what line of the program contains the call to that function, and what are the parameters?

- What are the values of program variables at a particular point during execution of the program?

- What is the result of a particular expression in a program?

## dos2unix – Removing Hidden Windows Characters from Files
While editing files on a machine running some form of Windows and uploading them to a Linux server is convenient, it can cause unforeseen complications. Windows-based text editors put special characters at the end of lines to denote a line return or newline. Normally harmless, some applications on a Linux server cannot understand these characters and can cause the service to not respond correctly. There is a simple way to correct this problem: dos2unix.

## OpenSSH
Short for Open Secure Shell, OpenSSH is a free suite of tools (similar to the SSH connectivity tools) that help secure your network connections. OpenSSH encrypts all traffic (including passwords) to effectively eliminate eavesdropping, connection hijacking and other network-level attacks. 

