---
title: "Pipe: Feature Flag Code Usages"
sidebar_position: 2
---

## Overview

**[Bitbucket Marketplace Listing](https://bitbucket.org/product/features/pipelines/integrations?&p=devcyclehq/devcycle-code-refs-pipe)**


With this Bitbucket pipe, your [DevCycle](https://devcycle.com/) dashboard will be updated to display code snippets for each DevCycle variable usage within your project.

**Note: This is intended to run when pushing changes to your main branch**

### Example Output

![Example Output](https://bitbucket.org/devcyclehq/devcycle-code-refs-pipe/raw/main/example-output.png)

### Usage
Add the following snippet to your `bitbucket-pipelines.yml` file:

```yaml
branches:
  main:
    - step:
        script:
          - pipe: devcyclehq/devcycle-code-refs-pipe:1.2.0
            variables:
              PROJECT_KEY: '<string>'
              CLIENT_ID: '<string>'
              CLIENT_SECRET: '<string>'
```

Your [DevCycle API credentials](https://app.devcycle.com/r/settings) and [project token](https://app.devcycle.com/r/projects) are required to update the DevCycle dashboard.

### Inputs

- To add variables to be used in the bitbucket-pipelines.yml, an admin must add Repository Variables in Repository Settings > Repository Variables, and then add all necessary variables as secured variables

| Variable           | Description                                                       |
| --------------------- | ----------------------------------------------------------- |
| PROJECT_KEY (*) | Your DevCycle project key, see [Projects](https://app.devcycle.com/r/projects)  |
| CLIENT_ID (*)     | Your organization's API client ID, see [Organization Settings](https://app.devcycle.com/r/settings) |
| CLIENT_SECRET (*)     | Your organization's API client secret, see [Organization Settings](https://app.devcycle.com/r/settings) |

_(*) = required variable._

### Prerequisites

1. Create a new [Project](/tools-and-integrations/terraform#create-a-project) & a new [Feature](/tools-and-integrations/terraform#create-a-feature) 
2. Grab the PROJECT_KEY in [Projects](https://app.devcycle.com/r/projects), and find your specific project name & key
3. Grab the CLIENT_ID in [Settings](https://app.devcycle.com/r/settings), under `API AUTHENTICATION`
4. Grab the CLIENT_SECRET in [Settings](https://app.devcycle.com/r/settings), under `API AUTHENTICATION`


### Configuration
The patterns used to identify references to variables in your code are fully customizable.
This action uses the [DevCycle CLI](https://github.com/DevCycleHQ/cli) under the hood, for details on how to configure the pattern matcher see the [CLI configuration](https://github.com/DevCycleHQ/cli#configuration).
