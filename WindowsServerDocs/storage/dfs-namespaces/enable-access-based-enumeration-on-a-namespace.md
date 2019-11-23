---
title: Attivare l'enumerazione basata sull'accesso in uno spazio dei nomi
description: Questo articolo descrive come abilitare l'enumerazione basata sull'accesso in uno spazio dei nomi.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 246df5b13a1dbea614886ab7fe445dd448ae1763
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402174"
---
# <a name="enable-access-based-enumeration-on-a-namespace"></a>Attivare l'enumerazione basata sull'accesso in uno spazio dei nomi

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

L'enumerazione basata sull'accesso nasconde file e cartelle per i quali gli utenti non dispongono delle autorizzazioni di accesso. Per impostazione predefinita, questa funzionalità non è abilitata per Spazi dei nomi DFS. Puoi abilitare l'enumerazione basata sull'accesso delle cartelle DFS mediante Gestione DFS. Per controllare l'enumerazione basata sull'accesso di file e cartelle nelle destinazioni cartella, devi abilitare l'enumerazione basata sull'accesso in ogni cartella condivisa tramite Gestione condivisione e archiviazione.

Per abilitare l'enumerazione basata sull'accesso in uno spazio dei nomi, tutti i server dello spazio dei nomi devono eseguire Windows Server 2008 o versione successiva. Gli spazi dei nomi basati su dominio devono inoltre utilizzare la modalità Windows Server 2008. Per informazioni sui requisiti della modalità Windows Server 2008, vedere [scegliere un tipo di spazio dei nomi](choose-a-namespace-type.md).

In alcuni ambienti, l'abilitazione dell'enumerazione basata sull'accesso può causare un elevato uso della CPU sul server e tempi di risposta lenti per gli utenti.

> [!NOTE]
> Se si aggiorna il livello di funzionalità del dominio a Windows Server 2008 mentre sono presenti spazi dei nomi basati su dominio esistenti, gestione DFS consentirà di abilitare l'enumerazione basata sull'accesso per questi spazi dei nomi. Tuttavia, non sarà possibile modificare le autorizzazioni per nascondere le cartelle da gruppi o utenti a meno che non si effettui la migrazione degli spazi dei nomi alla modalità Windows Server 2008. Per ulteriori informazioni, vedere [Eseguire la migrazione di uno spazio dei nomi basato su dominio alla modalità Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md).


Per usare l'enumerazione basata sull'accesso con Spazi dei nomi DFS, esegui questa procedura:

-   Attivare l'enumerazione basata sull'accesso in uno spazio dei nomi
-   Controllare quali utenti e gruppi possono visualizzare le singole cartelle DFS


> [!WARNING]
> L'enumerazione basata sull'accesso non impedisce agli utenti di ottenere un riferimento a una destinazione cartella se conoscono già il percorso DFS. Solo le autorizzazioni di condivisione o le autorizzazioni di file system NTFS della destinazione cartella (cartella condivisa) possono impedire agli utenti di accedere a una destinazione cartella. Le autorizzazioni per le cartelle DFS vengono usate solo per visualizzare o nascondere le cartelle DFS, non per il controllo di accesso, rendendo l'accesso in lettura la solo autorizzazione rilevante a livello di cartella DFS. Per ulteriori informazioni, vedi [Uso delle autorizzazioni ereditate con enumerazione basata sull'accesso](https://technet.microsoft.com/library/dd834874(v=ws.11).aspx)

<br />
Puoi abilitare l'enumerazione basata sull'accesso in uno spazio dei nomi tramite l'interfaccia di Windows o una riga di comando.

## <a name="to-enable-access-based-enumeration-by-using-the-windows-interface"></a>Per abilitare l'enumerazione basata sull'accesso tramite l'interfaccia di Windows

1.  Nell'albero della console fai clic con il pulsante destro del mouse sullo spazio dei nomi appropriato nel nodo **Spazi dei nomi**, quindi scegli **Proprietà**.

2.  Fai clic sulla scheda **Avanzate**, quindi seleziona la casella di controllo **Abilita enumerazione basata sull'accesso per questo spazio dei nomi**.

## <a name="to-enable-access-based-enumeration-by-using-a-command-line"></a>Per abilitare l'enumerazione basata sull'accesso tramite una riga di comando

1.  Apri una finestra del prompt dei comandi in un server con installato il servizio ruolo **File system distribuito** o la funzionalità **Strumenti per File system distribuito (DFS)** .

2.  Digitare il comando seguente, dove *< spazio dei nomi\_radice >* è la radice dello spazio dei nomi:

    ```  
    dfsutil property abe enable \\ <namespace_root>
    ```

> [!TIP]
> Per gestire l'enumerazione basata sull'accesso su uno spazio dei nomi tramite Windows PowerShell, usa i cmdlet [Set-DfsnRoot](https://technet.microsoft.com/library/jj884281.aspx), [Grant-DfsnAccess](https://technet.microsoft.com/library/jj884272.aspx) e [Revoke-DfsnAccess](https://technet.microsoft.com/library/jj884273.aspx). Il modulo Windows PowerShell per Spazio dei nomi DFS è stato introdotto in Windows Server 2012.

Puoi controllare quali utenti e gruppi possono visualizzare le singole cartelle DFS tramite l'interfaccia di Windows o una riga di comando.

## <a name="to-control-folder-visibility-by-using-the-windows-interface"></a>Per controllare la visibilità di una cartella tramite l'interfaccia di Windows

1.  Nel nodo **Spazio dei nomi** nell'albero della consolo individua la cartella con destinazioni di cui controllare la visibilità, fai clic con il pulsante destro del mouse sulla cartella, quindi scegli **Proprietà**.

2.  Fare clic sulla scheda **Avanzate**.

3.  Fai clic su **Imposta autorizzazioni di visualizzazione esplicite per la cartella DFS**, quindi fai clic su **Configura autorizzazioni visualizzazione**.

4.  Aggiungere o rimuovere gruppi o utenti facendo clic su **Aggiungi** o **Rimuovi**.

5.  Per consentire agli utenti di visualizzare la cartella DFS, seleziona il gruppo o l'utente, quindi seleziona la casella di controllo **Consenti**.

    Per nascondere la cartella a un gruppo oppure a un utente, seleziona il gruppo o l'utente, quindi seleziona la casella di controllo **Nega**.

## <a name="to-control-folder-visibility-by-using-a-command-line"></a>Per controllare la visibilità di una cartella tramite una riga di comando

1. Apri una finestra del prompt dei comandi in un server con installato il servizio ruolo **File system distribuito** o la funzionalità **Strumenti per File system distribuito (DFS)** .

2. Digitare il comando seguente, dove *&lt;DFSPath&gt;* è il percorso della cartella DFS (collegamento), *< dominio\\account >* è il nome dell'account utente o di gruppo e *(...)* viene sostituito con voci di controllo di accesso (ACE) aggiuntive:

   ```
   dfsutil property sd grant <DFSPath> DOMAIN\Account:R (...) Protect Replace
   ```

   Ad esempio, per sostituire le autorizzazioni esistenti con autorizzazioni che consentano ai gruppi Domain Admins e CONTOSO\\trainers di leggere (R) l'accesso alla cartella \\contoso. office\public\training, digitare il comando seguente:

   ```
   dfsutil property sd grant \\contoso.office\public\training "CONTOSO\Domain Admins":R CONTOSO\Trainers:R Protect Replace 
   ```

3. Per eseguire attività aggiuntive dal prompt dei comandi, usa i comandi seguenti:


| Comando | Descrizione |
|---|---|
|[Proprietà di Dfsutil SD Deny](https://msdn.microsoft.com/library/dd759150(v=ws.11).aspx)|Impedisce a un gruppo oppure a un utente di visualizzare la cartella.|
|[Reimpostazione SD della proprietà Dfsutil](https://msdn.microsoft.com/library/dd759150(v=ws.11).aspx) |Rimuove tutte le autorizzazioni dalla cartella.|
|[Dfsutil proprietà SD revoca](https://msdn.microsoft.com/library/dd759150(v=ws.11).aspx)| Rimuove una voce di controllo di accesso per l'utente o il gruppo dalla cartella. |

## <a name="see-also"></a>Vedi anche

-   [Creare uno spazio dei nomi DFS](create-a-dfs-namespace.md)
-   [Delegare le autorizzazioni di gestione per Spazi dei nomi DFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Installazione di DFS](https://technet.microsoft.com/library/cc731089(v=ws.11).aspx)
-   [Utilizzo di autorizzazioni ereditate con enumerazione basata sull'accesso](using-inherited-permissions-with-access-based-enumeration.md)