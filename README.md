# OAuth 2.0 Providers

This repo lists OAuth 2.0 APIs providers and gives information about their OAuth 2.0 endpoints in a machine readable format.

The goal is that Tools using or depending on OAuth 2.0 authorization simply use these configuration files instead of having users/developers list these manually and potentiall avoid making assumptions.

## Information about OAuth 2.0 providers

We aim at providing the following information about OAuth 2.0 providers:
 - Provider name
 - Provider logo URL
 - Authorization endpoint URL
 - Token endpoint URL
 - OAuth 2.0 spcification draft versions supported (For instance Google supports draft 10 and draft 22 and above up to final)
 - Supported OAuth flows. Common flows described by the OAuth 2.0 specs are: implicit, client_credentials, authorization_code, owner_password). There may be other supported flows as the protocol allows for extensibility. We'll describe additional/custom flows and refer to them as ext_XXX, ext_YYY etc...
 - Supports refresh tokens? Uses an Access Token that never expires in time?
 - List of supported scopes and a description of the scope
 - Additional human readable notes about the provider. For instance How to create an app's credentials.
 - List of Rest APIs URI endpoints accessible for each scopes

## Updating 

If you'd like to add an OAuth provider or add/modify information about an OAuth provider please submit a Pull Request or [file an issue](https://github.com/nicolasgarnier/oauth-providers/issues)

## Format

Each OAuth 2.0 providers will have its specific folder for instance `google/`, `facebook/`, `github/`...

Under each of these folders the following files may be present:
 - `config.json`: Information about the OAuth 2.0 provider configuration and supported features in JSON
 - `endpoints.json`: List of 
 - `README.md`: Human readable information about the OAuth 2.0 Provider

