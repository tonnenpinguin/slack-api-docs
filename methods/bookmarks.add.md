## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/bookmarks.add`

[Bolt for Java](/tools/bolt)
`app.client().bookmarksAdd`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.bookmarks_add`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.bookmarks.add`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`bookmarks:write`](/scopes/bookmarks:write)

[User tokens](/docs/token-types#user)[`bookmarks:write`](/scopes/bookmarks:write)

### Content types

`application/x-www-form-urlencoded`[`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON")

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

`channel_id`

string

_·_Required

Channel to add bookmark in.

**Example**
`C1234567890`

`title`

string

_·_Required

Title for the bookmark.

`type`

string

_·_Required

Type of the bookmark i.e link.

Optional arguments

`emoji`

string

_·_Optional

Emoji tag to apply to the link.

`entity_id`

string

_·_Optional

ID of the entity being bookmarked. Only applies to message and file types.

`link`

string

_·_Optional

Link to bookmark.

`parent_id`

string

_·_Optional

Id of this bookmark's parent

## Usage info

Add bookmark to a channel.

**Note**  
At this current moment, the Bookmarks API only accepts the `link` type. Acceptance of the remaining types coming soon!

[bookmarks.add](/methods/bookmarks.add) currently accepts the following types:

- `link`

Stay tuned for more bookmark `types` coming soon.

Bookmarking a `link` requires; `channel_id`, `title`, `type`, and an `url`.

Example of a POST request to `bookmarks.add` for a `link`:

```
https://slack.com/api/bookmarks.add?channel_id=C123TGZ4XYX&title=bookmark-test&type=link&link=http%3A%2F%2Fslack.com
```

## Example responses

### Common successful response

Typical success response

```
{
    "ok": true,
    "bookmark": {
        "id": "Bk033XFJ9BTJ",
        "channel_id": "C1RQ000",
        "title": "bookmark-1",
        "link": "https://google.com",
        "emoji": ":clap:",
        "icon_url": "https://www.google.com/favicon.ico",
        "type": "link",
        "entity_id": null,
        "date_created": 1644956055,
        "date_updated": 0,
        "rank": "g",
        "last_updated_by_user_id": "U0334B6G6G5",
        "last_updated_by_team_id": "T018DF03GHY",
        "shortcut_id": null,
        "app_id": null
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
| `access_denied` | 
Access to a resource specified in the request is denied.
 |
| `not_implemented` | 
bookmarking not available for the user.
 |
| `invalid_link` | 
Invalid link, link should begin with either http:// or https://.
 |
| `invalid_emoji` | 
Invalid emoji, does not follow the pattern of a valid emoji name.
 |
| `invalid_entity_id` | 
Invalid entity\_id, file or message type bookmark should have original file or message ID.
 |
| `file_not_found` | 
File cannot be found.
 |
| `channel_not_found` | 
Channel cannot be found.
 |
| `permission_denied` | 
No permission to perform this operation.
 |
| `too_many_bookmarks` | 
Bookmark limit reached for channel.
 |
| `parent_with_link` | 
Parent bookmark should not have link.
 |
| `parent_bookmark_disabled` | 
Parent bookmark feature flag is off.
 |
| `invalid_parent_type` | 
Parent type is not valid.
 |
| `invalid_child_type` | 
Child type is not valid.
 |
| `invalid_bookmark_type` | 
Bookmark type is not valid.
 |
| `not_authed` | 
No authentication token provided.
 |
| `invalid_auth` | 
Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request.
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

