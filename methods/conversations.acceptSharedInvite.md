## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/conversations.acceptSharedInvite`

[Bolt for Java](/tools/bolt)
`app.client().conversationsAcceptSharedInvite`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.conversations_acceptSharedInvite`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.conversations.acceptSharedInvite`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`conversations.connect:write`](/scopes/conversations.connect:write)[`groups:write`](/scopes/groups:write)[`mpim:write`](/scopes/mpim:write)[`im:write`](/scopes/im:write)

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

`channel_name`

string

_·_Required

Name of the channel. If the channel does not exist already in your workspace, this name is the one that the channel will take.

**Example**
`puppies-r-us`

Optional arguments

`channel_id`

_·_Optional

ID of the channel that you'd like to accept. Must provide either invite\_id or channel\_id.

`free_trial_accepted`

boolean

_·_Optional

Whether you'd like to use your workspace's free trial to begin using Slack Connect.

**Example**
`true`

`invite_id`

_·_Optional

See the [`shared_channel_invite_received`](/events/shared_channel_invite_received) event payload for more details on how to retrieve the ID of the invitation.

`is_private`

boolean

_·_Optional

Whether the channel should be private.

**Example**
`true`

`team_id`

string

_·_Optional

The ID of the workspace to accept the channel in. If an org-level token is used to call this method, the `team_id` argument is required.

**Example**
`T1234567890`

## Usage info

This [Slack Connect API](/apis/connect) method accepts an invitation to a Slack Connect channel.

If the channel does not already exist in your app's workspace, this method creates, and names, the Slack Connect channel inside your workspace.

If your app's workspace is not on a paid plan, this API will also start a free trial for your workspace (as long as you qualify and you use the `free_trial_accepted` parameter.

After an invite is accepted by your app, the Slack Connect channel may still need to be [approved](/methods/conversations.approveSharedInvite) by Admins on your workspace or the host organization.

When your app successfully accepts a Slack Connect channel invite:

```
{
	"ok": true,
	"implicit_approval": true,
	"channel_id": "C0001111",
	"invite_id": "I00043221"
}
```

If your app cannot accept because the workspace on which your app is installed has already had a free trial in the past and is not a paid team currently:

```
{
	"ok": false,
	"error": "not_paid"
}
```

## Example responses

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `name_taken` | 
The desired channel name is already taken in your workspace.
 |
| `invalid_arguments` | 
The method was either called with invalid arguments or some detail about the arguments passed are invalid, which is more likely when using complex arguments like blocks or attachments.
 |
| `invalid_name_required` | 
The value passed for `channel_name` was empty.
 |
| `invalid_name_punctuation` | 
The value passed for `channel_name` contained only punctuation.
 |
| `invalid_name_maxlength` | 
The value passed for `channel_name` exceeded the maximum length.
 |
| `invalid_name_specials` | 
The value passed for `channel_name` contained unallowed special characters or upper case characters.
 |
| `invalid_name` | 
The value passed for `channel_name` was invalid.
 |
| `invite_not_found` | 
We couldn't find a Slack Connect channel invite with the ID provided.
 |
| `restricted_action` | 
A team preference prevents the authenticated user from creating private channels.
 |
| `is_pending_connected_to_org` | 
A team pending to join the channel is on the org of the team trying to accept.
 |
| `has_already_connected_to_org` | 
A team on the workspace of the org is already in the channel.
 |
| `email_does_not_match` | 
User's email does not match the email in the invite.
 |
| `connection_limit_exceeded` | 
This channel has hit the limit of external connections.
 |
| `legacy_connection_limit_exceeded` | 
You cannot share a legacy ESC channel with a third team
 |
| `invite_used` | 
This invite has already been accepted.
 |
| `invalid_link` | 
We couldn't find an invite associated with the ID provided.
 |
| `invalid_target_team` | 
The target workspace is invalid.
 |
| `invalid_host_team` | 
The host workspace is invalid.
 |
| `invalid_emoji_not_allowed` | 
The desired name contains emoji.
 |
| `legacy_connection_invalid_org` | 
Teams not previously connected to this legacy channel can't connect.
 |
| `not_paid` | 
This workspace doesn't have access to this feature.
 |
| `user_cannot_create_channel` | 
This user is not allowed to create a channel.
 |
| `invite_from_same_org` | 
You can't accept an invite from the same org or workspace.
 |
| `user_is_restricted` | 
This method cannot be called by a restricted user or single channel guest.
 |
| `not_allowed_for_grid_workspace` | 
Acceptance is not allowed for this workspace.
 |
| `user_required_to_accept_as_private_but_cannot` | 
This uer cannot accept a private channel invitation.
 |
| `invalid_privacy` | 
An invalid channel privacy was provided.
 |
| `team_not_found` | 
The team provided in the `team_id` argument does not exits.
 |
| `user_not_found` | 
The user accepting the invite is not a member of the team provided in the `team_id` argument.
 |
| `invalid_recipient_team` | 
The accepting team does not match the expected recipient team.
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
| `team_access_not_granted` | 
The token used is not granted the specific workspace access required to complete this request.
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

