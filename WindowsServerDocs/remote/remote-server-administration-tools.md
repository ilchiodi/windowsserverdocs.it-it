---
title: Strumenti di amministrazione server remoto
description: Argomento di primo livello per Strumenti di amministrazione remota del server
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-rsat
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d54a1f5e-af68-497e-99be-97775769a7a7
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: e8e8206531e0939a1b6d6dfd17f5c5dd59947c81
ms.sourcegitcommit: 51e0b575ef43cd16b2dab2db31c1d416e66eebe8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2020
ms.locfileid: "76259136"
---
# <a name="remote-server-administration-tools"></a>Strumenti di amministrazione server remoto

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento supporta Strumenti di amministrazione remota del server per Windows 10.

> [!IMPORTANT]
> A partire da Windows 10 ottobre 2018 aggiornamento, strumenti di amministrazione remota del server è incluso come set di **funzionalità su richiesta** in Windows 10. Vedere **quando usare la versione di amministrazione remota** di seguito per le istruzioni di installazione.

Strumenti di amministrazione remota del server consente agli amministratori IT di gestire ruoli e funzionalità di Windows Server da un PC Windows 10.

Strumenti di amministrazione remota del server include Server Manager, snap-in di Microsoft Management Console (MMC), console, cmdlet di Windows PowerShell e provider, oltre ad alcuni strumenti da riga di comando per la gestione di ruoli e funzionalità eseguiti in Windows Server.

Strumenti di amministrazione remota del server include i moduli dei cmdlet di Windows PowerShell che possono essere utilizzati per gestire ruoli e funzionalità in esecuzione in server remoti. Sebbene la gestione remota di Windows PowerShell è abilitata per impostazione predefinita in Windows Server 2016, non è abilitato per impostazione predefinita in Windows 10. Per eseguire i cmdlet che fanno parte di Strumenti di amministrazione remota del server su un server remoto, eseguire `Enable-PSremoting` in una sessione di Windows PowerShell aperta con diritti utente elevati, ovvero Esegui come amministratore, nel computer client Windows dopo l'installazione di Strumenti di amministrazione remota del server.

## <a name="BKMK_Thresh"></a>Strumenti di amministrazione remota del server per Windows 10
Usare Strumenti di amministrazione remota del server per Windows 10 per gestire tecnologie specifiche in computer che eseguono Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 e in casi limitati, Windows Server 2012 o Windows Server 2008 R2.

Remote Server Administration Tools per Windows 10 include il supporto per la gestione remota dei computer che eseguono l'opzione di installazione Server Core o la configurazione interfaccia Server minima di Windows Server 2016, Windows Server 2012 R2 e in alcuni casi, le opzioni di installazione Server Core di Windows Server 2012. Tuttavia, è Impossibile installare Remote Server Administration Tools per Windows 10 in tutte le versioni del sistema operativo Windows Server.

### <a name="tools-available-in-this-release"></a>Strumenti disponibili in questa versione
Per un elenco degli strumenti disponibili in Strumenti di amministrazione remota del server per Windows 10, vedere la tabella in [strumenti di amministrazione remota del server (strumenti di amministrazione remota del server) per i sistemi operativi Windows](https://support.microsoft.com/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

### <a name="system-requirements"></a>Requisiti di sistema
Remote Server Administration Tools per Windows 10 può essere installato solo su computer che eseguono Windows 10. Strumenti di amministrazione remota del Server non può essere installati nei computer che eseguono Windows RT 8.1 o altri dispositivi system on chip.

Strumenti di amministrazione remota del server per Windows 10 è in esecuzione su versioni di Windows 10 basate su x86 e su x64.

> [!IMPORTANT]
> Strumenti di amministrazione remota del server per Windows 10 non deve essere installato in un computer che esegue pacchetti di strumenti di amministrazione per Windows 8.1, Windows 8, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 o Windows 2000 Server. Rimuovere tutte le versioni precedenti del pacchetto di strumenti di amministrazione o Strumenti di amministrazione remota del server, incluse le versioni provvisorie precedenti e le versioni degli strumenti per lingue o impostazioni locali diverse dal computer prima di installare Amministrazione remota del server Strumenti per Windows 10.

Per utilizzare questa versione di Server Manager per accedere e gestire i server remoti che eseguono Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2, è necessario installare alcuni aggiornamenti per rendere gestibili i sistemi operativi Windows Server meno recenti utilizzando se Gestione rver. Per informazioni dettagliate su come preparare Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2 per la gestione usando Server Manager in Strumenti di amministrazione remota del server per Windows 10, vedere [gestire più server remoti con Server Manager](https://technet.microsoft.com/library/hh831456.aspx).

Gestione remota di Windows PowerShell e Server Manager deve essere abilitata su server remoti gestirli utilizzando strumenti che fanno parte di Remote Server Administration Tools per Windows 10. La gestione remota è abilitata per impostazione predefinita nei server che eseguono Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012. Per altre informazioni su come abilitare la gestione remota se è stata disabilitata, vedere [Gestire più server remoti con Server Manager](https://go.microsoft.com/fwlink/p/?LinkId=241358).

## <a name="install-uninstall-and-turn-offon-rsat-tools"></a>Installare, disinstallare e disattivare/su strumenti di strumenti di amministrazione remota del server

### <a name="use-features-on-demand-fod-to-install-specific-rsat-tools-on-windows-10-october-2018-update-or-later"></a>Usare le funzionalità su richiesta (Dom) per installare strumenti di strumenti di amministrazione remota specifici nell'aggiornamento di Windows 10 ottobre 2018 o versioni successive

A partire da Windows 10 ottobre 2018 aggiornamento, strumenti di amministrazione remota del server è incluso come set di **funzionalità su richiesta** direttamente da Windows 10. A questo punto, anziché scaricare un pacchetto di strumenti di amministrazione remota, è sufficiente passare a **Gestisci funzionalità facoltative** in **Impostazioni** e fare clic su **Aggiungi una funzionalità** per visualizzare l'elenco degli strumenti di amministrazione remota del server disponibili. Selezionare e installare gli strumenti specifici di strumenti di amministrazione remota del server necessari. Per visualizzare lo stato di avanzamento dell'installazione, fare clic sul pulsante **indietro** per visualizzare lo stato nella pagina **Gestisci funzionalità facoltative** .

Vedere l' [elenco degli strumenti di strumenti di amministrazione remota del server disponibili tramite **funzionalità su richiesta**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). Oltre a eseguire l'installazione tramite l'app **Impostazioni** grafiche, è anche possibile installare strumenti di strumenti di amministrazione remota specifici tramite la riga di comando o l'automazione con [**DISM/Add-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

Un vantaggio delle funzionalità su richiesta è che le funzionalità installate vengono mantenute negli aggiornamenti della versione di Windows 10.

#### <a name="to-uninstall-specific-rsat-tools-on-windows-10-october-2018-update-or-later-after-installing-with-fod"></a>Per disinstallare strumenti di strumenti di amministrazione remota specifici nell'aggiornamento di Windows 10 ottobre 2018 o versioni successive (dopo l'installazione con Dom)

In Windows 10 aprire l'app **Impostazioni** , passare a **Gestisci funzionalità facoltative**, selezionare e disinstallare gli strumenti di strumenti di amministrazione remota specifici che si vuole rimuovere. Si noti che in alcuni casi sarà necessario disinstallare manualmente le dipendenze. In particolare, se lo strumento strumenti di amministrazione remota A è necessario per lo strumento di amministrazione remota del server, la scelta di disinstallare lo strumento di amministrazione remota a avrà esito negativo se è ancora installato lo strumento strumenti In questo caso, disinstallare prima lo strumento di amministrazione remota e quindi disinstallare lo strumento strumenti di amministrazione remota A. Si noti inoltre che in alcuni casi è possibile che la disinstallazione di uno strumento di amministrazione remota dei server abbia esito positivo anche se lo strumento è ancora installato. In questo caso, il riavvio del PC comporterà la rimozione dello strumento.

Vedere l' [elenco degli strumenti di amministrazione remota del server, incluse le dipendenze](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). Oltre alla disinstallazione tramite l'app impostazioni grafiche, è anche possibile disinstallare strumenti di strumenti di amministrazione remota specifici tramite la riga di comando o l'automazione con [**DISM/Remove-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

### <a name="when-to-use-which-rsat-version"></a>Quando usare la versione di strumenti di amministrazione remota

Se si dispone di una versione di Windows 10 precedente all'aggiornamento del 2018 ottobre (1809), non sarà possibile utilizzare **funzionalità su richiesta**. È necessario scaricare e installare il pacchetto di strumenti di amministrazione remota del server.

- **Installare strumenti di amministrazione remota di FODs direttamente da Windows 10, come**descritto in precedenza: quando si esegue l'installazione in Windows 10 ottobre 2018 aggiornamento (1809) o versione successiva, per la gestione di windows server 2019 o versioni precedenti.

- **Scaricare e installare WS_1803 pacchetto di strumenti di amministrazione remota del server, come descritto di seguito**: quando si esegue l'installazione in Windows 10 aprile 2018 aggiornamento (1803) o versioni precedenti, per la gestione di Windows Server, versione 1803 o Windows Server, versione 1709.

- **Scaricare e installare il pacchetto WS2016 di gestione remota Windows, come descritto di seguito**: quando si esegue l'installazione in Windows 10 aprile 2018 aggiornamento (1803) o versioni precedenti, per la gestione di windows server 2016 o versioni precedenti.

#### <a name="BKMK_installthresh"></a>Scaricare il pacchetto di strumenti di amministrazione remota per installare Strumenti di amministrazione remota del server per Windows 10

1.  Scaricare il pacchetto Remote Server Administration Tools per Windows 10 dal [Microsoft Download Center](https://go.microsoft.com/fwlink/?LinkID=404281). È possibile eseguire il programma di installazione dal sito Web dell'Area download oppure salvare il pacchetto di download in una condivisione o in un computer locale.

    > [!IMPORTANT]
    > È possibile installare Remote Server Administration Tools per Windows 10 solo nei computer che eseguono Windows 10. Strumenti di amministrazione remota del Server non può essere installati nei computer che eseguono Windows RT 8.1 o altri dispositivi system on chip.

2.  Se si salva il pacchetto di download in una condivisione o in un computer locale, fare doppio clic sul programma di installazione **WindowsTH-KB2693643-x64.msu** o **WindowsTH-KB2693643-x86.msu**, a seconda dell'architettura del computer in cui si desidera installare gli strumenti.

3.  Quando nella finestra di dialogo **Programma di installazione Windows Update autonomo** viene richiesto di installare l'aggiornamento, fare clic su **Sì**.

4.  Leggere e accettare le condizioni di licenza. Fare clic su **Accetto**.

5.  L'installazione richiede alcuni minuti.

##### <a name="to-uninstall-remote-server-administration-tools-for-windows-10-after-rsat-package-install"></a>Per disinstallare Strumenti di amministrazione remota del server per Windows 10 (dopo l'installazione del pacchetto di strumenti di amministrazione remota)

1. Sul desktop fare clic su **Start**, quindi su **Tutte le app**, **Sistema Windows**e infine su **Pannello di controllo**.

2. In **Programmi**fare clic su **Disinstalla un programma**.

3. Fare clic su **Visualizza aggiornamenti installati**.

4. Fare clic con il pulsante destro del mouse su **Aggiornamento per Microsoft Windows (KB2693643)** e scegliere **Disinstalla**.

5. Quando viene richiesto di confermare la disinstallazione dell'aggiornamento, fare clic su **Sì**.
   S
   ##### <a name="to-turn-off-specific-tools-after-rsat-package-install"></a>Per disattivare strumenti specifici (dopo l'installazione del pacchetto di strumenti di amministrazione remota)

6. Sul desktop fare clic su **Start**, quindi su **Tutte le app**, **Sistema Windows**e infine su **Pannello di controllo**.

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
> -   **Pannello di controllo\Sistema e sicurezza\Strumenti di amministrazione**.
> -   Collegamento salvato sul desktop dalla cartella **Strumenti di amministrazione** . A questo scopo, fare clic con il pulsante destro del mouse sul collegamento **Pannello di controllo\Sistema e sicurezza\Strumenti di amministrazione** , quindi fare clic su **Crea collegamento**.

Gli strumenti installati come parte del Strumenti di amministrazione remota del server per Windows 10 non possono essere utilizzati per gestire il computer client locale. Indipendentemente dallo strumento eseguito, è necessario specificare un server remoto o più server remoti in cui eseguire lo strumento. Poiché la maggior parte degli strumenti sono integrati con Server Manager, aggiungere i server remoti che si desidera gestire per il pool di server di gestione Server prima di gestire il server utilizzando gli strumenti di **strumenti** menu. Per altre informazioni su come aggiungere server al pool di server e creare gruppi di server personalizzati, vedere [Aggiungere server a Server Manager](https://go.microsoft.com/fwlink/p/?LinkId=241353) e [Creare e gestire gruppi di server](https://go.microsoft.com/fwlink/?LinkId=247328).

In Strumenti di amministrazione remota del server per Windows 10, tutti gli strumenti di gestione del server basati su GUI, ad esempio gli snap-in di MMC e le finestre di dialogo, sono accessibili dal menu **strumenti** della console di Server Manager. Anche se il computer che esegue Remote Server Administration Tools per Windows 10 esegue un sistema operativo basato su client, dopo l'installazione degli strumenti, Server Manager, incluso in remoto Server Administration Tools per Windows 10, viene aperto automaticamente per impostazione predefinita nel computer client. Si noti che non esiste alcun **Server locale** pagina nella console di Server Manager che viene eseguito in un computer client.

##### <a name="to-start-server-manager-on-a-client-computer"></a>Per avviare Server Manager in un computer client

1.  Nel menu **Start** fare clic su **Tutte le app**e quindi su **Strumenti di amministrazione**.

2.  Nella cartella **Strumenti di amministrazione** fare clic su **Server Manager**.

Anche se non sono elencati nel menu **strumenti** della console di Server Manager, vengono installati anche i cmdlet di Windows PowerShell e gli strumenti di gestione del prompt dei comandi per i ruoli e le funzionalità come parte del strumenti di amministrazione remota del server. Se ad esempio si apre una sessione di Windows PowerShell con diritti utente elevati (Esegui come amministratore) e si esegue il cmdlet `Get-Command -Module RDManagement`, i risultati includono un elenco di cmdlet di Servizi Desktop remoto che sono ora disponibili per l'esecuzione nel computer locale dopo l'installazione di Strumenti di amministrazione remota del server, purché i cmdlet siano assegnati a un server remoto che esegue tutto o parte del ruolo Servizi Desktop remoto.

##### <a name="to-start-windows-powershell-with-elevated-user-rights-run-as-administrator"></a>Per avviare Windows PowerShell con diritti utente elevati (Esegui come amministratore)

1.  Nel menu **Start** fare clic su **Tutte le app**, quindi su **Sistema Windows**e infine su **Windows PowerShell**.

2.  Per eseguire Windows PowerShell come amministratore dal desktop, fare doppio clic sui **Windows PowerShell** scelta rapida e quindi fare clic su **Esegui come amministratore**.

> [!NOTE]
> È inoltre possibile avviare una sessione di Windows PowerShell destinato a un server specifico facendo clic su un server gestito in una pagina di gruppo o ruolo in Server Manager e quindi facendo clic su **Windows PowerShell**.


## <a name="known-issues"></a>Problemi noti

### <a name="issue-rsat-fod-installation-fails-with-error-code-0x800f0954"></a>**Problema**: l'installazione di amministrazione remota dei server non riesce con codice di errore 0x800f0954

> **Effetto**: strumenti di amministrazione remota del server FODs in Windows 10 1809 (aggiornamento ottobre 2018) in ambienti WSUS/SCCM
> 
> **Soluzione**: per installare FODs in un computer aggiunto a un dominio che riceve gli aggiornamenti tramite WSUS o SCCM, sarà necessario modificare un'impostazione di criteri di gruppo per abilitare il download di FODs direttamente da Windows Update o da una condivisione locale. Per altri dettagli e istruzioni su come modificare questa impostazione, vedere [come rendere disponibili le funzionalità su richiesta e i Language Pack quando si usa WSUS/SCCM](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs).

---

### <a name="issue-rsat-fod-installation-via-settings-app-does-not-show-statusprogress"></a>**Problema**: l'installazione di strumenti di amministrazione remota del server tramite impostazioni non Mostra lo stato o lo stato di avanzamento

> **Effetto**: strumenti di amministrazione remota di FODs in Windows 10 1809 (aggiornamento di ottobre 2018)
> 
> **Soluzione**: per visualizzare lo stato di avanzamento dell'installazione, fare clic sul pulsante **indietro** per visualizzare lo stato nella pagina **Gestisci funzionalità facoltative** .

---

### <a name="issue-rsat-fod-uninstallation-via-settings-app-may-fail"></a>**Problema**: la disinstallazione di strumenti di amministrazione remota del server tramite le impostazioni potrebbe non riuscire

> **Effetto**: strumenti di amministrazione remota di FODs in Windows 10 1809 (aggiornamento di ottobre 2018)
> 
> **Risoluzione**: in alcuni casi, gli errori di disinstallazione sono dovuti alla necessità di disinstallare manualmente le dipendenze. In particolare, se lo strumento strumenti di amministrazione remota A è necessario per lo strumento di amministrazione remota del server, la scelta di disinstallare lo strumento di amministrazione remota a avrà esito negativo se è ancora installato lo strumento strumenti In questo caso, disinstallare prima lo strumento di amministrazione remota e quindi disinstallare lo strumento strumenti di amministrazione remota A. Vedere l'elenco dei FODs di amministrazione remota del server, incluse le dipendenze.

---

### <a name="issue-rsat-fod-uninstallation-appears-to-succeed-but-the-tool-is-still-installed"></a>**Problema**: la disinstallazione di strumenti di amministrazione remota dei server ha esito positivo, ma lo strumento è ancora installato

> **Effetto**: strumenti di amministrazione remota di FODs in Windows 10 1809 (aggiornamento di ottobre 2018)
> 
> **Soluzione**: il riavvio del PC comporterà la rimozione dello strumento.

---

### <a name="issue-rsat-missing-after-windows-10-upgrade"></a>**Problema**: strumenti di amministrazione remota mancanti dopo l'aggiornamento di Windows 10

> **Impact**: qualsiasi strumento di amministrazione remota del server. Installazione del pacchetto MSU (prima di FODs di strumenti di amministrazione remota) non reinstallata automaticamente
> 
> **Soluzione**: un'installazione di strumenti di amministrazione remota non può essere resa permanente attraverso gli aggiornamenti del sistema operativo a causa di strumenti di amministrazione MSU recapitato come pacchetto di Windows Update. Installare di nuovo il server di amministrazione remota del server dopo aver aggiornato Windows 10. Si noti che questa limitazione è uno dei motivi per cui è stato spostato in FODs a partire da Windows 10 1809. Gli strumenti di amministrazione remota di FODs installati verranno mantenuti tra gli aggiornamenti delle versioni future di Windows 10.

## <a name="see-also"></a>Vedi anche
>- [Strumenti di amministrazione remota del server per Windows 10](https://go.microsoft.com/fwlink/?LinkID=404281)
>- [Strumenti di amministrazione remota del server (strumenti di amministrazione remota) per Windows Vista, Windows 7, Windows 8, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 e Windows Server 2012 R2](https://go.microsoft.com/fwlink/p/?LinkID=221055)
