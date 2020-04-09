---
title: Considerazioni sulla rete e account utente
description: Fornisce informazioni sulla pianificazione per diversi scenari di rete e utente con servizi MultiPoint
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: ef4859fc-b7ae-4827-ab9c-b1dc07ab6c16
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: ed9770ff6e91e548dfc38a1de927646590a25165
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853414"
---
# <a name="network-considerations-and-user-accounts"></a>Considerazioni sulla rete e account utente
MultiPoint Services può essere distribuito in diversi ambienti di rete e può supportare account utente locali e account utente di dominio. In genere, gli account utente di MultiPoint Services verranno gestiti in uno dei seguenti ambienti di rete:  
  
-   Un singolo computer che esegue MultiPoint Services con account utente locali  
  
-   Più computer che eseguono MultiPoint Services, ognuno con un account utente locale  
  
-   Più computer che eseguono MultiPoint Services e che usano account utente di dominio

Per definizione, è possibile accedere agli *account utente locali* solo dal computer in cui sono stati creati. Gli account utente locali sono account utente creati in un computer specifico che esegue MultiPoint Services. Al contrario, gli *account utente di dominio* sono account utente che si trovano in un controller di dominio e a cui è possibile accedere da qualsiasi computer connesso al dominio. Quando si decide quale tipo di ambiente di rete usare, tenere presente quanto segue:  
  
-   Le risorse verranno condivise tra i server?  
  
-   Gli utenti verranno scambiati tra i server?  
  
-   Gli utenti accederanno ai server di database che richiedono l'autenticazione?  
  
-   Gli utenti accedono ai server Web interni per i quali è necessaria l'autenticazione?  
  
-   Esiste un'infrastruttura di dominio Active Directory esistente?  
  
-   Chi utilizzerà la console di gestione MultiPoint per gestire i desktop degli utenti, visualizzare le anteprime, aggiungere utenti, limitare i siti Web e così via? L'utente deve gestire più di un server? Questa persona deve disporre di privilegi amministrativi sui server.  
  
Le sezioni seguenti indirizzano la gestione degli account utente in questi ambienti di rete.  
  
## <a name="single-multipoint-server-with-local-user-accounts"></a>Server MultiPoint singolo con account utente locali  
Negli ambienti con un singolo computer che esegue MultiPoint Services non è necessario disporre di una rete. Tuttavia, per sfruttare i vantaggi delle risorse Internet, i requisiti di rete possono essere di base come router e una connessione a un provider di servizi Internet (ISP). Per impostazione predefinita, le connessioni di rete associate a una scheda di rete in MultiPoint Services vengono configurate per ottenere automaticamente un indirizzo IP e un indirizzo del server DNS tramite DHCP. I router Internet vengono in genere configurati come server DHCP e forniscono indirizzi IP privati ai computer che si connettono alla rete interna. Pertanto, un singolo computer che esegue MultiPoint Services potrebbe essere in grado di connettersi all'interfaccia interna del router, ottenere informazioni IP automatiche e connettersi a Internet senza un impegno o una configurazione significativa da parte di un amministratore.  
  
Un modo comune per gestire gli utenti in questo tipo di ambiente consiste nel creare un account utente locale per ogni persona che accederà al sistema. Chiunque disponga di un account utente locale in tale computer può accedere a MultiPoint Services da qualsiasi stazione associata al sistema. Gli account utente locali possono essere creati e gestiti da Gestione MultiPoint.  
  
## <a name="multiple-multipoint-server-systems-with-local-user-accounts"></a>Più sistemi MultiPoint Server con account utente locali  
Poiché gli account utente locali sono accessibili solo dal computer in cui sono stati creati, quando si distribuiscono più sistemi MultiPoint Services in un ambiente, è possibile gestire gli account utente locali in uno dei due modi seguenti:  
  
-   È possibile creare account utente per utenti specifici in computer specifici che eseguono MultiPoint Services.  
  
-   È possibile usare Gestione MultiPoint per creare account per ogni utente in ogni computer che esegue MultiPoint Services.  
  
Ad esempio, se si prevede di assegnare gli utenti a un computer specifico che esegue MultiPoint Services, è possibile creare quattro account utente locali nel computer A (User01, User02, user03 e user04) e quattro account utente locali nel computer B (user05, user06, user07 e user08). In questo scenario, gli utenti 01\-04 possono accedere al computer A da qualsiasi stazione connessa. Tuttavia, non possono accedere al computer B. Lo stesso vale per gli utenti 05\-08, che potranno accedere solo al computer B, ma non al computer a. a seconda dell'ambiente di distribuzione specifico, questo può essere accettabile o addirittura auspicabile.  
  
Tuttavia, se ogni utente deve essere in grado di accedere a uno dei computer che eseguono MultiPoint Services, è necessario creare un account utente locale per ogni utente in ogni computer in cui è in esecuzione MultiPoint Services. La scelta di gestire gli utenti in questo modo introduce determinate complessità. Se, ad esempio, User01 accede al computer A su lunedì e salva un file nella cartella documenti, quindi l'utente accede al computer B il martedì, il file salvato nella cartella documenti del computer A non sarà accessibile nel computer B.  
  
Inoltre, se un utente dispone di account nel computer A e nel computer B, non è possibile sincronizzare automaticamente le password per gli account. Ciò può comportare una difficoltà di accesso degli utenti in caso di modifica della password dell'account in un computer, ma non nell'altra. È possibile semplificare la gestione degli account utente in questo tipo di ambiente di rete assegnando ogni utente a un singolo computer che esegue MultiPoint Services. In questo modo, l'utente può accedere a una delle stazioni associate a tale computer e accedere ai file appropriati.  
  
## <a name="multiple-multipoint-services-systems-with-domain-accounts"></a>Più sistemi MultiPoint Services con account di dominio  
Gli ambienti di dominio sono comuni in ambienti di rete di grandi dimensioni che includono più server. Ad esempio, è possibile aggiungere uno o più computer che eseguono il ruolo Servizi MultiPoint a un dominio e quindi usare Microsoft Active Directory per gestire gli account utente a cui è possibile accedere da qualsiasi computer nel dominio. In questo modo è possibile creare e accedere a singoli account utente di dominio da qualsiasi stazione in qualsiasi sistema MultiPoint Services aggiunto al dominio.  
 
Quando si distribuiscono servizi MultiPoint in un ambiente di dominio, è necessario considerare diversi fattori:  
  
-   Se si usano account di dominio, non possono essere gestiti da Gestione MultiPoint.  
  
-   Per impostazione predefinita, MultiPoint Services è configurato per concedere a ogni utente l'autorizzazione per accedere a una sola stazione alla volta. Se si decide di consentire agli utenti di accedere a più stazioni contemporaneamente usando un singolo account, è possibile usare l'opzione **Modifica impostazioni server** in Gestione MultiPoint.  
  
-   La posizione dei controller di dominio può influenzare la velocità e l'affidabilità con cui gli utenti saranno in grado di eseguire l'autenticazione con il dominio e di individuare le risorse.  
  
## <a name="single-user-account-for-multiple-stations"></a>Account utente singolo per più stazioni  
MultiPoint Services è in grado di accedere a più stazioni nello stesso computer contemporaneamente usando un singolo account utente. Questa funzionalità è utile negli ambienti in cui agli utenti non sono assegnati nomi utente univoci e in cui l'uso di un singolo account utente può semplificare la gestione del sistema MultiPoint Services.  
  
