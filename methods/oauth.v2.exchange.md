## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`GEThttps://slack.com/api/oauth.v2.exchange`

[Bolt for Java](/tools/bolt)
`app.client().oauthV2Exchange`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.oauth_v2_exchange`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.oauth.v2.exchange`
[Powered by Bolt](/tools/bolt)

### Content types

`application/x-www-form-urlencoded`

- 
### Rate limits
[Tier 3](/docs/rate-limits#tier_t3)

## Arguments

Required arguments

`client_id`

string

_·_Required

Issued when you created your application.

**Example**
`4123121235.9872358710`

`client_secret`

string

_·_Required

Issued when you created your application.

**Example**
`33fea0113f5b1`

## Usage info

There is no documentation for this method.

Sorry! There aren't any sample responses for this endpoint right now.

## Example responses

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
| `invalid_token` | 
The legacy token provided cannot be exchanged for a new pair of token credentials.
 |
| `token_already_exchanged` | 
This token has already been exchanged for a pair of token credentials.
 |
| `token_rotation_not_enabled` | 
The app does not have token rotation enabled.
 |
| `internal_error` | 
The server could not complete your operation(s) without encountering an error, likely due to a transient issue on our end. It's possible some aspect of the operation succeeded before the error was raised.
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

