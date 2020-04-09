---
ms.assetid: 4ddb927d-d65e-491d-840a-16049c083d13
title: Ruolo degli archivi attributi
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 595f0b3b11172df9bf95d3bb8aab90368bb0e77f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860194"
---
# <a name="the-role-of-attribute-stores"></a>Ruolo degli archivi attributi
Active Directory Federation Services utilizza il termine "archivi attributi" per fare riferimento a directory o database utilizzati da un'organizzazione per archiviare gli account utente e i relativi valori di attributo associati. Una volta configurato in un'organizzazione del provider di identità, AD FS recupera questi valori di attributo dall'archivio e crea attestazioni in base a tali informazioni in modo che un'applicazione o un servizio Web ospitato in un'organizzazione relying party possa prendere le decisioni di autorizzazione appropriate ogni volta che un utente federato \(un utente il cui account è archiviato nell'organizzazione del provider di identità\) tenta di accedere all'applicazione o al servizio.  
  
Per altre informazioni su come vengono generate le attestazioni, vedere [The Role of Claims](The-Role-of-Claims.md).  
  
## <a name="how-attribute-stores-fit-in-with-your-ad-fs-deployment-goals"></a>Come gli archivi attributi rispondono agli obiettivi di distribuzione di AD FS  
Il percorso dell'archivio attributi utente e la posizione da cui gli utenti si autenticano determinano come progettare AD FS per supportare le identità utente. A seconda della posizione in cui si trova l'archivio attributi e della posizione in cui gli utenti accedono all'applicazione \(in una rete Intranet o in Internet\), è possibile usare uno degli obiettivi di distribuzione seguenti:  
  
-   [Consentire agli utenti Active Directory di accedere ai servizi e alle applicazioni in grado di riconoscere attestazioni](https://technet.microsoft.com/library/dd807071.aspx): in questo obiettivo gli utenti dell'organizzazione accedono a un'applicazione o a un servizio protetto da ad FS \(la propria applicazione o servizio oppure l'\) applicazione o il servizio di un partner, quando gli utenti sono connessi a Active Directory nella Intranet aziendale.  
  
-   [Fornire agli utenti Active Directory l'accesso alle applicazioni e ai servizi di altre organizzazioni](https://technet.microsoft.com/library/dd807123.aspx): in questo obiettivo gli utenti dell'organizzazione accedono a un'applicazione o a un servizio protetto da ad FS \(la propria applicazione o servizio oppure l'\) applicazione o il servizio di un partner, quando gli utenti sono connessi a un archivio di attributi nella Intranet aziendale e quando eseguono l'accesso in remoto da Internet.  
  
-   [Fornire agli utenti di un'altra organizzazione l'accesso ai servizi e alle applicazioni in grado di riconoscere attestazioni](https://technet.microsoft.com/library/dd807099.aspx): in questo obiettivo gli account utente di un'altra organizzazione che si trovano in un archivio attributi nella Intranet aziendale di tale organizzazione devono accedere a un'applicazione protetta da ad FS nell'organizzazione. Questo obiettivo funziona anche quando gli account utente basati su consumer\-che si trovano in un archivio attributi nella rete perimetrale dell'organizzazione devono essere forniti con accesso a un'applicazione protetta da AD FS nell'organizzazione.  
  
A seconda della posizione dell'archivio attributi e di altri requisiti dell'organizzazione, è possibile combinare diversi obiettivi di distribuzione per completare la progettazione della distribuzione di AD FS.  
  
## <a name="attribute-stores-that-are-supported-by-ad-fs"></a>Archivi attributi supportati da AD FS  
AD FS supporta un'ampia gamma di archivi di directory e database che è possibile usare per estrarre i valori degli attributi definiti dall'amministratore\-e popolare le attestazioni con tali valori. AD FS supporta le directory o i database seguenti come archivi di attributi:  
  
-   Active Directory in Windows Server 2003, Active Directory Domain Services \(AD DS\) in Windows Server 2008, servizi di dominio Active Directory in Windows Server 2012 e 2012 R2 e Windows Server 2016. 
  
-   Tutte le edizioni di Microsoft SQL Server 2005, SQL Server 2008, SQL Server 2012, SQL Server 2014 e SQL Server 2016  
  
-   Archivi attributi personalizzati  
  

