---
title: Configurazione di protezione LSA aggiuntiva
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: eaebac19119525b659c09b5506c497afdbd9a263
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386994"
---
# <a name="configuring-additional-lsa-protection"></a>Configurazione di protezione LSA aggiuntiva

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento per professionisti IT viene illustrato come configurare protezione aggiuntiva per il processo dell'autorità di sicurezza locale (LSA) per impedire l'inserimento di codice che potrebbe compromettere le credenziali.

LSA, che include il processo del servizio server dell'autorità di sicurezza locale (LSASS), convalida gli utenti per gli accessi locali e remoti e impone i criteri di sicurezza locali. Il sistema operativo Windows 8.1 offre protezione aggiuntiva per LSA per impedire la lettura di memoria e l'inserimento di codice da processi non protetti. In questo modo le credenziali archiviate e gestite in LSA sono più sicure. L'impostazione del processo protetto per LSA può essere configurata in Windows 8.1, ma non può essere configurata in Windows RT 8,1. Quando questa impostazione viene utilizzata insieme all'avvio protetto, viene raggiunto un livello di protezione maggiore perché la disabilitazione della chiave del Registro di sistema HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa non ha alcun effetto.

### <a name="protected-process-requirements-for-plug-ins-or-drivers"></a>Requisiti del processo protetto per plug-in o driver
Affinché un plug-in o un driver LSA venga caricato correttamente come processo protetto, deve soddisfare i criteri seguenti:

1.  Verifica della firma

    La modalità protetta richiede che ogni plug-in che viene caricato in LSA sia firmato digitalmente con una firma Microsoft. Tutti i plug-in che non sono firmati oppure che sono firmati con una firma non Microsoft non verranno caricati in LSA. Esempi di questi plug-in sono i driver delle smart card, i plug-in crittografici e i filtri password.

    I plug-in che sono driver, come i driver delle smart card, devono essere firmati mediante la certificazione WHQL. Per ulteriori informazioni, vedere [firma della versione WHQL](https://msdn.microsoft.com/library/windows/hardware/ff553976%28v=vs.85%29.aspx).

    I plug-in che non dispongono di un processo di certificazione WHQL devono essere firmati mediante il [servizio di firma file per LSA](https://go.microsoft.com/fwlink/?LinkId=392590).

2.  Rispetto delle linee guida del processo Microsoft Security Development Lifecycle (SDL)

    Tutti i plug-in devono rispettare le linee guida del processo SDL applicabili. Per ulteriori informazioni, vedere l' [appendice relativa a Microsoft Security Development Lifecycle (SDL)](https://msdn.microsoft.com/library/windows/desktop/cc307891.aspx).

    Anche se i plug-in sono firmati correttamente con una firma Microsoft, il mancato rispetto del processo SDL può provocare errori di caricamento dei plug-in.

#### <a name="recommended-practices"></a>Procedure consigliate
Utilizzare l'elenco seguente per verificare che la protezione LSA sia abilitata prima di distribuire ampiamente la funzionalità:

-   Identificare tutti i plug-in e i driver LSA in uso nella propria organizzazione, 
    inclusi i driver e i plug-in non Microsoft come i driver delle smart card e plug-in crittografici, nonché eventuale software sviluppato internamente che viene utilizzato per imporre i filtri password o le notifiche di modifica della password.

-   Verificare che tutti i plug-in LSA siano firmati digitalmente con un certificato Microsoft per impedire che si verifichino errori di caricamento dei plug-in.

-   Assicurarsi che tutti i plug-in firmati correttamente vengano caricati in LSA e che funzionino come previsto.

-   Utilizzare i log di controllo per identificare i plug-in e i driver LSA che non vengono eseguiti come processi protetti.

#### <a name="limitations-introduced-with-enabled-lsa-protection"></a>Limitazioni introdotte con la protezione LSA abilitata

Se è abilitata la protezione LSA, non è possibile eseguire il debug di un plug-in LSA personalizzato.
Non è possibile aggiungere un debugger a LSASS quando si tratta di un processo protetto.
In generale, non esiste alcun modo supportato per eseguire il debug di un processo protetto in esecuzione.

## <a name="how-to-identify-lsa-plug-ins-and-drivers-that-fail-to-run-as-a-protected-process"></a>Come identificare i plug-in e i driver LSA che non vengono eseguiti come processi protetti
Gli eventi descritti in questa sezione sono disponibili nel log operativo in Registri applicazioni e servizi\Microsoft\Windows\CodeIntegrity. Tali eventi consentono di identificare i plug-in e i driver LSA che non vengono caricati per problemi di firma. Per gestire questi eventi, è possibile utilizzare lo strumento da riga di comando **wevtutil**. Per informazioni su questo strumento, vedere [Wevtutil](../../administration/windows-commands/Wevtutil.md).

### <a name="before-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>Prima di acconsentire esplicitamente: come identificare i plug-in e i driver caricati da lsass.exe
È possibile utilizzare la modalità di controllo per identificare i plug-in e i driver LSA il cui caricamento non potrà essere eseguito in modalità di protezione LSA. In modalità di controllo il sistema genererà registri eventi che consentiranno di identificare tutti i plug-in e i driver il cui caricamento non verrà eseguito in presenza di LSA se è abilitata la protezione LSA. I messaggi vengono registrati senza bloccare i plug-in e i driver.

##### <a name="to-enable-the-audit-mode-for-lsassexe-on-a-single-computer-by-editing-the-registry"></a>Per abilitare la modalità di controllo per Lsass.exe in un singolo computer modificando il Registro di sistema

1.  Aprire l'Editor del Registro di sistema (RegEdit.exe) e passare alla chiave che si trova in: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\LSASS.exe.

2.  Impostare il valore di questa chiave del Registro di sistema su **AuditLevel=dword:00000008**.

3.  Riavviare il computer.

Analizzare i risultati degli eventi 3065 e 3066.

Successivamente, è possibile che vengano visualizzati questi eventi in Visualizzatore eventi: Microsoft-Windows-CodeIntegrity/Operational:

-   **Evento 3065**: questo evento registra il fatto che un controllo dell'integrità del codice ha rilevato che un processo (in genere lsass.exe) ha tentato di caricare un driver specifico che non soddisfa i requisiti di sicurezza per le sezioni condivise. A causa dei criteri di sistema impostati, è stato comunque consentito il caricamento dell'immagine.

-   **Evento 3066**: questo evento registra il fatto che un controllo dell'integrità del codice ha rilevato che un processo (in genere lsass.exe) ha tentato di caricare un driver specifico che non soddisfa i requisiti del livello di firma Microsoft. A causa dei criteri di sistema impostati, è stato comunque consentito il caricamento dell'immagine.

> [!IMPORTANT]
> Questi eventi operativi non vengono generati quando in un sistema è collegato e abilitato un debugger del kernel.
> 
> Se un plug-in o un driver include sezioni condivise, l'evento 3066 viene registrato con l'evento 3065. Se si rimuovono le sezioni condivise, questi eventi non si verificano, a meno che il plug-in non soddisfi i requisiti del livello di firma Microsoft.

Per abilitare la modalità di controllo per più computer in un dominio, è possibile utilizzare le estensioni del Registro di sistema lato client per Criteri di gruppo per distribuire il valore del Registro di sistema a livello di controllo di Lsass.exe. È necessario modificare la chiave del Registro di sistema HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\LSASS.exe.

##### <a name="to-create-the-auditlevel-value-setting-in-a-gpo"></a>Per creare l'impostazione del valore AuditLevel in un oggetto Criteri di gruppo

1.  Aprire Console Gestione Criteri di gruppo.

2.  Creare un nuovo oggetto Criteri di gruppo collegato a livello di dominio oppure all'unità organizzativa che contiene gli account computer. In alternativa, è possibile selezionare un oggetto Criteri di gruppo già distribuito.

3.  Fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo e quindi scegliere **Modifica** per aprire Editor Gestione Criteri di gruppo.

4.  Espandere **Configurazione computer**, **Preferenze** e quindi **Impostazioni di Windows**.

5.  Fare clic con il pulsante destro del mouse su **Registro di sistema**, scegliere **Nuovo** e quindi fare clic su **Elemento Registro di sistema**. Verrà visualizzata la finestra di dialogo **Nuove proprietà Registro di sistema**.

6.  Nell'elenco **hive** fare clic su **HKEY_LOCAL_MACHINE.**

7.  Nell'elenco **Percorso chiave** passare a **SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\LSASS.exe**.

8.  Nella casella **Nome valore** digitare **AuditLevel**.

9. Nella casella **Tipo valore** fare clic per selezionare **REG_DWORD**.

10. Nella casella **Dati valore** digitare **00000008**.

11. Fare clic su **OK**.

> [!NOTE]
> Affinché l'oggetto Criteri di gruppo abbia effetto, la modifica di tale oggetto deve essere replicata in tutti i controller di dominio del dominio.

Per il consenso esplicito della protezione LSA aggiuntiva in più computer, è possibile utilizzare l'estensione del Registro di sistema lato client per Criteri di gruppo modificando HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa. Per i passaggi necessari per eseguire questa operazione, vedere [Come configurare protezione LSA aggiuntiva delle credenziali](#BKMK_HowToConfigure) in questo argomento.

### <a name="after-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>Dopo avere acconsentito esplicitamente: come identificare i plug-in e i driver caricati da lsass.exe
È possibile utilizzare il registro eventi per identificare i plug-in e i driver LSA il cui caricamento non è stato eseguito in modalità di protezione LSA. Quando è abilitato il processo protetto di LSA, il sistema genera registri eventi che identificano tutti i plug-in e i driver il cui caricamento non è riuscito in presenza di LSA.

Analizzare i risultati degli eventi 3033 e 3063.

Successivamente, è possibile che vengano visualizzati questi eventi in Visualizzatore eventi: Microsoft-Windows-CodeIntegrity/Operational:

-   **Evento 3033**: questo evento registra il fatto che un controllo dell'integrità del codice ha rilevato che un processo (in genere lsass.exe) ha tentato di caricare un driver che non soddisfa i requisiti del livello di firma Microsoft.

-   **Evento 3063**: questo evento registra il fatto che un controllo dell'integrità del codice ha rilevato che un processo (in genere lsass.exe) ha tentato di caricare un driver che non soddisfa i requisiti di sicurezza per le sezioni condivise.

Le sezioni condivise sono in genere il risultato di tecniche di programmazione che consentono ai dati dell'istanza di interagire con altri processi che utilizzano lo stesso contesto di sicurezza. Questo può creare vulnerabilità di sicurezza.

## <a name="BKMK_HowToConfigure"></a>Come configurare la protezione LSA aggiuntiva delle credenziali
Nei dispositivi che eseguono Windows 8.1 (con o senza avvio protetto o UEFI), è possibile eseguire la configurazione eseguendo le procedure descritte in questa sezione. Per i dispositivi che eseguono Windows RT 8,1, la protezione di Lsass. exe è sempre abilitata e non può essere disattivata.

### <a name="on-x86-based-or-x64-based-devices-using-secure-boot-and-uefi-or-not"></a>Utilizzo dell'avvio protetto e di UEFI su dispositivi basati su x86 o x64
Se la protezione LSA viene abilitata mediante la chiave del Registro di sistema, sui dispositivi basati su x86 o x64 che utilizzano l'avvio protetto e UEFI viene impostata una variabile UEFI nel firmware UEFI. Quando l'impostazione viene archiviata nel firmware, la variabile UEFI non può essere eliminata o modificata nella chiave del Registro di sistema. La variabile UEFI deve essere reimpostata.

Nei dispositivi basati su x86 o x64 che non supportano che UEFI o l'avvio protetto sia disabilitato, non è possibile archiviare la configurazione per la protezione LSA nel firmware e viene utilizzata solo la chiave del Registro di sistema. In questo scenario è possibile disabilitare la protezione LSA utilizzando l'accesso remoto al dispositivo.

Per abilitare o disabilitare la protezione LSA, è possibile utilizzare le procedure seguenti:

##### <a name="to-enable-lsa-protection-on-a-single-computer"></a>Per abilitare la protezione LSA in un singolo computer

1.  Aprire l'Editor del Registro di sistema (RegEdit.exe) e passare alla chiave che si trova in: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa.

2.  Impostare il valore di questa chiave del Registro di sistema su: "RunAsPPL"=dword:00000001.

3.  Riavviare il computer.

##### <a name="to-enable-lsa-protection-using-group-policy"></a>Per abilitare la protezione LSA mediante Criteri di gruppo

1.  Aprire Console Gestione Criteri di gruppo.

2.  Creare un nuovo oggetto Criteri di gruppo collegato a livello di dominio oppure all'unità organizzativa che contiene gli account computer. In alternativa, è possibile selezionare un oggetto Criteri di gruppo già distribuito.

3.  Fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo e quindi scegliere **Modifica** per aprire Editor Gestione Criteri di gruppo.

4.  Espandere **Configurazione computer**, **Preferenze** e quindi **Impostazioni di Windows**.

5.  Fare clic con il pulsante destro del mouse su **Registro di sistema**, scegliere **Nuovo** e quindi fare clic su **Elemento Registro di sistema**. Verrà visualizzata la finestra di dialogo **Nuove proprietà Registro di sistema**.

6.  Nell'elenco **Hive** fare clic su **HKEY_LOCAL_MACHINE**.

7.  Nell'elenco **percorso chiave** passare a **SYSTEM\CurrentControlSet\Control\Lsa**.

8.  Nella casella **nome valore** digitare **RunAsPPL**.

9. Nella casella **Tipo valore** fare clic su **REG_DWORD**.

10. Nella casella **dati valore** Digitare **00000001**.

11. Fare clic su **OK**.

##### <a name="to-disable-lsa-protection"></a>Per disabilitare la protezione LSA

1.  Aprire l'Editor del Registro di sistema (RegEdit.exe) e passare alla chiave che si trova in: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa.

2.  Eliminare il valore seguente dalla chiave del Registro di sistema: "RunAsPPL"=dword:00000001.

3.  Se il dispositivo usa l'avvio protetto, usare lo strumento per il rifiuto esplicito del processo protetto LSA per eliminare la variabile UEFI.

    Per ulteriori informazioni sullo strumento per il rifiuto esplicito, vedere la [pagina per il download dello strumento dall'Area download Microsoft](https://www.microsoft.com/download/details.aspx?id=40897).

    Per ulteriori informazioni sulla gestione dell'avvio protetto, vedere [Firmware UEFI](https://technet.microsoft.com/library/hh824898.aspx).

    > [!WARNING]
    > Se l'avvio protetto è disattivato, tutte le configurazioni correlate all'avvio protetto e a UEFI vengono reimpostate. È consigliabile disattivare l'avvio protetto solo quando non è possibile disabilitare la protezione LSA utilizzando le altre modalità.

### <a name="verifying-lsa-protection"></a>Verifica della protezione LSA
Per verificare se LSA è stato avviato in modalità protetta all'avvio di Windows, cercare l'evento WinInit seguente nel registro **Sistema** in **Registri di Windows**:

-   12: LSASS.exe è stato eseguito come processo protetto con il livello: 4

## <a name="additional-resources"></a>Risorse aggiuntive
[Gestione e protezione delle credenziali](credentials-protection-and-management.md)

[Servizio di firma file per LSA](https://go.microsoft.com/fwlink/?LinkId=392590)


