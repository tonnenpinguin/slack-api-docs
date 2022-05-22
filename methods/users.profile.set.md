## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/users.profile.set`

[Bolt for Java](/tools/bolt)
`app.client().usersProfileSet`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.users_profile_set`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.users.profile.set`
[Powered by Bolt](/tools/bolt)

### Required scopes

[User tokens](/docs/token-types#user)[`users.profile:write`](/scopes/users.profile:write)

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

Optional arguments

`name`

string

_·_Optional

Name of a single key to set. Usable only if `profile` is not passed.

**Example**
`first_name`

`profile`

string

_·_Optional

Collection of key:value pairs presented as a URL-encoded JSON hash. At most 50 fields may be set. Each field name is limited to 255 characters.

**Example**
`{ first_name: "John", ... }`

`user`

string

_·_Optional

ID of user to change. This argument may only be specified by team admins on paid teams.

**Example**
`W1234567890`

`value`

string

_·_Optional

Value to set a single key to. Usable only if `profile` is not passed.

**Example**
`John`

## Usage info

Use this method to set a user's profile information, including name, email, current status, and other attributes.

Update individual fields by passing the pair of arguments `name` and`value`; or multiple fields can be updated at once by passing the argument`profile`.

We **strongly recommend** using `application/json` POSTs when using this method. If you choose to use `application/x-www-form-urlencoded`, you must URL-encode the JSON provided to the `profile` field. See [below](#http_request)

The `first_name` and `last_name` fields can be up to 35 characters each. The name `slackbot` cannot be used for either of these fields.

The `email` field must be a valid email address. It cannot have spaces, and it must have an `@` and a domain. It cannot be in use by another member of the same team. Changing a user's email address will send an email to both the old and new addresses, and also post a slackbot to the user informing them of the change. This field can only be changed _by admins_ for users on **paid** teams.

After March 20, 2017 the `skype` field will always be an empty string and cannot be set otherwise. For more detail, please read [this changelog](/changelog/2017-02-minor-field-changes) entry.

To set a user's profile image, use [`users.setPhoto`](/methods/users.setPhoto). To clear them, call [`users.deletePhoto`](/methods/users.deletePhoto).

The `fields` key is an array of key:value pairs holding the values for the user's custom profile fields. The key is the ID of the definition for that field, which is per-team. This ID can also be used in the `name` field of this method. The `value` of a field is what should be displayed, unless the `alt` key is also present, in which case that is displayed instead. The `value` can be up to 256 characters for fields of type `text` and `link`. For fields of type `options_list`, the `value` must be one of the `possible_values` in the field definition. For fields of type `date`, the `value` must be a valid date. The `alt` field can be up to 256 characters for all field types. The `profile` argument must be used in order to set the `alt` field.

Use [`team.profile.get`](/methods/team.profile.get) to retrieve the profile fields used by a team.

## Building your HTTP request 

This example demonstrates setting some basic profile fields and one extended field:

```
{
    "profile": {
        "first_name": "John",
        "last_name": "Smith",
        "email": "john@smith.com",
        "fields": {
            "Xf06054BBB": {
                "value": "Barista",
                "alt": "I make the coffee & the tea!"
            }
        }
    }
}
```

To send that JSON to `users.profile.set`, build an HTTP request like this, setting your content type, authorization credentials, and, **for workspace tokens only** , an `X-Slack-User` header indicating the user you're [acting on behalf of](/docs/working-for-users):

```
POST /users/profile.set
Host: slack.com
Authorization: Bearer xoxp-secret-token
Content-type: application/json; charset=utf-8
{
    "profile": {
        "first_name": "John",
        "last_name": "Smith",
        "email": "john@smith.com",
        "fields": {
            "Xf06054BBB": {
                "value": "Barista",
                "alt": "I make the coffee & the tea!"
            }
        }
    }
}
```

### Updating a user's current status

This method is also used to set a user's [current status](/docs/presence-and-status#custom_status).

To set status, both the `status_text` and `status_emoji` profile fields must be provided. Optionally, you can also provide a `status_expiration` field to set a time in the future when the status will clear.

- `status_text` allows up to 100 characters, though we strongly encourage brevity.
- `status_emoji` is a string referencing an emoji enabled for the Slack team, such as `:mountain_railway:`.
- `status_expiration` is an integer specifying seconds since the epoch, more commonly known as "UNIX time". Providing `0` or omitting this field results in a custom status that will not expire.

For example, to set a custom status of `🚞 riding a train` and have it expire on July 26th, 2018 at 17:51:46 UTC, construct this JSON payload:

```
{
	"status_text": "riding a train",
	"status_emoji": ":mountain_railway:",
	"status_expiration": 1532627506
}
```

Next, place the custom status fields within the user's `profile` and use [`users.profile.set`](/methods/users.profile.set):

```
POST /api/users.profile.set
Host: slack.com
Content-type: application/json; charset=utf-8
Authorization: Bearer xoxp_secret_token
{
    "profile": {
        "status_text": "riding a train",
        "status_emoji": ":mountain_railway:",
        "status_expiration": 1532627506
    }
}
```

To manually unset a user's custom status, provide empty strings to both the `status_text` and `status_emoji` attributes: `""`.

## Profile update rate limits 

Update a user's profile, including custom status, sparingly. Special [rate limit](/docs/rate-limits) rules apply when updating profile data with [`users.profile.set`](/methods/users.profile.set). A token may update a single user's profile no more than **10** times per minute. And a single token may only set **30** user profiles per minute. Some burst behavior is allowed.

After making this call, the complete user's profile will be returned in the same format as above.

This method will generate a [`user_change`](/events/user_change) event on success, containing the complete user.

## Example responses

### Common successful response

Typical success response

```
{
    "ok": true,
    "profile": {
        "avatar_hash": "ge3b51ca72de",
        "status_text": "Print is dead",
        "status_emoji": ":books:",
        "status_expiration": 0,
        "real_name": "Egon Spengler",
        "display_name": "spengler",
        "real_name_normalized": "Egon Spengler",
        "display_name_normalized": "spengler",
        "email": "spengler@ghostbusters.example.com",
        "image_24": "https://.../avatar/e3b51ca72dee4ef87916ae2b9240df50.jpg",
        "image_32": "https://.../avatar/e3b51ca72dee4ef87916ae2b9240df50.jpg",
        "image_48": "https://.../avatar/e3b51ca72dee4ef87916ae2b9240df50.jpg",
        "image_72": "https://.../avatar/e3b51ca72dee4ef87916ae2b9240df50.jpg",
        "image_192": "https://.../avatar/e3b51ca72dee4ef87916ae2b9240df50.jpg",
        "image_512": "https://.../avatar/e3b51ca72dee4ef87916ae2b9240df50.jpg",
        "team": "T012AB3C4"
    }
}
```

### Common error response

Typical error response

```
{
    "ok": false,
    "error": "invalid_profile"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `reserved_name` | 
First or last name are reserved.
 |
| `invalid_profile` | 
Profile object passed in is not valid JSON (make sure it is URL encoded!).
 |
| `profile_set_failed` | 
Failed to set user profile.
 |
| `partial_profile_set_failed` | 
Failed to set user profile.
 |
| `not_admin` | 
Only admins can update the profile of another user. Some fields, like `email` may only be updated by an admin.
 |
| `not_app_admin` | 
Only team owners and selected members can update the profile of a bot user.
 |
| `cannot_update_admin_user` | 
Only a primary owner can update the profile of an admin.
 |
| `too_long` | 
You attempted to set a custom status but it was longer than the maximum allowed, 100.
 |
| `must_clear_both_status_text_and_status_emoji` | 
Clearing the status requires setting both `status_text` and `status_emoji` to ''.
 |
| `email_taken` | 
email taken
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

