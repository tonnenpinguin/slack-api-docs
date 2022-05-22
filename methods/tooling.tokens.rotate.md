## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`GEThttps://slack.com/api/tooling.tokens.rotate`

[Bolt for Java](/tools/bolt)
`app.client().toolingTokensRotate`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.tooling_tokens_rotate`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.tooling.tokens.rotate`
[Powered by Bolt](/tools/bolt)

### Required scopes

### Content types

`application/x-www-form-urlencoded`

- 
### Rate limits
[Special](/docs/rate-limits#tier_t5)

## Arguments

Required arguments

`refresh_token`

string

_·_Required

The `xoxe` refresh token that was issued along with the old app configuration token.

**Example**
`xoxe-1-abcdefg`

## Usage info

**App Manifest API beta**  
This API is in beta, and is subject to change without the usual notice period for changes.

**Keep your tokens secure**  
App configuration access and refresh tokens are unique to _you_. Do not share them with anyone.

## Rotating configuration tokens 

Each app configuration token will expire 12 hours after it has been generated. In order to continually rotate your config tokens, you are also provided with a **refresh token**.

It's strongly suggested that you refresh your token before it expires, rather than waiting for it to expire and checking for an error from the Slack API.

In order to refresh config tokens, make a call to [`tooling.tokens.rotate`](/methods/tooling.tokens.rotate), using the refresh token in the `refresh_token` argument. In response you'll receive something like this:

```
{
	"ok": true,
	"token": "xoxe.xoxp-...",
	"refresh_token": "xoxe-...",
	"team_id": "...",
	"user_id": "...",
	"iat": 1633095660,
	"exp": 1633138860
}
```

The `token` field contains your new config access token, which you can then store and use for Manifest API calls. The `refresh_token` field contains a _new_ refresh token.

The remainder of the response above contains fields which identify the source workspace and user of each token, as well as timestamps which indicate when the token was issued and when it will expire.

To learn how to use app configuration tokens, read [our guide to the App Manifest APIs](/reference/manifests#manifest_apis).

## Example responses

### Common successful response

Typical success response

```
{
    "ok": true,
    "token": "xoxe.xoxp-...",
    "refresh_token": "xoxe-...",
    "team_id": "...",
    "user_id": "...",
    "iat": 1633095660,
    "exp": 1633138860
}
```

### Common error response

Typical error response if incorrect refresh token used

```
{
    "ok": false,
    "error": "invalid_refresh_token"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `invalid_refresh_token` | 
The given refresh token is invalid.
 |
| `internal_error` | 
The server could not complete your operation(s) without encountering an error, likely due to a transient issue on our end. It's possible some aspect of the operation succeeded before the error was raised.
 |
| `unknown_error` | 
Temporary error for dev only restriction
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

