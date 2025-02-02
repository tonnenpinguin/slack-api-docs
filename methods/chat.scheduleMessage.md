## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/chat.scheduleMessage`

[Bolt for Java](/tools/bolt)
`app.client().chatScheduleMessage`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.chat_scheduleMessage`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.chat.scheduleMessage`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`chat:write`](/scopes/chat:write "Only for use with new Slack apps.")

[User tokens](/docs/token-types#user)[`chat:write`](/scopes/chat:write "Only for use with new Slack apps.")[`chat:write:user`](/scopes/chat:write:user)[`chat:write:bot`](/scopes/chat:write:bot)

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

Channel, private group, or DM channel to send message to. Can be an encoded ID, or a name. See [below](#channels) for more details.

**Example**
`C1234567890`

`post_at`

integer

_·_Required

Unix EPOCH timestamp of time in future to send the message.

**Example**
`299876400`

`text`

string

_·_Required

How this field works and whether it is required depends on other fields you use in your API call. [See below](#text_usage) for more detail.

**Example**
`Hello world`

Optional arguments

`as_user`

boolean

_·_Optional

Set to `true` to post the message as the authed user, instead of as a bot. Defaults to false. Cannot be used by [new Slack apps](/authentication/basics). See [chat.postMessage](chat.postMessage#authorship).

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

`link_names`

boolean

_·_Optional

Find and link user groups. No longer supports linking individual users; use syntax shown in [Mentioning Users](/reference/surfaces/formatting#mentioning-users) instead.

**Example**
`true`

`metadata`

string

_·_Optional

JSON object with event\_type and event\_payload fields, presented as a URL-encoded string. Metadata you post to Slack is accessible to any app or user who is a member of that workspace.

**Example**
`{"event_type": "task_created", "event_payload": { "id": "11223", "title": "Redesign Homepage"}}`

`parse`

string

_·_Optional

Change how messages are treated. See [chat.postMessage](chat.postMessage#formatting).

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

## Usage info

This method schedules [a message](/docs/messages) for delivery to a public channel, private channel, or direct message/IM channel at a specified time in the future.

The `post_at` argument is a Unix timestamp, representing the time the message should post to Slack in the future.

With the exception of the additional `post_at` argument, this method behaves identically to [`chat.postMessage`](/methods/chat.postMessage). Peruse that page for in-depth documentation on each parameter that this method accepts.

The usage of the `text` field changes depending on whether you're using `blocks`. If you are using `blocks`, this is used as a fallback string to display in notifications. If you aren't, this is the main body text of the message. It can be formatted as plain text, or with `mrkdwn`.

## Restrictions 

You will only be able to schedule a message up to 120 days into the future. If you specify a `post_at` timestamp beyond this limit, you’ll receive a `time_too_far` error response. Additionally, you cannot schedule more than 30 messages to post within a 5-minute window to the same channel. Exceeding this will result in a `restricted_too_many` error.

The response includes the `scheduled_message_id` assigned to your message. Use it with the [`chat.deleteScheduledMessage`](/methods/chat.deleteScheduledMessage) method to delete the message before it is sent.

For details on formatting, usage in threads, and rate limiting, check out [`chat.postMessage`](/methods/chat.postMessage) documentation.

## Channels

You **must** specify a public channel, private channel, or IM channel with the `channel` argument. Each one behaves slightly differently based on the authenticated user's permissions and additional arguments:

#### Post to a channel

You can either pass the channel's name (`#general`) or encoded ID (`C123456`), and the message will be posted to that channel. The channel's ID can be retrieved through the [channels.list](/methods/channels.list) API method.

#### Post to a DM

Pass the IM channel's ID (`D123456`) or a user's ID (`U123456`) as the value of `channel` to post to that IM channel as the app. The IM channel's ID can be retrieved through the [im.list](/methods/im.list) API method.

You might receive a `channel_not_found` error if your app doesn't have permission to enter into an IM with the intended user.

## Example responses

### Common successful response

Typical success response

```
{
    "ok": true,
    "channel": "C123456",
    "scheduled_message_id": "Q1298393284",
    "post_at": "1562180400",
    "message": {
        "text": "Here's a message for you in the future",
        "username": "ecto1",
        "bot_id": "B123456",
        "attachments": [
            {
                "text": "This is an attachment",
                "id": 1,
                "fallback": "This is an attachment's fallback"
            }
        ],
        "type": "delayed_message",
        "subtype": "bot_message"
    }
}
```

### Common error response

Typical error response if the `post_at` is invalid (ex. in the past or too far into the future)

```
{
    "ok": false,
    "error": "time_in_past"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `invalid_time` | 
value passed for `post_time` was invalid.
 |
| `time_in_past` | 
value passed for `post_time` was in the past.
 |
| `time_too_far` | 
value passed for `post_time` was too far into the future.
 |
| `channel_not_found` | 
Value passed for `channel` was invalid.
 |
| `metadata_too_large` | 
Metadata exceeds size limit
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
| `too_many_attachments` | 
Too many attachments were provided with this message. A maximum of 100 attachments are allowed on a message.
 |
| `rate_limited` | 
Application has posted too many messages, [read the Rate Limit documentation](/docs/rate-limits) for more information
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
| `restricted_too_many` | 
Too many messages were scheduled in the channel for a given period. See [usage info](/methods/chat.scheduleMessage#restrictions) for additional details
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

