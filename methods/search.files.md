## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`GEThttps://slack.com/api/search.files`

[Bolt for Java](/tools/bolt)
`app.client().searchFiles`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.search_files`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.search.files`
[Powered by Bolt](/tools/bolt)

### Required scopes

[User tokens](/docs/token-types#user)[`search:read`](/scopes/search:read)

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

`query`

string

_·_Required

Search query.

**Example**
`pickleface`

Optional arguments

`count`

integer

_·_Optional

Number of items to return per page.

**Default**
`20`

**Example**
`20`

`highlight`

boolean

_·_Optional

Pass a value of `true` to enable query highlight markers (see below).

**Example**
`true`

`page`

integer

_·_Optional

Page number of results to return.

**Default**
`1`

**Example**
`2`

`sort`

string

_·_Optional

Return matches sorted by either `score` or `timestamp`.

**Default**
`score`

**Example**
`timestamp`

`sort_dir`

string

_·_Optional

Change sort direction to ascending (`asc`) or descending (`desc`).

**Default**
`desc`

**Example**
`asc`

`team_id`

string

_·_Optional

encoded team id to search in, required if org token is used

**Example**
`T1234567890`

## Usage info

This method returns files matching a search query.

The response envelope contains paging and result information.

When using a user token with this method, search results will be affected by the search filters set in the Slack UI.

Matches contains a list of [file objects](/types/file).

All search methods support the `highlight` parameter. If specified, the matching query terms will be marked up in the results so that clients may replace them with appropriate highlighting markers (e.g. `<span class="highlight"></span>`). The UTF-8 markers we use are:

```
start: "\xEE\x80\x80"; # U+E000 (private-use)
end : "\xEE\x80\x81"; # U+E001 (private-use)
```

Please note that the max `count` value is `100` and the max `page` value is `100`.

## Example responses

### Common successful response

Typical success response

```
{
    "files": {
        "matches": [
            {
                "channels": [],
                "comments_count": 1,
                "created": 1507850315,
                "deanimate_gif": "https://files.slack.com/files-tmb/T2U81E2BB-F7H0D7ZBB-21624821e6/computer_deanimate_gif.png",
                "display_as_bot": false,
                "editable": false,
                "external_type": "",
                "filetype": "gif",
                "groups": [],
                "id": "F7H0D7ZBB",
                "image_exif_rotation": 1,
                "ims": [],
                "is_external": false,
                "is_public": true,
                "mimetype": "image/gif",
                "mode": "hosted",
                "name": "computer.gif",
                "original_h": 313,
                "original_w": 500,
                "permalink": "https://eventsdemo.slack.com/files/U2U85N1RZ/F7H0D7ZBB/computer.gif",
                "permalink_public": "https://slack-files.com/T2U81E2BB-F7H0D7ZBB-85b7f5557e",
                "pretty_type": "GIF",
                "preview": null,
                "public_url_shared": false,
                "reactions": [
                    {
                        "count": 1,
                        "name": "stuck_out_tongue_winking_eye",
                        "users": [
                            "U2U85N1RZ"
                        ]
                    }
                ],
                "score": "0.38899223746309",
                "size": 1639034,
                "thumb_160": "https://files.slack.com/files-tmb/T2U81E2BB-F7H0D7ZBB-21624821e6/computer_160.png",
                "thumb_360": "https://files.slack.com/files-tmb/T2U81E2BB-F7H0D7ZBB-21624821e6/computer_360.png",
                "thumb_360_gif": "https://files.slack.com/files-tmb/T2U81E2BB-F7H0D7ZBB-21624821e6/computer_360.gif",
                "thumb_360_h": 225,
                "thumb_360_w": 360,
                "thumb_480": "https://files.slack.com/files-tmb/T2U81E2BB-F7H0D7ZBB-21624821e6/computer_480.png",
                "thumb_480_gif": "https://files.slack.com/files-tmb/T2U81E2BB-F7H0D7ZBB-21624821e6/computer_480.gif",
                "thumb_480_h": 300,
                "thumb_480_w": 480,
                "thumb_64": "https://files.slack.com/files-tmb/T2U81E2BB-F7H0D7ZBB-21624821e6/computer_64.png",
                "thumb_80": "https://files.slack.com/files-tmb/T2U81E2BB-F7H0D7ZBB-21624821e6/computer_80.png",
                "timestamp": 1507850315,
                "title": "computer.gif",
                "top_file": false,
                "url_private": "https://files.slack.com/files-pri/T2U81E2BB-F7H0D7ZBB/computer.gif",
                "url_private_download": "https://files.slack.com/files-pri/T2U81E2BB-F7H0D7ZBB/download/computer.gif",
                "user": "U2U85N1RZ",
                "username": ""
            }
        ],
        "pagination": {
            "first": 1,
            "last": 3,
            "page": 1,
            "page_count": 1,
            "per_page": 20,
            "total_count": 3
        },
        "paging": {
            "count": 20,
            "page": 1,
            "pages": 1,
            "total": 3
        },
        "total": 3
    },
    "ok": true,
    "query": "computer.gif"
}
```

### Common error response

Typical error response

```
{
    "error": "No query passed",
    "ok": false
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
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

