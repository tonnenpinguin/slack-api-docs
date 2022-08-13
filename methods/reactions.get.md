## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`GEThttps://slack.com/api/reactions.get`

[Bolt for Java](/tools/bolt)
`app.client().reactionsGet`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.reactions_get`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.reactions.get`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`reactions:read`](/scopes/reactions:read)

[User tokens](/docs/token-types#user)[`reactions:read`](/scopes/reactions:read)

[Legacy bot tokens](/docs/token-types#bot)[`bot`](/scopes/bot)

### Content types

`application/x-www-form-urlencoded`

- 
### Rate limits
[Tier 3](/docs/rate-limits#tier_t3)

## Arguments

Required arguments

`token`

[token](/authentication/token-types)

_·_Required

Authentication token bearing required scopes. Tokens should be passed as an HTTP Authorization header or alternatively, as a POST parameter.

**Example**
`xxxx-xxxxxxxxx-xxxx`

Optional arguments

`channel`

string

_·_Optional

Channel where the message to get reactions for was posted.

**Example**
`C0NF841BK`

`file`

string

_·_Optional

File to get reactions for.

**Example**
`F1234567890`

`file_comment`

string

_·_Optional

File comment to get reactions for.

**Example**
`Fc1234567890`

`full`

boolean

_·_Optional

If true always return the complete reaction list.

**Example**
`true`

`timestamp`

string

_·_Optional

Timestamp of the message to get reactions for.

**Example**
`1524523204.000192`

## Usage info

This method returns a list of all reactions for a single item (file, file comment, channel message, group message, or direct message).

An item will always have a `type` property, while the other properties depend on the type of item. The possible types are:

- **`message`** : the item will have a `message` property containing a [message object](/docs/messages) and a `channel` property containing the channel ID for the message
- **`file`** : this item will have a `file` property containing a [file object](/types/file)
- **`file_comment`** : the item will have a `file` property containing the [file object](/types/file) and a `comment` property containing the file comment

The `users` array in the `reactions` property will always contain the authenticated user, but might not always contain all users that have reacted. The value of `count`, however, will always represent the count of all users who made that reaction (i.e. it may be greater than `users.length`).

## Example responses

### Common successful response

The response contains the item with reactions.

```
{
    "ok": true,
    "type": "message",
    "message": {
        "type": "message",
        "text": "Hi there!",
        "user": "W123456",
        "ts": "1648602352.215969",
        "team": "T123456",
        "reactions": [
            {
                "name": "grinning",
                "users": [
                    "W222222"
                ],
                "count": 1
            },
            {
                "name": "question",
                "users": [
                    "W333333"
                ],
                "count": 1
            }
        ],
        "permalink": "https://xxx.slack.com/archives/C123456/p1648602352215969"
    },
    "channel": "C123456"
}
```

### Common error response

Typical error response

```
{
    "ok": false,
    "error": "invalid_auth"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `file_comment_not_found` | 
File comment specified by `file_comment` does not exist.
 |
| `message_not_found` | 
Message specified by `channel` and `timestamp` does not exist.
 |
| `bad_timestamp` | 
Value passed for `timestamp` was invalid.
 |
| `no_item_specified` | 
`file`, `file_comment`, or combination of `channel` and `timestamp` was not specified.
 |
| `file_not_found` | 
File specified by `file` does not exist.
 |
| `channel_not_found` | 
Value passed for `channel` was invalid.
 |
| `not_authed` | 
No authentication token provided.
 |
| `invalid_auth` | 
Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request.
 |
| `access_denied` | 
Access to a resource specified in the request is denied.
 |
| `account_inactive` | 
Authentication token is for a deleted user or workspace when using a `bot` token.
 |
| `token_revoked` | 
Authentication token is for a deleted user or workspace or the app has been removed when using a `user` token.
 |
| `token_expired` | 
Authentication token has expired
 |
| `no_permission` | 
The workspace token used in this request does not have the permissions necessary to complete the request. Make sure your app is a member of the conversation it's attempting to post a message to.
 |
| `org_login_required` | 
The workspace is undergoing an enterprise migration and will not be available until migration is complete.
 |
| `ekm_access_denied` | 
Administrators have suspended the ability to post a message.
 |
| `missing_scope` | 
The token used is not granted the specific scope permissions required to complete this request.
 |
| `not_allowed_token_type` | 
The token type used in this request is not allowed.
 |
| `method_deprecated` | 
The method has been deprecated.
 |
| `deprecated_endpoint` | 
The endpoint has been deprecated.
 |
| `two_factor_setup_required` | 
Two factor setup is required.
 |
| `enterprise_is_restricted` | 
The method cannot be called from an Enterprise.
 |
| `team_access_not_granted` | 
The token used is not granted the specific workspace access required to complete this request.
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

