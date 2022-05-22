## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`GEThttps://slack.com/api/usergroups.list`

[Bolt for Java](/tools/bolt)
`app.client().usergroupsList`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.usergroups_list`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.usergroups.list`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`usergroups:read`](/scopes/usergroups:read)

[User tokens](/docs/token-types#user)[`usergroups:read`](/scopes/usergroups:read)

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

Optional arguments

`include_count`

boolean

_·_Optional

Include the number of users in each User Group.

**Example**
`true`

`include_disabled`

boolean

_·_Optional

Include disabled User Groups.

**Example**
`true`

`include_users`

boolean

_·_Optional

Include the list of users for each User Group.

**Example**
`true`

`team_id`

string

_·_Optional

encoded team id to list user groups in, required if org token is used

**Example**
`T1234567890`

## Usage info

This method returns a list of all User Groups in the team. This can optionally include disabled User Groups.

Returns a list of [usergroup objects](/types/usergroup), in no particular order.

## Example responses

### Common successful response

Typical success response

```
{
    "ok": true,
    "usergroups": [
        {
            "id": "S0614TZR7",
            "team_id": "T060RNRCH",
            "is_usergroup": true,
            "name": "Team Admins",
            "description": "A group of all Administrators on your team.",
            "handle": "admins",
            "is_external": false,
            "date_create": 1446598059,
            "date_update": 1446670362,
            "date_delete": 0,
            "auto_type": "admin",
            "created_by": "USLACKBOT",
            "updated_by": "U060RNRCZ",
            "deleted_by": null,
            "prefs": {
                "channels": [],
                "groups": []
            },
            "user_count": "2"
        },
        {
            "id": "S06158AV7",
            "team_id": "T060RNRCH",
            "is_usergroup": true,
            "name": "Team Owners",
            "description": "A group of all Owners on your team.",
            "handle": "owners",
            "is_external": false,
            "date_create": 1446678371,
            "date_update": 1446678371,
            "date_delete": 0,
            "auto_type": "owner",
            "created_by": "USLACKBOT",
            "updated_by": "USLACKBOT",
            "deleted_by": null,
            "prefs": {
                "channels": [],
                "groups": []
            },
            "user_count": "1"
        },
        {
            "id": "S0615G0KT",
            "team_id": "T060RNRCH",
            "is_usergroup": true,
            "name": "Marketing Team",
            "description": "Marketing gurus, PR experts and product advocates.",
            "handle": "marketing-team",
            "is_external": false,
            "date_create": 1446746793,
            "date_update": 1446747767,
            "date_delete": 1446748865,
            "auto_type": null,
            "created_by": "U060RNRCZ",
            "updated_by": "U060RNRCZ",
            "deleted_by": null,
            "prefs": {
                "channels": [],
                "groups": []
            },
            "user_count": "0"
        }
    ]
}
```

### Common error response

Typical error response

```
{
    "ok": false,
    "error": "invalid_auth"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `plan_upgrade_required` | 
This workspace does not have access to User Groups, as that feature is only available on Standard and above plans.
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

