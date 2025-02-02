## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`GEThttps://slack.com/api/reactions.list`

[Bolt for Java](/tools/bolt)
`app.client().reactionsList`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.reactions_list`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.reactions.list`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`reactions:read`](/scopes/reactions:read)

[User tokens](/docs/token-types#user)[`reactions:read`](/scopes/reactions:read)

[Legacy bot tokens](/docs/token-types#bot)[`bot`](/scopes/bot)

### Content types

`application/x-www-form-urlencoded`

- 
### Rate limits
[Tier 2](/docs/rate-limits#tier_t2)

## Arguments

Required arguments

`token`

[token](/authentication/token-types)

_·_Required

Authentication token bearing required scopes. Tokens should be passed as an HTTP Authorization header or alternatively, as a POST parameter.

**Example**
`xxxx-xxxxxxxxx-xxxx`

Optional arguments

`count`

integer

_·_Optional

Number of items to return per page.

**Default**
`100`

**Example**
`20`

`cursor`

string

_·_Optional

Parameter for pagination. Set `cursor` equal to the `next_cursor` attribute returned by the previous request's `response_metadata`. This parameter is optional, but pagination is mandatory: the default value simply fetches the first "page" of the collection. See [pagination](/docs/pagination) for more details.

**Example**
`dXNlcjpVMDYxTkZUVDI=`

`full`

boolean

_·_Optional

If true always return the complete reaction list.

**Example**
`true`

`limit`

integer

_·_Optional

The maximum number of items to return. Fewer than the requested number of items may be returned, even if the end of the list hasn't been reached.

**Default**
`0`

**Example**
`20`

`page`

integer

_·_Optional

Page number of results to return.

**Default**
`1`

**Example**
`2`

`team_id`

string

_·_Optional

encoded team id to list reactions in, required if org token is used

**Example**
`T1234567890`

`user`

string

_·_Optional

Show reactions made by this user. Defaults to the authed user.

**Example**
`W1234567890`

## Usage info

This method returns a list of all items (file, file comment, channel message, group message, or direct message) with reactions made by the user.

Every item in the list has a `type` property, while the other properties depend on the type of item. The possible types are:

- **`message`** : the item will have a `message` property containing a [message object](/docs/messages) and a `channel` property containing the channel ID for the message
- **`file`** : this item will have a `file` property containing a [file object](/types/file)
- **`file_comment`** : the item will have a `file` property containing the [file object](/types/file) and a `comment` property containing the file comment

The `users` array in the `reactions` property will always contain the authenticated user, but might not always contain all users that have reacted. The value of `count`, however, will always represent the count of all users who made that reaction (i.e. it may be greater than `users.length`).

Pagination information follows the returned list of items, and contains:

- the `count` of items returned, up to 1000
- the `total` number of items reacted to
- the `page` of results returned in this response, up to 100
- the total number of `pages` available

## Example responses

### Common successful response

Typical success response

```
{
    "items": [
        {
            "type": "message",
            "channel": "C3UKJTQAC",
            "message": {
                "bot_id": "B4VLRLMKJ",
                "reactions": [
                    {
                        "count": 1,
                        "name": "robot_face",
                        "users": [
                            "U2U85N1RV"
                        ]
                    }
                ],
                "subtype": "bot_message",
                "text": "Hello from Python! :tada:",
                "ts": "1507849573.000090",
                "username": "Shipit Notifications"
            }
        },
        {
            "comment": {
                "type": "file_comment",
                "comment": "This is a file comment",
                "created": 1508286096,
                "id": "Fc7LP08P1U",
                "reactions": [
                    {
                        "count": 1,
                        "name": "white_check_mark",
                        "users": [
                            "U2U85N1RV"
                        ]
                    }
                ],
                "timestamp": 1508286096,
                "user": "U2U85N1RV"
            },
            "file": {
                "channels": [
                    "C2U7V2YA2"
                ],
                "comments_count": 1,
                "created": 1507850315,
                "reactions": [
                    {
                        "count": 1,
                        "name": "stuck_out_tongue_winking_eye",
                        "users": [
                            "U2U85N1RV"
                        ]
                    }
                ],
                "title": "computer.gif",
                "user": "U2U85N1RV",
                "username": ""
            }
        },
        {
            "file": {
                "channels": [
                    "C2U7V2YA2"
                ],
                "comments_count": 1,
                "created": 1507850315,
                "id": "F7H0D7ZA4",
                "name": "computer.gif",
                "reactions": [
                    {
                        "count": 1,
                        "name": "stuck_out_tongue_winking_eye",
                        "users": [
                            "U2U85N1RV"
                        ]
                    }
                ],
                "size": 1639034,
                "title": "computer.gif",
                "user": "U2U85N1RV",
                "username": ""
            },
            "type": "file"
        }
    ],
    "ok": true,
    "response_metadata": {
        "next_cursor": "dGVhbTpDMUg5UkVTR0w="
    }
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
| `user_not_found` | 
Value passed for `user` was invalid.
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

