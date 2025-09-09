# Addition

This C code that defines 2 int variables, sums them and then returns:

```c
int main(){
	int x = 1;
	int y = 2;
	int z = x+y;
	return 0;
}
```

Becomes (in the core of the assembly file)

```asm
	movl	$1, -12(%rbp)
	movl	$2, -8(%rbp)
	movl	-12(%rbp), %edx
	movl	-8(%rbp), %eax
	addl	%edx, %eax
	movl	%eax, -4(%rbp)
	movl	$0, %eax
```

Basically stores the 2 variables in memory(-12, -8, -4 are positions on the stack), moves them to the registers, sums, moves output from register to memory, writes the return value (0) in the registry dedicated to return value.

If we use optimization -O3, the core of the assembly shrinks down to:

```asm
xorl	%eax, %eax
```

It recognizes that the variables are actually useless, therefore it just uses XOR between %eax registry and itself, turning it into 0 (this is faster than writing 0 on it).

