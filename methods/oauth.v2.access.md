## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/oauth.v2.access`

[Bolt for Java](/tools/bolt)
`app.client().oauthV2Access`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.oauth_v2_access`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.oauth.v2.access`
[Powered by Bolt](/tools/bolt)

### Content types

`application/x-www-form-urlencoded`

- 
### Rate limits
[Special](/docs/rate-limits#tier_t5)

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

This method allows you to exchange a temporary OAuth `code` for an API access token.

This is the third step of the [V2 OAuth authentication flow](/authentication/oauth-v2). Check out our [guide to new Slack apps](/authentication/basics) for more information.

We strongly recommend supplying the Client ID and Client Secret using the HTTP Basic authentication scheme, as discussed [in RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.3.1).

If at all possible, avoid sending `client_id` and `client_secret` as parameters in your request.

**Keep your tokens secure**. Do not share tokens with users or anyone else.

To learn how to use this method to **refresh** access tokens, read our page on [token rotation](/authentication/rotation).

A potential gotcha: while `redirect_uri` is optional, it is _required_ if your app passed it as a parameter to `oauth/authorization` in the first step of the OAuth flow.

When you use `grant_type=refresh_token` and pass your `refresh_token` as an argument, this method _refreshes_ an access token, whether bot or user. Read our [guide to token rotation](/authentication/rotation) for more information.

Look for the `is_enterprise_install` boolean if you're an [org-wide app](/enterprise/apps) and need to determine whether you've been installed on an entire organization or a single workspace.

## Example responses

### Common successful response

Successful token request with scopes for both a bot user and a user token

```
{
    "ok": true,
    "access_token": "xoxb-17653672481-19874698323-pdFZKVeTuE8sk7oOcBrzbqgy",
    "token_type": "bot",
    "scope": "commands,incoming-webhook",
    "bot_user_id": "U0KRQLJ9H",
    "app_id": "A0KRD7HC3",
    "team": {
        "name": "Slack Softball Team",
        "id": "T9TK3CUKW"
    },
    "enterprise": {
        "name": "slack-sports",
        "id": "E12345678"
    },
    "authed_user": {
        "id": "U1234",
        "scope": "chat:write",
        "access_token": "xoxp-1234",
        "token_type": "user"
    }
}
```

### Alternate response

Successful token request with scopes for both a bot user and a user token, and token rotation enabled

```
{
    "ok": true,
    "access_token": "xoxe.xoxb-1-..",
    "token_type": "bot",
    "scope": "commands,incoming-webhook",
    "bot_user_id": "U0KRQLJ9H",
    "app_id": "A0KRD7HC3",
    "expires_in": 43200,
    "refresh_token": "xoxe-1-...",
    "team": {
        "name": "Slack Softball Team",
        "id": "T9TK3CUKW"
    },
    "enterprise": {
        "name": "slack-sports",
        "id": "E12345678"
    },
    "authed_user": {
        "id": "U1234",
        "scope": "chat:write",
        "access_token": "xoxe.xoxp-1234",
        "expires_in": 43200,
        "refresh_token": "xoxe-1-...",
        "token_type": "user"
    }
}
```

### Alternate response

Successful Sign in with Slack response

```
{
    "ok": true,
    "app_id": "A0118NQPZZC",
    "authed_user": {
        "id": "U065VRX1T0",
        "scope": "identity.basic,identity.email,identity.avatar,identity.team",
        "access_token": "xoxp-yoda-yoda-yoda",
        "token_type": "user"
    },
    "team": {
        "id": "T024BE7LD"
    },
    "enterprise": null,
    "is_enterprise_install": false
}
```

### Common error response

Typical error response

```
{
    "ok": false,
    "error": "invalid_client_id"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `invalid_grant_type` | 
Value passed for `grant_type` was invalid.
 |
| `invalid_client_id` | 
Value passed for `client_id` was invalid.
 |
| `bad_client_secret` | 
Value passed for `client_secret` was invalid.
 |
| `invalid_code` | 
Value passed for `code` was invalid.
 |
| `bad_redirect_uri` | 
Value passed for `redirect_uri` did not match the `redirect_uri` in the original request.
 |
| `oauth_authorization_url_mismatch` | 
The OAuth flow was initiated on an incorrect version of the authorization url. The flow must be initiated via /oauth/v2/authorize .
 |
| `preview_feature_not_available` | 
Returned when the API method is not yet available on the team in context.
 |
| `cannot_install_an_org_installed_app` | 
Returned when the the org-installed app cannot be installed on a workspace.
 |
| `no_scopes` | 
Missing `scope` in the request.
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

