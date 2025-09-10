# Addition and print

Given the c code to sum 2 variables and print them

```c
#include<stdio.h>

int main(void){
	int x = 1;
	int y = 2;
	int z = x+y;
	printf("%d \n",z);
	return 0;
}
```

The core of the assembly is

```asm
	movl	$1, -12(%rbp)
	movl	$2, -8(%rbp)
	movl	-12(%rbp), %edx
	movl	-8(%rbp), %eax
	addl	%edx, %eax
	movl	%eax, -4(%rbp)
	movl	-4(%rbp), %eax
	movl	%eax, %esi
	leaq	.LC0(%rip), %rax
	movq	%rax, %rdi
	movl	$0, %eax
	call	printf@PLT
	movl	$0, %eax
```
The first part excutes the addition as previously seen. Then the result is then again moved to the register. Then with `movl   %eax, %esi`we pass from 32 bit register to 64 bit register. That is also the register needed for the second argument of functions.
Then `leaq    .LC0(%rip), %rax` takes the next instruction (`%rip`), which is stored prevously in the assembly code and is `.string	"%d \n"` and writes it to the register `%rax`. So basically it writed `"%d \n"` in register.
Then the string is put in the register `%rdi` which is the register for the first argument of functions. It uses movq since its 64 bits and not 32(that use movl).
The `%eax` is cleared to zero, that is needed by the `printf` function.
Then with `call    printf@PLT` the function `printf` gets called from the Procedure Linkage Table where the library `stdio` is. The arguments are found because they where moved to the correct registers.
