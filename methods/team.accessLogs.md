## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`GEThttps://slack.com/api/team.accessLogs`

[Bolt for Java](/tools/bolt)
`app.client().teamAccessLogs`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.team_accessLogs`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.team.accessLogs`
[Powered by Bolt](/tools/bolt)

### Required scopes

[User tokens](/docs/token-types#user)[`admin`](/scopes/admin)

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

Optional arguments

`before`

string

_·_Optional

End of time range of logs to include in results (inclusive).

**Default**
`now`

**Example**
`1457989166`

`count`

string

_·_Optional

Number of items to return per page.

**Default**
`100`

**Example**
`20`

`page`

string

_·_Optional

Page number of results to return.

**Default**
`1`

**Example**
`2`

`team_id`

string

_·_Optional

encoded team id to get logs from, required if org token is used

**Example**
`T1234567890`

## Usage info

This method is used to retrieve the "access logs" for users on a workspace.

Each access log entry represents a user accessing Slack from a specific user, IP address, and user agent combination.

Need to time travel? Set the `before` parameter to the oldest timestamp returned by the method to browse access logs from further back in time.

The method's paginated response contains a list of access log entries. Each item in the `logins` array represents a number of possible user interactions, collated by the user, IP address, and user agent combination. These include actual logins as well as other API calls that are typically made when accessing Slack.

Each access log entry contains the user `id` and `username` that accessed Slack.

`date_first` is a unix timestamp of the first access log entry for this user, IP address, and user agent combination.

`date_last` is the most recent for that combination.

`count` is the total number of access log entries for that combination.

`ip` is the IP address of the device used.

`user_agent` is the reported user agent string from the browser or client application.

`isp` is our _best guess_ at the internet service provider owning the IP address.

`country` and `region` are also our best guesses on where the access originated, based on the IP address.

### Pagination

The paging information contains the `count` of items returned, the `total` number of items reacted to, the `page` of results returned in this response and the total number of `pages` available. Please note that the max `count` value is `1000` and the max `page` value is `100`.

This method does not yet support cursored pagination.

## Example responses

### Common successful response

This response demonstrates pagination and two access log entries.

```
{
    "ok": true,
    "logins": [
        {
            "user_id": "U45678",
            "username": "alice",
            "date_first": 1422922864,
            "date_last": 1422922864,
            "count": 1,
            "ip": "127.0.0.1",
            "user_agent": "SlackWeb Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/41.0.2272.35 Safari/537.36",
            "isp": "BigCo ISP",
            "country": "US",
            "region": "CA"
        },
        {
            "user_id": "U12345",
            "username": "white_rabbit",
            "date_first": 1422922493,
            "date_last": 1422922493,
            "count": 1,
            "ip": "127.0.0.1",
            "user_agent": "SlackWeb Mozilla/5.0 (iPhone; CPU iPhone OS 8_1_3 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko) Version/8.0 Mobile/12B466 Safari/600.1.4",
            "isp": "BigCo ISP",
            "country": "US",
            "region": "CA"
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

A workspace must be on a paid plan to use this method, otherwise the `paid_only` error is thrown:

```
{
    "ok": false,
    "error": "paid_only"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `over_pagination_limit` | 
It is not possible to request more than 1000 items per page or more than 100 pages.
 |
| `paid_only` | 
This is only available to paid teams.
 |
| `missing_argument` | 
A required argument is missing.
 |
| `missing_scope` | 
The token used is not granted the specific scope permissions required to complete this request.
 |
| `token_revoked` | 
Authentication token is for a deleted user or workspace or the app has been removed when using a `user` token.
 |
| `not_allowed_token_type` | 
The token type used in this request is not allowed.
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

