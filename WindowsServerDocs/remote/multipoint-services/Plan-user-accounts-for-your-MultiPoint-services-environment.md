---
title: Pianificare gli account utente per l'ambiente MultiPoint Services
description: Informazioni sulla pianificazione per gli account utente in servizi MultiPoint
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d47be540-e891-47bd-85da-6df4bbf93b2f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 02862c1a317dfe5deff75be4a80595c8dc8bc3f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864172"
---
# <a name="plan-user-accounts-for-your-multipoint-services-environment"></a>Pianificare gli account utente per l'ambiente MultiPoint Services
Il modo migliore per implementare gli account utente in servizi MultiPoint dipende dalle dimensioni e della complessità della distribuzione:  
  
-   **Account utente locali** - per una piccola distribuzione con solo pochi computer dei servizi in esecuzione MultiPoind e pochi utenti, può risultare più comodo usare *gli account utente locali* che vengono creati in MultiPoint Services. È possibile creare un account singolo per ogni persona che usa il sistema o creare un account generico per ogni stazione, che chiunque può utilizzare per l'accesso. Gli amministratori di servizi multiPoint creare e gestire gli account utente locali tramite Gestione MultiPoint. Gli account locali possono essere amministratori, dispongano di diritti amministrativi limitati o essere degli utenti normali senza accesso al computer MultiPoint Services Desktop o gestione MultiPoint.  
  
-   **Gli account di dominio** -se l'ambiente dispone di molti computer che esegue servizi MultiPoint e molti utenti, probabilmente risulterà più utile per configurare un Active Directory Domain Services \(Active Directory Domain Services\) dominio e usare *gli account utente di dominio*, che consentire agli utenti di accedere a proprio profilo utente e impostazioni da qualsiasi stazione nel dominio. L'amministratore di dominio è necessario creare account utente di dominio nel controller di dominio.  
  
> [!NOTE]  
> Le sezioni seguenti illustrano scenari in cui è possibile implementare per gli account utente locale in MultiPoint Services. Se si usa gli account utente di dominio, consultare lo scenario "uno o più MultiPoint Server in un ambiente di rete di dominio" in [gli scenari di esempio: Gli account utente di multiPoint Services](Example-scenarios--MultiPoint-Services-user-accounts.md).  
  
## <a name="planning-local-user-accounts"></a>Pianificazione di account utente locali  
Le sezioni seguenti prendere in considerazione i vantaggi, svantaggi, requisiti e per diversi modi per implementare gli account utente locale singolo o condivisa nell'ambiente Windows MultiPoint Services.  
  
### <a name="use-individual-local-user-accounts"></a>Usare gli account utente locale singolo  
Durante la creazione di account utente locali, si dispone di due approcci di opzione.  Assegnare ogni utente a un determinato server che esegue MultiPoint Services e la creazione di un singolo account per ogni utente. Oppure creare gli account utente locale per tutti gli utenti in ogni computer che esegue Multipoint services. Dei principali vantaggi dell'implementazione di singoli account utente è che ogni utente ha la propria esperienza desktop Windows che include cartelle private per l'archiviazione dei dati. 
  
Dal punto di vista di gestione del sistema, l'assegnazione di utenti a un computer di servizi MultiPoint specifico potrebbe essere più opportuno. Ad esempio, se sono presenti due server MultiPoint con cinque stazioni ognuna, si potrebbe creare account utente locale come illustrato nella tabella seguente.  
  
**Tabella 1: Assegnazione di account utente locali a specifici computer che esegue MultiPoint Services**  
  
|Computer|Computer B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_06|  
|UserAccount_02|UserAccount_07|  
|UserAccount_03|UserAccount_08|  
|UserAccount_04|UserAccount_09|  
|UserAccount_05|UserAccount_10|  
  
In questo scenario, ogni utente dispone di un singolo account di un determinato computer. Pertanto, chiunque abbia un account locale nel Computer A è consentito accedere all'account sue da qualsiasi stazione associati a Computer A. Tuttavia, questi utenti non possono accedere i propri account che usano una stazione associata a Computer B e viceversa. Un vantaggio di questo approccio è che, connettendo sempre nello stesso computer, gli utenti possono sempre individuare e accedere ai file.  
  
Al contrario, è anche possibile eseguire la replica di singoli account utente in tutti i computer che esegue MultiPoint Services, come illustrato nella tabella seguente.  
  
**Tabella 2: La replica degli account utente in tutti i computer che esegue MultiPoint Services**  
  
|Computer|Computer B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_01|  
|UserAccount_02|UserAccount_02|  
|UserAccount_03|UserAccount_03|  
|UserAccount_04|UserAccount_04|  
|UserAccount_05|UserAccount_05|  
  
Un vantaggio di questo approccio è che gli utenti dispongano di un account utente locale in ogni disponibile di MultiPoint Services. Tuttavia, gli svantaggi potrebbero superare questo vantaggio. Ad esempio, anche se il nome utente e password per un particolare utente sono uguali in entrambi i computer, gli account non sono collegati tra loro. Pertanto, se un utente accede al proprio o l'account nel Computer un lunedì, consente di salvare un file e quindi accede al proprio account nel Computer B martedì, egli non sarà in grado di accedere al file salvato in precedenza nel Computer A. inoltre , la replica degli account utente in più computer aumenta i requisiti di archiviazione e il sovraccarico amministrativi.  
  
### <a name="use-generic-local-user-accounts"></a>Usare gli account utente locale generico  
Se il sistema MultiPoint Services non è connesso a un dominio e non si desidera creare un account singolo per ogni utente, è possibile creare account generici per ogni stazione. Ad esempio, se si dispone di due computer che esegue MultiPoint Services e cinque le stazioni sono associate a ogni computer, si potrebbe decidere di creare gli account utente simili a quelli illustrati nella tabella seguente.  
  
**Tabella 3: Creazione di account utente generico, un account per ogni stazione**  
  
|Computer|Computer B|  
|--------------|--------------|  
|Computer_A-Station_01|Computer_B-Station_01|  
|Computer_A-Station_02|Computer_B-Station_02|  
|Computer_A-Station_03|Computer_B-Station_03|  
|Computer_A-Station_04|Computer_B-Station_04|  
|Computer_A-Station_05|Computer_B-Station_05|  
  
In questo scenario, ogni account di stazione è la stessa password e la password sia i nomi degli account utente generico sono disponibili a tutti gli utenti. Un vantaggio di questo approccio è che l'overhead della gestione degli account utente è probabilmente meno se si usano account individuali, perché in genere sono presenti meno stazioni rispetto agli utenti. Inoltre, viene eliminato il sovraccarico dovuto alla replica degli account utente in ogni server.  
  
Un'altra opzione consiste nel creare account generici in ogni server. Ogni utente accede a un server con lo stesso account. A tale scopo, è necessario abilitare più sessioni per ogni account. È possibile semplificare ulteriormente con il nome dell'account stesso e la password in tutti i server. Ciò semplifica l'accesso per gli utenti, che devono conoscere solo un nome di account e password da utilizzare per qualsiasi stazione su qualunque server. Si noti che in questo scenario tutti gli utenti possono visualizzare qualsiasi modifica apportata qualsiasi utente. Ad esempio, se un file viene salvato sul desktop, tutti gli utenti possono vedere il file.  
  
> [!IMPORTANT]  
> È importante tenere presente che quando gli utenti condividono un account utente, uno per ogni server o uno per ogni stazione, i file salvati nel server, inclusi i file salvati nella cartella documenti - non privati. Qualsiasi utente che esegue l'accesso con l'account ha accesso a tali file. Quando si usa un account per ogni stazione, se un utente salva i file nella cartella documenti in una stazione, l'utente non ha accesso a tali file in una stazione diversa. Lo stesso si verifica quando l'accesso a diversi computer MultiPoint Services.  
  
Per consentire agli utenti di accedere ai file da qualsiasi stazione, è possibile usare un file server, creare una condivisione file per ogni account utente o consentire agli utenti di archiviare i propri documenti personali in un'unità flash USB o altro dispositivo di archiviazione privato. Singola unità memoria flash USB abilitare utenti singoli per archiviano documenti privati, anche se condividono un account utente in un MultiPoint Services.