# nodejs-helper-files

## Prerequisities

-   **NodeJS**: v12.20.1 or higher
## Adding nodejs helpers to your project
- In your project directory, create or edit your package.json file to include the SDK as a dependency:

```
{
  "name": "project",
  "version": "0.0.0",
  "private": true,
  "dependencies": {
     "blockid-nodejs-helpers": "https://github.com/1Kosmos/nodejs-helper-files#v2.0.0"
  },
 "description": "My project"
}
```

- Next open a terminal, navigate to your project directory, and execute the following command to install the NodeJS SDK :

```
npm install
```

## How to use
- Know your tenant (BIDTenant) `dns`, `communityName` and `licenseKey`

- Request OTP
```
const BIDOTP = require('blockid-nodejs-helpers/BIDOTP');

let otpResponse = await BIDOTP.requestOTP({ "dns": "<dns>", "communityName": "<communityName>", "lecenseKey": "<lecenseKey>" }, "<username>", "<emailToOrNull>", "<smsToOrNull>", "<smsISDCodeOrNull>");
```

- Verify OTP
```
const BIDOTP = require('blockid-nodejs-helpers/BIDOTP');

let verifyOtpResponse = await BIDOTP.verifyOTP({ "dns": "<dns>", "communityName": "<communityName>", "lecenseKey": "<lecenseKey>" }, "<username>", "<otpcode>");
```

- Create new UWL2.0 session
```
const BIDSessions = require('blockid-nodejs-helpers/BIDSessions');

let createdSessionResponse = await BIDSessions.createNewSession({ "dns": "<dns>", "communityName": "<communityName>", "lecenseKey": "<lecenseKey>" }, "<authType>", "<scopes>");
```

- Poll for UWL2.0 session response
```
const BIDSessions = require('blockid-nodejs-helpers/BIDSessions');

let authSessionRespone = await BIDSessions.pollSession({ "dns": "<dns>", "communityName": "<communityName>", "lecenseKey": "<lecenseKey>" }, "<sessionId>", true, true);
```

- FIDO device registration options
```
const BIDWebAuthn = require('blockid-nodejs-helpers/BIDWebAuthn.js');

//authenticatorSelection

// if your device is a security key, such as a YubiKey:
'attestation': 'direct',
'authenticatorSelection': {
    'requireResidentKey': true
}

// if your device is a platform authenticator, such as TouchID
'attestation': 'direct',
'authenticatorSelection': {
    'authenticatorAttachment': platform
}

// if your device is a MacBook
'attestation': 'none'

let attestationOptionsResponse = await ​BIDWebAuthn.fetchAttestationOptions({ "dns": "<dns>", "communityName": "<communityName>", "lecenseKey": "<lecenseKey>" }, {
    "displayName":"<displayname>",
    "username":"<username>",
    "dns":"<current domain>",
    "attestation":"<attestation>"
    "authenticatorSelection":"<authenticatorSelection>"
})
```

- FIDO device registration result
```
const BIDWebAuthn = require('blockid-nodejs-helpers/BIDWebAuthn.js');

let attestationResultResponse = await BIDWebAuthn.submitAttestationResult({ "dns": "<dns>", "communityName": "<communityName>", "lecenseKey": "<lecenseKey>" }, {
    "rawId": <rawId>,
    "response": {
      "attestationObject": "<attestationObject>",
      "getAuthenticatorData": {},
      "getPublicKey": {},
      "getPublicKeyAlgorithm": {},
      "getTransports": {},
      "clientDataJSON": "<clientDataJSON>"
    },
    "authenticatorAttachment": "<authenticatorAttachment>",
    "getClientExtensionResults": "<getClientExtensionResults>",
    "id": "<id>",
    "type": "<type>",
    "dns": "<current domain>"
})
```

- FIDO device authentication options
```
const BIDWebAuthn = require('blockid-nodejs-helpers/BIDWebAuthn.js');

let assertionOptionsResponse = await BIDWebAuthn.fetchAssertionOptions({ "dns": "<dns>", "communityName": "<communityName>", "lecenseKey": "<lecenseKey>" }, {
  "username": "<username>",
  "displayName": "<displayName>",
  "dns": "<current domain>",
});

```

- FIDO device authentication result
```
const BIDWebAuthn = require('blockid-nodejs-helpers/BIDWebAuthn.js');

let assertionResultResponse = await BIDWebAuthn.submitAssertionResult({ "dns": "<dns>", "communityName": "<communityName>", "lecenseKey": "<lecenseKey>" }, {
    "rawId": "<rawId>",
    "response": {
        "authenticatorData": "<authenticatorData>",
        "signature": "<signature>",
        "userHandle": "<userHandle>",
        "clientDataJSON": "<clientDataJSON>"
    },
    "getClientExtensionResults": "<getClientExtensionResults>",
    "id": "<id>",
    "type": "<type>",
    "dns": "<current domain>"
});
```

- Create new Driver's License verification session
```
const BIDVerifyDocument = require('blockid-nodejs-helpers/BIDVerifyDocument');

let createdSessionResponse = await BIDVerifyDocument.createDocumentSession({ "dns": "<dns>", "communityName": "<communityName>", "lecenseKey": "<lecenseKey>" }, "<dvcId>", "<documentType>");
    
```

- Trigger SMS 
```
const BIDMessaging = require('blockid-nodejs-helpers/BIDMessaging');

let smsResponse = await BIDMessaging.sendSMS({ "dns": "<dns>", "communityName": "<communityName>", "lecenseKey": "<lecenseKey>" }, "<smsTo>", "<smsISDCode>", "<smsTemplateB64>");
```

- Poll for Driver's License session response
```
const BIDVerifyDocument = require('blockid-nodejs-helpers/BIDVerifyDocument');

let pollSessionResponse = await BIDVerifyDocument.pollSessionResult({ "dns": "<dns>", "communityName": "<communityName>", "lecenseKey": "<lecenseKey>" }, "<dvcId>", "<sessionId>");
```

- Request Email verification link
```
const BIDAccessCodes = require('blockid-nodejs-helpers/BIDAccessCodes');

const requestEmailVerificationResponse = await BIDAccessCodes.requestEmailVerificationLink({ "dns": "<dns>", "communityName": "<communityName>", "lecenseKey": "<lecenseKey>" }, "<emailTo>", "<emailTemplateB64>", "<emailSubject>", "<ttl_seconds>");
```

- Verify and Redeem Email verification link
```
const BIDAccessCodes = require('blockid-nodejs-helpers/BIDAccessCodes');

let redeemVerificationCodeResponse = await BIDAccessCodes.verifyAndRedeemEmailVerificationCode({ "dns": "<dns>", "communityName": "<communityName>", "lecenseKey": "<lecenseKey>" }, "<sessionId>");
```

- Request verifiable credentials for ID
```
const BIDVerifiableCredential = require('blockid-nodejs-helpers/BIDVerifiableCredential.js');
 
// sample vcs object (see {tenant-dns}/vcs/docs for up to date request structure)
// example https://blockid-trial.1kosmos.net/vcs/docs/#/Credentials/post_tenant__tenantId__community__communityId__vc_from_document__type_

let issuedVerifiableCredential = await BIDVerifiableCredential.requestVCForID({ "dns": "<dns>", "communityName": "<communityName>", "licenseKey": "<licenseKey>" }, <type>, <document>, <userDid>, <userPublickey>, <userUrn>);

```

- Verify verifiable credentials

```
const BIDVerifiableCredential = require('blockid-nodejs-helpers/BIDVerifiableCredential.js');

// sample vcs object (see {tenant-dns}/vcs/docs for up to date request structure)
// example https://blockid-trial.1kosmos.net/vcs/docs/#/Credentials/post_tenant__tenantId__community__communityId__vc_verify

const verifiedVCResponse = await BIDVerifiableCredential.verifyCredential({ "dns": "<dns>", "communityName": "<communityName>", "licenseKey": "<licenseKey>" }, <issuedVerifiableCredential>);

```

- Request verifiable presentation

```
const BIDVerifiableCredential = require('blockid-nodejs-helpers/BIDVerifiableCredential.js');

// sample vcs object (see {tenant-dns}/vcs/docs for up to date request structure)
// example https://blockid-trial.1kosmos.net/vcs/docs/#/Credentials/post_tenant__tenantId__community__communityId__vp_create

const vpResponse = await BIDVerifiableCredential.requestVPForCredentials({ "dns": "<dns>", "communityName": "<communityName>", "licenseKey": "<licenseKey>" }, <vcs>);

```

- Verify verifiable presentation

```
const BIDVerifiableCredential = require('blockid-nodejs-helpers/BIDVerifiableCredential.js');

// sample vcs object (see {tenant-dns}/vcs/docs for up to date request structure)
// example https://blockid-trial.1kosmos.net/vcs/docs/#/Credentials/post_tenant__tenantId__community__communityId__vp_verify

let verifiedVP = await BIDVerifiableCredential.verifyPresentation({ "dns": "<dns>", "communityName": "<communityName>", "licenseKey": "<licenseKey>" }, <vp>);

```

- Request verifiable credentials for Payload

```
const BIDVerifiableCredential = require('blockid-nodejs-helpers/BIDVerifiableCredential.js');

// sample vcs object (see {tenant-dns}/vcs/docs for up to date request structure)
// example https://blockid-trial.1kosmos.net/vcs/docs/#/Credentials/post_tenant__tenantId__community__communityId__vc_from_payload__type_

let verifiedVP = await BIDVerifiableCredential.requestVCForPayload({ "dns": "<dns>", "communityName": "<communityName>", "licenseKey": "<licenseKey>" }, <type>, <issuer>, <info>, <userDid>, <userPublickey>, <userUrn>);

```

- Get verifiable credentials status

```
const BIDVerifiableCredential = require('blockid-nodejs-helpers/BIDVerifiableCredential.js');

// sample vcs object (see {tenant-dns}/vcs/docs for up to date request structure)
// example https://blockid-trial.1kosmos.net/vcs/docs/#/Credentials/get_tenant__tenantId__community__communityId__vc__vcId__status

let vcStatus = await BIDVerifiableCredential.getVcStatusById({ "dns": "<dns>", "communityName": "<communityName>", "licenseKey": "<licenseKey>" }, <vcId>);

```

- Request OAuth2 authorization code 

```
const BIDOauth2 = require('blockid-nodejs-helpers/BIDOauth2');

let authorizationCodeResponse = await BIDOauth2.requestAuthorizationCode({ "dns": "<dns>", "communityName": "<communityName>", "licenseKey": "<licenseKey>" }, <proofOfAuthenticationJwt>, <clientId>, <responseType>, <scope>, <redirectUri>, <stateOrNull>, <nonceOrNull>);

```

- Request OAuth2 Tokens

```
const BIDOauth2 = require('blockid-nodejs-helpers/BIDOauth2');

let requestTokenResponse = await BIDOauth2.requestToken({ "dns": "<dns>", "communityName": "<communityName>", "licenseKey": "<licenseKey>" }, <clientId>, <clientSecret>, <grantType>, <redirectUri>, <codeOrNull>, <refreshTokenOrNull>);

```
