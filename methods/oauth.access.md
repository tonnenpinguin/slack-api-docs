## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/oauth.access`

[Bolt for Java](/tools/bolt)
`app.client().oauthAccess`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.oauth_access`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.oauth.access`
[Powered by Bolt](/tools/bolt)

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

`redirect_uri`

string

_·_Optional

This must match the originally submitted URI (if one was sent).

**Example**
`http://example.com`

`single_channel`

boolean

_·_Optional

Request the user to add your app only to a single channel. Only valid with a [legacy workspace app](https://api.slack.com/legacy-workspace-apps).

**Default**
`false`

**Example**
`true`

## Usage info

**This is a legacy method only used by classic Slack apps.** [Use `oauth.v2.access`](/methods/oauth.v2.access) for [new Slack apps](/authentication/basics).

This method allows you to exchange a temporary OAuth `code` for an API access token.

This is the third step of the [OAuth authentication flow](/authentication).

We strongly recommend supplying the Client ID and Client Secret using the HTTP Basic authentication scheme, as discussed [in RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.3.1).

If at all possible, avoid sending `client_id` and `client_secret` as parameters in your request.

**Keep your tokens secure**. Do not share tokens with users or anyone else.

When used with a legacy workspace app, this method's response differs significantly.

A potential gotcha: while `redirect_uri` is optional, it is _required_ if your app passed it as a parameter to `oauth/authorization` in the first step of the OAuth flow.

The response schema for this step of OAuth differs depending on [the scopes](/scopes) requested and the type of application used. When asking for the `bot` scope, you'll receive the token separately from the user token.

`enterprise_id` will be populated if the installing team is part of an enterprise. Otherwise, it will be `null`.

## Example responses

### Common successful response

Successful user token negotiation for a single scope

```
{
    "access_token": "xoxp-XXXXXXXX-XXXXXXXX-XXXXX",
    "scope": "groups:write",
    "team_name": "Wyld Stallyns LLC",
    "team_id": "TXXXXXXXXX",
    "enterprise_id": null
}
```

### Alternate response

Success example when asking for multiple scopes, a bot user token, and an incoming webhook

```
{
    "access_token": "xoxp-XXXXXXXX-XXXXXXXX-XXXXX",
    "scope": "incoming-webhook,commands,bot",
    "team_name": "Team Installing Your Hook",
    "team_id": "TXXXXXXXXX",
    "enterprise_id": null,
    "incoming_webhook": {
        "url": "https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX",
        "channel": "#channel-it-will-post-to",
        "configuration_url": "https://teamname.slack.com/services/BXXXXX"
    },
    "bot": {
        "bot_user_id": "UTTTTTTTTTTR",
        "bot_access_token": "xoxb-XXXXXXXXXXXX-TTTTTTTTTTTTTT"
    }
}
```

### Alternate response

Success example using a workspace app produces a very different kind of response

```
{
    "ok": true,
    "access_token": "xoxa-access-token-string",
    "token_type": "app",
    "app_id": "A012345678",
    "app_user_id": "U0NKHRW57",
    "team_name": "Subarachnoid Workspace",
    "team_id": "T061EG9R6",
    "enterprise_id": null,
    "authorizing_user": {
        "user_id": "U061F7AUR",
        "app_home": "D0PNCRP9N"
    },
    "installer_user": {
        "user_id": "U061F7AUR",
        "app_home": "D0PNCRP9N"
    },
    "scopes": {
        "app_home": [
            "chat:write",
            "im:history",
            "im:read"
        ],
        "team": [],
        "channel": [
            "channels:history",
            "channels:read",
            "chat:write"
        ],
        "group": [
            "chat:write"
        ],
        "mpim": [
            "chat:write"
        ],
        "im": [
            "chat:write"
        ],
        "user": []
    }
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
The OAuth flow was initiated on an incorrect version of the authorization url. The flow must be initiated via /oauth/authorize.
 |
| `code_already_used` | 
Value passed for `code` was already exchanged.
 |
| `missing_resource` | 
Missing permission resource.
 |
| `internal_error` | 
The server could not complete your operation(s) without encountering an error, likely due to a transient issue on our end. It's possible some aspect of the operation succeeded before the error was raised.
 |
| `ratelimited` | 
The request has been ratelimited. Refer to the `Retry-After` header for when to retry the request.
 |
| `invalid_token` | 
Invalid refresh token.
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

