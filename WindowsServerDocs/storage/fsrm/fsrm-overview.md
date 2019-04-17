---
title: Panoramica di Gestione risorse file server
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 7/7/2017
description: "Gestione risorse file server è uno strumento che consente di gestire e classificare dati su un un file server Windows Server."
ms.openlocfilehash: ddfc0a0f4bede89a3c3a624d4f128717d0f1ac08
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="file-server-resource-manager-fsrm-overview"></a>Panoramica di Gestione risorse file server

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Gestione risorse file server è un servizio ruolo in Windows Server che consente di gestire e classificare i dati archiviati nei file server. Gestione risorse file server include le funzionalità seguenti:
  
-   **Infrastruttura di classificazione file** Infrastruttura di classificazione file offre approfondimenti sui dati automatizzando i processi di classificazione in modo da consentire una gestione più efficace dei dati. È possibile classificare i file e applicare criteri in base a questa classificazione. Criteri di esempio includono il controllo dell'accesso dinamico per limitare l'accesso ai file, la crittografia dei file e la scadenza dei file. I file possono essere classificati automaticamente tramite le regole di classificazione dei file o manualmente modificando le proprietà di una cartella o un file selezionato.  
  
-   **Attività di gestione file** Attività di gestione file consente di applicare criteri o azioni condizionali ai file in base alla relativa classificazione. Le condizioni per un'attività di gestione file includono il percorso del file, le proprietà di classificazione, la data di creazione e la data dell'ultima modifica o dell'ultimo accesso. Le azioni eseguibili da un'attività di gestione file includono l'impostazione della scadenza per i file, la crittografia dei file o l'esecuzione di un comando personalizzato.  
  
-   **Gestione quote** Le quote permettono di impostare un limite allo spazio consentito per un volume o una cartella e possono essere applicate automaticamente alle nuove cartelle create in un volume. È inoltre possibile definire i modelli di quote applicabili a nuovi volumi o nuove cartelle.  
  
-   **Gestione screening dei file** Gli screening dei file consentono di controllare i tipi di file archiviabili da un utente in un file server. È possibile limitare i file archiviabili nei file condivisi in base all'estensione. Ad esempio, è possibile creare uno screening dei file che non consenta l'archiviazione dei file con estensione MP3 nelle cartelle condivise personali in un file server.  
  
-   **Rapporti di archiviazione** I rapporti di archiviazione vengono utilizzati per facilitare l'identificazione delle tendenze a livello di utilizzo del disco e di classificazione dei dati. È inoltre possibile monitorare un gruppo selezionato di utenti per individuare i tentativi di salvataggio di file non autorizzati.  
  
 Le funzionalità incluse in Gestione risorse file server possono essere configurate e gestite tramite Microsoft Management Console (MMC) Gestione risorse file server o tramite Windows PowerShell.  
  
> [!IMPORTANT]
>  Gestione risorse file server supporta volumi formattati solo con il file system NTFS. Il file system ReFS (Resilient File System) non è supportato.  
  
## <a name="practical-applications"></a>Applicazioni pratiche  
 Alcune applicazioni pratiche di Gestione risorse file server includono:  
  
-   Utilizzare Infrastruttura di classificazione file con lo scenario Controllo dinamico degli accessi per creare criteri che concedono l'accesso a file e cartelle in base al modo in cui i file vengono classificati nel file server.  
  
-   Creare una regola di classificazione file per contrassegnare qualsiasi file che contenga almeno 10 codici fiscali come file con informazioni che consentono l'identificazione personale.  
  
-   Impostare come scaduto qualsiasi file non modificato negli ultimi 10 anni.  
  
-   Creare una quota di 200 megabyte per la home directory di ogni utente e inviare notifica quando l'utilizzo raggiunge i 180 megabyte.  
  
-   Non consentire l'archiviazione di file musicali nelle cartelle condivise personali.  
  
-   Pianificare un rapporto da eseguire ogni domenica a mezzanotte, che generi un elenco dei file utilizzati più di recente nei due giorni precedenti. Ciò può essere utile per determinare l'attività di archiviazione nei fine settimana e pianificare di conseguenza i periodi di inattività del server.  

## <a name="see-also"></a>Vedere anche

- [Novità di Gestione risorse file server](https://technet.microsoft.com/library/dn383587.aspx)
- [Controllo dinamico degli accessi](https://technet.microsoft.com/library/dn408191(v=ws.11).aspx) 