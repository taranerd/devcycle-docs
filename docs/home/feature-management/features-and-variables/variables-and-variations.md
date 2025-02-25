---
title: Variables & Variations
sidebar_position: 2
---

## Overview 

This article will cover how to manage and create Variables and Variations within a Feature. 

In short, a Feature may have any number of Variables. Variable values change depending on the Variation a user is in. There may be as many Variations of a Feature as desired. All Variables and Variations apply across all environments. 

## Creating and Managing Variables in a Feature

To view the Variables and Variations within a Feature, navigate to the Remote Variables section on a Feature page:

![Feature Page Sidebar Highlighting Variables](/march-2022-variables-sidebar.png)

This will lead the user to a table containing all of the Variables used by this Feature and all of their values across all Variations:

![Feature on the Variables Page showing Initial Sate](/december_2021_variables-fancy.png)

Each Feature manages its own set of Variables. **By default, upon creation of a Feature, a Boolean Variable will be created which has the same name as the Feature's key for easier reference.** Depending on the Feature type, the default Variations will be pre-set. The most common of which will be the Variations of "Variation OFF" and "Variation ON", with the boolean Variable being set to false and true, respectively.

When a user is "Served" a Variation based on the [Targeting Rules](/home/feature-management/features-and-variables/targeting-users), the Variable Values the user receives on their devices or as an API response will be the values for the served Variation.

A user can add as many Variables as they desire by simply clicking the "Add Variable" button. 

![Add Variable Modal](/april-2023-add-variable-modal.png)

Give your new Variable a **key**, a **type**, and its **values** for each of the current Variations.

The unique Variable **key** is used to reference the Variable in code. Variables cannot be used in multiple existing Features, so their keys must be unique.

The Variable **Type** helps enforce consistent usage across the team to avoid type mismatches in different use cases.

Variables may be the following types:

* Boolean
* String
* Number
* JSON

The Variable **Values** for each **Variation** will be what the Variable's value will be in SDK and API responses if a targeting rule is targeting those specific Variations. 

Once your Variable is created, it will appear on the Variables screen:

![Variables Page of a Feature with Two Variables](/december_2021_two-variables.png)

### Removing a Variable

To remove a Variable from a feature, simply click on the edit icon next to the variable key and select the option to remove the variable from the variable edit modal.

![Remove Variable Modal](/feb-2023-remove-variable.png)

Removing a Variable from this page does not completely remove the Variable from DevCycle. The Variable will still be visible in the [Variable Dashboard](/home/feature-management/organizing-your-flags-and-variables/variable-dashboard), but it will not be associated with any features.

Taking this action will cause all references to the Variable in any code usage to default only to the default value used in your codebase.

To fully delete a Variable you must do so via our [Management API](/management-api/#operation/VariablesController_remove).


### Re-associating a Variable
DevCycle has the ability to re-use existing variables and re-associate them to different features. 

In the Variable Key input field, a drop down will display all **unassociated, unarchived** variables that can be re-associated to your feature while also providing you the option to add a net new variable.

![Add Variable Modal Dropdown](/april-2023-add-variable-dropdown.png)

In the example above, the variables `demo` and `realtime-demo` are not associated with any other feature and can be added to my feature. 

If you select an existing, unassociated variable from the dropdown, the Variable type will be populated with the type of the selected variable and cannot be changed. 

![Add New Variable Variable Selected](/april-2023-add-new-variable-variable-selected-tooltip.png)

If you input a variable key that matches an existing archived variable, the error below will appear, as you must first unarchive the variable.

![Add New Variable Archived Variable Error](/april-2023-add-variable-modal-archived-variable-error.png)

To use it, click the hyperlinked "**variable**" text, and you'll be directed to the archived variable page where you can unarchive it.

:::info
If you want to move a variable between features, you must first remove it from the previous feature, making it unassociated.
:::

### Archiving a Variable

Archiving Variables is a good way to clean up the DevCycle dashboard and ensure that it is easy to understand which Variables are available for use and which should no longer be leveraged going forward.

To archive a Variable it must first be [removed from any active features](./variables-and-variations#removing-a-variable). Variables can be archived at the same time as removing from a feature. When the option to remove has been selected the confirmation modal will also provide the option to archive the Variable.

![Archive Variable While Removing](/march-2023-archive-variable-on-remove.png)

If a Variable is not archived when it is removed from a feature it will remain active, but it won't be associated with any features and the default value will be delivered whenever the Variable is evaluated in code. If you are archiving a Variable from the Variable list or Variable details page, the Variable must be in this unassociated state.

Here is an example of a Variable that cannot be archived because it is still associated with a feature:
![Example of a Variable that Cannot be Archived](/march-2023-active-variable.png)

When archiving a Variable from the Variable list or details page you will need to confirm your desire to archive by entering the Variable's key in the archive confirmation modal.

![Variable Archive Confirmation Modal](/march-2023-variable-archive-confirmation.png)

Once archived, Variables can be viewed by toggling the Variable status filter to either All or Archived Variables on the Variable list page. From here Variables can also be unarchived if desired.

![List of Archived Variables](/march-2023-archived-variables.png)

### Creating a New Feature with a Duplicate Initial Variable Key

If a duplicate variable key belonging to an unassociated variable is submitted when creating a new feature, this modal will appear that will allow you to re-associate the variable to your new feature.

![Duplicate Variable Key Modal ](/april-2023-duplicate-variable-key-modal.png)

If the unassociated variable key submitted is archived, a similar modal will appear with the option to unarchive the variable & re-associate it to the new feature.

![Duplicate Variable Key Modal Unarchive](/april-2023-duplicate-variable-key-unarchive-both-states.png)

If you wish to unarchive & re-associate, click on the toggle and click `Yes, Proceed`. 

The feature will be created along with the newly re-associated variable. The variations and corresponding variable values will be populated depending on the [Feature Type](/home/feature-management/getting-started/feature-types#types-within-devcycle) selected. 


If a duplicate variable key that belongs to a variable that is associated with an existing feature the dashboard will return the error below. 

![Duplicate Variable Key Modal Error Associated to Another Feature](/april-2023-create-new-feature-variable-associated-to-another-feature.png)


## Creating and using Variations in a Feature

While a Feature Flag may most commonly be used as a simple toggle of turning a Feature on or off, DevCycle offers further flexibility by allowing users to create as many different Variations of a Feature as they desire. In conjunction with the ability for a Feature to have numerous Variables, this unlocks a great range of possible use cases such as Personalization, Experimentation, and even gating aspects of Features for various reasons such as billing, permissions, or preferences.

By default, most Feature types within DevCycle will begin with two Variations. At any time, extra Variations can be added by clicking the "Add Variation" button on the Variables section of a Feature:

![Variables Page of a Feature with Arrow Pointing to Add Variation button](/december_2021_add-variation.png)

This will allow you to create a new Variation and assign all of the relevant values. 

![Variables Page of a Feature](/december_2021_new-variation.png)

Give your new Variation a **name** as well as a **key**, as well as its **values** For each of the current Variables.

The Variation **key** is used for easy reference within the DevCycle SDKs and APIs 

The Variable **values** will be what the Variable's value will be in SDK and API responses if a targeting rule is targeting those specific Variations. 

Once your Variable is created, it will appear on the Variables screen:

![Variables Page of a Feature with three Variations](/december_2021_three-variations.png)

Once this Variation is created, it will become available as an option within the "Serve" dropdown for [Targeting Rules](/home/feature-management/features-and-variables/targeting-users).

Users who are served this new Variation will receive the Variable Values associated with this new Variation!

### Deleting a Variation

A Variation may be deleted at any time by clicking the edit icon on the Variation column of the Remote Variables page. Variations that are currently being used in any **Enabled** environment cannot be deleted. First remove any audience being targeted by this Variation prior to deletion.


