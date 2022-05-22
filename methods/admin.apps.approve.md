## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/admin.apps.approve`

[Bolt for Java](/tools/bolt)
`app.client().adminAppsApprove`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.admin_apps_approve`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.admin.apps.approve`
[Powered by Bolt](/tools/bolt)

### Required scopes

[User tokens](/docs/token-types#user)[`admin.apps:write`](/scopes/admin.apps:write)

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

`app_id`

string

_·_Optional

The id of the app to approve.

**Example**
`A12345`

`enterprise_id`

_·_Optional

The ID of the enterprise to approve the app on

**Example**
`E12345`

`request_id`

string

_·_Optional

The id of the request to approve.

**Example**
`Ar12345`

`team_id`

_·_Optional

The ID of the workspace to approve the app on

**Example**
`T12345`

## Usage info

This [App Management API](/admins/approvals) method approves an app install request, approves an app for a particular workspace, approves an app across an entire enterprise. When approved for an enterprise, an app can still be independently [restricted](/methods/admin.apps.restrict) on particular workspaces.

This method requires an `admin.*` scope. It's obtained through the normal [OAuth process](/authentication), but there are a few additional requirements. The scope must be requested by an Enterprise Grid admin or owner, and the OAuth install must take place on the entire Grid org, not an individual workspace. See the [`admin.apps:write` page](/scopes/admin.apps:write) for more detailed instructions.

This [API method for admins](/enterprise/managing) may only be used on [Enterprise Grid](/enterprise).

Exactly one of the `team_id` or `enterprise_id` arguments is required, not both.

Either `app_id` or `request_id` is required. These IDs can be obtained either directly via the [`app_requested` event](/events/app_requested), or by the [`admin.apps.requests.list` method](/methods/admin.apps.requests.list).

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
| `invalid_request_id` | 
The `request_id` passed is invalid.
 |
| `invalid_app_id` | 
The `app_id` passed is invalid.
 |
| `request_id_required_for_custom_integrations` | 
A `request_id` is required for custom integrations
 |
| `feature_not_enabled` | 
Returned when the Admin APIs feature is not enabled for this team
 |
| `not_an_admin` | 
This method is only accessible by org owners and admins
 |
| `request_id_or_app_id_is_required` | 
Must include a `request_id` or `app_id`
 |
| `too_many_ids_provided` | 
Please provide only `app_id` OR `request_id`
 |
| `too_many_teams_provided` | 
Please provide only `team_id` OR `enterprise_id`
 |
| `request_already_resolved` | 
The app request has already been resolved
 |
| `team_not_found` | 
Returned when team id is not found.
 |
| `app_management_app_not_installed_on_org` | 
The app management app must be installed on the org.
 |
| `app_restricted_org_wide` | 
The app is already restricted org wide.
 |
| `invalid_scopes` | 
Some of the provided scopes do not exist
 |
| `custom_integration_not_allowed_at_enterprise` | 
Returned when the install request is for custom integration app.
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

