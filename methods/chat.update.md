## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/chat.update`

[Bolt for Java](/tools/bolt)
`app.client().chatUpdate`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.chat_update`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.chat.update`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`chat:write`](/scopes/chat:write "Only for use with new Slack apps.")

[User tokens](/docs/token-types#user)[`chat:write`](/scopes/chat:write "Only for use with new Slack apps.")[`chat:write:bot`](/scopes/chat:write:bot)[`chat:write:user`](/scopes/chat:write:user)

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

Channel containing the message to be updated.

**Example**
`C1234567890`

`ts`

string

_·_Required

Timestamp of the message to be updated.

**Example**
`"1405894322.002768"`

Optional arguments

`as_user`

boolean

_·_Optional

Pass true to update the message as the authed user. [Bot users](/bot-users) in this context are considered authed users.

**Example**
`true`

`attachments`

string

_·_Optional

A JSON-based array of structured attachments, presented as a URL-encoded string. This field is required when not presenting `text`. If you don't include this field, the message's previous `attachments` will be retained. To remove previous `attachments`, include an empty array for this field.

**Example**
`[{"pretext": "pre-hello", "text": "text-world"}]`

`blocks`

[blocks[] as string](/reference/block-kit/blocks)

_·_Optional

A JSON-based array of [structured blocks](/block-kit/building), presented as a URL-encoded string. If you don't include this field, the message's previous `blocks` will be retained. To remove previous `blocks`, include an empty array for this field.

**Example**
`[{"type": "section", "text": {"type": "plain_text", "text": "Hello world"}}]`

`file_ids`

array

_·_Optional

Array of new file ids that will be sent with this message.

**Example**
`F013GKY52QK,F013GL22D0T or ["F013GKY52QK","F013GL22D0T"]`

`link_names`

boolean

_·_Optional

Find and link channel names and usernames. Defaults to `none`. If you do not specify a value for this field, the original value set for the message will be overwritten with the default, `none`.

**Example**
`true`

`metadata`

string

_·_Optional

JSON object with event\_type and event\_payload fields, presented as a URL-encoded string. If you don't include this field, the message's previous `metadata` will be retained. To remove previous `metadata`, include an empty object for this field. Metadata you post to Slack is accessible to any app or user who is a member of that workspace.

**Example**
`{"event_type": "task_created", "event_payload": { "id": "11223", "title": "Redesign Homepage"}}`

`parse`

string

_·_Optional

Change how messages are treated. Defaults to `client`, unlike `chat.postMessage`. Accepts either `none` or `full`. If you do not specify a value for this field, the original value set for the message will be overwritten with the default, `client`.

**Example**
`none`

`reply_broadcast`

boolean

_·_Optional

Broadcast an existing thread reply to make it visible to everyone in the channel or conversation.

**Default**
`false`

**Example**
`true`

`text`

string

_·_Optional

New text for the message, using the [default formatting rules](/reference/surfaces/formatting). It's not required when presenting `blocks` or `attachments`.

**Example**
`Hello world`

## Usage info

This method updates a message in a channel. Though related to [`chat.postMessage`](/methods/chat.postMessage), some parameters of `chat.update` are handled differently.

Ephemeral messages created by [`chat.postEphemeral`](/methods/chat.postEphemeral) or otherwise cannot be updated with this method.

New Slack apps may use this method with the [`chat:write`](/scopes/chat:write) scope and either a bot or user token.

To define your message, refer to our [formatting spec](/reference/surfaces/formatting) and our guide to [composing messages](/messaging/composing).

## Valid message types

Only messages posted by the authenticated user are able to be updated using this method. This includes regular chat messages, as well as messages containing the `me_message` subtype. [Bot users](/bot-users) may also update the messages they post.

Attempting to update other message types will return a `cant_update_message` error.

To use `chat.update` with a [bot user](/bot-users) token, you'll need to _think of your bot user as a user_, and pass `as_user` set to `true` while editing a message created by that same bot user.

The response includes the `text`, `channel` and `timestamp` properties of the updated message so clients can keep their local copies of the message in sync.

### Updating interactive messages

If you're posting an [interactive message](/messaging/interactivity), you may use `chat.update` to continue updating ongoing state changes around a message. Provide the `ts` field the message you're updating and follow the bot user instructions above to update message text, and remove or add blocks.

## Example responses

### Common successful response

Typical success response

```
{
    "ok": true,
    "channel": "C024BE91L",
    "ts": "1401383885.000061",
    "text": "Updated text you carefully authored",
    "message": {
        "text": "Updated text you carefully authored",
        "user": "U34567890"
    }
}
```

### Common error response

Typical error response

```
{
    "ok": false,
    "error": "cant_update_message"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `message_not_found` | 
No message exists with the requested timestamp.
 |
| `cant_update_message` | 
Authenticated user does not have permission to update this message.
 |
| `cant_broadcast_message` | 
Unable to broadcast this message.
 |
| `update_failed` | 
Internal update failure.
 |
| `channel_not_found` | 
Value passed for `channel` was invalid.
 |
| `team_not_found` | 
Team associated with the message and channel could not be found.
 |
| `edit_window_closed` | 
The message cannot be edited due to the team message edit settings
 |
| `msg_too_long` | 
Message text is too long
 |
| `too_many_attachments` | 
Too many attachments were provided with this message. A maximum of 100 attachments are allowed on a message.
 |
| `no_dual_broadcast_content_update` | 
Can't broadcast an old reply and update the content at the same time.
 |
| `no_text` | 
No message text provided
 |
| `as_user_not_supported` | 
The `as_user` parameter does not function with workspace apps.
 |
| `is_inactive` | 
The message cannot be edited within a frozen, archived or deleted channel.
 |
| `invalid_blocks` | 
The blocks were invalid for the requesting user.
 |
| `invalid_attachments` | 
The attachments were invalid.
 |
| `external_channel_migrating` | 
The channel is in the process of migrating and so the message cannot be updated at this time.
 |
| `slack_connect_file_link_sharing_blocked` | 
Admin has disabled Slack File sharing in all Slack Connect communications
 |
| `metadata_too_large` | 
Metadata exceeds size limit
 |
| `invalid_metadata_schema` | 
Invalid metadata schema provided
 |
| `invalid_metadata_format` | 
Invalid metadata format provided
 |
| `metadata_must_be_sent_from_app` | 
Message metadata can only be posted or updated using an app token
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
| `message_truncated` | 
The `text` field of a message should have no more than 40,000 characters. We [truncate really long messages](/changelog/2018-04-truncating-really-long-messages).
 |
| `missing_charset` | 
The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended.
 |
| `superfluous_charset` | 
The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous.
 |

