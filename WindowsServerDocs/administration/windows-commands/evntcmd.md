---
title: Evntcmd
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c1aabb74-76e7-4304-95a6-50ad87e92fd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2a4a7f9e6bdf6f200b86e028617cae924571455
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439448"
---
# <a name="evntcmd"></a>Evntcmd

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura la traduzione di eventi da trap, le destinazioni trap o entrambi, in base alle informazioni contenute in un file di configurazione.   
## <a name="syntax"></a>Sintassi  
```  
evntcmd [/s <computerName>] [/v <verbosityLevel>] [/n] <FileName>  
```  
### <a name="parameters"></a>Parametri  

|      Parametro      |                                                                                                                                                            Descrizione                                                                                                                                                             |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  /s <computerName>  |                                                         Specifica il nome del computer in cui si desidera configurare la conversione di eventi da trap, le destinazioni trap o entrambi. Se non si specifica un computer, si verifica la configurazione del computer locale.                                                          |
| /v <verbosityLevel> | Specifica i tipi di stato, i messaggi vengono visualizzati come trap e le destinazioni trap configurati. Questo parametro deve essere un numero intero compreso tra 0 e 10. Se si specifica 10, vengono visualizzati tutti i tipi di messaggi, inclusi analisi messaggi e avvisi sull'eventuale configurazione di trap è stata completata. Se si specifica 0, verrà visualizzato alcun messaggio. |
|         /n          |                                                                                                           Specifica che il servizio SNMP non deve essere riavviato se il computer riceve le modifiche alla configurazione di trap.                                                                                                            |
|     <FileName>      |                                                                                     Specifica il nome, il file di configurazione che contiene informazioni sulla conversione di eventi da trap e le destinazioni trap che si desidera configurare.                                                                                     |
|         /?          |                                                                                                                                                Visualizza la guida al prompt dei comandi.                                                                                                                                                |

## <a name="remarks"></a>Note  
- Se si desidera configurare i trap ma non le destinazioni trap, è possibile creare un file di configurazione valido tramite evento a Trap, che è un'utilità grafica. Se è installato il servizio SNMP, è possibile avviare l'evento a trap digitando **evntwin** un prompt dei comandi. Dopo aver definito i trap desiderati, fare clic su Esporta per creare un file può essere utilizzato con **evntcmd**. Evento a trap consente di creare facilmente un file di configurazione e quindi utilizzare il file di configurazione con **evntcmd** al prompt dei comandi per configurare rapidamente i trap in più computer.  
- La sintassi per la configurazione di trap è come segue:  
  **#pragma add**<em><EventLogFile> <EventSource> <EventID> [<Count> [<Period>]]</em>  
  -   Il testo **#pragma** deve essere visualizzato all'inizio di ogni voce nel file.  
  -   Il parametro **aggiungere** specifica che si desidera aggiungere un evento per la configurazione trap.  
  -   I parametri *FileRegistrazioneEvento*, *EventSource*, e *EventID* sono necessari. Il parametro *FileRegistrazioneEvento* Specifica il file in cui viene registrato l'evento. Il parametro *EventSource* Specifica l'applicazione che genera l'evento. Il *EventID* parametro specifica il numero univoco che identifica ogni evento. Per individuare valori che corrispondono a determinati eventi, avviare l'evento a trap digitando **evntwin** un prompt dei comandi. Fare clic su **Custom**, quindi fare clic su **modificare**. Sotto **origini eventi**, sfogliare le cartelle fino a individuare l'evento che si desidera configurare, selezionarlo e quindi fare clic su **aggiungere**. Informazioni sull'origine dell'evento, il file di log eventi e l'ID evento vengono visualizzate in **del codice sorgente, accedere**, e **Trap ID specifico**, rispettivamente.  
  -   Il *conteggio* parametro è facoltativo e specifica quante volte l'evento deve verificarsi prima che venga inviato un messaggio trap. Se non si utilizza il *conteggio* parametro, viene inviato il messaggio trap dopo l'evento si verifica una volta.  
  -   Il *periodo* parametro è facoltativo, ma è necessario utilizzare il *conteggio* parametro. Il *periodo* parametro specifica un periodo di tempo (in secondi) durante il quale l'evento deve verificarsi il numero di volte specificato con il parametro Count prima che venga inviato un messaggio trap. Se non si utilizza il *periodo* parametro, viene inviato un messaggio trap dopo l'evento si verifica il numero di volte specificato con il *conteggio* parametro, indipendentemente da quanto tempo deve trascorrere tra le occorrenze.  
- La sintassi per la rimozione di un trap è come segue:  
  **#pragma delete**<em><EventLogFile> <EventSource> <EventID></em>  
  -   Il testo **#pragma** deve essere visualizzato all'inizio di ogni voce nel file.  
  -   Il parametro *eliminare* specifica che si desidera rimuovere un evento per la configurazione trap.  
  -   I parametri *FileRegistrazioneEvento*,  *EventSource*, e *EventID* sono necessari. Il parametro *FileRegistrazioneEvento* Specifica il registro in cui viene registrato l'evento. Il parametro *EventSource* Specifica l'applicazione che genera l'evento. Il *EventID* parametro specifica il numero univoco che identifica ogni evento.  
- La sintassi per la configurazione di una destinazione trap è come segue:  
  **#pragma add_TRAP_DEST**<em><CommunityName> <HostID></em>  
  -   Il testo **#pragma** deve essere visualizzato all'inizio di ogni voce nel file.  
  -   Il parametro **add_TRAP_DEST** specifica che si desidera messaggi trap da inviare all'host specificato all'interno di una community.  
  -   Il parametro *NomeComunità* Specifica il nome, la community in cui trap vengono inviati messaggi.  
  -   Il parametro *HostID* Specifica il nome o l'indirizzo IP dell'host che si desidera inviare i messaggi trap.  
- La sintassi per la rimozione di una destinazione trap è come segue:  
  **#pragma delete_TRAP_DEST**<em><CommunityName> <HostID></em>  
  - Il testo **#pragma** deve essere visualizzato all'inizio di ogni voce nel file.  
  - Il parametro *delete_TRAP_DEST* specifica che non si desidera messaggi trap da inviare all'host specificato all'interno di una community.  
  - Il parametro *NomeComunità* Specifica il nome, la community in cui trap vengono inviati messaggi.  
  - Il parametro *HostID* Specifica il nome o l'indirizzo IP dell'host a cui non si desidera inviare i messaggi trap.  
    ## <a name="BKMK_Examples"></a>Esempi  
    Gli esempi seguenti illustrano le voci nel file di configurazione per il **evntcmd** comando. Non sono progettate per essere digitate al prompt dei comandi.  
    Per inviare un messaggio trap se viene riavviato il servizio Registro eventi, digitare:  
    ```  
    #pragma add System "Eventlog" 2147489653  
    ```  
    Per inviare un messaggio trap se il servizio Registro eventi viene riavviato due volte in tre minuti, digitare:  
    ```  
    #pragma add System "Eventlog" 2147489653 2 180  
    ```  
    Per arrestare l'invio di un messaggio trap ogni volta che viene riavviato il servizio Registro eventi, digitare:  
    ```  
    #pragma delete System "Eventlog" 2147489653  
    ```  
    Per inviare messaggi trap all'interno della community denominata Public all'host con l'indirizzo IP 192.168.100.100, digitare:  
    ```  
    #pragma add_TRAP_DEST public 192.168.100.100  
    ```  
    Per inviare messaggi trap all'interno della Comunità privata per l'host denominato Host1, digitare:  
    ```  
    #pragma add_TRAP_DEST private Host1  
    ```  
    Per arrestare l'invio di messaggi trap all'interno della Comunità privata nello stesso computer in cui si siano configurando le destinazioni trap, digitare:  
    ```  
    #pragma delete_TRAP_DEST private localhost  
    ```  
    ## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
