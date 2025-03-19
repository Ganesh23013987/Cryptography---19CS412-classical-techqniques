
# Cryptography---19CS412-classical-techqniques

# Caeser Cipher
Caeser Cipher using with different key values

### Name : GANESH D
### RegNo: 212223240035


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

### Name : GANESH D
### RegNo: 212223240035

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
#include <string.h>
#include <ctype.h>

int keymat[3][3] = {{1, 2, 1}, {2, 3, 2}, {2, 2, 1}};
int invkeymat[3][3] = {{-1, 0, 1}, {2, -1, 0}, {-2, 2, -1}};
char key[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

void encode(char a, char b, char c, char *ret) {
    int x, y, z;
    int posa = (int)a - 65;
    int posb = (int)b - 65;
    int posc = (int)c - 65;

    x = posa * keymat[0][0] + posb * keymat[1][0] + posc * keymat[2][0];
    y = posa * keymat[0][1] + posb * keymat[1][1] + posc * keymat[2][1];
    z = posa * keymat[0][2] + posb * keymat[1][2] + posc * keymat[2][2];

    ret[0] = key[x % 26];
    ret[1] = key[y % 26];
    ret[2] = key[z % 26];
    ret[3] = '\0';
}

void decode(char a, char b, char c, char *ret) {
    int x, y, z;
    int posa = (int)a - 65;
    int posb = (int)b - 65;
    int posc = (int)c - 65;

    x = posa * invkeymat[0][0] + posb * invkeymat[1][0] + posc * invkeymat[2][0];
    y = posa * invkeymat[0][1] + posb * invkeymat[1][1] + posc * invkeymat[2][1];
    z = posa * invkeymat[0][2] + posb * invkeymat[1][2] + posc * invkeymat[2][2];

    ret[0] = key[(x % 26 + 26) % 26];
    ret[1] = key[(y % 26 + 26) % 26];
    ret[2] = key[(z % 26 + 26) % 26];
    ret[3] = '\0';
}

int main() {
    char msg[1000];
    char enc[1000] = "";
    char dec[1000] = "";
    int n;

    strcpy(msg, "SecurityLaboratory");
    printf("Simulation of Hill Cipher\n");
    printf("Input message: %s\n", msg);

    // Convert to uppercase
    for (int i = 0; i < strlen(msg); i++) {
        msg[i] = toupper(msg[i]);
    }

    // Add padding if needed
    n = strlen(msg) % 3;
    if (n != 0) {
        for (int i = 1; i <= (3 - n); i++) {
            strcat(msg, "X");
        }
    }

    printf("Padded message: %s\n", msg);

    // Encoding
    for (int i = 0; i < strlen(msg); i += 3) {
        char ret[4];
        encode(msg[i], msg[i + 1], msg[i + 2], ret);
        strcat(enc, ret);
    }
    printf("Encrypted message: %s\n", enc);

    // Decoding
    for (int i = 0; i < strlen(enc); i += 3) {
        char ret[4];
        decode(enc[i], enc[i + 1], enc[i + 2], ret);
        strcat(dec, ret);
    }
    printf("Decrypted message: %s\n", dec);

    return 0;
}

```

## OUTPUT:

<img width="500" alt="image" src="https://github.com/user-attachments/assets/4dafdf1f-2281-4983-a1fe-489663f0aa16" />

## RESULT:
The program is executed successfully

-------------------------------------------------

# Vigenere Cipher
Vigenere Cipher using with different key values

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

// Function to perform Vigenère encryption
void vigenereEncrypt(char *text, const char *key) {
    int textLen = strlen(text);
    int keyLen = strlen(key);

    for (int i = 0; i < textLen; i++) {
        char c = text[i];
        if (c >= 'A' && c <= 'Z') {
            // Encrypt uppercase letters
            text[i] = ((c - 'A' + key[i % keyLen] - 'A') % 26) + 'A';
        } else if (c >= 'a' && c <= 'z') {
            // Encrypt lowercase letters
            text[i] = ((c - 'a' + key[i % keyLen] - 'A') % 26) + 'a';
        }
    }
}

// Function to perform Vigenère decryption
void vigenereDecrypt(char *text, const char *key) {
    int textLen = strlen(text);
    int keyLen = strlen(key);

    for (int i = 0; i < textLen; i++) {
        char c = text[i];
        if (c >= 'A' && c <= 'Z') {
            // Decrypt uppercase letters
            text[i] = ((c - 'A' - (key[i % keyLen] - 'A') + 26) % 26) + 'A';
        } else if (c >= 'a' && c <= 'z') {
            // Decrypt lowercase letters
            text[i] = ((c - 'a' - (key[i % keyLen] - 'A') + 26) % 26) + 'a';
        }
    }
}

int main() {
    const char *key = "KEY"; // Replace with your desired key
    char message[] = "This is a secret message."; // Replace with your message

    // Encrypt the message
    vigenereEncrypt(message, key);
    printf("Encrypted Message: %s\n", message);

    // Decrypt the message back to the original
    vigenereDecrypt(message, key);
    printf("Decrypted Message: %s\n", message);

    return 0;
}

```

## OUTPUT:

<img width="500" alt="image" src="https://github.com/user-attachments/assets/aff903ac-a0a3-4fa4-8fdd-f52fe4a9f523" />

## RESULT:
The program is executed successfully

-----------------------------------------------------------------------

# Rail Fence Cipher
Rail Fence Cipher using with different key values

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
#include <stdlib.h>

int main() {
    int i, j, len, rails, count;
    int code[100][1000];
    char str[1000];

    printf("Enter a Secret Message: ");
    fgets(str, sizeof(str), stdin);
    str[strcspn(str, "\n")] = '\0'; // Remove newline character

    len = strlen(str);

    printf("Enter number of rails: ");
    scanf("%d", &rails);

    // Initialize the code matrix with 0
    for (i = 0; i < rails; i++) {
        for (j = 0; j < len; j++) {
            code[i][j] = 0;
        }
    }

    count = 0;
    j = 0;

    while (j < len) {
        if (count % 2 == 0) {
            // Fill downwards
            for (i = 0; i < rails && j < len; i++) {
                code[i][j] = (int)str[j];
                j++;
            }
        } else {
            // Fill upwards
            for (i = rails - 2; i > 0 && j < len; i--) {
                code[i][j] = (int)str[j];
                j++;
            }
        }
        count++;
    }

    // Print encoded message
    printf("\nEncoded Message: ");
    for (i = 0; i < rails; i++) {
        for (j = 0; j < len; j++) {
            if (code[i][j] != 0) {
                printf("%c", code[i][j]);
            }
        }
    }

    printf("\n");
    return 0;
}

```
## OUTPUT:

<img width="500" alt="image" src="https://github.com/user-attachments/assets/e5b62535-98e3-43a4-a37b-7ab250cffd09" />

## RESULT:
The program is executed successfully
