## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/chat.unfurl`

[Bolt for Java](/tools/bolt)
`app.client().chatUnfurl`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.chat_unfurl`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.chat.unfurl`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`links:write`](/scopes/links:write)

[User tokens](/docs/token-types#user)[`links:write`](/scopes/links:write)

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

`channel`

string

_·_Required

Channel ID of the message. Both `channel` and `ts` must be provided together, _or_ `unfurl_id` and `source` must be provided together.

**Example**
`C1234567890`

`ts`

string

_·_Required

Timestamp of the message to add unfurl behavior to.

`unfurls`

string

_·_Required

URL-encoded JSON map with keys set to URLs featured in the the message, pointing to their unfurl blocks or message attachments.

Optional arguments

`source`

string

_·_Optional

The source of the link to unfurl. The source may either be `composer`, when the link is inside the message composer, or `conversations_history`, when the link has been posted to a conversation.

**Example**
`composer`

`unfurl_id`

string

_·_Optional

The ID of the link to unfurl. Both `unfurl_id` and `source` must be provided together, _or_ `channel` and `ts` must be provided together.

**Example**
`Uxxxxxxx-909b5454-75f8-4ac4-b325-1b40e230bbd8`

`user_auth_blocks`

_·_Optional

Provide a JSON based array of structured blocks presented as URL-encoded string to send as an ephemeral message to the user as invitation to authenticate further and enable full unfurling behavior

`user_auth_message`

null

_·_Optional

Provide a simply-formatted string to send as an ephemeral message to the user as invitation to authenticate further and enable full unfurling behavior. Provides two buttons, `Not now` or `Never ask me again`.

`user_auth_required`

boolean

_·_Optional

Set to `true` or `1` to indicate the user must install your Slack app to trigger unfurls for this domain

**Default**
`0`

**Example**
`true`

`user_auth_url`

null

_·_Optional

Send users to this custom URL where they will complete authentication in your app to fully trigger unfurling. Value should be properly URL-encoded.

**Example**
`https://example.com/onboarding?user_id=xxx`

## Usage info

This method [unfurls](/docs/message-link-unfurling#slack_app_unfurling) a link—either in the message composer or in a posted message.

This method supports both granular bot and user tokens. The bot token is recommended.

Both `unfurl_id` and `source` must be provided together, _or_ `channel` and `ts` must be provided together. `unfurl_id` and `source` are the preferred choice.

The first time this method is executed with a particular `ts` and `channel` (or `unfurl_id` and `source`) combination, the valid `unfurls` attachments you provide will be attached to the message. Subsequent attempts with the same `ts` and `channel` values will modify the same attachments, rather than adding more.

The `ts` value you supply must correspond to a message in the specified `channel`. Also, the message must contain a fully-qualified URL pointing to a domain that is already registered and associated with your Slack app.

The `unfurls` parameter expects a URL-encoded string of JSON. Unlike [`chat.postMessage`](/methods/chat.postMessage)'s `attachments` parameter, it does not expect a JSON array but instead, a hash keyed on the specific URLs you're offering an unfurl for. Each URL can have a [single attachment](/docs/message-attachments), including message buttons.

You can define your own preview for a link that you're unfurling inside the message composer by passing the `preview` field:

```
"unfurls": {
  "https://example.com": {
    "blocks": [...],
    "preview": {
        "title": {
          "type": "plain_text",
          "text": "custom preview"
        },
        "icon_url":
          "https://images.pexels.com/photos/774731/pexels-photo-774731.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500"
      }
    }
}
```

This `preview` field is optional, and if the preview is not provided, we will generate the preview based on the blocks layout or legacy attachment.

If the preview is provided in an unfurl for a **posted message** , rather than a link inside the message composer, we will simply ignore this property.

Read the [unfurling](/docs/message-link-unfurling#slack_app_unfurling) docs for [more guidance](/docs/message-link-unfurling#unfurls_parameter) on this parameter.

The `user_auth_required` parameter is optional. By providing a `1` or `true` value, it will require the user posting the link first authenticate themselves with your app. See also the [authenticated unfurling docs](/docs/message-link-unfurling#authenticated_unfurls).

If you'd rather directly point users to a specific page on your server to authenticate, pass a valid URL using the `user_auth_url` parameter. When sending this parameter via `application/x-www-form-urlencoded` GETs or POSTs, values must be URL-encoded such that `https://example.com/onboarding?user_id=xxx` becomes `https%3A%2F%2Fexample.com%2Fonboarding%3Fuser_id%3Dxxx`.

Or, you can send an ephemeral message to that user by providing a simple string-based `user_auth_message` value or JSON array of blocks using `user_auth_blocks`. [Simple slack message formatting](/docs/message-formatting) like `*bold*`, `_italics_`, and linking is supported, so you can wrap your custom URLs in a blanket of situationally accurate, actionable text.

`user_auth_message` offers two default buttons, `Not now` and `Never ask me again` which allows your app to prompt a user multiple times before opting out of an install. To make your ephemeral message extra fancy, you can also use `user_auth_blocks` which will override the default buttons. Using both properties shows the `user_auth_message` in a notification and the `user_auth_blocks` in the ephemeral message.

Specifying `user_auth_url` or `user_auth_message` will automatically imply `user_auth_required` being set to `true`. If both `user_auth_url` and `user_auth_message` are provided, `user_auth_message` takes precedence.

As you can see, we provide a minimal positive response when your unfurl attempt is successful. When it is not, you'll receive one of the [errors](#errors) below and `ok` will be `false`.

## Example responses

### Common successful response

Typical, minimal success response

```
{
    "ok": true
}
```

### Common error response

Typical error response

```
{
    "ok": false,
    "error": "cannot_unfurl_url"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `missing_channel` | 
The request is missing the `channel` parameter
 |
| `missing_ts` | 
The request is missing the `ts` parameter
 |
| `cannot_find_channel` | 
The specified channel could not be located for this token.
 |
| `cannot_find_service` | 
A record of your app being allowed to unfurl for this workspace could not be found.
 |
| `cannot_parse_attachment` | 
The provided `unfurls` argument could not be parsed or understood.
 |
| `missing_unfurls` | 
The request is missing the `unfurls` parameter.
 |
| `invalid_unfurls_format` | 
The `unfurls` parameter cannot be JSON-decoded into a map of URLs to attachments.
 |
| `cannot_unfurl_url` | 
The URL cannot be unfurled. This error may be returned if you haven't acknowledged a `link_shared` event tied to the same URL. It is also returned when the domain appears in a workspace's administrative blocklists.
 |
| `cannot_prompt` | 
The current user has already interacted with and dismissed a prompt for this application.
 |
| `cannot_find_message` | 
The `ts` value in the request does not match a message.
 |
| `cannot_unfurl_message` | 
The URL cannot be unfurled because the URL provided does not appear in the message.
 |
| `cannot_auth_user` | 
The current user cannot be authenticated.
 |
| `missing_unfurl_id` | 
The request is missing the `unfurl_id` parameter.
 |
| `missing_source` | 
The request is missing the `source` parameter.
 |
| `invalid_unfurl_id` | 
The unfurl ID is invalid.
 |
| `invalid_source` | 
The unfurl source is invalid.
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

