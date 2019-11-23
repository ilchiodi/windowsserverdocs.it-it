---
title: Processo di distribuzione dell'accesso wireless
description: Questo argomento fa parte della Guida alla rete di Windows Server 2016 "distribuire l'accesso wireless autenticato con 802.1 X basato su password"
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2555f238-926e-4b20-9bfb-9774831062da
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 10c69e1aa6157c7f088f190c0283b33b630bc25f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406261"
---
# <a name="wireless-access-deployment-process"></a>Processo di distribuzione dell'accesso wireless

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Il processo usato per distribuire l'accesso wireless si verifica in queste fasi:

## <a name="stage-1--ap-deployment"></a>Fase 1: distribuzione AP

Pianificare, distribuire e configurare i punti di accesso per la connettività dei client wireless e per l'uso con server dei criteri di rete. A seconda della preferenza e delle dipendenze di rete, è possibile pre\-configurare le impostazioni nei punti di accesso wireless prima di installarli nella rete oppure è possibile configurarli in modalità remota dopo l'installazione.

## <a name="stage-2--adds-group-configuration"></a>Fase 2: configurazione del gruppo di servizi di dominio Active Directory

In servizi di dominio Active Directory è necessario creare uno o più gruppi di sicurezza per gli utenti wireless.

Identificare quindi gli utenti a cui è consentito l'accesso wireless alla rete.

Infine, aggiungere gli utenti ai gruppi di sicurezza degli utenti wireless appropriati creati.

>[!NOTE]
>Per impostazione predefinita, l'impostazione delle **autorizzazioni di accesso alla rete** nelle proprietà di connessione dell'account utente viene configurata con l'impostazione **Controlla accesso tramite criteri di rete NPS**. A meno che non esistano motivi specifici per modificare questa impostazione, è consigliabile mantenerne il valore predefinito. Ciò consente di controllare l'accesso alla rete tramite i criteri di rete configurati nel server dei criteri di rete.

## <a name="stage-3--group-policy-configuration"></a>Fase 3: configurazione di Criteri di gruppo

Configurare la rete wireless \(IEEE 802,11\) criteri estensione di Criteri di gruppo utilizzando la Editor Gestione Criteri di gruppo Microsoft Management Console \(MMC\).

Per configurare i computer membri del\-di dominio utilizzando le impostazioni nei criteri di rete wireless, è necessario applicare Criteri di gruppo. Quando un computer viene aggiunto al dominio per la prima volta, Criteri di gruppo viene applicato automaticamente. Se vengono apportate modifiche ai Criteri di gruppo, le nuove impostazioni vengono applicate automaticamente:

- Per Criteri di gruppo a intervalli prestabiliti\-

- Se un utente di dominio si disconnette e quindi torna alla rete

- Riavviando il computer client ed effettuando l'accesso al dominio

È anche possibile forzare l'aggiornamento Criteri di gruppo durante l'accesso a un computer eseguendo il comando **gpupdate** al prompt dei comandi.

## <a name="stage-4--nps-configuration"></a>Fase 4: configurazione di NPS

Utilizzare una configurazione guidata in NPS per aggiungere punti di accesso wireless come client RADIUS e per creare i criteri di rete utilizzati da server dei criteri di rete per l'elaborazione delle richieste di connessione.

Quando si utilizza la procedura guidata per creare i criteri di rete, specificare PEAP come tipo EAP e il gruppo di sicurezza utenti wireless creato nella seconda fase.

## <a name="stage-5--deploy-wireless-clients"></a>Fase 5: distribuire client wireless

Usare i computer client per connettersi alla rete.

Per i computer membri del dominio che possono accedere alla LAN cablata, le impostazioni di configurazione wireless necessarie vengono applicate automaticamente quando Criteri di gruppo viene aggiornato.

Se è stata abilitata l'impostazione in rete wireless \(IEEE 802,11\) criteri per la connessione automatica quando il computer si trova all'interno di un intervallo di trasmissione della rete wireless, i computer wireless, di dominio\-aggiunti verranno tentati automaticamente di connettersi alla LAN wireless.

Per connettersi alla rete wireless, gli utenti devono fornire le credenziali di nome utente e password di dominio solo quando richiesto da Windows.

Per pianificare la distribuzione dell'accesso wireless, vedere [pianificazione della distribuzione dell'accesso wireless](d-wireless-access-planning.md).
