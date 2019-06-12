---
title: Pulizia guidata server
description: Argomento di Windows Server Update Service (WSUS) - come usare la procedura guidata di pulizia di Server per gestire lo spazio su disco
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c351797-2716-4442-a668-60d5b4e77751
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9d9287bb8bfc0fd51c53c598ccbc1f0498942e2f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439811"
---
# <a name="the-server-cleanup-wizard"></a>Pulizia guidata server

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La procedura guidata pulizia del Server è integrata nell'interfaccia utente e può essere usata per gestire lo spazio su disco. Questa procedura guidata è possibile eseguire le operazioni seguenti:

- Rimuovere gli aggiornamenti non utilizzati e le revisioni di aggiornamento rimuove tutti gli aggiornamenti precedenti e le revisioni di aggiornamento che non sono state approvate.

- Eliminare tutti i computer client che non hanno contattato il server di trenta giorni o più computer non contattare l'eliminazione di server.

- Elimina Elimina i file di aggiornamento non necessari che tutti aggiornare i file non necessari tramite gli aggiornamenti o i server downstream.

- Rifiutare il rifiuto di aggiornamenti scaduti tutti gli aggiornamenti scaduti da Microsoft.

- Gli aggiornamenti sostituito rifiuto rifiutare tutti gli aggiornamenti che soddisfano i criteri seguenti:

  -   L'aggiornamento sostituito non è obbligatorio

  -   L'aggiornamento sostituito è stato sul server per 30 giorni o più

  -   L'aggiornamento sostituito non attualmente segnalato in base alle esigenze da qualsiasi client

  -   L'aggiornamento sostituito non è stato esplicitamente distribuito a un gruppo di computer per 90 giorni o più

  -   L'aggiornamento sostitutivo deve essere approvato per l'installazione di un gruppo di computer

  > [!WARNING]
  >  In una gerarchia WSUS, è consigliabile eseguire il processo di pulizia nel server WSUS downstream/di replica più basso, prima di tutto e quindi spostare verso l'alto della gerarchia. Esecuzione non corretta di pulizia in qualsiasi server a monte prima di eseguire la pulizia in tutti i server downstream può causare una mancata corrispondenza tra i dati che sono presenti nel database upstream e downstream database. La mancata corrispondenza dei dati può causare errori di sincronizzazione tra il server upstream e downstream. 
  > 
  > [!IMPORTANT]
  >  Se si rimuove il contenuto non necessario tramite la pulitura guidata del Server, vengono rimossi anche tutti i file di aggiornamento privata scaricato dal sito Microsoft Update Catalog. È necessario reimportare questi file dopo aver eseguito la pulitura guidata del Server. 

Se gli aggiornamenti approvati utilizzando una regola di approvazione automatica, potrebbe essere ancora nello stato "Approvato" e non verranno rimossi dal processo di pulizia Server della procedura guidata. Per rimuovere gli aggiornamenti approvati automaticamente presenti in uno stato "approvato", l'amministrazione di WSUS deve - minimo - manualmente impostare lo stato di approvazione degli aggiornamenti sostituiti per "Non approvati" in modo che siano idonei per rifiuto per la pulizia guidata del Server. La pulizia del Server che consente di garantire un aggiornamento più recente è stata approvata e che nessun sistema client ancora segnala che aggiornare come necessario prima di contrassegnare l'aggiornamento come "Rifiutato".




