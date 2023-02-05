**TL;DR**
In this repository, an analysis of the EsxiArgs encrypted binary file is produced. Based on reverse engineering analysis, we have become capable of creating a sample decryption process that is based on the sosemanuk_encryption and gen_stream_key of the ransomware.

![sosemanuk_encrypt](https://i.imgur.com/nrUf33D.png)
Details:

his is a C++ function that encrypts a file. The function takes five arguments:

-   `param_1`: A filename for the file to be encrypted.
-   `param_2`: RSA public key to encrypt the stream key.
-   `param_3`: Algorithm to encrypt the data.
-   `param_4`: Size of data to encrypt.
-   `param_5`: Key derivation iteration count.

The function first opens the file using the `open_read_write` function. If it fails, it prints an error message and returns 1. If it succeeds, the function generates a stream key using the `gen_stream_key` function. If this fails, it prints an error message and returns 2. If it succeeds, it uses the RSA public key and the `rsa_encrypt` function to encrypt the stream key. If this fails, it prints an error message and returns 3. If it succeeds, it uses the `encrypt_simple` function to encrypt the data in the file using the encrypted stream key. If this fails, it prints an error message and returns 4. If it succeeds, the function uses the `lseek` function to position the file pointer at the end of the file and then writes the encrypted stream key to the end of the file using the `write` function. If this fails, it prints an error message and returns 5. If everything succeeds, the function returns 0.

gen_stream_key: 
![gen_stream_key: ](https://i.imgur.com/O6Poau9.png)
Implementation of Sosemanuk decryption in C++:

    #include <iostream>
    #include <vector>
    #include "sosemanuk.h"
    
    int main() {
      std::vector<unsigned char> key(16);
      std::vector<unsigned char> nonce(16);
      std::vector<unsigned char> ciphertext(32);
      std::vector<unsigned char> plaintext(32);
    
      // Fill `key` and `nonce` with actual values
      // Fill `ciphertext` with the ciphertext to be decrypted
    
      sosemanuk::Sosemanuk cipher(key.data(), key.size(), nonce.data(), nonce.size());
      cipher.decrypt(ciphertext.data(), plaintext.data(), plaintext.size());
    
      // The decrypted plaintext is now in the `plaintext` vector
    
      return 0;
    }

References: 

https://github.com/cchcc/SOSEMANUK/tree/master/C

https://www.cryptopp.com/wiki/Sosemanuk

Encryption script https://pastebin.com/y6wS2BXh

Encrypted binary 11b1b2375d9d840912cfd1f0d0d04d93ed0cddb0ae4ddb550a5b62cd044d6b66

index.html

    How to Restore Your Files  
    Security Alert!!!  
    We hacked your company successfully  
    　  
    All files have been stolen and encrypted by us  
    If you want to restore files or avoid file leaks, please send (PRICE) bitcoins to the wallet BTC WALLET
    If money is received, encryption key will be available on TOX_ID: (TOX ID)  
    　  
    Attention!!!  
    Send money within 3 days, otherwise we will expose some data and raise the price  
    Don't try to decrypt important files, it may damage your files  
    Don't trust who can decrypt, they are liars, no one can decrypt without key file  
    If you don't send bitcoins, we will notify your customers of the data breach by email and text message  
    And sell your data to your opponents or criminals, data may be made release  
    　  
    Note  
    SSH is turned on  
    Firewall is disabled
