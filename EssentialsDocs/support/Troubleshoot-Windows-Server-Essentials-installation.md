---
title: Risolvere i problemi relativi all'installazione di Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ecf19216-7aac-4aca-839a-342ac28f5329
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f8f8adf97011629dbdd68622b5d6ad1ba40f7bc2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318618"
---
# <a name="troubleshoot-windows-server-essentials-installation"></a>Risolvere i problemi relativi all'installazione di Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Questo argomento descrive la risoluzione dei problemi che si verificano durante l'installazione di Windows Server Essentials. Vengono fornite istruzioni nelle aree seguenti:  
  

-   [Procedura generale per la risoluzione dei problemi](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [Risolvere i problemi relativi ai driver](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

-   [Procedura generale per la risoluzione dei problemi](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_GeneralTroubleshootingSteps)  
  
-   [Risolvere i problemi relativi ai driver](Troubleshoot-Windows-Server-Essentials-installation.md#BKMK_TroubleshootDrivers)  

  
> [!NOTE]
>  Per le informazioni più aggiornate sulla risoluzione dei problemi della community di Windows Server Essentials, è consigliabile visitare il [Forum di Windows Server Essentials](https://social.technet.microsoft.com/Forums/winserveressentials/threads). Il forum di Windows Server Essentials è ideale per cercare informazioni o per porre una domanda.  
  
##  <a name="general-troubleshooting-steps"></a><a name="BKMK_GeneralTroubleshootingSteps"></a>Procedura generale per la risoluzione dei problemi  
 Se l'installazione di Windows Server Essentials non riesce, seguire questa procedura per identificare il problema che ha causato l'errore.  
  
> [!IMPORTANT]
>  È importante non riavviare manualmente il server durante l'installazione di Windows Server Essentials. Il server viene riavviato automaticamente diverse volte durante l'installazione e la configurazione iniziale. Se il server è stato riavviato manualmente prima della visualizzazione del messaggio relativo alla **corretta installazione del server**, è possibile che l'installazione sia stata interrotta, causando l'errore.  
  
#### <a name="to-identify-issues-in-a-failed-installation-of-windows-server-essentials"></a>Per identificare i problemi in un'installazione non riuscita di Windows Server Essentials  
  
1.  Verificare che l'hardware del server soddisfi i requisiti minimi. Per informazioni sui requisiti hardware, vedere [requisiti di sistema per Windows Server Essentials](../get-started/system-requirements.md).  
  
2.  Se è stato ricevuto il DVD di installazione di Windows Server Essentials da MSDN, verificare che il DVD sia valido controllando la somma di SHA1. Per ulteriori informazioni, vedere [disponibilità e Descrizione dell'utilità File Checksum Integrity Verifier](https://go.microsoft.com/fwlink/?LinkId=220495) (https://go.microsoft.com/fwlink/?LinkId=220495).  
  
3.  Verificare che la scheda di rete del server sia connessa a un router tramite un cavo di rete.  
  
4.  Se il server ha più di una scheda di rete, verificare che sia abilitata solo una scheda di rete.  
  
    > [!IMPORTANT]
    >  Non disconnettere il cavo di rete o riavviare il router durante l'installazione di Windows Server Essentials.  
  
5.  Vedere "installazione e distribuzione del server" nella [documentazione della versione per Windows Server Essentials](../get-started/release-notes.md)  
  
6.  Se viene visualizzato un messaggio di errore durante la configurazione del server durante l'installazione, usare il DVD di ripristino del server e le istruzioni fornite dal produttore dell'hardware per ripristinare le impostazioni predefinite del server.  
  
##  <a name="troubleshoot-driver-issues"></a><a name="BKMK_TroubleshootDrivers"></a>Risolvere i problemi relativi ai driver  
 Il problema più comune quando si installa Windows Server Essentials sono i controller di archiviazione per i quali è necessario installare manualmente i driver. Windows include i driver per molti controller di archiviazione, ma potrebbe non includere i driver per l'hardware specifico.  
  
 Potrebbe anche essere necessario installare manualmente i driver della scheda di rete per l'hardware specifico.  
  
###  <a name="adding-drivers-for-storage-controllers"></a><a name="BKMK_StorageDrivers"></a>Aggiunta di driver per i controller di archiviazione  
 Se l'hardware richiede driver di archiviazione che non sono inclusi in Windows Server Essentials, usare le informazioni seguenti per completare l'installazione.  
  
 Se viene visualizzato il messaggio seguente durante l'installazione, è necessario aggiungere manualmente i driver per il controller di archiviazione:  
  
 **Errore di installazione di Windows Server**  
  
 Impossibile trovare l'unità disco rigido in grado di ospitare Windows Server Essentials. Caricare driver di archiviazione aggiuntivi?  
  
 Usare la procedura seguente per installare un driver del controller di archiviazione.  
  
##### <a name="to-manually-install-a-storage-controller-driver"></a>Per installare manualmente un driver del controller di archiviazione  
  
1. Trovare i driver per il controller di archiviazione. Questi vengono forniti dal produttore dell'hardware e potrebbero essere disponibili sul sito Web del produttore.  
  
2. Creare una cartella denominata DRIVERS su un disco floppy o un'unità flash USB e quindi copiare i driver nella cartella.  
  
3. Collegare al computer l'unità floppy o l'unità flash USB con i driver  
  
4. Avviare il computer dal DVD di Windows Server Essentials.  
  
    Se mancano alcuni driver del controller di archiviazione, viene visualizzata la finestra di dialogo errore di installazione di Windows Server Essentials.  
  
5. Nella finestra di dialogo errore di installazione di Windows Server Essentials, fare clic su **Sì** per caricare i driver di archiviazione aggiuntivi.  
  
6. Alla richiesta di **selezionare il file inf del driver**, passare al file INF nella cartella DRIVERS sul disco floppy o sull'unità flash USB, selezionare il file, fare clic con il pulsante destro del mouse sul nome del file e quindi scegliere **Apri**. Il driver viene caricato.  
  
   > [!NOTE]
   >  Prima di provare a caricare il file, verificare che l'estensione di file sia in lettere minuscole (.inf). Questa operazione fa distinzione tra maiuscole e minuscole e il file di driver non verrà caricato se l'estensione è in lettere maiuscole.  
  
7. Quando chiesto fare clic su **Sì** per rendere disponibile il driver di archiviazione durante la fase di installazione in modalità testo.  
  
   Il programma di installazione dovrebbe continuare normalmente.  
  
###  <a name="adding-drivers-for-network-adapters"></a><a name="BKMK_AddingNICdrivers"></a>Aggiunta di driver per le schede di rete  
 Se una scheda di rete nel computer non è supportata da Windows Server Essentials, il server non avrà la connettività di rete al termine dell'installazione e non sarà possibile connettere i computer al server.  
  
 Al termine dell'installazione di Windows Server Essentials, si viene informati se il driver di una scheda di rete non è stato installato automaticamente. È anche possibile usare **Connessioni di rete** nel Pannello di controllo per verificare se manca un driver per scheda di rete. Se non è presente una connessione di rete associata alla scheda di rete sul server, è necessario installare un driver.  
  
 Se nel computer manca un driver supportato per una scheda di rete, è necessario installare manualmente il driver per scheda di rete corretto e quindi riavviare il server. Usare la procedura seguente.  
  
##### <a name="to-install-a-network-adapter-driver"></a>Per installare un driver per scheda di rete  
  
1.  Ottenere il driver mancante dal produttore della scheda di rete.  
  
2.  Seguire le istruzioni di installazione del produttore per installare il driver.  
  
3.  Riavvia il computer.  
  
    > [!IMPORTANT]
    >  Se il server non viene riavviato dopo l'installazione del driver della scheda di rete mancante, l'installazione del software connettore di Windows Server Essentials nei computer client potrebbe non riuscire.
