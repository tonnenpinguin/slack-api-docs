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
`POSThttps://slack.com/api/files.upload`

[Bolt for Java](/tools/bolt)
`app.client().filesUpload`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.files_upload`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.files.upload`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`files:write`](/scopes/files:write)

[User tokens](/docs/token-types#user)[`files:write`](/scopes/files:write)[`files:write:user`](/scopes/files:write:user)

[Legacy bot tokens](/docs/token-types#bot)[`bot`](/scopes/bot)

### Content types

`multipart/form-data``application/x-www-form-urlencoded`

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

`channels`

string

_·_Optional

Comma-separated list of channel names or IDs where the file will be shared.

**Example**
`C1234567890,C2345678901,C3456789012`

`content`

string

_·_Optional

File contents via a POST variable. If omitting this parameter, you must provide a `file`.

**Example**
`...`

`file`

_·_Optional

File contents via `multipart/form-data`. If omitting this parameter, you must submit `content`.

**Example**
`...`

`filename`

string

_·_Optional

Filename of file.

**Example**
`foo.txt`

`filetype`

string

_·_Optional

A [file type](/types/file#file_types) identifier.

**Example**
`php`

`initial_comment`

string

_·_Optional

The message text introducing the file in specified `channels`.

**Example**
`Best!`

`thread_ts`

string

_·_Optional

Provide another message's `ts` value to upload this file as a reply. Never use a reply's `ts` value; use its parent instead.

**Example**
`1234567890.123456`

`title`

string

_·_Optional

Title of file.

**Example**
`My File`

## Usage info

This method allows you to create or upload an existing file.

**You must provide either a `file` or `content` parameter.**

The content of the file can either be posted using an `enctype` of `multipart/form-data` (with the file parameter named `file`), in the usual way that files are uploaded via the browser, or the content of the file can be sent as a POST var called `content`. The latter should be used for creating a "file" from a long message/paste and forces "editable" mode.

In both cases, the type of data in the file will be intuited from the filename and the magic bytes in the file, for supported formats.

Sending a valid `filetype` parameter will override this behavior. Possible `filetype` values can be found in the [`file` object definition](/types/file#file_types).

Files uploaded via `multipart/form-data` will be stored either in hosted or editable mode, based on certain heuristics (determined type, file size).

The file can also be shared directly into channels on upload, by specifying an optional argument `channels`. If there's more than one channel name or ID in the `channels` string, they should be comma-separated. The owner of the token used to upload the file must also be a member of any channel you wish to post to this way.

There is a 1 megabyte file size limit for files uploaded as snippets.

Upload files and images into [message threads](/docs/message-threading) by providing the thread parent's `ts` value with the `thread_ts` parameter.

The `initial_comment` field is used in messages to introduce the file in conversation.

If you only specify a `filename`, we remove the file extension and populate the file's `title` with the remaining value. So, if you want the file's extension to be shown in Slack, you must include it in the `title` argument when uploading it.

By default all newly-uploaded files are private and only visible to the owner. They become public once they are shared into a public channel (which can happen at upload time via the `channels` argument).

If successful, the response will include a [file object](/types/file).

## Examples

Upload "dramacat.gif" from the current directory and share it in two channels, using `multipart/form-data`:

```
curl -F file=@dramacat.gif -F "initial_comment=Shakes the cat" -F channels=C024BE91L,D032AC32T -H "Authorization: Bearer xoxb-xxxxxxxxx-xxxx" https://slack.com/api/files.upload
```

Create an editable text file containing the text "launch plan":

```
curl -F "content=launch plan" -H "Authorization: Bearer xoxb-xxxxxxxxx-xxxx" https://slack.com/api/files.upload
```

Upload another image to an existing message thread:

```
curl -F file=@drworm.gif -F "initial_comment=I play the drums." -F channels=C024BE91L -F thread_ts=1532293503.000001 -H "Authorization: Bearer xoxp-xxxxxxxxx-xxxx" https://slack.com/api/files.upload
```

## Example responses

### Common successful response

Success response after uploading a file to a channel with an initial message

```
{
    "ok": true,
    "file": {
        "id": "F0TD00400",
        "created": 1532293501,
        "timestamp": 1532293501,
        "name": "dramacat.gif",
        "title": "dramacat",
        "mimetype": "image/jpeg",
        "filetype": "gif",
        "pretty_type": "JPEG",
        "user": "U0L4B9NSU",
        "editable": false,
        "size": 43518,
        "mode": "hosted",
        "is_external": false,
        "external_type": "",
        "is_public": false,
        "public_url_shared": false,
        "display_as_bot": false,
        "username": "",
        "url_private": "https://.../dramacat.gif",
        "url_private_download": "https://.../dramacat.gif",
        "thumb_64": "https://.../dramacat_64.gif",
        "thumb_80": "https://.../dramacat_80.gif",
        "thumb_360": "https://.../dramacat_360.gif",
        "thumb_360_w": 360,
        "thumb_360_h": 250,
        "thumb_480": "https://.../dramacat_480.gif",
        "thumb_480_w": 480,
        "thumb_480_h": 334,
        "thumb_160": "https://.../dramacat_160.gif",
        "image_exif_rotation": 1,
        "original_w": 526,
        "original_h": 366,
        "permalink": "https://.../dramacat.gif",
        "permalink_public": "https://.../More-Path-Components",
        "comments_count": 0,
        "is_starred": false,
        "shares": {
            "private": {
                "D0L4B9P0Q": [
                    {
                        "reply_users": [],
                        "reply_users_count": 0,
                        "reply_count": 0,
                        "ts": "1532293503.000001"
                    }
                ]
            }
        },
        "channels": [],
        "groups": [],
        "ims": [
            "D0L4B9P0Q"
        ],
        "has_rich_preview": false
    }
}
```

### Alternate response

Uploading a file with the `content` parameter creates an editable `text/plain` file by default

```
{
    "ok": true,
    "file": {
        "id": "F0TD0GUTS",
        "created": 1532294750,
        "timestamp": 1532294750,
        "name": "-.txt",
        "title": "Untitled",
        "mimetype": "text/plain",
        "filetype": "text",
        "pretty_type": "Plain Text",
        "user": "U0L4B9NSU",
        "editable": true,
        "size": 11,
        "mode": "snippet",
        "is_external": false,
        "external_type": "",
        "is_public": true,
        "public_url_shared": false,
        "display_as_bot": false,
        "username": "",
        "url_private": "https://.../.txt",
        "url_private_download": "https://...download/-.txt",
        "permalink": "https://.../.txt",
        "permalink_public": "https://.../.txt",
        "edit_link": "https://.../.txt/edit",
        "preview": "launch plan",
        "preview_highlight": "<div class=\"CodeMirror cm-s-default CodeMirrorServer\" oncopy=\"if(event.clipboardData){event.clipboardData.setData('text/plain',window.getSelection().toString().replace(/\\u200b/g,''));event.preventDefault();event.stopPropagation();}\">\n<div class=\"CodeMirror-code\">\n<div><pre>launch plan</pre></div>\n</div>\n</div>\n",
        "lines": 1,
        "lines_more": 0,
        "preview_is_truncated": false,
        "comments_count": 0,
        "is_starred": false,
        "shares": {
            "public": {
                "C061EG9SL": [
                    {
                        "reply_users": [],
                        "reply_users_count": 0,
                        "reply_count": 0,
                        "ts": "1532294750.000001",
                        "channel_name": "general",
                        "team_id": "T061EG9R6"
                    }
                ]
            }
        },
        "channels": [
            "C061EG9SL"
        ],
        "groups": [],
        "ims": [],
        "has_rich_preview": false
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
| `posting_to_general_channel_denied` | 
An admin has restricted posting to the #general channel.
 |
| `invalid_channel` | 
One or more channels supplied are invalid
 |
| `not_in_channel` | 
Authenticated user is not in the channel.
 |
| `channel_not_found` | 
At least one of the values passed for `channels` was invalid.
 |
| `slack_connect_file_upload_sharing_blocked` | 
Admin has disabled File uploads in all Slack Connect communications
 |
| `slack_connect_clip_sharing_blocked` | 
Admin has disabled Clip sharing in Slack Connect channels
 |
| `slack_connect_blocked_file_type` | 
File uploads with certain types are blocked in all Slack Connect communications
 |
| `malware_detected` | 
This file may contain a virus or other malware and can't be uploaded to Slack
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

