---
title: Preparare la migrazione a MultiPoint Services
description: Descrive le informazioni da raccogliere prima di eseguire la migrazione a MultiPoint Services in Windows Server 2016
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 3060c531-98a2-4957-a02c-be273f25f493
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 3333570aae34f2c102c36382eeffcb5411b7dd83
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858704"
---
# <a name="prepare-to-migrate-to-multipoint-services-in-windows-server-2016"></a>Preparare la migrazione a MultiPoint Services in Windows Server 2016

>Si applica a: Windows Server 2016

Usare le informazioni seguenti per raccogliere le informazioni necessarie per eseguire la migrazione del servizio ruolo Servizi MultiPoint da un server di origine che esegue una versione precedente di Windows Server 2016 a un server di destinazione che esegue Windows Server 2016 RTM.

Come minimo, è necessario essere un membro del gruppo Administrators nel server di origine e nel server di destinazione per installare, rimuovere o configurare servizi MultiPoint.

>[!NOTE]
> Questa procedura non fornisce indicazioni per la migrazione dei dati salvati nella cartella utente o nelle cartelle condivise. Assicurarsi che gli utenti eseguono il backup dei dati prima di iniziare la migrazione.

Usare Gestione MultiPoint per recuperare le informazioni necessarie per la migrazione. Per usare Gestione MultiPoint è necessario disporre delle autorizzazioni di amministratore del server.

Registrare le impostazioni del server MultiPoint, dell'utente e dell'ambiente nel foglio di documento relativo alla [raccolta dei dati di migrazione](multipoint-services-migration-worksheet.md). Usare la procedura seguente per raccogliere tali informazioni.

## <a name="multipoint-server-settings-for-the-local-server"></a>Impostazioni del server MultiPoint per il server locale
1. Avviare Gestione MultiPoint.
2. Nella scheda **Home** selezionare il server locale e quindi fare clic su **Modifica impostazioni server.**
3. Registrare le impostazioni nel foglio di dati.
4. Chiudere la finestra impostazioni.

## <a name="managed-servers-and-computers"></a>Server e computer gestiti

È possibile trovare i nomi dei server e dei computer gestiti nella scheda **Home** in Gestione MultiPoint.

## <a name="station-settings"></a>Impostazioni stazione
Se per la stazione sono configurati gli indirizzi di accesso automatico o di visualizzazione, attenersi alla procedura seguente per recuperare tali informazioni. In caso contrario, è possibile ignorare questo passaggio.

Per recuperare le impostazioni della stazione:

1. Passare alla scheda **stazioni** in Gestione MultiPoint.
2. Trovare una stazione con "Sì" nella colonna di **accesso automatico** .
3. Selezionare la stazione, quindi fare clic su **Configura stazione**.
4. Registrare l'utente usato per l'accesso automatico.

Per recuperare le impostazioni di orientamento di visualizzazione, visualizzare le **impostazioni della stazione** per ogni stazione.

## <a name="list-of-users"></a>Elenco di utenti
1. Fare clic sulla scheda **utenti** in Gestione MultiPoint.
2. Registrare l'utente ACCOUTNS di **amministratore** e **multipoint dashboard** .
3. Registrare gli utenti standard.

## <a name="vdi-template-location"></a>Percorso del modello VDI
 Se in precedenza è stata abilitata la funzionalità del modello VDI, registrare il percorso del modello VDI. Fino a quando i server di origine e di destinazione si trovano nella stessa rete, è possibile importare il modello usando Gestione MultiPoint.
 
## <a name="next-step"></a>Passaggio successivo
A questo punto si è pronti per [eseguire la migrazione a MultiPoint Services](multipoint-services-migration-steps.md) nella versione RTM di Windows Server 2016.