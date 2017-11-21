---
title: "Azure Functions �R�����r�q�������U���~���� | Microsoft Docs"
description: "Azure Functions �N Azure Notification Hub �U���~�����y�ϥ��@�r��k�R�K���M?�����e�@�C"
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "Azure Functions, ??, �~������?�z, �ʪ�ǯ�����������}��Ǭ, Ǳ�������Q���U�|��ǩ��ǫǽ��"
ms.assetid: 0ff0c949-20bf-430b-8dd5-d72b7b6ee6f7
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/26/2017
ms.author: glenga
ms.openlocfilehash: 02d01d0f6e945ed54dbe766aec2a0fd7c17c510f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-notification-hub-output-binding"></a>Azure Functions �R�����r�q�������U�X�O���~����
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

���U�O���N�V�BAzure Functions �N Azure Notification Hub �U���~�����y�c�����F�qǯ�����}��Ǭ���F�q�@�r��k�R�K���M?�����e�@�C 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

??�y�ϥ����M�B�c�����s�F Azure Notification Hub �N?���Uǯ�����y���J�M��ǿǳ��q���y�e�H�N���e�@�C �F�G���BAzure Notification Hub �V�B�ϥ��@�r����ǿ����ǥ���ܳq��Ǳ����ǵ (PNS) ���R�c���@�r���n�����q�e�@�C Azure Notification Hub �U�c����k�k�B�q���y�������r�F�h�R�n?�@�rǫ���~�|���� �|����ǭ��ǳ�����U�}?��k�U�Բ��R�K���M�V�B [Notification Hubs �U�ϥ�](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) �R?�@�r����Ǵ�y?�����M�B�W���U�ت��Uǫ���~�|���� ����ǿ����ǥ�����yǫ��ǿǫ���M���G����C

�e�H�N���r�q���V�B���~���}�ҳq���e�F�V�����������ĳq���N�@�C ���~���}�ҳq���V�B�X�O���~�����U `platform` ���������}�N�c�����s�F�S�w�U�q������ǿ����ǥ�����y?�H�O���M���e�@�C �����������ĳq���V�B��?�U����ǿ����ǥ�����y?�H�O���M�ϥ��N���e�@�C   

## <a name="notification-hub-output-binding-properties"></a>Notification Hub �X�O���~�����U���������}
function.json ���{�~���V�B���U���������}�y�������e�@�C


|���������}  |Description  |
|---------|---------|
|**name** | �q������ ��ǿǷ��Ǵ�U??ǯ�����N�ϥ����s�r??�W�C |
|**type** | `notificationHub` �R�]�w�@�r���n�����q�e�@�C |
|**tagExpression** | ǻǬ���C���s�R�o�q�BǻǬ���R�@�P�@�r�q���y���H�@�r�o���R�n?���F�@�s�U�����~ǵ�R�q���y�t�H�@�r�o���R���w�N���e�@�C  �Բ��R�K���M�V�B�u[�������}��Ǭ�OǻǬ��](../notification-hubs/notification-hubs-tags-segment-push-message.md)�v�y?�����M���G����C |
|**hubName** | Azure Portal ?�U�q������ ��ǹ��ǵ�U�W�e�C |
|**connection** | ���U��?��r�C�V�B�ϥ����M���r�q�������U *DefaultFullSharedAccessSignature* ?�R�]�w���s�F**�|����ǭ��ǳ����]�w**�U��?��r�C�N���r���n�����q�e�@�C |
|**direction** | `out` �R�]�w�@�r���n�����q�e�@�C | 
|**platform** | platform ���������}�V�B�q���U?�H�O�Q�r�q������ǿ����ǥ�����y�����e�@�C �J�w�N�V�B����ǿ����ǥ�����U���������}��X�O���~�������p�ٲ����s�M���r���X�B�����������ĳq���V�BAzure �q�������N�c�����s�M���r���N�U����ǿ����ǥ�����y?�H�O���M�ϥ��N���e�@�C Azure Notification Hub �N�������������y�ϥ����Mǫ��ǵ����ǿ����ǥ�����U�q���y�e�H�@�r�@�몺�Q��k�U�Բ��R�K���M�V�B�u[Templates (������������)](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md)�v�y?�����M���G����C �]�w���s�M���r���X�B_platform_ �R�V�B���U���A�s���U?�y���w�@�r���n�����q�e�@�C <ul><li><code>apns</code>&mdash;Apple Push Notification Service�C APNS �V���R Notification Hub �y�c���@�r��k�O�Bǫ���~�|���� �|�����N�q���y���H�@�r��k�U�Բ��R�K���M�V�B�u[Azure Notification Hubs ���p iOS �_�U��ǿǳ��q���U�e�H](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md)�v�y?�����M���G����C</li><li><code>adm</code>&mdash;[Amazon Device Messaging](https://developer.amazon.com/device-messaging)�C ADM �V���R Notification Hub �y�c���@�r��k�O�BKindle �|�����N�q���y���H�@�r��k�U�Բ��R�K���M�V�B�u[Notification Hubs �U�ϥ� (Kindle �|����)](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md)�v�y?�����M���G����C</li><li><code>gcm</code>&mdash;[Google Cloud Messaging](https://developers.google.com/cloud-messaging/)�C GCM �U�s��������Ǵ�����N���r Firebase Cloud Messaging �iǱ���������s�e�@�C �Բ��R�K���M�V�B�u[Azure Notification Hubs ���p Android �_�U��ǿǳ��q���U�e�H](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)�v�y?�����M���G����C</li><li><code>wns</code>&mdash;Windows ����ǿ����ǥ���ܦV���U [Windows Push Notification Services](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)�C WNS �N�V Windows Phone 8.1 �H���iǱ���������s�e�@�C �Բ��R�K���M�V�B�u[Notification Hubs �U�ϥ� (Windows ��������Ǳ�� ����ǿ����ǥ���� �|����)](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)�v�y?�����M���G����C</li><li><code>mpns</code>&mdash;[Microsoft Push Notification Service](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx)�C ���U����ǿ����ǥ�����N�V�BWindows Phone 8 ���o�Z�D�s�H�e�U Windows Phone ����ǿ����ǥ������Ǳ���������s�e�@�C �Բ��R�K���M�V�B�u[Windows Phone �N�U Azure Notification Hubs �y�ϥ����F��ǿǳ��q���U�e�H](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)�v�y?�����M���G����C</li></ul> |

function.json �U��:

```json
{
  "bindings": [
    {
      "name": "notification",
      "type": "notificationHub",
      "tagExpression": "",
      "hubName": "my-notification-hub",
      "connection": "MyHubConnectionString",
      "platform": "gcm",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

## <a name="notification-hub-connection-string-setup"></a>Notification Hub �U��?��r�C�U�]�w
Notification Hub �X�O���~�����y�ϥ��@�r�R�V�B�����U��?��r�C�y�]�w�@�r���n�����q�e�@�C �J�s�U�q�������y��?�@�r��B??�U "*�ΦX*" ǻ�����p�s���������y�@���N���e�@�C ��?��r�C�y����N�c���@�r���O�i�N���e�@�C 

�J�s�U�q�������R?�@�r��?��r�C�y�c���@�r�R�V:

1. [Azure Portal](https://portal.azure.com) �N�q�������R�������B**[�|ǫǷǵ����ǳ��]** �y��?���M�B**[DefaultFullSharedAccessSignature]** ����ǳ���U?�R���rǯ���� ��ǻ���y��?���e�@�C ���s�R�o�q�B*DefaultFullSharedAccessSignature* ����ǳ���U��?��r�C��q�������Rǯ�������s�e�@�C ���U��?��r�C�R�o�q�B�q����ǿǷ��Ǵ�y�e�H�@�r�F�h�U??�|ǫǷǵ?����I�O���s�e�@�C 
    ![�q�������U��?��r�C�yǯ�����@�r](./media/functions-bindings-notification-hubs/get-notification-hub-connection.png)
1. Azure Portal �U??�|�����R�������B**[�|����ǭ��ǳ����]�w]** �y��?���B`MyHubConnectionString` �Q�P�Uǩ���y�l�[���e�@�C���R�B�q�����ҥ��Rǯ�������s�F *DefaultFullSharedAccessSignature* �y?�O���M�K�q�I���M�B**[�O�s]** �yǫ��ǿǫ���e�@�C

���s�N�B�X�O���~�����U�q�����ұ�?�y�w�q�@�r�B���U�W�e�I���|����ǭ��ǳ����]�w�y�ϥ��N���e�@�C

## <a name="apns-native-notifications-with-c-queue-triggers"></a>C# ǩ���� ����Ǩ���y�ϥ����F APNS ���~���}�ҳq��
���U���N�V�B[Microsoft Azure Notification Hubs ���~������](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)�R�w�q���s�Fǻ�~���y�ϥ����M���~���}���U APNS �q���y�e�H�@�r��k�y�����e�@�C 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The JSON format for a native APNS notification is ...
    // { "aps": { "alert": "notification message" }}  

    log.Info($"Sending APNS notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants to be added (" + 
                                        user.name + ")\" }}";
    log.Info($"{apnsNotificationPayload}");
    await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
}
```

## <a name="gcm-native-notifications-with-c-queue-triggers"></a>C# ǩ���� ����Ǩ���y�ϥ����F GCM ���~���}�ҳq��
���U���N�V�B[Microsoft Azure Notification Hubs ���~������](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)�R�w�q���s�Fǻ�~���y�ϥ����M���~���}���U GCM �q���y�e�H�@�r��k�y�����e�@�C 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The JSON format for a native GCM notification is ...
    // { "data": { "message": "notification message" }}  

    log.Info($"Sending GCM notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants to be added (" + 
                                        user.name + ")\" }}";
    log.Info($"{gcmNotificationPayload}");
    await notification.AddAsync(new GcmNotification(gcmNotificationPayload));        
}
```

## <a name="wns-native-notifications-with-c-queue-triggers"></a>C# ǩ���� ����Ǩ���y�ϥ����F WNS ���~���}�ҳq��
���U���N�V�B[Microsoft Azure Notification Hubs ���~������](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)�R�w�q���s�Fǻ�~���y�ϥ����M���~���}���U WNS ����ǵ�ĳq���y�e�H�@�r��k�y�����e�@�C 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The XML format for a native WNS toast notification is ...
    // <?xml version="1.0" encoding="utf-8"?>
    // <toast>
    //      <visual>
    //     <binding template="ToastText01">
    //       <text id="1">notification message</text>
    //     </binding>
    //   </visual>
    // </toast>

    log.Info($"Sending WNS toast notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string wnsNotificationPayload = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                                    "<toast><visual><binding template=\"ToastText01\">" +
                                        "<text id=\"1\">" + 
                                            "A new user wants to be added (" + user.name + ")" + 
                                        "</text>" +
                                    "</binding></visual></toast>";

    log.Info($"{wnsNotificationPayload}");
    await notification.AddAsync(new WindowsNotification(wnsNotificationPayload));        
}
```

## <a name="template-example-for-nodejs-timer-triggers"></a>Node.js ǻ�~���� ����Ǩ���U�������������U��
���U���N�V�B`location` �O `message` �y�t�g[�����������ĵn?](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md)�U�q���y�e�H���e�@�C

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    context.log('Node.js timer trigger function ran!', timeStamp);  
    context.bindings.notification = {
        location: "Redmond",
        message: "Hello from Node!"
    };
    context.done();
};
```

## <a name="template-example-for-f-timer-triggers"></a>F# ǻ�~���� ����Ǩ���U�������������U��
���U���N�V�B`location` �O `message` �y�t�g[�����������ĵn?](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md)�U�q���y�e�H���e�@�C

```fsharp
let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
    notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]
```

## <a name="template-example-using-an-out-parameter"></a>out ��������ǻ���y�ϥ����F�������������U��
���U���N�V�B�������������R `message` ������ǵ ����Ǽ���y�t�g[�����������ĵn?](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md)�U�q���y�e�H���e�@�C

```cs
using System;
using System.Threading.Tasks;
using System.Collections.Generic;

public static void Run(string myQueueItem,  out IDictionary<string, string> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = GetTemplateProperties(myQueueItem);
}

private static IDictionary<string, string> GetTemplateProperties(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["message"] = message;
    return templateProperties;
}
```

## <a name="template-example-with-asynchronous-function"></a>�D�P��??�y�ϥ��@�r�������������U��
�D�P��ǯ�����y�ϥ����M���r���X�V�Bout ��������ǻ���y�ϥ��N���e�B�z�C �D�U���X�V�B`IAsyncCollector` �y�ϥ����M�����������ĳq���y�����e�@�C ���Uǯ�����N�V�B�W�O�Uǯ�����U�D�P���U���y�����e�@�C 

```cs
using System;
using System.Threading.Tasks;
using System.Collections.Generic;

public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    log.Info($"Sending Template Notification to Notification Hub");
    await notification.AddAsync(GetTemplateProperties(myQueueItem));    
}

private static IDictionary<string, string> GetTemplateProperties(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["user"] = "A new user wants to be added : " + message;
    return templateProperties;
}
```

## <a name="template-example-using-json"></a>JSON �y�ϥ����F�������������U��
���U���N�V�B��?�Q JSON ��r�C�y�ϥ����M�B�������������R `message` ������ǵ ����Ǽ���y�t�g[�����������ĵn?](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md)�U�q���y�e�H���e�@�C

```cs
using System;

public static void Run(string myQueueItem,  out string notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
}
```

## <a name="template-example-using-notification-hubs-library-types"></a>Notification Hubs ���~�������Uǻ�~���y�ϥ����F�������������U��
���U���N�V�B[Microsoft Azure Notification Hubs ���~������](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)�R�w�q���s�Fǻ�~���U�ϥΤ�k�y�����e�@�C 

```cs
#r "Microsoft.Azure.NotificationHubs"

using System;
using System.Threading.Tasks;
using Microsoft.Azure.NotificationHubs;

public static void Run(string myQueueItem,  out Notification notification, TraceWriter log)
{
   log.Info($"C# Queue trigger function processed: {myQueueItem}");
   notification = GetTemplateNotification(myQueueItem);
}

private static TemplateNotification GetTemplateNotification(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["message"] = message;
    return new TemplateNotification(templateProperties);
}
```

## <a name="next-steps"></a>���Uǵ��ǿ��
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

