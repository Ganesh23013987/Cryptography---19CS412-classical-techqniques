# Cryptography---19CS412-classical-techqniques

# Caeser Cipher
Caeser Cipher using with different key values

## Name : GANESH D
## RegNo: 212223240035


# AIM:

To encrypt and decrypt the given message by using Ceaser Cipher encryption algorithm.


## DESIGN STEPS:

### Step 1:

Design of Caeser Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

1.	In Ceaser Cipher each letter in the plaintext is replaced by a letter some fixed number of positions down the alphabet.
2.	For example, with a left shift of 3, D would be replaced by A, E would become B, and so on.
3.	The encryption can also be represented using modular arithmetic by first transforming the letters into numbers, according to the   
    scheme, A = 0, B = 1, Z = 25.
4.	Encryption of a letter x by a shift n can be described mathematically as,
                       En(x) = (x + n) mod26
5.	Decryption is performed similarly,
                       Dn (x)=(x - n) mod26


## PROGRAM:

```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    char message[100]; // Array to store the message
    int key;

    printf("Enter the message to encrypt: ");
    fgets(message, sizeof(message), stdin); // Read input from the user

    // Remove trailing newline from fgets
    message[strcspn(message, "\n")] = '\0';

    printf("Enter the Caesar Cipher key (an integer): ");
    scanf("%d", &key); // Read the key from the user

    // Encryption logic (directly in main)
    for (int i = 0; message[i] != '\0'; i++) {
        char c = message[i];

        if (c >= 'A' && c <= 'Z') {
            message[i] = ((c - 'A' + key) % 26 + 26) % 26 + 'A';
        } else if (c >= 'a' && c <= 'z') {
            message[i] = ((c - 'a' + key) % 26 + 26) % 26 + 'a';
        }
    }

    printf("Encrypted Message: %s\n", message);

    // Decryption logic (directly in main)
    for (int i = 0; message[i] != '\0'; i++) {
        char c = message[i];

        if (c >= 'A' && c <= 'Z') {
            message[i] = ((c - 'A' - key) % 26 + 26) % 26 + 'A';
        } else if (c >= 'a' && c <= 'z') {
            message[i] = ((c - 'a' - key) % 26 + 26) % 26 + 'a';
        }
    }

    printf("Decrypted Message: %s\n", message);

    return 0;
}
```

## OUTPUT:
![image](https://github.com/user-attachments/assets/1742beb6-fc79-407d-a0fc-1e0dacc87625)

## RESULT:
The program is executed successfully

---------------------------------


# PlayFair Cipher
Playfair Cipher using with different key values

## Name : GANESH D
## RegNo: 212223240035


# AIM:

To implement a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique.

 
## DESIGN STEPS:

### Step 1:

Design of PlayFair Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

ALGORITHM DESCRIPTION:
The Playfair cipher uses a 5 by 5 table containing a key word or phrase. To generate the key table, first fill the spaces in the table with the letters of the keyword, then fill the remaining spaces with the rest of the letters of the alphabet in order (usually omitting "Q" to reduce the alphabet to fit; other versions put both "I" and "J" in the same space). The key can be written in the top rows of the table, from left to right, or in some other pattern, such as a spiral beginning in the upper-left-hand corner and ending in the centre.
The keyword together with the conventions for filling in the 5 by 5 table constitutes the cipher key. To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. Then apply the following 4 rules, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter. Encrypt the new pair and continue. Some   
   variants of Playfair use "Q" instead of "X", but any letter, itself uncommon as a repeated pair, will do.
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively (wrapping 
   around to the left side of the row if a letter in the original pair was on the right side of the row).
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively (wrapping around 
   to the top side of the column if a letter in the original pair was on the bottom side of the column).
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of 
   corners of the rectangle defined by the original pair. The order is important – the first letter of the encrypted pair is the one that 
    lies on the same row as the first letter of the plaintext pair.
To decrypt, use the INVERSE (opposite) of the last 3 rules, and the 1st as-is (dropping any extra "X"s, or "Q"s that do not make sense in the final message when finished).


## PROGRAM:

```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define MX 5

void playfair(char ch1, char ch2, char key[MX][MX]) {
    int i, j, w, x, y, z;

    for (i = 0; i < MX; i++) {
        for (j = 0; j < MX; j++) {
            if (ch1 == key[i][j]) {
                w = i;
                x = j;
            } 
            if (ch2 == key[i][j]) {
                y = i;
                z = j;
            }
        }
    }

    if (w == y) { 
        x = (x + 1) % 5;
        z = (z + 1) % 5;
    } else if (x == z) { 
        w = (w + 1) % 5;
        y = (y + 1) % 5;
    } else { 
        int temp = x;
        x = z;
        z = temp;
    }

    printf("%c%c", key[w][x], key[y][z]); // Print directly
}

void remove_duplicates(char *str) {
    int hash[26] = {0}, i, j = 0;
    for (i = 0; str[i]; i++) {
        if (!hash[str[i] - 'A']) {
            str[j++] = str[i];
            hash[str[i] - 'A'] = 1;
        }
    }
    str[j] = '\0';
}

void prepare_input(char *str) {
    int i, j = 0;
    for (i = 0; str[i]; i++) {
        if (isalpha(str[i])) {
            str[j++] = toupper(str[i] == 'J' ? 'I' : str[i]);
        }
    }
    str[j] = '\0';
}

void generate_key_matrix(char key[MX][MX], char *keystr) {
    char alphabet[26] = "ABCDEFGHIKLMNOPQRSTUVWXYZ";  
    char temp[26] = {0};
    int i, j, k = 0;

    strcpy(temp, keystr);
    remove_duplicates(temp);

    for (i = 0; i < 26; i++) {
        if (!strchr(temp, alphabet[i])) {
            temp[strlen(temp)] = alphabet[i];
        }
    }

    k = 0;
    for (i = 0; i < MX; i++) {
        for (j = 0; j < MX; j++) {
            key[i][j] = temp[k++];
            printf("%c ", key[i][j]);
        }
        printf("\n");
    }
}

void encrypt(char *str, char key[MX][MX]) {
    printf("\nCipher Text: ");
    int i;
    for (i = 0; str[i]; i++) {
        if (str[i + 1] == '\0') 
            playfair(str[i], 'X', key);
        else {
            if (str[i] == str[i + 1]) {
                playfair(str[i], 'X', key);
            } else {
                playfair(str[i], str[i + 1], key);
                i++;
            }
        }
    }
    printf("\n");
}

int main() {
    char key[MX][MX], keystr[20], str[100];

    printf("Enter key: ");
    fgets(keystr, sizeof(keystr), stdin);
    keystr[strcspn(keystr, "\n")] = '\0'; 

    printf("Enter the plain text: ");
    fgets(str, sizeof(str), stdin);
    str[strcspn(str, "\n")] = '\0';

    prepare_input(keystr);
    prepare_input(str);

    generate_key_matrix(key, keystr);
    
    printf("\nEntered text: %s", str);
    encrypt(str, key);

    return 0;
}
```

## OUTPUT:
![image](https://github.com/user-attachments/assets/4b4b4ab7-1b75-4d1f-bf35-b3708682f19e)




## RESULT:
The program is executed successfully


---------------------------

# Hill Cipher
Hill Cipher using with different key values

## Name : GANESH D
## RegNo: 212223240035

# AIM:

To develop a simple C program to implement Hill Cipher.

## DESIGN STEPS:

### Step 1:

Design of Hill Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
The Hill cipher is a substitution cipher invented by Lester S. Hill in 1929. Each letter is represented by a number modulo 26. To encrypt a message, each block of n letters is multiplied by an invertible n × n matrix, again modulus 26.
To decrypt the message, each block is multiplied by the inverse of the matrix used for encryption. The matrix used for encryption is the cipher key, and it should be chosen randomly from the set of invertible n × n matrices (modulo 26).
The cipher can, be adapted to an alphabet with any number of letters. All arithmetic just needs to be done modulo the number of letters instead of modulo 26.


## PROGRAM:
```
#include <stdio.h>

int key[3][3] = {
    {6, 24, 1},
    {13, 16, 10},
    {20, 17, 15}
};

int inverseKey[3][3] = {
    {8, 5, 10},
    {21, 8, 21},
    {21, 12, 8}
};

int main() {
    char msg[4];  
    unsigned int enc[3] = {0}, dec[3] = {0};

    printf("Enter plain text (3 letters): ");
    scanf("%3s", msg);
    msg[3] = '\0';  

    // Convert to uppercase
    for (int i = 0; i < 3; i++) {
        if (msg[i] >= 'a' && msg[i] <= 'z') {
            msg[i] -= 32;
        }
    }

    // Encryption
    for (int i = 0; i < 3; i++) {
        enc[i] = 0;
        for (int j = 0; j < 3; j++) {
            enc[i] += key[i][j] * (msg[j] - 'A');
        }
        enc[i] = enc[i] % 26;
    }

    printf("Encrypted Cipher Text: %c%c%c\n", enc[0] + 'A', enc[1] + 'A', enc[2] + 'A');

    // Decryption
    for (int i = 0; i < 3; i++) {
        dec[i] = 0;
        for (int j = 0; j < 3; j++) {
            dec[i] += inverseKey[i][j] * enc[j];
        }
        dec[i] = (dec[i] % 26 + 26) % 26;
    }

    printf("Decrypted Cipher Text: %c%c%c\n", dec[0] + 'A', dec[1] + 'A', dec[2] + 'A');

    return 0;
}

```

## OUTPUT:
<img width="700" alt="image" src="https://github.com/user-attachments/assets/d1bd57a3-cef5-41d2-a75c-733803557fe9" />

## RESULT:
The program is executed successfully

-------------------------------------------------

# Vigenere Cipher
Vigenere Cipher using with different key values

## Name : GANESH D
## RegNo: 212223240035

# AIM:

To develop a simple C program to implement Vigenere Cipher.

## DESIGN STEPS:

### Step 1:

Design of Vigenere Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
The Vigenere cipher is a method of encrypting alphabetic text by using a series of different Caesar ciphers based on the letters of a keyword. It is a simple form of polyalphabetic substitution.To encrypt, a table of alphabets can be used, termed a Vigenere square, or Vigenere table. It consists of the alphabet written out 26 times in different rows, each alphabet shifted cyclically to the left compared to the previous alphabet, corresponding to the 26 possible Caesar ciphers. At different points in the encryption process, the cipher uses a different alphabet from one of the rows used. The alphabet at each point depends on a repeating keyword.



## PROGRAM:

```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define MAX_LENGTH 100

int main() 
{
    char input[MAX_LENGTH];
    char key[MAX_LENGTH];
    char result[MAX_LENGTH];

    printf("Enter the text to encrypt: ");
    fgets(input, MAX_LENGTH, stdin);
    input[strcspn(input, "\n")] = '\0'; 

    printf("Enter the key: ");
    fgets(key, MAX_LENGTH, stdin);
    key[strcspn(key, "\n")] = '\0'; 

    int inputLength = strlen(input);
    int keyLength = strlen(key);

    for (int i = 0, j = 0; i < inputLength; ++i) 
    {
        char currentChar = input[i];

        if (isalpha(currentChar))
        {
            int shift = toupper(key[j % keyLength]) - 'A';
            int base = isupper(currentChar) ? 'A' : 'a';

            result[i] = ((currentChar - base + shift + 26) % 26) + base;
            ++j;
        }
        else
        {
            result[i] = currentChar;
        }
    }

    result[inputLength] = '\0';
    printf("Encrypted text: %s\n", result);

    for (int i = 0, j = 0; i < inputLength; ++i) 
    {
        char currentChar = result[i];

        if (isalpha(currentChar)) 
        {
            int shift = toupper(key[j % keyLength]) - 'A';
            int base = isupper(currentChar) ? 'A' : 'a';

            result[i] = ((currentChar - base - shift + 26) % 26) + base;
            ++j;
        }
    }

    result[inputLength] = '\0';
    printf("Decrypted text: %s\n", result);

    return 0;
}
```

## OUTPUT:

<img width="700" alt="image" src="https://github.com/user-attachments/assets/4e2ed205-ecfd-44d7-bc82-ec7141900c58" />


## RESULT:
The program is executed successfully

-----------------------------------------------------------------------

# Rail Fence Cipher
Rail Fence Cipher using with different key values

## Name : GANESH D
## RegNo: 212223240035

# AIM:

To develop a simple C program to implement Rail Fence Cipher.

## DESIGN STEPS:

### Step 1:

Design of Rail Fence Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
In the rail fence cipher, the plaintext is written downwards and diagonally on successive "rails" of an imaginary fence, then moving up when we reach the bottom rail. When we reach the top rail, the message is written downwards again until the whole plaintext is written out. The message is then read off in rows.

## PROGRAM:

```
#include <stdio.h>
#include <string.h>

int main() {
    int i, j, k, l;
    char a[20], c[20], d[20];

    printf("\n\t\t RAIL FENCE TECHNIQUE");
    printf("\n\nEnter the input string : ");
    fgets(a, sizeof(a), stdin);
    a[strcspn(a, "\n")] = '\0';  // Remove newline

    l = strlen(a);

    // Encryption
    for (i = 0, j = 0; i < l; i++) {
        if (i % 2 == 0)
            c[j++] = a[i];
    }
    for (i = 0; i < l; i++) {
        if (i % 2 == 1)
            c[j++] = a[i];
    }
    c[j] = '\0';

    printf("\nCipher text after applying rail fence: %s", c);

    // Decryption
    if (l % 2 == 0)
        k = l / 2;
    else
        k = (l / 2) + 1;

    for (i = 0, j = 0; i < k; i++) {
        d[j] = c[i];
        j += 2;
    }
    for (i = k, j = 1; i < l; i++) {
        d[j] = c[i];
        j += 2;
    }
    d[l] = '\0';

    printf("\nText after decryption: %s\n", d);

    return 0;
}

```
## OUTPUT:

<img width="700" alt="image" src="https://github.com/user-attachments/assets/aa2863de-9da8-4bff-92ea-518cb0c100dd" />


## RESULT:
The program is executed successfully
