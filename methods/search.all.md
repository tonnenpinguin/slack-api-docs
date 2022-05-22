## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`GEThttps://slack.com/api/search.all`

[Bolt for Java](/tools/bolt)
`app.client().searchAll`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.search_all`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.search.all`
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

Search query. May contains booleans, etc.

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

This method allows users and applications to search both messages and files in a single call.

The response returns matches broken down by their type of content, similar to the facebook/gmail auto-completed search widgets.

When using a user token with this method, search results will be affected by the search filters set in the Slack UI.

Within each content group, data is returned in the following format:

```
{
    "matches": [],
    "paging": {
        "count": 100,
        "total": 15,
        "page": 1,
        "pages": 1
    }
}
```

- `count` - number of records per page
- `total` - total records matching query
- `page` - page of records returned
- `pages` - total pages matching query

This block gives the (estimated) total number of matches of this type, then has an array containing the specified page of the top matches. The format of matches depends on the match type, as described in the documentation for[search.messages](/methods/search.messages) and [search.files](/methods/search.files). These methods can be used to fetch further pages of messages or files.

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
                "created": 1508804330,
                "display_as_bot": false,
                "editable": false,
                "external_type": "",
                "filetype": "png",
                "groups": [],
                "id": "F7PKF1NR7",
                "image_exif_rotation": 1,
                "ims": [],
                "initial_comment": {
                    "comment": "Sure! Here's the workflow diagram!",
                    "created": 1508804330,
                    "id": "Fc7NLL52E7",
                    "is_intro": true,
                    "timestamp": 1508804330,
                    "user": "U2U85N1RZ"
                },
                "is_external": false,
                "is_public": true,
                "mimetype": "image/png",
                "mode": "hosted",
                "name": "slack workflow diagram.png",
                "original_h": 117,
                "original_w": 128,
                "permalink": "https://example.slack.com/files/U2U85N1RZ/F7PKF1NR7/slack_workflow_diagram.png",
                "permalink_public": "https://slack-files.com/T2U81E2FZ-F7PKF1NR7-bea9143f18",
                "pretty_type": "PNG",
                "preview": null,
                "public_url_shared": false,
                "score": "0.99982661240974",
                "size": 35705,
                "thumb_160": "https://files.slack.com/files-tmb/T2U81E2FZ-F7PKF1NR7-19f33fc256/slack_workflow_diagram_160.png",
                "thumb_360": "https://files.slack.com/files-tmb/T2U81E2FZ-F7PKF1NR7-19f33fc256/slack_workflow_diagram_360.png",
                "thumb_360_h": 117,
                "thumb_360_w": 128,
                "thumb_64": "https://files.slack.com/files-tmb/T2U81E2FZ-F7PKF1NR7-19f33fc256/slack_workflow_diagram_64.png",
                "thumb_80": "https://files.slack.com/files-tmb/T2U81E2FZ-F7PKF1NR7-19f33fc256/slack_workflow_diagram_80.png",
                "timestamp": 1508804330,
                "title": "slack workflow diagram",
                "top_file": false,
                "url_private": "https://files.slack.com/files-pri/T2U81E2FZ-F7PKF1NR7/slack_workflow_diagram.png",
                "url_private_download": "https://files.slack.com/files-pri/T2U81E2FZ-F7PKF1NR7/download/slack_workflow_diagram.png",
                "user": "U2U85N1RZ",
                "username": "amy"
            }
        ],
        "pagination": {
            "first": 1,
            "last": 1,
            "page": 1,
            "page_count": 1,
            "per_page": 20,
            "total_count": 1
        },
        "paging": {
            "count": 20,
            "page": 1,
            "pages": 1,
            "total": 1
        },
        "total": 1
    },
    "messages": {
        "matches": [
            {
                "channel": {
                    "id": "C2U86NC6M",
                    "is_ext_shared": false,
                    "is_mpim": false,
                    "is_org_shared": false,
                    "is_pending_ext_shared": false,
                    "is_private": false,
                    "is_shared": false,
                    "name": "general",
                    "pending_shared": []
                },
                "iid": "35692677-e60e-43d9-ac45-1987cea88975",
                "next": {
                    "iid": "6f510ea1-e1d3-4f3f-bdb9-f9c6f6e9d609",
                    "text": "Thanks!",
                    "ts": "1508804378.000219",
                    "type": "message",
                    "user": "U2U85HJ7R",
                    "username": "john"
                },
                "permalink": "https://example.slack.com/archives/C2U86NC6M/p1508804330000296",
                "previous": {
                    "iid": "aba8603c-0543-4fb2-9118-a5ac85f3d138",
                    "text": "Can you send me the Slack workflow diagram?",
                    "ts": "1508804301.000026",
                    "type": "message",
                    "user": "U2U85HJ7R",
                    "username": "john"
                },
                "team": "T2U81E2FZ",
                "text": "uploaded a file: <https://example.slack.com/files/U2U85N1RZ/F7PKF1NR7/slack_workflow_diagram.png|slack workflow diagram> and commented: Sure! Here's the workflow diagram!",
                "ts": "1508804330.000296",
                "type": "message",
                "user": "U2U85N1RZ",
                "username": "amy"
            }
        ],
        "pagination": {
            "first": 1,
            "last": 1,
            "page": 1,
            "page_count": 1,
            "per_page": 20,
            "total_count": 1
        },
        "paging": {
            "count": 20,
            "page": 1,
            "pages": 1,
            "total": 1
        },
        "total": 1
    },
    "ok": true,
    "posts": {
        "matches": [],
        "total": 0
    },
    "query": "diagram"
}
```

### Common error response

Typical error response

```
{
    "error": "missing_scope",
    "needed": "search:read",
    "ok": false,
    "provided": "identify,bot:basic"
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

