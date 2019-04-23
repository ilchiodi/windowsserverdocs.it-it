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
ms.openlocfilehash: a905fd0c11237edd3a408998f8f71aa25a054328
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847902"
---
# <a name="core-network-guidance-for-windows-server"></a>Guida alla rete core per Windows Server

>Si applica a: Windows Server, Windows Server 2016

In questo argomento viene fornita una panoramica della Guida per la rete Core per Windows Server&reg; 2016 e include le sezioni seguenti.  
  
-   [Introduzione alla rete Core di Windows Server](#bkmk_intro)  
  
-   [Guida alla rete core per Windows Server](#bkmk_core)  
  
## <a name="bkmk_intro"></a>Introduzione alla rete Core di Windows Server

Una rete core è un insieme di componenti hardware, dispositivi e componenti software di rete che forniscono i servizi essenziali per i requisiti IT dell'organizzazione.

Una rete core basata su Windows Server offre numerosi vantaggi, inclusi i seguenti.

- Protocolli di base per la connettività di rete tra computer e altri dispositivi compatibili con TCP/IP (Transmission Control Protocol/Internet Protocol), una suite di protocolli standard per la connessione dei computer e la creazione di reti. TCP/IP è un software di protocollo di rete fornito con Microsoft&reg; Windows&reg; sistemi operativi che implementano e supportano il TCP/IP del protocollo suite.

- Assegnazione automatica degli indirizzi IP con DHCP (Dynamic Host Configuration Protocol) La configurazione manuale degli indirizzi IP in tutti i computer di una rete richiede molto tempo ed è meno flessibile dell'assegnazione dinamica di lease di indirizzi IP a computer e altri dispositivi da parte di un server DHCP.

- Servizi di risoluzione dei nomi DNS (Domain Name System). Il sistema DNS consente a utenti, computer, applicazioni e servizi di trovare gli indirizzi IP di computer e dispositivi della rete, utilizzando il nome di dominio completo del computer o dispositivo.

- Una foresta, ovvero un contenitore che include uno o più domini di Active Directory che condividono le stesse definizioni di classi e attributi (schema), le informazioni relative ai siti e alla replica (configurazione) e funzionalità di ricerca a livello di foresta (catalogo globale).

- Un dominio radice della foresta, ovvero il primo dominio creato in una nuova foresta. I gruppi Enterprise Admins e Schema Admins, che sono gruppi amministrativi a livello di foresta, si trovano nel dominio radice della foresta. Come tutti gli altri domini, un dominio radice della foresta è un insieme di oggetti computer, utente e gruppo che vengono definiti dall'amministratore in Servizi di dominio Active Directory. Tali oggetti condividono un database di directory e criteri di sicurezza comuni. Possono inoltre condividere relazioni di sicurezza con gli altri domini eventualmente aggiunti a mano a mano che l'organizzazione si espande. Il servizio directory gestisce inoltre l'archiviazione dei dati di directory e consente l'accesso a tali dati da parte di computer, applicazioni e utenti autorizzati.

- Un database di account di utenti e computer. Il servizio directory fornisce un database centralizzato di account utente che consente di creare gli account per utenti e computer autorizzati a connettersi alla rete e ad accedere alle relative risorse, quali applicazioni, database, stampanti e cartelle e file condivisi.

Una rete core può essere inoltre ridimensionata a mano a mano che l'organizzazione cresce e i requisiti IT cambiano. Ad esempio, con una rete core è possibile aggiungere domini, subnet IP, servizi di accesso remoto, servizi wireless e altre funzionalità e ruoli server offerti da Windows Server 2016.

## <a name="bkmk_core"></a>Guida alla rete core per Windows Server

La Guida alla rete di Core di Windows Server 2016 fornisce istruzioni su come pianificare e distribuire i componenti di base necessari per una rete completamente funzionante e un nuovo Active Directory&reg; dominio in una nuova foresta. Utilizzando questa guida è possibile distribuire computer configurati con i componenti server di Windows seguenti:

- Ruolo server Servizi di dominio Active Directory

- Ruolo server DNS (Domain Name System)

- Ruolo server DHCP (Dynamic Host Configuration Protocol)

- Servizio ruolo Server dei criteri di rete del ruolo server Servizi di accesso e criteri di rete

- Ruolo server Server Web (IIS)

- Connessioni TCP/IP (Transmission Control Protocol/Internet Protocol) versione 4 su singoli server

In questa guida è disponibile nel percorso seguente.

- Il [Guida alla rete di base](../core-network-guide/Core-Network-Guide.md) nella raccolta di documentazione tecnica di Windows Server 2016.
  


