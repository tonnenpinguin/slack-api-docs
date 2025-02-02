## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/dialog.open`

[Bolt for Java](/tools/bolt)
`app.client().dialogOpen`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.dialog_open`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.dialog.open`
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

`dialog`

string

_·_Required

The dialog definition. This must be a JSON-encoded string.

`trigger_id`

string

_·_Required

Exchange a trigger to post to the user.

**Example**
`12345.98765.abcd2358fdea`

## Usage info

Open a dialog with a user by exchanging a `trigger_id` received from another interaction. See the [dialogs](/dialogs) documentation to learn how to obtain triggers define form elements.

As with all of our Web API methods, `dialog.open` primarily takes URL-encoded parameters as arguments. Similar to [`chat.postMessage`](/methods/chat.postMessage), [`chat.unfurl`](/methods/chat.unful), and [`chat.update`](/methods/chat.update) this method also includes a parameter that expects a JSON object encoded with `application/x-www-form-urlencoded`.

A simple form you might create could be modeled in JSON as:

```
{
  "callback_id": "ryde-46e2b0",
  "title": "Request a Ride",
  "submit_label": "Request",
  "state": "Limo",
  "elements": [
    {
      "type": "text",
      "label": "Pickup Location",
      "name": "loc_origin"
    },
    {
      "type": "text",
      "label": "Dropoff Location",
      "name": "loc_destination"
    }
  ]
}
```

To prepare that as a HTTP POST to `dialog.open`, you'd optionally minify and then URL encode that JSON to a single string, displayed below as the value for the `dialog` POST body parameter.

```
POST /api/dialog.open
token=xoxb-such-and-such
&trigger_id=13345224609.738474920.8088930838d88f008e0
&dialog=%7B%22callback_id%22%3A%22ryde-46e2b0%22%2C%22title%22%3A%22Request%20a%20Ride%22%2C%22submit_label%22%3A%22Request%22%2C%22state%22%3A%22Limo%22%2C%22elements%22%3A%5B%7B%22type%22%3A%22text%22%2C%22label%22%3A%22Pickup%20Location%22%2C%22name%22%3A%22loc_origin%22%7D%2C%7B%22type%22%3A%22text%22%2C%22label%22%3A%22Dropoff%20Location%22%2C%22name%22%3A%22loc_destination%22%7D%5D%7D
```

You can avoid this nonsense by [posting JSON instead](/web#posting_json).

Assuming your submitted dialog elements were properly formatted, valid, and the `trigger_id` was viable, you will receive a minimal success response.

If your form was invalid, Slack will try to provide a detailed accounting of why and provide the `validation_errors` error. Look for the `response_metadata` node and the `messages` array contained within for human-readable validation errors.

When we encounter an error with a specific form element, we'll indicate which element by a zero-indexed `integer`: `0` indicates the first provided element, `1` the second, and so on.

## Example responses

### Common successful response

Typical success response is quite minimal.

```
{
    "ok": true
}
```

### Common error response

Typical error response, before getting to any possible validation errors.

```
{
    "ok": false,
    "error": "missing_trigger"
}
```

### Alternate response

The impossibility of all possible errors expressed in a single response:

```
{
    "ok": false,
    "error": "validation_errors",
    "response_metadata": {
        "messages": [
            "The field `title` is required",
            "The field `callback_id` is required",
            "The field `elements` is required",
            "The field `title` cannot be longer than 24 characters",
            "The field `submit_button` cannot be longer than 24 characters",
            "The field `submit_button` can only be one word",
            "The field `callback_id` cannot be longer than 255 characters",
            "The field `state` cannot be longer than 3000 characters",
            "The field `elements` must be a list",
            "The field `elements` has to include at least one element",
            "The field `elements` cannot include more than ten elements",
            "Element 0 must be an object",
            "Element 1 is missing required field `type`",
            "Element 2 has an invalid `type`",
            "Element 3 is missing required field `name`",
            "Element 4 is missing required field `label`",
            "Element 5 has a repeated name",
            "Element 6 field `name` cannot be longer than 300 characters",
            "Element 7 field `hint` cannot be longer than 150 characters",
            "Element 8 field `label` cannot be longer than 48 characters",
            "Element 9 has an invalid `subtype`",
            "Element 10 field `min_length` must be between 0 and 150 (inclusive)",
            "Element 11 field `max_length` must be between 1 and 150 (inclusive)",
            "Element 12 field `value` cannot be longer than 150 characters",
            "Element 13 field `placeholder` cannot be longer than 150 characters",
            "Element 14 field `min_length` must be between 0 and 500 (inclusive)",
            "Element 14 field `max_length` must be between 1 and 500 (inclusive)",
            "Element 15 field `value` cannot be longer than 500 characters",
            "Element 16 field `placeholder` cannot be longer than 150 characters",
            "Element 17 field `placeholder` cannot be longer than 150 characters",
            "Element 18 field `options` must be a list",
            "Element 19 field `options` must have at least one option",
            "Element 20 field `options` must have fewer than 100 options",
            "Element 21 `options` item is missing required field `label`",
            "Element 22 `options` item is missing required field `value`",
            "Element 23 `options` item `label` field cannot exceed 75 characters",
            "Element 24 `options` item `value` field cannot exceed 75 characters"
        ]
    }
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `validation_errors` | 
The provided `dialog` could not be validated.
 |
| `trigger_exchanged` | 
The provided `trigger_id` has already been exchanged.
 |
| `failed_sending_dialog` | 
Something exceptional occurred and the dialog could not be sent.
 |
| `missing_dialog` | 
No `dialog` argument was presented.
 |
| `app_missing_action_url` | 
The app associated with the used token doesn't have a Action URL configured in its interactive components settings.
 |
| `cannot_create_dialog` | 
Something exceptional occurred and the dialog could not be created.
 |
| `missing_trigger` | 
No `trigger_id` argument was presented.
 |
| `trigger_expired` | 
The provided `trigger_id` was presented after the 3 second limit.
 |
| `invalid_trigger` | 
The provided `trigger_id` is invalid or cannot be used by this token.
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

