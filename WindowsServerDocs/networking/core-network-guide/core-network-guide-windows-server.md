---
title: Guida alla rete core per Windows Server
description: In questo argomento viene fornita una panoramica della Guida alla rete Core, che consente di pianificare e distribuire i componenti di base necessari per una rete completamente funzionante e un nuovo dominio di Active Directory in una nuova foresta con Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 9b3ef3eb-4246-4e0e-8bf1-53224ca5f2f9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 63e4cf8c5bf56ef5131e835163a5fcb5dfd98b55
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="core-network-guide-for-windows-server"></a>Guida alla rete core per Windows Server

>Si applica a: Windows Server, Windows Server 2016

In questo argomento viene fornita una panoramica della Guida alla rete di Core per Windows Server&reg; 2016 e contiene le sezioni seguenti.  
  
-   [Introduzione alla rete di Windows Server Core](#bkmk_intro)  
  
-   [Guida alla rete core per Windows Server](#bkmk_core)  
  
## <a name="bkmk_intro"></a>Introduzione alla rete di Windows Server Core

Una rete core è una raccolta di hardware di rete, dispositivi, e deve software che fornisce i servizi essenziali per l'IT dell'organizzazione (IT).

Una rete core di Windows Server offre numerosi vantaggi, inclusi i seguenti.

- Protocolli di base per la connettività di rete tra computer e altri dispositivi compatibili Protocol/Internet Protocol (TCP). TCP/IP è una suite di protocolli standard per la connessione di computer e la creazione di reti. TCP/IP è software dei protocolli di rete fornito con Microsoft&reg; Windows&reg; suite di protocolli sistemi operativi che implementano e supportano il protocollo TCP/IP.

- Dynamic Host Configuration Protocol (DHCP) server automatica gli indirizzi IP. Configurazione manuale degli indirizzi IP in tutti i computer nella rete è richiede molto tempo ed è meno flessibile rispetto all'assegnazione dinamica di computer e altri dispositivi con lease di indirizzi IP da un server DHCP.

- Servizio di risoluzione dei nome Domain Name System (DNS). DNS consente agli utenti, computer, applicazioni e servizi trovare gli indirizzi IP dei computer e dispositivi nella rete utilizzando il nome di dominio completo del computer o dispositivo.

- Un insieme di strutture, ovvero uno o più domini di Active Directory che condividono il definizioni di classi e attributi (schema), le informazioni del sito e di replica (configurazione) e le funzionalità di ricerca a livello di foresta (catalogo globale).

- Un dominio radice della foresta, ovvero il primo dominio creato in una nuova foresta. I gruppi Enterprise Admins e Schema Admins, che sono gruppi amministrativi a livello di foresta, si trovano nel dominio radice della foresta. Inoltre, un dominio radice della foresta, come con altri domini, è una raccolta di computer, utente e gruppo oggetti definiti dall'amministratore in servizi di dominio Active Directory (AD DS). Tali oggetti condividono un comune directory database e criteri di sicurezza. Possono inoltre condividere relazioni di sicurezza con altri domini eventualmente aggiunti come alla crescita dell'organizzazione. Il servizio directory, inoltre, vengono archiviati i dati di directory e consente di computer autorizzati, applicazioni e utenti di accedere ai dati.

- Un database degli account utente e computer. Il servizio directory fornisce un database degli account utente centralizzata che consente di creare account utente e computer per utenti e computer che sono autorizzati a connettersi alla rete di accesso rete e risorse, ad esempio applicazioni, database, file condivisi e le cartelle e stampanti.

Una rete core consente anche di ridimensionare la rete come alla crescita dell'organizzazione e i requisiti IT cambiano. Ad esempio, con una rete core è possibile aggiungere domini, subnet IP, servizi di accesso remoto, servizi wireless e altre funzionalità e ruoli server offerti da Windows Server 2016.

## <a name="bkmk_core"></a>Guida alla rete core per Windows Server

Guida alla rete Windows Server 2016 Core vengono fornite istruzioni su come pianificare e distribuire i componenti di base necessari per una rete completamente funzionante e un nuovo Active Directory&reg; dominio in una nuova foresta. Utilizzando questa Guida, è possibile distribuire computer configurati con i componenti server di Windows seguenti:

- Il ruolo server Servizi di dominio Active Directory (AD DS)

- Il ruolo server di sistema DNS (Domain Name)

- Il ruolo server Dynamic Host Configuration Protocol (DHCP)

- Il servizio ruolo Server dei criteri di rete (NPS) del ruolo server Servizi di accesso e criteri di rete

- Il ruolo server Server Web (IIS)

- Transmission Control Protocol/Internet Protocol versione 4 (TCP/IP) connessioni nei singoli server

Questa guida è disponibile nel percorso seguente.

- Il [Guida alla rete Core](../core-network-guide/Core-Network-Guide.md) nella raccolta di documentazione tecnica di Windows Server 2016.
  


