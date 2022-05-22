## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/admin.apps.uninstall`

[Bolt for Java](/tools/bolt)
`app.client().adminAppsUninstall`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.admin_apps_uninstall`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.admin.apps.uninstall`
[Powered by Bolt](/tools/bolt)

### Required scopes

[User tokens](/docs/token-types#user)[`admin.apps:write`](/scopes/admin.apps:write)

### Content types

`application/x-www-form-urlencoded`[`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON")

- 
### Rate limits
[Tier 1](/docs/rate-limits#tier_t1)

## Arguments

Required arguments

`token`

[token](/authentication/token-types)

_·_Required

Authentication token bearing required scopes. Tokens should be passed as an HTTP Authorization header or alternatively, as a POST parameter.

**Example**
`xxxx-xxxxxxxxx-xxxx`

`app_id`

string

_·_Required

The ID of the app to uninstall.

**Example**
`A12345`

Optional arguments

`enterprise_id`

string

_·_Optional

The enterprise to completely uninstall the application from (across all workspaces). With an org-level token, this or `team_ids` is required.

**Example**
`E12345`

`team_ids`

string

_·_Optional

IDs of the teams to uninstall from (max 100). With an org-level token, this or `enterprise_id` is required.

## Usage info

This endpoint will uninstall an app from one or many workspaces, or an entire enterprise.

Exactly one of the arguments `enterprise_id` or `team_ids` is required to specify where the app should be removed.

Admin apps that work with [SCIM](/admins/scim), [app management](/admins/approvals), or certain other enterprise-level features cannot be uninstalled using this API. To remove one of those, please [contact support](mailto:devsupport@slack.com).

This [API method for admins](/enterprise/managing) may only be used on [Enterprise Grid](/enterprise).

Uninstalling an app **cannot be undone**. Once an application is uninstalled, users must reinstall it manually if they wish to restore it.

```
{
	"ok": true
}
```

## Example responses

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `permission_denied` | 
You do not have permission to uninstall applications from the reqeusted org or workspaces.
 |
| `app_not_found` | 
The provided `app_id` was not found.
 |
| `enterprise_not_found` | 
The provided `enterprise_id` was not found.
 |
| `teams_not_found` | 
One or more of the provided `team_ids` were not found.
 |
| `too_many_teams` | 
Too many IDs are present in `team_ids`.
 |
| `can_not_uninstall` | 
This application can not be uninstalled via the API.
 |
| `specify_enterprise_or_teams` | 
Provide exactly one of `enterprise_id` or `team_ids`.
 |
| `must_revoke_access` | 
Organization-deployed apps cannot be uninstalled from specific workspaces using this API.
 |
| `feature_not_enabled` | 
The Admin APIs feature is not enabled for this team.
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

