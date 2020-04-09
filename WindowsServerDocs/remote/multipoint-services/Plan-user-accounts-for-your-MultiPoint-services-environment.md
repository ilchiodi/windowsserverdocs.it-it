---
title: Pianificare gli account utente per l'ambiente MultiPoint Services
description: Informazioni sulla pianificazione per gli account utente in MultiPoint Services
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: d47be540-e891-47bd-85da-6df4bbf93b2f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 28ee7a1475ec55352fe344842b8df7633abb9137
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853394"
---
# <a name="plan-user-accounts-for-your-multipoint-services-environment"></a>Pianificare gli account utente per l'ambiente MultiPoint Services
Il modo migliore per implementare gli account utente in MultiPoint Services dipende dalle dimensioni e dalla complessità della distribuzione:  
  
-   **Account utente locali** : per una distribuzione di piccole dimensioni con pochi computer che eseguono Servizi MultiPoind e pochi utenti, potrebbe essere più pratico usare *account utente locali* creati in MultiPoint Services. È possibile creare un singolo account per ogni persona che utilizzerà il sistema o creare un account generico per ogni stazione, che chiunque può utilizzare per l'accesso. Gli amministratori di MultiPoint Services creano e gestiscono gli account utente locali tramite Gestione MultiPoint. Gli account locali possono essere amministratori, avere diritti amministrativi limitati o essere utenti normali senza accesso a MultiPoint Services desktop o MultiPoint Manager.  
  
-   **Account di dominio** : se nell'ambiente sono presenti molti computer che eseguono multipoint Services e molti utenti, probabilmente sarà più utile configurare un Active Directory Domain Services \(dominio di servizi di dominio Active\) directory e usare *account utente di dominio*, che consentono a un utente di accedere al proprio profilo utente e alle proprie impostazioni da qualsiasi stazione nel dominio. Gli account utente di dominio devono essere creati nel controller di dominio da un amministratore di dominio.  
  
> [!NOTE]  
> Le sezioni seguenti illustrano gli scenari che è possibile implementare per gli account utente locali in MultiPoint Services. Se si usano account utente di dominio, vedere lo scenario "uno o più server MultiPoint in un ambiente di rete di dominio" in [scenari di esempio: account utente di servizi multipoint](Example-scenarios--MultiPoint-Services-user-accounts.md).  
  
## <a name="planning-local-user-accounts"></a>Pianificazione degli account utente locali  
Nelle sezioni seguenti vengono considerati i vantaggi, gli svantaggi e i requisiti per diversi modi di implementare account utente locali singoli o condivisi nell'ambiente Windows MultiPoint Services.  
  
### <a name="use-individual-local-user-accounts"></a>Usare account utente locali singoli  
Quando si creano account utente locali, è possibile scegliere tra due approcci.  Assegnare ogni utente a un particolare server che esegue MultiPoint Services e creare un singolo account per ogni utente. In alternativa, creare account utente locali per tutti gli utenti in tutti i computer che eseguono multipoint Services. Un vantaggio fondamentale dell'implementazione di singoli account utente è che ogni utente dispone di una propria esperienza desktop di Windows che include cartelle private per l'archiviazione dei dati. 
  
Dal punto di vista della gestione del sistema, l'assegnazione di utenti a uno specifico computer Servizi MultiPoint potrebbe essere più utile. Se, ad esempio, si dispone di due server MultiPoint con cinque stazioni ciascuno, è possibile creare account utente locali come illustrato nella tabella seguente.  
  
**Tabella 1: assegnazione di account utente locali a computer specifici che eseguono MultiPoint Services**  
  
|Computer A|Computer B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_06|  
|UserAccount_02|UserAccount_07|  
|UserAccount_03|UserAccount_08|  
|UserAccount_04|UserAccount_09|  
|UserAccount_05|UserAccount_10|  
  
In questo scenario ogni utente dispone di un singolo account in un determinato computer. Pertanto, chiunque disponga di un account locale nel computer A può accedere al proprio account da qualsiasi stazione associata al computer A. Tuttavia, questi utenti non possono accedere ai propri account se utilizzano una stazione associata al computer B e viceversa. Un vantaggio di questo approccio consiste nel fatto che, connettendosi sempre allo stesso computer, gli utenti possono sempre trovare e accedere ai file.  
  
Al contrario, è anche possibile replicare singoli account utente in tutti i computer che eseguono MultiPoint Services, come illustrato nella tabella seguente.  
  
**Tabella 2: replica degli account utente in tutti i computer che eseguono MultiPoint Services**  
  
|Computer A|Computer B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_01|  
|UserAccount_02|UserAccount_02|  
|UserAccount_03|UserAccount_03|  
|UserAccount_04|UserAccount_04|  
|UserAccount_05|UserAccount_05|  
  
Un vantaggio di questo approccio consiste nel fatto che gli utenti dispongono di un account utente locale in tutti i servizi MultiPoint disponibili. Tuttavia, gli svantaggi potrebbero superare questo vantaggio. Ad esempio, anche se il nome utente e la password di una determinata persona sono gli stessi in entrambi i computer, gli account non sono collegati tra loro. Pertanto, se un utente accede al proprio account nel computer A lunedì, salva un file e quindi accede al proprio account nel computer B il martedì, non sarà in grado di accedere al file salvato in precedenza nel computer A. Inoltre, la replica degli account utente in più computer aumenta il sovraccarico amministrativo e i requisiti di archiviazione.  
  
### <a name="use-generic-local-user-accounts"></a>Usare account utente locali generici  
Se il sistema MultiPoint Services non è connesso a un dominio e non si vuole creare un singolo account per ogni utente, è possibile creare account generici per ogni stazione. Se, ad esempio, si dispone di due computer che eseguono MultiPoint Services e cinque stazioni sono associate a ogni computer, è possibile decidere di creare account utente simili a quelli illustrati nella tabella seguente.  
  
**Tabella 3: creazione di account utente generici, un account per stazione**  
  
|Computer A|Computer B|  
|--------------|--------------|  
|Computer_A Station_01|Computer_B Station_01|  
|Computer_A Station_02|Computer_B Station_02|  
|Computer_A Station_03|Computer_B Station_03|  
|Computer_A Station_04|Computer_B Station_04|  
|Computer_A Station_05|Computer_B Station_05|  
  
In questo scenario, ogni account della stazione ha la stessa password e le password e i nomi degli account utente generici sono disponibili per tutti gli utenti. Un vantaggio di questo approccio consiste nel fatto che il sovraccarico della gestione degli account utente è probabilmente inferiore rispetto all'uso di singoli account, perché in genere sono presenti meno stazioni rispetto agli utenti. Inoltre, l'overhead causato dalla replica degli account utente in ogni server viene eliminato.  
  
Un'altra opzione consiste nel creare account generici in ogni server. Ogni utente accede a un server con lo stesso account. Per consentire questa operazione, è necessario abilitare più sessioni per ogni account. È possibile semplificare ulteriormente utilizzando lo stesso nome di account e la stessa password in tutti i server. Questa operazione semplifica l'accesso per gli utenti, che necessitano solo di un nome di account e di una password per usare qualsiasi stazione in qualsiasi server. Si noti che in questo scenario tutti gli utenti possono visualizzare tutte le modifiche apportate da qualsiasi utente. Se, ad esempio, un file viene salvato sul desktop, tutti gli utenti possono visualizzare il file.  
  
> [!IMPORTANT]  
> È importante comprendere che, quando gli utenti condividono un account utente, uno per server o uno per stazione, i file salvati nel server, anche i file salvati nei documenti, non sono privati. Tutti gli utenti che accedono con l'account hanno accesso a tali file. Quando si usa un account per stazione, se un utente salva i file in documenti in una stazione, l'utente non ha accesso a tali file in una stazione diversa. Lo stesso si verifica quando si accede a diversi computer Servizi MultiPoint.  
  
Per consentire agli utenti di accedere ai file da qualsiasi stazione, è possibile usare un file server, creare una condivisione file per ogni account utente oppure consentire agli utenti di archiviare i documenti personali in un'unità flash USB o in un altro dispositivo di archiviazione privata. Le singole unità flash USB consentono a singoli utenti di archiviare documenti privati anche se condividono un account utente in un servizio MultiPoint.