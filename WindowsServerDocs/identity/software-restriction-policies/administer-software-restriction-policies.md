---
title: Gestire i criteri di restrizione software
description: Sicurezza di Windows Server
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863592"
---
# <a name="administer-software-restriction-policies"></a>Gestire i criteri di restrizione software

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento destinato ai professionisti IT contiene le procedure amministrare i criteri di controllo delle applicazioni usando i criteri di restrizione Software (SRP) inizia con Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introduzione
I criteri di restrizione software sono una funzionalità basata su Criteri di gruppo che identifica i programmi software in esecuzione nei computer di un dominio e controlla la possibilità di tali programmi di essere eseguiti. È possibile utilizzare i criteri di restrizione software per creare una configurazione molto restrittiva per i computer, in cui consentire l'esecuzione solo di applicazioni specifiche identificate. Questi sono integrati con Microsoft Active Directory Domain Services e criteri di gruppo, ma può anche essere configurati in computer autonomi. Per altre informazioni sui criteri di restrizione software, vedere la [criteri di restrizione Software](software-restriction-policies.md).

A partire da Windows Server 2008 R2 e Windows 7, Windows AppLocker utilizzabile al posto di o in concerto con criteri di restrizione software per una parte della strategia di controllo dell'applicazione.

In questo argomento contiene:

-   [Per aprire Criteri restrizione Software](#BKMK_Open_SRP)

-   [Per creare nuovi criteri di restrizione software](#BKMK_Create_SRP)

-   [Per aggiungere o eliminare un tipo di file](#BKMK_Add_Del)

-   [Per impedire che i criteri di restrizione software applicati agli amministratori locali](#BKMK_Prevent_Admin)

-   [Per modificare il livello di sicurezza predefinito dei criteri di restrizione software](#BKMK_Sec_Lvl)

-   [Per applicare i criteri di restrizione software alle DLL](#BKMK_Apply_SRP_DLLs)

Per informazioni su come eseguire attività specifiche utilizzando criteri di restrizione software, vedere gli argomenti seguenti:

-   [Determinare elenco Consenti-Nega e l'inventario delle applicazioni per i criteri di restrizione Software](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

-   [Lavorare con le regole di criteri di restrizione Software](work-with-software-restriction-policies-rules.md)

-   [Usare i criteri di restrizione Software per proteggere il Computer da un Virus di posta elettronica](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

## <a name="BKMK_Open_SRP"></a>Per aprire Criteri restrizione Software

-   [Per il computer locale](#BKMK_1)

-   [Per un dominio, sito, o unità organizzativa e si sia in un server membro o una workstation appartenente a un dominio](#BKMK_2)

-   [Per un dominio o unità organizzativa e sono in un controller di dominio o in una workstation con strumenti di amministrazione Server remota installato](#BKMK_3)

-   [Per un sito e sono in un controller di dominio o in una workstation con strumenti di amministrazione Server remota installato](#BKMK_4)

### <a name="BKMK_1"></a>Per il computer locale

1.  Aprire Impostazioni sicurezza locale.

2.  Nell'albero della console, fare clic su **criteri di restrizione Software**.

    **Dove?**

    -   Criteri di restrizione Software/Impostazioni sicurezza

> [!NOTE]
> Per eseguire questa procedura è necessario essere membri del gruppo Administrators nel computer locale oppure disporre della delega per l'autorità appropriata.

### <a name="BKMK_2"></a>Per un dominio, sito, o unità organizzativa e si sia in un server membro o una workstation appartenente a un dominio

1.  Aprire Microsoft Management Console (MMC).

2.  Scegliere **Aggiungi/Rimuovi snap-in** dal menu **File** e quindi fare clic su **Aggiungi**.

3.  Fare clic su **Editor oggetti Criteri di gruppo locale** e quindi fare clic su **Aggiungi**.

4.  In **Selezione oggetto Criteri di gruppo** fare clic su **Sfoglia**.

5.  Nelle **Cerca un oggetto Criteri di gruppo**, selezionare un oggetto Criteri di gruppo (GPO) nel dominio appropriato, sito o unità organizzativa- o crearne uno nuovo e quindi fare clic su **fine**.

6.  Fare clic su **Chiudi**e quindi su **OK**.

7.  Nell'albero della console, fare clic su **criteri di restrizione Software**.

    **Dove?**

    -   *Oggetto Criteri di gruppo* [*ComputerName*] configurazione Computer/criteri o

        Criteri di restrizione impostazioni/Software o della sicurezza delle impostazioni utente configurazione/Windows

> [!NOTE]
> Per eseguire questa procedura, è necessario essere un membro del gruppo Domain Admins.

### <a name="BKMK_3"></a>Per un dominio o unità organizzativa e sono in un controller di dominio o in una workstation con strumenti di amministrazione Server remota installato

1.  Aprire Console Gestione criteri di gruppo.

2.  Nell'albero della console, fare doppio clic su di oggetto (criteri di gruppo) che si desidera aprire Criteri di restrizione software per.

3.  Fare clic su **Modifica** per aprire l'oggetto Criteri di gruppo che si desidera modificare. È inoltre possibile fare clic su **Nuovo** per creare un nuovo oggetto Criteri di gruppo e quindi fare clic su **Modifica**.

4.  Nell'albero della console, fare clic su **criteri di restrizione Software**.

    **Dove?**

    -   *Oggetto Criteri di gruppo* [*ComputerName*] configurazione Computer/criteri o

        Criteri di restrizione impostazioni/Software o della sicurezza delle impostazioni utente configurazione/Windows

> [!NOTE]
> Per eseguire questa procedura, è necessario essere un membro del gruppo Domain Admins.

### <a name="BKMK_4"></a>Per un sito e sono in un controller di dominio o in una workstation con strumenti di amministrazione Server remota installato

1.  Aprire Console Gestione criteri di gruppo.

2.  Nell'albero della console, fare clic sul sito che si desidera impostare criteri di gruppo.

    **Dove?**

    -   Servizi e siti di Active Directory [*nome_controller_dominio. nome_dominio*] / Sites/sito

3.  Fare clic su una voce nella **collegamenti oggetti Criteri di gruppo** per selezionare un oggetto esistente di criteri di gruppo (GPO) e quindi fare clic su **modificare**. È inoltre possibile fare clic su **Nuovo** per creare un nuovo oggetto Criteri di gruppo e quindi fare clic su **Modifica**.

4.  Nell'albero della console, fare clic su **criteri di restrizione Software**.

    **Where**

    -   *Oggetto Criteri di gruppo* [*ComputerName*] configurazione Computer/criteri o

        Criteri di restrizione impostazioni/Software o della sicurezza delle impostazioni utente configurazione/Windows

> [!NOTE]
> -   Per eseguire questa procedura è necessario essere membri del gruppo Administrators nel computer locale oppure disporre della delega per l'autorità appropriata. Se il computer fa parte di un dominio, i membri del gruppo Domain Admins potrebbero essere in grado di eseguire questa procedura.
> -   Per configurare le impostazioni di criteri che verranno applicate ai computer, indipendentemente dal fatto che all'accesso degli utenti, fare clic su **configurazione Computer**.
> -   Per configurare le impostazioni di criteri che verranno applicate agli utenti, indipendentemente dal computer a cui accede, fare clic su **configurazione utente**.

## <a name="BKMK_Create_SRP"></a>Per creare nuovi criteri di restrizione software

1.  Aprire Criteri restrizione software.

2.  Scegliere **Nuovi criteri restrizione software** dal menu **Azione**.

> [!WARNING]
> -   Per eseguire questa procedura sono necessarie credenziali amministrative diverse, a seconda dell'ambiente:
> 
>     -   Per creare nuovi criteri di restrizione software nel computer locale: Appartenenza al gruppo locale **amministratori** o gruppo equivalente, è il requisito minimo necessario per completare questa procedura.
>     -   Per creare nuovi criteri di restrizione software per un computer aggiunto a un dominio, la procedura può essere eseguita dai membri del gruppo Domain Admins.
> -   Se sono già stati creati criteri di restrizione software per un oggetto Criteri di gruppo, il comando **Nuovi criteri restrizione software** non compare nel menu **Azione**. Per eliminare i criteri di restrizione software applicati a un oggetto Criteri di gruppo, nell'albero della console fare clic con il pulsante destro del mouse su **Criteri restrizione software** e quindi scegliere **Elimina criteri restrizione software**. Quando si elimina i criteri di restrizione software per un oggetto Criteri di gruppo, vengono eliminati anche tutte le regole di criteri di restrizione software per quell'oggetto. Dopo aver eliminato i criteri di restrizione software, è possibile creare nuovi criteri per l'oggetto Criteri di gruppo in questione.

## <a name="BKMK_Add_Del"></a>Per aggiungere o eliminare un tipo di file

1.  Aprire Criteri restrizione software.

2.  Nel riquadro dei dettagli fare doppio clic su **Tipi di file designati**.

3.  Effettua una delle seguenti operazioni:

    -   Per aggiungere un tipo di file, in **Estensione file** digitare l'estensione del nome di file e quindi fare clic su **Aggiungi**.

    -   Per eliminare un tipo di file, in **Tipi di file designati** fare clic sul tipo di file e quindi fare clic su **Rimuovi**.

> [!NOTE]
> -   Per eseguire questa procedura sono necessarie credenziali amministrative diverse, a seconda dell'ambiente in cui si aggiunge o elimina un tipo di file designato:
> 
>     -   Se si aggiunge o elimina un tipo di file designato per il computer locale: Appartenenza al gruppo locale **amministratori** o gruppo equivalente, è il requisito minimo necessario per completare questa procedura.
>     -   Per creare nuovi criteri di restrizione software per un computer aggiunto a un dominio, la procedura può essere eseguita dai membri del gruppo Domain Admins.
> -   Potrebbe essere necessario creare una nuova impostazione dei criteri di restrizione software per l'oggetto Criteri di gruppo, se non è ancora stato fatto.
> -   L'elenco dei tipi di file designati è condiviso da tutte le regole per i Computer e configurazione utente per un oggetto Criteri di gruppo.

## <a name="BKMK_Prevent_Admin"></a>Per impedire che i criteri di restrizione software applicati agli amministratori locali

1.  Aprire Criteri restrizione software.

2.  Nel riquadro dei dettagli fare doppio clic su **Imposizione**.

3.  In **Applica criteri restrizione software a** fare clic su **Tutti gli utenti, esclusi gli amministratori locali**.

> [!WARNING]
> -   Appartenenza al gruppo locale **amministratori** o gruppo equivalente, è il requisito minimo necessario per completare questa procedura.
> -   Potrebbe essere necessario creare una nuova impostazione dei criteri di restrizione software per l'oggetto Criteri di gruppo, se non è ancora stato fatto.
> -   Se è comune per gli utenti essere membri del gruppo Administrators locale nei loro computer nell'organizzazione, potrebbe essere opportuno evitare di abilitare questa opzione.
> -   Per definire un'impostazione dei criteri di restrizione software per il computer locale, utilizzare questa procedura per evitare l'applicazione dei criteri di restrizione software agli amministratori locali. Se si sta definendo un'impostazione di criteri di restrizione software per la rete, filtrare le impostazioni di criteri utente basate sull'appartenenza a gruppi di sicurezza tramite criteri di gruppo.

## <a name="BKMK_Sec_Lvl"></a>Per modificare il livello di sicurezza predefinito dei criteri di restrizione software

1.  Aprire Criteri restrizione software.

2.  Nel riquadro dei dettagli fare doppio clic su **Livelli di sicurezza**.

3.  Fare clic con il pulsante destro del mouse sul livello di sicurezza che si desidera impostare come predefinito e quindi scegliere **Imposta come predefinito**.

> [!CAUTION]
> In determinate directory, l'impostazione del livello di sicurezza predefinito su **Non consentito** può influire negativamente sul sistema operativo.

> [!NOTE]
> -   Per eseguire questa procedura sono necessarie credenziali amministrative diverse, a seconda dell'ambiente in cui si modifica il livello di sicurezza predefinito dei criteri di restrizione software.
> -   Potrebbe essere necessario creare una nuova impostazione dei criteri di restrizione software per l'oggetto Criteri di gruppo, se non è ancora stato fatto.
> -   Nel riquadro dei dettagli, il livello di sicurezza predefinito corrente è indicato da un cerchio nero con un segno di spunta all'interno. Se si fa clic con il pulsante destro del mouse sul livello di sicurezza predefinito corrente, il comando **Imposta come predefinito** non compare nel menu.
> -   Vengono create regole dei criteri di restrizione software consentono di specificare eccezioni a livello di sicurezza predefinito. Quando il livello di sicurezza predefinito è impostato su **Senza restrizioni**, è possibile utilizzare le regole per specificare il software di cui non è consentita l'esecuzione. Quando il livello di sicurezza predefinito è impostato su **Non consentito**, è possibile utilizzare le regole per specificare il software di cui è consentita l'esecuzione.
> -   Al momento dell'installazione, il livello di sicurezza predefinito per i criteri di restrizione software per tutti i file nel sistema è impostato su **Senza restrizioni**.

## <a name="BKMK_Apply_SRP_DLLs"></a>Per applicare i criteri di restrizione software alle DLL

1.  Aprire Criteri restrizione software.

2.  Nel riquadro dei dettagli fare doppio clic su **Imposizione**.

3.  In **Applica criteri restrizione software a** fare clic su **Tutti i file nel software**.

> [!NOTE]
> -   Per eseguire questa procedura è necessario essere membri del gruppo Administrators nel computer locale oppure disporre della delega per l'autorità appropriata. Se il computer fa parte di un dominio, i membri del gruppo Domain Admins potrebbero essere in grado di eseguire questa procedura.
> -   Per impostazione predefinita, i criteri di restrizione software non prevedono il controllo di librerie di collegamento dinamico (DLL). Il controllo delle DLL può ridurre le prestazioni del sistema, perché la valutazione dei criteri di restrizione software deve avvenire a ogni caricamento di una DLL. Si potrebbe comunque decidere di controllare le DLL se si teme la ricezione di un virus progettato per le DLL. Se il livello di sicurezza predefinito è impostato su **vietate**, e si Abilita controllo delle DLL, è necessario creare regole di criteri che consentono di eseguire ogni DLL restrizione software.


