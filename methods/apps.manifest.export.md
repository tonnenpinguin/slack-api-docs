## Notices

**Beta API** —&nbsp;this API is in beta, and is subject to change without the usual notice period for changes.

## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/apps.manifest.export`

[Bolt for Java](/tools/bolt)
`app.client().appsManifestExport`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.apps_manifest_export`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.apps.manifest.export`
[Powered by Bolt](/tools/bolt)

### Required scopes

[App configuration token](/authentication/config-tokens)

### Content types

`application/x-www-form-urlencoded`[`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON")

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

`app_id`

_·_Required

The ID of the app whose configuration you want to export as a manifest.

## Usage info

There is no documentation for this method.

Sorry! There aren't any sample responses for this endpoint right now.

## Example responses

### Common successful response

Typical success response

```
{
    "ok": true,
    "manifest": {
        "_metadata": {
            "major_version": 1,
            "minor_version": 1
        },
        "display_information": {
            "name": "Zork",
            "description": "You are likely to be eaten by a grue.",
            "background_color": "#0000AA",
            "long_description": "Play the Infocom classic text adventure and find your way to the end of the maze. ZORK is a game of adventure, danger, and low cunning. In it you will explore some of the most amazing territory ever seen by mortals. No workspace should be without one!"
        },
        "features": {
            "app_home": {
                "home_tab_enabled": true,
                "messages_tab_enabled": false,
                "messages_tab_read_only_enabled": false
            },
            "bot_user": {
                "display_name": "zork",
                "always_online": true
            },
            "slash_commands": [
                {
                    "command": "/zork",
                    "description": "You are standing in an open field west of a white house, with a boarded front door. There is a small mailbox here.",
                    "usage_hint": "/zork open mailbox",
                    "should_escape": false
                }
            ],
            "workflow_steps": [
                {
                    "name": "Example step",
                    "callback_id": "tutorial_example_step"
                }
            ]
        },
        "oauth_config": {
            "redirect_urls": [
                "https://example.com/slack/auth"
            ],
            "scopes": {
                "bot": [
                    "commands",
                    "workflow.steps:execute"
                ]
            }
        },
        "settings": {
            "event_subscriptions": {
                "bot_events": [
                    "workflow_step_execute"
                ]
            },
            "interactivity": {
                "is_enabled": true
            },
            "org_deploy_enabled": false,
            "socket_mode_enabled": true,
            "is_hosted": false,
            "token_rotation_enabled": false
        }
    }
}
```

### Common error response

Typical error response if invalid app ID used

```
{
    "ok": false,
    "error": "invalid_app_id"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `invalid_app_id` | 
The app ID passed is invalid.
 |
| `app_not_eligible` | 
The specified app is not elgible for this API.
 |
| `app_not_found` | 
The specified app was not found.
 |
| `unknown_method` | 
This method does not exist.
 |
| `failed_export` | 
Failed to export manifest for given app ID
 |
| `no_permission` | 
The workspace token used in this request does not have the permissions necessary to complete the request. Make sure your app is a member of the conversation it's attempting to post a message to.
 |
| `internal_error` | 
The server could not complete your operation(s) without encountering an error, likely due to a transient issue on our end. It's possible some aspect of the operation succeeded before the error was raised.
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

