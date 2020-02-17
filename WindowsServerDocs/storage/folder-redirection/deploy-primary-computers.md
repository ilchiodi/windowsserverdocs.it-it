---
title: Distribuire i computer primari per Reindirizzamento cartelle e Profili utente mobili
description: Come abilitare il supporto dei computer primari e designare i computer primari per gli utenti con Reindirizzamento cartelle e Profili utente mobili.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: be2b41cf32e2020422c32415e2d8f4273eb09859
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394442"
---
# <a name="deploy-primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Distribuire i computer primari per Reindirizzamento cartelle e Profili utente mobili

>Si applica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2

Questo argomento descrive come abilitare il supporto dei computer primari e come designare i computer primari per gli utenti. In questo modo è possibile controllare quali computer usano Reindirizzamento cartelle e Profili utente mobili.

> [!IMPORTANT]
> Quando abiliti il supporto dei computer primari per Profili utente mobili, abilita sempre il supporto dei computer primari anche per Reindirizzamento cartelle. In questo modo, i documenti e altri file degli utenti vengono mantenuti fuori dai profili utente, che quindi rimangono di piccole dimensioni e consentono tempi di accesso rapidi.

## <a name="prerequisites"></a>Prerequisiti

## <a name="software-requirements"></a>Requisiti software

Il supporto dei computer primari prevede i requisiti seguenti:

- Lo schema di Active Directory Domain Services deve essere aggiornato in modo da includere le aggiunte per lo schema di Windows Server 2012. Lo schema viene aggiornato automaticamente con l'installazione di un controller di dominio di Windows Server 2012. Per informazioni sull'aggiornamento dello schema di Active Directory Domain Services, vedi [Integrazione di Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh472161(v=ws.11)#adprepexe-integration>) ed [Esecuzione di Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)>).
- I computer client devono eseguire Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.

> [!TIP]
> Benché il supporto dei computer primari richieda Reindirizzamento cartelle e/o Profili utente mobili, se distribuisci queste tecnologie per la prima volta, è preferibile configurare tale supporto prima di abilitare gli oggetti Criteri di gruppo che configurano Reindirizzamento cartelle e Profili utente mobili. In tal modo, i dati utente non verranno copiati nei computer non primari prima di aver abilitato il supporto del computer primario. Per informazioni sulla configurazione, vedi [Distribuire Reindirizzamento cartelle](deploy-folder-redirection.md) e [Distribuire Profili utente mobili](deploy-roaming-user-profiles.md).

## <a name="step-1-designate-primary-computers-for-users"></a>Passaggio 1: Designare i computer primari per gli utenti

Il primo passaggio della distribuzione del supporto dei computer primari è la designazione dei computer primari per ogni utente. A tale scopo, usa Centro amministrativo di Active Directory per ottenere il nome distinto dei computer rilevanti e quindi imposta l'attributo **msDs-PrimaryComputer**.

> [!TIP]
> Per usare Windows PowerShell per operare con i computer primari, vedi il post di blog [Digging a little deeper into Windows 8 Primary Computer](<https://blogs.technet.microsoft.com/askds/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer/>) (Approfondimenti sul computer primario Windows 8).

Di seguito viene illustrato come specificare i computer primari per gli utenti:

1. Aprire Server Manager in un computer in cui sono installati gli strumenti di amministrazione di Active Directory.
2. Scegli **Centro amministrativo di Active Directory** dal menu **Strumenti**. Verrà visualizzato Centro amministrativo di Active Directory.
3. Passa al contenitore **Computer** del dominio appropriato.
4. Fai clic con il pulsante destro del mouse su un computer che vuoi designare come computer primario e scegli **proprietà**.
5. Nel riquadro di spostamento seleziona **Estensioni**.
6. Fai clic sulla scheda **Editor attributi**, scorri fino a **distinguishedName**, seleziona **Visualizza**, fai clic con il pulsante destro del mouse sul valore elencato e scegli **Copia**, quindi scegli **OK** e infine **Annulla**.
7. Passa al contenitore **Utenti** del dominio appropriato, fai clic con il pulsante destro del mouse sull'utente a cui vuoi assegnare il computer e quindi scegli **Proprietà**.
8. Nel riquadro di spostamento seleziona **Estensioni**.
9. Fai clic sulla scheda **Editor attributi**, seleziona **msDs-PrimaryComputer** e quindi scegli **Modifica**. Verrà visualizzata la finestra di dialogo Editor stringa multivalore.
10. Fai clic con il pulsante destro del mouse sulla casella di testo e scegli **Incolla**, quindi scegli **Aggiungi**, **OK** e infine ancora **OK**.

## <a name="step-2-optionally-enable-primary-computers-for-folder-redirection-in-group-policy"></a>Passaggio 2: Abilitare facoltativamente i computer primari per Reindirizzamento cartelle in Criteri di gruppo

Il passaggio successivo consiste nel configurare facoltativamente Criteri di gruppo per abilitare il supporto dei computer primari per Reindirizzamento cartelle. In questo modo, le cartelle di un utente vengono reindirizzate nei computer designati come computer primari dell'utente, ma non in altri computer. Puoi controllare i computer primari per Reindirizzamento cartelle per i singoli computer o i singoli utenti.

Di seguito viene illustrato come abilitare i computer primari per Reindirizzamento cartelle:

1. In Gestione Criteri di gruppo fai clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo creato durante la configurazione iniziale di Reindirizzamento cartelle e/o Profili utente mobili (ad esempio, **Impostazioni di reindirizzamento cartelle** o **Impostazioni dei profili utente mobili**) e quindi scegli **Modifica**.
2. Per abilitare il supporto dei computer primari usando Criteri di gruppo basati su computer, passa a **Configurazione computer**. Per Criteri di gruppo basati su utenti, passa a **Configurazione utente**.
    - Con Criteri di gruppo basati su computer è possibile applicare l'elaborazione del computer primario a tutti i computer a cui si applica l'oggetto Criteri di gruppo, influendo su tutti gli utenti dei computer.
    - Con Criteri di gruppo basati su utenti è possibile applicare l'elaborazione del computer primario a tutti gli account utente a cui si applica l'oggetto Criteri di gruppo, influendo su tutti i computer a cui accedono gli utenti.
3. In **Configurazione computer** o **Configurazione utente** passa a **Criteri**, **Modelli amministrativi**, **Sistema** e quindi **Reindirizzamento cartelle**.
4. Fai clic con il pulsante destro del mouse su **Reindirizza cartelle solo nei computer primari** e quindi scegli **Modifica**.
5. Seleziona **Abilitato** e quindi fai clic su **OK**.

## <a name="step-3-optionally-enable-primary-computers-for-roaming-user-profiles-in-group-policy"></a>Passaggio 3: Abilitare facoltativamente i computer primari per Profili utente mobili in Criteri di gruppo

Il passaggio successivo consiste nel configurare facoltativamente Criteri di gruppo per abilitare il supporto dei computer primari per Profili utente mobili. In questo modo, viene effettuato il roaming del profilo di un utente nei computer designati come computer primari dell'utente, ma non in altri computer.

Di seguito viene illustrato come abilitare i computer primari per Profili utente mobili:

1. Abilita il supporto dei computer primari per Reindirizzamento cartelle, se non hai ancora provveduto.<br>In questo modo, i documenti e altri file degli utenti vengono mantenuti fuori dai profili utente, che quindi rimangono di piccole dimensioni e consentono tempi di accesso rapidi.
2. In Gestione Criteri di gruppo fai clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo creato (ad esempio, **Impostazioni di reindirizzamento cartelle e Impostazioni dei profili utente mobili**) e quindi scegli **Modifica**.
3. Passa a **Configurazione computer**, **Criteri**, **Modelli amministrativi**, **Sistema**e quindi **Profili utente**.
4. Fai clic con il pulsante destro del mouse su **Scarica profili mobili solo nei computer primari** e quindi scegli **Modifica**.
5. Seleziona **Abilitato** e quindi fai clic su **OK**.

## <a name="step-4-enable-the-gpo"></a>Passaggio 4: Abilitare l'oggetto Criteri di gruppo

Dopo aver completato la configurazione di Reindirizzamento cartelle e Profili utente mobili, abilita l'oggetto Criteri di gruppo, se non è già stato fatto. In questo modo, l'oggetto può essere applicato agli utenti e ai computer interessati.

Di seguito viene illustrato come abilitare gli oggetti Criteri di gruppo di Reindirizzamento cartelle e/o Profili utente mobili:

1. Apri Gestione Criteri di gruppo.
2. Fai clic con il pulsante destro del mouse sugli oggetti Criteri di gruppo creati e scegli **Collegamento abilitato**. Accanto alla voce del menu deve venire visualizzata una casella di controllo.

## <a name="step-5-test-primary-computer-function"></a>Passaggio 5: Testare la funzionalità dei computer primari

Per testare il supporto dei computer primari, accedi a un computer primario, verifica che le cartelle e i profili vengano reindirizzati, quindi accedi a un computer non primario e verifica che le cartelle e i profili non vengano reindirizzati.

Di seguito viene illustrato come testare la funzionalità dei computer primari:

1. Accedi a un computer primario designato con un account utente per il quale hai abilitato Reindirizzamento cartelle e/o Profili utente mobili.
2. Se l'account utente ha eseguito l'accesso al computer in precedenza, apri una sessione di Windows PowerShell o una finestra del prompt dei comandi come amministratore, digita il comando seguente e quindi disconnettiti quando viene richiesto di verificare che le impostazioni di Criteri di gruppo più recenti siano state applicate al computer client:

    ```PowerShell
    Gpupdate /force
    ```

3. Aprire Esplora file.
1. Fai clic con il pulsante destro del mouse su una cartella reindirizzata (ad esempio, la cartella Documenti nella raccolta Documenti) e quindi scegli **Proprietà**.
1. Fai clic sulla scheda **Posizione** e verifica che come percorso venga visualizzata la condivisione file specificata anziché un percorso locale. Per verificare l'esecuzione del roaming del profilo utente, apri il **Pannello di controllo**, seleziona **Sistema e sicurezza**, **Sistema**, **Impostazioni di sistema avanzate**, **Impostazioni** nella sezione Profili utente e quindi cerca **Roaming** nella colonna **Tipo**.
1. Accedi con lo stesso account utente a un computer non designato come computer primario dell'utente.
1. Ripeti i passaggi 2-5 invece di cercare percorsi locali e un tipo di profilo **Locale**.

> [!NOTE]
> Se le cartelle sono state reindirizzate in un computer prima che abilitassi il supporto dei computer primari, le cartelle rimarranno reindirizzate, a meno che non sia configurata l'impostazione seguente nell'impostazione criterio di reindirizzamento cartelle di ogni cartella: **Reindirizza la cartella nel percorso del profilo utente locale quando i criteri vengono rimossi**. Analogamente, per i profili di cui precedentemente è stato effettuato il roaming in un determinato computer verrà visualizzato **Roaming** nelle colonne **Tipo**. Nella colonna **Stato** verrà tuttavia visualizzato **Locale**.

## <a name="more-information"></a>Ulteriori informazioni

- [Distribuire Reindirizzamento cartelle con File offline](deploy-folder-redirection.md)
- [Distribuire profili utente mobili](deploy-roaming-user-profiles.md)
- [Panoramica di Reindirizzamento cartelle, File offline e Profili utente mobili](folder-redirection-rup-overview.md)
- [Digging a little deeper into Windows 8 Primary Computer](https://blogs.technet.com/b/askds/archive/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer.aspx) (Approfondimenti sul computer primario Windows 8)