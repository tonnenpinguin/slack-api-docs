## Facts

### Method access

HTTP

JavaScript

Python

Java

HTTP
`POSThttps://slack.com/api/conversations.listConnectInvites`

[Bolt for Java](/tools/bolt)
`app.client().conversationsListConnectInvites`
[Powered by Bolt](/tools/bolt)

[Bolt for Python](/tools/bolt)
`app.client.conversations_listConnectInvites`
[Powered by Bolt](/tools/bolt)

[Bolt for JavaScript](/tools/bolt)
`app.client.conversations.listConnectInvites`
[Powered by Bolt](/tools/bolt)

### Required scopes

[Bot tokens](/docs/token-types#granular_bot)[`conversations.connect:manage`](/scopes/conversations.connect:manage)[`channels:manage`](/scopes/channels:manage)

### Content types

`application/x-www-form-urlencoded`[`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON")

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

`count`

integer

_·_Optional

Maximum number of invites to return

**Default**
`100`

`cursor`

string

_·_Optional

Set to `next_cursor` returned by previous call to list items in subsequent page

**Example**
`5c3e53d5`

`team_id`

string

_·_Optional

Encoded team id for the workspace to retrieve invites for, required if org token is used

**Example**
`T1234567890`

## Usage info

This [Slack Connect API](/apis/connect) method returns information about pending shared channel invites that have been sent or received by a workspace. Pending shared channel invites are invites that have been sent or received by the workspace but have not yet been denied or approved and may turn into shared channels on the team in the future.

This API does not currently return Slack Connect DM invite information.

Returns a [paginated list](/docs/pagination) of invites that represesent pending shared channel invitations sent or received by the

```
{
  "ok": true,
  "invites": [
    {
      "direction": "outgoing",
      "status": "sent",
      "date_last_updated": 1622591318,
      "invite_type": "channel",
      "invite": {
        "id": "I0139HVDSDQ",
        "date_created": 1622591287,
        "date_invalid": 1623800887,
        "inviting_team": {
          "id": "E0ANZNL03",
          "name": "oonamole",
          "icon": {
            ...
          },
          "is_verified": false,
          "domain": "oonamole",
          "date_created": 1559335482
        },
        "inviting_user": {
          "id": "W0ANZNNAX",
          "team_id": "E0ANZNL03",
          "name": "ben_boss",
          "updated": 1616521338,
          "profile": {
            "real_name": "Puppy InCharge",
            "display_name": "puppy_incharge",
            "real_name_normalized": "Puppy InCharge",
            "display_name_normalized": "puppy_incharge",
            "team": "E0ANZNL03",
            "avatar_hash": "g767309df8c3",
            "email": "puppyincharge@slack.com",
            "image_24": "...",
            "image_32": "...",
            "image_48": "...",
            "image_72": "...",
            "image_192": "...",
            "image_512": "..."
          }
        },
        "link": "..."
      },
      "channel": {
        "id": "C013AGH1GBD",
        "is_private": false,
        "is_im": false,
        "name": "pb-shared-7"
      },
      "acceptances": [
        {
          "approval_status": "pending_approval",
          "date_accepted": 1622591318,
          "date_invalid": 1623800918,
          "date_last_updated": 1622591318,
          "accepting_team": {
            "id": "T0PP93X0Q",
            "name": "Doughboy",
            "icon": {
              ...
            },
            "is_verified": false,
            "domain": "doughboy",
            "date_created": 1521573656
          },
          "accepting_user": {
            "id": "U0PP93X1N",
            "team_id": "T0PP93X0Q",
            "name": "bredman",
            "updated": 1619479454,
            "profile": {
              "real_name": "Brent Doodle",
              "display_name": "Brent Doodle",
              "real_name_normalized": "Brent",
              "display_name_normalized": "Brent Doodle",
              "team": "T0PP93X0Q",
              "avatar_hash": "ge2de9bb3fde",
              "email": "bdoodle@slack.com",
              "image_24": "...",
              "image_32": "...",
              "image_48": "...",
              "image_72": "...",
              "image_192": "...",
              "image_512": "..."
            }
          },
          "reviews": [
            {
              "type": "approval",
              "date_review": 1622591318,
              "reviewing_team": {
                "id": "T0PP93X0Q",
                "name": "Doughboy",
                "icon": {
                  ...
                },
                "is_verified": false,
                "domain": "doughboy",
                "date_created": 1521573656
              }
            }
          ]
        }
      ]
    },
    {
      "direction": "incoming",
      "status": "sent",
      "date_last_updated": 1622591697,
      "invite_type": "channel",
      "invite": {
        "id": "I0139HWQ134",
        "date_created": 1622591671,
        "date_invalid": 1623801271,
        "inviting_team": {
          "id": "T0PP93X0Q",
          "name": "Doughboy",
          "icon": {
            ...
          },
          "is_verified": false,
          "domain": "doughboy",
          "date_created": 1521573656
        },
        "inviting_user": {
          "id": "U0PP93X1N",
          "team_id": "T0PP93X0Q",
          "name": "bredman",
          "updated": 1619479454,
          "profile": {
            "real_name": "Pet Dog",
            "display_name": "Pet Dog",
            "real_name_normalized": "Pet Dog",
            "display_name_normalized": "Pet Dog",
            "team": "T0PP93X0Q",
            "avatar_hash": "ge2de9bb3fde",
            "email": "bront@slack.com",
            "image_24": "...",
            "image_32": "...",
            "image_48": "...",
            "image_72": "...",
            "image_192": "...",
            "image_512": "..."
          }
        },
        "link": "..."
      },
      "channel": {
        "id": "C013AGJCG9H",
        "is_private": false,
        "is_im": false,
        "name": "shared-channel-72"
      },
      "acceptances": [
        {
          "approval_status": "pending_approval",
          "date_accepted": 1622591697,
          "date_invalid": 1623801297,
          "date_last_updated": 1622591697,
          "accepting_team": {
            "id": "E0ANZNL03",
            "name": "oonamole",
            "icon": {
              ...
            },
            "is_verified": false,
            "domain": "oonamole",
            "date_created": 1559335482
          },
          "accepting_user": {
            "id": "W0ANZNNAX",
            "team_id": "E0ANZNL03",
            "name": "ben_boss",
            "updated": 1616521338,
            "profile": {
              "real_name": "Brent Puppies",
              "display_name": "Bront",
              "real_name_normalized": "Brent Puppies",
              "display_name_normalized": "brent_puppies",
              "team": "E0ANZNL03",
              "avatar_hash": "g767309df8c3",
              "email": "brent@slack.com",
              "image_24": "...",
              "image_32": "...",
              "image_48": "...",
              "image_72": "...",
              "image_192": "...",
              "image_512": "..."
            }
          },
          "reviews": [
            {
              "type": "approval",
              "date_review": 1622591697,
              "reviewing_team": {
                "id": "E0ANZNL03",
                "name": "oonamole",
                "icon": {
                  ...
                },
                "is_verified": false,
                "domain": "oonamole",
                "date_created": 1559335482
              }
            }
          ]
        }
      ]
    }
  ]
}
```

Some fields in the response, like `unread_count` and `unread_count_display`, are included for DM conversations only.

## Example responses

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `restricted_action` | 
A team preference prevents the authenticated user from viewing shared channel invites.
 |
| `invalid_arguments` | 
The method was either called with invalid arguments or some detail about the arguments passed are invalid, which is more likely when using complex arguments like blocks or attachments.
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
| `team_access_not_granted` | 
The token used is not granted the specific workspace access required to complete this request.
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

