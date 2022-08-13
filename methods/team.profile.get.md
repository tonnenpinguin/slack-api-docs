## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`GEThttps://slack.com/api/team.profile.get`

[Bolt for Java](/tools/bolt)
`app.client().teamProfileGet`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.team_profile_get`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.team.profile.get`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`users.profile:read`](/scopes/users.profile:read)

[User tokens](/docs/token-types#user)[`users.profile:read`](/scopes/users.profile:read)

### Content types

`application/x-www-form-urlencoded`

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

Optional arguments

`visibility`

string

_·_Optional

Filter by visibility.

**Example**
`all`

## Usage info

This method is used to get the profile field definitions for this team.

The optional `visibility` argument allows the client to filter the profile fields based on visibility. The following values are supported:

- `all` is the default, and returns all fields.
- `visible` returns only fields for which the `is_hidden` option is missing or false.
- `hidden` returns only fields for which the `is_hidden` option is true.

The response contains a `profile` item with an array of key value pairs. Right now only the `fields` key is supported, which contains a list of field definitions for this team.

There are two ways to update a field. The value of `is_scim` determines which method to use:

- If `is_scim` is `True`, update the field via a SCIM API call
- If `is_scim` is `False`, update the field via [`users.profile.set`](/methods/users.profile.set). You'll need its `id` to do so.

| Field | Type | Description |
| --- | --- | --- |
| `id` | String | The ID of either the section or field |
| `is_protected` | Boolean | |
| `is_scim` | Boolean | If true, can be updated via SCIM APIs |
| `field_name` | String | The name of the field |
| `hint` | String | Any additional context the user may need to understand the field |
| `label` | String | The text that will appear under the field or section |
| `possible_values` | String | The values that allowed to be chosen by the user |
| `options` | String | An object containing the `is_protected` and `is_scim` fields |
| `ordering` | Integer | The placement of the field or section on the profile |
| `section_id` | String | The `id` of the section the field is in |
| `section_type` | String | The type of content in the section. Users can only create `custom` section types |
| `type` | String | The format the field supports. Can be `file`, `text`, `user` and ... |

## Example responses

### Common successful response

Typical success response

```
{
    "ok": true,
    "profile": {
        "fields": [
            {
                "id": "111111ABC",
                "ordering": 0,
                "label": "Phone extension",
                "hint": "Enter the extension to reach your desk",
                "type": "text",
                "possible_values": null,
                "options": {
                    "is_scim": true,
                    "is_protected": true
                },
                "is_hidden": false,
                "section_id": "123ABC"
            },
            {
                "id": "222222ABC",
                "ordering": 1,
                "label": "Date of birth",
                "hint": "When you were born",
                "type": "date",
                "possible_values": null,
                "options": {
                    "is_scim": true,
                    "is_protected": true
                },
                "is_hidden": true,
                "section_id": "123ABC"
            },
            {
                "id": "333333ABC",
                "ordering": 2,
                "label": "House",
                "hint": "Put on the sorting hat",
                "type": "options_list",
                "possible_values": [
                    "Gryffindor",
                    "Hufflepuff",
                    "Ravenclaw",
                    "Slytherin"
                ],
                "options": {
                    "is_scim": false,
                    "is_protected": false
                },
                "is_hidden": false,
                "section_id": "456DEF"
            }
        ],
        "sections": [
            {
                "id": "123ABC",
                "team_id": "T123456",
                "section_type": "contact",
                "label": "Contact Information",
                "order": 1,
                "is_hidden": true
            },
            {
                "id": "456DEF",
                "team_id": "T123456",
                "section_type": "custom",
                "label": "About Me",
                "order": 2,
                "is_hidden": true
            }
        ]
    }
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

