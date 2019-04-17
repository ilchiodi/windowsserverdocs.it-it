---
ms.assetid: 4ddb927d-d65e-491d-840a-16049c083d13
title: Il ruolo degli archivi attributi
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 730411ed7efbb9cf0db3d7e94a486cec4c363849
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
 >Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-attribute-stores"></a>Il ruolo degli archivi attributi
Active Directory Federation Services viene utilizzato il termine "archivio attributi" per fare riferimento a una directory o i database che un'organizzazione Usa per archiviare gli account utente e i relativi valori di attributo associato. Dopo che è configurato in un'organizzazione di provider di identità, ADFS recupera questi valori di attributo dall'archivio e crea attestazioni in base a tali informazioni in modo che un'applicazione Web o un servizio ospitato in un'organizzazione con relying party possa prendere le decisioni di autorizzazione appropriata ogni volta che un utente federato \ (un utente il cui account è archiviato in organization\ di provider di identità) tenta di accedere all'applicazione o servizio.  
  
Per ulteriori informazioni su come vengono generate le attestazioni, vedere [il ruolo di attestazioni](The-Role-of-Claims.md).  
  
## <a name="how-attribute-stores-fit-in-with-your-ad-fs-deployment-goals"></a>Come gli archivi attributi rispondono gli obiettivi di distribuzione di ADFS  
Il percorso dell'archivio attributi utente e la posizione da cui gli utenti si autenticano determinano come progettare ADFS per supportare le identità utente. A seconda di dove si trova l'archivio di attributi e in cui gli utenti potranno accedere all'applicazione \ (una intranet o l'Internet), è possibile utilizzare uno degli obiettivi di distribuzione seguenti:  
  
-   [Fornire l'accesso ad Active Directory gli utenti a Your Claims-Aware Applications and Services](https://technet.microsoft.com/library/dd807071.aspx): In questo obiettivo gli utenti dell'organizzazione accedono a un'applicazione protetta con ADFS di Active Directory o un servizio \ (la propria applicazione o servizio o un partner applicazione o Service \) quando gli utenti sono connessi ad Active Directory nella intranet aziendale.  
  
-   [Fornire l'accesso ad Active Directory gli utenti per le applicazioni e servizi di altre organizzazioni](https://technet.microsoft.com/library/dd807123.aspx): In questo obiettivo gli utenti dell'organizzazione accedono a un'applicazione protetta con ADFS di Active Directory o un servizio \ (la propria applicazione o servizio o un partner applicazione o Service \) quando gli utenti sono connessi a un archivio attributi nella intranet aziendale e quando effettuano l'accesso remoto da Internet.  
  
-   [Fornire agli utenti in un'altra organizzazione l'accesso a servizi e delle applicazioni in grado di riconoscere attestazioni](https://technet.microsoft.com/library/dd807099.aspx): In questo obiettivo gli account utente in un'altra organizzazione che si trovano in un archivio attributi nella intranet aziendale dell'organizzazione che devono accedere a un'applicazione protetta con ADFS nell'organizzazione. Questo obiettivo funziona anche quando è necessario specificare gli account utente basati su consumer\ che si trovano in un archivio attributi nella rete perimetrale dell'organizzazione con accesso a un'applicazione protetta con ADFS nell'organizzazione Active Directory.  
  
A seconda della posizione dell'archivio attributi e altri requisiti dell'organizzazione, è possibile combinare più obiettivi di distribuzione per completare la progettazione della distribuzione di ADFS.  
  
## <a name="attribute-stores-that-are-supported-by-ad-fs"></a>Sono supportati da AD FS archivi attributi  
ADFS supporta un'ampia gamma di directory e database archivia che è possibile utilizzare per l'estrazione degli attributi definiti dal administrator\ e popolare le attestazioni con tali valori. ADFS supporta le seguenti directory o database come gli archivi di attributi:  
  
-   Active Directory in Windows Server 2003, Active Directory Domain Services \(AD DS\) in Windows Server 2008, servizi di dominio Active Directory in Windows Server 2012 e 2012 R2 e Windows Server 2016. 
  
-   Tutte le edizioni di Microsoft SQL Server 2005, SQL Server 2008, SQL Server 2012, SQL Server 2014 e SQL Server 2016  
  
-   Archivi di attributi personalizzati  
  

