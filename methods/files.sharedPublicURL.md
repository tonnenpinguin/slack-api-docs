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
`POSThttps://slack.com/api/files.sharedPublicURL`

[Bolt for Java](/tools/bolt)
`app.client().filesSharedPublicURL`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.files_sharedPublicURL`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.files.sharedPublicURL`
[Powered by Bolt](/tools/bolt)

### Required scopes

[User tokens](/docs/token-types#user)[`files:write`](/scopes/files:write)[`files:write:user`](/scopes/files:write:user)

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

`file`

string

_·_Required

File to share

**Example**
`F1234567890`

## Usage info

This method enables public/external sharing for a file.

The response contains a [file object](/types/file), including the `permalink_public` url.

```
{
    "ok": true,
    "file": {
        "id" : "F2147483862",
        "timestamp" : 1356032811,

        "name" : "file.htm",
        "title" : "My HTML file",
        "mimetype" : "text\/plain",
        "filetype" : "text",
        "pretty_type": "Text",
        "user" : "U2147483697",

        "mode" : "hosted",
        "editable" : true,
        "is_external": false,
        "external_type": "",

        "size" : 12345,

        "url_private": "https:\/\/slack.com\/files-pri\/T024BE7LD-F024BERPE\/1.png",
        "url_private_download": "https:\/\/slack.com\/files-pri\/T024BE7LD-F024BERPE\/download\/1.png",

        "thumb_64": "https:\/\/slack-files.com\/files-tmb\/T024BE7LD-F024BERPE-c66246\/1_64.png",
        "thumb_80": "https:\/\/slack-files.com\/files-tmb\/T024BE7LD-F024BERPE-c66246\/1_80.png",
        "thumb_360": "https:\/\/slack-files.com\/files-tmb\/T024BE7LD-F024BERPE-c66246\/1_360.png",
        "thumb_360_gif": "https:\/\/slack-files.com\/files-tmb\/T024BE7LD-F024BERPE-c66246\/1_360.gif",
        "thumb_360_w": 100,
        "thumb_360_h": 100,

        "permalink": "https:\/\/coolkids.slack.com\/files\/cal\/F024BERPE\/1.png",
        "permalink_public": "https:\/\/slack-files.com\/T024BE7LD-F024BERPE-8004f909b1",
        "edit_link": "https:\/\/coolkids.slack.com\/files\/cal\/F024BERPE\/1.png/edit",
        "preview": "&lt;!DOCTYPE html&gt;\n&lt;html&gt;\n&lt;meta charset='utf-8'&gt;",
        "preview_highlight": "&lt;div class=\"sssh-code\"&gt;&lt;div class=\"sssh-line\"&gt;&lt;pre&gt;&lt;!DOCTYPE html...",
        "lines" : 123,
        "lines_more": 118,

        "is_public": true,
        "public_url_shared": false,
        "channels": ["C024BE7LT", ...],
        "groups": ["G12345", ...],
        "initial_comment": {...},
        "num_stars": 7,
        "is_starred": true
    },
    "comments": [
        {
            "id": "Fc027BN9L9",
            "timestamp": 1356032811,
            "user": "U2147483697",
            "comment": "This is a comment"
        },
        ...
    ],
    "paging": {
        "count": 100,
        "total": 2,
        "page": 1,
        "pages": 0
    }
}
```

The [file object](/types/file) contains information about the uploaded file.

Only the original creator of the file can use the `files.sharedPublicURL` method unless the file is shared into a public channel.

Each comment object in the comments array contains details about a single comment. Comments are returned oldest first.

The paging information contains the `count` of comments returned, the `total` number of comments, the `page` of results returned in this response and the total number of `pages` available. Please note that the max `count` value is `1000` and the max `page` value is `100`.

## Example responses

### Common successful response

Typical success response

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
| `not_allowed` | 
Public sharing has been disabled for this team
 |
| `public_video_not_allowed` | 
Public sharing of videos is not available. A `Free` instance of Slack may encounter this error because free teams don't have the ability to share video files publicly.
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
| `user_is_restricted` | 
This method cannot be called by a restricted user or single channel guest.
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

