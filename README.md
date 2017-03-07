# Common OAuth 2.0 Providers Configuration

This repo lists OAuth 2.0 APIs providers and provides information about their OAuth 2.0 implementations in a machine readable format. 


## Goals

We're hoping that Tools using or depending on OAuth 2.0 will be able to simply use these configuration files instead of having users/developers manually provide the OAuth 2.0 provider settings, be able to automatically configure themselves to work with the particularities of OAuth 2.0 providers and potentially avoid having to make assumptions about particular OAuth 2.0 implementations.

One of the goals of these documents and configuration files is to identify differences and specificities of each OAuth 2.0 implementations and try to find a way to abstract and generalize these differences to potentially allow for fully generic OAuth 2.0 clients.


## Information about OAuth 2.0 providers

We aim at providing the following information about OAuth 2.0 providers:
 - Provider name
 - Provider logo URL
 - Authorization endpoint URL
 - Token endpoint URL
 - Other non-spec endpoint URLs and a descriptions (like a token information endpoint for instance) 
 - Which OAuth 2.0 specification draft versions are supported (For instance Google supports [draft 10](https://tools.ietf.org/html/draft-ietf-oauth-v2-10) and draft 22 and above up to [final spec](https://tools.ietf.org/html/rfc6749). Some providers might still be based only on old specs.) This is purely indicative as OAuth 2.0 spec is loose clients should rely on other attributes listed to accomodate for the behavior of a particular implementation.
 - Supported additional Query parameters that might modify the auth flow behaviour (e.g. Force approval prompt, user hint) with a description for each (Note: common non-spec query parameters should be standardized).
 - Supported Formats for exchanging auth code, refreshing tokens etc... (JSON, Form URL-encoded)
 - Supported OAuth flows. Common flows described by the OAuth 2.0 specs are: implicit, client credentials, authorization code and resource owner password). There can be other custom flows supported by the provider as the protocol allows for extensibility. We'll also describe additional/custom flows if there are any.
 - Supports refresh tokens? Uses an Access Token that never expires in time?
 - List of supported scopes and a description of each scopes
 - App registration URL (Where do I create new client credentials?)
 - Additional human readable notes about the provider. For instance How to create an app's credential, description of additional custom flows, description of non-spec features.
 - List of Rest APIs URI endpoints (Resource servers URIs) accessible for each scopes
 - How to pass the access token to the resource server: HTTP Header (which one? What format?), Query Parameter (Which one?)


## Updating / Contributing

If you'd like to add an OAuth provider or add/modify information about an OAuth provider please submit a GitHub Pull Request (prefered) or [file an issue](https://github.com/nicolasgarnier/oauth-providers/issues).

We're happy even if you can only contribute partial/incomplete data. For instance only providing the Authorization and Token endpoints URLs is a good start.


## Format

Each OAuth 2.0 providers will have its specific folder for instance `google/`, `facebook/`, `github/`...

Under each of these folders the following files may be present:
 - `config.json`: Information about the OAuth 2.0 provider configuration and supported features in JSON
 - `endpoints.json`: List of APIs endpoints that you can access for each given scopes.
 - `README.md`: Human readable information about the OAuth 2.0 Provider and some of its specificities. For instance you may describe custom flows here.

The specification of the information provided for each OAuth 2.0 providers is not set in stone and reasonable efforts will be made to accomodate unforseen requirements and specificities of OAuth implementations as long as they can be generalized and especially if they can be applied to the whole corpus.

Below is a descritpion of the format of each of the files listed above:

### File format for `config.json`

`config.json` is a JSON based file containing information about the implementation and supported features of the OAuth provider. Except for `provider_name` and `authorization_endpoint.url` all attributes are optionnal. Check the [JSON Schema for the `config.json`](config.schema.json) file for a formal description or simply have a look at the examples below.

Here is an example of a rather minimal `config.json` file:

```javascript
{
  "provider_name": "GitHub", // Name of the OAuth 2.0 Provider
  "authorization_endpoint": {
    "url": "https://github.com/login/oauth/authorize" // URL of the Authroization endpoint
  },
  "token_endpoint": {
    "url": "https://github.com/login/oauth/access_token" // URL to exchange the auth code
  },
  "scopes": { // Available scopes with a description
    "": "Grants read-only access to public information (includes public user profile info, public repository info, and gists)", // Some providers allow an empty scope.
    "user": "Grants read/write access to profile info only. Note that this scope includes user:email and user:follow.",
    ...
  },
  "supported_oauth_flow": ["authorization_code"],
  "app_registration_url": "https://github.com/settings/applications"
}
```

Here is an example of a `config.json` file for an almost exhaustively developed OAuth 2.0 implementation (Google):

```javascript
{
  "provider_name": "Google", // Name of the OAuth 2.0 Provider
  "provider_logo_url": "https://...", // URL to a logo of the Provider
  "authorization_endpoint": {
    "url": "https://accounts.google.com/o/oauth2/auth", // URL of the Authroization endpoint
    "query_parameters": {
      "approval_prompt": {"allowed_values": ["force"], "description": "Forces display of the approval prompt. Avoids pass-through if the user has already granted access."},
      "access_type": {"allowed_values": ["offline, online"], "description": "If the value is 'offline' Will grant a refresh token in the authorization code flow. 'online' will only grant you an access token in the authorization code flow."}
      ...
    },
  },
  "token_endpoint": {
    "url": "https://www.googleapis.com/oauth2/v3/token", // URL to exchange the auth code 
    "supported_methods": ["POST"], // Supported HTTP methods
    "prefered_request_content_type": "application/x-www-form-urlencoded", // Prefered MIME type of the data that can be passed in the body of requests to the token endpoint.
    "default_response_content_type": "application/json" // MIME type of the data that is returned by the token endpoint by default.
  },
  "additional_endpoints": { // Other endpoints that are related to OAuth
    "https://www.googleapis.com/oauth2/v3/token": {"supported_methods": "GET", "description": "Returns the information about the access token. You have to provide an Access Token as a query parameter"}
  },
  "oauth2_version": ["draft_10", "draft_22", "final"],
  "scopes": { // Available scopes with a description
    "https://spreadsheets.google.com/feeds/": "Manage your spreadsheets."
    ...
  },
  "jwt_support": {
    "supported_alg": "RS256",
    "headers_parameters": {
      "alg": {"mandatory": true, "allowed_values": ["RS256"]},
      "typ": {"mandatory": true, "allowed_values": ["JWT"]}
    },
    "claim_names": {
      "iss": {"mandatory": true, "description": "The email address of the service account."}, // For registered/public claim names you may add a description.
      "scope" : {"mandatory": true, "description": "A space-delimited list of the permissions that the application requests."}, // For non-spec/private claim names you MUST add a description.
      "aud": {"mandatory": true, "allowed_values": ["https://www.googleapis.com/oauth2/v3/token"]},
      "exp": {"mandatory": true},
      "iat": {"mandatory": true},
      "sub": {"mandatory": false, "description": "The email address of the user for which the application is requesting delegated access. This can be used in the case of a Google Apps domain application that has been granted delegated access rights."},
    }
  },
  "supported_oauth_flow": ["implicit", "authorization_code", "jwt_bearer_token", "ext_android", "ext_post_message", "ext_installed_apps", "ext_authorization_code_oob"],
  "app_registration_url": "https://console.developers.google.com"
}
```
