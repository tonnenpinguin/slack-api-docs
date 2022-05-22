## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/admin.users.session.list`

[Bolt for Java](/tools/bolt)
`app.client().adminUsersSessionList`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.admin_users_session_list`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.admin.users.session.list`
[Powered by Bolt](/tools/bolt)

### Required scopes

[User tokens](/docs/token-types#user)[`admin.users:read`](/scopes/admin.users:read)

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

Optional arguments

`cursor`

string

_·_Optional

Set `cursor` to `next_cursor` returned by the previous call to list items in the next page.

**Example**
`5c3e53d5`

`limit`

integer

_·_Optional

The maximum number of items to return. Must be between 1 - 1000 both inclusive.

**Default**
`1000`

**Example**
`100`

`team_id`

string

_·_Optional

The ID of the workspace you'd like active sessions for. If you pass a `team_id`, you'll need to pass a `user_id` as well.

**Example**
`T1234`

`user_id`

string

_·_Optional

The ID of user you'd like active sessions for. If you pass a `user_id`, you'll need to pass a `team_id` as well.

**Example**
`U1234`

## Usage info

This [Admin API](/admins) lists active login sessions to your Slack organization.

If no `user_id` and `team_id` are passed, you'll receive a paginated list of _all_ sessions.

When you pass `user_id` and `team_id` (which must be used together), you'll receive a list of active sessions by that user on the workspace specified by `team_id`.

These sessions can be used to reset a session with the [`invalidate` method](/methods/admin.users.session.invalidate): the user will be logged out and forced to login again. If the user has multiple sessions with multiple devices, the other sessions will be unaffected.

If you'd like to reset _all sessions_ for a given user, use the [`admin.users.session.reset`](/methods/admin.users.session.reset) method instead.

This [API method for admins](/enterprise/managing) may only be used on [Enterprise Grid](/enterprise).

Inside an `active_session`, you'll find two objects: `recent` and `created`.

`recent` signifies the most recent version of the session, while `created` represents the original version of the session. This covers the case where the same session persists across multiple `slack_client_version`s (or OS versions or IPs).

If the session hasn't changed since creation, you'll **only** find `created` and not `recent`.

Both `recent` and `created` contain the following info:

- `device_hardware`: The type of device for the session.
- `os`: The operating system for the device.
- `os_version`: The version of the OS.
- `slack_client_version`: The version of the Slack client, if available.
- `ip`: The IP address for the session.

## Example responses

### Common successful response

Typical success response

```
{
    "ok": true,
    "active_sessions": [
        {
            "user_id": "U012S9M77JP",
            "team_id": "E011E2SBBFC",
            "session_id": 1112275520242,
            "recent": {
                "device_hardware": "Intel",
                "os": "OS X",
                "os_version": "10.15.7",
                "slack_client_version": "91.0.4472.77",
                "ip": "24.6.145.138"
            },
            "created": {
                "device_hardware": "Intel",
                "os": "OS X",
                "os_version": "10.15.7",
                "slack_client_version": "91.0.4472.77",
                "ip": "24.6.145.138"
            }
        }
    ]
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `no_active_sessions` | 
No active sessions were found.
 |
| `user_not_found` | 
There was an error finding the requested user.
 |
| `team_not_found` | 
There was an error finding the requested workspace.
 |
| `missing_team` | 
A `team_id` must be provided with a `user_id`.
 |
| `missing_user` | 
A `user_id` must be provided with a `team_id`.
 |
| `bots_not_allowed` | 
Bot sessions are not listed by this method.
 |
| `invalid_cursor` | 
The cursor passed was invalid.
 |
| `admin_unauthorized` | 
The owner of this token isn't authorized to list sessions.
 |
| `not_an_admin` | 
The owner of this token isn't an Org Owner or Admin.
 |
| `unknown_method` | 
This method is currently not available.
 |
| `feature_not_enabled` | 
This method is only available to Enterprise customers.
 |
| `invalid_auth` | 
Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request.
 |
| `not_authed` | 
No authentication token provided.
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

