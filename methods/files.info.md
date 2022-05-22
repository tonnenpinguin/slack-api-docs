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
`GEThttps://slack.com/api/files.info`

[Bolt for Java](/tools/bolt)
`app.client().filesInfo`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.files_info`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.files.info`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`files:read`](/scopes/files:read)

[User tokens](/docs/token-types#user)[`files:read`](/scopes/files:read)

[Legacy bot tokens](/docs/token-types#bot)[`bot`](/scopes/bot)

### Content types

`application/x-www-form-urlencoded`

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

`file`

string

_·_Required

Specify a file by providing its ID.

**Example**
`F2147483862`

Optional arguments

`count`

integer

_·_Optional

Number of items to return per page.

**Default**
`100`

**Example**
`20`

`cursor`

string

_·_Optional

Parameter for pagination. File comments are paginated for a single file. Set `cursor` equal to the `next_cursor` attribute returned by the previous request's `response_metadata`. This parameter is optional, but pagination is mandatory: the default value simply fetches the first "page" of the collection of comments. See [pagination](/docs/pagination) for more details.

**Example**
`dXNlcjpVMDYxTkZUVDI=`

`limit`

integer

_·_Optional

The maximum number of items to return. Fewer than the requested number of items may be returned, even if the end of the list hasn't been reached.

**Default**
`0`

**Example**
`20`

`page`

integer

_·_Optional

Page number of results to return.

**Default**
`1`

**Example**
`2`

## Usage info

This method returns information about a file in your team.

The response contains a [file object](/types/file), and a list of comment objects followed by paging information.

The [file object](/types/file) contains information about the uploaded file.

Each comment object in the comments array contains details about a single comment. Comments are returned oldest first.

The paging information contains the `count` of comments returned, the `total` number of comments, the `page` of results returned in this response and the total number of `pages` available. Please note that the max `count` value is `1000` and the max `page` value is `100`.

[Bot user tokens](/bot-users) may use this method to access information about files appearing in the channels they belong to.

## Pagination

This method uses cursor-based pagination to make it easier to incrementally collect information. To begin pagination, specify a `limit` value under `1000`. We recommend no more than `200` results at a time.

Responses will include a top-level `response_metadata` attribute containing a `next_cursor` value. By using this value as a `cursor` parameter in a subsequent request, along with `limit`, you may navigate through the collection page by virtual page.

See [pagination](/docs/pagination) for more information.

## Example responses

### Common successful response

Typical success response

```
{
    "ok": true,
    "file": {
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
        "comments_count": 0,
        "is_starred": false,
        "shares": {
            "public": {
                "C0T8SE4AU": [
                    {
                        "reply_users": [
                            "U061F7AUR"
                        ],
                        "reply_users_count": 1,
                        "reply_count": 1,
                        "ts": "1531763348.000001",
                        "thread_ts": "1531763273.000015",
                        "latest_reply": "1531763348.000001",
                        "channel_name": "file-under",
                        "team_id": "T061EG9R6"
                    }
                ]
            }
        },
        "channels": [
            "C0T8SE4AU"
        ],
        "groups": [],
        "ims": [],
        "has_rich_preview": false,
        "alt_txt": "tedair.gif"
    },
    "comments": [],
    "response_metadata": {
        "next_cursor": "dGVhbTpDMUg5UkVTR0w="
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
| `file_not_found` | 
Value passed for `file` was invalid
 |
| `file_deleted` | 
The requested file has been deleted
 |
| `access_denied` | 
Access to a resource specified in the request is denied.
 |
| `not_visible` | 
Do not have permission to view the file
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

