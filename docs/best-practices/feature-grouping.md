---
title: Feature Flag Grouping
sidebar_label: Flag Grouping
sidebar_position: 4
description: hidden
sidebar_custom_props: {icon: people-group}
---

## Overview

This guide describes how to manage large amounts of feature flags with DevCycle. DevCycle’s Feature Grouping can act as a “master switch” for multiple flags in a group. In this article, you will learn the difference between Features and Variables in DevCycle and how they play into feature flag organization.

## Why Group Flags?

Feature Flags can accumulate very quickly. Without an organizational mechanism, the feature management dashboard would be extremely cluttered. Imagine developing a complex feature with various parts, such as the [Metrics and Experimentation feature in DevCycle](/home/feature-management/features-and-variables/metrics-and-analysis/feature-experimentation). Each piece of functionality needs a flag: There’s a flag for the API, a flag for the UI, flags for each type of experimentation, string flags for naming, and so on. It would be helpful to have the ability to find all these flags in the same place.

Not only do we want flags for each part of a feature, but we also want the ability to turn off the entire feature all at once in case something goes wrong. Other Feature Flag management solutions require you to create complex feature flag dependencies to set up a “master switch” for a group of flags. In contrast, DevCycle allows you to group feature flags and disable them simultaneously with one switch.

## How DevCycle Sees Features

DevCycle sees features as complex functionalities made up of several simpler working components. Each component requires a flag. As a result, DevCycle prefers to view Feature Flags as mere parts of a feature. When you create a feature, you can create flags to toggle different aspects of that feature. To do so, we can use Remote Variables. Remote Variables are flags for toggling values that affect subparts of features. This way, you can group your flags, or Variables, by feature.

## Using Flag Grouping

When you add a feature on DevCycle, DevCycle automatically creates a Variable with the same name as your feature. You can also make additional Variables to gate different components of your feature.

For instance, let’s return to our example about DevCycle’s Metrics feature. Let’s say we’ve added a feature to our dashboard called “Metrics and Experimentation”, and we want to create one flag to control the API and one for the UI. We can create a variable called `show-metrics-api` and another variable called `show-metrics-ui`, setting their values to `true` for Variation On and `false` for Variation Off.

![Variables for Metrics Feature](/july-2022-metrics-example-variables.png)

We now have two flags within our “Metrics and Experimentation” feature. 

### Disabling Individual Variables

Let’s say we need to disable just the API for our feature. There are a few methods we can do this:

**The first method is to change the individual value of our `show-metrics-api` variable.** Under “Variation On” we can set `show-metrics-api` to `false` for Variation On. That way, the API will be disabled for everyone receiving Variation On. 

We should also rename our variation. Since our `show-metrics-api` flag is no longer enabled, having it under “Variation On” would be misleading.

![Metrics API Variable is Off](/july-2022-metrics-api-false.png)

But what if we only want a specific group of users to have the API disabled? Let’s say we want our Internal QA users to continue seeing the API, but we want it to be hidden from external users. **This leads to the second method to disable Variables: creating a new [Variation](/home/feature-management/features-and-variables/variables-and-variations).** We can create a new Variation called “General Users” and set the `show-metrics-api` variable to `false`.

![General users variation for Metrics Feature](/july-2022-general-users-variation.png)

Doing so will hide the API from users receiving the General Users variation, while still allowing our QA users (receiving Variation On) to see the API.

:::info

**Managing multiple Variables and Variations:**

To learn more about managing multiple Variables and Variations within a feature, [Read here for more about managing multiple Variables and Variations](/home/feature-management/features-and-variables/variables-and-variations).

:::

### Using Targeting as a “Master Switch”

One challenge with many Feature Flag management solutions is creating a switch that controls a whole feature and its feature flag dependencies. DevCycle, however, makes it easy to disable an entire Feature and its Remote Variables using [Targeting Rules](/home/feature-management/features-and-variables/targeting-users).

**Method 1: Disabling a feature for everyone**

To disable a feature for everyone in an environment, simply toggle the Targeting switch for the desired environment.

![toggling the targeting switch](/july-2022-targeting-toggle.gif)

Changing which variation “All Users” receive is like toggling the entire feature without affecting the experience of the QA Users. 

**Method 2: Disabling a feature for a target group**

Targeting Rules also allow you to disable the entire feature for only a specific group of people. We can create a rule to target Internal QA Users, and a second rule to target everyone else (All Users). We can set “Internal QA users” to be served “Variation On” and “All Users” to “Variation Off”.

![Variation off for external users](/july-2022-metrics-targeting-variation-off.png)


:::info

**Targeting Users:**

[Read here to learn more about how to Target Users](/home/feature-management/features-and-variables/targeting-users).


:::

## Summary

This guide explained how to manage large amounts of features flags by:

- using Features to group Variables
- creating new Variations to toggle individual Variables
- enabling and disabling entire Features via Targeting Rules

Here are similar resources to help with organizing Feature Flags:

- [Feature Types](/home/feature-management/getting-started/feature-types)
- [Effectively Organizing Your Feature Flags](/best-practices/effectively-organizing-feature-flags)
- [Creating Variables & Variations](/home/feature-management/features-and-variables/variables-and-variations#overview)