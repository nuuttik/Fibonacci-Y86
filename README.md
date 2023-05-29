# Fibonacci Y86

_Y86 algorithm that checks if a given number is Fibonacci number._

### What is Y86

Y86 is a processor architecture described in Chapter 4 of _Computer Systems: A Programmer’s Perspective, Second Edition_.

More specifically this program uses the Y86-64 architecture and you can run it for example in Y86-64 [web simulator](https://boginw.github.io/js-y86-64/).

### Description

- The program checks every number in array starting at memory address 0x700.
	- The array always ends in 0.
- The program immediately stops execution when it finds a number in the array that is not a Fibonacci number. This number is returned in %rax register.
- If all the numbers are Fibonacci numbers then the programs returns 0 in %rax register.

You can change the number array at ".pos 0" section of the program. By default the numbers are 3, 5, 8 and 0.

### Algorithm efficiency

- With number array 3, 5, 8, 0 the program takes 3339 instructions to complete.
- When you call isFibonacci function with number 8 it takes 948 instructions to complete.

isFibonacci is Y86 implementation of this C algorithm that checks whether a number is Fibonacci number:
```c
bool isPerfectSquare(int x) {
    int s = sqrt(x);
    return (s*s == x);
}
  
// Returns true if n is a Fibonacci Number, else false
bool isFibonacci(int x) {
    return isPerfectSquare(5*x*x + 4) || isPerfectSquare(5*x*x - 4);
}
```

You can calculate the number of instructions in the Y86-64 [web simulator](https://boginw.github.io/js-y86-64/) with this script:
```js
function countInstructions() {
  document.querySelector('#app > div > div.header > div.actions > button.compile').click();
  const old_s = STEP;
  const old_p = PAUSE;
  let count = 0;
  STEP = () => {
    count++;
    return old_s.call(this);
  }
  PAUSE = () => {
    console.log(`The program took ${count} instructions to complete.`);
    STEP = old_s;
    PAUSE = old_p;
    return old_p.call(this);
  }
  RUN();
}
countInstructions();
```