---
title: Processo di distribuzione dell'accesso wireless
description: Questo argomento fa parte della Guida alla rete di Windows Server 2016 "Distribuisci con 802.1x basato su Password X Authenticated Wireless Access"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2555f238-926e-4b20-9bfb-9774831062da
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 6a286cf10e066043ee6f514bbf468bfb2b13f162
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879842"
---
# <a name="wireless-access-deployment-process"></a>Processo di distribuzione dell'accesso wireless

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Il processo che si usa per distribuire accesso senza fili avviene in queste fasi:

## <a name="stage-1--ap-deployment"></a>Fase 1: distribuzione di AP

Pianificare, distribuire e configurare i punti di accesso per la connettività client senza fili e per l'uso con criteri di rete. A seconda delle dipendenze delle preferenze e di rete, è possibile innanzitutto\-configurare le impostazioni nei punti di accesso wireless prima di eseguirne l'installazione in rete, oppure è possibile configurarle in modalità remota dopo l'installazione.

## <a name="stage-2--adds-group-configuration"></a>Nella fase 2: configurazione di Active Directory gruppo

In Active Directory Domain Services, è necessario creare uno o più utenti wireless gruppi di sicurezza.

Successivamente, identificare gli utenti sono consentiti l'accesso wireless alla rete.

Infine, aggiungere gli utenti ai gruppi di sicurezza utenti wireless appropriati che è stato creato.

>[!NOTE]
>Per impostazione predefinita, il **l'autorizzazione di accesso di rete** nelle proprietà di connessione a account utente è configurata con l'impostazione **controllare l'accesso tramite criteri di rete NPS**. A meno che non si hanno motivi specifici per modificare questa impostazione, è consigliabile mantenere il valore predefinito. In questo modo è possibile controllare l'accesso alla rete tramite i criteri di rete configurato in Criteri di rete.

## <a name="stage-3--group-policy-configuration"></a>Fase 3: configurazione dei criteri di gruppo

Configurare la rete senza fili \(IEEE 802.11\) estensione di criteri di criteri di gruppo usando Microsoft Management Console Editor Gestione criteri di gruppo \(MMC\).

Per configurare dominio\-computer membri usando le impostazioni di criteri di rete wireless, è necessario applicare criteri di gruppo. Quando un computer prima di tutto viene aggiunto al dominio, criteri di gruppo vengono applicati automaticamente. Se vengono apportate modifiche a criteri di gruppo, le nuove impostazioni vengono applicate automaticamente:

- Criteri di gruppo in pre\-determinati intervalli

- Se un utente di dominio si disconnette e quindi nuovamente alla rete

- Il riavvio del computer client e l'accesso al dominio

È anche possibile imporre criteri di gruppo per aggiornare mentre si è connessi a un computer eseguendo il comando **gpupdate** al prompt dei comandi.

## <a name="stage-4--nps-configuration"></a>Fase 4: configurazione dei criteri di rete

Per aggiungere punti di accesso wireless come client RADIUS e per creare i criteri di rete che Criteri di rete viene utilizzato durante l'elaborazione delle richieste di connessione, usare una procedura guidata configurazione in Criteri di rete.

Quando si usa la procedura guidata per creare i criteri di rete, specificare il protocollo PEAP come il tipo EAP e il gruppo di sicurezza utenti wireless che è stato creato nella seconda fase.

## <a name="stage-5--deploy-wireless-clients"></a>Fase 5: distribuire i client wireless

Usare i computer client per connettersi alla rete.

Per i computer membro di dominio in grado di accedere alla rete LAN cablata, le impostazioni di configurazione senza fili vengono applicate automaticamente quando viene aggiornato criteri di gruppo.

Se è stata abilitata l'impostazione nella rete senza fili \(IEEE 802.11\) i criteri per connettersi automaticamente quando il computer è all'interno di broadcast intervallo della rete wireless, wireless, dominio\-unita in join i computer verranno quindi tentano automaticamente di connettersi alla rete LAN wireless.

Per connettersi alla rete wireless, gli utenti devono fornire solo le credenziali utente di dominio nome e una password quando richiesto da Windows.

Per pianificare la distribuzione di accesso wireless, vedere [pianificazione della distribuzione di accesso Wireless](d-wireless-access-planning.md).
