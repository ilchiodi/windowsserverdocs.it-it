---
ms.assetid: bbb5b68f-00ad-4715-8176-0c2769b706c4
title: Guida alla distribuzione di ADFS in Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c0f8dca425f644952c36a289ec72651f6383e846
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192193"
---
# <a name="deploying-a-federation-server-farm"></a>Distribuzione di una server farm di federazione


Per distribuire una server farm federativa, completare le attività dell'elenco di controllo nell'ordine indicato. In presenza di un collegamento di riferimento a un argomento concettuale, dopo aver consultato quest'ultimo tornare a questo elenco di controllo, in modo da poter continuare con le attività rimanenti.  
  
![distribuzione di federazione di server farm](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**elenco di controllo: Distribuzione di una Server Farm federativa**  
  
||Attività|Riferimenti|  
|-|--------|-------------|  
|![distribuzione di federazione di server farm](media/icon_checkboxo.gif)|Rivedere importanti concetti e considerazioni durante la preparazione per la distribuzione di Active Directory Federation Services \(ADFS\). **Nota:**|![distribuzione di federazione di server farm](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Guida alla progettazione di AD FS in Windows Server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)<br /><br />![distribuzione di federazione di server farm](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
||Se si decide di usare Microsoft SQL Server come archivio di configurazione di AD FS, assicurarsi di distribuire un'istanza funzionale di SQL Server.|[SQL Server](https://technet.microsoft.com/sqlserver) **avviso:** In Windows Server 2012 R2 se si vuole creare una farm AD FS e usare SQL Server per archiviare i dati di configurazione, è possibile usare SQL Server 2008 e versioni successive, incluso SQL Server 2012.|  
|![distribuzione di federazione di server farm](media/icon_checkboxo.gif)|Aggiungere il computer a un dominio di Active Directory|![distribuzione di federazione di server farm](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[aggiungere un Computer a un dominio](Join-a-Computer-to-a-Domain.md)|  
|![distribuzione di federazione di server farm](media/icon_checkboxo.gif)|Registrare Secure Socket Layer \(SSL\) certificato per ADFS.|![distribuzione di federazione di server farm](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[registrare un certificato SSL per ADFS](Enroll-an-SSL-Certificate-for-AD-FS.md)|  
|![distribuzione di federazione di server farm](media/icon_checkboxo.gif)|Installare il servizio ruolo AD FS.|![distribuzione di federazione di server farm](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[installare il servizio ruolo ADFS](Install-the-AD-FS-Role-Service.md)|  
|![distribuzione di federazione di server farm](media/icon_checkboxo.gif)|Configurare un server federativo.|![distribuzione di federazione di server farm](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[configurare un Server federativo](Configure-a-Federation-Server.md)|  
|![distribuzione di federazione di server farm](media/icon_checkboxo.gif)|Passaggio facoltativo: Configurare un server federativo con Device Registration Service \(DRS\).|![distribuzione di federazione di server farm](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configurare un server federativo con Device Registration Service](Configure-a-federation-server-with-Device-Registration-Service.md)|  
|![distribuzione di federazione di server farm](media/icon_checkboxo.gif)|Aggiungere un host \(A\) e alias \(CNAME\) record di risorse di sistema di nome di dominio aziendale \(DNS\) per il servizio federativo e il servizio DRS.|![distribuzione di federazione di server farm](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configurare DNS aziendale per il servizio federativo e il servizio DRS](Configure-Corporate-DNS-for-the-Federation-Service-and-DRS.md)|  
|![distribuzione di federazione di server farm](media/icon_checkboxo.gif)|Verificare che un server federativo sia operativo.|![distribuzione di federazione di server farm](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[verificare che una federazione Server sia operativo](Verify-That-a-Federation-Server-Is-Operational.md)|  
  

## <a name="see-also"></a>Vedere anche  
[Distribuzione di AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
  

