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

There are two ways to update non-custom profile fields with this method:

- Update one field at a time by passing the pair of arguments `name` and `value`.
- Update multiple fields by passing the argument `profile`.

You can update any of the following non-custom fields from the `profile` object within a Slack [`user`](/types/user) object:

| Field | Description |
| --- | --- |
| `title` | The user's title. |
| `phone` | The user's phone number, in any format. |
| `real_name` | The user's first and last name. Updating this field will update `first_name` and `last_name`. If only one name is provided, the value of `last_name` will be cleared. |
| `display_name` | The display name the user has chosen to identify themselves by in their workspace profile. |
| `email` | A valid email address. It cannot have spaces, or be in use by another member of the same team. It must have an `@` and a domain. Changing a user's email address will send an email to both the old and new addresses, and also post a slackbot message to the user informing them of the change. This field can only be changed _by admins_ for users on **paid** teams. |
| `pronouns` | The user's pronouns. |
| `first_name` | The user's first name. The name `slackbot` cannot be used. Updating `first_name` will update the first name within `real_name`. |
| `last_name` | The user's last name. The name `slackbot` cannot be used. Updating `last_name` will update the second name within `real_name`. |

The `skype` field will always be an empty string and cannot be set otherwise. For more details, please read [this changelog](/changelog/2017-02-minor-field-changes) entry.

While profile image fields are present in the `profile` object, they cannot be set using the [`users.profile.set`](/methods/users.profile.set) method. Use [`users.setPhoto`](/methods/users.setPhoto) and [`users.deletePhoto`](/methods/users.deletePhoto) to update a user's profile image.

The following example payload updates the values of `first_name`, `last_name`, `pronoun`, and `email` for a user:

```
{
    "profile": {
        "first_name": "John",
        "last_name": "Smith",
        "pronouns": "he/him/his",
        "email": "john@smith.com",
    }
}
```

### Custom profile fields 

Each custom profile field has a unique per-team ID. You can update a custom profile field by providing a key:value pair where the key is the relevant ID. Use [`team.profile.get`](/methods/team.profile.get) to retrieve the profile fields and their IDs used by a team.

You can update a profile field by using the ID in the `name` field. To update _custom_ profile fields, use the `fields` object instead. The `fields` object is an array of custom profile fields' key:value pairs.

If you update fields within the `fields` array, you can also choose to set the `alt` field. While the `value` of a field is what is usually displayed, the `alt` key will be displayed instead if it is set. The `alt` field can be up to 256 characters for all field types.

A field within the `fields` array needs a `type`. This determines the `type` of information `value` contains.

| Type | Description |
| --- | --- |
| `date` | the `value` must be a valid date. |
| `link` | the `value` can be any valid link that's not more than 256 characters. The link text will be the `alt` value if set, or the data element name if `alt` is not set. |
| `long_text` | the `value` can be up to 5,000 characters of [basic formatted](https://api.slack.com/reference/surfaces/formatting#basics) `mrkdown`. See the `long_text` section [below](#long-text). |
| `options_list` | the `value` must be one of the `possible_values` in the field definition. |
| `tags` | the `value` contains distinct elements known as multi-value tags. Individual tags can hold up to 50 characters. Any number of tags can be created by admins, but end-users can only add 75 tags to their profiles. See the `tags` section [below](#tags). |
| `text` | the `value` can be up to 256 characters of plain text. |

The following example sets the `value` of three fields; one with plain text, one with a date, and another with a link.

```
{
    "profile": {
        "fields": {
            "Xf0111111": {
                "value": "Barista",
                "alt": ""
            },
            "Xf0222222": {
                "value": "2022-04-11",
                "alt": ""
            },
            "Xf0333333": {
                "value": "https://example.com",
                "alt": ""
            }
        }
    }
}
```

#### Long\_text (Flexible Text) 

The `value` of a `long_text` field can be up to 5,000 characters of [basic formatted](https://api.slack.com/reference/surfaces/formatting#basics) `mrkdown`.

While you should use [`team.profile.get`](/methods/team.profile.get) to get the ID for any field, including long-text fields, you should _not_ copy the received schema. Below is a properly formatted payload for updating the `long_text` field with `user.profile.set`.

```
{
    "profile": {
        "fields": {
            "Xf0222222": {
                "value": "​​I make absolutely the best coffee you will *_ever_* taste. Learn more about where I studied <http://www.example.com|how to pull an espresso shot>. :coffee:", 
                "alt": ""
            }
        }
    }
}
```

#### Tags (Smart Tags) 

The `value` of a `tags` field contains distinct elements known as multi-value tags. Individual tags can hold up to 50 characters. Any number of tags can be created by admins, but end-users can only add 75 tags to each smart tag element on their profiles.

While you should use [`team.profile.get`](/methods/team.profile.get) to get the ID for any field, including smart tag fields, you should _not_ copy the received schema. Below is a properly formatted payload for updating the `tags` field with `user.profile.set`.

```
{
    "profile": {
        "fields": {
            "Xf0333333": [
				{
					"value": "Mocha",
					"alt": ""
				},
				{
					"value": "Latte",
					"alt": ""
				},
				{
					"value": "Americano",
					"alt": ""
				}
			]
        }
    }
}
```

#### Name Pronunciation 

The Name Pronunciation field lets a user provide a text description of how their name is pronounced. This text is displayed under their job title.

Use `team.profile.get` to obtain the field ID. Below is an example payload for updating the field once you have that ID:

```
{
    "profile": {
        "fields": {
            "Xf0444444": {
                "value": "Zoë is pronounced zo-ee", 
                "alt": ""
            }
        }
    }
}
```

**Name Recordings**  
A user can also record an audio file of how their name is pronounced. This name recording will appear on their profile as a speaker icon. A Name Recording cannot be updated via API.

### Status updates 

This method is also used to set a user's [current status](/docs/presence-and-status#custom_status).

Place the following status fields within the `profile` object when calling [`users.profile.set`](/methods/users.profile.set):

| Field | Type | Description |
| --- | --- | --- |
| `status_emoji` | string | The displayed emoji that is enabled for the Slack team, such as `:train:`. |
| `status_expiration` | integer | the Unix Timestamp of when the status will expire. Providing `0` or omitting this field results in a custom status that will not expire. |
| `status_text` | string | The displayed text of up to 100 characters. We strongly encourage brevity. |

For example, the following payload sets a custom status of `🚆 riding a train` and has it expire on July 26th, 2018 at 17:51:46 UTC:

```
{
    "profile": {
        "status_text": "riding a train",
        "status_emoji": ":train:",
        "status_expiration": 1532627506
    }
}
```

To manually unset a user's custom status, provide empty strings to both the `status_text` and `status_emoji` attributes: `""`.

### Building your HTTP request 

We **strongly recommend** using `application/json` POSTs when using this method. If you choose to use `application/x-www-form-urlencoded`, you must URL-encode the JSON provided to the `profile` field.

In general, you need to set your content type and authorization credentials to make an HTTP request. If you're using a workspace token, you need to provide an `X-Slack-User` header indicating the user you're [acting on behalf of](/docs/working-for-users).

You can send a JSON payload to `users.profile.set` with such an HTTP request:

```
POST /users/profile.set
Host: slack.com
Authorization: Bearer xoxp-secret-token
Content-type: application/json; charset=utf-8
{
    "profile": {
        "first_name": "John",
        "last_name": "Smith",
        "pronouns": "he/him/his",
        "email": "john@smith.com",
        "fields": {
            "Xf0111111": {
                "value": "Barista",
                "alt": ""
            },
            "Xf0222222": {
                "value": "2022-04-11",
                "alt": ""
            },
            "Xf0333333": {
                "value": "https://example.com",
                "alt": ""
            }
        }
    }
}
```

This method will generate a [`user_change`](/events/user_change) event on success, containing the complete user.

## Profile update rate limits 

Update a user's profile, including custom status, sparingly. Special [rate limit](/docs/rate-limits) rules apply when updating profile data with [`users.profile.set`](/methods/users.profile.set). A token may update a single user's profile no more than **10** times per minute. And a single token may only set **30** user profiles per minute. Some burst behavior is allowed.

## Example responses

### Common successful response

The complete user's profile will be returned.

```
{
    "ok": true,
    "profile": {
        "title": "Head of Coffee Production",
        "phone": "",
        "skype": "",
        "real_name": "John Smith",
        "real_name_normalized": "John Smith",
        "display_name": "john",
        "display_name_normalized": "john",
        "fields": {
            "Xf0111111": {
                "value": "Barista",
                "alt": ""
            },
            "Xf0222222": {
                "value": "2022-04-11",
                "alt": ""
            },
            "Xf0333333": {
                "value": "https://example.com",
                "alt": ""
            }
        },
        "status_text": "Watching cold brew steep",
        "status_emoji": ":coffee:",
        "status_emoji_display_info": [],
        "status_expiration": 0,
        "avatar_hash": "123xyz",
        "email": "johnsmith@example.com",
        "huddle_state": "default_unset",
        "huddle_state_expiration_ts": 0,
        "first_name": "john",
        "last_name": "smith",
        "image_24": "https://.../...-24.png",
        "image_32": "https://.../...-32.png",
        "image_48": "https://.../...-48.png",
        "image_72": "https://.../...-72.png",
        "image_192": "https://.../....-192png",
        "image_512": "https://.../...-512.png"
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

