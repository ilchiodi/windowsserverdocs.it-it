---
title: Processo di distribuzione di accesso wireless
description: Questo argomento fa parte della Guida "Distribuzione basata su Password 802.1 X Authenticated Wireless Access" rete di Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2555f238-926e-4b20-9bfb-9774831062da
author: shortpatti
ms.author: pashort
ms.openlocfilehash: fcdbe796e4d530604dfec448e817eab877d34c4f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment-process"></a>Processo di distribuzione di accesso wireless

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Il processo utilizzato per distribuire accesso senza fili si verifica in tali fasi:

## <a name="stage-1--ap-deployment"></a>Fase 1: distribuzione di punto di accesso

Pianificare, distribuire e configurare i punti di accesso per la connettività dei client wireless e per l'utilizzo con criteri di rete. A seconda delle dipendenze di preferenza e rete, è possibile entrambi pre-configurare le impostazioni nei punti di accesso wireless prima di eseguirne l'installazione della rete o è possibile configurarli in modalità remota dopo l'installazione.

## <a name="stage-2--ad-ds-group-configuration"></a>Fase 2: configurazione del gruppo di Active Directory

In servizi di dominio Active Directory, è necessario creare gruppi di sicurezza di uno o più utenti wireless.

È quindi necessario identificare gli utenti sono autorizzati l'accesso wireless alla rete.

Infine, aggiungere gli utenti ai gruppi di sicurezza utenti wireless appropriati che è stato creato.

>[!NOTE]
>Per impostazione predefinita, il **autorizzazione di accesso alla rete** nelle proprietà di connessione account utente è configurata con l'impostazione **controllare l'accesso tramite criteri di rete NPS**. A meno che non si dispone di motivi specifici per modificare questa impostazione, è consigliabile mantenere il valore predefinito. Ciò consente di controllare l'accesso alla rete tramite i criteri di rete configurati in Criteri di rete.

## <a name="stage-3--group-policy-configuration"></a>Fase 3: configurazione dei criteri di gruppo

Configurare la rete Wireless \ (IEEE 802.11\) estensione di criteri di criteri di gruppo utilizzando il \(MMC\) Editor Gestione criteri di gruppo di Microsoft Management Console.

Per configurare i computer membro di domain \ utilizzando le impostazioni di criteri di rete wireless, è necessario applicare criteri di gruppo. Quando un computer prima di tutto viene aggiunto al dominio, criteri di gruppo viene applicato automaticamente. Se vengono apportate modifiche a criteri di gruppo, vengono applicate automaticamente le nuove impostazioni:

- Criteri di gruppo a intervalli di pre-determinato

- Se un utente di dominio si disconnette e poi è tornato alla rete

- Riavviando il computer client e l'accesso al dominio

È anche possibile imporre criteri di gruppo per aggiornare mentre si è connessi a un computer eseguendo il comando **gpupdate** al prompt dei comandi.

## <a name="stage-4--nps-server-configuration"></a>Fase 4: configurazione del server dei criteri di rete

Utilizzare una procedura guidata configurazione in Criteri di rete per aggiungere punti di accesso wireless come client RADIUS e per creare i criteri di rete utilizzata da NPS durante l'elaborazione delle richieste di connessione.

Quando si utilizza la procedura guidata per creare i criteri di rete, specificare il protocollo PEAP come il tipo EAP e il gruppo di sicurezza utenti wireless che è stato creato nella seconda fase.

## <a name="stage-5--deploy-wireless-clients"></a>Fase 5: distribuire client senza fili

Utilizzare i computer client per connettersi alla rete.

Per i computer membro di dominio in grado di accedere alla rete LAN, le impostazioni di configurazione senza fili vengono applicate automaticamente quando viene aggiornato criteri di gruppo.

Se è stata abilitata l'impostazione nella rete senza fili \ (IEEE 802.11\) criteri per la connessione automatica quando il computer è nel campo di trasmissione della rete wireless, il computer wireless, aggiunto Domain \ verrà automaticamente tentano di connettersi alla rete LAN wireless.

Per connettersi alla rete wireless, gli utenti devono fornire solo le credenziali utente di dominio nome e una password quando richiesto da Windows.

Per pianificare la distribuzione di accesso senza fili, vedere [pianificazione della distribuzione di accesso Wireless](d-wireless-access-planning.md).
