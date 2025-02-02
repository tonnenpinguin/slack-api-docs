## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/admin.conversations.search`

[Bolt for Java](/tools/bolt)
`app.client().adminConversationsSearch`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.admin_conversations_search`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.admin.conversations.search`
[Powered by Bolt](/tools/bolt)

### Required scopes

[User tokens](/docs/token-types#user)[`admin.conversations:read`](/scopes/admin.conversations:read)

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

Optional arguments

`connected_team_ids`

array

_·_Optional

Array of encoded team IDs, signifying the external orgs to search through.

**Example**
`['T00000000','T00000001']`

`cursor`

string

_·_Optional

Set `cursor` to `next_cursor` returned by the previous call to list items in the next page.

**Example**
`dXNlcjpVMEc5V0ZYTlo=`

`limit`

integer

_·_Optional

Maximum number of items to be returned. Must be between 1 - 20 both inclusive. Default is 10.

**Default**
`10`

**Example**
`20`

`query`

string

_·_Optional

Name of the the channel to query by.

**Example**
`announcement`

`search_channel_types`

array

_·_Optional

The type of channel to include or exclude in the search. For example `private` will search private channels, while `private_exclude` will exclude them. For a full list of types, check the [Types section](#types).

**Example**
`private,archived`

`sort`

string

_·_Optional

Possible values are `relevant` (search ranking based on what we think is closest), `name` (alphabetical), `member_count` (number of users in the channel), and `created` (date channel was created). You can optionally pair this with the `sort_dir` arg to change how it is sorted

**Default**
`member_count`

**Example**
`name`

`sort_dir`

string

_·_Optional

Sort direction. Possible values are `asc` for ascending order like (1, 2, 3) or (a, b, c), and `desc` for descending order like (3, 2, 1) or (c, b, a)

**Default**
`desc`

**Example**
`asc`

`team_ids`

array

_·_Optional

Comma separated string of team IDs, signifying the internal workspaces to search through.

**Example**
`T00000000,T00000001`

## Usage info

This [Admin API method](/admins) searches for channels across an organization given a search query.

This [API method for admins](/enterprise/managing) may only be used on [Enterprise Grid](/enterprise).

This `admin` scope is obtained through [version two of the OAuth V2 flow](/authentication/oauth-v2), but there are a few additional requirements. The app requesting this scope **must** be [installed](/start/overview#installing_distributing) by an _ **admin or Owner** _ of an Enterprise Grid organization. Also, the app must be installed on the **entire** org, not on an individual workspace. See below for more details.

If the app is installed by an Org Admin or Owner, ensure the Channel Management settings provide the appropriate permissions.

Admin API endpoints reach across **an entire Enterprise Grid organization** , not individual workspaces.

For a token to be imbued with Admin scopes, it must be obtained from installing an app on the **entire Grid org** , not just a workspace within the organization.

To configure and install an app supporting Admin API endpoints on your Enterprise Grid organization:

1. [Create a new Slack app](/apps). Your app will need to be able to handle a standard [OAuth 2 flow](/docs/oauth#flow).
2. In the app's settings, select **OAuth & Permissions** from the left navigation. Scroll down to the section titled **Scopes** and add the `admin.*` scope you want. Click the green **Save Changes** button.
3. In the app's settings, select **Manage Distribution** from the left navigation. Under the section titled **Share Your App with Other Workspaces** , make sure all four sections have the green check. Then click the green **Activate Public Distribution** button.
4. Under the **Share Your App with Your Workspace** section, copy the **Sharable URL** and paste it into a browser to initiate the OAuth handshake that will install the app on your organization. You must be logged in as an **admin or Owner** of your Enterprise Grid organization to install the app.
5. Check the dropdown in the upper right of the installation screen to make sure you are installing the app on the organization, not an individual workspace within the organization. See the image below for a visual.
6. Once your app completes the OAuth flow, you will be granted an OAuth token that can be used for calling Admin API methods for your organization.
 ![](https://a.slack-edge.com/80588/img/api/workspace-v-org-audit.png)
_When installing an app to use an Admin API endpoint, be sure to install it on your Grid organization, not a workspace within the organization._

### More on channel types 

The `search_channel_types` allows an array of types to be passed, each of which filters the channels that the search returns. If you pass multiple types, the search occurs in channels that satisfy **any** of the types. You can pass the following values in your list of `search_channel_types`:

- `private`
- `private_exclude`
- `archived`
- `exclude_archived`
- `private_exclude_archived`
- `multi_workspace`
- `org_wide`
- `external_shared_exclude`
- `external_shared`
- `external_shared_private`
- `external_shared_archived`

`private` will search private channels, and `private_exclude` will exclude them from the search. The other names follow the same patterns for channels connected to other organizations and archived channels.

The `member_count` field may return `-1` when the count cannot be retrieved for performance reasons. Counts of `0` and above indicate current membership.

## Example responses

### Common successful response

Typical success response

```
{
    "ok": true,
    "conversations": [
        {
            "id": "GSEV0B5PY",
            "name": "privacy-channel",
            "purpose": "Group messaging with: @rita @nwhere @meanie",
            "member_count": -1,
            "created": 1578423973,
            "creator_id": "WPQ65MVKK",
            "is_private": true,
            "is_archived": true,
            "is_general": false,
            "last_activity_ts": 1583198954000200,
            "is_ext_shared": false,
            "is_global_shared": true,
            "is_org_default": false,
            "is_org_mandatory": false,
            "is_org_shared": true,
            "is_frozen": false,
            "connected_team_ids": [],
            "internal_team_ids_count": 4,
            "internal_team_ids_sample_team": "T013F30DBAB",
            "pending_connected_team_ids": [],
            "is_pending_ext_shared": false
        },
        {
            "id": "C013JDPD6CR",
            "name": "proj-decomposed-monolith",
            "purpose": "",
            "member_count": 1,
            "created": 1588786531,
            "creator_id": "WPQ65MVKK",
            "is_private": false,
            "is_archived": false,
            "is_general": false,
            "last_activity_ts": 1589854024000200,
            "is_ext_shared": false,
            "is_global_shared": false,
            "is_org_default": false,
            "is_org_mandatory": false,
            "is_org_shared": true,
            "is_frozen": false,
            "connected_team_ids": [],
            "internal_team_ids_count": 1,
            "internal_team_ids_sample_team": "TPQ67R81F",
            "pending_connected_team_ids": [],
            "is_pending_ext_shared": false
        }
    ],
    "next_cursor": "aWQ6Mw=="
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `feature_not_enabled` | 
The token provided doesn't have access to this method.
 |
| `not_an_admin` | 
The token provided is not associated with an Org Admin or Owner.
 |
| `not_an_enterprise` | 
This endpoint can only be called by an Enterprise organization.
 |
| `team_not_found` | 
One of the workspaces provided in the list wasn't found.
 |
| `connected_team_passed_in_is_not_top_level_team` | 
One of the orgs provided in the external connected teams filter is not a top level team.
 |
| `external_team_not_connected_to_this_org` | 
One of the teams provided in the external connected teams filter is not connected to the org.
 |
| `not_allowed` | 
The authenticated user does not have the permission to call this method.
 |
| `invalid_auth` | 
Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request.
 |
| `invalid_cursor` | 
The provided cursor is not valid, often due to not urlencoding query parameters.
 |
| `invalid_search_channel_type` | 
An invalid `search_channel_types` arg was passed. Make sure there are no spaces between your args and that each is one of the enumerated options listed above.
 |
| `invalid_sort` | 
The provided `sort` argument wasn't valid.
 |
| `invalid_sort_dir` | 
The provided `sort_dir` argument wasn't valid.
 |
| `not_authed` | 
No authentication token provided.
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

