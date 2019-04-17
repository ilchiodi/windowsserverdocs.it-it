---
title: Distribuire i computer primari per il reindirizzamento delle cartelle e profili utente mobili
description: Come abilitare il supporto di computer primario e designare un computer primario per gli utenti con reindirizzamento cartelle e profili utente mobili.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7b3c87597e07102d00fc068b7ecd5744e4ba366f
ms.sourcegitcommit: 505505b7ec99506e7ff50eddbdd6aa94a602abe6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/11/2018
ms.locfileid: "3831401"
---
# Distribuire i computer primari per il reindirizzamento delle cartelle e profili utente mobili

>Si applica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

Questo argomento descrive come abilitare il supporto di computer primario e designare un computer primario per gli utenti. In questo modo consente di controllare quali computer utilizzare Reindirizzamento cartelle e profili utente mobili.

>[!IMPORTANT]
>Quando si abilita supporto per computer primario per i profili utente, sempre Abilita supporto per computer primario per il reindirizzamento cartelle anche. In questo modo documenti e altri file degli utenti all'esterno dei profili utente, che aiuta i profili di rimangano piccolo e accedano volte rimanere veloce.

## Prerequisiti

## Requisiti software

Supporto per computer primario ha i requisiti seguenti:

- Lo schema di Active Directory Domain Services (AD DS) deve essere aggiornato per includere aggiunte allo schema di Windows Server 2012 (l'installazione di un controller di dominio Windows Server 2012 automaticamente gli aggiornamenti dello schema). Per informazioni sull'aggiornamento dello schema di Active Directory Domain Services, vedi [l'integrazione Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh472161(v=ws.11)#adprepexe-integration>) e [Adprep.exe in esecuzione](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)>).
- I computer client devono eseguire Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.

>[!TIP]
>Anche se il supporto di computer primario richiede il reindirizzamento delle cartelle e/o profili utente mobili, se stai distribuendo queste tecnologie per la prima volta, si consiglia di configurare il supporto di computer principale prima dell'abilitazione di oggetti Criteri di gruppo che consentono di configurare il reindirizzamento delle cartelle e Profili utente mobili. Ciò impedisce che i dati utente viene copiato i computer non principale prima di supporto per computer principale è abilitato. Per informazioni sulla configurazione, vedi [Distribuire reindirizzamento](deploy-folder-redirection.md) cartelle e [Distribuire i profili utente](deploy-roaming-user-profiles.md).

## Passaggio 1: Designare computer primario per gli utenti

Il primo passaggio nella distribuzione di supporto per i computer primario è definendo i computer primari per ogni utente. A tale scopo, utilizzare il centro di amministrazione di Active Directory per ottenere il nome distinto dei computer in questione e quindi imposta l'attributo **msDs-PrimaryComputer** .

>[!TIP]
>Per usare Windows PowerShell per lavorare con i computer primari, vedi il blog post [leggermente approfondimenti in Computer principale di Windows 8](<https://blogs.technet.microsoft.com/askds/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer/>).

Ecco come specificare i computer primari per gli utenti:

1. Apri Server Manager in un computer in cui sono installati gli strumenti di amministrazione di Active Directory.
2. Nel menu **Strumenti** , seleziona **Il centro di amministrazione di Active Directory**. Verrà visualizzato Centro amministrativo di Active Directory.
3. Passa al contenitore **computer** del dominio appropriato.
4. Fare clic su un computer che si desidera designare un computer primario e quindi seleziona **proprietà**.
5. Nel riquadro di spostamento, seleziona **le estensioni**.
6. Seleziona la scheda **Editor attributi** , Scorri fino alla **distinguishedName**, seleziona **visualizzazione**, fai il valore elencato, selezionare **Copia**, fare **clic su OK**e quindi **annullare**.
7. Passa al contenitore **utenti** del dominio appropriato, fai l'utente a cui si desidera assegnare il computer e quindi seleziona **proprietà**.
8. Nel riquadro di spostamento, seleziona **le estensioni**.
9. Seleziona la scheda **Editor attributi** , seleziona **msDs-PrimaryComputer** e quindi seleziona **Modifica**. Verrà visualizzata la finestra di dialogo Editor stringa multivalore.
10. Fai la casella di testo, selezionare **Incolla**, scegli **Aggiungi**, fare **clic su OK**e quindi seleziona **OK** nuovamente.

## Passaggio 2: Eventualmente abilitare i computer primari per il reindirizzamento delle cartelle in Criteri di gruppo

Il passaggio successivo consiste nel configurare facoltativamente criteri di gruppo per abilitare il supporto di computer primario per il reindirizzamento delle cartelle. In questo modo consente di cartelle di un utente essere reindirizzato nei computer designate come computer principale dell'utente, ma non su altri computer. Puoi controllare i computer primari per il reindirizzamento cartelle in ogni computer o per ogni utente.

Ecco come abilitare i computer primari per il reindirizzamento delle cartelle:

1. In Gestione criteri di gruppo, fai doppio clic l'oggetto Criteri di gruppo creato quando si esegue la configurazione iniziale di reindirizzamento cartelle e/o i profili utente (ad esempio, **Impostazioni di reindirizzamento cartelle** o **Il Roaming delle impostazioni di profili utente**) e quindi Selezionare **Modifica**.
2. Per abilitare il supporto di computer primario tramite criteri di gruppo basato sul computer, passa a **Configurazione Computer**. Criteri di gruppo basate sull'utente per passare alla **Configurazione utente**.
    - Criteri di gruppo basato sul computer elaborazione del computer principale si applica a tutti i computer a cui si applica l'oggetto Criteri di gruppo, che interessa tutti gli utenti dei computer.
    - Criteri di gruppo basate sull'utente per l'elaborazione del computer principale si applica a tutti gli account utente a cui si applica l'oggetto Criteri di gruppo, effetto su tutti i computer a cui gli utenti effettuano il su.
3. In **Configurazione Computer** o **Configurazione utente**, passare **i criteri**, **Modelli amministrativi**, quindi **sistema**e quindi **Il reindirizzamento delle cartelle**.
4. Fai **reindirizzamento delle cartelle nei solo computer primario**e quindi seleziona **Modifica**.
5. Seleziona **abilitato**e quindi seleziona **OK**.

## Passaggio 3: Attivare facoltativamente computer primario per i profili utente in Criteri di gruppo

Il passaggio successivo consiste nel configurare facoltativamente criteri di gruppo per abilitare il supporto di computer primario per i profili utente. In questo modo consente un profilo utente per eseguire il roaming nei computer designate come computer principale dell'utente, ma non su altri computer.

Ecco come attivare i computer primari per profili utente mobili:

1. Abilita supporto per computer primario per il reindirizzamento cartelle, se hai già fatto.
    * In questo modo documenti e altri file degli utenti all'esterno dei profili utente, che aiuta i profili di rimangano piccolo e accedano volte rimanere veloce.
2. In Gestione criteri di gruppo, fai l'oggetto Criteri di gruppo creato (ad esempio, **reindirizzamento cartelle e il Roaming delle impostazioni di profili utente**) e quindi seleziona **Modifica**.
3. Passa a **Configurazione Computer**, quindi **i criteri**, quindi **modelli amministrativi**, quindi **sistema**e quindi **profili utente**.
4. Fai **scaricare i profili nei computer primario solo** e quindi seleziona **Modifica**.
5. Seleziona **abilitato**e quindi seleziona **OK**.

## Passaggio 4: Abilitare l'oggetto Criteri di gruppo

Dopo avere completato la configurazione di cartelle di reindirizzamento cartelle e profili utente mobili, abilitare l'oggetto Criteri di gruppo, se hai non è già. In questo modo consente di applicare ai computer e utenti interessati.

Ecco come abilitare il reindirizzamento cartelle e/o Roaming GPO profili utente:

1. Gestione criteri di gruppo Apri
2. Fai oggetti Criteri di gruppo che hai creato e quindi seleziona **Collegamento abilitato**. Dovrebbe essere visualizzata una casella di controllo accanto a voce di menu.

## Passaggio 5: Verificare la funzione principale computer

Per testare il supporto di computer principale, accedi a un computer principale, verificare che le cartelle e profili vengono reindirizzati, quindi accedi a un computer non principale e verificare che le cartelle e profili non vengono reindirizzati.

Ecco come funzionalità principali di computer di test:

1. Accedi a un computer primario designato con un account utente per cui è stato attivato il reindirizzamento delle cartelle e/o i profili utente.
2. Se l'account utente ha effettuato l'accesso al computer in precedenza, aprire una sessione di Windows PowerShell o la finestra del prompt dei comandi come amministratore, digita il comando seguente e quindi autorizzare quando richiesto per garantire che le impostazioni di criteri di gruppo più recenti vengono applicate per il computer client:
    ```PowerShell
    Gpupdate /force
    ```
3. Apri Esplora File.
4. Fai una reindirizzato (ad esempio, la cartella documenti nella raccolta di documenti) e quindi seleziona **proprietà**.
5. Seleziona la scheda di **posizione** e verificare che il percorso visualizzato nella condivisione di file specificati invece di un percorso locale. Per verificare che il profilo utente è in roaming, Apri **Il pannello di controllo**, seleziona **sistema e sicurezza**, seleziona **sistema**, selezionare **Le impostazioni di sistema avanzate**, seleziona **le impostazioni** nella sezione profili utente e quindi cercare ** Roaming** nella colonna **tipo** .
6. Accedi con lo stesso account utente in un computer che non è designato come computer principale dell'utente.
7. Ripetere i passaggi da 2 a 5, cercando invece percorsi locali e un tipo di profilo **locale** .

>[!NOTE]
>Se le cartelle sono state reindirizzate in un computer prima di attivare il supporto di computer principale, le cartelle rimarrà reindirizzate a meno che non è configurata l'impostazione seguente nell'impostazione del criterio di reindirizzamento cartelle ogni cartella: **Reindirizza la cartella locale posizione USERPROFILE quando il criterio viene rimosso**. Analogamente, i profili a cui sono state roaming in precedenza in un determinato computer mostrerà **Roaming** nelle colonne del **tipo** ; Tuttavia, la colonna **stato** mostrerà **locale**.

## Ulteriori informazioni

- [Distribuire cartelle di reindirizzamento cartelle con i file Offline](deploy-folder-redirection.md)
- [Distribuire i profili utente mobili](deploy-roaming-user-profiles.md)
- [Panoramica di cartelle di reindirizzamento cartelle, file Offline e profili utente mobili](folder-redirection-rup-overview.md)
- [Un po' approfondimenti in Computer principale di Windows 8](https://blogs.technet.com/b/askds/archive/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer.aspx)