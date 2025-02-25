---
title: Javascript SDK Usage
sidebar_label: Usage
sidebar_position: 3
---

## Waiting for Features

You can wait on the features to be loaded from our servers by using `.onClientInitialized()` function. It returns a promise that you can use to wait until features are ready to be used:

```javascript
dvcClient.onClientInitialized().then(() => {
    const featureToggle = dvcClient.variable('<YOUR_VARIABLE_KEY>', false)
    if (featureToggle.value) {
        ...
    } else {
        ...
    }
})
```

You can also pass in a callback which will get called after the features are loaded:

```javascript
dvcClient.onClientInitialized((err) => {
    if (err) {
        // error state
    }

    const featureToggle = dvcClient.variable('<YOUR_VARIABLE_KEY>', false)
    if (featureToggle.value) {
        ...
    } else {
        ...
    }
})
```

## Getting Variable Values

To get values from your Features, `.variable` is used to fetch variable values using the identifier `key` coupled with a default value. The default value can be of type string, boolean, number, or object.

```javascript
const variable = dvcClient.variable("<YOUR_VARIABLE_KEY>", "default value");
```

To grab the value, there is a property on the object returned to grab the value:

```javascript
const value = variable.value;
```

If the value is not ready, it will return the default value passed in the creation of the variable.

The `onUpdate` accepts a handler function that will be called whenever a variable value has changed.
This can occur as a result of a project configuration change or calls to `identifyUser` or `resetUser`. To learn more, visit our [Realtime Updates](/sdk/features/realtime-updates) page.

There can only be one onUpdate function registered at a time. Subsequent calls to this method will overwrite the previous handler:

```javascript
variable.onUpdate((value) => {
  // value returned when the value of the variable changes
});
```

The `DVCVariable` type interface can be found [here](https://github.com/DevCycleHQ/js-sdks/blob/main/sdk/js/src/types.ts#L233).

## Identifying User

To identify a different user, or the same user passed into the initialize with more attributes, pass in the entire user attribute object into `identifyUser`:

```javascript
const user = {
  user_id: "user1",
  name: "user 1 name",
  customData: {
    customKey: "customValue",
  },
};
dvcClient.identifyUser(user);
```

To wait on Variables that will be returned from the identify call, you can pass in a callback or use the Promise returned if no callback is passed in:

```javascript
const variableSet = await dvcClient.identifyUser(user);

// OR

dvcClient.identifyUser(user, (err, variables) => {
  // variables is the variable set for the identified user
});
```

## Reset User

To reset the user into an anonymous user, `resetUser` will reset to the anonymous user created before or will create one with an anonymous `user_id`.

```javascript
dvcClient.resetUser();
```

To wait on the Features of the anonymous user, you can pass in a callback or use the Promise returned if no callback is passed in:

```javascript
const variableSet = await client.resetUser();

// OR

dvcClient.resetUser((err, variables) => {
  // variables is the variable set for the anonymous user
});
```

## Get All Features

To retrieve all the Features returned in the config:

```js
const features = dvcClient.allFeatures()
```

If the SDK has not finished initializing, these methods will return an empty object. Read [Waiting for Features](/sdk/client-side-sdks/javascript/javascript-usage#waiting-for-features) to mitigate this.

See [getFeatures](https://docs.devcycle.com/bucketing-api/#operation/getFeatures) in the Bucketing API for detailed response formats.

## Get All Variables

To grab all the Features returned in the config:

```js
const variables = dvcClient.allVariables()
```

If the SDK has not finished initializing, these methods will return an empty object. Read [Waiting for Features](/sdk/client-side-sdks/javascript/javascript-usage#waiting-for-features) to mitigate this.

See [getVariables](https://docs.devcycle.com/bucketing-api/#operation/getVariables) in the Bucketing API for detailed response formats.

## Tracking Events

To track events, pass in an object with at least a `type` key:

```javascript
const event = {
  type: "my_event_type", // this is required
  date: new Date(),
  target: "my_target",
  value: 5,
  metaData: {
    key: "value",
  },
};
dvcClient.track(event);
```

The SDK will flush events every 10s or `flushEventsMS` specified in the options. To manually flush events, call:

```javascript
await dvcClient.flushEvents();

// or

dvcClient.flushEvents(() => {
  // called back after flushed events
});
```

## Subscribing to SDK Events

The SDK can emit certain events when specific actions occur which can be listened on by subscribing to them:

```javascript
dvcClient.subscribe(
  "variableUpdated:*",
  (key: string, variable: DVCVariable | null) => {
    // key is the variable that has been updated
    // The new value can be accessed from the variable object passed in: variable?.value
    // The variable argument will be null if the variable is no longer being served a value 
    console.log(`New variable value for variable ${key}: ${variable?.value}`);
  }
);
```

The first argument is the name of the event that you can subscribe to. The `subscribe` method will throw an error if you try to
subscribe to an event that doesn't exist. These are the events you can subscribe to:

| **Event**        | **Key**             | **Handler Params**                     | **Description**                                                                                                                                                                                                                                       |
| ---------------- | ------------------- | -------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Initialized      | `initialized`       | `(initialized: boolean)`               | An initialized event is emitted once the SDK has received its first config from DevCycle. This event will only be emitted once.                                                                                                                       |
| Error            | `error`             | `(error: Error)`                       | If any error occurs in the SDK, this event emits that error.                                                                                                                                                                                          |
| Variable Updated | `variableUpdated:*` | `(key: string, variable: DVCVariable | null)` | This event gets triggered when a variable value changes for a user. You can subscribe to all variable updates using the `*` identifier, or you can pass in the key of the variable you want to subscribe to, e.g. `variableUpdated:my_variable_key`.  |
| Feature Updated  | `featureUpdated:*`  | `(key: string, feature: DVCFeature | null)`   | This event gets triggered when a feature's variation changes for a user. You can subscribe to all feature updates using the `*` identifier, or you can pass in the key of the feature you want to subscribe to, e.g. `featureUpdated:my_feature_key`. |

## EdgeDB

EdgeDB allows you to save user data to our EdgeDB storage so that you don't have to pass in all the user data every time you identify a user.

To get started, contact us at support@devcycle.com to enable EdgeDB for your project.

Once you have EdgeDB enabled in your project, pass in the `enableEdgeDB` option to turn on EdgeDB mode for the SDK:

```javascript
const user = {
  user_id: "my_user",
  customData: {
    amountSpent: 50,
  },
};
const options = {
  enableEdgeDB: true,
};
const dvcClient = initialize("<DVC_CLIENT_SDK_KEY>", user, options);
```

This will send a request to our EdgeDB API to save the custom data under the user `my_user`.

In the example, `amountSpent` is associated to the user `my_user`. In your next `identify` call for the same `user_id`,
you may omit any of the data you've sent already as it will be pulled from the EdgeDB storage when segmenting to experiments and features:

```
dvcClient.identifyUser({ user_id: 'my_user' }) // no need to pass in "amountSpent" any more!
```
