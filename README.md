# AES_plus_Blowfish_in_CBC_mode
The AES_plus_Blowfish_in_CBC_mode repository contains two sketches that enable you to encrypt your data using a combination of AES and Blowfish encryption algorithms in CBC mode. The only difference between them, from the functional point of view, is that one verifies the integrity of the decrypted data (plaintext) while the other doesn't.

## Copyright/Ownership/Licenses

Attention! I didn't develop the libraries utilized by these sketches. I took them from the following repositories:
</br>
</br>
https://github.com/zhouyangchao/AES
</br>
https://github.com/ddokkaebi/Blowfish
</br>
https://github.com/intrbiz/arduino-crypto
</br>
</br>
All libraries are the properties of their respective owners.
</br>
Copyright notices from the used libraries are in the "Copyright Notices" directory.
</br>
*Note that the library with the implementation of AES was slightly modified to make it compatible with the RTL8720DN.

## Compatibility

Both sketches successfully tested on the following development boards:
- RTL8720DN
- ESP32
- Teensy 4.1
- Raspberry Pi Pico
- ESP8266


## Usage

As for the sketches themselves, there are only two parts that you should pay attention to:
</br>
</br>
The keys at the beginning of the sketches:
</br>

    byte hmackey[] = {"qwerty"}; // Verification key
    uint8_t AES_key[32] = { // AES key
       0xd1,0xf0,0x68,0x5b,
       0x33,0xa0,0xb1,0x73,
       0xb6,0x25,0x54,0xf9,
       0xdd,0x2c,0xd3,0x1d,
       0xc1,0x93,0xb3,0x14,
       0x16,0x76,0x28,0x59,
       0x04,0x85,0xd4,0x24,
       0x9d,0xe0,0x2a,0x74
    };
    unsigned char Blwfsh_key[] = { // Blowfish key
       0xd1,0xf0,0x68,0x5b,
       0x33,0xa0,0xb1,0x73,
       0xb6,0x25,0x54,0xf9,
       0xdd,0x2c,0xd3,0x1d,
       0xc1,0x93,0xb3,0x14,
       0x16,0x76,0x28,0x69
    };

</br>
And the content of the "setup()" function:
</br>
</br>


    void setup() {
      Serial.begin(115200); // Init. Serial Terminal
      while (!Serial) {
        ; // wait for serial port to connect.
      }
      delay(500); // Wait for half a second
      //m = 0; // Set AES to 128-bit mode
      //m = 1; // Set AES to 192-bit mode
      m = 2; // Set AES to 256-bit mode
      char iv[16] = {255, 254, 253, 252, 251, 250, 249, 248, 247, 246, 245, 244, 243, 242, 241, 240}; // Initialization Vector (Should be random)
      String plaintext = "This string is encrypted using the combination of AES and Blowfish encryption algorithms in cipher block chaining mode. The integrity of this string (when decrypted) is verified using the HMAC-SHA256."; // What's being encrypted
      encrypt_string_with_AES_plus_Blowfish_in_CBC(iv, plaintext); // Encrypt data
      Serial.println();
      Serial.println("Ciphertext:");
      Serial.println(dec_st); // Ciphertext
      String ciphertext = dec_st; // Back the ciphertext up
      decrypt_string_with_AES_plus_Blowfish_in_CBC(ciphertext); // Decrypt data
      Serial.println();
      Serial.println("Plaintext:");
      Serial.println(dec_st); // Plaintext
      Serial.println();
      bool intgr = verify_integrity(); // Verify integrity of the plaintext
      if (intgr == true){
        Serial.println("Integrity Verified Successfully!");
      }
      else{
        Serial.println("Integrity Verification Failed!!!");
      }
    }


## Visual representation of the encryption and decryption processes
![image text](https://github.com/Northstrix/AES_plus_Blowfish_in_CBC_mode/blob/main/AES%2BBlowfish%20Encryption%20Algorithm%20in%20CBC%20Mode.png)
