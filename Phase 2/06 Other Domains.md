# Domain: Cryptography

## Mod 26

**Flag:** `picoCTF{next_time_I'll_try_2_rounds_of_rot13_ulYvpVag}`

We are given the flag as `cvpbPGS{arkg_gvzr_V'yy_gel_2_ebhaqf_bs_ebg13_hyLicInt}`. This is a simple `ROT-13` cipher, a special case of the Caesar cipher in which the letters are shifted by 13 positions.
Putting this into any online `ROT-13` decoder (I used *https://rot13.com/*), we get `picoCTF{next_time_I'll_try_2_rounds_of_rot13_ulYvpVag}`.

## The Numbers

**Flag:** `PICOCTF{THENUMBERSMASON}`

We're given an image with a list of numbers along with opening and closing brackets.

![The Numbers](../Images/the_numbers.png)

It seems to be the flag with the letters encoded as their positions in the alphabet. We write some simple C code to decode it

```
#include <stdio.h>

int main()
{
	int numbers[] = {
		16, 9, 3, 15, 3,  20, 6, // The part before the brackets
		20, 8, 5, 14, 21, 13, 2, 5, 18, 19, 13, 1, 19, 15, 14 // The part before the brackets
	};

	for (int i = 0; i < 22; i++) {
		printf("%c", numbers[i] + 65);
	}

	return 0;
}
```

Compiling and running it, we get
```
~/Projects $ gcc main.c -o main
~/Projects $ ./main
QJDPDUGUIFOVNCFSTNBTPO
~/Projects $
```

This seems to be a Caesar cipher. Using the solver on [dcode](https://www.dcode.fr/caesar-cipher) and decrypting it using brute-force, we get the answer as `PICOCTFTHENUMBERSMASON`.
