## Notices

**Information about required scopes**  
This [Conversations API](/docs/conversations-api) method's required scopes depend on the type of channel-like object you're working with. To use the method, you'll need at least one of the `channels:`, `groups:`, `im:` or `mpim:` scopes corresponding to the conversation type you're working with.

## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/conversations.open`

[Bolt for Java](/tools/bolt)
`app.client().conversationsOpen`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.conversations_open`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.conversations.open`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`channels:manage`](/scopes/channels:manage)[`groups:write`](/scopes/groups:write)[`im:write`](/scopes/im:write)[`mpim:write`](/scopes/mpim:write)

[User tokens](/docs/token-types#user)[`channels:write`](/scopes/channels:write)[`groups:write`](/scopes/groups:write)[`im:write`](/scopes/im:write)[`mpim:write`](/scopes/mpim:write)

[Legacy bot tokens](/docs/token-types#bot)[`bot`](/scopes/bot)

### Content types

`application/x-www-form-urlencoded`[`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON")

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

Resume a conversation by supplying an `im` or `mpim`'s ID. Or provide the `users` field instead.

**Example**
`G1234567890`

`prevent_creation`

boolean

_·_Optional

Do not create a direct message or multi-person direct message. This is used to see if there is an existing dm or mpdm.

**Default**
``

**Example**
`true`

`return_im`

boolean

_·_Optional

Boolean, indicates you want the full IM channel definition in the response.

**Example**
`true`

`users`

string

_·_Optional

Comma separated lists of users. If only one user is included, this creates a 1:1 DM. The ordering of the users is preserved whenever a multi-person direct message is returned. Supply a `channel` when not supplying `users`.

**Example**
`W1234567890,U2345678901,U3456789012`

## Usage info

This [Conversations API](/docs/conversations-api) method opens a multi-person direct message or just a 1:1 direct message.

Use [`conversations.create`](/methods/conversations.create) for public or private channels.

Provide 1 to 8 user IDs in the `users` parameter to open or resume a conversation. Providing only 1 ID will create a direct message. Providing more will create an `mpim`. Don’t include the ID of the user you’re calling `conversations.open` on behalf of – we do that for you.

If there are no conversations already in progress including that exact set of members, a new multi-person direct message conversation begins.

Subsequent calls to `conversations.open` with the same set of users will return the already existing conversation.

Response structure is altered by providing `return_im` parameter. When set to `false`, the default, just a conversation's ID is returned. When set to `true`, the entire conversation object is returned.

## Example responses

### Common successful response

Typical success response

```
{
    "ok": true,
    "channel": {
        "id": "D069C7QFK"
    }
}
```

### Alternate response

Passing `return_im` will expand the response to include more info about a conversation

```
{
    "ok": true,
    "no_op": true,
    "already_open": true,
    "channel": {
        "id": "D069C7QFK",
        "created": 1460147748,
        "is_im": true,
        "is_org_shared": false,
        "user": "U069C7QF3",
        "last_read": "0000000000.000000",
        "latest": null,
        "unread_count": 0,
        "unread_count_display": 0,
        "is_open": true,
        "priority": 0
    }
}
```

### Common error response

Typical error response

```
{
    "ok": false,
    "error": "channel_not_found"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | 
Value passed for `channel` was invalid.
 |
| `user_not_found` | 
Value(s) passed for `users` was invalid.
 |
| `user_not_visible` | 
The calling user is restricted from seeing the requested user.
 |
| `user_disabled` | 
A specified `user` has been disabled.
 |
| `users_list_not_supplied` | 
Missing `users` in request
 |
| `not_enough_users` | 
Needs at least 2 users to open
 |
| `too_many_users` | 
Needs at most 8 users to open
 |
| `method_not_supported_for_channel_type` | 
This type of conversation cannot be used with this method.
 |
| `invalid_user_combination` | 
To message people from multiple organizations, those organizations must be in at least one channel together.
 |
| `missing_scope` | 
The token used is not granted the specific scope permissions required to complete this request.
 |
| `team_access_not_granted` | 
The token used is not granted the specific workspace access required to complete this request.
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
| `no_op` | 
Slack did nothing when serving this request but it did not fail.
 |
| `already_open` | 
The conversation was already open so Slack did nothing.
 |
| `missing_charset` | 
The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended.
 |
| `superfluous_charset` | 
The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous.
 |

