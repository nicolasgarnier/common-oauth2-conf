# OAuth 2.0 Providers

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
 - Which OAuth 2.0 specification draft versions are supported (For instance Google supports [draft 10](https://tools.ietf.org/html/draft-ietf-oauth-v2-10) and draft 22 and above up to [final spec](https://tools.ietf.org/html/rfc6749). Some providers might still be based only on old specs.) This is purely indicative as OAuth 2.0 spec is loose clients should rely on other attributes listed to accomodate for the behavior of a particular implementation.
 - How to pass the access token to the resource server: HTTP Header (which one? What format?), Query Parameter (Which one?)
 - Supported additional Query parameters that might modify the auth flow behaviour (e.g. Force approval prompt, user hint) with a description for each (Note: common non-spec query parameters should be standardized).
 - Supported Formats for exchanging auth code, refreshing tokens etc... (JSON, Form URL-encoded)
 - Supported OAuth flows. Common flows described by the OAuth 2.0 specs are: implicit, client credentials, authorization code and resource owner password). There can be other custom flows supported by the provider as the protocol allows for extensibility. We'll also describe additional/custom flows if there are any.
 - Supports refresh tokens? Uses an Access Token that never expires in time?
 - List of supported scopes and a description of each scopes
 - App registration URL (Where do I create new client credentials?)
 - Additional human readable notes about the provider. For instance How to create an app's credential.
 - List of Rest APIs URI endpoints accessible for each scopes


## Updating / Contributing

If you'd like to add an OAuth provider or add/modify information about an OAuth provider please submit a GitHub Pull Request (prefered) or [file an issue](https://github.com/nicolasgarnier/oauth-providers/issues)


## Format

Each OAuth 2.0 providers will have its specific folder for instance `google/`, `facebook/`, `github/`...

Under each of these folders the following files may be present:
 - `config.json`: Information about the OAuth 2.0 provider configuration and supported features in JSON
 - `endpoints.json`: List of APIs endpoints that you can access for each given scopes.
 - `README.md`: Human readable information about the OAuth 2.0 Provider and some of its specificities. For instance you may describe custom flows here.

Below is a descritpion of the format of each of these files:

### File format for `config.json`

`config.json` is a JSON based file with the following example attributes and definition:

```json
{
  "provider_name": "Google", # Name of the OAuth 2.0 Provider
  "provider_logo_url": "" # URL to a logo of the Provider
}
```
