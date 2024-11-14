# Vault Door 3

This vault uses for-loops and byte arrays. The source code for this vault is here:

```java
import java.util.*;

class VaultDoor3 {
    public static void main(String args[]) {
        VaultDoor3 vaultDoor = new VaultDoor3();
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter vault password: ");
        String userInput = scanner.next();
	String input = userInput.substring("picoCTF{".length(),userInput.length()-1);
	if (vaultDoor.checkPassword(input)) {
	    System.out.println("Access granted.");
	} else {
	    System.out.println("Access denied!");
        }
    }

    // Our security monitoring team has noticed some intrusions on some of the
    // less secure doors. Dr. Evil has asked me specifically to build a stronger
    // vault door to protect his Doomsday plans. I just *know* this door will
    // keep all of those nosy agents out of our business. Mwa ha!
    //
    // -Minion #2671
    public boolean checkPassword(String password) {
        if (password.length() != 32) {
            return false;
        }
        char[] buffer = new char[32];
        int i;
        for (i=0; i<8; i++) {
            buffer[i] = password.charAt(i);
        }
        for (; i<16; i++) {
            buffer[i] = password.charAt(23-i);
        }
        for (; i<32; i+=2) {
            buffer[i] = password.charAt(46-i);
        }
        for (i=31; i>=17; i-=2) {
            buffer[i] = password.charAt(i);
        }
        String s = new String(buffer);
        return s.equals("jU5t_a_sna_3lpm18g947_u_4_m9r54f");
    }
}
```
**FLAG:- picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_79958f}**

## My Approach to the challenge:-

The code given in the challenge anagrams the user input (within picoCTF{}) and then checks it with a specific harcoded string.

I used the hardcoded string and wrote the following code in java to print the flag.

``` java
import java.util.*;

class VaultDoor3 {
    public static void main(String args[]) {     
  
        String s="jU5t_a_sna_3lpm18g947_u_4_m9r54f";

        int i;
        char passwd[]=new char[32]; // given that the length of password is 32
        
        for(i=0;i<8;i++)
        {
            passwd[i]=s.charAt(i);
        }
        for(i=8;i<16;i++)
        {
            passwd[i]=s.charAt(23-i);
        }
        for (i=16; i<32; i+=2) {
            passwd[i]=s.charAt(46-i);
        }
        for (i=31; i>=17; i-=2) {
            passwd[i]=s.charAt(i);
        }

        String concatpasswd =new String(passwd);

        System.out.println("picoCTF{"+concatpasswd+"}");


    }
}

```
The above program does the reverse of the anagram and prints the flag in picoCTf flag format.

## Learning outcomes:-

1. What an anagram?

   > a word, phrase, or name formed by rearranging the letters of another
