---
title: Distribuire computer primari per il reindirizzamento delle cartelle e profili utente mobili
description: Come abilitare il supporto di computer principale e designare i computer primari per gli utenti con il reindirizzamento delle cartelle e profili utente mobili.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7b3c87597e07102d00fc068b7ecd5744e4ba366f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854012"
---
# <a name="deploy-primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Distribuire computer primari per il reindirizzamento delle cartelle e profili utente mobili

>Si applica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

Questo argomento descrive come abilitare il supporto di computer principale e designare i computer primari per gli utenti. In questo modo consente di controllare i computer in cui utilizzano il reindirizzamento delle cartelle e profili utente mobili.

>[!IMPORTANT]
>Quando si abilita supporto del computer primario per i profili utente mobili, abilitare sempre supporto dei computer primari per il reindirizzamento cartelle anche. In questo modo i documenti e altri file di utente all'esterno di profili utente mobili, che consente di profili di rimangano ridotte e tempi di accesso rimangono fast.

## <a name="prerequisites"></a>Prerequisiti

## <a name="software-requirements"></a>Requisiti software

Supporto del computer primario presenta i requisiti seguenti:

- Lo schema di Active Directory Domain Services (AD DS) deve essere aggiornato per includere le aggiunte allo schema di Windows Server 2012 (installazione di un controller di dominio di Windows Server 2012 automaticamente Aggiorna lo schema). Per informazioni sull'aggiornamento dello schema di Active Directory Domain Services, vedere [integrazione Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh472161(v=ws.11)#adprepexe-integration>) e [esecuzione Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)>).
- I computer client devono eseguire Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.

>[!TIP]
>Anche se il supporto di computer primario richiede il reindirizzamento delle cartelle e/o i profili utente mobili, se si distribuiscono queste tecnologie per la prima volta, è consigliabile impostare il supporto di computer primari prima di abilitare oggetti Criteri di gruppo che consentono di configurare il reindirizzamento delle cartelle e I profili utente mobili. In tal modo, i dati utente non verranno copiati nei computer non primari prima di aver abilitato il supporto del computer primario. Per informazioni sulla configurazione, vedere [distribuire reindirizzamento cartelle](deploy-folder-redirection.md) e [distribuire i profili utente mobili](deploy-roaming-user-profiles.md).

## <a name="step-1-designate-primary-computers-for-users"></a>Passaggio 1: Designare i computer primari per gli utenti

Il primo passaggio della distribuzione di supporto per i computer primari viene designare i computer primari per ogni utente. A tale scopo, usare il centro di amministrazione di Active Directory per ottenere il nome distinto del computer in questione e quindi impostare il **msDs-PrimaryComputer** attributo.

>[!TIP]
>Per usare Windows PowerShell per lavorare con i computer primari, vedere il post di blog [approfondita entrare nel merito del Computer primario con Windows 8](<https://blogs.technet.microsoft.com/askds/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer/>).

Di seguito viene illustrato come specificare i computer primari per gli utenti:

1. Aprire Server Manager in un computer in cui sono installati gli strumenti di amministrazione di Active Directory.
2. Nel **degli strumenti** dal menu **centro di amministrazione di Active Directory**. Verrà visualizzato Centro amministrativo di Active Directory.
3. Passare il **computer** contenitore del dominio appropriato.
4. Fare doppio clic su un computer che si desidera designare come computer primario e quindi selezionare **proprietà**.
5. Nel riquadro di spostamento, selezionare **estensioni**.
6. Selezionare il **Editor attributi** scheda, scorrere fino a **distinguishedName**, selezionare **visualizzazione**, fare doppio clic su valore elencato, selezionare **copia**, Selezionare **OK**, quindi selezionare **Annulla**.
7. Passare il **gli utenti** contenitore del dominio appropriato, fare doppio clic sull'utente a cui si desidera assegnare i computer e quindi selezionare **proprietà**.
8. Nel riquadro di spostamento, selezionare **estensioni**.
9. Selezionare il **Editor attributi** scheda, seleziona **msDs-PrimaryComputer** e quindi selezionare **Edit**. Verrà visualizzata la finestra di dialogo Editor stringa multivalore.
10. Fare clic su casella di testo, selezionare **Incolla**, selezionare **Add**, selezionare **OK**, quindi selezionare **OK** nuovamente.

## <a name="step-2-optionally-enable-primary-computers-for-folder-redirection-in-group-policy"></a>Passaggio 2: Se lo si desidera abilitare i computer primari per il reindirizzamento delle cartelle in Criteri di gruppo

Il passaggio successivo consiste nel configurare facoltativamente criteri di gruppo per abilitare il supporto di computer primario per il reindirizzamento cartelle. In tal modo le cartelle dell'utente sia reindirizzato nei computer designati come computer primari dell'utente, ma non su tutti gli altri computer. È possibile controllare i computer primari per il reindirizzamento cartelle in ogni computer o per ogni utente.

Di seguito viene illustrato come abilitare i computer primari per il reindirizzamento delle cartelle:

1. In Gestione criteri di gruppo, fare doppio clic su oggetto Criteri di gruppo è stato creato quando si esegue la configurazione iniziale di reindirizzamento cartelle e/o i profili utente mobili (ad esempio, **impostazioni di reindirizzamento cartelle** o **utente Roaming Le impostazioni dei profili**), quindi selezionare **modifica**.
2. Per abilitare il supporto di computer primari tramite criteri di gruppo basate sul computer, passare a **configurazione Computer**. Per criteri di gruppo basate sull'utente, passare a **configurazione utente**.
    - Criteri di gruppo basate sul computer si applica l'elaborazione di computer primario a tutti i computer a cui si applica l'oggetto Criteri di gruppo, che interessano tutti gli utenti dei computer.
    - Criteri di gruppo basate sull'utente per l'elaborazione di computer primari viene applicata a tutti gli account utente a cui si applica l'oggetto Criteri di gruppo, che interessano tutti i computer in cui gli utenti accedano.
3. Sotto **configurazione Computer** oppure **configurazione utente**, passare a **criteri**, quindi **modelli amministrativi**, quindi  **System**, quindi **reindirizzamento cartelle**.
4. Fare doppio clic su **reindirizza cartelle solo nei computer primari**, quindi selezionare **modificare**.
5. Selezionare **Enabled**, quindi selezionare **OK**.

## <a name="step-3-optionally-enable-primary-computers-for-roaming-user-profiles-in-group-policy"></a>Passaggio 3: Se lo si desidera abilitare i computer primari per i profili utente in Criteri di gruppo

Il passaggio successivo consiste nel configurare facoltativamente criteri di gruppo per abilitare il supporto di computer primario per i profili utente. In questo modo consente a un profilo utente per il roaming nei computer designati come computer primari dell'utente, ma non su tutti gli altri computer.

Di seguito viene illustrato come abilitare i computer primari per i profili utente mobili:

1. Abilita supporto per computer primari per il reindirizzamento cartelle, se non è già presente.
    * In questo modo i documenti e altri file di utente all'esterno di profili utente mobili, che consente di profili di rimangano ridotte e tempi di accesso rimangono fast.
2. In Gestione criteri di gruppo, fare doppio clic su oggetto Criteri di gruppo è stato creato (ad esempio, **reindirizzamento cartelle e impostazioni dei profili utente mobili**), quindi selezionare **modificare**.
3. Passare a **configurazione Computer**, quindi **criteri**, quindi **modelli amministrativi**, quindi **sistema**e quindi **Profili utente mobili**.
4. Fare doppio clic su **Scarica profili mobili nei computer primari, solo** e quindi selezionare **modificare**.
5. Selezionare **Enabled**, quindi selezionare **OK**.

## <a name="step-4-enable-the-gpo"></a>Passaggio 4: Abilitare l'oggetto Criteri di gruppo

Dopo avere completato la configurazione di reindirizzamento cartelle e profili utente mobili, abilitare l'oggetto Criteri di gruppo se non è ancora disponibile. In questo modo lo consente da applicare a computer e utenti interessati.

Di seguito viene illustrato come abilitare il reindirizzamento cartelle e/o i GPO di profili utente mobili:

1. Gestione criteri di gruppo aprire
2. Fare doppio clic su oggetti Criteri di gruppo creato e quindi selezionare **collegamento abilitato**. Dovrebbe essere visualizzata una casella di controllo accanto alla voce di menu.

## <a name="step-5-test-primary-computer-function"></a>Passaggio 5: Testare la funzione computer primario

Per testare il supporto dei computer primari, accedere a un computer primario, confermare che i profili e le cartelle vengano reindirizzati, quindi accedere a un computer non principale e verificare che le cartelle e profili non vengono reindirizzati.

Di seguito viene illustrato come testare la funzionalità computer primario:

1. Accedere a un computer primario designato con un account utente per cui è stato abilitato il reindirizzamento delle cartelle e/o i profili utente mobili.
2. Se l'account utente ha effettuato l'accesso al computer in precedenza, aprire una sessione di Windows PowerShell o una finestra del prompt dei comandi come amministratore, digitare il comando seguente e quindi disconnettersi quando viene richiesto di verificare che vengano applicate le impostazioni di criteri di gruppo più recenti per il computer client:
    ```PowerShell
    Gpupdate /force
    ```
3. Aprire Esplora file.
4. Fare doppio clic su una cartella reindirizzata (ad esempio, la cartella documenti nella raccolta documenti) e quindi selezionare **proprietà**.
5. Selezionare il **posizione** scheda e verificare che il percorso siano presenti nella condivisione file specificata anziché un percorso locale. Per verificare che il profilo utente di roaming, aprire **Pannello di controllo**, selezionare **sistema e sicurezza**, selezionare **System**, selezionare **impostazioni di sistema avanzate** , selezionare **delle impostazioni** nei profili utente della sezione e quindi cercare **Roaming** nel **tipo** colonna.
6. Accedere con lo stesso account utente a un computer che non viene designato come computer primari dell'utente.
7. Ripetere i passaggi da 2 a 5, cercando invece i percorsi locali e un **locale** tipo di profilo.

>[!NOTE]
>Se le cartelle sono state reindirizzate in un computer prima di attivare il supporto di computer primari, le cartelle rimarranno reindirizzate a meno che non è configurata l'impostazione seguente nell'impostazione del criterio Reindirizzamento cartelle ogni cartella: **Reindirizza la cartella al percorso del profilo utente locale quando il criterio viene rimosso**. Allo stesso modo, verranno visualizzati i profili che sono stati precedentemente roaming in un computer specifico **Roaming** nel **tipo** colonne; tuttavia, il **stato** colonna visualizzerà **Locale**.

## <a name="more-information"></a>Altre informazioni

- [Distribuire reindirizzamento cartelle con i file Offline](deploy-folder-redirection.md)
- [Distribuire i profili utente mobili](deploy-roaming-user-profiles.md)
- [Panoramica di reindirizzamento cartelle, file Offline e profili utente mobili](folder-redirection-rup-overview.md)
- [Approfondire ulteriormente un po' Computer primario con Windows 8](https://blogs.technet.com/b/askds/archive/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer.aspx)