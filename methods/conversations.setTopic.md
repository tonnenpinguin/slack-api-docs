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
`POSThttps://slack.com/api/conversations.setTopic`

[Bolt for Java](/tools/bolt)
`app.client().conversationsSetTopic`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.conversations_setTopic`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.conversations.setTopic`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`channels:manage`](/scopes/channels:manage)[`groups:write`](/scopes/groups:write)[`im:write`](/scopes/im:write)[`mpim:write`](/scopes/mpim:write)

[User tokens](/docs/token-types#user)[`channels:write`](/scopes/channels:write)[`groups:write`](/scopes/groups:write)[`im:write`](/scopes/im:write)[`mpim:write`](/scopes/mpim:write)

[Legacy bot tokens](/docs/token-types#bot)[`bot`](/scopes/bot)

### Content types

`application/x-www-form-urlencoded`[`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON")

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

`channel`

string

_·_Required

Conversation to set the topic of

**Example**
`C1234567890`

`topic`

string

_·_Required

The new topic string. Does not support formatting or linkification.

**Example**
`Apply topically for best effects`

## Usage info

This method is used to change the topic of a conversation. The calling user must be a member of the conversation. Not all conversation types support a new topic.

**Bots belonging to Slack apps are not supported**  

This method cannot be called with bot user tokens belonging to Slack apps, although legacy bot tokens will work. To use this method in a Slack app, use a [user token](/docs/token-types#user) imbued with the necessary scope. [**Stay tuned**](/changelog) for updates as we bring a fuller feast of features to bots belonging to Slack apps.

## Example responses

### Common successful response

A [conversation object](/types/conversation) is returned:

```
{
    "ok": true,
    "channel": {
        "id": "C12345678",
        "name": "tips-and-tricks",
        "is_channel": true,
        "is_group": false,
        "is_im": false,
        "is_mpim": false,
        "is_private": false,
        "created": 1649195947,
        "is_archived": false,
        "is_general": false,
        "unlinked": 0,
        "name_normalized": "tips-and-tricks",
        "is_shared": false,
        "is_frozen": false,
        "is_org_shared": false,
        "is_pending_ext_shared": false,
        "pending_shared": [],
        "parent_conversation": null,
        "creator": "U12345678",
        "is_ext_shared": false,
        "shared_team_ids": [
            "T12345678"
        ],
        "pending_connected_team_ids": [],
        "is_member": true,
        "last_read": "1649869848.627809",
        "latest": {
            "type": "message",
            "subtype": "channel_topic",
            "ts": "1649952691.429799",
            "user": "U12345678",
            "text": "set the channel topic: Apply topically for best effects",
            "topic": "Apply topically for best effects"
        },
        "unread_count": 1,
        "unread_count_display": 0,
        "topic": {
            "value": "Apply topically for best effects",
            "creator": "U12345678",
            "last_set": 1649952691
        },
        "purpose": {
            "value": "",
            "creator": "",
            "last_set": 0
        },
        "previous_names": []
    }
}
```

### Common error response

Typical error response

```
{
    "ok": false,
    "error": "invalid_arguments",
    "response_metadata": {
        "messages": [
            "[ERROR] missing required field: topic"
        ]
    }
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | 
Value passed for `channel` was invalid.
 |
| `not_in_channel` | 
Authenticated user is not in the channel.
 |
| `is_archived` | 
Channel has been archived.
 |
| `too_long` | 
Topic was longer than 250 characters.
 |
| `user_is_restricted` | 
This method cannot be called by a restricted user or single channel guest.
 |
| `method_not_supported_for_channel_type` | 
This type of conversation cannot be used with this method.
 |
| `missing_scope` | 
The token used is not granted the specific scope permissions required to complete this request.
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

