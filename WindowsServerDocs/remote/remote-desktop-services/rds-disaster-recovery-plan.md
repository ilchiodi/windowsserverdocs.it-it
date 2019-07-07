---
title: Creare un piano di ripristino di emergenza
description: Informazioni su come creare un piano di ripristino di emergenza per la distribuzione di Servizi Desktop remoto.
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
ms.openlocfilehash: e7bfe19258662a8e334ea0476689d8e860bfc8e5
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "63743894"
---
# <a name="create-your-disaster-recovery-plan-for-rds"></a>Creare un piano di ripristino di emergenza per Servizi Desktop remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

È possibile creare un piano di ripristino di emergenza in Azure Site Recovery per automatizzare il processo di failover. Aggiungere tutte le VM del componente Servizi Desktop remoto al piano di ripristino.

Usare la procedura seguente in Azure per creare il piano di ripristino:

1. Aprire l'insieme di credenziali di Azure Site Recovery nel portale di Azure e quindi fare clic su **Piani di ripristino**.
2. Fare clic su **Crea** e immettere un nome per il piano.
3. Selezionare **Origine** e **Destinazione**. La destinazione è un sito di Servizi Desktop remoto secondario o Azure.
4. Selezionare le VM che ospitano i componenti Servizi Desktop remoto e quindi fare clic su **OK**.

Le sezioni seguenti includono informazioni aggiuntive sulla creazione di piani di ripristino per i diversi tipi di distribuzione di Servizi Desktop remoto.

## <a name="sessions-based-rds-deployment"></a>Distribuzione basata su sessioni di Servizi Desktop remoto

Per una distribuzione basata su sessioni di Servizi Desktop remoto, raggruppare le VM in modo da apparire in sequenza:

1. Gruppo di failover 1: VM Host di sessione
2. Gruppo di failover 2: VM del Gestore connessione
3. Gruppo di failover 3: VM di accesso Web

Il piano dovrebbe essere simile al seguente: 

![Piano di ripristino di emergenza per una distribuzione di Servizi Desktop remoto basata su sessioni](media/rds-asr-session-drplan.png)

## <a name="pooled-desktops-rds-deployment"></a>Distribuzione di Servizi Desktop remoto con desktop in pool

Per una distribuzione di Servizi Desktop remoto con desktop in pool, raggruppare le VM in modo da apparire in sequenza, aggiungendo passaggi manuali e script.

1. Gruppo di failover 1: VM del Gestore connessione di Servizi Desktop remoto
2. Azione manuale gruppo 1: aggiornare il DNS

   Eseguire PowerShell con privilegi elevati nella VM del Gestore connessione. Eseguire il comando seguente e attendere un paio di minuti per assicurarsi che il DNS sia aggiornato con il nuovo valore:

   ```
   ipconfig /registerdns
   ```
3. Script gruppo 1: aggiungere gli host di virtualizzazione

   Modificare lo script seguente per eseguirlo su ogni host di virtualizzazione nel cloud. In genere, dopo aver aggiunto un host di virtualizzazione per un Gestore connessione, è necessario riavviare l'host. Assicurarsi che l'host non abbia riavvii in sospeso prima di eseguire gli script, altrimenti l'esito sarà negativo.

   ```
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. Gruppo di failover 2: VM modello
5. Script 1, gruppo 2: spegnere la VM modello
   
   La VM modello si avvierà quando sarà recuperata nel sito secondario, ma è una VM con Sysprep e non può avviarsi completamente. Servizi Desktop remoto richiede inoltre che la VM sia arrestata per creare una configurazione di VM in pool a partire da essa. Pertanto, è necessario disattivarla. Se si dispone di un singolo server VMM, il nome della VM modello è lo stesso nel database primario e secondario. Per questo motivo si usa l'ID della VM come specificato dalla variabile *Contesto* nello script seguente. Se si dispone di più modelli, disattivarli tutti.

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. Gruppo 2, script 2: rimuovere le macchine virtuali in pool esistenti

   È necessario rimuovere le macchine virtuali in pool nel sito primario da Gestore connessione in modo da poter creare le nuove macchine virtuali nel sito secondario. In questo caso, è necessario specificare l'host esatto in cui creare la VM in pool. Si noti che questa operazione eliminerà le VM solo dalla raccolta.

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName Win8Desktops; 
   Foreach($vm in $desktops){
      Remove-RDVirtualDesktopFromCollection -CollectionName Win8Desktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   ```
7. Azione manuale gruppo 2: assegnare nuovo modello

   È necessario assegnare il nuovo modello al Gestore connessione per la raccolta in modo da poter creare nuove macchine virtuali in pool nel sito di ripristino. Passare al Gestore connessione di Servizi Desktop remoto e identificare la raccolta. Modificare le proprietà e specificare una nuova immagine della VM come modello.
8. Gruppo 2, script 3: ricreare tutte le macchine virtuali in pool

   Ricreare le macchine virtuali in pool nel sito di ripristino tramite il Gestore connessione. In questo caso, è necessario specificare l'host esatto in cui creare la VM in pool.

   Il nome della VM in pool deve essere univoco, usando il prefisso e suffisso. Se il nome della VM esiste già, lo script avrà esito negativo. Inoltre, se le macchine virtuali del lato primario sono numerate da 1 a 5, la numerazione del sito di ripristino sarà a partire da 6.

   ```powershell
   ipmo RemoteDesktop; 
   Add-RDVirtualDesktopToCollection -CollectionName Win8Desktops -VirtualDesktopAllocation @{"RDVH1.contoso.com" = 1} 
   ```
9. Gruppo di failover 3: macchina virtuale del server Gateway e accesso Web

Il piano di ripristino avrà un aspetto simile al seguente:

![Piano di ripristino di emergenza per una distribuzione di Servizi Desktop remoto con desktop in pool](media/rds-asr-pooled-drplan.png)

## <a name="personal-desktops-rds-deployment"></a>Distribuzione di Servizi Desktop remoto con desktop personali

Per una distribuzione di Servizi Desktop remoto con desktop personali, raggruppare le VM in modo da apparire in sequenza, aggiungendo passaggi manuali e script.

1. Gruppo di failover 1: VM del Gestore connessione di Servizi Desktop remoto
2. Azione manuale gruppo 1: aggiornare il DNS

   Eseguire PowerShell con privilegi elevati nella VM del Gestore connessione. Eseguire il comando seguente e attendere un paio di minuti per assicurarsi che il DNS sia aggiornato con il nuovo valore:

   ```
   ipconfig /registerdns
   ```
3. Script gruppo 1: aggiungere gli host di virtualizzazione
      
   Modificare lo script seguente per eseguirlo su ogni host di virtualizzazione nel cloud. In genere, dopo aver aggiunto un host di virtualizzazione per un Gestore connessione, è necessario riavviare l'host. Assicurarsi che l'host non abbia riavvii in sospeso prima di eseguire gli script, altrimenti l'esito sarà negativo.

   ```powershell
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. Gruppo di failover 2: VM modello
5. Script 1, gruppo 2: spegnere la VM modello
   
   La VM modello si avvierà quando sarà recuperata nel sito secondario, ma è una VM con Sysprep e non può avviarsi completamente. Servizi Desktop remoto richiede inoltre che la VM sia arrestata per creare una configurazione di VM in pool a partire da essa. Pertanto, è necessario disattivarla. Se si dispone di un singolo server VMM, il nome della VM modello è lo stesso nel database primario e secondario. Per questo motivo si usa l'ID della VM come specificato dalla variabile *Contesto* nello script seguente. Se si dispone di più modelli, disattivarli tutti.

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. Gruppo di failover 3: macchine virtuali personali
7. Gruppo 3, script 1: rimuovere le macchine virtuali personale esistenti e aggiungerle

   Rimuovere le macchine virtuali personali nel sito primario da Gestore connessione in modo da poter creare le nuove macchine virtuali nel sito secondario. È necessario estrarre le assegnazioni delle VM e aggiungere nuovamente le macchine virtuali al Gestore connessione con l'hash delle assegnazioni. Questo rimuoverà solo le macchine virtuali personali dalla raccolta per poi aggiungerle nuovamente. L'allocazione del desktop personale sarà esportata e importata nuovamente nella raccolta.

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

Il piano dovrebbe essere simile al seguente: 

![Piano di ripristino di emergenza per una distribuzione di Servizi Desktop remoto con desktop personali](media/rds-asr-personal-desktops-drplan.png)
