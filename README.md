# Mobile App Secure Development

Prevent unauthorized use of privileged accounts. According to various online reports, 80% of breaches involve privileged credentials.

## Paradigm Shift

1. Use of Managed Digital Identity Services
2. Use of Managed Data Services

Insecure Implementation => Security Misconfigurations

## Objective 

1. Protect digital identities
2. Minimize attack surfaces

## Areas to focus on as a (secure) developer? 

* Authentication: No more coding errors. Only security misconfigurations.
* Hardcoded Secrets
* Missing Server-Side Validations
* Improper Error Handling

## OWASP Mobile Top 10 (2016)

* M1: Improper Platform Usage
* M2: Insecure Data Storage
* M3: Insecure Communication
* M4: Insecure Authentication
* M5: Insufficient Cryptography
* M6: Insecure Authorization
* M7: Client Code Quality
* M8: Code Tampering
* M9: Reverse Engineering
* M10: Extraneous Functionality

## Things Todo

**Mobile App Authentication**

* Enforce strong password policy
* Implement API rate limiting
* Do not include sensitive data in JWT tokens
* Terminate session on the server side and delete session information stored in the mobile app after the session times out or the user logs out

**Network Communication**



## Hands-On

1. **Test for insecure keystore usage**
   1. Insecure keystore type in use - AndroidKeystore supports hardware-backed containers and should be preferred.
   2. Key not invalidated on new fingerprint enrollment
   3. Keystore accessible without screen unlock
   4. Weak cryptography algorithms in use
   5. Weak/hardcoded password for keystore or keystore entry - The AndroidKeystore is the recommended keystore type but if the application design requires the usage of a software backed keystore then setting a strong user-derived password is advised.

    **Threat:**
    Android Keystore is considered secure as we cannot access key material. However, an attacker might not actually need the key contents. The Keystore API could be used to retrieve key references, which could be used to initialize the Cipher object and then they could be used to decrypt or encrypt application storage.

    As an attacker with physical access to the device or a privileged malware can:
    1. Start the victim application
    2. Hook the victim application using Frida to execute code within context of the victim application which will do following:
       * Retrieve reference to the AndroidKeystore key using Keystore API.
       * Initialize the Cipher object with the retrieved key reference.
       * Decrypt/Encrypt/Sign data within application storage.

    **Recommendation:**
    Developers must mark the keystore keys as accessible only after:
    1. The device has been unlocked.
    2. Fingerprint or other biometrics have been validated.

    Set `setUserAuthenticationRequired()` to true during key generation. The other important property is `setUserAuthenticationValidityDurationSeconds()`. If it is set to -1 then the key can only be unlocked using Fingerprint or Biometrics. If it is set to any other value, the key can be unlocked using a device screenlock too. For highly sensitive applications like banking apps, password managers or secure messengers setUserAuthenticationValidityDurationSeconds() should not have any value other than -1.

    **Implementation of Secure Local Authentication:**
    Use **AndroidKeystore**.
    1. Create the Android keystore key with `setUserAuthenticationRequired` and `setInvalidatedByBiometricEnrollment` set to true. Additionally, `setUserAuthenticationValidityDurationSeconds` should be set to -1.
    2. Initialize cipher object with keystore key created above.
    3. Create `BiometricPrompt.CryptoObject` using cipher object from previous step.
    4. Implement `BiometricPrompt.AuthenticationCallback.onAuthenticationSucceeded` callback which will retrieve cipher object from the parameter and USE this cipher object to decrypt some other crucial data such as session key, or a secondary symmetric key which will be used to decrypt application data.
    5. Call `BiometricPrompt.authenticate` function with crypto object and callbacks created in steps 3 and 4.

    Refer - https://github.com/android/security-samples

    **Example:**
    **Transaction signing** requires authentication of the user's approval of critical transactions. Asymmetric cryptography is the best way to implement transaction signing. The app will generate a public/private key pair when the user signs up, then registers the public key on the backend. The private key is securely stored in the KeyStore (Android) or KeyChain (iOS). To authorize a transaction, the backend sends the mobile app a push notification containing the transaction data. The user is then asked to confirm or deny the transaction. After confirmation, the user is prompted to unlock the Keychain (by entering the PIN or fingerprint), and the data is signed with user's private key. The signed transaction is then sent to the server, which verifies the signature with the user's public key.

# References

* https://github.com/riddhi-shree/knowledge-sharing/blob/master/Mobile/Android/README.md
* https://slides.com/riddhishreechaurasia/breaking-an-android-app-in-7-steps/#/4/4
* https://github.com/appsecco/VyAPI
* https://github.com/OWASP/MSTG-Hacking-Playground
* https://github.com/OWASP/owasp-mstg/tree/master/Crackmes
* https://github.com/nullblr/flutter/blob/master/nullMobileApp/README.md
* https://labs.f-secure.com/blog/how-secure-is-your-android-keystore-authentication/
* https://github.com/FSecureLABS/android-keystore-audit/tree/master/frida-scripts
* https://github.com/FSecureLABS/android-keystore-audit/tree/master/keystorecrypto-app
* https://github.com/android/security-samples
* https://cypherpunk.nl/papers/spsm14.pdf
* https://doridori.github.io/android-security-the-forgetful-keystore/#sthash.GLFXjBai.dpbs
* https://owasp.org/www-project-mobile-top-10/
* https://mobile-security.gitbook.io/mobile-security-testing-guide/general-mobile-app-testing-guide/0x04e-testing-authentication-and-session-management
* https://www.sjoerdlangkemper.nl/2016/09/28/attacking-jwt-authentication/
* https://github.com/Sjord/jwtdemo/
* https://portswigger.net/bappstore/82d6c60490b540369d6d5d01822bdf61
* https://portswigger.net/bappstore/f923cbf91698420890354c1d8958fee6
* https://github.com/jmaxxz/jwtbrute
* https://github.com/Sjord/jwtcrack
* https://tools.ietf.org/html/rfc6749
* https://tools.ietf.org/html/draft-ietf-oauth-native-apps-12
* https://tools.ietf.org/html/rfc6819
* https://portswigger.net/burp
* https://www.zaproxy.org/
* https://www.charlesproxy.com/