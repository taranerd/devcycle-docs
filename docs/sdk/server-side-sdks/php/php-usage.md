---
title: DevCycle PHP Server SDK Usage
sidebar_label: Usage
sidebar_position: 3
---

## User Object

The full user data must be passed into every method. The only required field is `user_id`.
The rest are optional and are used by the system for user segmentation into variables and features.

See the User model in the [PHP user model doc](https://github.com/DevCycleHQ/php-server-sdk/blob/main/lib/Model/UserData.php) for all accepted fields including custom fields.


```php
    $user_data = new \DevCycle\Model\UserData(array(
    "user_id"=>"my-user"
    )); // \DevCycle\Model\UserData
```

## Get and use Variable by key
```php
try {
    $result = $apiInstance->variable($user_data, "my-key", "default");
    print_r($result);
} catch (Exception $e) {
    echo 'Exception when calling DVCClient->variable: ', $e->getMessage(), PHP_EOL;
}
```
See [getVariableByKey](/bucketing-api/#operation/getVariableByKey) on the Bucketing API for the variable response format.

## Get all Variables
```php
try {
    $result = $apiInstance->allVariables($user_data);
    print_r($result);
} catch (Exception $e) {
    echo 'Exception when calling DVCClient->allVariables: ', $e->getMessage(), PHP_EOL;
}
```
See [getVariables](/bucketing-api/#operation/getVariables) on the Bucketing API for the variable response format.

## Getting all Features
```php
try {
    $result = $apiInstance->allFeatures($user_data);
    print_r($result);
} catch (Exception $e) {
    echo 'Exception when calling DVCClient->allFeatures: ', $e->getMessage(), PHP_EOL;
}
```
See [getFeatures](/bucketing-api/#operation/getFeatures) on the Bucketing API for the feature response format.

## Track Event
```php
$event_data = new \DevCycle\Model\Event(array(
  "type"=>"my-event"
));

try {
    $result = $apiInstance->track($user_data, $event_data);
    print_r($result);
} catch (Exception $e) {
    echo 'Exception when calling DVCClient->track: ', $e->getMessage(), PHP_EOL;
}
```

## EdgeDB

EdgeDB allows you to save user data to our EdgeDB storage so that you don't have to pass in all the user data every time you identify a user. Read more about [EdgeDB](/home/feature-management/edgedb/what-is-edgedb).

To get started, contact us at support@devcycle.com to enable EdgeDB for your project.

Once you have EdgeDB enabled in your project, pass in the enableEdgeDB option to turn on EdgeDB mode for the SDK:

```php
$config = DevCycle\Configuration::getDefaultConfiguration()->setApiKey('Authorization', '<DVC_SERVER_SDK_KEY>');
$options = new DevCycle\DVCOptions(true);
$apiInstance = new DevCycle\Api\DVCClient(
    $config,
    dvcOptions:$options
);
```

## Async Methods

Each method in the [Usage](#Usage) section has a corresponding asynchronous method:
```php
    $result = $apiInstance->allVariables($user_data);
    $apiInstance->allVariablesAsync($user_data)->then(function($result) {
      print_r($result);
    });
```

## Models

### UserData

User data is provided to most SDK requests to identify the user / context of the feature evaluation

| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **user_id** | **String** | Unique id to identify the user |  |
| **email** | **String** | User&#39;s email used to identify the user on the dashboard / target audiences | [optional] |
| **name** | **String** | User&#39;s name used to identify the user on the dashboard / target audiences | [optional] |
| **language** | **String** | User&#39;s language in ISO 639-1 format | [optional] |
| **country** | **String** | User&#39;s country in ISO 3166 alpha-2 format | [optional] |
| **app_version** | **String** | App Version of the running application | [optional] |
| **app_build** | **String** | App Build number of the running application | [optional] |
| **custom_data** | **Object** | User&#39;s custom data to target the user with, data will be logged to DevCycle for use in dashboard. | [optional] |
| **private_custom_data** | **Object** | User&#39;s custom data to target the user with, data will not be logged to DevCycle only used for feature bucketing. | [optional] |
| **created_date** | **Float** | Date the user was created, Unix epoch timestamp format | [optional] |
| **last_seen_date** | **Float** | Date the user was created, Unix epoch timestamp format | [optional] |
| **platform** | **String** | Platform the Client SDK is running on | [optional] |
| **platform_version** | **String** | Version of the platform the Client SDK is running on | [optional] |
| **device_model** | **String** | User&#39;s device model | [optional] |
| **sdk_type** | **String** | DevCycle SDK type | [optional] |
| **sdk_version** | **String** | DevCycle SDK Version | [optional] |

### Event

Event data is provided to `track` calls to log events to DevCycle

| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **type** | **String** | Custom event type |  |
| **target** | **String** | Custom event target / subject of event. Contextual to event type | [optional] |
| **date** | **Float** | Unix epoch time the event occurred according to client | [optional] |
| **value** | **Float** | Value for numerical events. Contextual to event type | [optional] |
| **meta_data** | **Object** | Extra JSON metadata for event. Contextual to event type | [optional] |

### Variable

Variable objects are returned by the SDK when calling `variable` or `allVariables`.

| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **_id** | **String** | unique database id |  |
| **key** | **String** | Unique key by Project, can be used in the SDK / API to reference by &#39;key&#39; rather than _id. |  |
| **type** | **String** | Variable type |  |
| **value** | **Object** | Variable value can be a string, number, boolean, or JSON |  |

### Feature

Feature objects are returned by the SDK when calling `allFeatures`

| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **_id** | **String** | unique database id |  |
| **key** | **String** | Unique key by Project, can be used in the SDK / API to reference by &#39;key&#39; rather than _id. |  |
| **type** | **String** | Feature type |  |
| **_variation** | **String** | Bucketed feature variation |  |
| **eval_reason** | **String** | Evaluation reasoning | [optional] |


## Tests

To run the tests, use:

```bash
composer install
vendor/bin/phpunit
```
