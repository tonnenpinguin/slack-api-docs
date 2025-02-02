## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`GEThttps://slack.com/api/migration.exchange`

[Bolt for Java](/tools/bolt)
`app.client().migrationExchange`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.migration_exchange`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.migration.exchange`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`tokens.basic`](/scopes/tokens.basic)

[User tokens](/docs/token-types#user)[`tokens.basic`](/scopes/tokens.basic)

[Legacy bot tokens](/docs/token-types#bot)[`bot`](/scopes/bot)

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

`users`

array

_·_Required

A comma-separated list of user ids, up to 400 per request

Optional arguments

`team_id`

string

_·_Optional

Specify team\_id starts with `T` in case of Org Token

**Example**
`T1234567890`

`to_old`

boolean

_·_Optional

Specify `true` to convert `W` global user IDs to workspace-specific `U` IDs. Defaults to `false`.

**Default**
``

**Example**
`true`

## Usage info

Easily convert your vintage user IDs to [Enterprise Grid](/enterprise-grid)-friendly global user IDs.

This method is best used in conjunction with turning off the [translation layer](/enterprise-grid#translation_layer) for your app, as a bulk conversion step just after a workspace migrates to Enterprise Grid.

By providing a list of "local" user IDs associated with the same workspace as your token, you can exchange your IDs for "global" user IDs beginning with the letter `W` or `U`.

You can use any existing tokens authorized for the team to request for the user mappings.

The `to_old` parameter is `false` by default. When `false`, the method returns a `user_id_map` mapping from local user IDs to global user IDs. For a reverse mapping from _global_ user IDs back to _local_ user IDs, set `to_old` to `true`.

The method may only be used on workspaces that have migrated to enterprise. When used on typical workspaces, a `not_enterprise_team` error is thrown.

## Additional nuance

Users that were already part of a workspace migrating to Enterprise Grid have two user IDs: a local user ID and a global user ID. Users that are created post-migration or on workspaces that are created _after_ migration have only global user IDs.

When using this method and attempting to convert a global user ID to a local user ID and that corresponding user _only_ has a global user ID, you'll receive the global user ID on both sides of the map.

Providing invalid users or user IDs not belonging to the related workspace will result with those IDs being listed in an `invalid_user_ids` array.

## Example responses

### Common successful response

Typical success response when mappings exist for the specified user IDs

```
{
    "ok": true,
    "team_id": "T1KR7PE1W",
    "enterprise_id": "E1KQTNXE1",
    "user_id_map": {
        "U06UBSUN5": "W06M56XJM",
        "U06UEB62U": "W06PTT6GH",
        "U06UBSVB3": "W06PUUDLY",
        "U06UBSVDX": "W06PUUDMW",
        "W06UAZ65Q": "W06UAZ65Q"
    },
    "invalid_user_ids": [
        "U21ABZZXX"
    ]
}
```

### Common error response

Typical error response when there are no mappings to provide

```
{
    "ok": false,
    "error": "not_enterprise_team"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_enterprise_team` | 
The workspace associated with the token is not part of an Enterprise organization. User IDs have not changed and there is nothing to map.
 |
| `too_many_users` | 
Too many user IDs provided in `users`. Up to 400 user IDs are allowed per request.
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

