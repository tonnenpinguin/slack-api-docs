## Notices

**Beta API** —&nbsp;this API is in beta, and is subject to change without the usual notice period for changes.

## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/apps.manifest.create`

[Bolt for Java](/tools/bolt)
`app.client().appsManifestCreate`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.apps_manifest_create`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.apps.manifest.create`
[Powered by Bolt](/tools/bolt)

### Required scopes

[App configuration token](/authentication/config-tokens)

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

`manifest`

[manifest object as string](/reference/manifests#fields)

_·_Required

A JSON app manifest encoded as a string. This manifest **must** use a valid [app manifest schema - read our guide to creating one](/reference/manifests#fields).

## Usage info

### Using manifests 

This method accepts an app manifest as an argument. Read our [guide to app manifests](/reference/manifests) to learn how create or reuse them.

If you receive an `invalid_manifest` response when trying to use any App Manifest API, it indicates that the manifest you supplied didn't match the [correct schema](/reference/manifests#fields).

To better locate the problem with your manifest, the `invalid_manifest` error should be accompanied by an `errors` array:

```
{
	"ok": false,
	"error": "invalid_manifest",
	"errors": [
		{
			"message": "Event Subscription requires either Request URL or Socket Mode Enabled",
			"pointer": "/settings/event_subscriptions"
		},
		{
			"message": "Interactivty requires a Request URL",
			"pointer": "/settings/interactivity"
		},
		{
			"message": "Interactivity requires Socket Mode enabled",
			"pointer": "/settings/interactivity"
		}
	]
}
```

Each of the items in this array contain a `message` which describes the problem, and a `pointer` which indicates the problem's location within your supplied manifest. Use these two pieces of info to correct your manifest and try again.

## Example responses

### Common successful response

Typical success response

```
{
    "ok": true,
    "app_id": "A012ABCD0A0",
    "credentials": {
        "client_id": "...",
        "client_secret": "...",
        "verification_token": "...",
        "signing_secret": "..."
    },
    "oauth_authorize_url": "https://slack.com/oauth/v2/authorize?client_id=...&scope=commands,workflow.steps:execute"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `invalid_manifest` | 
The provided manifest file does not validate against schema. Consult the additional errors field to locate specific issues.
 |
| `invalid_app` | 
An app created from the provided manifest would not be valid.
 |
| `failed_creating_app` | 
Failed to create the app model
 |
| `failed_adding_collaborator` | 
Failed writing a collaborator record for this new app
 |
| `unknown_method` | 
Unknown method
 |
| `ratelimited` | 
The request has been ratelimited. Refer to the `Retry-After` header for when to retry the request.
 |
| `client_secret_env_key_not_found` | 
Unable to find the specified client\_secret\_env
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

