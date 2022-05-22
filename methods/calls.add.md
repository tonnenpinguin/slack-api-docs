## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/calls.add`

[Bolt for Java](/tools/bolt)
`app.client().callsAdd`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.calls_add`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.calls.add`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`calls:write`](/scopes/calls:write)

[User tokens](/docs/token-types#user)[`calls:write`](/scopes/calls:write)

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

`external_unique_id`

string

_·_Required

An ID supplied by the 3rd-party Call provider. It must be unique across all Calls from that service.

**Example**
`025169F6-E37A-4E62-BB54-7F93A0FC4C1F`

`join_url`

string

_·_Required

The URL required for a client to join the Call.

**Example**
`https://example.com/calls/1234567890`

Optional arguments

`created_by`

string

_·_Optional

The valid Slack user ID of the user who created this Call. When this method is called with a user token, the `created_by` field is optional and defaults to the authed user of the token. Otherwise, the field is required.

**Example**
`U1H77`

`date_start`

integer

_·_Optional

Call start time in UTC UNIX timestamp format

**Example**
`1562002086`

`desktop_app_join_url`

string

_·_Optional

When supplied, available Slack clients will attempt to directly launch the 3rd-party Call with this URL.

**Example**
`callapp://join/1234567890`

`external_display_id`

string

_·_Optional

An optional, human-readable ID supplied by the 3rd-party Call provider. If supplied, this ID will be displayed in the Call object.

**Example**
`705-292-868`

`title`

string

_·_Optional

The name of the Call.

**Example**
`Kimpossible sync up`

`users`

array

_·_Optional

The list of users to register as participants in the Call. [Read more on how to specify users here](/apis/calls#users).

**Example**
`[{"slack_id": "U1H77"}, {"external_id": "54321678", "display_name": "External User", "avatar_url": "https://example.com/users/avatar1234.jpg"}]`

## Usage info

This method is part of our [Calls API](/apis/calls).

For more information on how to specify `users`, read our [additional documentation in the usage guide](/apis/calls#users).

```
{
	"ok": true,
	"call": {
		"id": "R0E69JAIF",
		"date_start": 1562002086,
		"external_unique_id": "025169F6-E37A-4E62-BB54-7F93A0FC4C1F",
		"join_url": "https://example.com/calls/1234567890",
		"desktop_app_join_url": "callapp://join/1234567890",
		"external_display_id": "705-292-868",
		"title": "Kimpossible sync up",
		"users": [
			{
				"slack_id": "U0MQG83FD"
			},
			{
				"external_id": "54321678",
				"display_name": "Kim Possible",
				"avatar_url": "https://callmebeepme.com/users/avatar1234.jpg"
			}
		]
	}
}
```

Note that the success response includes an `id` associated with the Call. This ID can be used to post the Call to a channel by using the [`chat.postMessage`](/methods/chat.postMessage) method with the following block:

```
{
	"blocks": [
		{
			"type": "call",
			"call_id": "R0E69JAIF"
		}
	]
}
```

## Example responses

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_implemented` | 
This method is not available.
 |
| `internal_error` | 
The server could not complete your operation(s) without encountering an error, likely due to a transient issue on our end. It's possible some aspect of the operation succeeded before the error was raised.
 |
| `invalid_start_time` | 
The start time is invalid.
 |
| `not_authorized` | 
The specified user is not authorized to create a Call in this channel.
 |
| `user_not_found` | 
A specified user wasn't found.
 |
| `invalid_created_by` | 
The `created_by` user ID is invalid.
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

