# Integer Overflows

|data type|size (bytes)| unsigned range  |        signed range       |
|:-------:|:----------:|:---------------:|:-------------------------:|
|   char  |      1     |     0 to 255    |         -128 to 127       |
|  short  |      2     |    0 to 65535   |       -32768 to 32767     |
|  int    |      4     | 0 to 429497296  | -2147483648 to 2147483647 |

```
char password_buf[11];
unsigned char password_len = strlen(password);
if (password_len > 4 && password_len < 10)
{
    strcpy(passwd_buf, password);
}

```
in this case if we provide a password with the size 256 to overflow the int past the max unsigned char size of 255
and add 4 so that the password length is within the length check.


# Off-By-One
This occurs when the programmer forgets about the need for a null byte, if you write exactly the size of the buffer with data we may be able to overwrite the least signification bit of EBP with null  which when returned will alter the control flow. if the address is on the stack for example EBP of another function overwriting the LSB with null may force it to be pointed at data that we can control which will in turn allow us to overwrite EIP.

The ability to overwrite EBP is possible when the callers EBP is before any other local variables on the stack
e.g code such as below will not allow us to overwrite the LSB of EBP.
```
var bad (char *arg)
{
int x = 10; //in the way local char
char buf[256];
strcpy(buf, arg);
}

```

for this case to work, the stack protector (-fno-stack-protector) should be off and the stack boundary set to 2 (-mpreferred-stack-boundary=2)

# ret2libc
with aslr off:
1) open binary print the address of functions you want to use while the app is running e.g. "p system"
2) use find in peda to locate the address of any strings you need e.g. "find sh"
3) structure payload as follows, like a fake stack frame: [libc address] [EBP for returning] [args to libc call]
