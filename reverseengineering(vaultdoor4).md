# Vault Door 4

This vault uses ASCII encoding for the password. The source code for this vault is here:
```java
import java.util.*;

class VaultDoor4 {
    public static void main(String args[]) {
        VaultDoor4 vaultDoor = new VaultDoor4();
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

   
    public boolean checkPassword(String password) {
        byte[] passBytes = password.getBytes();
        byte[] myBytes = {
            106 , 85  , 53  , 116 , 95  , 52  , 95  , 98  ,
            0x55, 0x6e, 0x43, 0x68, 0x5f, 0x30, 0x66, 0x5f,
            0142, 0131, 0164, 063 , 0163, 0137, 0146, 064 ,
            'a' , '8' , 'c' , 'd' , '8' , 'f' , '7' , 'e' ,
        };
        for (int i=0; i<32; i++) {
            if (passBytes[i] != myBytes[i]) {
                return false;
            }
        }
        return true;
    }
}
```

**FLAG:- picoCTF{jU5t_4_bUnCh_0f_bYt3s_f4a8cd8f7e}**

## My Approach to the challenge:-

The code given in the challenge takes the user input, gets the bytes of each character and saves them in a byte array. Thus the byte array consists of all the ascii values of the characters in input string.

Now it checks the byte values of the input string with another hardcoded byte array (`myBytes`).

The myBytes array consists of 32 items of which
1. First 8 are represented in ascii.
2. Second 8 in Hexadecimal
3. Third 8 in Octal
4. rest 8 in character/ alphabet form

Now the byte values of all these items is stored in the myBytes byte array.

Thus , I used the byte array and wrote the following program in java to print the flag.

```java
import java.util.*;

class VaultDoor4 {
    public static void main(String args[]) {   
        byte[] myBytes = {
            106 , 85  , 53  , 116 , 95  , 52  , 95  , 98  ,
            0x55, 0x6e, 0x43, 0x68, 0x5f, 0x30, 0x66, 0x5f,
            0142, 0131, 0164, 063 , 0163, 0137, 0146, 064 ,
            'a' , '8' , 'c' , 'd' , '8' , 'f' , '7' , 'e' ,
        };        

        System.out.print("picoCTF{");
        for(int i=-0;i<32;i++)
        { 
            char c;
            c=(char)myBytes[i];
            System.out.print(c);
        }
        System.out.print("}");
    }
}

```

My program converts the byte values stored in the byte array to characters then prints the output in flag format.

## Learning Outcomes:-

1.  Octal numbers are represented by appending a zero in front of a number in java.
2.  getBytes function is used to get the byte values of all the characters present in the string.
