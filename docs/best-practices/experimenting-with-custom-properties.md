---
title: Using Custom Properties
sidebar_label: Custom Properties
description: hidden
sidebar_position: 3
sidebar_custom_props: {icon: flask-vial}

---

## Overview

Custom Properties are useful way to Target users for Experimentation or Permissions. To complete this guide, we recommend an understanding of User Targeting in DevCycle, found on our docs [Targeting Users for Features](/home/feature-management/features-and-variables/targeting-users).

DevCycle provides several properties that can be used to target users for a feature. You can target users by their User ID, Email, Country, Platform, etc. However, it may sometimes be helpful to identify users by attributes that are not predefined in DevCycle. 

Let’s say you would like to enable a new feature for only the beta users of your product. It may be cumbersome to indicate each User’s ID or Email in your targeting rule definition. This is where Custom Properties come into play.

## What are Custom Properties?

Custom Properties are Targeting attributes that developers can create to use in the dashboard. If you’d like to target users based on a property that is not in DevCycle, you can add one. For instance, if you would like to target users based on a number, such as the amount of money they’ve spent, you can create a numerical property. Or if you’d like to target beta users based on a boolean like “isBetaUser”, you can make a boolean property. As a result, Custom Properties are incredibly useful for gradual rollouts, experimentation, and permissions features.

## Creating Properties on the DevCycle Dashboard

The first step to using Custom Properties is to initialize a property on the DevCycle dashboard. In the Targeting Rules section of your feature, open the Target’s definition dropdown and select “Add Property”.

![add property window](/march-2022-add-property.png)

A modal will open allowing you to create a new property. We will create a boolean property called `isBetaUser`.

![property key](/august-2022-isBetaUser-property-type.png)

The Property Key and Property Type are mandatory fields.

- The **Property Key** must match what is sent by the SDK or API. 
- The **Property Type** must match the type sent by the SDK. The available types are boolean, number, and string.

The following fields are optional:

- The **Display Name** only changes the property’s display name in the DevCycle UI. This is useful for properties with long or auto-generated names. However, the *Property Key* will be used to match the SDK or API when bucketing users.
- The **DevCycle Key** is auto-generated based on the **Property Key**. You can use the DevCycle Key to reference the Property in the [DevCycle Management API](/management-api/#tag/Custom-Properties).

Once you’ve created a property, you can find it in the Definition dropdown when you modify the Targeting Rules. The property will be accessible across all features within your project. 

[To learn more about creating Custom Properties, read our docs here](/home/feature-management/features-and-variables/custom-properties#creating-a-new-property-for-use).

## Implementing Custom Properties

Custom Properties can be added to any user object in your code by including it in the `customData` field. For example, let’s implement the boolean `isBetaUser` property using the React SDK:

```jsx
import { useDVCClient } from '@devcycle/devcycle-react-sdk'

const user = {
  user_id: 'user1',
  name: 'user 1 name',
  customData: {
    'isBetaUser': true
  }
}
const client = useDVCClient()
client.identifyUser(user)
```
Notice that we added the Custom Property Key, `isBetaUser`, and its custom value, `true`, within the `customData` field of our user object. Remember that the Custom Property Key must be the exact same as it is displayed on the dashboard. As a result, it is a good practice to use quotation marks when indicating the key in your code, especially if the Property Key has spaces or hyphens.

To reference our user’s features and variables, we use the [Identify](/sdk/features/identify) method from the DevCycle SDKs. A call to the Identify function will return the list of relevant Features and Variables for the user. After we define our user’s `customData`, we call the `identifyUser` method on the client object, obtained from [using the `useDVCClient` hook](/sdk/client-side-sdks/react-native/react-native-usage#usedvcclient). That way, we can reference the user’s features and variable values based on the targeting of their custom property.

For more documentation about the Identify method with different SDKs, read [Identifying Users & Setting Properties](/sdk/features/identify).

:::info

Every time you identify a particular user, you must pass the custom data into the SDK. 

DevCycle's EdgeDB feature enables the saving of user data into DevCycle's EdgeDB storage, allowing you to segment by custom properties without having to repeatedly pass data to the SDK. [View our EdgeDB docs to find out how it works](/home/feature-management/edgedb/edge-flags).
:::

## Common Use Cases for Custom Properties

You can target users in numerous ways using Custom Properties. The following list describes some of the more common ways organizations utilize Custom Properties for experimentation and feature flags.

**Internal Users or Beta Users**. Some of the most common Custom Properties are used to experiment on beta users or differentiate between internal and external users. You can create property names such as `userType` `isEmployee` `isQAUser` etc.

**Geographic Location**. DevCycle has a built-in “Country” property, but you can target other forms of location by creating Custom Properties such as `storeLocation` `province` `state` `city` `school` etc.

**Special Users**. Sometimes organizations want to release a feature to special users only, such as users with a paid membership, those with a free trial, or those who have made large contributions to the company. In this case, some property name suggestions are `accountType` `pricingPlan` `isSubscriber` `isTrialUser` `amountContributed` etc.

**User Behaviour or Preferences.** You can experiment on users based on their behaviour or preferences. Some examples include `numberOfPageVisits` `gaveConsent` `preferredColor` etc.

## Summary

In this guide, we explained how to enhance experimentation and feature flag targeting with Custom Properties. The common use case list above can be used as a reference guide to provide ideas for new properties.