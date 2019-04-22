---
title: Preparare la migrazione a MultiPoint Services
description: Descrive le informazioni da raccogliere prima della migrazione ai servizi MultiPoint in Windows Server 2016
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3060c531-98a2-4957-a02c-be273f25f493
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 9650ebcae7e6207a226617d401d892049405901f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824382"
---
# <a name="prepare-to-migrate-to-multipoint-services-in-windows-server-2016"></a>Preparare la migrazione ai servizi MultiPoint in Windows Server 2016

>Si applica a: Windows Server 2016

Usare le informazioni seguenti per raccogliere le informazioni necessarie per eseguire la migrazione di servizio di ruolo di servizi MultiPoint da un server di origine che esegue una versione precedente di Windows Server 2016 a un server di destinazione che esegue Windows Server 2016 RTM.

Come minimo, è necessario essere un membro del gruppo Administrators nel server di origine e il server di destinazione per installare, rimuovere o impostare i servizi MultiPoint.

>[!NOTE]
> I passaggi indicati qui non forniscono materiale sussidiario per la migrazione dei dati salvati nella cartella utente o le cartelle condivise. Assicurarsi che gli utenti il backup dei dati prima di iniziare la migrazione.

Utilizzare Gestione MultiPoint per recuperare le informazioni necessarie per la migrazione. È necessario utilizzare MultiPoint Manager le autorizzazioni di amministratore di server.

Registrare le impostazioni di server, utente e ambiente MultiPoint nel [foglio di lavoro di migrazione dei dati raccolta](multipoint-services-migration-worksheet.md). Utilizzare la procedura seguente per raccogliere tali informazioni.

## <a name="multipoint-server-settings-for-the-local-server"></a>Impostazioni di multiPoint Server per il server locale
1. Avviare Gestione MultiPoint.
2. Nel **casa** scheda, selezionare il server locale e quindi fare clic su **modificare le impostazioni del server.**
3. Registrare le impostazioni nel foglio di lavoro dei dati.
4. Chiudere la finestra di dialogo.

## <a name="managed-servers-and-computers"></a>I computer e i server gestiti

È possibile trovare i nomi dei server gestiti e computer nel **Home** scheda selezionare Gestione MultiPoint.

## <a name="station-settings"></a>Impostazioni della stazione
Se l'orientamento di visualizzazione o l'accesso automatico è configurata per la stazione, usare la procedura seguente per recuperare tali informazioni. In caso contrario, è possibile ignorare questo passaggio.

Per recuperare le impostazioni di espansione:

1. Andare alla **stazioni** scheda selezionare Gestione MultiPoint.
2. Trovare una stazione con "yes" nel **l'accesso automatico** colonna.
3. Selezionare tale stazione e quindi fare clic su **Configura stazione**.
4. Registrare l'utente che viene usato per l'accesso automatico.

Per recuperare le impostazioni di orientamento dello schermo, visualizzare il **le impostazioni della stazione** per ogni stazione.

## <a name="list-of-users"></a>Elenco di utenti
1. Scegliere il **utenti** scheda selezionare Gestione MultiPoint.
2. Record di **Administrator** e **MultiPoint Dashboard utente** accoutns.
3. Registrare gli utenti standard.

## <a name="vdi-template-location"></a>Percorso del modello VDI
 Se è stata abilitata la funzionalità del modello VDI, registrare il percorso del modello di VDI. Fino a quando il server di origine e destinazione sono nella stessa rete, è possibile importare il modello tramite Gestione MultiPoint.
 
## <a name="next-step"></a>Passaggio successivo
Ora possibile [eseguire la migrazione ai servizi MultiPoint](multipoint-services-migration-steps.md) nella versione RTM di Windows Server 2016.