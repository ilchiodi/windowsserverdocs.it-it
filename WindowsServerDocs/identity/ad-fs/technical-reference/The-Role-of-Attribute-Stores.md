---
ms.assetid: 4ddb927d-d65e-491d-840a-16049c083d13
title: Ruolo degli archivi attributi
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 730411ed7efbb9cf0db3d7e94a486cec4c363849
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860412"
---
 >Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-attribute-stores"></a>Ruolo degli archivi attributi
Active Directory Federation Services viene utilizzato il termine "archivio attributi" per fare riferimento alla directory o i database usati da un'organizzazione per archiviare gli account utente e i relativi valori di attributo associati. Dopo averlo configurato in un'organizzazione di provider di identità, AD FS recupera i valori degli attributi dall'archivio e crea attestazioni basate su tali informazioni in modo che un'applicazione Web o un servizio ospitato in un'organizzazione relying party possa prendere appropriato le decisioni di autorizzazione ogni volta che un utente federato \(un utente il cui account è archiviato nell'organizzazione del provider di identità\) tenta di accedere all'applicazione o servizio.  
  
Per altre informazioni su come vengono generate le attestazioni, vedere [The Role of Claims](The-Role-of-Claims.md).  
  
## <a name="how-attribute-stores-fit-in-with-your-ad-fs-deployment-goals"></a>Come gli archivi attributi rispondono agli obiettivi di distribuzione di AD FS  
Il percorso dell'archivio attributi utente e la posizione da cui eseguire l'autenticazione degli utenti determinano la modalità di progettazione di ADFS per supportare le identità utente. A seconda di dove si trova l'archivio di attributi e in cui gli utenti accedono all'applicazione \(in una rete intranet o Internet\), è possibile usare uno degli obiettivi di distribuzione seguenti:  
  
-   [Fornire l'accesso ad Active Directory gli utenti ai servizi e Your Claims-Aware Applications](https://technet.microsoft.com/library/dd807071.aspx): In questo obiettivo gli utenti dell'organizzazione accedere a un'applicazione AD protetta con ADFS o un servizio \(le proprie applicazioni o servizi o un oggetto applicazione o un servizio del partner\) quando gli utenti sono connessi ad Active Directory nella intranet aziendale.  
  
-   [Fornire l'accesso ad Active Directory gli utenti alle applicazioni e servizi di altre organizzazioni](https://technet.microsoft.com/library/dd807123.aspx): In questo obiettivo gli utenti dell'organizzazione accedere a un'applicazione AD protetta con ADFS o un servizio \(le proprie applicazioni o servizi o un oggetto applicazione o un servizio del partner\) quando gli utenti sono connessi a un archivio attributi nella intranet aziendale e quando effettuano l'accesso remoto da Internet.  
  
-   [Fornire agli utenti in un'altra organizzazione l'accesso ai servizi e Your Claims-Aware Applications](https://technet.microsoft.com/library/dd807099.aspx): In questo obiettivo, gli account utente in un'altra organizzazione che si trovano in un archivio attributi nella intranet aziendale dell'organizzazione devono accedere a un AD FS: protetti dell'applicazione all'interno dell'organizzazione. Questo obiettivo funziona anche quando consumer\-è necessario specificare gli account utente in base che si trovano in un archivio attributi nella rete perimetrale dell'organizzazione con accesso a un'istanza di AD applicazione protetta con ADFS nell'organizzazione.  
  
A seconda della posizione dell'archivio attributi e altri requisiti dell'organizzazione, è possibile combinare più obiettivi di distribuzione per completare la progettazione della distribuzione di AD FS.  
  
## <a name="attribute-stores-that-are-supported-by-ad-fs"></a>Archivi attributi supportati da AD FS  
ADFS supporta un'ampia gamma di directory e database sono archiviati che è possibile usare per l'estrazione amministratore\-definiti i valori di attributo e popolare le attestazioni con tali valori. ADFS supporta le directory seguenti o i database come archivi attributi:  
  
-   Active Directory in Windows Server 2003, Active Directory Domain Services \(Active Directory Domain Services\) in Windows Server 2008, Active Directory Domain Services in Windows Server 2012 e 2012 R2 e Windows Server 2016. 
  
-   Tutte le edizioni di Microsoft SQL Server 2005, SQL Server 2008, SQL Server 2012, SQL Server 2014 e SQL Server 2016  
  
-   Archivi attributi personalizzati  
  

