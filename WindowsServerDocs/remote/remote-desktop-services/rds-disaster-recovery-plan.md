---
title: Creare un piano di ripristino di emergenza
description: Informazioni su come creare un piano di ripristino di emergenza per la distribuzione di servizi desktop remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 05/05/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 8ad759a73e4a0ce1dc28f2b8e8d80f4365895430
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879502"
---
# <a name="create-your-disaster-recovery-plan-for-rds"></a>Creare il piano di ripristino di emergenza per Servizi Desktop remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile creare un piano di ripristino di emergenza in Azure Site Recovery per automatizzare il processo di failover. Aggiungere tutte le VM del componente Servizi Desktop remoto per il piano di ripristino.

Usare la procedura seguente in Azure per creare il piano di ripristino:

1. Aprire l'insieme di credenziali di Azure Site Recovery nel portale di Azure e quindi fare clic su **piani di ripristino**.
2. Fare clic su **Create** e immettere un nome per il piano.
3. Selezionare i **origine** e **destinazione**. La destinazione è un sito di servizi desktop remoto secondario o Azure.
4. Selezionare le macchine virtuali che ospitano i componenti Servizi Desktop remoto e quindi fare clic su **OK**.

Le sezioni seguenti forniscono informazioni aggiuntive sulla creazione di piani di ripristino per i diversi tipi di distribuzione di servizi desktop remoto.

## <a name="sessions-based-rds-deployment"></a>Distribuzione basata su sessioni di servizi desktop remoto

Per una distribuzione basata su sessioni di servizi desktop remoto, raggruppare le macchine virtuali in modo che si verificano in sequenza:

1. Gruppo di failover 1: macchina virtuale Host sessione
2. Gruppo di failover 2 - connessione macchina virtuale di Service Broker
3. Gruppo di failover 3: macchina virtuale l'accesso Web

Il piano avrà un aspetto simile al seguente: 

![Un piano di ripristino di emergenza per una distribuzione di servizi desktop remoto basato sulla sessione](media/rds-asr-session-drplan.png)

## <a name="pooled-desktops-rds-deployment"></a>Distribuzione di servizi desktop remoto in pool di desktop

Per una distribuzione di servizi desktop remoto con desktop in pool, raggruppare le macchine virtuali in modo che si verificano in sequenza, l'aggiunta di passaggi manuali e script.

1. Gruppo di failover 1: macchina virtuale di Gestore connessione Servizi Desktop remoto
2. Gruppo 1 azione manuale - aggiornamento DNS

   Eseguire PowerShell con privilegi elevati nella macchina virtuale di Gestore connessione. Eseguire il comando seguente e attendere un paio di minuti per assicurarsi che il DNS viene aggiornato con il nuovo valore:

   ```
   ipconfig /registerdns
   ```
3. Raggruppare 1 script: aggiungere gli host di virtualizzazione

   Modificare lo script seguente per eseguire per ogni host di virtualizzazione nel cloud. In genere dopo aver aggiunto un host di virtualizzazione per un gestore di connessione, è necessario riavviare l'host. Assicurarsi che l'host non dispone di un riavvio in sospeso prima dell'esecuzione di script, altrimenti viene restituito un errore.

   ```
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. Gruppo di failover 2: modello di macchina virtuale
5. Gruppo 2 dello script 1 - Turn off modello VM
   
   Verrà avviato il modello di macchina virtuale quando recuperato nel sito secondario, ma è una macchina virtuale con Sysprep e non è possibile avviare completamente. Servizi Desktop remoto richiede inoltre che la macchina virtuale sia shutdown per creare una configurazione di macchine Virtuali in pool da quest'ultimo. Pertanto, è necessario disattivare tale funzionalità. Se si dispone di un singolo server VMM, il nome della macchina virtuale del modello è lo stesso per il database primario e secondario. Per questo motivo, utilizziamo l'ID di macchina virtuale come specificato dalle *contesto* variabili nello script seguente. Se si dispone di più modelli, disattivarle tutte.

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. Gruppo 2 script 2: rimuovere le macchine virtuali in pool esistenti

   È necessario rimuovere le macchine virtuali in pool nel sito primario da Service Broker connessione in modo che le nuove macchine virtuali possono essere create nel sito secondario. In questo caso è necessario specificare l'host esatto in cui creare la macchina virtuale in pool. Si noti che questa operazione eliminerà le macchine virtuali dal solo della raccolta.

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName Win8Desktops; 
   Foreach($vm in $desktops){
      Remove-RDVirtualDesktopFromCollection -CollectionName Win8Desktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   ```
7. Azione manuale gruppo 2 - assegna nuovo modello

   È necessario assegnare il nuovo modello per il gestore di connessione per la raccolta in modo che è possibile creare nuove macchine virtuali in pool nel sito di ripristino. Vai al Gestore connessione Servizi Desktop remoto e identificare la raccolta. Modificare le proprietà e specificare una nuova immagine di macchina virtuale come modello.
8. Gruppo 2 script 3 - ricreare le macchine virtuali in tutti i pool

   Ricreare le macchine virtuali in pool nel sito di ripristino tramite il gestore di connessione. In questo caso, è necessario specificare l'host esatto in cui creare la macchina virtuale in pool.

   Nome della macchina virtuale in pool deve essere univoco, usando il prefisso e suffisso. Se esiste già il nome VM, lo script avrà esito negativo. Inoltre, se lato primario macchine virtuali sono numerate da 1 a 5, la numerazione di site recovery continua da 6.

   ```powershell
   ipmo RemoteDesktop; 
   Add-RDVirtualDesktopToCollection -CollectionName Win8Desktops -VirtualDesktopAllocation @{"RDVH1.contoso.com" = 1} 
   ```
9. Gruppo di failover 3: macchina virtuale del server Gateway e accesso Web

Il piano di ripristino avrà un aspetto simile al seguente:

![Un piano di ripristino di emergenza per una distribuzione di servizi desktop remoto con desktop in pool](media/rds-asr-pooled-drplan.png)

## <a name="personal-desktops-rds-deployment"></a>Distribuzione di servizi desktop remoto di desktop personali

Per una distribuzione di servizi desktop remoto con desktop personali, raggruppare le macchine virtuali in modo che si verificano in sequenza, l'aggiunta di passaggi manuali e script.

1. Gruppo di failover 1: macchina virtuale di Gestore connessione Servizi Desktop remoto
2. Gruppo 1 azione manuale - aggiornamento DNS

   Eseguire PowerShell con privilegi elevati nella macchina virtuale di Gestore connessione. Eseguire il comando seguente e attendere un paio di minuti per assicurarsi che il DNS viene aggiornato con il nuovo valore:

   ```
   ipconfig /registerdns
   ```
3. Raggruppare 1 script: aggiungere gli host di virtualizzazione
      
   Modificare lo script seguente per eseguire per ogni host di virtualizzazione nel cloud. In genere dopo aver aggiunto un host di virtualizzazione per un gestore di connessione, è necessario riavviare l'host. Assicurarsi che l'host non dispone di un riavvio in sospeso prima dell'esecuzione di script, altrimenti viene restituito un errore.

   ```powershell
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. Gruppo di failover 2: modello di macchina virtuale
5. Gruppo 2 dello script 1 - Turn off modello di macchina virtuale
   
   Verrà avviato il modello di macchina virtuale quando recuperato nel sito secondario, ma è una macchina virtuale con Sysprep e non è possibile avviare completamente. Servizi Desktop remoto richiede inoltre che la macchina virtuale sia shutdown per creare una configurazione di macchine Virtuali in pool da quest'ultimo. Pertanto, è necessario disattivare tale funzionalità. Se si dispone di un singolo server VMM, il nome della macchina virtuale del modello è lo stesso per il database primario e secondario. Per questo motivo, utilizziamo l'ID di macchina virtuale come specificato dalle *contesto* variabili nello script seguente. Se si dispone di più modelli, disattivarle tutte.

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. Gruppo di failover 3 - macchine virtuali personali
7. Gruppo 3 dello script 1: rimuovere le macchine virtuali personale esistenti e aggiungerli

   Rimuovere le macchine virtuali personale nel sito primario da Service Broker connessione in modo che le nuove macchine virtuali possono essere create nel sito secondario. È necessario per estrarre le assegnazioni delle VM e aggiungere nuovamente le macchine virtuali per il gestore di connessione con l'hash delle assegnazioni. Verrà solo rimuovere le macchine virtuali personale dall'insieme e aggiungerli di nuovo. L'allocazione di desktop personale sarà possibile esportare e importare nuovamente nella raccolta.

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName CEODesktops; 
   Export-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com 

   Foreach($vm in $desktops){
     Remove-RDVirtualDesktopFromCollection -CollectionName CEODesktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   
   Import-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com 
   ```
8. Gruppo di failover 3: macchina virtuale del server Gateway e accesso Web

Il piano avrà un aspetto simile al seguente: 

![Un piano di ripristino di emergenza per una distribuzione di servizi desktop remoto di desktop personali](media/rds-asr-personal-desktops-drplan.png)
