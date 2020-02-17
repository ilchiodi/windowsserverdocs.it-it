---
title: Strumenti di amministrazione server remoto
description: Argomento di carattere generale per Strumenti di amministrazione remota del server
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
ms.openlocfilehash: 4e6452947af236f3021d493a42f536fed0cd110a
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822334"
---
# <a name="remote-server-administration-tools"></a>Strumenti di amministrazione server remoto

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento supporta Strumenti di amministrazione remota del server per Windows 10.

> [!IMPORTANT]
> A partire dall'aggiornamento di Windows 10 di ottobre 2018, la funzionalità Strumenti di amministrazione remota del server è inclusa come set di **funzionalità su richiesta** in Windows 10. Per istruzioni per l'installazione, vedi **Quando usare le diverse versioni di Strumenti di amministrazione remota del server** più avanti.

Strumenti di amministrazione remota del server consente agli amministratori IT di gestire ruoli e funzionalità di Windows Server da un PC Windows 10.

Strumenti di amministrazione remota del server include Server Manager, gli snap-in Microsoft Management Console (MMC), le console, i provider e i cmdlet di Windows PowerShell, nonché alcuni strumenti da riga di comando per la gestione dei ruoli e delle funzionalità che vengono eseguiti in Windows Server.

Strumenti di amministrazione remota del server include moduli di cmdlet di Windows PowerShell che possono essere usati per gestire ruoli e funzionalità che sono in esecuzione su server remoti. Sebbene la gestione remota di Windows PowerShell è abilitata per impostazione predefinita in Windows Server 2016, non è abilitato per impostazione predefinita in Windows 10. Per eseguire i cmdlet che fanno parte di Strumenti di amministrazione remota del Server in un server remoto, esegui `Enable-PSremoting` in una sessione di Windows PowerShell aperta con diritti utente elevati (ovvero da Esegui come amministratore) sul computer client Windows dopo l'installazione di Strumenti di amministrazione remota del server.

## <a name="BKMK_Thresh"></a>Strumenti di amministrazione remota del server per Windows 10
Usa Strumenti di amministrazione remota del server per Windows 10 per gestire tecnologie specifiche nei computer che eseguono Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 e, in alcuni casi, Windows Server 2012 o Windows Server 2008 R2.

Remote Server Administration Tools per Windows 10 include il supporto per la gestione remota dei computer che eseguono l'opzione di installazione Server Core o la configurazione interfaccia Server minima di Windows Server 2016, Windows Server 2012 R2 e in alcuni casi, le opzioni di installazione Server Core di Windows Server 2012. Tuttavia, è Impossibile installare Remote Server Administration Tools per Windows 10 in tutte le versioni del sistema operativo Windows Server.

### <a name="tools-available-in-this-release"></a>Strumenti disponibili in questa versione
Per un elenco degli strumenti disponibili in Strumenti di amministrazione remota del server per Windows 10, vedi la tabella in [Strumenti di amministrazione remota del server per i sistemi operativi Windows](https://support.microsoft.com/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

### <a name="system-requirements"></a>Requisiti di sistema
Remote Server Administration Tools per Windows 10 può essere installato solo su computer che eseguono Windows 10. Strumenti di amministrazione remota del Server non può essere installati nei computer che eseguono Windows RT 8.1 o altri dispositivi system on chip.

La funzionalità Strumenti di amministrazione remota del server per Windows 10 viene eseguita sia nelle edizioni di Windows 10 basate su x86 che in quelle basate su x64.

> [!IMPORTANT]
> Non installare Strumenti di amministrazione remota del server per Windows 10 in un computer che esegue pacchetti di strumenti di amministrazione per Windows 8.1, Windows 8, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 o Windows 2000 Server. Prima di installare Strumenti di amministrazione remota del Server per Windows 10, rimuovi dal computer tutte le versioni precedenti di strumenti di amministrazione o di Strumenti di amministrazione remota del server, tra cui le versioni non definitive precedenti e versioni degli strumenti per diverse lingue o impostazioni locali.

Per usare questa versione di Server Manager per l'accesso e la gestione di server remoti che eseguono Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2, devi installare alcuni aggiornamenti per rendere i precedenti sistemi operativi Windows Server gestibili tramite Server Manager. Per informazioni dettagliate sulla preparazione di Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2 per la gestione tramite Server Manager in Strumenti di amministrazione remota del server per Windows 10, vedi [Gestire più server remoti con Server Manager](https://technet.microsoft.com/library/hh831456.aspx).

Gestione remota di Windows PowerShell e Server Manager deve essere abilitata su server remoti gestirli utilizzando strumenti che fanno parte di Remote Server Administration Tools per Windows 10. La funzionalità Gestione remota è abilitata per impostazione predefinita nei server che eseguono Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012. Per altre informazioni su come abilitare la gestione remota se è stata disabilitata, vedere [Gestire più server remoti con Server Manager](https://go.microsoft.com/fwlink/p/?LinkId=241358).

## <a name="install-uninstall-and-turn-offon-rsat-tools"></a>Installare, disinstallare e disabilitare/abilitare strumenti specifici di Strumenti di amministrazione remota del server

### <a name="use-features-on-demand-fod-to-install-specific-rsat-tools-on-windows-10-october-2018-update-or-later"></a>Usa Funzionalità su richiesta per installare strumenti specifici di Strumenti di amministrazione remota del server nell'aggiornamento di Windows 10 di ottobre 2018 o versioni successive

A partire dall'aggiornamento di Windows 10 di ottobre 2018, la funzionalità Strumenti di amministrazione remota del server è inclusa come set di **funzionalità su richiesta** in Windows 10. A questo punto, anziché scaricare un pacchetto Strumenti di amministrazione remota del server, puoi semplicemente passare a **Gestisci funzionalità facoltative** in **Impostazioni** e fare clic su **Aggiungi una funzionalità** per visualizzare l'elenco degli strumenti di Strumenti di amministrazione remota del server disponibili. Seleziona e installa gli strumenti specifici di Strumenti di amministrazione remota del server di cui necessiti. Per visualizzare lo stato di avanzamento dell'installazione, fai clic sul pulsante **Indietro** per visualizzare lo stato nella pagina **Gestisci funzionalità facoltative**.

Vedi l'[elenco degli strumenti disponibili tramite **Funzionalità su richiesta**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). Oltre a eseguire l'installazione tramite l'app grafica **Impostazioni**, puoi anche installare strumenti specifici di Strumenti di amministrazione remota del server tramite la riga di comando o l'automazione usando [**DISM/Add-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

Un vantaggio offerto da Funzionalità su richiesta è rappresentato dal fatto che le funzionalità installate vengono mantenute da un aggiornamento all'altro della versione di Windows 10.

#### <a name="to-uninstall-specific-rsat-tools-on-windows-10-october-2018-update-or-later-after-installing-with-fod"></a>Per disinstallare strumenti specifici di Strumenti di amministrazione remota del server nell'aggiornamento di Windows 10 di ottobre 2018 o versioni successive (dopo l'installazione con Funzionalità su richiesta)

In Windows 10 apri l'app **Impostazioni**, passa a **Gestisci funzionalità facoltative**, seleziona e disinstalla gli strumenti specifici di Strumenti di amministrazione remota del server che vuoi rimuovere. Considera che in alcuni casi dovrai disinstallare manualmente le dipendenze. In particolare, se lo strumento A di Strumenti di amministrazione remota del server è necessario per lo strumento B di Strumenti di amministrazione remota del server, la disinstallazione dello strumento A avrà esito negativo se è ancora installato lo strumento B. In questo caso, disinstalla prima lo strumento B e quindi lo strumento A. Considera inoltre che in alcuni casi è possibile che la disinstallazione di uno strumento di Strumenti di amministrazione remota del server risulti aver avuto esito positivo anche se lo strumento è ancora installato. In questo caso, il riavvio del PC completerà la rimozione dello strumento.

Vedi l'[elenco degli strumenti di Strumenti di amministrazione remota del server che includono dipendenze](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). Oltre a eseguire la disinstallazione tramite l'app grafica Impostazioni, puoi anche disinstallare strumenti specifici di Strumenti di amministrazione remota del server tramite la riga di comando o l'automazione usando [**DISM/Remove-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

### <a name="when-to-use-which-rsat-version"></a>Quando usare le diverse versioni di Strumenti di amministrazione remota del server

Se disponi di una versione di Windows 10 precedente all'aggiornamento di ottobre 2018 (1809), non potrai usare **Funzionalità su richiesta**. Dovrai scaricare e installare il pacchetto Strumenti di amministrazione remota del server.

- **Installa le funzionalità su richiesta di Strumenti di amministrazione remota del server direttamente da Windows 10, come illustrato in precedenza**: quando esegui l'installazione nell'aggiornamento di Windows 10 di ottobre 2018 (1809) o versioni successive per la gestione di Windows Server 2019 o versioni precedenti.

- **Scarica e installa il pacchetto Strumenti di amministrazione remota del server WS_1803, come descritto più avanti**: quando esegui l'installazione nell'aggiornamento di Windows 10 di aprile 2018 (1803) o versioni precedenti per la gestione di Windows Server, versione 1803 o Windows Server, versione 1709.

- **Scarica e installa il pacchetto Strumenti di amministrazione remota del server WS2016, come descritto più avanti**: quando esegui l'installazione nell'aggiornamento di Windows 10 di aprile 2018 (1803) o versioni precedenti per la gestione di Windows Server 2016 o versioni precedenti.

#### <a name="BKMK_installthresh"></a>Scaricare il pacchetto Strumenti di amministrazione remota del server per installare Strumenti di amministrazione remota del server per Windows 10

1.  Scaricare il pacchetto Remote Server Administration Tools per Windows 10 dal [Microsoft Download Center](https://go.microsoft.com/fwlink/?LinkID=404281). È possibile eseguire il programma di installazione dal sito Web dell'Area download oppure salvare il pacchetto di download in una condivisione o in un computer locale.

    > [!IMPORTANT]
    > È possibile installare Remote Server Administration Tools per Windows 10 solo nei computer che eseguono Windows 10. Strumenti di amministrazione remota del Server non può essere installati nei computer che eseguono Windows RT 8.1 o altri dispositivi system on chip.

2.  Se si salva il pacchetto di download in una condivisione o in un computer locale, fare doppio clic sul programma di installazione **WindowsTH-KB2693643-x64.msu** o **WindowsTH-KB2693643-x86.msu**, a seconda dell'architettura del computer in cui si desidera installare gli strumenti.

3.  Quando nella finestra di dialogo **Programma di installazione Windows Update autonomo** viene richiesto di installare l'aggiornamento, fare clic su **Sì**.

4.  Leggere e accettare le condizioni di licenza. Fare clic su **Accetto**.

5.  L'installazione richiede alcuni minuti.

##### <a name="to-uninstall-remote-server-administration-tools-for-windows-10-after-rsat-package-install"></a>Per disinstallare Strumenti di amministrazione remota del server per Windows 10 (dopo l'installazione del pacchetto Strumenti di amministrazione remota del server)

1. Sul desktop fare clic su **Start**, quindi su **Tutte le app**, **Sistema Windows**e infine su **Pannello di controllo**.

2. In **Programmi**fare clic su **Disinstalla un programma**.

3. Fare clic su **Visualizza aggiornamenti installati**.

4. Fare clic con il pulsante destro del mouse su **Aggiornamento per Microsoft Windows (KB2693643)** e scegliere **Disinstalla**.

5. Quando viene richiesto di confermare la disinstallazione dell'aggiornamento, fare clic su **Sì**.
   S
   ##### <a name="to-turn-off-specific-tools-after-rsat-package-install"></a>Per disabilitare strumenti specifici (dopo l'installazione del pacchetto Strumenti di amministrazione remota del server)

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

Gli strumenti installati come parte di Strumenti di amministrazione remota del server per Windows 10 possono essere usati per gestire il computer client locale. Indipendentemente dallo strumento eseguito, devi specificare uno o più server remoti in cui eseguire lo strumento. Poiché la maggior parte degli strumenti sono integrati con Server Manager, aggiungere i server remoti che si desidera gestire per il pool di server di gestione Server prima di gestire il server utilizzando gli strumenti di **strumenti** menu. Per altre informazioni su come aggiungere server al pool di server e creare gruppi di server personalizzati, vedere [Aggiungere server a Server Manager](https://go.microsoft.com/fwlink/p/?LinkId=241353) e [Creare e gestire gruppi di server](https://go.microsoft.com/fwlink/?LinkId=247328).

In Strumenti di amministrazione remota del server per Windows 10 tutti gli strumenti di gestione del server basati sulla GUI, ad esempio gli snap-in di MMC e le finestre di dialogo, sono accessibili dal menu **Strumenti** della console di Server Manager. Anche se il computer che esegue Remote Server Administration Tools per Windows 10 esegue un sistema operativo basato su client, dopo l'installazione degli strumenti, Server Manager, incluso in remoto Server Administration Tools per Windows 10, viene aperto automaticamente per impostazione predefinita nel computer client. Si noti che non esiste alcun **Server locale** pagina nella console di Server Manager che viene eseguito in un computer client.

##### <a name="to-start-server-manager-on-a-client-computer"></a>Per avviare Server Manager in un computer client

1.  Nel menu **Start** fare clic su **Tutte le app**e quindi su **Strumenti di amministrazione**.

2.  Nella cartella **Strumenti di amministrazione** fare clic su **Server Manager**.

Anche se non sono elencati nel menu **Strumenti** della console di Server Manager, vengono installati anche i cmdlet di Windows PowerShell e gli strumenti di gestione del prompt dei comandi per i ruoli e le funzionalità come parte di Strumenti di amministrazione remota del server. Se ad esempio apri una sessione di Windows PowerShell con diritti utente elevati (ovvero da Esegui come amministratore) ed esegui il cmdlet `Get-Command -Module RDManagement`, i risultati includono un elenco di cmdlet di Servizi Desktop remoto che ora sono disponibili per l'esecuzione nel computer locale dopo l'installazione di Strumenti di amministrazione remota del server, purché i cmdlet siano destinati a un server remoto che esegue per intero o in parte il ruolo Servizi Desktop remoto.

##### <a name="to-start-windows-powershell-with-elevated-user-rights-run-as-administrator"></a>Per avviare Windows PowerShell con diritti utente elevati (Esegui come amministratore)

1.  Nel menu **Start** fare clic su **Tutte le app**, quindi su **Sistema Windows**e infine su **Windows PowerShell**.

2.  Per eseguire Windows PowerShell come amministratore dal desktop, fare doppio clic sui **Windows PowerShell** scelta rapida e quindi fare clic su **Esegui come amministratore**.

> [!NOTE]
> È inoltre possibile avviare una sessione di Windows PowerShell destinato a un server specifico facendo clic su un server gestito in una pagina di gruppo o ruolo in Server Manager e quindi facendo clic su **Windows PowerShell**.


## <a name="known-issues"></a>Problemi noti

### <a name="issue-rsat-fod-installation-fails-with-error-code-0x800f0954"></a>**Problema**: l'installazione di funzionalità su richiesta di Strumenti di amministrazione remota del server ha esito negativo con codice di errore 0x800f0954

> **Impatto**: le funzionalità su richiesta di Strumenti di amministrazione remota del server in Windows 10 1809 (aggiornamento di ottobre 2018) in ambienti WSUS/Configuration Manager
> 
> **Soluzione**: per installare funzionalità su richiesta in un computer aggiunto a un dominio che riceve gli aggiornamenti tramite WSUS o Configuration Manager, devi modificare un'impostazione di Criteri di gruppo per abilitare il download delle funzionalità su richiesta direttamente da Windows Update o da una condivisione locale. Per altri dettagli e istruzioni su come modificare questa impostazione, vedi [Come rendere disponibili le funzionalità su richiesta e i Language Pack quando si usa WSUS/SCCM](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs).

---

### <a name="issue-rsat-fod-installation-via-settings-app-does-not-show-statusprogress"></a>**Problema**: l'installazione delle funzionalità su richiesta di Strumenti di amministrazione remota del server tramite l'app Impostazioni non mostra lo stato o l'avanzamento

> **Impatto**: le funzionalità su richiesta di Strumenti di amministrazione remota del server in Windows 10 1809 (aggiornamento di ottobre 2018)
> 
> **Soluzione**: per visualizzare lo stato di avanzamento dell'installazione, fai clic sul pulsante **Indietro** per visualizzare lo stato nella pagina **Gestisci funzionalità facoltative**.

---

### <a name="issue-rsat-fod-uninstallation-via-settings-app-may-fail"></a>**Problema**: la disinstallazione di funzionalità su richiesta di Strumenti di amministrazione remota del server tramite l'app Impostazioni potrebbe avere esito negativo

> **Impatto**: le funzionalità su richiesta di Strumenti di amministrazione remota del server in Windows 10 1809 (aggiornamento di ottobre 2018)
> 
> **Soluzione**: in alcuni casi, gli errori di disinstallazione sono dovuti alla necessità di disinstallare manualmente le dipendenze. In particolare, se lo strumento A di Strumenti di amministrazione remota del server è necessario per lo strumento B di Strumenti di amministrazione remota del server, la disinstallazione dello strumento A avrà esito negativo se è ancora installato lo strumento B. In questo caso, disinstalla prima lo strumento B e quindi lo strumento A. Vedi l'elenco delle funzionalità su richiesta di Strumenti di amministrazione remota del server che includono dipendenze.

---

### <a name="issue-rsat-fod-uninstallation-appears-to-succeed-but-the-tool-is-still-installed"></a>**Problema**: la disinstallazione di funzionalità su richiesta di Strumenti di amministrazione remota del server sembra avere esito positivo, ma lo strumento è ancora installato

> **Impatto**: le funzionalità su richiesta di Strumenti di amministrazione remota del server in Windows 10 1809 (aggiornamento di ottobre 2018)
> 
> **Soluzione**: in questo caso, il riavvio del PC completerà la rimozione dello strumento.

---

### <a name="issue-rsat-missing-after-windows-10-upgrade"></a>**Problema**: la funzionalità Strumenti di amministrazione remota del server non è presente dopo l'aggiornamento di Windows 10

> **Impatto**: l'installazione dei pacchetti MSU di Strumenti di amministrazione remota del server (antecedenti alle funzionalità su richiesta di Strumenti di amministrazione remota del server) non viene reinstallata automaticamente
> 
> **Soluzione**: non è possibile salvare in modo permanente un'installazione di Strumenti di amministrazione remota del server da un aggiornamento all'altro del sistema operativo perché il file MSU viene fornito come pacchetto di Windows Update. Installa di nuovo la funzionalità Strumenti di amministrazione remota del server dopo aver aggiornato Windows 10. Tieni presente che questa limitazione è uno dei motivi per cui sono state rese disponibili le funzionalità su richiesta a partire da Windows 10 1809. Le funzionalità su richiesta di Strumenti di amministrazione remota del server verranno mantenute in tutti i prossimi aggiornamenti di versione di Windows 10.

## <a name="see-also"></a>Vedere anche
>- [Strumenti di amministrazione remota del server per Windows 10](https://go.microsoft.com/fwlink/?LinkID=404281)
>- [Strumenti di amministrazione remota del server per Windows Vista, Windows 7, Windows 8, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 e Windows Server 2012 R2](https://go.microsoft.com/fwlink/p/?LinkID=221055)
