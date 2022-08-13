## Notices

**File threads are here**

A new file commenting experience arrived on July 23, 2018. [**Learn more**](/changelog/2018-05-file-threads-soon-tread) about what's new and the migration path for apps already working with files and file comments.

## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`GEThttps://slack.com/api/files.remote.add`

[Bolt for Java](/tools/bolt)
`app.client().filesRemoteAdd`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.files_remote_add`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.files.remote.add`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`remote_files:write`](/scopes/remote_files:write)

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

`external_id`

string

_·_Required

Creator defined GUID for the file.

**Example**
`123456`

`external_url`

string

_·_Required

URL of the remote file.

**Example**
`http://example.com/my_cloud_service_file/abc123`

`title`

string

_·_Required

Title of the file being shared.

**Example**
`Danger, High Voltage!`

Optional arguments

`filetype`

string

_·_Optional

type of file

**Example**
`doc`

`indexable_file_contents`

_·_Optional

A text file (txt, pdf, doc, etc.) containing textual search terms that are used to improve discovery of the remote file.

**Example**
`...`

`preview_image`

_·_Optional

Preview of the document via `multipart/form-data`.

**Example**
`...`

## Usage info

The `add` method adds a remote file to Slack. Adding a file **does not** share it to a channel. To make your beautiful remote file visible, use the [`files.remote.share`](/methods/files.remote.share) method.

Remote files exist across the whole workspace (or organization, for Enterprise Grid). Because of that, remote files _must_ be added by bots with the [`bot` scope](/scopes/bot), not by an individual user. Dimension requirements for previews are a minimum of `800w x 400h`.

`preview_image` is displayed when your remote file is [shared](/methods/files.remote.share). It's a binary image file.

`external_id` is the unique ID of the remote file, **according to** the external host of the file.

`indexable_file_contents` is a text file that represents the file for search. When a user searches in Slack, their query will be compared against the contents of this text file for matching. Think of this text file like the `alt` parameter on an HTML `<img>` tag — a textual representation of a non-textual object. The text file can contain a description of the remote file, or it can contain search keywords, or anything else text-based.

Note: this method performs an upsert. If you add a file that has been added before, the existing file will be updated.

```
{
        "ok": true,
        "file": {
            "id": "F0GDJ3XMH",
            "created": 1563919925,
            "timestamp": 1563919925,
            "name": "LeadvilleAndBackAgain",
            "title": "LeadvilleAndBackAgain",
            "mimetype": "application/vnd.slack-remote",
            "filetype": "remote",
            "pretty_type": "Remote",
            "user": "U0F8RBVNF",
            "editable": false,
            "size": 0,
            "mode": "external",
            "is_external": true,
            "external_type": "app",
            "is_public": false,
            "public_url_shared": false,
            "display_as_bot": false,
            "username": "",
            "url_private": "https://docs.google.com/document/d/1TA9fIaph4eSz2fC_1JGMuYaYUc4IvieIop0WqfCXw5Y/edit?usp=sharing",
            "permalink": "https://kraneflannel.slack.com/files/U0F8RBVNF/F0GDJ3XMH/leadvilleandbackagain",
            "comments_count": 0,
            "is_starred": false,
            "shares": {},
            "channels": [],
            "groups": [],
            "ims": [],
            "external_id": "1234",
            "external_url": "https://docs.google.com/document/d/1TA9fIaph4eSz2fC_1JGMuYaYUc4IvieIop0WqfCXw5Y/edit?usp=sharing",
            "has_rich_preview": false
        }
    }
```

One tricky bit: in the response, the file object may indicate that `"has_rich_preview"` is `false`, even if you include `preview_image`. That's because it takes a few seconds for Slack to parse the `preview_image` you pass. If you call `files.remote.add` with the same `external_id` later, you'll see `"has_preview_image": true`.

## Example responses

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `bad_image` | 
The uploaded image could not be processed - try passing a JPG or PNG
 |
| `too_large` | 
The uploaded image had excessive dimensions
 |
| `invalid_external_id` | 
The external\_id provided is too long.
 |
| `bad_title` | 
The title provided is too long.
 |
| `bot_user_required` | 
bot user token is required
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

