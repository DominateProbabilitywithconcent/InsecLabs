# 21110779 - Trần Thanh Luân
#### Link to GitHub: https://github.com/DominateProbabilitywithconcent/InsecLabs/tree/main
# 4.1. Encrypt and Decrypt Text File

 4.1.1. Create new file.
 - First, create new file named ___plain.txt___ with some context. Context I choose here is __"Non-dairy milkshake"__.<br>
     ```sh
    cat > plain.txt
    Non-dairy milkshake
     # [Enter] then [Ctrl + D] to save file.
    ```
    **Notes**:
    ```sh
    key [K] = 00112233445566778899AABBCCDDEEFF00112233445566778899AABBCCDDEEFF
    initialization vector [IV] = 0102030405060708090A0B0C0D0E0F10
    ```   
 4.1.2. ECB Encryption.
 - Encrypt file plain.txt in __aes-256 bit__ in __ECB__ with key [K].<br>
    ```sh
    openssl enc -aes-256-ecb -nosalt -in plain.txt -out ecb_encrypted.txt -K 00112233445566778899AABBCCDDEEFF00112233445566778899AABBCCDDEEFF
    ```
 - Check context of ecb_encrypted file.<br>
 <img width="500" alt="Screenshot" src="https://github.com/DominateProbabilitywithconcent/InsecLabs/blob/main/SecLabImages/EnLargeMSG/ecbencrypted.png?raw=true"><br>
 <br>
 - Decrypt from ECB file.<br>
    ```sh
    openssl enc -d -aes-256-ecb -nosalt -in ecb_encrypted.txt -out ecb_decrypted.txt -K 00112233445566778899AABBCCDDEEFF00112233445566778899AABBCCDDEEFF
    ```
 - Check context of decrypted file.<br>
 <img width="500" alt="Screenshot" src="https://github.com/DominateProbabilitywithconcent/InsecLabs/blob/main/SecLabImages/EnLargeMSG/ecbdecrypted.png?raw=true"><br>

 4.1.3. CBC Encryption.
 - Encrypt file plain.txt in __aes-256 bit__ in __CBC__ with key [K] and initialization vector [IV].
    ```sh
    openssl enc -aes-256-cbc -nosalt -in plain.txt -out cbc_encrypted.txt -K        00112233445566778899AABBCCDDEEFF00112233445566778899AABBCCDDEEFF -iv 0102030405060708090A0B0C0D0E0F10
    ```
 - Check context of cbc_encrypted file.<br>
 <img width="500" alt="Screenshot" src="https://github.com/DominateProbabilitywithconcent/InsecLabs/blob/main/SecLabImages/EnLargeMSG/cbcencrypted.png?raw=true"><br>
 - Decrypt encrypted file.
    ```sh
    openssl enc -d -aes-256-cbc -nosalt -in cbc_encrypted.txt -out cbc_decrypted.txt -K 00112233445566778899AABBCCDDEEFF00112233445566778899AABBCCDDEEFF -iv 0102030405060708090A0B0C0D0E0F10
    ```
 - Check context of decrypted file.<br>
 <img width="500" alt="Screenshot" src="https://github.com/DominateProbabilitywithconcent/InsecLabs/blob/main/SecLabImages/EnLargeMSG/cbcdecrypted.png?raw=true"><br>


# 4.2. Encryption Mode -ECB vs. CBC
 4.2.1. Store the bitmap file in the linux virtual computer.<br>
 <img width="500" alt="Screenshot" src="https://github.com/DominateProbabilitywithconcent/InsecLabs/blob/main/SecLabImages/EnLargeMSG/originstore.png?raw=true"><br>
 4.2.2. Encryption - ECB vs CBC.<br>
 - Encryption using ECB with key [K]. Named the encrypted file "originECB.bmp".
    ```sh
    openssl enc -aes-256-ecb -nosalt -in origin.bmp -out originECB.bmp -K 00112233445566778899AABBCCDDEEFF00112233445566778899AABBCCDDEEFF
    ```
 - Encryption using CBC with key [K] and initialization vector [IV]. Named the encrypted fule "originCBC.bmp".
    ```sh
    openssl enc -aes-256-cbc -nosalt -in origin.bmp -out originCBC.bmp -K 00112233445566778899AABBCCDDEEFF00112233445566778899AABBCCDDEEFF -iv 0102030405060708090A0B0C0D0E0F10
    ```
    <img width="500" alt="Screenshot" src="https://github.com/DominateProbabilitywithconcent/InsecLabs/blob/main/SecLabImages/EnLargeMSG/Screenshot%202024-07-18%20144700.png?raw=true"><br>

 4.2.3. Create a partially encrypted file.<br>
 - origin.bmp context:<br>
 <img width="750" alt="Screenshot" src="https://github.com/DominateProbabilitywithconcent/InsecLabs/blob/main/SecLabImages/EnLargeMSG/Screenshot%202024-07-18%20135048.png?raw=true"><br>

    4.2.3.1. ECB Mode.
    - Use these following command:
    ```sh
    dd if=origin.bmp of=header.bin bs=1 count=54 #store the header into header.bin file
    dd if=origin.bmp of=body.bin bs=1 skip=54 #store the rest of the file into body.bin file
    openssl enc -aes-256-ecb -nosalt -in body.bin -out ecb_encrypted_body.bin -K 00112233445566778899AABBCCDDEEFF00112233445566778899AABBCCDDEEFF #ECB encrypt the body.bin file. 
    cat header.bin encrypted_body.bin > partially_encrypted.bmp #Link header file into encrypted body file.
    ```
    - After encryption:<br>
 <img width="750" alt="Screenshot" src="https://github.com/DominateProbabilitywithconcent/InsecLabs/blob/main/SecLabImages/EnLargeMSG/Screenshot%202024-07-18%20185236.png?raw=true"><br>
     => The picture comprises of shapes and elements similar to the original context, even though the color is changed. The message can still mostly be seen and undertsood by other.<br>
    4.2.3.2. CBC Mode.
    - Use these following command:
    ```sh
    openssl enc -aes-256-cbc -nosalt -in body.bin -out encrypted_body.bin -K 00112233445566778899AABBCCDDEEFF00112233445566778899AABBCCDDEEFF -iv 0102030405060708090A0B0C0D0E0F10 #CBC encrypt the body.bin file. 
    cat header.bin encrypted_body.bin > partially_encrypted.bmp #Link header file into encrypted body file.
    ```
    - After encryption:<br>
 <img width="750" alt="Screenshot" src="https://github.com/DominateProbabilitywithconcent/InsecLabs/blob/main/SecLabImages/EnLargeMSG/Screenshot%202024-07-18%20140657.png?raw=true"><br>
    => The picture comprises of random colored pixels with no noticable shapes or patterns. There are also no objects, texts or elements presented in the picture. It's impossible to get any useful information just from observing this picture with human eyes.
# 4.3. Encryption Mode -Corrupted Cipher Test
 
 4.3.1. Creating a 64 bytes long file.<br>
 - Create a new text file named __"64long.txt"__ which is 96 bytes (> 64 bytes).<br>
    ```sh
    cat > 64long.txt
    This is a 64 bytes long text file. Only purpose is for testing and other related scenarios. End
      # [Enter] then [Ctrl + D] to save file.
   ```

    <img width="550" alt="Screenshot" src="https://github.com/DominateProbabilitywithconcent/InsecLabs/blob/main/SecLabImages/EnLargeMSG/Screenshot%202024-07-18%20173743.png?raw=true"><br>
    <img width="550" alt="Screenshot" src="https://github.com/DominateProbabilitywithconcent/InsecLabs/blob/main/SecLabImages/EnLargeMSG/Screenshot%202024-07-18%20174229.png?raw=true"><br>
 
 4.3.2. Encryption.<br>
 - Using EBC with key [K]. Named __ECB64long.txt__<br>
    ```sh
    openssl enc -aes-256-ecb -nosalt -in 64long.txt -out ECB64long.txt -K 00112233445566778899AABBCCDDEEFF00112233445566778899AABBCCDDEEFF
    ```
- Using CBC with [K] and initialization vector [IV]. Named __CBC64long.txt__<br>
    ```sh
    openssl enc -aes-256-cbc -nosalt -in 64long.txt -out CBC64long.txt -K
    00112233445566778899AABBCCDDEEFF00112233445566778899AABBCCDDEEFF -iv 0102030405060708090A0B0C0D0E0F10
    ```
    <img width="500" alt="Screenshot" src="https://github.com/DominateProbabilitywithconcent/InsecLabs/blob/main/SecLabImages/EnLargeMSG/Screenshot%202024-07-18%20175423.png?raw=true"><br>

 4.3.3. Alter the encrypted file.<br>
 - We will modify the 5th byte of the encrypted file using dd command.
 - Alter ECB64long.txt.
    ```sh
    printf '\\x01' | dd of=ECB64long.txt bs=1 seek=5 conv=notrunc #modify the 5th character of this file into "\\01'

    ```
    <img width="500" alt="Screenshot" src="https://github.com/DominateProbabilitywithconcent/InsecLabs/blob/main/SecLabImages/EnLargeMSG/Screenshot%202024-07-18%20181005.png?raw=true"><br>
 - Alter CBC64long.txt.
    ```sh
    printf '\\x01' | dd of=CBC64long.txt bs=1 seek=5 conv=notrunc #modify the 5th character of this file into "\\01'
    ```
    <img width="500" alt="Screenshot" src="https://github.com/DominateProbabilitywithconcent/InsecLabs/blob/main/SecLabImages/EnLargeMSG/Screenshot%202024-07-18%20181010.png?raw=true"><br>

 4.3.4. Decrypt and check information of decrypted files.<br>
 - Assume we decrypt the file(s) using ECB, CBC respectively. This below is the recovery expectation for each of them:
    - ECB: only the block that has corrupted 5th byte, the rest is still intact. This is because each block is encrypted seperately. One got corrupted simply does not affect other blocks.
    - CBC: only blocks that are previous to the block that has corrupted 5th bytes are safe (in this case is nothing). The rest is corrupted. This is because each block in CBC mode is encrypted dependently and respectively. One error on a previous block will cause subsequent blocks trouble encrypting the actual message.
 - ECB decrypting process:
    - Decrypt with ECB mode using key [K]:
    ```sh
    openssl enc -d -aes-256-ecb -nosalt -in ECB64long.txt -out ecb_decrypted_64long.txt -K 00112233445566778899AABBCCDDEEFF00112233445566778899AABBCCDDEEFF
    ```
    - Result:<br>
    <img width="500" alt="Screenshot" src="https://github.com/DominateProbabilitywithconcent/InsecLabs/blob/main/SecLabImages/EnLargeMSG/Screenshot%202024-07-18%20182550.png?raw=true"><br>
    => Only the first block that contains corrupted 5th byte is altered. The rest is safe.
 - CBC decrypting process:<br>
    - Decrypt with CBC mode using key [K] and [IV]:
    ```sh
    openssl enc -d -aes-256-cbc -nosalt -in CBC64long.txt -out cbc_decrypted_64long.txt -K 00112233445566778899AABBCCDDEEFF00112233445566778899AABBCCDDEEFF -iv 0102030405060708090A0B0C0D0E0F10
    ```
    - Result:<br>
    <img width="500" alt="Screenshot" src="https://github.com/DominateProbabilitywithconcent/InsecLabs/blob/main/SecLabImages/EnLargeMSG/Screenshot%202024-07-18%20183352.png?raw=true"><br>
    => Whole message is corrupted.<br>

    **Conclusion**:<br>
        1. CBC due to its complexity in encrypting, has create a flaw where if the message is under any unauthorized modification, depend on the position of the modified byte, the original information will cease to exist. But it is still a good mode to protect the integrity of the message.<br>
        2. ECB due to its simplicity in encrypting, is actually good for recovering original message in case any unauthorized modification happens. But it does not privde any significant protection to message integrity.
 

