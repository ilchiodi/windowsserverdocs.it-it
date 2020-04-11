---
title: Configurare un'installazione Server Core di Windows Server con Sconfig.cmd
description: Descrive come usare Sconfig.cmd
ms.prod: windows-server
ms.date: 10/17/2017
ms.technology: server-general
ms.topic: article
ms.assetid: e6cac074-c6fc-46dd-9664-fa0342c0a5e8
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: e6c218b08cc39edd9b3d93ae78b0b5c7aa293858
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80826674"
---
# <a name="configure-a-server-core-installation-of-windows-server-2016-or-windows-server-version-1709-with-sconfigcmd"></a>Configurare un'installazione Server Core di Windows Server 2016 o Windows Server versione 1709 con Sconfig.cmd

> Si applica a: Windows Server (Canale semestrale) e Windows Server 2016

In Windows Server 2016 e Windows Server versione 1709 puoi usare lo strumento Server Configuration (Sconfig.cmd) per configurare e gestire diversi aspetti comuni delle installazioni Server Core. Per utilizzare questo strumento, è necessario essere membri del gruppo Administrators.

Puoi usare Sconfig.cmd nelle installazioni Server Core e Server con Esperienza desktop (solo Windows Server 2016).

## <a name="start-the-server-configuration-tool"></a>Avviare lo strumento Server Configuration

1. Passare all'unità di sistema.

2. Digitare `Sconfig.cmd` e quindi premere INVIO. Viene aperta l'interfaccia dello strumento Server Configuration:

    ![Cattura di schermata dell'interfaccia utente Sconfig.cmd](media/mainsconfigpage.png)

## <a name="domainworkgroup-settings"></a>Impostazioni di dominio/gruppo di lavoro

Le impostazioni di dominio/gruppo di lavoro correnti vengono visualizzate nella schermata predefinita dello strumento Server Configuration. Per aggiungere il computer a un dominio o a un gruppo di lavoro, accedi alla pagina delle impostazioni **Dominio/Gruppo di lavoro** dal menu principale e segui le istruzioni specificando le informazioni richieste.

Se un utente di dominio non è stato aggiunto al gruppo Administrators locale, non potrai apportare modifiche di sistema, ad esempio modificare il nome del computer, usando tale utente di dominio. Per aggiungere un utente di dominio al gruppo Administrators locale, eseguire il riavvio del computer. Accedi quindi al computer come amministratore locale e segui la procedura descritta nella sezione [Impostazioni degli amministratori locali](#local-administrator-settings) più avanti in questo articolo.

> [!NOTE]
> Per applicare le modifiche apportate all'appartenenza al dominio o al gruppo di lavoro, devi riavviare il server. È tuttavia possibile apportare ulteriori modifiche e riavviare il server dopo averle inserite tutte per evitare di riavviare il server più volte. Per impostazione predefinita, le macchine virtuali in esecuzione vengono salvate automaticamente prima del riavvio del server Hyper-V.

## <a name="computer-name-settings"></a>Impostazioni del nome del computer

Il nome del computer corrente viene visualizzato nella schermata predefinita dello strumento Server Configuration. Per modificare il nome del computer, accedi alla pagina delle impostazioni **Nome computer** dal menu principale e segui le istruzioni visualizzate.

> [!NOTE]
> Per applicare le modifiche apportate all'appartenenza al dominio o al gruppo di lavoro, devi riavviare il server. È tuttavia possibile apportare ulteriori modifiche e riavviare il server dopo averle inserite tutte per evitare di riavviare il server più volte. Per impostazione predefinita, le macchine virtuali in esecuzione vengono salvate automaticamente prima del riavvio del server Hyper-V.

## <a name="local-administrator-settings"></a>Impostazioni degli amministratori locali

Per aggiungere altri utenti al gruppo Administrators locale, utilizzare l'opzione **Add Local Administrator** del menu principale. Su un computer incluso in un dominio immettere l'utente nel formato seguente: dominio\nomeutente. Su un computer non appartenente a un dominio (computer del gruppo di lavoro), immettere solo il nome utente. Le modifiche avranno effetto immediatamente.

## <a name="network-settings"></a>Impostazioni di rete

È possibile configurare l'indirizzo IP che un server DHCP deve assegnare automaticamente oppure assegnare manualmente un indirizzo IP statico. Questa opzione consente di configurare anche le impostazioni del server DNS per il server.

> [!NOTE]
> Sono ora disponibili queste e molte altre opzioni tramite i cmdlet di rete di Windows PowerShell. Per altre informazioni, vedere l'argomento relativo ai [cmdlet per la scheda di rete](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps) nella libreria di Windows Server.

## <a name="windows-update-settings"></a>Impostazioni di Windows Update

Le impostazioni correnti di Windows Update vengono visualizzate nella schermata predefinita dello strumento Server Configuration. È possibile configurare il server in modo che utilizzi gli aggiornamenti automatici o manuali con l'opzione di configurazione **Windows Update Settings** del menu principale.

Se si seleziona **Automatic Updates** , il sistema controlla e installa gli aggiornamenti ogni giorno alle 3.00. Le impostazioni avranno effetto immediatamente. Se si seleziona **Manual** , il sistema non verificherà automaticamente la disponibilità di aggiornamenti.

In qualsiasi momento, è possibile scaricare e installare gli aggiornamenti applicabili tramite l'opzione **Download and Install Updates** del menu principale.

L'opzione **Solo download** rileva gli aggiornamenti, scarica quelli disponibili e quindi ti informa in Centro notifiche che sono pronti per l'installazione. Questo è l'opzione predefinita.

## <a name="remote-desktop-settings"></a>Impostazioni del desktop remoto

Lo stato corrente delle impostazioni del desktop remoto viene visualizzato nella schermata predefinita dello strumento Server Configuration. È possibile configurare le impostazioni del desktop remoto elencate di seguito accedendo all'opzione del menu principale **Remote Desktop** e seguendo le istruzioni visualizzate.

- Abilitare Desktop remoto per i client che eseguono Desktop remoto con Autenticazione a livello di rete

- Abilitare Desktop remoto per i client che eseguono qualsiasi versione di Desktop remoto

- Disabilitare Desktop remoto

## <a name="date-and-time-settings"></a>Impostazioni di data e ora

Puoi accedere alle impostazioni di data e ora e modificarle tramite l'opzione **Data e ora** del menu principale.

## <a name="telemetry-settings"></a>Impostazioni di telemetria

Questa opzione consente di configurare quali dati inviare a Microsoft.

## <a name="windows-activation-settings"></a>Impostazioni di Attivazione di Windows

Questa opzione consente di configurare Attivazione di Windows.

## <a name="to-enable-remote-management"></a>Per abilitare la gestione remota

È possibile abilitare vari scenari di gestione remota dall'opzione del menu principale **Configure Remote Management** :

- Gestione remota della console MMC (Microsoft Management Console)

- Windows PowerShell

- Server Manager  

## <a name="to-log-off-restart-or-shut-down-the-server"></a>Per disconnettersi, riavviare o chiudere il server

Per disconnettersi, riavviare o chiudere il server, accedere alla voce di menu corrispondente dal menu principale. Queste opzioni sono disponibili anche nel menu **Sicurezza di Windows**, accessibile da qualsiasi applicazione e in qualsiasi momento premendo CTRL+ALT+CANC.  

## <a name="to-exit-to-the-command-line"></a>Per chiudere la riga di comando
  
Per chiudere la riga di comando, selezionare l'opzione **Exit to the Command Line** e premere INVIO. Per tornare allo strumento Server Configuration, digita **Sconfig.cmd** e quindi premi INVIO.
