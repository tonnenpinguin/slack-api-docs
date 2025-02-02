## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/admin.conversations.setConversationPrefs`

[Bolt for Java](/tools/bolt)
`app.client().adminConversationsSetConversationPrefs`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.admin_conversations_setConversationPrefs`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.admin.conversations.setConversationPrefs`
[Powered by Bolt](/tools/bolt)

### Required scopes

[User tokens](/docs/token-types#user)[`admin.conversations:write`](/scopes/admin.conversations:write)

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

`channel_id`

string

_·_Required

The channel to set the prefs for

**Example**
`C1234`

`prefs`

string

_·_Required

The prefs for this channel in a stringified JSON format.

**Example**
`{'who_can_post':'type:admin,user:U1234'}`

## Usage info

This [Admin API method](/admins) sets the posting permissions for a channel.

This [API method for admins](/enterprise/managing) may only be used on [Enterprise Grid](/enterprise).

This method allows you to adjust several conversation settings. To set any of these permissions, you'll use the `prefs` argument with some stringified JSON. "Stringified JSON" means JSON with white space removed and fields marked by single quotations. Since this argument won't contain more complex characters, you don't need to do further encoding.

Note: calling this method requires that you—the user associated with your app's token—have **permission to change conversation preferences**. You can see or change the users who have permission in your Org’s Channel Management settings dashboard.

### Posting permissions

To adjust who's allowed to post in a channel, use the `who_can_post` field inside your `prefs` argument:

```
"prefs": "{'who_can_post':'type:admin,user:U1234'}"
```

Inside your stringified JSON for `who_can_post`, you can specify who the permission applies to in a few different ways.

One is by `type`: you can set posting permissions to include all `admin` users, or just all `user`s in general.

Alternatively, you can specifically list users who have the permission: `user:U1234` for a single user or `user:U1234,user:U5678` for multiple users.

Note that `org_admin` can only be used if the channel is an Org-Wide Channel, otherwise you may receive the `invalid_value` error.

The `user` type is only honored when `admin` or `org_admin` is also provided.

The `can_thread` field works exactly the same inside the `prefs` object, only it determines who can respond in threads. You can pass both `who_can_post` and `can_thread` to the `prefs` argument in this method at the same time.

For example:

```
"prefs": "{'who_can_post':'type:admin,user:U1234','can_thread':'type:user'}"
```

For for fields above, you can always reset the channel to allow everyone to post by using the value `ra`:

```
"prefs": "{'who_can_post':'type:ra','can_thread':'type:ra'}"
```

#### Slack Connect

In [Slack Connect channels](/apis/channels-between-orgs), the same fields and values are used by the channel owner (home workspace) to control who can post in a given channel but they have slightly different meanings:

- `type:ra` means that all home and away workspace members and guests can post.
- `type:regular` means that only home workspace members can post but away workspace members and home/away workspace guests cannot.
- `type:admin` means that only home admins can post but home/away workspace members or guests cannot.
- `type:org_admin` means that only home org admins can post but home/away workspace members or guests cannot.

When listing individual users who can post in the channel, you are free to include user IDs of both home and away users.

### Huddles

The `can_huddle` field determines if a huddle can be started in the channel.

For example:

```
"prefs": "{'can_huddle':'false'}"
```

### Channel mentions

Channel mention restriction prefs `enable_at_channel` and `enable_at_here` can be used to place channel, here, and everyone mention restrictions in a channel.

The `enable_at_channel` field determines if channel mentions can be used in a channel. The `enable_at_here` field determines if here mentions can be used in a channel. If these channel prefs have never been set before for a channel, then the relevant mention can be used in a channel. We require that both of these prefs remain synced so if you would like to set one of these prefs, you must also update the other pref and they need to be the same value; we do this because the everyone mention restriction in general channels is also controlled by these prefs.

For example:

```
"prefs": "{'enable_at_channel':'false', 'enable_at_here':'false'}"
```

* * *

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

## Example responses

### Common successful response

Typical success response

```
{
    "ok": true
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `feature_not_enabled` | 
The Admin APIs feature is not enabled for this team.
 |
| `not_an_admin` | 
The token provided is not associated with an org admin.
 |
| `channel_not_found` | 
Value passed for `channel` was invalid.
 |
| `channel_type_not_supported` | 
Value given for `channel_id` was a #general channel.
 |
| `restricted_action` | 
A workspace preference prevents the authenticated user from archiving.
 |
| `missing_scope` | 
The token used is not granted the specific scope permissions required to complete this request.
 |
| `could_not_set_channel_pref` | 
Setting the preference or permission failed.
 |
| `default_org_wide_channel` | 
Returned when you try to modify a default org wide channel.
 |
| `channel_mention_sync_required` | 
If setting channel mention restriction prefs, both mention prefs must be passed and they must be the same value.
 |
| `invalid_value` | 
Value passed for the preferences are invalid
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

