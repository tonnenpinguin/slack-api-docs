## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/chat.postMessage`

[Bolt for Java](/tools/bolt)
`app.client().chatPostMessage`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.chat_postMessage`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.chat.postMessage`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`chat:write`](/scopes/chat:write "Only for use with new Slack apps.")

[User tokens](/docs/token-types#user)[`chat:write`](/scopes/chat:write "Only for use with new Slack apps.")[`chat:write:user`](/scopes/chat:write:user)[`chat:write:bot`](/scopes/chat:write:bot)

[Legacy bot tokens](/docs/token-types#bot)[`bot`](/scopes/bot)

### Content types

`application/x-www-form-urlencoded`[`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON")

- 
### Rate limits
[Special](/docs/rate-limits#tier_t5)

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

Channel, private group, or IM channel to send message to. Can be an encoded ID, or a name. See [below](#channels) for more details.

**Example**
`C1234567890`

_At least one of_`attachments``blocks``text`

One of these arguments is required to describe the content of the message. If `attachments` or `blocks` are included, `text` will be used as fallback text for notifications only.

`attachments`

string

A JSON-based array of structured attachments, presented as a URL-encoded string.

**Example**
`[{"pretext": "pre-hello", "text": "text-world"}]`

`blocks`

[blocks[] as string](/reference/block-kit/blocks)

A JSON-based array of structured blocks, presented as a URL-encoded string.

**Example**
`[{"type": "section", "text": {"type": "plain_text", "text": "Hello world"}}]`

`text`

string

The formatted text of the message to be published. If `blocks` are included, this will become the fallback text used in notifications.

**Example**
`Hello world`

Optional arguments

`as_user`

boolean

_·_Optional

Pass true to post the message as the authed user, instead of as a bot. Defaults to false. See [authorship](#authorship) below.

**Example**
`true`

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

`metadata`

string

_·_Optional

JSON object with event\_type and event\_payload fields, presented as a URL-encoded string. Metadata you post to Slack is accessible to any app or user who is a member of that workspace.

**Example**
`{"event_type": "task_created", "event_payload": { "id": "11223", "title": "Redesign Homepage"}}`

`mrkdwn`

boolean

_·_Optional

Disable Slack markup parsing by setting to `false`. Enabled by default.

**Default**
`true`

**Example**
`false`

`parse`

string

_·_Optional

Change how messages are treated. Defaults to `none`. See [below](#formatting).

**Example**
`full`

`reply_broadcast`

boolean

_·_Optional

Used in conjunction with `thread_ts` and indicates whether reply should be made visible to everyone in the channel or conversation. Defaults to `false`.

**Example**
`true`

`thread_ts`

string

_·_Optional

Provide another message's `ts` value to make this message a reply. Avoid using a reply's `ts` value; use its parent instead.

`unfurl_links`

boolean

_·_Optional

Pass true to enable unfurling of primarily text-based content.

**Example**
`true`

`unfurl_media`

boolean

_·_Optional

Pass false to disable unfurling of media content.

**Example**
`false`

`username`

string

_·_Optional

Set your bot's user name. Must be used in conjunction with `as_user` set to false, otherwise ignored. See [authorship](#authorship) below.

**Example**
`My Bot`

## Usage info

This method posts [a message](/docs/messages) to a public channel, private channel, or direct message/IM channel.

Please note that the `as_user` parameter _may not_ be used by [new Slack apps](/authentication/basics). Read more about [Authorship](#authorship) to understand how `as_user` works for classic Slack apps.

**Using `text` with `blocks` or `attachments`**

The usage of the `text` field changes depending on whether you're using `blocks`. If you are using `blocks`, this is used as a fallback string to display in notifications. If you aren't, this is the main body text of the message. It can be formatted as plain text, or with `mrkdwn`.

The `text` field is not enforced as required when using `blocks` or `attachments`. However, we highly recommended that you include `text` to provide a fallback when using `blocks`, as described above.

### JSON POST support

As of October 2017, it's now possible to send a well-formatted `application/json` POST body to `chat.postMessage` and other [Web API](/web) write methods. No need to carefully URL-encode your JSON `attachments` and present all other fields as URL encoded key/value pairs; just send JSON instead.

Learn more about this support in the [Web API](/web) docs or [this changelog](/changelog/2017-10-keeping-up-with-the-jsons).

The response includes the "timestamp ID" (`ts`) and the channel-like thing where the message was posted. It also includes the complete message object, as parsed by our servers. This may differ from the provided arguments as our servers sanitize links, attachments, and other properties. Your message may mutate.

## Formatting messages 

Messages are formatted as described in the [formatting spec](/docs/message-formatting). You can specify values for `parse` and `link_names` to change formatting behavior.

When POSTing with `application/x-www-form-urlencoded` data, the optional `attachments` argument should contain a JSON-encoded array of attachments. Make it easy on yourself and send your entire messages as `application/json` instead.

For more information, see the [attachments spec](/docs/message-attachments). If you're using a [Slack app](/slack-apps), you can also use this method to attach [message buttons](/docs/message-buttons).

By default links to media are unfurled, but links to text content are not. Links found in [`blocks`](/reference/block-kit/blocks) will also be unfurled by default.

If you want to suppress link unfurls in messages containing [blocks](/reference/block-kit/blocks), set `unfurl_links` and `unfurl_media` to false. For more information on the differences and how to control this, see the [the unfurling documentation](/docs/message-attachments#unfurling).

For best results, limit the number of characters in the `text` field to 4,000 characters. Ideally, messages should be short and human-readable. Slack will [truncate messages](/changelog/2018-truncating-really-long-messages) containing more than 40,000 characters.

If you need to post longer messages, please consider [uploading a snippet instead](/methods/files.upload).

Consider reviewing our [message guidelines](/docs/message-guidelines), especially if you're using attachments or message buttons.

## Threads and replies

Provide a `thread_ts` value for the posted message to act as a reply to a parent message. Sparingly set `reply_broadcast` to `true` if your reply is important enough for everyone in the channel to receive.

See [message threading](/docs/message-threading) for a more in depth look at message threading.

## Channels

You **must** specify a public channel, private channel, or an IM channel with the `channel` argument. Each one behaves slightly differently based on the authenticated user's permissions and additional arguments:

#### Post to a public channel

Pass the channel's ID (`C024BE91L`) to the `channel` parameter and the message will be posted to that channel. The channel's ID can be retrieved through the [conversations.list](/methods/conversations.list) API method.

#### Post to a multi-person direct message

As long as the authenticated user is a member of the multi-person direct message (AKA "private group" or MPIM), you can pass the group's ID (`G012AC86C`), and the message will be posted to that group. The private group's ID can be retrieved through the [conversations.list](/methods/conversations.list) API method.

#### Post to a direct message channel

Posting to direct messages (also known as DMs or IMs) can be a little more complex, depending on what you actually are meaning to accomplish.

If you simply want your app's bot user to start a 1:1 conversation with another user in a workspace, provide the user's user ID as the `channel` value and a direct message conversation will be opened, if it hasn't already. Resultant messages and associated direct message objects will have a direct message ID you can use from that point forward, if you'd rather.

Bot users **can't** post to a direct message conversation between two users using `chat.postMessage`. If your app was involved in the conversation, then it would be a multi-person direct message instead. Apps can post to direct message conversations between users when a [shortcut](/interactivity/shortcuts) or [slash command](/slash-commands) belonging to that app is used in the conversation.

You might receive a `channel_not_found` error if your app doesn't have permission to enter into a DM with the intended user.

Passing a "username" as a `channel` value is deprecated, along with [the whole concept of _usernames_ on Slack](/changelog/2017-09-the-one-about-usernames). Please _always_ use channel-like IDs instead to make sure your message gets to where it's going.

## Begin a conversation in a user's App Home 

Start a conversation with users in your [App Home](/reference/app-home).

With the `chat:write` scope enabled, call `chat.postMessage` and pass a user's ID (`U0G9QF9C6`) as the value of `channel` to post to that user's App Home channel. You can use their direct message channel ID (as found with `im.open`, for instance) instead.

## Rate limiting

`chat.postMessage` has special [rate limiting](/docs/rate-limits) conditions. It will generally allow an app to post 1 message per second to a specific channel. There are limits governing your app's relationship with the entire workspace above that, limiting posting to several hundred messages per minute. Generous burst behavior is also granted.

## Channel membership 

New Slack apps do _not_ begin life with the ability to post in all public channels.

For your new Slack app to gain the ability to post in all public channels, request the [`chat:write.public`](/scopes/chat:write.public) scope.

* * *

## Sending messages as other entities 

Apps can publish messages that appear to have been created by a user in the conversation. The message will be attributed to the user and show their profile photo beside it.

This is a powerful ability and must only be used when the user themselves gives permission to do so. For this reason, this ability is only available when an app has requested and been granted an additional scope — [`chat:write.customize`](/scopes/chat:write.customize).

Your app should only use this feature in response to an inciting user action. It should never be unexpected or surprising to a user that a message was posted on their behalf, and it should be heavily signposted in advance.

With all that in mind, the act of authoring a message as a user is relatively simple. With the aforementioned scope, you'll be able to make calls to [`chat.postMessage`](/methods/chat.postMessage) and provide:

- `username` to specify the username for the published message.
- `icon_url` to specify a URL to an image to use as the profile photo alongside the message.
- `icon_emoji` to specify an emoji (using colon shortcodes, eg. `:white_check_mark:`) to use as the profile photo alongside the message.

This feature works differently for classic Slack apps, [see below for more details](#legacy_authorship).

* * *

## Legacy concerns 

Information in the section below applies only to classic Slack apps.

### Legacy authorship 

Classic Slack apps using the umbrella `bot` scope can't request additional scopes to adjust message authorship.

For classic Slack apps, the best way to control the authorship of a message is to be explicit with the `as_user` parameter.

If you don't use the `as_user` parameter, `chat.postMessage` will guess the most appropriate `as_user` interpretation based on the kind of token you're using.

If `as_user` is not provided at all, then the value is inferred, based on the scopes granted to the caller: If the caller _could_ post with `as_user` passed as `false`, then that is how the method behaves; otherwise, the method behaves as if `as_user` were passed as `true`.

#### When `as_user` is false

When the `as_user` parameter is set to `false`, messages are posted as "[`bot_messages`](/events/message/bot_message)", with message authorship attributed to the user name and icons associated with the [Slack App](/slack-apps).

With `as_user` set to `false`, you may also provide a `username` to explicitly specify the bot user's identity for this message, along with `icon_url` or `icon_emoji`.

#### Effect on identity

Token types provide varying default identity values for `username`, `icon_url`, and `icon_emoji`.

- [test tokens](/docs/oauth-test-tokens)
  - inherits the icon and username of the token owner
- [Slack App user token](/slack-apps) with [`chat:write:user`](/docs/oauth-scopes)
  - inherits icon and username of the token owner
- [Slack App bot user token](/bot-users#share_your_bot_user_as_a_slack_app)
  - inherits Slack App's icon and app's bot username

#### Legacy identity rules in DMs

If you're using `icon_url`, `icon_emoji`, or `username` with `chat.postMessage` and a direct message, some special rules apply to make sure the receiver is crystal clear on just who is sending the message:

- If `as_user` is false:
  - Pass the DM channel's ID (`D023BB3L2`) as the value of `channel` to post to that DM channel _as the app, bot, or user associated with the token_. You can change the icon and username that go with the message using the `icon_url` and `username` parameters. The IM channel's ID can be retrieved through the [conversations.list](/methods/conversations.list) API method.
- If `as_user` is true:
  - Pass the DM channel's ID (`D023BB3L2`) or a user's ID (`U0G9QF9C6`) as the value of `channel` to post to that DM channel _as the app, bot, or user associated with the token_. The IM channel's ID can be retrieved through the [conversations.list](/methods/conversations.list) API method. When `as_user` is true, the caller may _not_ manipulate the icon and username on the message.

## Example responses

### Common successful response

Typical success response

```
{
    "ok": true,
    "channel": "C1H9RESGL",
    "ts": "1503435956.000247",
    "message": {
        "text": "Here's a message for you",
        "username": "ecto1",
        "bot_id": "B19LU7CSY",
        "attachments": [
            {
                "text": "This is an attachment",
                "id": 1,
                "fallback": "This is an attachment's fallback"
            }
        ],
        "type": "message",
        "subtype": "bot_message",
        "ts": "1503435956.000247"
    }
}
```

### Common error response

Typical error response if too many attachments are included

```
{
    "ok": false,
    "error": "too_many_attachments"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | 
Value passed for `channel` was invalid.
 |
| `duplicate_channel_not_found` | 
Channel associated with `client_msg_id` was invalid.
 |
| `duplicate_message_not_found` | 
No duplicate message exists associated with `client_msg_id`.
 |
| `not_in_channel` | 
Cannot post user messages to a channel they are not in.
 |
| `is_archived` | 
Channel has been archived.
 |
| `msg_too_long` | 
Message text is too long
 |
| `no_text` | 
No message text provided
 |
| `restricted_action` | 
A workspace preference prevents the authenticated user from posting.
 |
| `restricted_action_read_only_channel` | 
Cannot post any message into a read-only channel.
 |
| `restricted_action_thread_only_channel` | 
Cannot post top-level messages into a thread-only channel.
 |
| `restricted_action_non_threadable_channel` | 
Cannot post thread replies into a non\_threadable channel.
 |
| `restricted_action_thread_locked` | 
Cannot post replies to a thread that has been locked by admins.
 |
| `too_many_attachments` | 
Too many attachments were provided with this message. A maximum of 100 attachments are allowed on a message.
 |
| `too_many_contact_cards` | 
Too many contact\_cards were provided with this message. A maximum of 10 contact cards are allowed on a message.
 |
| `rate_limited` | 
Application has posted too many messages, [read the Rate Limit documentation](/docs/rate-limits) for more information
 |
| `as_user_not_supported` | 
The `as_user` parameter does not function with workspace apps.
 |
| `ekm_access_denied` | 
Administrators have suspended the ability to post a message.
 |
| `slack_connect_file_link_sharing_blocked` | 
Admin has disabled Slack File sharing in all Slack Connect communications
 |
| `invalid_blocks` | 
Blocks submitted with this message are not valid
 |
| `invalid_blocks_format` | 
The `blocks` is not a valid JSON object or doesn't match the Block Kit syntax.
 |
| `messages_tab_disabled` | 
Messages tab for the app is disabled.
 |
| `metadata_too_large` | 
Metadata exceeds size limit
 |
| `team_access_not_granted` | 
The token used is not granted the specific workspace access required to complete this request.
 |
| `invalid_metadata_format` | 
Invalid metadata format provided
 |
| `invalid_metadata_schema` | 
Invalid metadata schema provided
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

