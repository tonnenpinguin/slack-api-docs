## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/openid.connect.token`

[Bolt for Java](/tools/bolt)
`app.client().openidConnectToken`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.openid_connect_token`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.openid.connect.token`
[Powered by Bolt](/tools/bolt)

### Required scopes

### Content types

`application/x-www-form-urlencoded`

- 
### Rate limits
[Tier 4](/docs/rate-limits#tier_t4)

## Arguments

Optional arguments

`client_id`

string

_·_Optional

Issued when you created your application.

**Example**
`4b39e9-752c4`

`client_secret`

string

_·_Optional

Issued when you created your application.

**Example**
`33fea0113f5b1`

`code`

string

_·_Optional

The `code` param returned via the OAuth callback.

**Example**
`ccdaa72ad`

`grant_type`

string

_·_Optional

The `grant_type` param as described in the OAuth spec.

**Example**
`authorization_code`

`redirect_uri`

string

_·_Optional

This must match the originally submitted URI (if one was sent).

**Example**
`http://example.com`

`refresh_token`

string

_·_Optional

The `refresh_token` param as described in the OAuth spec.

**Example**
`xoxe-1-abcdefg`

## Usage info

This special method is part of implementing [Sign in with Slack](/authentication/sign-in-with-slack).

As part of [Sign in with Slack](/authentication/sign-in-with-slack), this method allows your app to receive information about a user who signs into your service with their Slack profile.

A potential gotcha: while `redirect_uri` is optional, it is _required_ if your app passed it as a parameter to `/openid/connect/authorize` in the first step of the Sign in with Slack flow.

The `id_token` in the response is a [standard](https://openid.net/specs/openid-connect-core-1_0.html#TokenResponse) JSON Web Token (JWT). . When it's decoded, you'll see a payload like:

```
"iss": "https://slack.com",
  "sub": "U0R7MFMJM",
  "aud": "25259531569.11152291",
  "exp": 1626874955,
  "iat": 1626874655,
  "auth_time": 1626874655,
  "nonce": "abcd",
  "at_hash": "tUbyWGBHe0V32FJEupkgVQ",
  "https://slack.com/team_id": "T0RR",
  "https://slack.com/user_id": "U0JM",
  "email": "bront@slack-corp.com",
  "email_verified": true,
  "date_email_verified": 1622128723,
  "locale": "en-US",
  "name": "brent",
  "given_name": "",
  "family_name": "",
  "https://slack.com/user_image_24": "https://secure.gravatar.com/avatar/bc.png",
  "https://slack.com/user_image_32": "...",
  "https://slack.com/user_image_48": "...",
  "https://slack.com/user_image_72": "...",
  "https://slack.com/user_image_192": "...",
  "https://slack.com/user_image_512": "...",
  "https://slack.com/team_image_34": "...",
  "https://slack.com/team_image_44": "...",
  "https://slack.com/team_image_68": "...",
  "https://slack.com/team_image_88": "...",
  "https://slack.com/team_image_102": "...",
  "https://slack.com/team_image_132": "...",
  "https://slack.com/team_image_230": "...",
  "https://slack.com/team_image_default": true
```

`iss`, `sub`, `aud`, `exp`, `iat`, `auth_time`, `nonce`, and `at_hash` are each defined by the [OpenID standard](https://openid.net/specs/openid-connect-core-1_0.html#TokenResponse), but here's an overview:

- `iss` signifies the issuer of the token.
- `sub` signifies the subject of the token.
- `aud` signifies the intended audience of the token, the client ID of the OpenID Relying Party.
- `exp` signifies the expiration time of the request, meaning that it shouldn't be trusted if it's not received by the expiration time.
- `iat` signifies the time when the token was issued.
- `auth_time` signifies the time when the end-user authenticated.
- `nonce` is a state variable that you pass to the `/openid/connect/authorize` endpoint at the beginning of Sign in with Slack, and that Slack then returns to you at the end of the flow here. Verify that it matches the `nonce` you passed to `/authorize`.

## Example responses

### Common successful response

Successful token request during the Sign in with Slack flow

```
{
    "ok": true,
    "access_token": "xoxp-1234",
    "token_type": "Bearer",
    "id_token": "eyJhbGcMjY5OTA2MzcWNrLmNvbVwvdGVhbV9p..."
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `invalid_grant_type` | 
The value passed for `grant_type` was invalid.
 |
| `invalid_client_id` | 
The value passed for `client_id` was invalid.
 |
| `bad_client_secret` | 
The value passed for `client_secret` was invalid.
 |
| `invalid_code` | 
The value passed for `code` was invalid.
 |
| `bad_redirect_uri` | 
The value passed for `redirect_uri` did not match the `redirect_uri` in the original request.
 |
| `oauth_authorization_url_mismatch` | 
The OAuth flow was initiated on an incorrect version of the authorization URL. The flow must be initiated via /openid/connect/authorize .
 |
| `preview_feature_not_available` | 
The API method is not yet available on the team.
 |
| `cannot_install_an_org_installed_app` | 
An org-installed app cannot be installed on a workspace.
 |
| `invalid_refresh_token` | 
The given refresh token is invalid.
 |
| `invalid_arguments` | 
The method was either called with invalid arguments or some detail about the arguments passed are invalid, which is more likely when using complex arguments like blocks or attachments.
 |
| `invalid_arg_name` | 
The method was passed an argument whose name falls outside the bounds of accepted or expected values. This includes very long names and names with non-alphanumeric characters other than `_`. If you get this error, it is typically an indication that you have made a _very_ malformed API call.
 |
| `invalid_array_arg` | 
The method was passed an array as an argument. Please only input valid strings.
 |
| `invalid_charset` | 
The method was called via a `POST` request, but the `charset` specified in the `Content-Type` header was invalid. Valid charset names are: `utf-8` `iso-8859-1`.
 |
| `invalid_form_data` | 
The method was called via a `POST` request with `Content-Type` `application/x-www-form-urlencoded` or `multipart/form-data`, but the form data was either missing or syntactically invalid.
 |
| `invalid_post_type` | 
The method was called via a `POST` request, but the specified `Content-Type` was invalid. Valid types are: `application/json` `application/x-www-form-urlencoded` `multipart/form-data` `text/plain`.
 |
| `missing_post_type` | 
The method was called via a `POST` request and included a data payload, but the request did not include a `Content-Type` header.
 |
| `team_added_to_org` | 
The workspace associated with your request is currently undergoing migration to an Enterprise Organization. Web API and other platform operations will be intermittently unavailable until the transition is complete.
 |
| `ratelimited` | 
The request has been ratelimited. Refer to the `Retry-After` header for when to retry the request.
 |
| `accesslimited` | 
Access to this method is limited on the current network
 |
| `request_timeout` | 
The method was called via a `POST` request, but the `POST` data was either missing or truncated.
 |
| `service_unavailable` | 
The service is temporarily unavailable
 |
| `fatal_error` | 
The server could not complete your operation(s) without encountering a catastrophic error. It's possible some aspect of the operation succeeded before the error was raised.
 |
| `internal_error` | 
The server could not complete your operation(s) without encountering an error, likely due to a transient issue on our end. It's possible some aspect of the operation succeeded before the error was raised.
 |

## Warnings

This table lists the expected warnings that this method will return. However, other warnings can be returned in the case where the service is experiencing unexpected trouble.

| Warning | Description |
| --- | --- |
| `missing_charset` | 
The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended.
 |
| `superfluous_charset` | 
The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous.
 |

