---
title: Risolvere i problemi di installazione di Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ecf19216-7aac-4aca-839a-342ac28f5329
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 293b392203269a65efffcefb3744bedc659f71c9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-windows-server-essentials-installation"></a>Risolvere i problemi di installazione di Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Questo argomento fornisce informazioni sulla risoluzione dei problemi che si verificano durante l'installazione di Windows Server Essentials. Vengono fornite istruzioni nelle aree seguenti:  
  

-   [Procedure di risoluzione dei problemi generali](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [Risolvere i problemi di driver](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

-   [Procedure di risoluzione dei problemi generali](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [Risolvere i problemi di driver](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

  
> [!NOTE]
>  Per informazioni aggiornate sulla risoluzione dei problemi dalla community Windows Server Essentials, si consiglia di visitare il [Forum di Windows Server Essentials](https://social.technet.microsoft.com/Forums/winserveressentials/threads). Forum di Windows Server Essentials è un ottimo punto per cercare informazioni o per porre una domanda.  
  
##  <a name="BKMK_GeneralTroubleshootingSteps"></a>Procedure di risoluzione dei problemi generali  
 Se l'installazione di Windows Server Essentials non riesce, eseguire la procedura seguente per identificare il problema che ha causato l'errore.  
  
> [!IMPORTANT]
>  È importante che è non riavviare manualmente il server durante l'installazione di Windows Server Essentials. Il server viene riavviato automaticamente diverse volte durante l'installazione e configurazione iniziale. Se il server è stato riavviato manualmente prima che si è visto il **corretta installazione del Server** messaggio, che potrebbe avere interrotto l'installazione e ha causato l'errore.  
  
#### <a name="to-identify-issues-in-a-failed-installation-of-windows-server-essentials"></a>Per identificare i problemi in un'installazione non riuscita di Windows Server Essentials  
  
1.  Verificare che l'hardware del server soddisfi i requisiti minimi. Per informazioni sui requisiti hardware, vedere [requisiti di sistema per Windows Server Essentials](../get-started/system-requirements.md).  
  
2.  Se hai ricevuto il DVD di installazione di Windows Server Essentials da MSDN, verificare che sia valido controllando la somma di SHA1. Per ulteriori informazioni, vedere [disponibilità e descrizione dell'utilità File Checksum Integrity Verifier](https://go.microsoft.com/fwlink/?LinkId=220495) (https://go.microsoft.com/fwlink/?LinkId=220495).  
  
3.  Verificare che la scheda di rete nel server è connesso a un router tramite un cavo di rete.  
  
4.  Se il server ha più di una scheda di rete, verificare che solo una scheda di rete sia abilitata.  
  
    > [!IMPORTANT]
    >  Non disconnettere il cavo di rete o riavviare il router durante l'installazione di Windows Server Essentials.  
  
5.  Vedere "installazione del Server e distribuzione" in [versione documentazione per Windows Server Essentials](../get-started/release-notes.md)  
  
6.  Se viene visualizzato il messaggio di errore Errore durante la configurazione del server durante l'installazione, utilizzare il DVD di ripristino del Server e le istruzioni fornite dal produttore dell'hardware per ripristinare il server per le impostazioni predefinite.  
  
##  <a name="BKMK_TroubleshootDrivers"></a>Risolvere i problemi di driver  
 Il problema più comune quando si installa Windows Server Essentials è controller di archiviazione che è necessario installare i driver manualmente. Windows include i driver per molti controller di archiviazione, ma potrebbe non includere i driver per l'hardware specifico.  
  
 Potresti anche dover installare manualmente i driver della scheda di rete per l'hardware specifico.  
  
###  <a name="BKMK_StorageDrivers"></a>Aggiunta di driver per i controller di archiviazione  
 Se l'hardware richiede driver di archiviazione che non sono inclusi in Windows Server Essentials, è possibile utilizzare le informazioni seguenti per completare l'installazione.  
  
 Se viene visualizzato il messaggio seguente durante l'installazione, è necessario aggiungere manualmente i driver per il controller di archiviazione:  
  
 **Errore del programma di installazione di Windows Server**  
  
 Unità disco rigido in grado di ospitare Windows Server Essentials non trovato. Si desidera caricare driver di archiviazione aggiuntivi?  
  
 Utilizzare la procedura seguente per installare un driver di controller di archiviazione.  
  
##### <a name="to-manually-install-a-storage-controller-driver"></a>Per installare manualmente un driver di controller di archiviazione  
  
1.  Trovare i driver per il controller di archiviazione. Questi vengono forniti dal produttore dell'hardware e potrebbero essere disponibili nel sito Web del produttore.  
  
2.  Creare una cartella denominata DRIVERS su un disco floppy o un'unità flash USB e quindi copiare i driver nella cartella.  
  
3.  Collegare l'unità floppy o un'unità flash USB con i driver nel computer  
  
4.  Avviare il computer dal DVD di Windows Server Essentials.  
  
     Se mancano i driver di controller di archiviazione, viene visualizzata la finestra di dialogo Errore di installazione di Windows Server Essentials.  
  
5.  Nella finestra di dialogo Errore di installazione di Windows Server Essentials, fare clic su **Sì** per caricare il driver di archiviazione aggiuntivi.  
  
6.  Nel **selezionare file inf del driver** prompt, passare al file con estensione inf nella cartella DRIVERS sul disco floppy o un'unità flash USB, selezionare il file, fare doppio clic il nome del file e quindi fare clic su **aprire**. Il driver viene caricato.  
  
    > [!NOTE]
    >  Prima di tentare di caricare il file, verificare che l'estensione di file (con estensione inf) sia in lettere minuscole. Questa operazione è tra maiuscole e minuscole e un file di driver non verrà caricato se l'estensione di file è in lettere maiuscole.  
  
7.  Al prompt, fare clic su **Sì** per rendere disponibile il driver di archiviazione durante la fase di testo in modalità di installazione.  
  
 Il programma di installazione dovrebbe continuare normalmente.  
  
###  <a name="BKMK_AddingNICdrivers"></a>Aggiunta di driver per schede di rete  
 Se una scheda di rete nel computer non è supportata da Windows Server Essentials, il server non sarà possibile la connettività di rete dopo il completamento dell'installazione e non sarà in grado di connettere i computer al server.  
  
 Al termine dell'installazione di Windows Server Essentials, l'utente viene informato se non è stato installato automaticamente un driver della scheda di rete. È inoltre possibile utilizzare **connessioni di rete** nel Pannello di controllo per cercare un driver della scheda di rete mancante. Se non è presente una connessione di rete associata alla scheda di rete nel server, è necessario installare un driver.  
  
 Se nel computer manca un driver supportato per una scheda di rete, è necessario installare manualmente il driver della scheda di rete corretto e quindi riavviare il server. Utilizzare la procedura seguente.  
  
##### <a name="to-install-a-network-adapter-driver"></a>Per installare un driver della scheda di rete  
  
1.  Ottenere il driver mancante dal produttore della scheda di rete.  
  
2.  Seguire le istruzioni per installare il driver del produttore.  
  
3.  Riavviare il computer.  
  
    > [!IMPORTANT]
    >  Se non si riavvia il server dopo aver installato il driver della scheda di rete mancante, l'installazione del software connettore Windows Server Essentials nei computer client potrebbe non riuscire.
