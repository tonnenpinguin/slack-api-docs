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
`GEThttps://slack.com/api/conversations.replies`

[Bolt for Java](/tools/bolt)
`app.client().conversationsReplies`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.conversations_replies`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.conversations.replies`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`channels:history`](/scopes/channels:history)[`groups:history`](/scopes/groups:history)[`im:history`](/scopes/im:history)[`mpim:history`](/scopes/mpim:history)

[User tokens](/docs/token-types#user)[`channels:history`](/scopes/channels:history)[`groups:history`](/scopes/groups:history)[`im:history`](/scopes/im:history)[`mpim:history`](/scopes/mpim:history)

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

`channel`

string

_·_Required

Conversation ID to fetch thread from.

**Example**
`C1234567890`

`ts`

string

_·_Required

Unique identifier of either a thread’s parent message or a message in the thread. `ts` must be the timestamp of an existing message with 0 or more replies. If there are no replies then just the single message referenced by `ts` will return - it is just an ordinary, unthreaded message.

**Example**
`1234567890.123456`

Optional arguments

`cursor`

string

_·_Optional

Paginate through collections of data by setting the `cursor` parameter to a `next_cursor` attribute returned by a previous request's `response_metadata`. Default value fetches the first "page" of the collection. See [pagination](/docs/pagination) for more detail.

**Example**
`dXNlcjpVMDYxTkZUVDI=`

`inclusive`

boolean

_·_Optional

Include messages with `oldest` or `latest` timestamps in results. Ignored unless either timestamp is specified.

**Default**
`0`

**Example**
`true`

`latest`

string

_·_Optional

Only messages before this Unix timestamp will be included in results.

**Default**
`now`

**Example**
`1234567890.123456`

`limit`

number

_·_Optional

The maximum number of items to return. Fewer than the requested number of items may be returned, even if the end of the users list hasn't been reached.

**Default**
`1000`

**Example**
`20`

`oldest`

string

_·_Optional

Only messages after this Unix timestamp will be included in results.

**Default**
`0`

**Example**
`1234567890.123456`

## Usage info

This [Conversations API](/docs/conversations-api) method returns a cursor-paginated thread of messages posted to a conversation.

The scopes and token types required to use this method vary by conversation type.

[Bot user tokens](/docs/token-types#bot) may use this method for direct message and multi-party direct message conversations but lack sufficient permissions to use this method on public and private channels.

To use `conversations.replies` with public or private channel threads, use a [user token](/docs/token-types#user) with [`channels:history`](/scopes/channels:history) or [`groups:history`](/scopes/groups:history) scopes.

The `channel` and `ts` arguments are always required. `ts` must be the timestamp of an existing message with 0 or more replies. If there are no replies then just the single message referenced by `ts` will return - it is just an ordinary message.

## Pagination

This method uses cursor-based pagination to make it easier to incrementally collect information. To begin pagination, specify a `limit` value under `1000`. We recommend no more than `200` results at a time.

Responses will include a top-level `response_metadata` attribute containing a `next_cursor` value. By using this value as a `cursor` parameter in a subsequent request, along with `limit`, you may navigate through the collection page by virtual page.

See [pagination](/docs/pagination) for more information.

### Pagination by time

This form of pagination can be used in conjunction with cursors.

The `messages` array contains up to 1000 messages between the `oldest` and `latest` timestamps. The earliest messages in the time range are returned first.

If there were more than 1000 messages between `oldest` and `latest`, then `has_more` will be `true` in the response. In an additional call, set the `ts` value of the final message as `latest` to get the next page of messages.

If a message has the same timestamp as `oldest` or `latest` it will not be included in the list. This functionality allows you to use the timestamps of specific messages as boundaries for the results. You can, however, have both timestamps be included in the time range by setting `inclusive` to `true`. The `inclusive` parameter is ignored when `oldest` or `latest` is not specified.

## Example responses

### Common successful response

Typical success response

```
{
    "messages": [
        {
            "type": "message",
            "user": "U061F7AUR",
            "text": "island",
            "thread_ts": "1482960137.003543",
            "reply_count": 3,
            "subscribed": true,
            "last_read": "1484678597.521003",
            "unread_count": 0,
            "ts": "1482960137.003543"
        },
        {
            "type": "message",
            "user": "U061F7AUR",
            "text": "one island",
            "thread_ts": "1482960137.003543",
            "parent_user_id": "U061F7AUR",
            "ts": "1483037603.017503"
        },
        {
            "type": "message",
            "user": "U061F7AUR",
            "text": "two island",
            "thread_ts": "1482960137.003543",
            "parent_user_id": "U061F7AUR",
            "ts": "1483051909.018632"
        },
        {
            "type": "message",
            "user": "U061F7AUR",
            "text": "three for the land",
            "thread_ts": "1482960137.003543",
            "parent_user_id": "U061F7AUR",
            "ts": "1483125339.020269"
        }
    ],
    "has_more": true,
    "ok": true,
    "response_metadata": {
        "next_cursor": "bmV4dF90czoxNDg0Njc4MjkwNTE3MDkx"
    }
}
```

### Common error response

Typical error response

```
{
    "ok": false,
    "error": "thread_not_found"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | 
Value for `channel` was missing or invalid.
 |
| `thread_not_found` | 
Value for `ts` was missing or invalid.
 |
| `missing_scope` | 
The token used is not granted the specific scope permissions required to complete this request.
 |
| `invalid_cursor` | 
Value passed for `cursor` was not valid or is no longer valid.
 |
| `invalid_ts_latest` | 
Value passed for `latest` was invalid
 |
| `invalid_ts_oldest` | 
Value passed for `oldest` was invalid
 |
| `invalid_metadata_filter_keys` | 
Value passed for `metadata_keys_to_include` was invalid. Must be valid json array of strings.
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

