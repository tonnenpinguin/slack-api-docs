## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/chat.postEphemeral`

[Bolt for Java](/tools/bolt)
`app.client().chatPostEphemeral`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.chat_postEphemeral`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.chat.postEphemeral`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`chat:write`](/scopes/chat:write "Only for use with new Slack apps.")

[User tokens](/docs/token-types#user)[`chat:write`](/scopes/chat:write "Only for use with new Slack apps.")[`chat:write:user`](/scopes/chat:write:user)[`chat:write:bot`](/scopes/chat:write:bot)

[Legacy bot tokens](/docs/token-types#bot)[`bot`](/scopes/bot)

### Content types

`application/x-www-form-urlencoded`[`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON")

- 
### Rate limits
[Tier 4](/docs/rate-limits#tier_t4)

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

Channel, private group, or IM channel to send message to. Can be an encoded ID, or a name.

**Example**
`C1234567890`

`text`

string

_·_Required

How this field works and whether it is required depends on other fields you use in your API call. [See below](#text_usage) for more detail.

**Example**
`Hello world`

`user`

_·_Required

`id` of the user who will receive the ephemeral message. The user should be in the channel specified by the `channel` argument.

**Example**
`U0BPQUNTA`

Optional arguments

`as_user`

boolean

_·_Optional

Pass true to post the message as the authed user. Defaults to true if the chat:write:bot scope is not included. Otherwise, defaults to false.

**Example**
`true`

`attachments`

string

_·_Optional

A JSON-based array of structured attachments, presented as a URL-encoded string.

**Example**
`[{"pretext": "pre-hello", "text": "text-world"}]`

`blocks`

[blocks[] as string](/reference/block-kit/blocks)

_·_Optional

A JSON-based array of structured blocks, presented as a URL-encoded string.

**Example**
`[{"type": "section", "text": {"type": "plain_text", "text": "Hello world"}}]`

`icon_emoji`

string

_·_Optional

Emoji to use as the icon for this message. Overrides `icon_url`. Must be used in conjunction with `as_user` set to `false`, otherwise ignored. See [authorship](#authorship) below.

**Example**
`:chart_with_upwards_trend:`

`icon_url`

string

_·_Optional

URL to an image to use as the icon for this message. Must be used in conjunction with `as_user` set to false, otherwise ignored. See [authorship](#authorship) below.

**Example**
`http://lorempixel.com/48/48`

`link_names`

boolean

_·_Optional

Find and link channel names and usernames.

**Example**
`true`

`parse`

string

_·_Optional

Change how messages are treated. Defaults to `none`. See [below](#formatting).

**Example**
`full`

`thread_ts`

string

_·_Optional

Provide another message's `ts` value to post this message in a thread. Avoid using a reply's `ts` value; use its parent's value instead. Ephemeral messages in threads are only shown if there is already an active thread.

**Example**
`1234567890.123456`

`username`

string

_·_Optional

Set your bot's user name. Must be used in conjunction with `as_user` set to false, otherwise ignored. See [authorship](#authorship) below.

**Example**
`My Bot`

## Usage info

This method posts an ephemeral message, which is visible only to the assigned user in a specific public channel, private channel, or private conversation.

Ephemeral message delivery is not guaranteed — the user must be currently active in Slack and a member of the specified `channel`. By nature, ephemeral messages do not persist across reloads, desktop and mobile apps, or sessions.

Use ephemeral messages to send users context-sensitive messages, relevant to the channel they're detectably participating in. Avoid sending unexpected or unsolicited ephemeral messages.

**Using `text` with `blocks` or `attachments`**

The usage of the `text` field changes depending on whether you're using `blocks`. If you are using `blocks`, this is used as a fallback string to display in notifications. If you aren't, this is the main body text of the message. It can be formatted as plain text, or with `mrkdwn`.

The `text` field is not enforced as required when using `blocks` or `attachments`. However, we highly recommended that you include `text` to provide a fallback when using `blocks`, as described above.

## Formatting

Messages are formatted as described in the [formatting spec](/docs/message-formatting). You can specify values for `parse` and `link_names` to change formatting behavior.

The optional `attachments` argument should contain a JSON-encoded array of attachments.

For more information, see the [attachments spec](/docs/message-attachments). If you're using a [Slack app](/slack-apps), you can also use this method to attach [message buttons](/docs/message-buttons).

For best results, limit the number of characters in the `text` field to a few thousand bytes at most. Ideally, messages should be short and human-readable, if you need to post longer messages, please consider [uploading a snippet instead](/methods/files.upload). (A single message should be no larger than 4,000 bytes.)

Consider reviewing our [message guidelines](/docs/message-guidelines), especially if you're using attachments or message buttons.

## Authorship

How message authorship is attributed varies by a few factors, with some behaviors varying depending on the kinds of tokens you're using to post a message.

The best way to realize your intended result is to be explicit with the `as_user` parameter.

`chat.postEphemeral` wants your message posting to succeed and may attempt to guess the most appropriate `as_user` interpretation based on the kind of token you're using.

If `as_user` is not provided at all, then the value is inferred, based on the scopes granted to the caller: If the caller _could_ post with `as_user` passed as `false`, then that is how the method behaves; otherwise, the method behaves as if `as_user` were passed as `true`.

## Legacy concerns 

Information in the section below applies only to classic Slack apps.

### Legacy authorship 

Classic Slack apps using the umbrella `bot` scope can't request additional scopes to adjust message authorship.

For classic Slack apps, the best way to control the authorship of a message is to be explicit with the `as_user` parameter.

If you don't use the `as_user` parameter, `chat.postEphemeral` will guess the most appropriate `as_user` interpretation based on the kind of token you're using.

If `as_user` is not provided at all, then the value is inferred, based on the scopes granted to the caller: If the caller _could_ post with `as_user` passed as `false`, then that is how the method behaves; otherwise, the method behaves as if `as_user` were passed as `true`.

### When `as_user` is false

When the `as_user` parameter is set to `false`, messages are posted as "[`bot_messages`](/events/message/bot_message)", with message authorship attributed to the user name and icons associated with the [Slack App](/slack-apps).

### When `as_user` is true

Set `as_user` to `true` and the authenticated user will appear as the author of the message. Posting as the authenticated user **requires** the`client` or the more preferred `chat:write:user` [scopes](/scopes).

## Target channels and users

You **must** specify a conversation container (public channel, private channel, or an IM channel) by providing its ID to the `channel` argument. You also must specify a target `user`.

Each type of channel behaves slightly differently based on the authenticated user's permissions and additional arguments. If the target `user` is not in the given channel, the ephemeral message will not be delivered, and we'll return a `user_not_in_channel` error.

Workspace apps will receive a `no_permission` error when they are not a member of the specified `channel`.

Note that the `user` parameter expects a user's `id`, and not a username or display name.

#### Post to a public channel

You can either pass the channel's name (`#general`) or encoded ID (`C024BE91L`), and the message will be posted to that channel. The channel's ID can be retrieved through the [channels.list](/methods/channels.list) API method.

#### Post to a private group

As long as the authenticated user is a member of the private group, you can either pass the group's name (`secret-group`) or encoded ID (`G012AC86C`), and the message will be posted to that group. The private group's ID can be retrieved through the [groups.list](/methods/groups.list) API method.

#### Post to an IM channel

Posting to an IM channel is a little more complex depending on the value of `as_user`.

- If `as_user` is false:
  - Pass a username (`@chris`) as the value of `channel` to post to that user's @slackbot channel _as the bot_.
  - Pass the IM channel's ID (`D023BB3L2`) as the value of `channel` to post to that IM channel _as the bot_. The IM channel's ID can be retrieved through the [im.list](/methods/im.list) API method.
- If `as_user` is true:
  - Pass the IM channel's ID (`D023BB3L2`) as the value of `channel` to post to that IM channel _as the authenticated user_. The IM channel's ID can be retrieved through the [im.list](/methods/im.list) API method.

To send a direct message to the user _owning_ the token used in the request, provide the `channel` field with the a conversation/IM ID value found in a method like [`im.list`](/methods/im.list).

The `message_ts` included with the response _cannot_ be used with [`chat.update`](/methods/chat.update), as it does not represent an actual message written to the database like it does with other api methods.

## Example responses

### Common successful response

Typical success response

```
{
    "ok": true,
    "message_ts": "1502210682.580145"
}
```

### Common error response

Typical error response

```
{
    "ok": false,
    "error": "user_not_in_channel"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `restricted_action` | 
A workspace preference prevents the authenticated user from posting.
 |
| `too_many_attachments` | 
Too many attachments were provided with this message. A maximum of 100 attachments are allowed on a message.
 |
| `is_archived` | 
Channel has been archived.
 |
| `channel_not_found` | 
Value passed for `channel` was invalid.
 |
| `messages_tab_disabled` | 
Messages tab for the app is disabled.
 |
| `msg_too_long` | 
Message text is too long
 |
| `no_text` | 
No message text provided
 |
| `user_not_in_channel` | 
Intended recipient is not in the specified channel.
 |
| `invalid_blocks` | 
Blocks submitted with this message are not valid
 |
| `invalid_blocks_format` | 
The `blocks` is not a valid JSON object or doesn't match the Block Kit syntax.
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

