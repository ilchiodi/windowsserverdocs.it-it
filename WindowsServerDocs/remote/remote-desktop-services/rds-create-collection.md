---
title: Creare una raccolta di Servizi Desktop remoto
description: Informazioni su come aggiungere e programmi host sessione Desktop remoto e RemoteApp per la distribuzione di servizi desktop remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/08/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae9767e3-864a-4eb2-96c0-626759ce6d60
author: lizap
manager: dongill
ms.openlocfilehash: 52806e28c4ef87453995728623efe2954a76dfd9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839242"
---
# <a name="create-a-remote-desktop-services-collection-for-desktops-and-apps-to-run"></a>Creare una raccolta di Servizi Desktop remoto per desktop e App per l'esecuzione

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Usare la procedura seguente per creare un insieme di sessioni di Servizi Desktop remoto. Un insieme di sessioni contiene le App e si desidera rendere disponibili agli utenti i desktop. Dopo aver creato la raccolta, è possibile pubblicarlo in modo che gli utenti possano accedervi.

Prima di creare una raccolta, è necessario decidere quale tipo di raccolta è necessario: in pool di sessioni di desktop o le sessioni di desktop personale. 

- **Usare le sessioni di desktop in pool per la virtualizzazione basata sulla sessione**: Sfruttare la potenza di calcolo di Windows Server per fornire un ambiente di sessione a più conveniente per guidare i carichi di lavoro quotidiane degli utenti
- **Usare le sessioni di desktop personale per creare una virtual desktop infrastructure (VDI)**: Usare client Windows per fornire l'ad alte prestazioni, compatibilità tra app e la conoscenza che gli utenti hanno aspettano della propria esperienza desktop di Windows.
 
Con una sessione in pool, gli utenti più accedono a un pool condiviso di risorse, mentre con una sessione di desktop personale, gli utenti vengono assegnati i propri desktop da all'interno del pool. La sessione in pool fornisce riduzione dei costi complessiva, anche se sessioni personale consentono agli utenti di personalizzare l'esperienza desktop.

Se è necessario per le applicazioni di condivisione ospitato che sono a elevato utilizzo di grafica, è possibile combinare i desktop di sessioni personali con GPU virtualizzata RemoteFX configurato per l'accelerazione grafica. In alternativa, è possibile combinare i desktop personali sessione con la nuova funzionalità di assegnazione dispositivo discreti (DDA) a garantiscono inoltre supporto per le applicazioni ospitate che richiedono grafica accelerata. Consulta [la tecnologia di virtualizzazione della grafica è adatta a te](rds-graphics-virtualization.md) per altre informazioni.


Indipendentemente dal tipo di raccolta a che scelta, è possibile popolare tali raccolte con RemoteApp - programmi e risorse che gli utenti possono accedere da qualsiasi dispositivo supportato e lavorare con come se il programma è stato in esecuzione in locale.

## <a name="create-a-pooled-desktop-session-collection"></a>Creare un insieme di sessioni di desktop in pool

1.  In Server Manager, fare clic su **Servizi Desktop remoto > raccolte > attività > creare insiemi di sessioni**.  
2.  Immettere un nome per la raccolta, ad esempio **ContosoAps**.  
3.  Selezionare il server Host sessione Desktop remoto creato (ad esempio, Contoso-Shr1).  
4.  Accettare il valore predefinito **gruppi di utenti**.  
5.  Immettere il percorso della condivisione file creata per i dischi dei profili utente per questa raccolta (ad esempio, **\Contoso-Cb1\UserDisksr**).   
6.  Fare clic su **Crea**. Quando viene creata la raccolta, fare clic su **Chiudi**.  


## <a name="create-a-personal-desktop-session-collection"></a>Creare un insieme di sessioni di desktop personali

Usare il cmdlet New-RDSessionCollection per creare un insieme di desktop personali della sessione. I tre parametri seguenti forniscono le informazioni di configurazione necessarie per i desktop di sessioni personali:

- **-PersonalUnmanaged** -specifica il tipo di insieme di sessioni che consente di assegnare gli utenti a un server host sessione personali. Se non si specifica questo parametro, viene creata la raccolta come una raccolta di Host sessione Desktop remoto tradizionali, in cui gli utenti vengono assegnati quando effettuano l'accesso all'host sessione disponibile successivo.
- **-GrantAdministrativePrivilege** : se si usa **- PersonalUnmanaged**, specifica che l'utente assegnato all'host della sessione da ha privilegi amministrativi. Se non si usa questo parametro, gli utenti vengono concessi solo i privilegi di utente standard.
- **-AutoAssignUser** : se si usa **- PersonalUnmanaged**, specifica i nuovi utenti che si connettono tramite il Gestore connessione desktop remoto vengono assegnati automaticamente a un host di sessione non assegnati. Se non sono presenti host sessione non assegnati nella raccolta, l'utente verrà visualizzato un messaggio di errore. Se non si usa questo parametro, devi [assegnare manualmente gli utenti a un host sessione](rds-manage-personal-collection.md#manually-assign-a-user-to-a-personal-session-host) prima che effettuano l'accesso.

È possibile usare i cmdlet di PowerShell per gestire gli insiemi di sessioni di desktop personali. Visualizzare [gestire gli insiemi di sessioni di desktop personali](rds-manage-personal-collection.md) per altre informazioni.

## <a name="publish-remoteapp-programs"></a>Pubblica programmi RemoteApp
Usare la procedura seguente per pubblicare l'App e alle risorse nella raccolta:

1.  In Server Manager, selezionare la nuova raccolta (**ContosoApps**).  
2.  In programmi RemoteApp, fare clic su **programmi RemoteApp pubblicare**.  
3. Selezionare i programmi che si desidera pubblicare e quindi fare clic su **Pubblica**.  
