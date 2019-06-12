---
title: Strumenti di amministrazione server remoto
description: Argomento di livello superiore per strumenti di amministrazione remota del Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-rsat
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d54a1f5e-af68-497e-99be-97775769a7a7
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 30ca0a1e8a2f17f54a8f05d7270bf9512be7a8dc
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805181"
---
# <a name="remote-server-administration-tools"></a>Strumenti di amministrazione server remoto

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento supporta Remote Server Administration Tools per Windows 10.

> [!IMPORTANT]
> Avvio con Windows 10 ottobre 2018 Update, amministrazione remota del server è incluso come un set di **funzionalità su richiesta** in Windows 10 se stesso. Visualizzare **quando usare la versione di amministrazione remota del server** sotto per le istruzioni di installazione.

Amministrazione remota del server consente agli amministratori IT di gestire ruoli e funzionalità Windows Server da un PC Windows 10.

Strumenti di amministrazione remota del Server include Server Manager, snap-in Microsoft Management Console (mmc), console, cmdlet di Windows PowerShell e i provider oltre ad alcuni strumenti da riga di comando per la gestione dei ruoli e funzionalità eseguiti in Windows Server.

Strumenti di amministrazione remota del Server include i moduli dei cmdlet Windows PowerShell possono essere utilizzati per gestire ruoli e funzionalità in esecuzione su server remoti. Sebbene la gestione remota di Windows PowerShell è abilitata per impostazione predefinita in Windows Server 2016, non è abilitato per impostazione predefinita in Windows 10. Per eseguire i cmdlet che fanno parte di strumenti di amministrazione remota del Server in un server remoto, eseguire `Enable-PSremoting` in una sessione di Windows PowerShell che è stata aperta con diritti utente elevati (ovvero con Esegui come amministratore) nel computer client Windows dopo installazione di strumenti di amministrazione remota del Server.

## <a name="BKMK_Thresh"></a>Strumenti di amministrazione remota del Server per Windows 10
Utilizzare Remote Server Administration Tools per Windows 10 per gestire tecnologie specifiche nei computer che eseguono Windows Server 2016, Windows Server 2012 R2 e in alcuni casi, Windows Server 2012 o Windows Server 2008 R2.

Remote Server Administration Tools per Windows 10 include il supporto per la gestione remota dei computer che eseguono l'opzione di installazione Server Core o la configurazione interfaccia Server minima di Windows Server 2016, Windows Server 2012 R2 e in alcuni casi, le opzioni di installazione Server Core di Windows Server 2012. Tuttavia, è Impossibile installare Remote Server Administration Tools per Windows 10 in tutte le versioni del sistema operativo Windows Server.

### <a name="tools-available-in-this-release"></a>Strumenti disponibili in questa versione
per un elenco degli strumenti disponibili in remoto Server Administration Tools per Windows 10, vedere la tabella in [amministrazione remota del Server strumenti () per i sistemi operativi Windows](https://support.microsoft.com/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

### <a name="system-requirements"></a>Requisiti di sistema
Remote Server Administration Tools per Windows 10 può essere installato solo su computer che eseguono Windows 10. Strumenti di amministrazione remota del Server non può essere installati nei computer che eseguono Windows RT 8.1 o altri dispositivi system on chip.

Remote Server Administration Tools per Windows 10 viene eseguita nelle edizioni basate su x86 e basate su x64 di Windows 10.

> [!IMPORTANT]
> Remote Server Administration Tools per Windows 10 non deve essere installato in un computer che esegue pacchetti di strumenti di amministrazione per Windows 8.1, Windows 8, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 o Windows 2000 Server. Rimuovere tutte le versioni precedenti di strumenti di amministrazione o strumenti di amministrazione Server remota, tra cui le versioni non definitive precedenti e versioni degli strumenti per diverse lingue o impostazioni locali dal computer prima di installare amministrazione remota del Server Strumenti per Windows 10.

Per usare questa versione di Server Manager per accedere e gestire server remoti che eseguono Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2, è necessario installare alcuni aggiornamenti per rendere i sistemi operativi Windows Server precedenti gestibili tramite SA Server Manager. Per informazioni dettagliate su come preparare Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2 per la gestione tramite Server Manager in remoto Server Administration Tools per Windows 10, vedere [Manage Multiple, Remote Servers con Server Manager](https://technet.microsoft.com/library/hh831456.aspx).

Gestione remota di Windows PowerShell e Server Manager deve essere abilitata su server remoti gestirli utilizzando strumenti che fanno parte di Remote Server Administration Tools per Windows 10. Gestione remota è abilitata per impostazione predefinita nei server che eseguono Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012. Per altre informazioni su come abilitare la gestione remota se è stata disabilitata, vedere [Gestire più server remoti con Server Manager](https://go.microsoft.com/fwlink/p/?LinkId=241358).

## <a name="install-uninstall-and-turn-offon-rsat-tools"></a>Installare, disinstallare e attivare/disattivare gli strumenti di amministrazione remota del server

### <a name="use-features-on-demand-fod-to-install-specific-rsat-tools-on-windows-10-october-2018-update-or-later"></a>Usare le funzionalità su richiesta (DOM) per installare gli strumenti di amministrazione remota del server specifici in Windows 10 ottobre 2018 Update, o versione successiva

Avvio con Windows 10 ottobre 2018 Update, amministrazione remota del server è incluso come un set di **funzionalità su richiesta** direttamente da Windows 10. A questo punto, invece di scaricare un pacchetto di amministrazione remota del server è sufficiente passare a **Gestisci funzionalità facoltative** nelle **impostazioni** e fare clic su **aggiunge una funzionalità** per visualizzare l'elenco di strumenti di amministrazione remota del server disponibili. Selezionare e installare gli strumenti di amministrazione remota del server specifici che necessari. Per visualizzare lo stato dell'installazione, fare clic il **nuovamente** pulsante per visualizzare lo stato sulle **Gestisci funzionalità facoltative** pagina.

Vedere le [elenco di amministrazione remota del server degli strumenti disponibili tramite **funzionalità su richiesta**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). Oltre a installare tramite la grafica **le impostazioni** app, è anche possibile installare gli strumenti di amministrazione remota del server specifici tramite riga di comando o mediante automazione [ **DISM /Add-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

Uno dei vantaggi delle funzionalità su richiesta è che le funzionalità installate mantenuto tra gli aggiornamenti di versione di Windows 10.

#### <a name="to-uninstall-specific-rsat-tools-on-windows-10-october-2018-update-or-later-after-installing-with-fod"></a>Per disinstallare l'amministrazione remota del server specifici tools in Windows 10 ottobre 2018 Update o versione successiva (dopo l'installazione con DOM)

In Windows 10, aprire il **le impostazioni** app e passare alla **Gestisci funzionalità facoltative**, selezionare e disinstallare gli strumenti di amministrazione remota del server specifici che si desidera rimuovere. Si noti che in alcuni casi, è necessario disinstallare manualmente le dipendenze. In particolare, se lo strumento di amministrazione remota del server A è necessario dallo strumento di amministrazione remota del server B, scegliendo la disinstallazione di un strumento di amministrazione remota del server avrà esito negativo se lo strumento di amministrazione remota del server B è ancora installato. In questo caso, disinstallare prima lo strumento di amministrazione remota del server B e quindi disinstallare r. lo strumento di amministrazione remota del server Si noti inoltre che in alcuni casi, la disinstallazione di uno strumento di amministrazione remota del server venga visualizzato l'esito positivo anche se è ancora installato lo strumento. In questo caso, riavviare il PC verrà completata la rimozione dello strumento.

Vedere le [elenco di strumenti di amministrazione remota del server, incluse le dipendenze](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). Oltre alla disinstallazione tramite l'app per le impostazioni con interfaccia grafica, è anche possibile disinstallare gli strumenti di amministrazione remota del server specifici tramite riga di comando o automazione seguendo [ **DISM /Remove-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

### <a name="when-to-use-which-rsat-version"></a>Quando usare la versione di amministrazione remota del server

Se si dispone di una versione di Windows 10 prima di ottobre 2018 aggiornamento (1809), non sarà in grado di usare **funzionalità su richiesta**. È necessario scaricare e installare il pacchetto di amministrazione remota del server.

- **Installare amministrazione remota del server FODs direttamente da Windows 10, come descritto in precedenza**: Quando si installa in Windows 10 ottobre 2018 Update (1809) o versione successiva, per la gestione di Windows Server 2019 o versioni precedenti.

- **Scaricare e installare il pacchetto RSAT WS_1803, come indicato di seguito**: Quando si installa in Windows 10 April 2018 Update (1803) o versioni precedenti, per la gestione di Windows Server, versione 1803 o Windows Server, versione 1709.

- **Scaricare e installare il pacchetto RSAT WS2016, come indicato di seguito**: Quando si installa in Windows 10 April 2018 Update (1803) o versioni precedenti, per la gestione di Windows Server 2016 o versioni precedenti.

#### <a name="BKMK_installthresh"></a>Scaricare il pacchetto RSAT per installare Remote Server Administration Tools per Windows 10

1.  Scaricare il pacchetto Remote Server Administration Tools per Windows 10 dal [Microsoft Download Center](https://go.microsoft.com/fwlink/?LinkID=404281). È possibile eseguire il programma di installazione dal sito Web dell'Area download oppure salvare il pacchetto di download in una condivisione o in un computer locale.

    > [!IMPORTANT]
    > È possibile installare Remote Server Administration Tools per Windows 10 solo nei computer che eseguono Windows 10. Strumenti di amministrazione remota del Server non può essere installati nei computer che eseguono Windows RT 8.1 o altri dispositivi system on chip.

2.  Se si salva il pacchetto di download in una condivisione o in un computer locale, fare doppio clic sul programma di installazione **WindowsTH-KB2693643-x64.msu** o **WindowsTH-KB2693643-x86.msu**, a seconda dell'architettura del computer in cui si desidera installare gli strumenti.

3.  Quando nella finestra di dialogo **Programma di installazione Windows Update autonomo** viene richiesto di installare l'aggiornamento, fare clic su **Sì**.

4.  Leggere e accettare le condizioni di licenza. Fare clic su **Accetto**.

5.  L'installazione richiede alcuni minuti.

##### <a name="to-uninstall-remote-server-administration-tools-for-windows-10-after-rsat-package-install"></a>Per disinstallare gli strumenti di amministrazione remota del Server per Windows 10 (dopo l'installazione del pacchetto RSAT)

1. Sul desktop fare clic su **Start**, quindi su **Tutte le app**, **Sistema Windows** e infine su **Pannello di controllo**.

2. In **Programmi** fai clic su **Disinstalla un programma**.

3. Fare clic su **Visualizza aggiornamenti installati**.

4. Fare clic con il pulsante destro del mouse su **Aggiornamento per Microsoft Windows (KB2693643)** e scegliere **Disinstalla**.

5. Quando viene richiesto di confermare la disinstallazione dell'aggiornamento, fare clic su **Sì**.
   S
   ##### <a name="to-turn-off-specific-tools-after-rsat-package-install"></a>Per disattivare strumenti specifici (dopo aver installato il pacchetto RSAT)

6. Sul desktop fare clic su **Start**, quindi su **Tutte le app**, **Sistema Windows** e infine su **Pannello di controllo**.

7. Fare clic su **Programmi**e quindi, in **Programmi e funzionalità** , fare clic su **Attivazione o disattivazione delle funzionalità Windows**.

8. Nella finestra di dialogo **Funzionalità Windows** espandere **Strumenti di amministrazione remota del server**, quindi espandere **Strumenti di amministrazione ruoli** o **Strumenti di amministrazione funzionalità**.

9. Deselezionare le caselle di controllo per tutti gli strumenti che si desidera disattivare.

   > [!NOTE]
   > Se disattivare Gestione Server, è necessario riavviare il computer e gli strumenti che fosse accessibile dal **strumenti** dal menu di Server Manager deve essere aperto dal **Strumenti di amministrazione** cartella.

10. Terminata la disattivazione degli strumenti che non si desidera utilizzare, fare clic su **OK**.

### <a name="run-remote-server-administration-tools"></a>Eseguire Strumenti di amministrazione remota del server

> [!NOTE]
> Dopo l'installazione di Remote Server Administration Tools per Windows 10, il **Strumenti di amministrazione** viene visualizzata nella cartella di **avviare** menu. È possibile accedere agli strumenti dai percorsi seguenti.
>
> -   Il **strumenti** menu nella console di Server Manager.
> -   Da **Pannello di controllo\Sistema e sicurezza\Strumenti di amministrazione**.
> -   Collegamento salvato sul desktop dalla cartella **Strumenti di amministrazione**. A questo scopo, fare clic con il pulsante destro del mouse sul collegamento **Pannello di controllo\Sistema e sicurezza\Strumenti di amministrazione**, quindi fare clic su **Crea collegamento**.

Gli strumenti installati come parte di Remote Server Administration Tools per Windows 10 non possono essere usati per gestire il computer client locale. Indipendentemente dallo strumento di esecuzione, è necessario specificare un server remoto, o più server remoti, in cui eseguire lo strumento. Poiché la maggior parte degli strumenti sono integrati con Server Manager, aggiungere i server remoti che si desidera gestire per il pool di server di gestione Server prima di gestire il server utilizzando gli strumenti di **strumenti** menu. Per altre informazioni su come aggiungere server al pool di server e creare gruppi di server personalizzati, vedere [Aggiungere server a Server Manager](https://go.microsoft.com/fwlink/p/?LinkId=241353) e [Creare e gestire gruppi di server](https://go.microsoft.com/fwlink/?LinkId=247328).

In strumenti di amministrazione remota del Server per Windows 10, tutti gli strumenti di gestione basata su GUI server, ad esempio lo snap-in di mmc e finestra di dialogo finestre, sono accessibili dal **strumenti** menu della console di Server Manager. Anche se il computer che esegue Remote Server Administration Tools per Windows 10 esegue un sistema operativo basato su client, dopo l'installazione degli strumenti, Server Manager, incluso in remoto Server Administration Tools per Windows 10, viene aperto automaticamente per impostazione predefinita nel computer client. Si noti che non esiste alcun **Server locale** pagina nella console di Server Manager che viene eseguito in un computer client.

##### <a name="to-start-server-manager-on-a-client-computer"></a>Per avviare Server Manager in un computer client

1.  Nel menu **Start** fare clic su **Tutte le app** e quindi su **Strumenti di amministrazione**.

2.  Nella cartella **Strumenti di amministrazione** fare clic su **Server Manager**.

Anche se non sono elencati nella console di Server Manager **strumenti** menu, i cmdlet di Windows PowerShell e il prompt dei comandi strumenti di gestione vengono installati anche per ruoli e funzionalità come parte di strumenti di amministrazione remota del Server. Ad esempio, se si apre una sessione di Windows PowerShell con diritti utente elevati (Esegui come amministratore) ed esegue il cmdlet `Get-Command -Module RDManagement`, i risultati includono un elenco di cmdlet di Servizi Desktop remoto sono disponibili per l'esecuzione nel computer locale dopo installazione di Remote Server Administration Tools, purché i cmdlet sono destinati a un server remoto che esegue tutto o parte del ruolo Servizi Desktop remoto.

##### <a name="to-start-windows-powershell-with-elevated-user-rights-run-as-administrator"></a>Per avviare Windows PowerShell con diritti utente elevati (Esegui come amministratore)

1.  Nel menu **Start** fare clic su **Tutte le app**, quindi su **Sistema Windows** e infine su **Windows PowerShell**.

2.  Per eseguire Windows PowerShell come amministratore dal desktop, fare doppio clic sui **Windows PowerShell** scelta rapida e quindi fare clic su **Esegui come amministratore**.

> [!NOTE]
> È inoltre possibile avviare una sessione di Windows PowerShell destinato a un server specifico facendo clic su un server gestito in una pagina di gruppo o ruolo in Server Manager e quindi facendo clic su **Windows PowerShell**.


## <a name="known-issues"></a>Problemi noti

### <a name="issue-rsat-fod-installation-fails-with-error-code-0x800f0954"></a>**Problema**: Installazione RSAT FOD ha esito negativo con codice di errore 0x800f0954

> **Impatto**: FODs di amministrazione remota del server in Windows 10 1809 (aggiornamento di ottobre 2018) negli ambienti di WSUS/SCCM
> 
> **Risoluzione**: Per installare FODs in un PC aggiunti a un dominio che riceve gli aggiornamenti tramite WSUS o SCCM, è necessario modificare un'impostazione di criteri di gruppo per abilitare il download FODs direttamente da Windows Update o una condivisione locale. Per altre informazioni e istruzioni su come modificare tale impostazione, vedere [come rendere le funzionalità su richiesta e language pack disponibili quando si usa WSUS/SCCM](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs).

---

### <a name="issue-rsat-fod-installation-via-settings-app-does-not-show-statusprogress"></a>**Problema**: Installazione RSAT FOD tramite app per le impostazioni non viene visualizzato lo stato/stato di avanzamento

> **Impatto**: FODs di amministrazione remota del server in Windows 10 1809 (aggiornamento di ottobre 2018)
> 
> **Risoluzione**: Per visualizzare lo stato dell'installazione, fare clic il **nuovamente** pulsante per visualizzare lo stato sulle **Gestisci funzionalità facoltative** pagina.

---

### <a name="issue-rsat-fod-uninstallation-via-settings-app-may-fail"></a>**Problema**: La disinstallazione RSAT FOD tramite app per le impostazioni potrebbe non riuscire

> **Impatto**: FODs di amministrazione remota del server in Windows 10 1809 (aggiornamento di ottobre 2018)
> 
> **Risoluzione**: In alcuni casi, gli errori di disinstallazione sono a causa della necessità di disinstallare manualmente le dipendenze. In particolare, se lo strumento di amministrazione remota del server A è necessario dallo strumento di amministrazione remota del server B, scegliendo la disinstallazione di un strumento di amministrazione remota del server avrà esito negativo se lo strumento di amministrazione remota del server B è ancora installato. In questo caso, disinstallare prima lo strumento di amministrazione remota del server B e quindi disinstallare r. lo strumento di amministrazione remota del server Visualizzare l'elenco di FODs amministrazione remota del server, incluse le dipendenze.

---

### <a name="issue-rsat-fod-uninstallation-appears-to-succeed-but-the-tool-is-still-installed"></a>**Problema**: La disinstallazione RSAT FOD sembra avere esito positivo, ma è ancora installato lo strumento

> **Impatto**: FODs di amministrazione remota del server in Windows 10 1809 (aggiornamento di ottobre 2018)
> 
> **Risoluzione**: Riavviare il PC verrà completata la rimozione dello strumento.

---

### <a name="issue-rsat-missing-after-windows-10-upgrade"></a>**Problema**: Amministrazione remota del server mancante dopo l'aggiornamento a Windows 10

> **Impatto**: Qualsiasi amministrazione remota del server. Installazione di (precedente a FODs RSAT) MSU pacchetto reinstallati automaticamente
> 
> **Risoluzione**: Un'installazione di amministrazione remota del server non può essere persistente tra gli aggiornamenti del sistema operativo a causa di amministrazione remota del server. MSU vengono recapitati come pacchetto di Windows Update. Installare amministrazione remota del server dopo l'aggiornamento di Windows 10. Si noti che questa limitazione è uno dei motivi per cui siamo passati alla FODs a partire da Windows 10 1809. FODs di amministrazione remota del server che vengono installati verranno mantenuti nei futuri aggiornamenti di versione di Windows 10.

## <a name="see-also"></a>Vedere anche
>- [Strumenti di amministrazione remota del Server per Windows 10](https://go.microsoft.com/fwlink/?LinkID=404281)
>- [Strumenti di amministrazione remota del Server (RSAT) per Windows Vista, Windows 7, Windows 8, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 e Windows Server 2012 R2](https://go.microsoft.com/fwlink/p/?LinkID=221055)
