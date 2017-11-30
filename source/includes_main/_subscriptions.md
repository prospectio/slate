# Subscriptions
## The subscription object
To start sending a campaign to a prospect you have to create a **subscription**. The subscription is then used to described the current status of the campaign for the prospect.

You can take manual actions on a subscription to `pause`, `resume` or `stop` a campaign for a specific prospect.

A subscription can also be automatically `completed` after the last [campaign step](#campaign-steps) is sent.
A subscription can laos be automatically `stopped` if we register one of the [`campaigns.stop_when` events](#the-campaign-object).

```
# EXAMPLE OBJECT
```
```json
{
  "data": {
    "id": "1",
    "type": "campaign_deliveries",
    "attributes": {
      "prospect_id": 2,
      "current_step": 0,
      "started_at": "2016-11-19T09:18:44.276Z",
      "completed_at": null,
      "stopped_at": null,
      "stop_reason": null,
      "start_at": "2016-11-19T09:18:44.190Z",
      "send_next_step_at": "2016-11-19T09:18:00.000Z",
      "paused_at": null,
      "pause_reason": null,
      "created_at": "2015-08-15T16:48:46+02:00",
      "updated_at": "2016-11-25T12:40:46+01:00"
    },
    "relationships": {
      "sender": {
        "data": {
          "id": "1",
          "type": "users"
        }
      },
      "creator": {
        "data": {
          "id": "1",
          "type": "users"
        }
      },
      "campaign": {
        "data": {
          "id": "6367",
          "type": "campaigns"
        }
      },
      "prospect": {
        "data": {
          "id": "2",
          "type": "prospects"
        }
      }
    }
  }
}
```

### Object attributes
Attribute | Description
--------- | -----------
id | **integer** <br />A unique identifier for the subscription
prospect_id | **integer** <br />The prospect's ID this subscription is associated to
current_step | **integer** <br />Define the last sent step (starts at 0)
started_at | **datetime** <br />The date and time when the campaign has been started in ISO 8601 format with timezone
started_at | **datetime** <br />The date and time when the campaign has been completed in ISO 8601 format with timezone. Completed means that we sent the last step of the campaign
stopped_at | **datetime** <br />The date and time when the campaign has been stopped in ISO 8601 format with timezone. Stopped means that we encountered a [`campaigns.stop_when` event](#the-campaign-object) or that the campaign has been manually stopped
stop_reason | **string** <br />The reason why the campaign has been stopped. Can be a [`campaigns.stop_when` event](#the-campaign-object) or `manual`
start_at | **datetime** <br />If the campaign is scheduled , the date and time when the campaign will start sending the first step
send_next_step_at | **datetime** <br />The date and time the next step is scheduled to be sent in ISO 8601 format with timezone
paused_at | **datetime** <br />The date and time when the campaign has been paused in ISO 8601 format with timezone
pause_reason | **string** <br />The reason why the campaign has been paused
created_at | **datetime** | ISO 8601 format with timezone offset
updated_at | **datetime** | ISO 8601 format with timezone offset

### Relationships
Object | Description
--------- | -----------
sender | The [user](#users) that send the emails
creator | The [user](#users) who created the subscription
campaign | Describe a [campaign object](#the-campaign-object)
prospect | Describe a [prospect object](#the-prospect-object)

## Create a subscription
```shell
# DEFINITION
POST https://prospect.io/api/public/v1/campaigns/{{CAMPAIGN_ID}}/subscriptions

# EXAMPLE
curl -X POST "https://prospect.io/api/public/v1/campaigns/1/subscriptions" \
-H "Authorization: your_api_key" \
-H "Content-Type: application/vnd.api+json; charset=utf-8" \
-d '{
  "data": {
    "type": "subscriptions",
    "attributes": {
      "prospect_id": 1,
      "user_id": 1
    }
  }
}'
```

This will create a new subscription.

### Parameters
Parameter | Default | Description
--------- | ------- | ------------
prospect_id<br />**required** - *integer* | / | The prospect's ID
user_id<br />**required** - *integer* | / | The sender's ID

### Returns
Returns the [subscription object](#the-subscription-object).

## Pause a subscription
```shell
# DEFINITION
POST https://prospect.io/api/public/v1/campaigns/{{CAMPAIGN_ID}}/subscriptions/{{SUBSCRIPTION_ID}}/pause

# EXAMPLE
curl -X POST "https://prospect.io/api/public/v1/campaigns/1/subscriptions/1/pause" \
-H "Authorization: your_api_key" \
-H "Content-Type: application/vnd.api+json; charset=utf-8" \
```

This will pause a subscription.

### Parameters
Parameter | Required? | Type | Description
--------- | --------- | -----| -----------
id | **yes** | integer | The ID of the subscription to pause

### Returns
Returns the [subscription object](#the-subscription-object).

## Resume a subscription
```shell
# DEFINITION
POST https://prospect.io/api/public/v1/campaigns/{{CAMPAIGN_ID}}/subscriptions/{{SUBSCRIPTION_ID}}/resume

# EXAMPLE
curl -X POST "https://prospect.io/api/public/v1/campaigns/1/subscriptions/1/resume" \
-H "Authorization: your_api_key" \
-H "Content-Type: application/vnd.api+json; charset=utf-8" \
```

This will resume a subscription.

### Parameters
Parameter | Required? | Type | Description
--------- | --------- | -----| -----------
id | **yes** | integer | The ID of the subscription to resume

### Returns
Returns the [subscription object](#the-subscription-object).

## Stop a subscription
```shell
# DEFINITION
DELETE https://prospect.io/api/public/v1/campaigns/{{CAMPAIGN_ID}}/subscriptions/{{SUBSCRIPTION_ID}}

# EXAMPLE
curl -X DELETE "https://prospect.io/api/public/v1/campaigns/1/subscriptions/1" \
-H "Authorization: your_api_key" \
-H "Content-Type: application/vnd.api+json; charset=utf-8" \
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "id": "1",
    "type": "campaign_deliveries"
  }
}
```

This will stop a subscription.

### Parameters
Parameter | Required? | Type | Description
--------- | --------- | -----| -----------
id | **yes** | integer | The ID of the subscription to stop

### Returns
Returns the [subscription object](#the-subscription-object).

## Retrieve a subscription
```shell
# DEFINITION
GET https://prospect.io/api/public/v1/campaigns/{{CAMPAIGN_ID}}/subscriptions/{{SUBSCRIPTION_ID}}

# EXAMPLE
curl -X GET "https://prospect.io/api/public/v1/campaigns/1/subscriptions/1" \
-H "Authorization: your_api_key" \
-H "Content-Type: application/vnd.api+json; charset=utf-8"
```

### Parameters
Parameter | Description
--------- | -----------
id<br />**required** - *integer* | The ID of the subscription to retrieve

### Returns
Returns the [subscription object](#the-subscription-object).