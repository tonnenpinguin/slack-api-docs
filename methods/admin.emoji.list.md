## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`GEThttps://slack.com/api/admin.emoji.list`

[Bolt for Java](/tools/bolt)
`app.client().adminEmojiList`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.admin_emoji_list`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.admin.emoji.list`
[Powered by Bolt](/tools/bolt)

### Required scopes

[User tokens](/docs/token-types#user)[`admin.teams:read`](/scopes/admin.teams:read)

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

`cursor`

string

_·_Optional

Set `cursor` to `next_cursor` returned by the previous call to list items in the next page

**Example**
`5c3e53d5`

`limit`

integer

_·_Optional

The maximum number of items to return. Must be between 1 - 1000 both inclusive.

**Default**
`100`

**Example**
`100`

## Usage info

This [Admin API method](/enterprise/managing) lists the emoji across an Enterprise Grid organization.

If you're looking to list emoji in a single workspace, use the regular [`emoji.list`](/methods/emoji.list) method. Also, this Admin method only lists _custom_ emoji. Use the regular `emoji.list` method with the `include_categories` boolean to see standard emoji included with Slack.

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

The `cache_ts` field in the response indicates the last time the emoji set was updated. You can use it to cache this list.

## Example responses

### Common successful response

Typical success response

```
{
    "ok": true,
    "emoji": {
        "workflow": {
            "url": "https://emoji.slack-edge.com/TM315QLU8/workflow/530de66adccc59c5.png",
            "date_created": 1591720632,
            "uploaded_by": "WLWLQDAL9"
        },
        "welcome": {
            "url": "https://emoji.slack-edge.com/TM315QLU8/welcome/763d3659699d2ef7.gif",
            "date_created": 1593383451,
            "uploaded_by": "WPU7MCTFH"
        },
        "person": {
            "url": "https://emoji.slack-edge.com/TM315QLU8/person/81295a4f69d8b122.png",
            "date_created": 1593383817,
            "uploaded_by": "WPU7MCTFH"
        },
        "people": {
            "url": "https://emoji.slack-edge.com/TM315QLU8/people/0b40796ab677b47f.png",
            "date_created": 1593383822,
            "uploaded_by": "WPU7MCTFH"
        },
        "slackbot": {
            "url": "https://emoji.slack-edge.com/TM315QLU8/slackbot/561d6e545263d92b.png",
            "date_created": 1593383989,
            "uploaded_by": "WPU7MCTFH"
        },
        "plus1": {
            "url": "https://emoji.slack-edge.com/TM315QLU8/plus1/42b92e57a79eb27e.png",
            "date_created": 1593724572,
            "uploaded_by": "WPU7MCTFH"
        },
        "bc": {
            "url": "https://emoji.slack-edge.com/TM315QLU8/bc/fb3dfdea697528b9.png",
            "date_created": 1594854289,
            "uploaded_by": "WPU7MCTFH"
        },
        "wf": {
            "url": "https://emoji.slack-edge.com/TM315QLU8/wf/04dad3aa28b57cd3.png",
            "date_created": 1594854443,
            "uploaded_by": "WPU7MCTFH"
        },
        "kb": {
            "url": "https://emoji.slack-edge.com/TM315QLU8/kb/bab417c375703f7b.png",
            "date_created": 1598467537,
            "uploaded_by": "WPU7MCTFH"
        },
        "ignore": {
            "url": "https://emoji.slack-edge.com/TM315QLU8/ignore/9506cda43addbad8.png",
            "date_created": 1598467835,
            "uploaded_by": "WPU7MCTFH"
        }
    },
    "response_metadata": {
        "next_cursor": ""
    }
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `invalid_cursor` | 
Value passed for `cursor` was not valid or is no longer valid.
 |
| `feature_not_enabled` | 
The Admin APIs feature is not enabled for this team.
 |
| `not_an_admin` | 
This method is only accessible by org owners and Admins.
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

