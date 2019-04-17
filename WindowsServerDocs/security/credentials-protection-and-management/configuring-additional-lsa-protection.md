---
title: Configurazione della protezione LSA aggiuntiva
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 038e7c2b-c032-491f-8727-6f3f01116ef9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: fcfb0dab10d28413cf4ad06dd583274f217c91fa
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="configuring-additional-lsa-protection"></a>Configurazione della protezione LSA aggiuntiva

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento, destinato ai professionisti IT spiega come configurare protezione aggiuntiva per il processo di autorità di sicurezza locale (LSA) impedire l'inserimento di codice che potrebbe compromettere le credenziali.

LSA, che include il processo di protezione Server servizio LSASS (Local Authority), convalida gli utenti per gli accessi locali e remoti e impone i criteri di sicurezza locali. Il sistema operativo Windows 8.1 offre protezione aggiuntiva per LSA per impedire la lettura e l'inserimento di codice da processi non protetti. Ciò offre protezione aggiuntiva per le credenziali archiviate e gestite in LSA. L'impostazione del processo protetto per LSA può essere configurata in Windows 8.1, ma non può essere configurata in Windows RT 8.1. Quando questa impostazione viene utilizzata in combinazione con avvio protetto, viene ottenuta protezione aggiuntiva perché la disattivazione della chiave del Registro di sistema HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa non ha alcun effetto.

### <a name="protected-process-requirements-for-plug-ins-or-drivers"></a>Requisiti del processo protetto per plug-in o driver
Per un plug-in LSA o driver caricato correttamente come processo protetto, deve soddisfare i criteri seguenti:

1.  Verifica della firma

    La modalità protetta richiede che ogni plug-in che viene caricato in LSA sia firmato digitalmente con una firma Microsoft. Di conseguenza, qualsiasi plug-in che sono firmati oppure non sono firmati con una firma Microsoft avrà esito negativo caricati in LSA. Esempi di questi plug-in sono i driver di smart card, plug-in crittografici e i filtri password.

    I plug-in che sono driver, ad esempio driver di smart card, devono essere firmati mediante la certificazione WHQL. Per ulteriori informazioni, vedere [versione WHQL (driver Windows)](https://msdn.microsoft.com/library/windows/hardware/ff553976%28v=vs.85%29.aspx).

    I plug-in che non dispongono di un processo di certificazione WHQL devono essere firmati mediante il [file servizio di firma per LSA](https://go.microsoft.com/fwlink/?LinkId=392590).

2.  Conformità alle linee guida per il processo di Microsoft Security Development Lifecycle (SDL)

    Tutti i plug-in devono rispettare le linee guida del processo SDL applicabili. Per ulteriori informazioni, vedere il [appendice di Microsoft Security Development Lifecycle (SDL)](https://msdn.microsoft.com/library/windows/desktop/cc307891.aspx).

    Anche se il plug-in sono firmati correttamente con una firma Microsoft, il mancato rispetto del processo SDL può provocare errori di caricare un plug-in.

#### <a name="recommended-practices"></a>Procedure consigliate
Utilizzare l'elenco seguente per verificare che la protezione LSA viene abilitata prima di distribuire ampiamente la funzionalità:

-   Identificare tutti i plug-in LSA e i driver sono in uso all'interno dell'organizzazione. 
    Questo include i driver non Microsoft o plug-in, ad esempio driver di smart card e plug-in crittografici e qualsiasi software sviluppato internamente che viene utilizzato per applicare i filtri password o le notifiche di modifica delle password.

-   Assicurarsi che tutti i plug-in LSA siano firmati digitalmente con un certificato Microsoft in modo che il plug-in non potrà essere caricata.

-   Assicurarsi che tutti i plug-in firmati correttamente vengano caricati in LSA e che funzionino come previsto.

-   Utilizzare i registri di controllo per identificare i plug-in e i driver che non vengono eseguiti come processi protetti.

## <a name="how-to-identify-lsa-plug-ins-and-drivers-that-fail-to-run-as-a-protected-process"></a>Come identificare i plug-in e i driver che non vengono eseguiti come processi protetti
Gli eventi descritti in questa sezione si trovano in operativo registro applicazioni e servizi\microsoft\windows\codeintegrity.. Questi consentono di identificare i plug-in e i driver che non vengono caricati per problemi di firma. Per gestire questi eventi, è possibile utilizzare il **wevtutil** strumento da riga di comando. Per informazioni su questo strumento, vedere [Wevtutil](../../administration/windows-commands/Wevtutil.md).

### <a name="before-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>Prima di acconsentire esplicitamente: come identificare i plug-in e i driver caricati da lsass.exe
È possibile utilizzare la modalità di controllo per identificare i plug-in e i driver che avrà esito negativo per il caricamento nella modalità di protezione LSA. In modalità di controllo, il sistema genera registri eventi, che identifica tutti i plug-in e i driver che non verranno caricato in LSA se è abilitata la protezione LSA. I messaggi vengono registrati senza bloccare il plug-in o driver.

##### <a name="to-enable-the-audit-mode-for-lsassexe-on-a-single-computer-by-editing-the-registry"></a>Per abilitare la modalità di controllo per Lsass.exe in un singolo computer modificando il Registro di sistema

1.  Aprire l'Editor del Registro di sistema di (RegEdit.exe) e passare alla chiave del Registro di sistema che si trova in: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution options\lsass.exe..

2.  Impostare il valore della chiave del Registro di sistema per **AuditLevel = DWORD: 00000008**.

3.  Riavviare il computer.

Analizzare i risultati degli eventi 3065 e 3066.

-   **Evento 3065**: questo evento registra il fatto che l'integrità del codice di un controllo ha rilevato che un processo (in genere lsass.exe) ha tentato di caricare un driver specifico che non soddisfa i requisiti di sicurezza per le sezioni condivise. Tuttavia, a causa di criteri di sistema sono impostato, l'immagine è stato consentito il caricamento.

-   **Evento 3066**: questo evento registra il fatto che l'integrità del codice di un controllo ha rilevato che un processo (in genere lsass.exe) ha tentato di caricare un driver specifico che non soddisfa i requisiti del livello di firma Microsoft. Tuttavia, a causa di criteri di sistema sono impostato, l'immagine è stato consentito il caricamento.

> [!IMPORTANT]
> Questi eventi operativi non vengono generati quando è collegato e abilitato in un sistema a un debugger del kernel.
> 
> Se un plug-in o driver include sezioni condivise, l'evento 3066 viene registrato con l'evento 3065. Rimuovere le sezioni condivise, entrambi gli eventi di impedisce che si verificano, a meno che il plug-in non soddisfa i requisiti del livello di firma Microsoft.

Per abilitare la modalità di controllo per più computer in un dominio, è possibile utilizzare l'estensione lato Client Registro di sistema per criteri di gruppo per distribuire il valore del Registro di sistema a livello di controllo di Lsass.exe. È necessario modificare una chiave del Registro di sistema HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution options\lsass.exe..

##### <a name="to-create-the-auditlevel-value-setting-in-a-gpo"></a>Per creare l'impostazione del valore AuditLevel in un oggetto Criteri di gruppo

1.  Aprire Console Gestione criteri di gruppo (GPMC).

2.  Creare un nuovo oggetto (criteri di gruppo) che è collegato a livello di dominio o che è collegato all'unità organizzativa che contiene gli account computer. Oppure è possibile selezionare un oggetto Criteri di gruppo è già stato distribuito.

3.  Fare doppio clic su oggetto Criteri di gruppo, quindi fare clic su **modifica** per aprire Editor Gestione criteri di gruppo.

4.  Espandere **configurazione Computer**, espandere **preferenze**, quindi espandere **le impostazioni di Windows**.

5.  Fare doppio clic su **Registro di sistema**, scegliere **New**, quindi fare clic su **elemento Registro di sistema**. Il **nuove proprietà Registro di sistema** viene visualizzata la finestra di dialogo.

6.  Nel **Hive** elenco, fare clic su **HKEY_LOCAL_MACHINE.**

7.  Nel **percorso chiave** elenco, passare a **SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution options\lsass.exe.**.

8.  Nel **nome valore** digitare **AuditLevel**.

9. Nel **tipo di valore** casella, fare clic per selezionare il **REG_DWORD**.

10. Nel **dati valore** digitare **00000008**.

11. Fare clic su **OK**.

> [!NOTE]
> Per l'oggetto Criteri di gruppo abbia effetto, la modifica dell'oggetto Criteri di gruppo deve essere replicata in tutti i controller di dominio nel dominio.

Per consenso esplicito per la protezione LSA aggiuntiva in più computer, è possibile utilizzare l'estensione lato Client Registro di sistema per criteri di gruppo modificando HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\LSA.. Per informazioni su come eseguire questa operazione, vedere [come configurare protezione LSA aggiuntiva delle credenziali](#BKMK_HowToConfigure) in questo argomento.

### <a name="after-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>Dopo avere acconsentito esplicitamente: come identificare i plug-in e i driver caricati da lsass.exe
È possibile utilizzare il registro eventi per identificare i plug-in e i driver che non è riuscito a caricare in modalità di protezione LSA. Quando è abilitato il processo protetto di LSA, il sistema genera registri eventi che identificano tutti i plug-in e i driver che non è riuscito a caricare in LSA.

Analizzare i risultati degli eventi 3033 e 3063.

-   **Evento 3033**: questo evento registra il fatto che l'integrità del codice di un controllo ha rilevato che un processo (in genere lsass.exe) ha tentato di caricare un driver che non soddisfa i requisiti del livello di firma Microsoft.

-   **Evento 3063**: questo evento registra il fatto che l'integrità del codice di un controllo ha rilevato che un processo (in genere lsass.exe) ha tentato di caricare un driver che non soddisfa i requisiti di sicurezza per le sezioni condivise.

Sezioni condivise sono in genere il risultato di tecniche di programmazione che consentono di dati dell'istanza di interagire con altri processi che utilizzano lo stesso contesto di sicurezza. Ciò può creare vulnerabilità di sicurezza.

## <a name="BKMK_HowToConfigure"></a>Come configurare protezione LSA aggiuntiva delle credenziali
Nei dispositivi che eseguono Windows 8.1 (con o senza avvio protetto o UEFI), è possibile configurazione eseguendo le procedure descritte in questa sezione. Per i dispositivi che eseguono Windows RT 8.1, protezione di lsass.exe è sempre abilitata e non può essere disattivata.

### <a name="on-x86-based-or-x64-based-devices-using-secure-boot-and-uefi-or-not"></a>In dispositivi basati su x86 o x64 con avvio protetto e UEFI o non
Nei dispositivi basati su x86 o x64 che utilizzano l'avvio protetto e UEFI, una variabile UEFI è impostata nel firmware UEFI quando è abilitata la protezione LSA utilizzando la chiave del Registro di sistema. Quando l'impostazione viene archiviata nel firmware, la variabile UEFI non può essere eliminata né modificata nella chiave del Registro di sistema. La variabile UEFI deve essere reimpostata.

I dispositivi basati su x86 o x64 che non supportano l'avvio protetto o UEFI sono disabilitati, non può archiviare la configurazione per la protezione LSA nel firmware e fare affidamento esclusivamente sulla presenza della chiave del Registro di sistema. In questo scenario, è possibile disabilitare la protezione LSA utilizzando l'accesso remoto al dispositivo.

Per abilitare o disabilitare la protezione LSA, è possibile utilizzare le procedure seguenti:

##### <a name="to-enable-lsa-protection-on-a-single-computer"></a>Per abilitare la protezione LSA in un singolo computer

1.  Aprire l'Editor del Registro di sistema di (RegEdit.exe) e passare alla chiave del Registro di sistema che si trova in: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa.

2.  Impostare il valore della chiave del Registro di sistema: "RunAsPPL" = DWORD: 00000001.

3.  Riavviare il computer.

##### <a name="to-enable-lsa-protection-using-group-policy"></a>Per abilitare la protezione LSA mediante criteri di gruppo

1.  Aprire Console Gestione criteri di gruppo (GPMC).

2.  Creare un nuovo GPO collegato a livello di dominio o che è collegato all'unità organizzativa che contiene gli account computer. Oppure è possibile selezionare un oggetto Criteri di gruppo è già stato distribuito.

3.  Fare doppio clic su oggetto Criteri di gruppo, quindi fare clic su **modifica** per aprire Editor Gestione criteri di gruppo.

4.  Espandere **configurazione Computer**, espandere **preferenze**, quindi espandere **le impostazioni di Windows**.

5.  Fare doppio clic su **Registro di sistema**, scegliere **New**, quindi fare clic su **elemento Registro di sistema**. Il **nuove proprietà Registro di sistema** viene visualizzata la finestra di dialogo.

6.  Nel **Hive** elenco, fare clic su **HKEY_LOCAL_MACHINE**.

7.  Nel **percorso chiave** elenco, passare a **SYSTEM\CurrentControlSet\Control\Lsa**.

8.  Nel **nome valore** digitare **RunAsPPL**.

9. Nel **tipo di valore**, fare clic il **REG_DWORD**.

10. Nel **dati valore** digitare **00000001**.

11. Fare clic su **OK**.

##### <a name="to-disable-lsa-protection"></a>Per disabilitare la protezione LSA

1.  Aprire l'Editor del Registro di sistema di (RegEdit.exe) e passare alla chiave del Registro di sistema che si trova in: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa.

2.  Eliminare il valore seguente dalla chiave del Registro di sistema: "RunAsPPL" = DWORD: 00000001.

3.  Utilizzare lo strumento di autorità di sicurezza locale (LSA) Protected Process Opt-out per eliminare la variabile UEFI se il dispositivo utilizza l'avvio protetto.

    Per ulteriori informazioni sullo strumento rifiuto esplicito, vedere [Download di autorità protezione locale (LSA) Protected rifiuto esplicito del processo nell'area Download Microsoft](https://www.microsoft.com/download/details.aspx?id=40897).

    Per ulteriori informazioni sulla gestione dell'avvio protetto, vedere [Firmware UEFI](https://technet.microsoft.com/library/hh824898.aspx).

    > [!WARNING]
    > Quando l'avvio protetto è disattivato, tutti gli avvio protetto e configurazioni correlate a UEFI vengono reimpostate. È consigliabile disattivare l'avvio protetto solo quando tutti gli altri mezzi per disabilitare la protezione LSA non sono riuscita.

### <a name="verifying-lsa-protection"></a>Verifica della protezione LSA
Per individuare se LSA è stato avviato in modalità protetta all'avvio di Windows, cercare l'evento WinInit seguente nel **sistema** accedere in **registri di Windows**:

-   12: LSASS.exe è stato avviato come processo protetto con il livello: 4

## <a name="additional-resources"></a>Risorse aggiuntive
[Gestione e protezione delle credenziali](credentials-protection-and-management.md)

[Servizio di firma per LSA file](https://go.microsoft.com/fwlink/?LinkId=392590)


