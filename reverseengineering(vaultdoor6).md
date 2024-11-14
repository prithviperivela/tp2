# Vault Door 6

This vault uses an XOR encryption scheme. The source code for this vault is here: 

```java
import java.util.*;

class VaultDoor6 {
    public static void main(String args[]) {
        VaultDoor6 vaultDoor = new VaultDoor6();
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
        if (password.length() != 32) {
            return false;
        }
        byte[] passBytes = password.getBytes();
        byte[] myBytes = {
            0x3b, 0x65, 0x21, 0xa , 0x38, 0x0 , 0x36, 0x1d,
            0xa , 0x3d, 0x61, 0x27, 0x11, 0x66, 0x27, 0xa ,
            0x21, 0x1d, 0x61, 0x3b, 0xa , 0x2d, 0x65, 0x27,
            0xa , 0x6c, 0x61, 0x6d, 0x37, 0x6d, 0x6d, 0x6d,
        };
        for (int i=0; i<32; i++) {
            if (((passBytes[i] ^ 0x55) - myBytes[i]) != 0) {
                return false;
            }
        }
        return true;
    }
}
```

**FLAG:- picoCTF{n0t_mUcH_h4rD3r_tH4n_x0r_948b888}**

## My Approach to solve this question

The given challenge takes user input, gets the byte values of each character of its string then xors it with 0x55. It should be equal to myBytes. Only then does it authenticate the user.

Since it is XOR operation, performing the XOR again on myBytes gives the previous value. I wrote the following code which does that and prints it in the flag format.

```java
import java.util.*;

class VaultDoor6 {
    public static void main(String args[]) {
        byte[] myBytes = {
            0x3b, 0x65, 0x21, 0xa , 0x38, 0x0 , 0x36, 0x1d,
            0xa , 0x3d, 0x61, 0x27, 0x11, 0x66, 0x27, 0xa ,
            0x21, 0x1d, 0x61, 0x3b, 0xa , 0x2d, 0x65, 0x27,
            0xa , 0x6c, 0x61, 0x6d, 0x37, 0x6d, 0x6d, 0x6d,
        };       
        int i;
        System.out.print("picoCTF{");
        for(i=0;i<32;i++)
        {   byte b;
            char c;
            b=(byte) (myBytes[i]^(0x55));
            c=(char)b;
            System.out.print(c);

        }
        System.out.print("}");  
    }
}
```
