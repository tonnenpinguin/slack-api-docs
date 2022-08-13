## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/admin.audit.anomaly.allow.updateItem`

[Bolt for Java](/tools/bolt)
`app.client().adminAuditAnomalyAllowUpdateItem`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.admin_audit_anomaly_allow_updateItem`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.admin.audit.anomaly.allow.updateItem`
[Powered by Bolt](/tools/bolt)

### Required scopes

### Content types

`application/x-www-form-urlencoded`[`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON")

- 
### Rate limits
[Tier 2](/docs/rate-limits#tier_t2)

## Arguments

Optional arguments

`trusted_asns`

array

_·_Optional

allow list of Autonomous System Numbers (ASN) in the enterprise grid configuarion

`trusted_cidr`

array

_·_Optional

allow list of IPv4 addressses using cidr notation in the enterprise grid configuarion

## Usage info

API to allow enterprise grid admins to write/overwrite the allow list of IP blocks and ASNs from the enterprise configuration.

_NOTE:_ Writing to this endpoint will over write previously added IP ranges and ASNs. If customers are adding new IP ranges and ASNs_they must include all of the previously added IP ranges and ASNs in your new API call or they will be overwritten._

The `token` argument is **required** and is a session token.

The `trusted_asns` argument is **optional** and is an allow list of asns as integers.

The `trusted_cidrs` argument is **optional** and is an allow list of IPs using CIDR notation as strings.

The response is a JSON string containing the field `ok`. Sucessful writes have a `true` status while unsuccessful writes that are not errors will return `false`.

## Example Request:

```
{
	"token":"xoxb-...",
	 "trusted_cidr":["8.8.8.8/24","8.8.4.4/22"],
	 "trusted_asn":[34245,8530]
	 }
```

s:

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
| `invalid_field_or_data` | 
user attempted to write and invalid field or data to the team config
 |
| `unable_to_process_post_request` | 
The user sent a malformed post request
 |
| `unsupported_action_describe` | 
The user specified describe action is unsupported
 |
| `unsupported_action_put` | 
The user specified put action is unsupported
 |
| `endpoint_unavailable` | 
The requested endpoint is currently unavailable.
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

