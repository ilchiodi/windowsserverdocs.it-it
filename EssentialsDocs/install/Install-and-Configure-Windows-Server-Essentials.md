---
title: Installare e configurare Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 06/17/2013
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e95cf219-46a4-4041-bd81-0c4c2a0622cf
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cdad118c30fbf303b55ec7ea25bbe3e209c016db
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="install-and-configure-windows-server-essentials"></a>Installare e configurare Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_InstallConfigure"></a>   

 Questo documento fornisce istruzioni dettagliate per l'installazione e configurazione di Windows Server Essentials. Prima di iniziare l'installazione, esaminare e completare le attività descritte in [prima di installare Windows Server Essentials](Before-You-Install-Windows-Server-Essentials.md).  

 Questo documento fornisce istruzioni dettagliate per l'installazione e configurazione di Windows Server Essentials. Prima di iniziare l'installazione, esaminare e completare le attività descritte in [prima di installare Windows Server Essentials](../install/Before-You-Install-Windows-Server-Essentials.md).  
  
 Per installare e configurare Windows Server Essentials in due passaggi:  
  

1.  [Passaggio 1: Installare il sistema operativo Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials.md#BKMK_ManualInstallation) In questo passaggio installare il sistema operativo nel server.  
  
2.  [Passaggio 2: Configurare il sistema operativo Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials.md#BKMK_Step2Configure) In questo passaggio si completa l'installazione fornendo informazioni sulla società, le impostazioni di dominio e amministratore di rete. Queste informazioni viene utilizzate per preparare il server da utilizzare.  

1.  [Passaggio 1: Installare il sistema operativo Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials.md#BKMK_ManualInstallation) In questo passaggio installare il sistema operativo nel server.  
  
2.  [Passaggio 2: Configurare il sistema operativo Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials.md#BKMK_Step2Configure) In questo passaggio si completa l'installazione fornendo informazioni sulla società, le impostazioni di dominio e amministratore di rete. Queste informazioni viene utilizzate per preparare il server da utilizzare.  

  
###  <a name="BKMK_ManualInstallation"></a>Passaggio 1: Installare il sistema operativo Windows Server Essentials  
  
> [!IMPORTANT]
>  Dopo aver installato il sistema operativo, non personalizzare il server prima di aver completato [passaggio 2: configurare il sistema operativo Windows Server Essentials](#BKMK_Step2Configure).  
  
 **Tempo previsto di completamento:** circa 30 minuti.  
  
> [!NOTE]
>  I tempi di previsto di completamento in tutta questa procedura si basano sui requisiti hardware minimi.  
  
##### <a name="to-install-the-operating-system"></a>Per installare il sistema operativo  
  
1.  Connettere il computer alla rete con un cavo di rete.  
  
    > [!IMPORTANT]
    >  Non disconnettere il computer dalla rete durante l'installazione. Questa operazione può causare l'installazione di esito negativo.  
  
2.  Accendi il computer e quindi inserire il DVD di Windows Server Essentials nell'unità DVD.  
  
     Se si esegue un'installazione automatica, connettere il supporto rimovibile (ad esempio, un floppy disk o un'unità flash USB) che contiene i file di risposte. In base al contenuto del file di risposte, non sarà possibile visualizzare alcune o delle schermate di installazione seguenti.  
  
3.  Riavviare il computer. Quando il messaggio **premere un tasto qualsiasi per avviare da CD o DVD** viene visualizzato, premere un tasto qualsiasi.  
  
    > [!NOTE]
    >  Se il computer non viene avviato dal DVD, assicurarsi che l'unità CD-ROM sia elencata per prima nella sequenza di avvio del BIOS. Per ulteriori informazioni sulla sequenza di avvio del BIOS, vedere la documentazione fornita dal produttore del computer.  
  
4.  Selezionare il **lingua** che si desidera installare, **formato ora e valuta**, e **tastiera o metodo di input**, quindi fare clic su **Avanti**.  
  
5.  Fare clic su **installa**.  
  
6.  In **immettere il codice product key**, digitare il codice product key.  
  
7.  Lettura di **condizioni di licenza**. Se si accettano tali condizioni, selezionare il **accetto le condizioni di licenza** casella di controllo, quindi fare clic su **Avanti**.  
  
    > [!NOTE]
    >  Se si sceglie di non accettare le condizioni di licenza, non continuare l'installazione.  
  
8.  In **il tipo di installazione vuoi eseguire? **, fare clic su **personalizzata: installa solo Windows (utenti esperti)**  
  
9. In **in cui si desidera installare Windows? **, selezionare il disco rigido in cui si desidera installare il sistema operativo Windows. Verificare che tutte le unità disco rigide interne siano disponibili per l'installazione.  
  
    > [!IMPORTANT]
    >   Windows Server Essentials deve essere installato come volume c:, e le dimensioni del volume devono essere di almeno 60 GB. È consigliabile creare due partizioni sul disco del sistema operativo e non utilizzare l'unità c: (partizione di sistema) per archiviare i dati aziendali.  
  
    > [!NOTE]
    >  Se non è elencato un disco rigido (ad esempio, un disco rigido SATA Serial Advanced Technology Attachment ()), è necessario caricare i driver di dispositivo per quel disco rigido. Ottenere il driver di dispositivo dal produttore e salvarlo in un supporto rimovibile (ad esempio un disco floppy o un'unità flash USB). Collegare il supporto rimovibile al computer, quindi fare clic su **carica Driver**.  
  
     Se è necessario eliminare e/o creare partizioni, vedere i passaggi seguenti:  
  
    1.  Per eliminare una partizione, selezionare la partizione, fare clic su **opzioni unità (avanzate)**, quindi fare clic su **eliminare**. Dopo aver eliminato la partizione di sistema, creare una nuova partizione utilizzando le istruzioni nel passaggio **b** o passaggio **c**.  
  
        > [!NOTE]
        >  Dopo aver fatto clic **opzioni unità (avanzate)**, che opzione non verrà più visualizzata. In tal caso, ignorare la parte del passaggio che riguarda le opzioni dell'unità.  
  
    2.  Per creare una partizione da uno spazio non partizionato, fare clic sul disco rigido che si desidera partizionare, selezionare **opzioni unità (avanzate)**, fare clic su **New**e quindi il **dimensioni** testo, digitare la dimensione della partizione da creare. Ad esempio, se si utilizza la dimensione della partizione consigliato di 120 gigabyte (GB), digitare **122880**, quindi fare clic su **applica**. Dopo aver creata la partizione, fare clic su **Avanti**. La partizione viene formattata prima che l'installazione continua.  
  
    3.  Per creare una partizione che utilizza tutto lo spazio non partizionato, fare clic sul disco rigido che si desidera partizionare, selezionare **opzioni unità (avanzate)**, fare clic su **New**, quindi fare clic su **applica** per accettare la dimensione di partizione predefinita. Dopo aver creata la partizione, fare clic su **Avanti**. La partizione viene formattata prima che l'installazione continua.  
  
        > [!IMPORTANT]
        >  Dopo aver completato questo passaggio, è possibile spostare il sistema operativo in un'altra partizione.  
  
 Durante l'installazione, i file temporanei vengono copiati in una cartella di installazione nel computer, che dura circa 30 minuti. Dopo aver installato il sistema operativo Windows Server Essentials, il computer viene riavviato. A questo punto, si è pronti a configurare il sistema operativo Windows Server Essentials.  
  
###  <a name="BKMK_Step2Configure"></a>Passaggio 2: Configurare il sistema operativo Windows Server Essentials  
  
> [!IMPORTANT]
>  Se esegue la migrazione da una versione precedente di Windows Small Business Server a Windows Server Essentials, è necessario seguire una diversa procedura. Per informazioni sulle installazioni con migrazione, vedere quanto segue:  
>   
>  -   [Eseguire la migrazione da Windows SBS 2003](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
> -   [Eseguire la migrazione da Windows SBS 2008](../migrate/Migrate-Windows-Small-Business-Server-2008-to-Windows-Server-Essentials.md)  
  
 Durante questa fase dell'installazione, viene chiesto di rispondere ad alcune domande sull'organizzazione. Queste informazioni viene utilizzate per configurare il sistema operativo.  
  
> [!IMPORTANT]
>  Prima di iniziare questo passaggio, assicurarsi che la scheda di rete locale sia connesso a un router o a uno switch acceso e funzioni correttamente.  
  
 **Tempo previsto di completamento:** circa 30 minuti  
  
##### <a name="to-configure-the-operating-system"></a>Per configurare il sistema operativo  
  
1.  Nel **verificare la data e ora impostazioni** pagina, fare clic su **data di sistema di modificare le impostazioni e ora** per selezionare le impostazioni di data, ora e fuso orario per il server. Al termine, fare clic su **Avanti**.  
  
    > [!IMPORTANT]
    >  Se si installa Windows Server Essentials in una macchina virtuale, assicurarsi di scegliere lo stesso fuso orario utilizzato dal sistema operativo host. Se le impostazioni di fuso orario sono diverse, l'installazione del server potrebbe non riuscire.  
  
2.  Nel **Scegli modalità di installazione server** pagina, effettuare una delle operazioni seguenti:  
  
    1.  Scegliere **un'installazione pulita** per impostare una nuova installazione completa del software del server Windows Server Essentials.  
  
    2.  Scegliere **migrazione del Server** per installare Windows Server Essentials e aggiungere il server a un dominio Windows esistente.  
  
3.  Nel **personalizzare il server** pagina, immettere il nome dell'organizzazione, un nome di dominio interno e il nome del server.  
  
    > [!IMPORTANT]
    >  Il nome del server deve essere un nome univoco nella rete. Dopo aver completato questo passaggio, è possibile modificare il nome del server o il nome di dominio interno.  
  
4.  Fare clic su **Avanti**.  
  
5.  Nel **fornire le informazioni sull'account amministratore** , digitare le informazioni per un nuovo account amministratore.  
  
    > [!CAUTION]
    >  Non utilizzare il nome di account di amministratore di rete amministratore o amministratore di rete. Questi nomi di account sono riservati per l'utilizzo dal sistema.  
  
6.  Nel **fornire le informazioni sull'account utente standard** pagina, digitare le informazioni per un nuovo account utente standard e quindi fare clic su **Avanti**.  
  
7.  Nel **mantenere aggiornati automaticamente il server** pagina, selezionare la modalità di ricevere gli aggiornamenti di Windows per il server e quindi fare clic su **Avanti**.  
  
8.  Il **aggiornamento e preparazione del server** pagina viene visualizzato l'avanzamento del processo di installazione. Ciò richiede tempo per completare e il computer verrà riavviato un paio di volte.  
  
9. Dopo l'ultimo server riavviare, il **il server è pronto per essere utilizzato** verrà visualizzata la pagina. Fare clic su **Chiudi**.  
  
10. Fare clic sul riquadro Dashboard nel **Start** dello schermo e quindi nel Dashboard, completare la **configura Server** attività del **Home** pagina. È consigliabile completare queste attività subito dopo l'installazione di Windows Server Essentials fine.  
  
> [!NOTE]
>  Al termine dell'installazione, accede automaticamente al server con il nuovo account amministratore aggiunto durante l'installazione. La password dell'account amministratore predefinita è impostata per la stessa password il nuovo account amministratore e quindi l'account Administrator predefinito è disabilitata.  
  
 È possibile utilizzare il server appena installato per un periodo limitato di tempo (noto come il periodo di valutazione) senza immettere un codice product key. Dopo il periodo di valutazione, è necessario immettere un codice product key per attivare il server o estendere il periodo di valutazione. È possibile estendere il periodo di valutazione un massimo di due volte. Quando si raggiunge il numero massimo di giorni consentiti per il periodo di valutazione, è necessario attivare il server con un codice product key.  
  
### <a name="customize-windows-server-essentials"></a>Personalizzare Windows Server Essentials  
 Il **Home** pagina del Dashboard di Windows Server Essentials contiene i collegamenti a **installazione** attività da effettuare immediatamente dopo l'installazione del server. Queste attività consentono di proteggere le informazioni archiviate sul server e abilitare le funzionalità disponibili in Windows Server Essentials.  
  
 Se si sceglie di non eseguire le attività, utenti potrebbero non avere accesso ad alcune funzionalità di rete. Per effettuare queste attività successivamente, tornare al Dashboard di Windows Server Essentials **Home** pagina.  
  
 Nella tabella seguente definisce gli elementi che possono essere visualizzate nell'elenco delle attività di configurazione.  
  
|Attività|Descrizione
|----------|-----------------|  
|Ottenere gli aggiornamenti per altri prodotti Microsoft|Fare clic su questa attività per accedere a un collegamento che esegue uno strumento che consente di specificare se si desidera utilizzare Microsoft Update per ottenere automaticamente gli aggiornamenti per Windows Server Essentials e altri prodotti Microsoft, come Office.  
|Aggiungere gli account utente|Fare clic su questa attività per visualizzare brevi istruzioni sull'aggiunta di account utente. Un collegamento per eseguire il **aggiungere una procedura guidata Account utente** viene fornito. Per ulteriori informazioni, vedere [aggiungere un account utente](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1).  
|Aggiungere le cartelle del server|Fare clic su questa attività per visualizzare brevi istruzioni sull'aggiunta di cartelle del server. Un collegamento per eseguire il **Aggiunta guidata cartella** viene fornito. Viene anche fornito un collegamento a un argomento online sull'utilizzo di cartelle del Server. Per ulteriori informazioni, vedere [aggiungere o spostare una cartella del server](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5). 
|Configurare il Backup di Server|Fare clic su questa attività per visualizzare brevi istruzioni sull'utilizzo di Backup del Server per proteggere i dati. Un collegamento per eseguire il **backup Server procedura guidata Configura Backup** viene fornito. Per ulteriori informazioni, vedere [impostare configurare o personalizzare un backup del server](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1). 
|Configurare accesso remoto via Internet|Fare clic su questa attività per visualizzare brevi istruzioni sulla funzionalità di accesso remoto via Internet in Windows Server Essentials. Un collegamento per il **impostazioni accesso remoto via Internet** pagina viene fornita. Per ulteriori informazioni, vedere [Gestione accesso remoto via Internet](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md). 
|Impostare le notifiche di avviso tramite posta elettronica|Fare clic su questa attività per visualizzare brevi istruzioni sulla notifica degli avvisi. Un collegamento per eseguire il **configura notifiche di posta elettronica per avvisi** viene fornito. Per ulteriori informazioni, vedere [impostare le notifiche di posta elettronica per avvisi](../manage/Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email).  
|Configurare il Server dei contenuti multimediali|Fare clic su questa attività per visualizzare brevi istruzioni sull'utilizzo di Server dei contenuti multimediali per condividere musica, video e i file di immagine. Un collegamento per il **impostazioni multimediali** pagina viene fornita. Viene anche fornito un collegamento a un argomento della Guida in linea per ulteriori informazioni su Server dei contenuti multimediali. Per ulteriori informazioni, vedere [gestire file multimediali digitali](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md). 
|Connettere i computer|Fare clic su questa attività per visualizzare brevi istruzioni su come connettere un computer di rete al server. Per ulteriori informazioni, vedere [connettere i computer al server](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).
  
## <a name="see-also"></a>Vedere anche  
  
-   [Installare Windows Server Essentials](Install-Windows-Server-Essentials.md)

