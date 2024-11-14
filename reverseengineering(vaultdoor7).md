# Vault Door 7
This vault uses bit shifts to convert a password string into an array of integers. Hurry, agent, we are running out of time to stop Dr. Evil's nefarious plans! The source code for this vault is here: 

```java
import java.util.*;
import javax.crypto.Cipher;
import javax.crypto.spec.SecretKeySpec;
import java.security.*;

class VaultDoor7 {
    public static void main(String args[]) {
        VaultDoor7 vaultDoor = new VaultDoor7();
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

    // Each character can be represented as a byte value using its
    // ASCII encoding. Each byte contains 8 bits, and an int contains
    // 32 bits, so we can "pack" 4 bytes into a single int. Here's an
    // example: if the hex string is "01ab", then those can be
    // represented as the bytes {0x30, 0x31, 0x61, 0x62}. When those
    // bytes are represented as binary, they are:
    //
    // 0x30: 00110000
    // 0x31: 00110001
    // 0x61: 01100001
    // 0x62: 01100010
    //
    // If we put those 4 binary numbers end to end, we end up with 32
    // bits that can be interpreted as an int.
    //
    // 00110000001100010110000101100010 -> 808542562
    //
    // Since 4 chars can be represented as 1 int, the 32 character password can
    // be represented as an array of 8 ints.
    //
    // - Minion #7816
    public int[] passwordToIntArray(String hex) {
        int[] x = new int[8];
        byte[] hexBytes = hex.getBytes();
        for (int i=0; i<8; i++) {
            x[i] = hexBytes[i*4]   << 24
                 | hexBytes[i*4+1] << 16
                 | hexBytes[i*4+2] << 8
                 | hexBytes[i*4+3];
        }
        return x;
    }

    public boolean checkPassword(String password) {
        if (password.length() != 32) {
            return false;
        }
        int[] x = passwordToIntArray(password);
        return x[0] == 1096770097
            && x[1] == 1952395366
            && x[2] == 1600270708
            && x[3] == 1601398833
            && x[4] == 1716808014
            && x[5] == 1734291511
            && x[6] == 960049251
            && x[7] == 1681089078;
    }
}
```

**FLAG:- picoCTF{A_b1t_0f_b1t_sh1fTiNg_07990cd3b6}**

## My Approach to solve the question

After reading the commented part of the given code. I understood what the code is doing. 

To reverse the encryption, I wrote down the following procedure. 

![image](https://github.com/user-attachments/assets/b08977c2-cffa-4215-83a1-e28a6cda28c2)

Thus to perform this, I wrote the following code in java to give the flag.

```java

class VaultDoor7{
    public static void main(String args[]) {
       int x[]={1096770097,1952395366,1600270708,1601398833,1716808014,1734291511,960049251,1681089078};

       int i;
       String s="";
       for(i=0;i<8;i++){
         String temp=Integer.toBinaryString(x[i]);
         while(temp.length()<32)
         {
            temp="0"+temp;
         }
         s+=temp;
        
       }
       System.out.println(s);
       int bytevaluesfrombinary[]=new int[s.length()/8];
       int length=s.length()/8;
       System.out.println(length);
    for(i=0;i<(s.length()/8);i++)
    {
        bytevaluesfrombinary[i]=Integer.parseInt((s.substring(i*8, i*8+8)),2);
        System.out.print(bytevaluesfrombinary[i]+" ");

    }
    System.out.println("\n");
    System.out.print("picoCTF{");
    for(i=0;i<s.length()/8;i++)
    {
        char c;
        int b;
        b=bytevaluesfrombinary[i];
        c=(char)b;
        System.out.print(c);

    }
    System.out.print("}");


   
}
}
```
This code first converts each long integer to a 32 bit binary number. From which I extracted the substrings of length 8 , converted them to their Integer values using `Integer.parseInt(string,2)` function.

Then I print the character values of the Integer array of byte values of the 8 bit binary numbers. This gives me the flag.
