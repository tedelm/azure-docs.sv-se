---
title: Säkra Azure Data Lake Analytics för flera användare
description: Lär dig hur du konfigurerar flera användare att köra jobb i Azure Data Lake Analytics.
ms.service: data-lake-analytics
services: data-lake-analytics
author: matt1883
ms.author: mahi
ms.reviewer: jasonwhowell
ms.topic: conceptual
ms.date: 05/30/2018
ms.openlocfilehash: 9fbc94259d6fdfb6758204efd6e6f0a346dc58da
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "60813369"
---
# <a name="configure-user-access-to-job-information-to-job-information-in-azure-data-lake-analytics"></a>Konfigurera användar åtkomst till jobb information till jobb information i Azure Data Lake Analytics 

I Azure Data Lake Analytics kan du använda flera användar konton eller tjänstens huvud namn för att köra jobb. 

För att samma användare ska kunna se den detaljerade jobb informationen måste användarna kunna läsa innehållet i arbetsmapparna. Jobbets mappar finns i `/system/` katalogen. 

Om de behörigheter som krävs inte har kon figurer ATS kan användaren se ett fel:`Graph data not available - You don't have permissions to access the graph data.` 

## <a name="configure-user-access-to-job-information"></a>Konfigurera användar åtkomst till jobb information

Du kan använda **guiden Lägg till användare** för att konfigurera ACL: er på mapparna. Mer information finns i [lägga till en ny användare](data-lake-analytics-manage-use-portal.md#add-a-new-user).

Om du behöver mer detaljerad kontroll eller om du behöver skripta behörigheterna skyddar du mapparna på följande sätt:

1. Bevilja **Kör** -behörigheter (via en åtkomst-ACL) i rotmappen:
   - /
   
2. Bevilja **Kör** -och **Läs** behörigheter (via en åtkomst-ACL och en standard-ACL) på de mappar som innehåller arbetsmapparna. Till exempel, för ett jobb som kördes den 25 maj 2018, behöver du åtkomst till dessa mappar:
   - /system
   - /system/jobservice
   - /system/jobservice/jobs
   - /system/jobservice/jobs/Usql
   - /system/jobservice/jobs/Usql/2018
   - /system/jobservice/jobs/Usql/2018/05
   - /system/jobservice/jobs/Usql/2018/05/25
   - /system/jobservice/jobs/Usql/2018/05/25/11
   - /system/jobservice/jobs/Usql/2018/05/25/11/01
   - /system/jobservice/jobs/Usql/2018/05/25/11/01/b074bd7a-1448-d879-9d75-f562b101bd3d

## <a name="next-steps"></a>Nästa steg
[Lägg till en ny användare](data-lake-analytics-manage-use-portal.md#add-a-new-user)
