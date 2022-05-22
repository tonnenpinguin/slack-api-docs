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
`GEThttps://slack.com/api/files.list`

[Bolt for Java](/tools/bolt)
`app.client().filesList`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.files_list`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.files.list`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`files:read`](/scopes/files:read)

[User tokens](/docs/token-types#user)[`files:read`](/scopes/files:read)

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

`channel`

string

_·_Optional

Filter files appearing in a specific channel, indicated by its ID.

**Example**
`C1234567890`

`count`

integer

_·_Optional

Number of items to return per page.

**Default**
`100`

**Example**
`20`

`files`

string

_·_Optional

`page`

integer

_·_Optional

Page number of results to return.

**Default**
`1`

**Example**
`2`

`show_files_hidden_by_limit`

boolean

_·_Optional

Show truncated file info for files hidden due to being too old, and the team who owns the file being over the file limit.

**Example**
`true`

`team_id`

string

_·_Optional

encoded team id to list files in, required if org token is used

**Example**
`T1234567890`

`ts_from`

string

_·_Optional

Filter files created after this timestamp (inclusive).

**Default**
`0`

**Example**
`123456789`

`ts_to`

string

_·_Optional

Filter files created before this timestamp (inclusive).

**Default**
`now`

**Example**
`123456789`

`types`

string

_·_Optional

Filter files by type ([see below](#file_types)). You can pass multiple values in the types argument, like `types=spaces,snippets`.The default value is `all`, which does not filter the list.

**Default**
`all`

**Example**
`images`

`user`

string

_·_Optional

Filter files created by a single user.

**Example**
`W1234567890`

## Usage info

This method returns a list of files within the team. It can be filtered and sliced in various ways.

The response contains a list of [file objects](/types/file), followed by paging information.

In order to gather information on [tombstoned files](/changelog/2019-03-wild-west-for-files-no-more) in Free workspaces, so that you can delete or revoke them, pass the `show_files_hidden_by_limit` parameter. While the yielded files will still be redacted, you'll gain the `id` of the files so that you can delete or revoke them.

### Types 

The file types you may encounter include (but are not limited to):

- `all` - All files
- `spaces` - Posts
- `snippets` - Snippets
- `images` - Image files
- `gdocs` - Google docs
- `zips` - Zip files
- `pdfs` - PDF files

## Example responses

### Common successful response

Typical success response

```
{
    "ok": true,
    "files": [
        {
            "id": "F0S43P1CZ",
            "created": 1531763254,
            "timestamp": 1531763254,
            "name": "billair.gif",
            "title": "billair.gif",
            "mimetype": "image/gif",
            "filetype": "gif",
            "pretty_type": "GIF",
            "user": "U061F7AUR",
            "editable": false,
            "size": 144538,
            "mode": "hosted",
            "is_external": false,
            "external_type": "",
            "is_public": true,
            "public_url_shared": false,
            "display_as_bot": false,
            "username": "",
            "url_private": "https://.../billair.gif",
            "url_private_download": "https://.../billair.gif",
            "thumb_64": "https://.../billair_64.png",
            "thumb_80": "https://.../billair_80.png",
            "thumb_360": "https://.../billair_360.png",
            "thumb_360_w": 176,
            "thumb_360_h": 226,
            "thumb_160": "https://.../billair_=_160.png",
            "thumb_360_gif": "https://.../billair_360.gif",
            "image_exif_rotation": 1,
            "original_w": 176,
            "original_h": 226,
            "deanimate_gif": "https://.../billair_deanimate_gif.png",
            "pjpeg": "https://.../billair_pjpeg.jpg",
            "permalink": "https://.../billair.gif",
            "permalink_public": "https://.../...",
            "channels": [
                "C0T8SE4AU"
            ],
            "groups": [],
            "ims": [],
            "comments_count": 0
        },
        {
            "id": "F0S43PZDF",
            "created": 1531763342,
            "timestamp": 1531763342,
            "name": "tedair.gif",
            "title": "tedair.gif",
            "mimetype": "image/gif",
            "filetype": "gif",
            "pretty_type": "GIF",
            "user": "U061F7AUR",
            "editable": false,
            "size": 137531,
            "mode": "hosted",
            "is_external": false,
            "external_type": "",
            "is_public": true,
            "public_url_shared": false,
            "display_as_bot": false,
            "username": "",
            "url_private": "https://.../tedair.gif",
            "url_private_download": "https://.../tedair.gif",
            "thumb_64": "https://.../tedair_64.png",
            "thumb_80": "https://.../tedair_80.png",
            "thumb_360": "https://.../tedair_360.png",
            "thumb_360_w": 176,
            "thumb_360_h": 226,
            "thumb_160": "https://.../tedair_=_160.png",
            "thumb_360_gif": "https://.../tedair_360.gif",
            "image_exif_rotation": 1,
            "original_w": 176,
            "original_h": 226,
            "deanimate_gif": "https://.../tedair_deanimate_gif.png",
            "pjpeg": "https://.../tedair_pjpeg.jpg",
            "permalink": "https://.../tedair.gif",
            "permalink_public": "https://.../...",
            "channels": [
                "C0T8SE4AU"
            ],
            "groups": [],
            "ims": [],
            "comments_count": 0
        }
    ],
    "paging": {
        "count": 100,
        "total": 2,
        "page": 1,
        "pages": 1
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
| `user_not_found` | 
Value passed for `user` was invalid
 |
| `unknown_type` | 
Value passed for `types` was invalid
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

