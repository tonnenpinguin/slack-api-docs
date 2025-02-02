## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/reactions.add`

[Bolt for Java](/tools/bolt)
`app.client().reactionsAdd`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.reactions_add`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.reactions.add`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`reactions:write`](/scopes/reactions:write)

[User tokens](/docs/token-types#user)[`reactions:write`](/scopes/reactions:write)

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

`channel`

string

_·_Required

Channel where the message to add reaction to was posted.

**Example**
`C1234567890`

`name`

string

_·_Required

Reaction (emoji) name.

**Example**
`thumbsup`

`timestamp`

string

_·_Required

Timestamp of the message to add reaction to.

**Example**
`1234567890.123456`

## Usage info

This method adds a reaction (emoji) to a message. Now that [file threads](/changelog/2018-05-file-threads-soon-tread#whats_changed) work the way you'd expect, the `file` and `file_comment` arguments are deprecated. Specify only `channel` and `timestamp` instead.

For Unicode emoji that support [skin tone modifiers](https://emojipedia.org/emoji-modifier-sequence/), `name` may indicate a modifier by appending `::skin-tone-` and a number from 2 to 6, like, `thumbsup::skin-tone-6` or `wave::skin-tone-3`. This will add a reaction with the base emoji and the specified skin color.

After making this call, the reaction is saved and [a `reaction_added` event](/events/reaction_added) is broadcast via the [Events](/events-api) and [RTM](/rtm) APIs.

A `not_reactable` error is thrown when we decline the opportunity to attach your app's personally selected emoji reaction to a file or file comment. It's not because of how your app feels, it's because that approach is retired. Your app can express its inner reacji for any message though, by specifying `channel` and `timestamp`.

## Example responses

### Common successful response

Typical success response

```
{
    "ok": true
}
```

### Common error response

Typical error response

```
{
    "error": "already_reacted",
    "ok": false
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_reactable` | 
Whatever you passed in, like a `file` or `file_comment`, can't be reacted to anymore. Your app can react to messages though.
 |
| `invalid_name` | 
Value passed for `name` was invalid.
 |
| `too_many_emoji` | 
The limit for distinct reactions (i.e emoji) on the item has been reached.
 |
| `message_not_found` | 
Message specified by `channel` and `timestamp` does not exist.
 |
| `already_reacted` | 
The specified item already has the user/reaction combination.
 |
| `bad_timestamp` | 
Value passed for `timestamp` was invalid.
 |
| `too_many_reactions` | 
The limit for reactions a person may add to the item has been reached.
 |
| `no_item_specified` | 
combination of `channel` and `timestamp` was not specified.
 |
| `is_archived` | 
Channel specified has been archived.
 |
| `channel_not_found` | 
Value passed for `channel` is invalid.
 |
| `external_channel_migrating` | 
The channel is in the process of being migrated.
 |
| `thread_locked` | 
Reactions are disabled as the specified message is part of a locked thread.
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

