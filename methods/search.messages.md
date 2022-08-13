## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`GEThttps://slack.com/api/search.messages`

[Bolt for Java](/tools/bolt)
`app.client().searchMessages`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.search_messages`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.search.messages`
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

`cursor`

string

_·_Optional

Use this when getting results with cursormark pagination. For first call send `*` for subsequent calls, send the value of `next_cursor` returned in the previous call's results

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

This method returns messages matching a search query.

The matching items are returned as hashes containing contextual messages. This response envelope also contains paging and result information.

When using a user token with this method, search results will be affected by the search filters set in the Slack UI.

When a search query matches multiple messages in close proximity to one another, only one match will be returned. Using the `highlights=true` parameter, you can identify which items match the query, and which are provided for context only.

Note: Previously, search results might be returned with matches on the `previous`, `previous_2`, `next`, or `next_2` messages, but not the message itself. However, the `previous`, `previous_2`, `next`, or `next_2` fields are now deprecated and will no longer be provided in responses beginning December 3, 2020.

If more than one search term is provided, users and channels are also matched at a lower priority. To specifically search within a channel, group, or DM, add `in:channel_name`, `in:group_name`, or `in:<@UserID>`. To search for messages from a specific speaker, add `from:<@UserID>` or`from:botname`.

For IM results, the `type` is set to `"im"` and the `channel.name` property contains the user ID of the target user. For private group results, type is set to `"group"`.

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
    "messages": {
        "matches": [
            {
                "channel": {
                    "id": "C12345678",
                    "is_ext_shared": false,
                    "is_mpim": false,
                    "is_org_shared": false,
                    "is_pending_ext_shared": false,
                    "is_private": false,
                    "is_shared": false,
                    "name": "general",
                    "pending_shared": []
                },
                "iid": "cb64bdaa-c1e8-4631-8a91-0f78080113e9",
                "permalink": "https://hitchhikers.slack.com/archives/C12345678/p1508284197000015",
                "team": "T12345678",
                "text": "The meaning of life the universe and everything is 42.",
                "ts": "1508284197.000015",
                "type": "message",
                "user": "U2U85N1RV",
                "username": "roach"
            },
            {
                "channel": {
                    "id": "C12345678",
                    "is_ext_shared": false,
                    "is_mpim": false,
                    "is_org_shared": false,
                    "is_pending_ext_shared": false,
                    "is_private": false,
                    "is_shared": false,
                    "name": "random",
                    "pending_shared": []
                },
                "iid": "9a00d3c9-bd2d-45b0-988b-6cff99ae2a90",
                "permalink": "https://hitchhikers.slack.com/archives/C12345678/p1508795665000236",
                "team": "T12345678",
                "text": "The meaning of life the universe and everything is 101010",
                "ts": "1508795665.000236",
                "type": "message",
                "user": "",
                "username": "robot overlord"
            }
        ],
        "pagination": {
            "first": 1,
            "last": 2,
            "page": 1,
            "page_count": 1,
            "per_page": 20,
            "total_count": 2
        },
        "paging": {
            "count": 20,
            "page": 1,
            "pages": 1,
            "total": 2
        },
        "total": 2
    },
    "ok": true,
    "query": "The meaning of life the universe and everything"
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

