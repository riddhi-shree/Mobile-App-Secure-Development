# M4: Insecure Authentication

>Paradigm Shift: **Security Misconfiguration**

## Plug-And-Play Examples

* Flutter login UI with Amazon Cognito authentication and authorization services implementation

    ![Flutter (Dart) + Cognito](../images/flutter/2a_flutter_cognito_login.png)

* Android (Java) app with Amazon Cognito hosted UI, authentication and authorization services implementation

    ![Android (Java) + Cognito](../images/flutter/2b_java_cognito.png)

* Flutter app with Auth0 authentication and authorization services implementation
  
    ![Flutter (Dart) + Auth0](../images/flutter/2c_flutter_auth0_login.png)

## A Short Implementation Story

*"The Amplify Command Line Interface (CLI) is a unified toolchain to create AWS cloud services for your app."*

![](../images/amazon_cognito/0_authenticationWithAmplify.png)

1. Initialize **AWS Amplify**

    ![amplify init](../images/amazon_cognito/1_amplify_init.png)
    ![amplify init: success](../images/amazon_cognito/2_amplify_init_success.png)
    ![amplifyconfiguration](../images/amazon_cognito/3_amplifyconfiguration.png)

    Note: An empty **configuration file** is created locally.

2. Create an **authentication service**: `amplify add auth`

    ![amplifyconfiguration](../images/amazon_cognito/4_add_auth.png)

    ![amplifyconfiguration](../images/amazon_cognito/4b_add_auth.png)

3. **Deploy** the authentication service: `amplify push`

    ![amplifyconfiguration](../images/amazon_cognito/4c_deploy_auth.png)

4. View the deployed authentication service in **Amplify Console**

    ![Amplify Console](../images/amazon_cognito/4d_amplify_console.png)
    ![User pool](../images/amazon_cognito/5_user_pool.png)

5. At this stage, make sure you understand the **user authentication security requirements** and then choose the desired configurations.

    ![Auth configuration](../images/amazon_cognito/4e_auth_configuration.png)

6. Also, check the contents of `amplifyconfiguration.dart` file. It now contains **sensitive details** associated with the deployed authentication service.

    ![amplifyconfiguration](../images/amazon_cognito/3b_amplifyconfiguration.png)

    **How will you protect the sensitive data stored in this file?**

    ![](../images/amazon_cognito/3c_amplifyconfiguration.png)

    A plausible solution is to pass a JSONObject containing the configuration from the `awsconfiguration.dart` file at runtime.

    ![](../images/misc/1b_JSONObject.png)

## References

* https://docs.amplify.aws/sdk/auth/hosted-ui/q/platform/android
* [How to protect PoolId in awsconfiguration.json?](https://github.com/aws-amplify/aws-sdk-android/issues/711)
* https://aws-amplify.github.io/aws-sdk-android/docs/reference/com/amazonaws/mobile/config/AWSConfiguration.html