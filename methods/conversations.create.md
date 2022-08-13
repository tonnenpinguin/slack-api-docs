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
`POSThttps://slack.com/api/conversations.create`

[Bolt for Java](/tools/bolt)
`app.client().conversationsCreate`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.conversations_create`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.conversations.create`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`channels:manage`](/scopes/channels:manage)[`groups:write`](/scopes/groups:write)[`im:write`](/scopes/im:write)[`mpim:write`](/scopes/mpim:write)

[User tokens](/docs/token-types#user)[`channels:write`](/scopes/channels:write)[`groups:write`](/scopes/groups:write)[`im:write`](/scopes/im:write)[`mpim:write`](/scopes/mpim:write)

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

`name`

string

_·_Required

Name of the public or private channel to create

**Example**
`mychannel`

Optional arguments

`is_private`

boolean

_·_Optional

Create a private channel instead of a public one

**Example**
`true`

`team_id`

string

_·_Optional

encoded team id to create the channel in, required if org token is used

**Example**
`T1234567890`

## Usage info

Create a public or private channel using this [Conversations API](/docs/conversations-api) method.

Use [`conversations.open`](/methods/conversations.open) to initiate or resume a direct message or multi-person direct message.

## Naming

Channel names may only contain lowercase letters, numbers, hyphens, and underscores, and must be 80 characters or less. When calling this method, we recommend storing both the channel's `id` and `name` value that returned in the response.

Channel names are always validated by this method, unlike [`channels.create`](/methods/channels.create).

## Example responses

### Common successful response

If successful, the command returns a rather stark [conversation object](/types/conversation)

```
{
    "ok": true,
    "channel": {
        "id": "C0EAQDV4Z",
        "name": "endeavor",
        "is_channel": true,
        "is_group": false,
        "is_im": false,
        "created": 1504554479,
        "creator": "U0123456",
        "is_archived": false,
        "is_general": false,
        "unlinked": 0,
        "name_normalized": "endeavor",
        "is_shared": false,
        "is_ext_shared": false,
        "is_org_shared": false,
        "pending_shared": [],
        "is_pending_ext_shared": false,
        "is_member": true,
        "is_private": false,
        "is_mpim": false,
        "last_read": "0000000000.000000",
        "latest": null,
        "unread_count": 0,
        "unread_count_display": 0,
        "topic": {
            "value": "",
            "creator": "",
            "last_set": 0
        },
        "purpose": {
            "value": "",
            "creator": "",
            "last_set": 0
        },
        "previous_names": [],
        "priority": 0
    }
}
```

### Common error response

Typical error response when name already in use

```
{
    "ok": false,
    "error": "name_taken"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `name_taken` | 
A channel cannot be created with the given name.
 |
| `restricted_action` | 
A team preference prevents the authenticated user from creating channels.
 |
| `no_channel` | 
Value passed for `name` was empty.
 |
| `missing_scope` | 
The token used is not granted the specific scope permissions required to complete this request.
 |
| `invalid_name_required` | 
Value passed for `name` was empty.
 |
| `invalid_name_punctuation` | 
Value passed for `name` contained only punctuation.
 |
| `invalid_name_maxlength` | 
Value passed for `name` exceeded max length.
 |
| `invalid_name_specials` | 
Value passed for `name` contained unallowed special characters or upper case characters.
 |
| `invalid_name` | 
Value passed for `name` was invalid.
 |
| `too_many_convos_for_team` | 
The workspace has exceeded its limit of public and private channels.
 |
| `too_many_convos_for_app_on_team` | 
This app has exceeded its per-workspace limit of public and private channels.
 |
| `cannot_create_channel` | 
This channel is unable to be created.
 |
| `missing_argument` | 
A required argument is missing.
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
| `is_bot` | 
This method cannot be called by a bot user.
 |
| `user_is_ultra_restricted` | 
This method cannot be called by a single channel guest.
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

