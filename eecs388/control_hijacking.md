# Control Hijacking

**Binary Exploitation**: the process of subverting a compiled application in order to violate some trust boundary in a way that helps an attacker

## Buffer Overflow
- some C functions like `strncpy()` can write arbitrary data to the stack in undesired ways if the sizes of variables used are wrong
```c
int foo(char *string, int length)
{
    char buffer[100];
    strncpy(buffer, string, length);
    return 1;
}
```
- if string is longer than 100 bytes, then `strncpy()` will write data to the stack past the end of `buffer`
- this will result in stack variables being overwritten, which can change how the rest of the program will execute

## Data Execution Prevention (DEP)

## Return To libc
- DEP prevents you from executing malicious code in the stack
- instead, just call a library function from the stack which does what you want
- call the function by "returning" to it
- use buffer overflow to manipulate stack so the desired function is returned to and so it has the desired arguments
- for example, a function like `execv("/bin/evil_program", args)` can be used to execute an arbitrary executable file on the computer with arbitrary arguments
- you can also call a function like `mprotect()` which changes executable permissions and disables DEP, meaning we can just execute code from the stack

## Extraneous Function Removal
- simple defense to return to libc, just get rid of functions that aren't used by the program, so an attacker can't call them

## Return Oriented Programming (ROP)
- 