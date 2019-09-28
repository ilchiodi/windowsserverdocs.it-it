---
title: Dynamic Host Configuration Protocol (DHCP)
description: Questo argomento fornisce una breve panoramica di Dynamic Host Configuration Protocol (DHCP) in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 0ff29ef3-c458-4432-9065-e50a7de5b4b9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c71517bc742cf9eda62cc7d83128f1ab9bd04547
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355403"
---
# <a name="dynamic-host-configuration-protocol-dhcp"></a>Dynamic Host Configuration Protocol (DHCP)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per una breve panoramica di DHCP in Windows Server 2016.

> [!NOTE]
> Oltre a questo argomento, è disponibile la seguente documentazione DHCP.
>
> - [Novità di DHCP](What-s-New-in-DHCP.md)
> - [Distribuire DHCP con Windows PowerShell](dhcp-deploy-wps.md)

Dynamic Host Configuration Protocol (DHCP) è un protocollo client/server che fornisce automaticamente un host IP (Internet Protocol) con l'indirizzo IP e altre informazioni di configurazione correlati, ad esempio il subnet mask e il gateway predefinito. RFC 2131 e 2132 definisce DHCP come un Internet Engineering Task Force (IETF) standard basata sul protocollo Bootstrap (BOOTP), un protocollo con cui DHCP condivide molti dettagli di implementazione. DHCP consente agli host ottenere informazioni sulla configurazione TCP/IP richieste da un server DHCP.

Windows Server 2016 include Server DHCP, che è un ruolo server di rete facoltativo che è possibile distribuire nella rete agli indirizzi IP di lease e altre informazioni ai client DHCP. Tutti i sistemi operativi basati su Windows client includono il client DHCP come parte di TCP/IP e client DHCP è abilitato per impostazione predefinita.

## <a name="why-use-dhcp"></a>Perché utilizzare DHCP?

Ogni dispositivo in una rete basata su TCP/IP deve avere un indirizzo IP unicast univoco per accedere alla rete e le relative risorse. Senza DHCP, gli indirizzi IP per i computer nuovi o spostati da una subnet a un altro è necessario configurarlo manualmente. Gli indirizzi IP per i computer che vengono rimossi dalla rete devono essere recuperati manualmente.

Con DHCP, l'intero processo viene automatizzato e gestita in modo centralizzato. Il server DHCP gestisce un pool di indirizzi IP e il lease un indirizzo a qualsiasi client DHCP abilitato all'avvio di rete. Poiché gli indirizzi IP dinamici (lease) anziché statico (assegnato in modo permanente), gli indirizzi non in uso vengono automaticamente restituiti al pool per la riassegnazione.

L'amministratore di rete stabilisce i server DHCP che gestiscono le informazioni di configurazione TCP/IP e forniscono la configurazione di indirizzo al client sotto forma di un'offerta di lease DHCP. Il server DHCP archivia le informazioni di configurazione in un database che include:

- Parametri di configurazione TCP/IP validi per tutti i client nella rete.

- Gli indirizzi IP validi, mantenuto in un pool per l'assegnazione ai client, nonché escludere gli indirizzi.

- Indirizzi IP riservati associati particolare client DHCP. In questo modo coerente con l'assegnazione di un singolo indirizzo IP di un singolo client DHCP.

- La durata del lease, o l'intervallo di tempo per cui l'indirizzo IP può essere utilizzata prima un rinnovo del lease è obbligatorio.

Un client DHCP abilitato all'accettazione di un'offerta di lease, riceve:

- Un indirizzo IP valido per la subnet a cui viene stabilita la connessione.  
  
- Richiesta opzioni DHCP, che sono parametri aggiuntivi che è configurato un server DHCP per assegnare ai client. Alcuni esempi di opzioni DHCP sono Router (gateway predefinito), i server DNS e nome di dominio DNS.

## <a name="benefits-of-dhcp"></a>Vantaggi di DHCP

DHCP offre i vantaggi seguenti.

- **Configurazione degli indirizzi IP affidabile**. DHCP riduce al minimo gli errori di configurazione causati dalla configurazione degli indirizzi IP manuale, ad esempio gli errori tipografici, o risolvere i conflitti causati dall'assegnazione di un indirizzo IP a più computer contemporaneamente.

- **Riduzione di amministrazione della rete**. DHCP include le funzionalità seguenti per ridurre l'amministrazione della rete:

    - Configurazione TCP/IP centralizzata e automatizzata.

    - La possibilità di definire le configurazioni TCP/IP da una posizione centrale.

    - La possibilità di assegnare una gamma completa di altri valori di configurazione TCP/IP tramite opzioni DHCP.

    - La gestione efficiente di modifiche all'indirizzo IP per i client che devono essere aggiornati di frequente, ad esempio quelli per i dispositivi portatili che si spostano in percorsi diversi in una rete wireless.

    - L'inoltro di messaggi DHCP iniziali tramite un agente di inoltro DHCP, che elimina la necessità di un server DHCP in ogni subnet.

