# Vault Door 8

Apparently Dr. Evil's minions knew that our agency was making copies of their source code, because they intentionally sabotaged this source code in order to make it harder for our agents to analyze and crack into! The result is a quite mess, but I trust that my best special agent will find a way to solve it. The source code for this vault is here:
```java

import java.util.*;
import javax.crypto.Cipher;
import javax.crypto.spec.SecretKeySpec;
import java.security.*;
class VaultDoor8 {
	public static void main(String args[]) {
	Scanner b = new Scanner(System.in);
	System.out.print("Enter vault password: ");
	String c = b.next();
	String f = c.substring(8,c.length()-1);
	VaultDoor8 a = new VaultDoor8();
	if (a.checkPassword(f))
	{
		System.out.println("Access granted."); 
	}
	else {
	System.out.println("Access denied!");
	} 
	
	}
	public char[] scramble(String password)
	{/* Scramble a password by transposing pairs of bits. */
	char[] a = password.toCharArray();
	for (int b=0; b<a.length; b++)
	{	char c = a[b];
		c = switchBits(c,1,2);
		c = switchBits(c,0,3); 
		/* c = switchBits(c,14,3); 
		c = switchBits(c, 2, 0); */ 
		c = switchBits(c,5,6); 
		c = switchBits(c,4,7);
		c = switchBits(c,0,1); 
		/* d = switchBits(d, 4, 5); 
		e = switchBits(e, 5, 6); */ 
		c = switchBits(c,3,4); 
		c = switchBits(c,2,5); 
		c = switchBits(c,6,7); 
		a[b] = c; 
	} 
	
	return a;	
	} 
	
	public char switchBits(char c, int p1, int p2) {
	/* Move the bit in position p1 to position p2, and move the bit that was in position p2 to position p1. Precondition: p1 < p2 */ 
	char mask1 = (char)(1 << p1);
	char mask2 = (char)(1 << p2); 
	/* char mask3 = (char)(1<<p1<<p2); 
	mask1++; mask1--; */ 
	char bit1 = (char)(c & mask1); char bit2 = (char)(c & mask2); 
	/* System.out.println("bit1 " + Integer.toBinaryString(bit1));
	System.out.println("bit2 " + Integer.toBinaryString(bit2)); */ 
	char rest = (char)(c & ~(mask1 | mask2)); 
	char shift = (char)(p2 - p1); 
	char result = (char)((bit1<<shift) | (bit2>>shift) | rest); 
	return result;
	}
	
	public boolean checkPassword(String password){
		char[] scrambled = scramble(password); 
		char[] expected = {0xF4, 0xC0, 0x97, 0xF0, 0x77, 0x97, 0xC0, 0xE4, 0xF0, 0x77, 0xA4, 0xD0, 0xC5, 0x77, 0xF4, 0x86, 0xD0, 0xA5, 0x45, 0x96, 0x27, 0xB5, 0x77, 0xC2, 0xD2, 0x95, 0xA4, 0xF0, 0xD2, 0xD2, 0xC1, 0x95 }; 
		return Arrays.equals(scrambled, expected); 
		}
		
	 }
		
```
**FLAG: picoCTF{s0m3_m0r3_b1t_sh1fTiNg_89eb3994e}**
## My Approach to solve the question

I understood the code and found out that the actual encryption happens in the scramble method

`scramble` Method:
 > Transposes bits in each character of the password. For each character, it:
   > 1. Calls switchBits multiple times to swap bits in specific positions.
   > 2. Each call to switchBits swaps two bits and stores the result back into the character array.

Thus , I just reversed the process of scrambling and wrote the following code.

```java
import java.util.*;

class VaultDoor8 {
    public static void main(String[] args) {
        VaultDoor8 vault = new VaultDoor8();
        char[] expected = {0xF4, 0xC0, 0x97, 0xF0, 0x77, 0x97, 0xC0, 0xE4, 
                           0xF0, 0x77, 0xA4, 0xD0, 0xC5, 0x77, 0xF4, 0x86, 
                           0xD0, 0xA5, 0x45, 0x96, 0x27, 0xB5, 0x77, 0xC2, 
                           0xD2, 0x95, 0xA4, 0xF0, 0xD2, 0xD2, 0xC1, 0x95};
        
        char[] originalPassword = vault.reverseScramble(expected);

        System.out.print("picoCTF{");
        for (char c : originalPassword) {
            System.out.print(c);
        }
        System.out.println("}");
    }

    public char[] reverseScramble(char[] scrambled) {
        // Reversing the scrambling here
        char[] a = Arrays.copyOf(scrambled, scrambled.length);
        for (int i = 0; i < a.length; i++) {
            char c = a[i];
            c = switchBits(c, 6, 7);
            c = switchBits(c, 2, 5);
            c = switchBits(c, 3, 4);
            c = switchBits(c, 0, 1);
            c = switchBits(c, 4, 7);
            c = switchBits(c, 5, 6);
            c = switchBits(c, 0, 3);
            c = switchBits(c, 1, 2);
            a[i] = c;
        }
        return a;
    }

    public char switchBits(char c, int p1, int p2) {
        
        char mask1 = (char) (1 << p1);
        char mask2 = (char) (1 << p2);
        char bit1 = (char) (c & mask1);
        char bit2 = (char) (c & mask2);
        char rest = (char) (c & ~(mask1 | mask2));
        int shift = p2 - p1;
        return (char) ((bit1 << shift) | (bit2 >> shift) | rest);
    }
}
