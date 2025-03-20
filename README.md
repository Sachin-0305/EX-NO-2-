## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
 

 

To write a Python program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




## Program:

```

def format_key(key):
    key = key.upper().replace('J', 'I') 
    formatted_key = []
    seen = set()
    for char in key:
        if char.isalpha() and char not in seen:
            formatted_key.append(char)
            seen.add(char)
    return ''.join(formatted_key)

def create_matrix(key):
    alphabet = 'ABCDEFGHIKLMNOPQRSTUVWXYZ'  
    matrix = list(key)
    

    for char in alphabet:
        if char not in matrix:
            matrix.append(char)
 
    return [matrix[i:i+5] for i in range(0, len(matrix), 5)]


def prepare_plaintext(plaintext):
    plaintext = plaintext.upper().replace('J', 'I')  
    prepared_text = []
    
    i = 0
    while i < len(plaintext):
        if i + 1 < len(plaintext) and plaintext[i] == plaintext[i + 1]:
            prepared_text.append(plaintext[i] + 'X') 
            i += 1
        else:
            prepared_text.append(plaintext[i:i+2]) 
            i += 2
            
    if len(prepared_text[-1]) == 1: 
        prepared_text[-1] += 'X'
    
    return prepared_text

def find_position(matrix, char):
    for i, row in enumerate(matrix):
        if char in row:
            return i, row.index(char)


def encrypt(plaintext, matrix):
    prepared_text = prepare_plaintext(plaintext)
    cipher_text = ''
    
    for bigram in prepared_text:
        row1, col1 = find_position(matrix, bigram[0])
        row2, col2 = find_position(matrix, bigram[1])
        
     
        if row1 == row2:
            cipher_text += matrix[row1][(col1 + 1) % 5]
            cipher_text += matrix[row2][(col2 + 1) % 5]
    
        elif col1 == col2:
            cipher_text += matrix[(row1 + 1) % 5][col1]
            cipher_text += matrix[(row2 + 1) % 5][col2]
       
        else:
            cipher_text += matrix[row1][col2]
            cipher_text += matrix[row2][col1]
    
    return cipher_text

def decrypt(ciphertext, matrix):
    bigrams = [ciphertext[i:i+2] for i in range(0, len(ciphertext), 2)]
    plain_text = ''
    
    for bigram in bigrams:
        row1, col1 = find_position(matrix, bigram[0])
        row2, col2 = find_position(matrix, bigram[1])
        
        if row1 == row2:
            plain_text += matrix[row1][(col1 - 1) % 5]
            plain_text += matrix[row2][(col2 - 1) % 5]
       
        elif col1 == col2:
            plain_text += matrix[(row1 - 1) % 5][col1]
            plain_text += matrix[(row2 - 1) % 5][col2]
      
        else:
            plain_text += matrix[row1][col2]
            plain_text += matrix[row2][col1]
    
    return plain_text

if __name__ == "__main__":
    key = input("Enter the key for Playfair Cipher: ")
    plaintext = input("Enter the plaintext to encrypt: ")
    
    formatted_key = format_key(key)
    matrix = create_matrix(formatted_key)
    
    encrypted_text = encrypt(plaintext, matrix)
    print("Encrypted Text: ", encrypted_text)
    
    decrypted_text = decrypt(encrypted_text, matrix)
    print("Decrypted Text: ", decrypted_text)

```



## Output:
![Screenshot 2025-03-20 090751](https://github.com/user-attachments/assets/c0086376-e048-46e1-b75f-49a891b254c3)

## RESULT :
The program is executed successfully
