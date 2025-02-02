## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/views.push`

[Bolt for Java](/tools/bolt)
`app.client().viewsPush`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.views_push`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.views.push`
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

`trigger_id`

string

_·_Required

Exchange a trigger to post to the user.

**Example**
`12345.98765.abcd2358fdea`

`view`

[view as string](/reference/surfaces/views)

_·_Required

A [view payload](/reference/surfaces/views). This must be a JSON-encoded string.

## Usage info

Push a new view onto the existing view stack by passing a view object and a valid `trigger_id` generated from an interaction within the existing modal. The pushed view is added to the top of the stack, so the user will go back to the previous view after they complete or cancel the pushed view.

After a modal is opened, the app is limited to pushing 2 additional views.

Read the [modals](/block-kit/surfaces/modals) documentation to learn more about the lifecycle and intricacies of views.

If you pass a valid view object along with a valid `trigger_id`, you'll receive a success response with the view object that was pushed to the stack.

## Example responses

### Common successful response

Typical success response includes the pushed view payload.

```
{
    "ok": true,
    "view": {
        "id": "VNM522E2U",
        "team_id": "T9M4RL1JM",
        "type": "modal",
        "title": {
            "type": "plain_text",
            "text": "Pushed Modal",
            "emoji": true
        },
        "close": {
            "type": "plain_text",
            "text": "Back",
            "emoji": true
        },
        "submit": {
            "type": "plain_text",
            "text": "Save",
            "emoji": true
        },
        "blocks": [
            {
                "type": "input",
                "block_id": "edit_details",
                "element": {
                    "type": "plain_text_input",
                    "action_id": "detail_input"
                },
                "label": {
                    "type": "plain_text",
                    "text": "Edit details"
                }
            }
        ],
        "private_metadata": "",
        "callback_id": "view_4",
        "external_id": "",
        "state": {
            "values": {}
        },
        "hash": "1569362015.55b5e41b",
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
    "error": "invalid_arguments",
    "response_metadata": {
        "messages": [
            "missing required field: title"
        ]
    }
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `duplicate_external_id` | 
Error returned when the given `external_id` has already be used.
 |
| `not_found` | 
Error returned when the requested view can't be found.
 |
| `push_limit_reached` | 
Error returned when the max push limit has been reached for views. Currently the limit is 3.
 |
| `view_too_large` | 
Error returned if the provided view is greater than 250kb.
 |
| `invalid_trigger_id` | 
The trigger\_id is invalid. The expected format for the trigger\_id argument is "132456.7890123.abcdef".
 |
| `expired_trigger_id` | 
The trigger\_id is expired.
 |
| `exchanged_trigger_id` | 
The trigger\_id was already exchanged in a previous call.
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

