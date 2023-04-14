---
title: DevCycle Java Cloud Server SDK Getting Started
sidebar_label: Getting Started
sidebar_position: 2
---

To use the DevCycle Java SDK, initialize a client object. 

```java
import com.devcycle.sdk.server.cloud.api.DVCCloudClient;

public class MyClass {
    
    private DVCCloudClient dvcCloudClient;
    
    public MyClass() {
        dvcCloudClient = new DVCCloudClient("<DVC_SERVER_SDK_KEY>");
    }
}
```

## Initialization Options

| DVC Option | Description |
| --- | ----------- |
| enableEdgeDB | Enables the usage of EdgeDB for DevCycle that syncs User Data to DevCycle. <br />NOTE: This is only available with Cloud Bucketing. |

```java
import com.devcycle.sdk.server.cloud.api.DVCCloudClient;
import com.devcycle.sdk.server.cloud.model.DVCCloudOptions;

public class MyClass {
    
    private DVCCloudClient dvcCloudClient;

    private DVCCloudOptions dvcCloudOptions = DVCLocalOptions.builder()
        .enableEdgeDB(false)
        .build();
    
    public MyClass() {
        dvcCloudClient = new DVCCloudClient("<DVC_SERVER_SDK_KEY>", dvcCloudOptions);
    }
}
```
