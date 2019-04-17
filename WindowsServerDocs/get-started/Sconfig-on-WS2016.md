---
title: Configurare un'installazione Server Core di Windows Server con Sconfig.cmd
description: Descrive come usare Sconfig.cmd
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 10/17/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e6cac074-c6fc-46dd-9664-fa0342c0a5e8
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: high
ms.openlocfilehash: 2473b4ffae79c29ec7505616c139c03b21a4427b
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/22/2018
ms.locfileid: "1284350"
---
# <a name="configure-a-server-core-installation-of-windows-server-2016-or-windows-server-version-1709-with-sconfigcmd"></a>Configurare un'installazione Server Core di Windows Server con Sconfig.cmd
> Si applica a: Windows Server 2016 (Canale semestrale) e Windows Server 2016

In Windows Server 2016 e Windows Server versione 1709 puoi usare lo strumento Configurazione server (Sconfig.cmd) per configurare e gestire diversi aspetti comuni delle installazioni Server Core. Per usare questo strumento, devi essere un membro del gruppo Administrators.  
  
Puoi usare Sconfig.cmd in Server Core e Server con installazioni Esperienza desktop (solo Windows Server 2016). 
  
## <a name="start-the-server-configuration-tool"></a>Avviare lo strumento Configurazione server  
  
1.  Passa all'unità di sistema.  
  
2.  Digita `Sconfig.cmd` e quindi premi INVIO. Viene avviata l'interfaccia dello strumento Configurazione server:  
  
 <img src="mainsconfigpage.png" style='float:left; padding:.5em;' alt="Screenshot of Sconfig.cmd user interface">  
Cattura di schermata dell'interfaccia utente Sconfig.cmd  
  
##  <a name="BKMK_Domainworkgroup"></a> Impostazioni di dominio/gruppo di lavoro  
 Le impostazioni Dominio/Gruppo di lavoro correnti vengono visualizzate nella schermata predefinita dello strumento Configurazione server. Puoi aggiungere il computer a un dominio o a un gruppo di lavoro accedendo alla pagina delle impostazioni **Dominio/Gruppo di lavoro** dal menu principale e seguendo le istruzioni visualizzate nelle pagine successive, in cui dovrai fornire le informazioni richieste.  
  
 Se un utente del dominio non è stato aggiunto al gruppo Administrators locale, non sarà possibile apportare modifiche di sistema, ad esempio modificare il nome del computer, con tale utente. Per aggiungere un utente di dominio al gruppo Administrators locale, esegui il riavvio del computer. Accedi quindi al computer come amministratore locale e segui la procedura descritta nella sezione [Impostazioni degli amministratori locali](assetId:///3c2f8ca4-6adc-4ebd-8daf-eb0de16c2c7d#BKMK_Localadministratorsettings) più avanti in questo documento.  
  
> [!NOTE]
>  Per applicare le modifiche apportate all'appartenenza al dominio o al gruppo di lavoro, è necessario riavviare il server. È tuttavia possibile apportare ulteriori modifiche e riavviare il server dopo averle inserite tutte per evitare di riavviare il server più volte. Per impostazione predefinita, le macchine virtuali in esecuzione vengono automaticamente salvate prima del riavvio del server Hyper-V.  
  
## <a name="computer-name-settings"></a>Impostazioni del nome del computer  
 Il nome del computer corrente viene visualizzato nella schermata predefinita dello strumento Server Configuration. È possibile modificare il nome del computer accedendo alla pagina delle impostazioni "Computer Name" dal menu principale e seguendo le istruzioni visualizzate.  
  
> [!NOTE]
>  Per applicare le modifiche apportate all'appartenenza al dominio o al gruppo di lavoro, è necessario riavviare il server. È tuttavia possibile apportare ulteriori modifiche e riavviare il server dopo averle inserite tutte per evitare di riavviare il server più volte. Per impostazione predefinita, le macchine virtuali in esecuzione vengono automaticamente salvate prima del riavvio del server Hyper-V.  
  
##  <a name="BKMK_Localadministratorsettings"></a> Impostazioni degli amministratori locali  
 Per aggiungere altri utenti al gruppo Administrators locale, usa l'opzione **Aggiunta amministratore locale** del menu principale. Su un computer incluso in un dominio immettere l'utente nel formato seguente: dominio\nomeutente. Su un computer non appartenente a un dominio (computer del gruppo di lavoro), immettere solo il nome utente. Le modifiche avranno effetto immediatamente.  
  
## <a name="network-settings"></a>Impostazioni di rete  
 È possibile configurare l'indirizzo IP che un server DHCP deve assegnare automaticamente oppure assegnare manualmente un indirizzo IP statico. Questa opzione consente di configurare anche le impostazioni del server DNS per il server.  
  
> [!NOTE]
>  Queste e molte altre opzioni sono ora disponibili tramite i cmdlet di rete di Windows PowerShell. Per altre informazioni, vedi l'articolo sui [cmdlet degli adattatori di rete](https://technet.microsoft.com/library/jj134956.aspx) in Windows Server Library.  
  
## <a name="windows-update-settings"></a>Impostazioni relative a Windows Update  
 Le impostazioni correnti di Windows Update vengono visualizzate nella schermata predefinita dello strumento Server Configuration. Puoi configurare il server in modo che usi gli aggiornamenti automatici o manuali con l'opzione di configurazione **Impostazioni di Windows Update** del menu principale.  
  
 Se selezioni **Aggiornamenti automatici**, il sistema controlla e installa gli aggiornamenti ogni giorno alle 3.00. Le impostazioni avranno effetto immediatamente. Se selezioni **Manuale**, il sistema non verificherà automaticamente la disponibilità di aggiornamenti.  
  
 In qualsiasi momento puoi scaricare e installare gli aggiornamenti applicabili tramite l'opzione **Scarica e installa aggiornamenti** del menu principale.

 L'opzione **Solo download** rileva gli aggiornamenti, scarica quelli disponibili e quindi ti informa in Centro notifiche che sono pronti per l'installazione. Questo è l'opzione predefinita.  
  
## <a name="remote-desktop-settings"></a>Impostazioni di Desktop remoto  
 Lo stato corrente delle impostazioni di Desktop remoto viene visualizzato nella schermata predefinita dello strumento Configurazione server. Puoi configurare le impostazioni di Desktop remoto seguenti accedendo all'opzione del menu principale **Remote Desktop** e seguendo le istruzioni visualizzate.  
  
-   Abilitare Desktop remoto per i client che eseguono Desktop remoto con Autenticazione a livello di rete  
  
-   Abilitare Desktop remoto per i client che eseguono qualsiasi versione di Desktop remoto  
  
-   Disabilitare Desktop remoto  
  
## <a name="date-and-time-settings"></a>Impostazioni di data e ora  
 Puoi accedere alle impostazioni di data e ora e modificarle tramite l'opzione del menu principale **Data e ora**. 

## <a name="telemetry-settings"></a>Impostazioni di telemetria
Questa opzione consente di configurare quali dati inviare a Microsoft.

## <a name="windows-activation-settings"></a>Impostazioni di Attivazione di Windows
Questa opzione consente di configurare Attivazione di Windows.
  
## <a name="to-enable-remote-management"></a>Per abilitare la gestione remota  
Puoi abilitare vari scenari di gestione remota dall'opzione del menu principale **Configura gestione remota**:  
  
-   Gestione remota della console MMC (Microsoft Management Console)  
-   Windows PowerShell  
-   Server Manager  
  
## <a name="to-log-off-restart-or-shut-down-the-server"></a>Per disconnettersi, riavviare o chiudere il server  
 Per disconnettersi, riavviare o chiudere il server, accedere alla voce di menu corrispondente dal menu principale. Queste opzioni sono disponibili anche nel menu Sicurezza di Windows accessibile da qualsiasi applicazione in qualsiasi momento premendo CTRL+ALT+CANC.  
  
## <a name="to-exit-to-the-command-line"></a>Per uscire dalla riga di comando  
 Per uscire dalla riga di comando, seleziona l'opzione **Passaggio alla riga di comando** e premi INVIO. Per tornare allo strumento Configurazione server, digita **Sconfig.cmd** e quindi premi INVIO.