**TL;DR**
In this repository, an analysis of the EsxiArgs encrypted binary file is produced. Based on reverse engineering analysis, we have become capable of creating a sample decryption process that is based on the sosemanuk_encryption and gen_stream_key of the ransomware.

![sosemanuk_encrypt](https://i.imgur.com/nrUf33D.png)

Details:

The function sosemanuk_encrypt implements the encryption process for the Sosemanuk stream cipher. The function takes 4 parameters: param_1, param_2, param_3, and param_4. The first parameter, param_1, is a pointer to a buffer of data used by the Sosemanuk algorithm, while the second and third parameters, param_2 and param_3, are pointers to the input plaintext and the output ciphertext, respectively. The fourth parameter, param_4, is the length of the plaintext.

The function uses a local variable local_38 to keep track of the remaining length of the plaintext that needs to be encrypted. The function also has several other local variables: local_30 and local_28, which are used to keep track of the current position in the input and output buffers, respectively, and local_10, which is used as a temporary variable.

The function first checks if the value stored at param_1 + 0x80 is less than 0x50, and if so, it XORs the first 0x50 - (param_1 + 0x80) bytes of the plaintext with the internal state of the Sosemanuk cipher. The function then enters a loop that encrypts the remaining plaintext in blocks of 0x50 bytes at a time, updating the internal state of the Sosemanuk cipher after each block is encrypted. The encrypted blocks are then XORed with the plaintext to produce the ciphertext.

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
