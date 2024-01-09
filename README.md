# fortnox-api-client

**FortnoxApiClient** is a third party [Fortnox API](https://apps.fortnox.se/apidocs) client for NodeJS. It is a wrapper around an API client that has been [automatically generated](https://www.npmjs.com/package/swagger-typescript-api) using the [OpenAPI schema](https://apps.fortnox.se/api/apidocs/swagger-v1) provided by Fortnox.

## Installation

Add to project's package.json:

```
npm install @rantalainen/fortnox-api-client
```

### Import

```javascript
import { FortnoxApiClient } from '@rantalainen/fortnox-api-client';
```

## Authentication/Authorization

One must authorize integration to use company data and to acquire accessToken and refreshToken for Fortnox API. This API client provides static helper functions to achieve the tokens.

In order to obtain your client id, client secret and redirect uri please see Fortnox documentation.

### Generate Authorization URI

Please see [Fortnox Authorization Code docs](https://www.fortnox.se/developer/authorization/get-authorization-code) for more information on the topic.

```javascript
const authorizationUri = FortnoxApiClient.createAuthorizationUri(
  'your client id',
  'your integration redirect uri',

  // required scope
  ['companyinformation'],

  // state that should match while getting tokens in the next step
  'state',

  // type of authorization
  'service'
);
```

### Fetch access token and refresh token

```javascript
// Authorization Code acquired from previous step
const code = 'xxxx';

// State acquired from previous step
const state = 'xxxx';

// Validate state
if (state !== expectedState) {
  // Something wrong with authorization
}

const tokens = await FortnoxApiClient.getTokensByAuthorizationCode(
  'your client id',
  'your client secret',
  'your integration redirect uri'
  code
);

// tokens.accessToken = 1hour token to use Fortnox API
// tokens.refreshToken = 45day long lived token to refresh access token when necessary
```

## Setup client with options

```javascript
const fortnox = new FortnoxApiClient({
  clientId: 'your client id',
  clientSecret: 'your client secret',

  // To use endpoints you need to provide either accessToken or refreshToken to the client
  // If refreshToken is provided, the accessToken will be acquired on first request and refreshToken will also refresh
  // To acquire these tokens see above information about authentication/authorization
  accessToken: 'token for Fortnox API',
  refreshToken: 'token for Fortnox API'
});
```

Example usage after initialization:

```javascript
const company = await fortnox.api.companyinformation.getCompanyInformationResource();

console.log(company);
```

Available methods can be found in the [API documentation](https://apps.fortnox.se/apidocs).

## Resources

- Fortnox: https://www.fortnox.se/
- Fortnox API Guide: https://apps.fortnox.se/apidocs
- Fortnox Authorization Guide: https://www.fortnox.se/developer/authorization
- Fortnox API scopes: https://www.fortnox.se/developer/guides-and-good-to-know/scopes
