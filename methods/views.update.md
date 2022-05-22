## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/views.update`

[Bolt for Java](/tools/bolt)
`app.client().viewsUpdate`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.views_update`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.views.update`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)No scope required

[User tokens](/docs/token-types#user)No scope required

[Legacy bot tokens](/docs/token-types#bot)[`bot`](/scopes/bot)

### Content types

`application/x-www-form-urlencoded`[`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON")

- 
### Rate limits
[Tier 4](/docs/rate-limits#tier_t4)

## Arguments

Required arguments

`token`

[token](/authentication/token-types)

_·_Required

Authentication token bearing required scopes. Tokens should be passed as an HTTP Authorization header or alternatively, as a POST parameter.

**Example**
`xxxx-xxxxxxxxx-xxxx`

`view`

[view as string](/reference/surfaces/views)

_·_Required

A [view object](/reference/surfaces/views). This must be a JSON-encoded string.

_One of_`external_id``view_id`

Exactly one of these arguments is required to identify the view that is to be updated.

`external_id`

string

A unique identifier of the view set by the developer. Must be unique for all views on a team. Max length of 255 characters. Either `view_id` or `external_id` is required.

**Example**
`bmarley_view2`

`view_id`

string

A unique identifier of the view to be updated. Either `view_id` or `external_id` is required.

**Example**
`VMM512F2U`

Optional arguments

`hash`

string

_·_Optional

A string that represents view state to protect against possible race conditions.

**Example**
`156772938.1827394`

## Usage info

Update a view by passing a new view definition object along with the `view_id` returned in [`views.open`](/methods/views.open) or the `external_id`. See the [modals](/surfaces/modals/using#updating_apis) documentation to learn more about updating views and avoiding race conditions with the `hash` argument.

**Preserving `input` entry**  

Data entered or selected in `input` blocks can be preserved while updating views. The new `view` object that you use with `views.update` should contain the same input blocks and elements with identical `block_id` and `action_id` values.

If you pass a valid `view` object along with a `view_id` or `external_id`, you'll receive a success response with the updated payload.

## Example responses

### Common successful response

Typical success response includes the updated view payload.

```
{
    "ok": true,
    "view": {
        "id": "VNM522E2U",
        "team_id": "T9M4RL1JM",
        "type": "modal",
        "title": {
            "type": "plain_text",
            "text": "Updated Modal",
            "emoji": true
        },
        "close": {
            "type": "plain_text",
            "text": "Close",
            "emoji": true
        },
        "submit": null,
        "blocks": [
            {
                "type": "section",
                "block_id": "s_block",
                "text": {
                    "type": "plain_text",
                    "text": "I am but an updated modal",
                    "emoji": true
                },
                "accessory": {
                    "type": "button",
                    "action_id": "button_4",
                    "text": {
                        "type": "plain_text",
                        "text": "Click me"
                    }
                }
            }
        ],
        "private_metadata": "",
        "callback_id": "view_2",
        "external_id": "",
        "state": {
            "values": {}
        },
        "hash": "1569262015.55b5e41b",
        "clear_on_close": true,
        "notify_on_close": false,
        "root_view_id": "VNN729E3U",
        "previous_view_id": null,
        "app_id": "AAD3351BQ",
        "bot_id": "BADF7A34H"
    }
}
```

### Common error response

Typical error response.

```
{
    "ok": false,
    "error": "not_found"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `hash_conflict` | 
Error returned when the provided `hash` doesn't match the current stored value.
 |
| `not_found` | 
Error returned when the given `view_id` or `external_id` doesn't exist.
 |
| `duplicate_external_id` | 
Error returned when the given `external_id` has already be used.
 |
| `view_too_large` | 
Error returned if the provided view is greater than 250kb.
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

