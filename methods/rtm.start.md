## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`GEThttps://slack.com/api/rtm.start`

[Bolt for Java](/tools/bolt)
`app.client().rtmStart`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.rtm_start`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.rtm.start`
[Powered by Bolt](/tools/bolt)

### Required scopes

[User tokens](/docs/token-types#user)[`client`](/scopes/client)

[Legacy bot tokens](/docs/token-types#bot)[`bot`](/scopes/bot)

### Content types

`application/x-www-form-urlencoded`

- 
### Rate limits
[Tier 1](/docs/rate-limits#tier_t1)

## Arguments

Required arguments

`token`

[token](/authentication/token-types)

_·_Required

Authentication token bearing required scopes. Tokens should be passed as an HTTP Authorization header or alternatively, as a POST parameter.

**Example**
`xxxx-xxxxxxxxx-xxxx`

Optional arguments

`batch_presence_aware`

boolean

_·_Optional

Batch presence deliveries via subscription. Enabling changes the shape of `presence_change` events. See [batch presence](/docs/presence-and-status#batching).

**Default**
`false`

**Example**
`1`

`include_locale`

boolean

_·_Optional

Set this to `true` to receive the locale for users and channels. Defaults to `false`

**Example**
`true`

`mpim_aware`

boolean

_·_Optional

Returns MPIMs to the client in the API response.

**Example**
`true`

`no_latest`

boolean

_·_Optional

Exclude latest timestamps for channels, groups, mpims, and ims. Automatically sets `no_unreads` to `1`

**Default**
`0`

**Example**
`1`

`no_unreads`

boolean

_·_Optional

Skip unread counts for each channel (improves performance).

**Example**
`true`

`presence_sub`

boolean

_·_Optional

Only deliver presence events when requested by subscription. See [presence subscriptions](/docs/presence-and-status#subscriptions).

**Default**
`true`

**Example**
`true`

`simple_latest`

boolean

_·_Optional

Return timestamp only for latest message object of each channel (improves performance).

**Example**
`true`

## Usage info

**DEPRECATED** : [Learn more about this method's deprecation.](/changelog/2021-10-rtm-start-to-stop)

This method began a Real Time Messaging API session and reserved your application a specific URL with which to connect via websocket.

It's user-centric and team-centric: your app connects _as_ a specific user or bot user on a specific team. Many apps will find the [Events API](/events-api)'s subscription model more scalable when working against multiple teams.

This method also returns a smorgasbord of data about the team, its channels, and members. Some times more information than can be provided in a timely or helpful manner.

Please use [`rtm.connect`](/methods/rtm.connect) instead, especially when connecting on behalf of an [Enterprise Grid](/enterprise-grid) customer.

Consult the [RTM API documentation](/rtm) for full details on using the RTM API.

The `members` array found in this and other methods will begin automatically truncating at 1,500 and eventually fewer results beginning December 1, 2017. As of March, 2018 the cap is 500. Please Use [`conversations.members`](/methods/conversations.members) to manage memberships instead. [Read on to learn more.](/changelog/2017-10-members-array-truncating)

[New Slack apps](/authentication/basics) may not use any Real Time Messaging API method. Create a [classic app](/rtm#classic) and [use the V1 Oauth flow](/docs/oauth) to use RTM.

For most applications, [Socket Mode](/apis/connections/socket) is a better way to communicate with Slack.

Note that setting `no_latest=1` will automatically set `no_unreads=1`.

This method returns lots of data about the current state of a team, along with a WebSocket Message Server URL:

```
{
    "ok": true,
    "url": "wss:\/\/wss-primary.slack.com\/websocket\/7I5yBpcvk",

    "self": {
        "id": "U023BECGF",
        "name": "bobby",
        "prefs": {
            ...
        },
        "created": 1402463766,
        "manual_presence": "active"
    },
    "team": {
        "id": "T024BE7LD",
        "name": "Example Team",
        "email_domain": "",
        "domain": "example",
        "icon": {
            ...
        },
        "msg_edit_window_mins": -1,
        "over_storage_limit": false
        "prefs": {
            ...
        },
        "plan": "std"
    },
    "users": [...],

    "channels": [...],
    "groups": [...],
    "mpims": [...],
    "ims": [...],

    "bots": [...],
}
```

The `url` property contains a WebSocket Message Server URL. Connecting to this URL will initiate a Real Time Messaging session. These URLs are only valid for 30 seconds, so connect quickly!

The `self` property contains details on the authenticated user.

The `team` property contains details on the authenticated user's team. If a team has not yet set a custom icon, the value of `team.icon.image_default` will be `true`.

The `users` property contains a list of [user objects](/types/user), one for every member of the team. `presence` is no longer included for users. The list of users may truncate on especially long teams. Use [Web API methods](/methods#users) to retrieve information about users instead.

The `channels` property is a list of [channel objects](/types/channel), one for every channel visible to the authenticated user. For regular or administrator accounts this list will include every team channel. The`is_member` property indicates if the user is a member of this channel. If true then the channel object will also include the topic, purpose, member list and read-state related information. The `latest`, `unread_count`, and `unread_count_display` attributes have been removed from this method's response. See [this changelog entry](/changelog/2017-04-start-using-rtm-connect-and-stop-using-rtm-start).

The `groups` property is a list of [group objects](/types/group), one for every group the authenticated user is in.

The `mpims` property is a list of [mpims objects](/types/mpim), one for every group the authenticated user is in. MPIMs are only returned to the client if `mpim_aware` is set when calling `rtm.start`. Otherwise, `mpims` are emulated using the groups API.

The `ims` property is a list of[IM objects](/types/im), one for every direct message channel visible to the authenticated user.

The `bots` property gives details of the integrations set up on this team.

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
| `migration_in_progress` | 
Workspace is being migrated between servers. See [the `team_migration_started` event documentation](/events/team_migration_started) for details.
 |
| `rtm_connect_required` | 
`rtm.start` is deprecated. Please use `rtm.connect` instead of `rtm.start`. Read [https://api.slack.com/changelog/2021-10-rtm-start-to-stop](https://api.slack.com/changelog/2021-10-rtm-start-to-stop) for more info.
 |
| `method_deprecated` | 
The method has been deprecated.
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
| `deprecated_endpoint` | 
The endpoint has been deprecated.
 |
| `two_factor_setup_required` | 
Two factor setup is required.
 |
| `enterprise_is_restricted` | 
The method cannot be called from an Enterprise.
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
| `member_list_truncated` | 
A `members` array included in a channel object was truncated at 1500 results. Use `conversations.members` to retrieve all results.
 |
| `method_deprecated` | 
`rtm.start` is deprecated. Please use `rtm.connect` instead of `rtm.start`. Read [https://api.slack.com/changelog/2021-10-rtm-start-to-stop](https://api.slack.com/changelog/2021-10-rtm-start-to-stop) for more info.
 |
| `missing_charset` | 
The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended.
 |
| `superfluous_charset` | 
The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous.
 |

