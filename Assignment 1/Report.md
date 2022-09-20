# Assignment 1 Report

## 2.1- Password & Key Generation and Encryption

### Exercise 01- Password Generation

#### Q1. 

```openssl rand -hex 10 > mypassword```

```cat mypassword```

#### Q2.

Size is 21 

#### Q3. 

1 byte has 2 hexadecimal characters so the total size should be around 20. I am getting 21 which maybe be due to some information of the file present.


### Exercise 02- Experimenting with different ciphers

#### Q1. 

```openssl enc -des-ecb -e -in alice.txt -out alice-des-ecb.bin -pass file:mypassword -iter 2 -p```

##### salt=D742B44E5839A9FD

##### key=CC52AAFCF88D80D1

#### Q2.
 
```openssl enc -des-ecb -d -in alice-des-ecb.bin -out alice-decrypt-des-ecb.txt -pass file:mypassword -iter 2 -p```

##### salt=D742B44E5839A9FD

##### key=CC52AAFCF88D80D1

```cmp alice-decrypt-des-ecb.txt alice.txt```

#### Q3. 

##### a)
```openssl enc -des-cbc -e -in alice.txt -out alice-des-cbc.bin -pass file:mypassword -iter 2 -p```

##### salt=4B0A798F3E366B30
##### key=99F12DB3775F2B48
##### iv =E505145E1A0C7AAE

```openssl enc -des-cbc -d -in alice-des-cbc.bin -out alice-decrypt-des-cbc.txt -pass file:mypassword -iter 2 -p```

##### salt=4B0A798F3E366B30
##### key=99F12DB3775F2B48
##### iv =E505145E1A0C7AAE

```cmp alice-decrypt-des-cbc.txt alice.txt```

##### b)

An initialization vector (IV) is an arbitrary number that can be used along with a secret key for data encryption.


Q4. 

```openssl enc -aes-128-cbc -e -in alice.txt -out alice-aes-cbc.bin -pass file:mypassword -iter 2 -p```

##### salt=DB40957B13BCF4BE
##### key=F242DB910A5C271C379739122362ED66
##### iv =8E806FD78FE18759D1E9FB85C5D3873F

```openssl enc -aes-128-cbc -d -in alice-aes-cbc.bin -out alice-decrypt-aes-cbc.txt -pass file:mypassword -iter 2 -p```

##### salt=DB40957B13BCF4BE
##### key=F242DB910A5C271C379739122362ED66
##### iv =8E806FD78FE18759D1E9FB85C5D3873F


Length of key and iv is 32



## 2.2- The Avalanche Effect of Encryption

### Exercise 03- The Avalanche Effect of Encryption

#### Q1. 

```echo -ne '\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00' > avalanche-in1.hex```

```openssl enc -des3 -e -in avalanche-in1.hex -out avalanche-out1-des3.bin -pass file:mypassword -iter 2 -p ```

##### salt=3EB4030474481200
##### key=7F12BD53A5DA6BE9136B44CC06DB61CA5D22C4FFF3C06C25
##### iv =970FCEA4D2AF6FD2


```openssl enc -des3 -e -in avalanche-in2.hex -out avalanche-out2-des3.bin -pass file:mypassword -iter 2 -p ```

##### salt=2DEEF76A51943023
##### key=37494395CEEE067C1B9A1F718C2D02A978498E01101D2C36
##### iv =3A0EBC286E793F0A


#### Q2.

Length of salt is 16 (8 bytes)
Length of key is 48 (24 bytes)
Length of iv is 16 (8 bytes)
Block size of des3 is 8 bytes

#### Q3.

bitdiff -f avalanche-out1-des3.bin avalanche-out2-des3.bin -s 8         
124



#### Q4.

$ openssl enc -aes-128-cbc -e -in avalanche-in1.hex -out avalanche-out1-aes-128.bin -pass file:mypassword -iter 2 -p

salt=469B842C5B8C77AC
key=A0938975D3118234CDF7EF6A01735D02
iv =E245D3755B62AD72835C6F3D8AA58BB4


$ openssl enc -aes-128-cbc -e -in avalanche-in2.hex -out avalanche-out2-aes-128.bin -pass file:mypassword -iter 2 -p

salt=52BD60C81EA2654A
key=808149CF89BA33E708D38C0560C5E15B
iv =DFFEC0272F7A1F57DEF53CE0FC8805DE


Q5.
Length of salt is 16 (8 bytes)
Length of key is 32 (16 bytes)
Length of IV is 32 (16 bytes)

Q6.

bitdiff -f avalanche-out1-aes-128.bin avalanche-out2-aes-128.bin -s 8 
161

Q7.

$ openssl enc -aes-256-cbc -e -in avalanche-in1.hex -out avalanche-out1-aes-256.bin -pass file:mypassword -iter 2 -p

salt=39AE95512158B9D7
key=6421019F9BEC490205C79E38C1B985494E5CE6B1226B101B9C439A812DA327DC
iv =327CF20DCF9F1D26A74069E6B8698F7E


$ openssl enc -aes-256-cbc -e -in avalanche-in2.hex -out avalanche-out2-aes-256.bin -pass file:mypassword -iter 2 -p

salt=26965C0AC891087D
key=BBE3FC914709288A3F550EA6329840902F19E9015083839EC9671005D8254F10
iv =4FCC01CD242156605EE91FB3C9FA842B


Q8.

Length of salt is 16 (8 bytes)
Length of key is 64 (32 bytes)
Lenght of IV is 32 (16 bytes)


Q9.

$ bitdiff -f avalanche-out1-aes-256.bin avalanche-out2-aes-256.bin -s 8
163

2.3

Exercise 4

Q1.

echo -n 123456781234567812345678 > plain.txt

Q2. 

openssl enc -des-ecb -e -in plain.txt -out cipher-ecb-1.bin -K 0011223344556677 -p    
 
salt=4AA2C7F3EF7F0000
key=0011223344556677

Q3.

openssl enc -des-ecb -d -in cipher-ecb-1.bin -out encrypt-ecb-1.txt -K 0011223344556677 -p

salt=4AD2DB42C77F0000
key=0011223344556677

Q4.

cp cipher-ecb-1.bin cipher-ecb-2.bin

Q5. 

Q6.

openssl enc -des-ecb -d -in cipher-ecb-2.bin -out decrypt-ecb-2.txt -K 0011223344556677 -p

Q7. 

cat plain.txt
123456781234567812345678

$ cat decrypt-ecb-2.txt
�jjXD���1234567812345678

$ xxd -p decrypt-ecb-2.txt                                                    
a06a6a5844b593c431323334353637383132333435363738

$ xxd -p plain.txt
313233343536373831323334353637383132333435363738


No they are not the same. plain.txt was originally  123456781234567812345678 but the first part, that is, 12345678 changed and the decrypt-ecb-2.txt became �jjXD���1234567812345678. The other part 1234567812345678 remained the same for both the files. 

DES is a block cipher where the input, plaintext, is divided into blocks of 64 bits, 8 bytes. DES-ECB (Electronic Code Book) does not use any chaining methods.
So when we changed the first character of encrypted file we are only changing the first 8 bytes so the first 12345678 changed in the plain.txt file while other remained unchanged.



Exercise 5




Q1.

openssl enc -des-cbc -e -in plain.txt -out cipher-cbc-1.bin -K 0123456789ABCDEF -iv 0123456789ABCDEF -p
salt=4A22B477107F0000
key=0123456789ABCDEF
iv =0123456789ABCDEF


Q2.

openssl enc -des-cbc -d -in cipher-cbc-1.bin -out decrypt-cbc-1.txt -K 0123456789ABCDEF -iv 0123456789ABCDEF -p
salt=4A0276F9727F0000
key=0123456789ABCDEF
iv =0123456789ABCDEF


Q3.

cp cipher-cbc-1.bin cipher-cbc-2.bin

Q4.

Q5.

openssl enc -des-cbc -d -in cipher-cbc-2.bin -out decrypt-cbc-2.txt -K 0123456789ABCDEF -iv 0123456789ABCDEF -p
salt=4A82161AA87F0000
key=0123456789ABCDEF
iv =0123456789ABCDEF

Q6.

cat decrypt-cbc-2.txt                                                            
���!�234567812345678

cat plain.txt                                                                      
123456781234567812345678

xxd -p plain.txt                                                                   
313233343536373831323334353637383132333435363738


xxd -p decrypt-cbc-2.txt                                                      
8def899919bddf2181323334353637383132333435363738


cmp decrypt-cbc-2.txt plain.txt                                              
decrypt-cbc-2.txt plain.txt differ: byte 1, line 1


No they are not the same.









