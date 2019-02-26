---
layout: post
title: Gnu provided Gcc extensions in C
subtitle: 
tags: [C, gcc]
---

### Content
1. What is Metaprogramming ?
2. What is generic Programming ?
3. Decorating function using macros
4. '#' and '##' operator in C
5. Variadic
6. Variadic macro
7. Alternative to ## operator
8. Generic

### What is Metaprogramming ?
It means programs that manipulate programs or Metaprogramming refers to a variety of ways a program has knowledge of itself or can manipulate itself. Example:- Bison, Flex. [Ref](https://stackoverflow.com/questions/514644/what-exactly-is-metaprogramming)

### What is generic Programming ?
Generic programming is a style of computer programming in which algorithms are written in terms of types to-be-specified-later that are then instantiated when needed for specific types provided as parameters. [Ref](https://en.wikipedia.org/wiki/Generic_programming)

### Decorating function using macros

```c
#include <stdio.h>
#include <time.h> 

#define wrapper(fn) \
	({	clock_t t; \
		t=clock(); \
		fn; \
		t=clock() - t; \
		double time_taken = ((double)t)/CLOCKS_PER_SEC; \
		printf("It took %f seconds to execute \n", time_taken);\
		time_taken; \
	})
void apoorva(int a,int b, int c){
	int d= a+b+c;
	printf("%d\n",d);
	// return;
}
int main(){
	double x=wrapper(apoorva(1,2,3));
	printf("%f\n",x);
}
```
This ends my intellectual struggle to get a programming pattern close to python's decorator in C. I am pretty satisfied with this pattern.

### # and ## operator in C

```c
#include <stdio.h> 
#define str(s) #s 
int main(void) 
{ 
    printf(str(Apoorva-Kumar\n)); 

}
```

```c
#include <stdio.h> 
#define concat(a) a##a##a 
int main(void) 
{ 
	int xy = 1302;
  	int xx=123;
  	int xxx=123456;
	printf("%d", concat(x)); 
	return 0; 
}
```
##### What is # and ## operator is doing ?  
Hash operator in C is converts to string. And Double hash operator -> it merges tokens which is passed to the defined macro, Such as in previous example it changes it to 'xxx'.  

### Variadic

```c
#include <stdarg.h>
#include <stdio.h>

int add_em_up (int count,...)
{
  va_list ap;
  int i, sum;
  va_start (ap, count);         /* Initialize the argument list. */
  sum = 0;
  for (i = 0; i < count; i++)
    sum += va_arg (ap, int);    /* Get the next argument value. */
  va_end (ap);                  /* Clean up. */
  return sum;
}

int main (void)
{
  printf ("%d\n", add_em_up (3, 5, 5, 6));
  printf ("%d\n", add_em_up (10, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));

  return 0;
}
```
Above example is taken from [gnu](http://www.gnu.org/software/libc/manual/html_node/Variadic-Example.html).   
Variadic helps in taking multiple arguments, you can also take arguments of different type.

```c
#include <stdlib.h>
#include <stdarg.h>
#include <stdio.h>

void foo(char *fmt, ...)
{
   va_list ap;
   int d;
   char c, *s;

   va_start(ap, fmt);

   while (*fmt) {
       switch (*(fmt++)) {
       case 's':
           s = va_arg(ap, char *);
           printf("string %s\n", s);
           break;
       case 'd':
           d = va_arg(ap, int);
           printf("int %d\n", d);
           break;
       case 'c':
           /* need a cast here since va_arg only
              takes fully promoted types */
           c = (char) va_arg(ap, int);
           printf("char %c\n", c);
           break;
       }
   }

   va_end(ap);
}


int main(void)
{
    char *s1 = "First";
    char *s2 = "Second";
    int   d = 42;
    char  c = '?';
    foo("sdcs", s1, d, c, s2);
    return EXIT_SUCCESS;
}
```
Above code is picked from stack overflow. This uses a neat trick to know which data-type is coming next. So the string "sdcs" encodes data of datatype being passed.

### Variadic Macro

```c
#include <stdio.h>

#define log_info(M, ...) fprintf(stderr, "[INFO] (%s:%d) " M "\n",__FILE__, __LINE__, ##__VA_ARGS__)

int main(int argc, char *argv[])
{
	log_info("Start of program");
	int var = 10;

	log_info("my variable is %d", var);

	log_info("End of program");
}
```
In the above example to log_info. 

### Alternative to ## operator

```c
#include <stdio.h>
#define A() 1
#define CAT(a, ...) PRIMITIVE_CAT(a, __VA_ARGS__)
#define PRIMITIVE_CAT(a, ...) a ## __VA_ARGS__
#define IIF(c) PRIMITIVE_CAT(IIF_, c)
#define IIF_0(t, ...) __VA_ARGS__
#define IIF_1(t, ...) t

int main(){
	int x=IIF(1)(2, 3);
	int z=IIF(A())(4,5);
	printf("%d,%d\n",x,z);
}
```

A subtle side effect of the ## operator is that it inhibits expansion. 
```c
#include <stdio.h>
#define IIF(cond) IIF_ ## cond
#define IIF_0(t, f) f
#define IIF_1(t, f) t
#define A() 1
//This correctly expands to true
int main(){
	IIF(1)(true, false) 
	// This will however expand to IIF_A()(true, false)
	// This is because A() doesn't expand to 1,
	// because its inhibited by the ## operator
	IIF(A())(true, false) 
}
```
[Ref](https://stackoverflow.com/questions/11632219/c-preprocessor-macro-specialisation-based-on-an-argument)  
[More-Depth](https://github.com/pfultz2/Cloak/wiki/C-Preprocessor-tricks,-tips,-and-idioms)

### Generic
```c
#include <stdio.h>

#define log_info(M, ...) fprintf(stderr, "[INFO] (%s:%d) " M "\n",__FILE__, __LINE__, ##__VA_ARGS__)
#define type_idx(T) _Generic( (T), char: "char", int: "int", long: "long", default: "zero")

#define printf_dec_format(x) _Generic((x), \
    char: "%c", \
    signed char: "%hhd", \
    unsigned char: "%hhu", \
    signed short: "%hd", \
    unsigned short: "%hu", \
    signed int: "%d", \
    unsigned int: "%u", \
    long int: "%ld", \
    unsigned long int: "%lu", \
    long long int: "%lld", \
    unsigned long long int: "%llu", \
    float: "%f", \
    double: "%f", \
    long double: "%Lf", \
    char *: "%s", \
    void *: "%p")
 
#define print(x) printf(printf_dec_format(x), x)
#define printnl(x) printf(printf_dec_format(x), x), printf("\n");

int main(int argc, char *argv[])
{
	printf("%s\n",type_idx(1));
	printnl('a');
	printnl((char)'a');
	printnl(1);
}
// #define acos(X) _Generic((X), \
//     long double complex: cacosl, \
//     double complex: cacos, \
//     float complex: cacosf, \
//     long double: acosl, \
//     float: acosf, \
//     default: acos \
//     )(X)
// This is important method to typecast and it might help sometimes later.
```
It acts as a switch and acts accordingly. [Ref](http://www.robertgamble.net/2012/01/c11-generic-selections.html)

### Other References
[Blog of founder of libmill](http://250bpm.com/blog:56)
#### What is libmill ?
Libmill is a library that introduces Go-style concurrency to C.

```c
go(foo(arg1, arg2, arg3));
chan ch = chmake(int, 0);
chan ch = chmake(int, 1000);
chs(ch, int, 42);
int i = chr(ch, int);
chdone(ch, int, 0);
chclose(ch);
choose {
	out(ch, int, 42):
		foo();
	in(ch, int, i):
		bar(i);
	otherwise:
		baz();
	end
}
```
He has used macros in the most fascinating and brilliant ways.
### That's all folks.