## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`GEThttps://slack.com/api/conversations.inviteShared`

[Bolt for Java](/tools/bolt)
`app.client().conversationsInviteShared`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.conversations_inviteShared`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.conversations.inviteShared`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`conversations.connect:write`](/scopes/conversations.connect:write)[`groups:write`](/scopes/groups:write)[`mpim:write`](/scopes/mpim:write)[`im:write`](/scopes/im:write)

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

`channel`

string

_·_Required

ID of the channel on your team that you'd like to share

**Example**
`C1234567890`

Optional arguments

`emails`

array

_·_Optional

Optional email to receive this invite. Either `emails` or `user_ids` must be provided.

`external_limited`

boolean

_·_Optional

Optional boolean on whether invite is to a external limited member. Defaults to `true`.

**Example**
`true`

`user_ids`

array

_·_Optional

Optional user\_id to receive this invite. Either `emails` or `user_ids` must be provided.

## Usage info

This [Slack Connect API](/apis/connect) method creates an invitation to a [Slack Connect](/apis/channels-between-orgs) channel. The invitation can be sent to users or bots in another workspace either as a shareable URL or as an email invite.

The invited user must be known to your app (i.e., a channel is already shared between the organization your app is installed on and the organization of the user).

If the channel to be joined is not already a Slack Connect channel, it _becomes_ a Connect channel when you use this method to invite users from another workspace.

### With the `user_ids` argument

If you supply `user_ids`, we'll send a notification to the recipients Slack client for them to accept.

```
{
	"ok": true,
	"invite_id": "I02UKAJ6RJA",
	"is_legacy_shared_channel": false
}
```

### With the `emails` argument

If you supply `emails`, we'll send transactional emails to the recipients containing a URL that triggers joining the Slack Connect channel. Some fields are omitted if `external_limited=true` (see [below](#limited)).

```
{
	"ok": true,
	"invite_id": "I02UKAJ6RJA",
	"is_legacy_shared_channel": false
}
```

### With the `external_limited` argument 

Setting this argument to `true` will send an invitation limiting recipient's actions to only sending messages. They will not be able to invite other members (or bots) from their organization into the channel. They will also not be able to export the channel history, change its visibility, or change the channel topic or name. The channel will be set as private in the guest workspace.

Since this argument defaults to `true`, your app will need to explicitly set it to `false` if you would like the recipient to have more control over the channel in their workspace.

The ability to send either Slack Connect invitation type is controlled by the workspace setting [Manage permissions for inviting people to channels](https://slack.com/help/articles/360050528953). If an invite is sent of a type your app does not have permission to send, the `restricted_action` error is returned. For example, if the workspace is configured to disallow users from sending invitations with permission to post, invite, and more, setting `external_limited=false` will return an error.

When `external_limited=true`, the `conf_code` and `url` fields are not returned in the success response.

## Example responses

### Common successful response

Typical success response

```
{
    "ok": true,
    "invite_id": "I02UKAJ6RJA",
    "is_legacy_shared_channel": false
}
```

### Common error response

Typical error response when no email address or user ID is provided

```
{
    "ok": false,
    "error": "restricted_action"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `cannot_share_cross_workspace_channel` | 
You cannot share a cross-workspace or org-wide channel.
 |
| `cannot_share_mandatory_channel` | 
You cannot share #general or mandatory channels.
 |
| `channel_archived` | 
You cannot share an archived channel.
 |
| `channel_not_found` | 
The channel provided was not found.
 |
| `connection_limit_exceeded` | 
This channel has hit the limit of external connections.
 |
| `connection_limit_exceeded_pending` | 
This channel already has a pending invite.
 |
| `invalid_channel_type` | 
You cannot share MPDMs or DMs.
 |
| `invalid_email` | 
At least one email address provided is invalid.
 |
| `invite_lookup_error` | 
An error occurred while attempting to look for existing invites.
 |
| `invite_not_found` | 
An error occurred while inviting users.
 |
| `legacy_connection_limit_exceeded` | 
You cannot share a legacy ESC channel with a third team
 |
| `member_limit_exceeded` | 
This channel that has hit the limit of members
 |
| `message_too_long` | 
The provided message was longer than 560 characters.
 |
| `not_owner` | 
Only the host organization for a channel can request to share it.
 |
| `not_paid` | 
This feature is only available to paid teams.
 |
| `not_supported` | 
This channel cannot be shared.
 |
| `restricted_action` | 
A team preference does not allow this authorization to send invites.
 |
| `url_in_message` | 
The message contained a URL.
 |
| `user_not_found` | 
User lookup failed.
 |
| `ratelimit` | 
The rate-limit for this method has been reached. The ratelimit is applied on a per-user basis when you pass the `emails` parameter.
 |
| `too_many_emails` | 
Too many email recipients were passed in the `emails` parameter.
 |
| `not_allowed_for_grid_workspace` | 
This workspace does not have Slack Connect enabled.
 |
| `recipients_not_specified` | 
Bots are required to specify which users to invite.
 |
| `already_in_channel` | 
User is already in the channel.
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
| `is_bot` | 
This method cannot be called by a bot user.
 |
| `user_is_restricted` | 
This method cannot be called by a restricted user or single channel guest.
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

