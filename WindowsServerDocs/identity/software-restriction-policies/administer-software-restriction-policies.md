---
title: Amministrare i criteri di restrizione Software
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8cc22093-67d1-47b6-9ddd-4569b6761ce9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 67ef99c95f10585ab72dee4bdf6512e715d13f4a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="administer-software-restriction-policies"></a>Amministrare i criteri di restrizione Software

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento destinato ai professionisti IT contiene le procedure amministrare i criteri di controllo delle applicazioni utilizzando inizio Software criteri di restrizione con Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introduzione
Software di criteri di restrizione è una funzionalità basata su criteri di gruppo che identifica i programmi software in esecuzione nel computer in un dominio e controlla la possibilità di eseguire tali programmi. Utilizzare criteri di restrizione software per creare una configurazione molto restrittiva per i computer, in cui consentire l'esecuzione solo di applicazioni specifiche identificate eseguire. Questi sono integrati in servizi di dominio Microsoft Active Directory e criteri di gruppo, ma possono anche essere configurati in computer autonomi. Per ulteriori informazioni su criteri di restrizione software, vedere il [criteri di restrizione Software](software-restriction-policies.md).

A partire da Windows Server 2008 R2 e Windows 7, Windows AppLocker può essere utilizzato invece di o in combinazione con criteri di restrizione software per una parte della strategia di controllo dell'applicazione.

In questo argomento contiene:

-   [Per aprire Criteri di restrizione Software](#BKMK_Open_SRP)

-   [Per creare nuovi criteri di restrizione software](#BKMK_Create_SRP)

-   [Per aggiungere o eliminare un tipo di file](#BKMK_Add_Del)

-   [Per impedire che i criteri di restrizione software applicati agli amministratori locali](#BKMK_Prevent_Admin)

-   [Per modificare il livello di sicurezza predefinito dei criteri di restrizione software](#BKMK_Sec_Lvl)

-   [Per applicare i criteri di restrizione software alle DLL](#BKMK_Apply_SRP_DLLs)

Per informazioni su come eseguire attività specifiche utilizzando criteri di restrizione software, vedere gli argomenti seguenti:

-   [Determinazione dell'elenco Consenti-Nega e l'inventario delle applicazioni per criteri di restrizione Software](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

-   [Utilizzo delle regole dei criteri di restrizione Software](work-with-software-restriction-policies-rules.md)

-   [Utilizzare criteri di restrizione Software per proteggere il Computer da un Virus di posta elettronica](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

## <a name="BKMK_Open_SRP"></a>Per aprire Criteri di restrizione Software

-   [Per il computer locale](#BKMK_1)

-   [Per un dominio, sito, o unità organizzativa e si sono su un server membro o una workstation in cui viene aggiunto a un dominio](#BKMK_2)

-   [Per un dominio o unità organizzativa e si presenti in un controller di dominio o in una workstation con strumenti di amministrazione Server remota installato](#BKMK_3)

-   [Per un sito e sono in un controller di dominio o in una workstation con strumenti di amministrazione Server remota installato](#BKMK_4)

### <a name="BKMK_1"></a>Per il computer locale

1.  Apri le impostazioni di sicurezza locali.

2.  Nell'albero della console, fare clic su **criteri di restrizione Software**.

    **Dove?**

    -   Criteri di restrizione Software/Impostazioni sicurezza

> [!NOTE]
> Per eseguire questa procedura, è necessario essere un membro del gruppo Administrators nel computer locale oppure aver ricevuto in delega l'autorità appropriata.

### <a name="BKMK_2"></a>Per un dominio, sito, o unità organizzativa e si sono su un server membro o una workstation in cui viene aggiunto a un dominio

1.  Aprire Microsoft Management Console (MMC).

2.  Nel **File** menu, fare clic su **Aggiungi/Rimuovi Snap-in**, quindi fare clic su **Aggiungi**.

3.  Fare clic su **Editor oggetti Criteri di gruppo locali**, quindi fare clic su **Aggiungi**.

4.  In **Selezione oggetto Criteri di gruppo**, fare clic su **Sfoglia**.

5.  In **cercare un oggetto Criteri di gruppo**, selezionare un oggetto Criteri di gruppo (GPO) nel dominio appropriato, sito o unità organizzativa- o crearne uno nuovo e quindi fare clic su **fine**.

6.  Fare clic su **Chiudi**, quindi fare clic su **OK**.

7.  Nell'albero della console, fare clic su **criteri di restrizione Software**.

    **Dove?**

    -   *Oggetto Criteri di gruppo* [*ComputerName*] configurazione Computer/criteri o

        Utente configurazione/Windows/protezione impostazioni impostazioni/Criteri restrizione Software

> [!NOTE]
> Per eseguire questa procedura, è necessario essere un membro del gruppo Domain Admins.

### <a name="BKMK_3"></a>Per un dominio o unità organizzativa e si presenti in un controller di dominio o in una workstation con strumenti di amministrazione Server remota installato

1.  Aprire Console Gestione criteri di gruppo.

2.  Nell'albero della console fare doppio clic sull'oggetto (criteri di gruppo) che si desidera aprire Criteri di restrizione software per.

3.  Fare clic su **modifica** per aprire l'oggetto Criteri di gruppo che si desidera modificare. È anche possibile fare clic su **New** per creare un nuovo oggetto Criteri di gruppo e quindi fare clic su **modifica**.

4.  Nell'albero della console, fare clic su **criteri di restrizione Software**.

    **Dove?**

    -   *Oggetto Criteri di gruppo* [*ComputerName*] configurazione Computer/criteri o

        Utente configurazione/Windows/protezione impostazioni impostazioni/Criteri restrizione Software

> [!NOTE]
> Per eseguire questa procedura, è necessario essere un membro del gruppo Domain Admins.

### <a name="BKMK_4"></a>Per un sito e sono in un controller di dominio o in una workstation con strumenti di amministrazione Server remota installato

1.  Aprire Console Gestione criteri di gruppo.

2.  Nell'albero della console, fare clic sul sito che si desidera impostare criteri di gruppo.

    **Dove?**

    -   Servizi e siti di Active Directory [*nome_controller_dominio. nome_dominio*] / siti al sito

3.  Fare clic su una voce nella **collegamenti oggetti Criteri di gruppo** per selezionare un oggetto esistente di criteri di gruppo (GPO) e quindi fare clic su **modifica**. È anche possibile fare clic su **New** per creare un nuovo oggetto Criteri di gruppo e quindi fare clic su **modifica**.

4.  Nell'albero della console, fare clic su **criteri di restrizione Software**.

    **Dove**

    -   *Oggetto Criteri di gruppo* [*ComputerName*] configurazione Computer/criteri o

        Utente configurazione/Windows/protezione impostazioni impostazioni/Criteri restrizione Software

> [!NOTE]
> -   Per eseguire questa procedura, è necessario essere un membro del gruppo Administrators nel computer locale oppure aver ricevuto in delega l'autorità appropriata. Se il computer è unito a un dominio, è possibile che i membri del gruppo Domain Admins potrebbero essere in grado di eseguire questa procedura.
> -   Per configurare le impostazioni di criteri che verranno applicate a computer, indipendentemente dall'utente che effettua l'accesso, fare clic su **configurazione Computer**.
> -   Per configurare le impostazioni di criteri che verranno applicate agli utenti, indipendentemente dal computer a cui accedono, fare clic su **configurazione utente**.

## <a name="BKMK_Create_SRP"></a>Per creare nuovi criteri di restrizione software

1.  Aprire criteri di restrizione Software.

2.  Nel **azione** menu, fare clic su **nuovi criteri di restrizione Software**.

> [!WARNING]
> -   Per eseguire questa procedura, a seconda dell'ambiente, sono necessarie credenziali amministrative diverse:
> 
>     -   Se crei nuovi criteri di restrizione software per il computer locale: appartenenza al gruppo locale **amministratori** o gruppo equivalente, è il requisito minimo necessario per completare questa procedura.
>     -   Se si creano nuovi criteri di restrizione software per un computer appartenente a un dominio, i membri del gruppo Domain Admins possono eseguire questa procedura.
> -   Se i criteri di restrizione software sono già stati creati per un oggetto di criteri di gruppo (GPO), il **nuovi criteri di restrizione Software** comando non viene visualizzato nel **azione** menu. Per eliminare i criteri di restrizione software applicati a un oggetto Criteri di gruppo, nell'albero della console, fare doppio clic su **criteri di restrizione Software**, quindi fare clic su **Elimina criteri restrizione Software**. Quando si elimina criteri restrizione software per un oggetto Criteri di gruppo, anche eliminare tutte le regole dei criteri di restrizione software per tale oggetto Criteri di gruppo. Dopo aver eliminato i criteri di restrizione software, è possibile creare nuovi criteri di restrizione software per tale oggetto Criteri di gruppo.

## <a name="BKMK_Add_Del"></a>Per aggiungere o eliminare un tipo di file

1.  Aprire criteri di restrizione Software.

2.  Nel riquadro dei dettagli fare doppio clic su **tipi di File designati**.

3.  Eseguire una delle operazioni seguenti:

    -   Per aggiungere un tipo di file, in **estensione del nome File**, digitare l'estensione di file e quindi fare clic su **Aggiungi**.

    -   Per eliminare un tipo di file, in **tipi di file designati**, fare clic sul tipo di file e quindi fare clic su **rimuovere**.

> [!NOTE]
> -   Per eseguire questa procedura, a seconda dell'ambiente in cui si aggiunge o elimina un tipo di file sono necessarie credenziali amministrative diverse:
> 
>     -   Se si aggiunge o elimina un tipo di file per il computer locale: appartenenza al gruppo locale **amministratori** o gruppo equivalente, è il requisito minimo necessario per completare questa procedura.
>     -   Se si creano nuovi criteri di restrizione software per un computer appartenente a un dominio, i membri del gruppo Domain Admins possono eseguire questa procedura.
> -   Potrebbe essere necessario creare una nuova impostazione di criteri di restrizione software per l'oggetto Criteri di gruppo (GPO) se non è già stato fatto.
> -   L'elenco di tipi di file designati è condiviso da tutte le regole per i Computer e configurazione utente per un oggetto Criteri di gruppo.

## <a name="BKMK_Prevent_Admin"></a>Per impedire che i criteri di restrizione software applicati agli amministratori locali

1.  Aprire criteri di restrizione Software.

2.  Nel riquadro dei dettagli fare doppio clic su **imposizione**.

3.  In **Applica criteri restrizione software agli utenti seguenti**, fare clic su **tutti gli utenti esclusi gli amministratori locali**.

> [!WARNING]
> -   L'appartenenza al gruppo **amministratori** o gruppo equivalente, è il requisito minimo necessario per completare questa procedura.
> -   Potrebbe essere necessario creare una nuova impostazione di criteri di restrizione software per l'oggetto Criteri di gruppo (GPO) se non è già stato fatto.
> -   Se è comune per gli utenti possono essere membri del gruppo Administrators locale nei loro computer nell'organizzazione, si desidera abilitare questa opzione.
> -   Se si sta definendo un'impostazione di criteri di restrizione software per il computer locale, è possibile utilizzare questa procedura per evitare che gli amministratori locali applicati i criteri di restrizione software. Se si sta definendo un'impostazione di criteri di restrizione software per la rete, filtrare le impostazioni di criteri utente in base all'appartenenza a gruppi di sicurezza tramite criteri di gruppo.

## <a name="BKMK_Sec_Lvl"></a>Per modificare il livello di sicurezza predefinito dei criteri di restrizione software

1.  Aprire criteri di restrizione Software.

2.  Nel riquadro dei dettagli fare doppio clic su **livelli di sicurezza**.

3.  Fare doppio clic sul livello di sicurezza che si desidera impostare come predefinito e quindi fare clic su **imposta come predefinito**.

> [!CAUTION]
> In determinate directory, impostazione livello per la sicurezza predefinita **non consentito** può influire negativamente sul sistema operativo.

> [!NOTE]
> -   Per eseguire questa procedura, a seconda dell'ambiente per cui si modifica il livello di sicurezza predefinito dei criteri di restrizione software sono necessarie credenziali amministrative diverse.
> -   Potrebbe essere necessario creare una nuova impostazione di criteri di restrizione software per questo oggetto Criteri di gruppo (GPO) se non è già stato fatto.
> -   Nel riquadro dei dettagli, il livello di sicurezza predefinito corrente è indicato da un cerchio nero con un segno di spunta in essa contenuti. Se il pulsante destro del mouse il livello di sicurezza predefinito corrente, il **imposta come predefinito** comando non viene visualizzato nel menu.
> -   Vengono create regole dei criteri di restrizione software consentono di specificare eccezioni a livello di protezione predefinito. Quando il livello di sicurezza predefinito è impostato su **senza restrizioni**, regole consentono di specificare il software che non è consentito eseguire. Quando il livello di sicurezza predefinito è impostato su **non consentito**, regole consentono di specificare il software che può essere eseguita.
> -   Al momento dell'installazione, il livello di sicurezza predefinito dei criteri di restrizione software in tutti i file nel sistema è impostato su **senza restrizioni**.

## <a name="BKMK_Apply_SRP_DLLs"></a>Per applicare i criteri di restrizione software alle DLL

1.  Aprire criteri di restrizione Software.

2.  Nel riquadro dei dettagli fare doppio clic su **imposizione**.

3.  In **Applica criteri restrizione software per le operazioni seguenti**, fare clic su **tutti i file software**.

> [!NOTE]
> -   Per eseguire questa procedura, è necessario essere un membro del gruppo Administrators nel computer locale oppure aver ricevuto in delega l'autorità appropriata. Se il computer è unito a un dominio, è possibile che i membri del gruppo Domain Admins potrebbero essere in grado di eseguire questa procedura.
> -   Per impostazione predefinita, i criteri di restrizione software non verificare librerie a collegamento dinamico (DLL). Il controllo delle DLL può ridurre le prestazioni del sistema, perché i criteri di restrizione software devono avvenire a ogni caricamento di una DLL. Tuttavia, è possibile decidere di controllare le DLL se teme la ricezione di un virus progettato per le DLL. Se il livello di sicurezza predefinito è impostato su **non consentito**, e si Abilita controllo delle DLL, è necessario creare regole che consentono di ogni DLL per l'esecuzione dei criteri di restrizione software.


