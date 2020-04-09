---
title: Pulizia guidata server
description: Argomento Windows Server Update Service (WSUS)-come usare la pulitura guidata del server per gestire lo spazio su disco
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: article
ms.assetid: 7c351797-2716-4442-a668-60d5b4e77751
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 12049e2bba28f2381e6e80db07768b4e180861d6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828535"
---
# <a name="the-server-cleanup-wizard"></a>Pulizia guidata server

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La pulizia guidata del server è integrata nell'interfaccia utente e può essere usata per semplificare la gestione dello spazio su disco. Questa procedura guidata è possibile eseguire le operazioni seguenti:

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
  >  In una gerarchia di WSUS è consigliabile eseguire prima il processo di pulizia sul server WSUS più basso, downstream/replica, quindi spostare verso l'alto la gerarchia. L'esecuzione non corretta della pulizia su un server upstream prima dell'esecuzione della pulizia in ogni server downstream può causare una mancata corrispondenza tra i dati presenti nei database upstream e nei database downstream. La mancata corrispondenza dei dati può causare errori di sincronizzazione tra i server upstream e downstream. 
  > 
  > [!IMPORTANT]
  >  Se si rimuove contenuto non necessario utilizzando la pulitura guidata del server, vengono rimossi anche tutti i file di aggiornamento privati scaricati dal sito di Microsoft Update Catalogo. È necessario reimportare questi file dopo l'esecuzione della pulitura guidata del server. 

Se gli aggiornamenti vengono approvati utilizzando una regola di approvazione automatica, potrebbero essere ancora in stato approvato e non verranno rimossi dalla pulitura guidata del server. Per rimuovere gli aggiornamenti approvati automaticamente che rientrano in uno stato approvato, l'amministratore di WSUS deve almeno impostare manualmente lo stato di approvazione degli aggiornamenti sostituiti su non approvato, in modo che siano idonei per la declinazione tramite la pulitura guidata del server. La pulizia guidata del server garantirà l'approvazione di un aggiornamento più recente e che nessun sistema client stia ancora segnalando tale aggiornamento in base alle esigenze prima di contrassegnare l'aggiornamento come rifiutato.




