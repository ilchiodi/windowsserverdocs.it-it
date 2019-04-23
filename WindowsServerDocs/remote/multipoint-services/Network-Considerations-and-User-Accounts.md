---
title: Considerazioni sulla rete e account utente
description: Fornisce informazioni sulla pianificazione per diversi scenari di rete e l'utente con MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef4859fc-b7ae-4827-ab9c-b1dc07ab6c16
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 9133f28d2c3b36b18a2b6bc81d238835156bf447
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880802"
---
# <a name="network-considerations-and-user-accounts"></a>Considerazioni sulla rete e account utente
Servizi multiPoint possono essere distribuiti in un'ampia gamma di ambienti di rete e può supportare gli account utente locali e account utente di dominio. In generale, gli account utente di MultiPoint Services verranno gestiti in uno degli ambienti di rete seguenti:  
  
-   Un singolo computer che esegue MultiPoint Services con gli account utente locale  
  
-   Più computer che eseguono MultiPoint servizi, ognuno con un account utente locale  
  
-   Più computer che esegue MultiPoint Services e che utilizzano account utente di dominio

Per definizione, *gli account utente locali* sono accessibili solo dal computer in cui sono stati creati. Account utente locali sono gli account utente creati in un computer specifico che esegue MultiPoint Services. Al contrario, *gli account utente di dominio* gli account utente che si trovano in un controller di dominio e siano accessibili da qualsiasi computer in cui è connesso al dominio. Quando si decide quale tipo di ambiente di rete da usare, considerare quanto segue:  
  
-   Le risorse verranno condivise tra i server?  
  
-   Gli utenti verranno passare tra i server?  
  
-   Gli utenti possono accedere i server di database che richiedono l'autenticazione?  
  
-   Gli utenti possono accedere ai server web interni che richiedono l'autenticazione?  
  
-   È disponibile un'infrastruttura di dominio Active Directory esistente posto?  
  
-   Chi userà la gestione MultiPoint console per gestire desktop utente, le anteprime di visualizzazione, aggiungere gli utenti, limitare i siti Web e così via? Questa persona gestiranno più di un server? Questa persona deve avere privilegi amministrativi sui server.  
  
Le sezioni seguenti indirizzi di gestione degli account utente in questi ambienti di rete.  
  
## <a name="single-multipoint-server-with-local-user-accounts"></a>Singolo MultiPoint Server con gli account utente locale  
Negli ambienti con un singolo computer che esegue MultiPoint Services, non è necessario disporre di una rete. Tuttavia, per sfruttare i vantaggi delle risorse Internet, i requisiti di rete potrebbero essere base come un router e una connessione a un provider di servizi Internet (ISP). Le connessioni di rete che sono associate a una scheda di rete in MultiPoint Services sono configurate, per impostazione predefinita, per ottenere un indirizzo IP e l'indirizzo del server DNS automaticamente tramite DHCP. Router Internet vengono in genere configurato come server DHCP e forniscono gli indirizzi IP privati ai computer che si connettono a essi nella rete interna. Pertanto, un singolo computer che esegue MultiPoint Services potrebbero essere in grado di connettersi all'interfaccia interna del router, ottenere informazioni sugli IP automatica e connettersi a Internet senza avere notevole impegno o la configurazione da un amministratore.  
  
Un modo comune per gestire gli utenti in questo tipo di ambiente consiste nel creare un account utente locale per ogni persona che avranno accesso al sistema. Chiunque abbia un account utente locale in tale computer possa accedere ai servizi MultiPoint da qualsiasi stazione associato con il sistema. Account utente locali possono essere creati e gestiti dalla console di gestione MultiPoint.  
  
## <a name="multiple-multipoint-server-systems-with-local-user-accounts"></a>Più sistemi MultiPoint Server con gli account utente locale  
Dato che sono accessibili dal computer in cui sono stati creati, quando si distribuiscono più sistemi MultiPoint Services in un ambiente solo gli account utente locali, è possibile gestire gli account utente locale in uno dei due modi:  
  
-   È possibile creare gli account utente per specifici utenti su computer specifici che esegue MultiPoint Services.  
  
-   È possibile utilizzare Gestione MultiPoint per creare account per ogni utente in ogni computer che esegue MultiPoint Services.  
  
Ad esempio, se si intende assegnare gli utenti a un computer specifico che esegue MultiPoint Services, è possibile creare quattro account di utente locale sul Computer (user01 user02, user03 e user04) e quattro gli account utente locale nel Computer B (user05 user06, user07 e user08). In questo scenario, gli utenti 01\-04 possono accedere al Computer oggetto da qualsiasi stazione che vi è connesso; tuttavia, non accedono al Computer B. Lo stesso vale per gli utenti 05\-08, che saranno in grado di accedere solo ai Computer B, ma non per Computer A. a seconda dell'ambiente di distribuzione specifico, può essere accettabile o addirittura utili.  
  
Tuttavia, se ogni utente deve essere in grado di accedere a uno o più computer che esegue MultiPoint Services, un account utente locale deve essere creato per ogni utente in ogni computer che esegue MultiPoint Services. Scelta di gestire gli utenti in questo modo presenta alcune complessità. Ad esempio, se user01 accede al Computer un lunedì e salva un file nella cartella documenti e quindi l'utente accede al Computer B sulla Tuesday, il file che è stato salvato nella cartella documenti sul Computer A verrà non sia accessibile nel Computer B.  
  
Inoltre, se un utente dispone di account sul Computer B e il Computer, non è alcuna possibilità di sincronizzare automaticamente le password per l'account. Ciò può comportare difficoltà durante la registrazione in un computer, ma non in altra deve essere modificata la password dell'account degli utenti. È possibile semplificare la gestione degli account utente in questo tipo di ambiente di rete tramite l'assegnazione ogni utente a un singolo computer che esegue MultiPoint Services. In questo modo, l'utente può accedere a qualsiasi le stazioni sono associati a tale computer e accedere ai file appropriati.  
  
## <a name="multiple-multipoint-services-systems-with-domain-accounts"></a>Più sistemi MultiPoint Services con gli account di dominio  
Ambienti di dominio sono comuni in ambienti di rete di grandi dimensioni che includono più server. Ad esempio, si potrebbe aggiungere uno o più computer che eseguono il ruolo di servizi MultiPoint a un dominio e quindi usare Microsoft Active Directory per gestire gli account utente che è possibile accedere da qualsiasi computer nel dominio. In questo modo singoli account utente di dominio creato e di accedervi da qualsiasi postazione in qualsiasi sistema MultiPoint Services che viene aggiunto al dominio.  
 
Quando si distribuiscono servizi MultiPoint in un ambiente di dominio, vi sono molti i fattori da considerare:  
  
-   Se si usano gli account di dominio, non possono essere gestiti dalla console di gestione MultiPoint.  
  
-   Per impostazione predefinita, MultiPoint Services è configurato per assegnare ogni utente l'autorizzazione per accedere a un solo stazione alla volta. Se si decide di consentire agli utenti di accedere a più stazioni allo stesso tempo usando un singolo account, è possibile usare la **modificare le impostazioni del Server** opzione selezionare Gestione MultiPoint.  
  
-   La posizione dei controller di dominio può influire sulla velocità e affidabilità con cui gli utenti saranno in grado di eseguire l'autenticazione con il dominio e individuare le risorse.  
  
## <a name="single-user-account-for-multiple-stations"></a>Account utente singolo per più stazioni  
Servizi multiPoint è in grado di accedere a più stazioni nello stesso computer contemporaneamente usando un singolo account utente. Questa funzionalità è utile in ambienti in cui gli utenti non vengono assegnati i nomi utente univoci e con un singolo account utente può semplificare la gestione del sistema MultiPoint Services.  
  
