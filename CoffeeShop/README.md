## Reconnaissance

So, for this easy reverse challenge we are given a zip folder !

![[Pasted image 20230909094852.png]]

Time to unzip it !

```
unzip CoffeeShop.zip
```

And we see a .jar file.
## Exploitation

The first step is to decompile this CoffeeShop.jar. If you're not used to reverse java code you can ask yourself what tool could be the best for this task. In fact Ghidra, Cutter etc can decompile .jar files but won't succed as others like _JD-GUI_ ! JD-GUI comes with a very user-friendly interface so it's suitable if you're a beginner like me.

So let's decompile CoffeeShop.jar with JD-GUI, we got the CoffeeShop.class :

![[Pasted image 20230909095520.png]]

Here's the text code :

```java
import java.util.Base64;
import java.util.Scanner;

class CoffeeShop {
  public static void failure() {
    System.out.println("Sorry! You didn't get it right...");
  }
  
  public static void success() {
    System.out.println("Nice! You got it!");
    System.out.println("Submit the name you entered inside CACI{} to get points!");
  }
  
  public static void main(String[] paramArrayOfString) {
    Scanner scanner = new Scanner(System.in);
    System.out.println("What's my name?");
    String str1 = scanner.nextLine();
    String str2 = Base64.getEncoder().encodeToString(str1.getBytes());
    if (str2.length() != 20) {
      failure();
    } else if (!str2.endsWith("NoZXI=")) {
      failure();
    } else if (!str2.startsWith("R2FsZU")) {
      failure();
    } else if (!str2.substring(6, 14).equals("JvZXR0aW")) {
      failure();
    } else {
      success();
    } 
  }
}
```

Some observations regarding this code :
- The password is encoded in base64
- The password must be 20 characters long
After encoding it must :
- end with NoZXI=
- start with R2FsZU
- Have from the 6 to the 14th character the value : JvZXR0aW

So we have the final flag encoded :
R2FsZUJvZXR0aWNoZXI=

After decoding from base64, it gives us the flag : GaleBoetticher !
