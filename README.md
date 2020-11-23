# The Changing Face of Mobile App Security

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

### M1: Improper Platform Usage

Misuse of Touch ID, the Keychain, Android intents, platform permissions, or other security features that are a part of the mobile operating system





### M3: Insecure Communication



### M4: Insecure Authentication - Obsolete?

Paradigm Shift: **Security Misconfiguration**

![](images/amazon_cognito/0_authenticationWithAmplify.png)

*"The Amplify Command Line Interface (CLI) is a unified toolchain to create AWS cloud services for your app."*

1. Initialize **AWS Amplify**

    ![amplify init](images/amazon_cognito/1_amplify_init.png)
    ![amplify init: success](images/amazon_cognito/2_amplify_init_success.png)
    ![amplifyconfiguration](images/amazon_cognito/3_amplifyconfiguration.png)

    Note: An empty **configuration file** is created locally.

2. Create an **authentication service**: `amplify add auth`

    ![amplifyconfiguration](images/amazon_cognito/4_add_auth.png)

    ![amplifyconfiguration](images/amazon_cognito/4b_add_auth.png)

3. **Deploy** the authentication service: `amplify push`

    ![amplifyconfiguration](images/amazon_cognito/4c_deploy_auth.png)

4. View the deployed authentication service in **Amplify Console**

    ![Amplify Console](images/amazon_cognito/4d_amplify_console.png)
    ![User pool](images/amazon_cognito/5_user_pool.png)

5. At this stage, make sure you understand the **user authentication security requirements** and then choose the desired configurations.

    ![](images/amazon_cognito/4e_auth_configuration.png)

6. Also, check the contents of `amplifyconfiguration.dart` file. It now contains sensitive details associated with the deployed authentication service.

    ![amplifyconfiguration](images/amazon_cognito/3b_amplifyconfiguration.png)

    **How would you protect this file?**

    ![](images/amazon_cognito/3c_amplifyconfiguration.png)

### M2: Insecure Data Storage - Obsolete?

Paradigm Shift: **Security Misconfiguration**

1. Create a storage service: `amplify add storage`

    ![amplify add storage](images/amazon_cognito/6_add_storage.png)

2. Make your choices consciously

    ![Configure storage](images/amazon_cognito/6b_configure_storage_service.png)
    ![](images/amazon_cognito/6c_kind_of_access.png)

3. Push local changes to the cloud: `amplify push`

    ![amplify push](images/amazon_cognito/7_amplify_push.png)
    ![](images/amazon_cognito/7b_s3buckets.png)

### M5: Insufficient Cryptography



### M6: Insecure Authorization

git clone git@github.com:appsecco/VyAPI.git

### M7: Client Code Quality



### M8: Code Tampering



### M9: Reverse Engineering

![](images/amazon_cognito/9a_awsconfigurationJSON.png)
![](images/amazon_cognito/9b_amplifyconfigurationJSON.png)
![](images/amazon_cognito/9c_flutter_amplify_configurationFiles.png)

### M10: Extraneous Functionality


## Exercise

1. 

# References

* https://github.com/riddhi-shree/knowledge-sharing/blob/master/Mobile/Android/environment_setup/setup_vyapi/README.md
* https://codifiedsecurity.com/owasp-mobile-top-10-2016-m1-improper-platform-usage/
* https://docs.amplify.aws/cli
* https://docs.amplify.aws/start/getting-started/auth/q/integration/react#create-authentication-service
* https://docs.amplify.aws/start/getting-started/add-api/q/integration/flutter#setup-aws-cloud-resources-with-amplify-cli
* https://github.com/aws-amplify/amplify-flutter