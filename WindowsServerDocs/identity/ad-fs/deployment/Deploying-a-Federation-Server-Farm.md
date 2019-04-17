---
ms.assetid: bbb5b68f-00ad-4715-8176-0c2769b706c4
title: Guida alla distribuzione di Windows Server 2012 R2 AD ADFS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 05f1ea6830237813e6fd2bd6a172f467e8d81065
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="deploying-a-federation-server-farm"></a>Distribuzione di una Server Farm federativa

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Per distribuire una server farm federativa, completare le attività nell'elenco di controllo nell'ordine. Quando un collegamento di riferimento porta a un argomento concettuale, dopo aver consultato l'argomento concettuale, in modo da poter continuare con le attività rimanenti nell'elenco di controllo, tornare a questo elenco di controllo.  
  
![Distribuzione di federazione di server farm](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**elenco di controllo: distribuzione di una Server Farm federativa**  
  
||Attività|Riferimento|  
|-|--------|-------------|  
|![Distribuzione di federazione di server farm](media/icon_checkboxo.gif)|Rivedere importanti concetti e considerazioni durante la preparazione per la distribuzione di Active Directory Federation Services \(AD FS\). **Nota:**|![Distribuzione di federazione di server farm](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Guida alla progettazione di AD FS in Windows Server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)<br /><br />![Distribuzione di federazione di server farm](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
||Se si decide di utilizzare Microsoft SQL Server per l'archivio di configurazione di AD FS, assicurarsi di distribuire un'istanza funzionale di SQL Server.|[SQL Server](https://technet.microsoft.com/sqlserver)**avviso:** In Windows Server 2012 R2, se si desidera creare una farm ADFS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni successive, incluso SQL Server 2012.|  
|![Distribuzione di federazione di server farm](media/icon_checkboxo.gif)|Aggiungere il computer a un dominio di Active Directory.|![Distribuzione di federazione di server farm](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[aggiungere un Computer a un dominio](Join-a-Computer-to-a-Domain.md)|  
|![Distribuzione di federazione di server farm](media/icon_checkboxo.gif)|Registrare un certificato SSL (Secure Socket Layer) \(SSL\) per AD FS.|![Distribuzione di federazione di server farm](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[registrare un certificato SSL per ADFS](Enroll-an-SSL-Certificate-for-AD-FS.md)|  
|![Distribuzione di federazione di server farm](media/icon_checkboxo.gif)|Installare il servizio ruolo di AD FS.|![Distribuzione di federazione di server farm](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[installare il servizio ruolo ADFS](Install-the-AD-FS-Role-Service.md)|  
|![Distribuzione di federazione di server farm](media/icon_checkboxo.gif)|Configurare un server federativo.|![Distribuzione di federazione di server farm](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[configurare un Server federativo](Configure-a-Federation-Server.md)|  
|![Distribuzione di federazione di server farm](media/icon_checkboxo.gif)|Passaggio facoltativo: configurare un server federativo con Device Registration Service \(DRS\).|![Distribuzione di federazione di server farm](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configurare un server federativo con Device Registration Service](Configure-a-federation-server-with-Device-Registration-Service.md)|  
|![Distribuzione di federazione di server farm](media/icon_checkboxo.gif)|Aggiungere un record host \(A\) e alias \(CNAME\) risorsa aziendale Domain Name System \(DNS\) per il servizio federativo e DRS.|![Distribuzione di federazione di server farm](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configurare DNS aziendale per il servizio federativo e DRS](Configure-Corporate-DNS-for-the-Federation-Service-and-DRS.md)|  
|![Distribuzione di federazione di server farm](media/icon_checkboxo.gif)|Verificare che un server federativo sia operativo.|![Distribuzione di federazione di server farm](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[verificare che una federazione Server sia operativo](Verify-That-a-Federation-Server-Is-Operational.md)|  
  

## <a name="see-also"></a>Vedere anche  
[Distribuzione di ADFS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD ADFS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
  

