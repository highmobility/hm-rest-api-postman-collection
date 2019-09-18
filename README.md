

# hm-rest-api-postman-collection
HM REST API Postman Collection and reference Environment

# Overview

This Collection and reference Environment make it possible to call any of the Auto APIs from POSTMAN.

The Environment needs to be configured for each particular User and Application.  

During the OAuth flow, a code is generated and exchanged for an Access Token. The Access Token is then used in the pre-request script of the HM REST Auto API Collection, which uses it to generate a JWT just before it sends each request.

Before calling one of the APIs, the Collection's Pre-request Script checks to see if the Access Token is getting close to expiring. If so, it sends the Refresh Token and updates the Environment with the new Access Token and the new Refresh Token before calling the API.

## Configuration

Import the Collection and the reference environment into POSTMAN (Collection: HM REST Auto API. Environment: REST API Sandbox Environment).
Update the following six variables in the REST API Sandbox Environment:

The following two variables can be found on the application's page
- **APP_ID**
- **REST_API_CONFIG:** Copy this from the application's page under the Client Certificate –> REST tab 

The other variables can be found under the User's account settings.
- **CLIENT_ID**
- **CLIENT_SECRET**
- **TOKEN_URI**
- **REDIRECT_URI:** for the purposes of this tutorial, use hm-postman.local

The seventh variable is **OAUTH_CODE**, and will be obtained in the next step.

Several more variables will be set automatically in subsequent steps.  These include the following: 

- **ACCESS_TOKEN**
- **ACCESS_TOKEN_EXPIRY**: The lifespan of the Access Token in milliseconds.
- **REFRESH_TOKEN**

## OAuth Flow: Getting an Authorization Code and Access Token

The next step is to get an authorization code and add it to the environment. 

To get the **OAUTH_CODE**, open a web browser and visit the **TOKEN_URI** with the following parameters: **CLIENT_ID**, **CLIENT_SECRET**, and **REDIRECT_URI**.

Here is the format:  ```TOKEN_URI?APP_ID&CLIENT_ID&REDIRECT_URI``` 

**Example:**
  ```https://high-mobility.com/hm_cloud/o/dbdda199a-e9b0-4b7e-8c45-b0ccb8df1cb8/oauth?app_id=APP_ID&client_id=CLIENT_ID&redirect_uri=REDIRECT_URI```

Upon completion of the consent flow, you will be sent to the **REDIRECT_URI** with the **OAUTH_CODE** in the parameters.  

**Example:**
  ```http://hm-postman.local/auth/oauth-callback?code=bc970866-be01-41dc-979a-687d21d506a6&state=&theme=```

Select the code (**bc970866-be01-41dc-979a-687d21d506a6** in the example URI above) and paste it into the **OAUTH_CODE** parameter in your POSTMAN Environment.

Run the Collection's “POST: Code -> Access Token” API call to exchange the **OAUTH_CODE** for the **ACCESS_TOKEN**. The response should be in the following format. The **REFRESH_TOKEN** and **ACCESS_TOKEN** are automatically added to the Environment.

```
{
    "token_type": "bearer",
    "refresh_token": "r311bS_iiIU_ztFdulr0J-BRv3WfYeSbtLhmpMj0xaIoEAnaiT7ZRslAiGJsK7_haGeBhHdXISjE78zpMz3bLZuwE9EgWHFodr5Bzziqle0txpgZELOHKP3maC-xcyrs5w",
    "expires_in": 3600000,
    "access_token": "92NEJ8oMNLpU6uUBZ5wPRJqJR7Yc53WWUWf0MFSJO-kr0z95slJUKhckHMVgFVyKf8fuytFQIhP_9ikrQKp1x0LCKZN0oHkpyiTwZT67ayECw9W6_7o4g_y9i1FpqxfBcQ"
}
```

That’s it! Now you can run the relevant vehicle emulator and start calling the APIs.

## Calling the APIs
Make sure the emulator of the authorized vehicle is running, then start calling the API to send a request to the emulator.

## Troubleshooting
If it is no longer possible to call the APIs, obtain a new **OAUTH_CODE** and update the environment with it, confirm that the variables set in the OAuth section are correct, and re-run the Collection's “POST: Code -> Access Token” API call to exchange the code for the **ACCESS_TOKEN**.


## Questions or Comments ?

If you have questions or if you would like to send us feedback, join our [Slack Channel](https://slack.high-mobility.com/) or email us at [support@high-mobility.com](mailto:support@high-mobility.com).


