## Bitwise operators
You are most likely familiar with the basic arithmetic operators such as `+`, `-`, `*`, and `/`. However, there is another class of operators known as *bitwise* operators which are especially useful in embedded programing.

First a brief reminder of how numbers are actually represented inside a computer:
- Numbers are represented in base 2 notation, also known as binary. This means they are made up of only 1's and 0's like the example below:
```c
0011 = 3
```
- Learn more about binary [here](https://www.w3schools.com/programming/prog_binary_numbers.php)

#### What do bitwise operators do?
Bitwise operators allow us to perform logical operations directly on the binary digits of numbers. They work by aligning the binary representation of numbers and compare each position to produce the result. 

**Bitwise AND (&)**:
The AND (`&`) operator compares two numbers bit by bit. It returns 1 if both bits in that position are 1, otherwise it returns 0. Consider the example shown below:
```c 
  0001
& 0011
= 0001 
```

**Bitwise OR (|)**:
The OR (`|`) operator compares each bit of two numbers. It returns 1 if eat least one of the bits is 1. Consider the following example:
```c
  0001
| 0011
= 0011
```

**Bitwise NOT (!)**:
The NOT (`~`) operator flips every bit of a number. A 1 becomes a 0, and a 0 becomes a 1. Consider the following example:
```c
~ 0011
= 1100
```

**Bitwise XOR (^)**:
The XOR (`^`) operator, short for exclusive or, returns 1 only if the bits are different from eachother. Consider the following example:
```c
  0001
^ 0011
= 0010
```

## Logical Shift Operators
Another commonly used group of bitwise operators is the *logical shift operators*. As the name suggests, these operators allow you to move the bits left or right within a number.

**Left shift (<<)**:
The left shift operator (`<<`) shifts all of the bits to the left by a given number of positions. Zeros are filled in from the right side. Consider the following example:
```c 
  0011 << 2
= 1100
```

**Right shift (<<)**:
The right shift operator (`>>`) does the same thing as left shift but in the opposite direction. It shifts all of the bits to the right by a given number of positions and zeros are filled in from the left side. Consider the following example:
```c
  1100 >> 2
= 0011
```


> [!NOTE]
> Fun fact: Shifting an **unsigned** number left is the equivalent of multiplying it by powers of 2. Shifting right is like dividing by powers of 2 (and rounding down).

## Masking
Now that we've learned about how bitwise operators work, how can we use them in embedded programming?

One commonly used technique is called *masking*. It's used when we want to manipulate only specific bits within a number (like when configuring a GPIO register).

**Setting a bit**:
Let's say we have an 8-bit value, and want to set the 7th bit (from the right) to a 1:
```c 
value = 00001010;
mask  = 01000000;

result = value | mask;
// result is 01001010
```
- The OR operator sets only the bits where the mask has 1s, and leaves the others unchanged. This technique works on any number.

**Clearing a bit**:
Now let's say we want to set set the 4th bit to a 0:
```c 
value = 01001010;
mask  = 11110111;

result = value & mask;
// result is 01000010
```
- The AND operator clears the bit where the masks has a 0, and leaves the others unchanged.

**Toggling a bit**:
To toggle a bit (changing a 1 to 0, and 0 to 1), use the XOR operator with a mask that has a 1 at the bit we want to flip. For example, to flip the 3rd bit:
```c 
value = 01000010;
mask  = 00000100;

result= value ^ mask;
// result is 01000110
```

**Checking a bit**:
To check if a specific bit is 1 or 0, use the AND operator with a mask that has a 1 set at the bit you want to test. For example, to check the 7th bit:
```c 
value = 01000110;
mask  = 01000000;

result = value & mask;
// result is 01000000

// Bonus! Since result is non-zero, it can be used in an if statement
if (result) {
	// do something
}
```

> [!NOTE]
> While the examples show manipulation of a single bit, these techniques can be applied to multiple bits too!

## Creating masks with shifting
Instead of writing binary masks by hand, we can build them using shift operators. This makes our code much easier to read and edit.

For example, to set the 5th bit to a 1:
```c 
value = 00000000;
mask = 1 << 5; // mask becomes 00100000
result = value | mask;
// result is 00100000
```

To clear the 5th bit:
```c 
value = 11111111;
mask = ~(1 << 5); // mask becomes 11011111
result = value & mask;
// result is 11011111
```

In general, use `1 << n` to target the n-th bit and combine it with bitwise operators to manipulate the bits.

> [!NOTE]
> Can you think of a way to create a multi-bit mask?