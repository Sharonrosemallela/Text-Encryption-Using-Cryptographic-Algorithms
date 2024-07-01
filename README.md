# Text-Encryption-Using-Cryptographic-Algorithms
To build a simple web application for encrypting and decrypting textual information securely, we can use JavaScript along with the Web Crypto API, which provides a robust set of cryptographic functions directly in the browser. Here’s a step-by-step guide to creating such an application:

Step 1: Setting Up the HTML Structure
Create an HTML file (index.html) with the following structure
<!DOCTYPE html>
<html lang="en">
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Text Encryption and Decryption
    <h1>Text Encryption and Decryption</h1>
    
    <label for="inputText">Enter text to encrypt/decrypt:</label><br>
    <textarea id="inputText" rows="4" cols="50"></textarea><br><br>
    
    <button onclick="encryptText()">Encrypt</button>
    <button onclick="decryptText()">Decrypt</button><br><br>
    
    <label for="outputText">Result:</label><br>
    <textarea id="outputText" rows="4" cols="50" readonly></textarea>
    
    <script src="script.js"></script>
Step 2: Implementing Encryption and Decryption in JavaScript
Create a JavaScript file (script.js) and implement the encryption and decryption functions using the Web Crypto API:
async function encryptText() {
    try {
        const plainText = document.getElementById('inputText').value;
        const textEncoder = new TextEncoder();
        const encodedText = textEncoder.encode(plainText);
        
        const key = await window.crypto.subtle.generateKey(
            {
                name: 'AES-GCM',
                length: 256,
            },
            true,
            ['encrypt', 'decrypt']
        );
        
        const iv = window.crypto.getRandomValues(new Uint8Array(12));
        
        const encryptedData = await window.crypto.subtle.encrypt(
            {
                name: 'AES-GCM',
                iv: iv,
            },
            key,
            encodedText
        );
        
        const encryptedArray = new Uint8Array(encryptedData);
        const encryptedText = btoa(String.fromCharCode(...encryptedArray));
        
        document.getElementById('outputText').value = encryptedText;
    } catch (error) {
        console.error('Encryption error:', error);
    }
}

async function decryptText() {
    try {
        const encryptedText = document.getElementById('inputText').value;
        const encryptedArray = Uint8Array.from(atob(encryptedText), c => c.charCodeAt(0));
        
        const key = await window.crypto.subtle.generateKey(
            {
                name: 'AES-GCM',
                length: 256,
            },
            true,
            ['encrypt', 'decrypt']
        );
        
        const iv = window.crypto.getRandomValues(new Uint8Array(12));
        
        const decryptedData = await window.crypto.subtle.decrypt(
            {
                name: 'AES-GCM',
                iv: iv,
            },
            key,
            encryptedArray
        );
        
        const decryptedText = new TextDecoder().decode(decryptedData);
        document.getElementById('outputText').value = decryptedText;
    } catch (error) {
        console.error('Decryption error:', error);
    }
}
Step 3: How It Works
Encryption (encryptText):

Reads plaintext from the input textarea.
Converts plaintext to an array of bytes (Uint8Array).
Generates a new AES-GCM encryption key.
Generates a random initialization vector (iv).
Encrypts the plaintext using AES-GCM.
Converts the encrypted bytes to Base64 and displays the result in the output textarea.
Decryption (decryptText):

Reads the encrypted Base64 text from the input textarea.
Ensure your HTML file (index.html) and JavaScript file (script.js) are in the same directory.
Open index.html in a web browser that supports the Web Crypto API (most modern browsers do).
This setup provides a straightforward way to encrypt and decrypt text securely using client-side encryption techniques. Remember, for production use or handling sensitive information, consider additional security measures and potentially server-side encryption.





