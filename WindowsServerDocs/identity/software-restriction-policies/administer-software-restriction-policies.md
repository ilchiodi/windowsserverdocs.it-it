---
title: Gestire i criteri di restrizione software
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c75c7813041870f79ed95250857a5c7d1576c7dc
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322983"
---
# <a name="administer-software-restriction-policies"></a>Gestire i criteri di restrizione software

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento per i professionisti IT contiene le procedure per amministrare i criteri di controllo delle applicazioni usando criteri di restrizione software a partire da Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introduzione
I criteri di restrizione software sono una funzionalità basata su Criteri di gruppo che identifica i programmi software in esecuzione nei computer di un dominio e controlla la possibilità di tali programmi di essere eseguiti. È possibile utilizzare i criteri di restrizione software per creare una configurazione molto restrittiva per i computer, in cui consentire l'esecuzione solo di applicazioni specifiche identificate. Queste sono integrate con Microsoft Active Directory Domain Services e Criteri di gruppo ma possono essere configurate anche nei computer autonomi. Per ulteriori informazioni su SRP, vedere [criteri di restrizione software](software-restriction-policies.md).

A partire da Windows Server 2008 R2 e Windows 7, è possibile utilizzare Windows AppLocker anziché o in concerto con SRP per una parte della strategia di controllo delle applicazioni.

Questo argomento contiene:

-   [Per aprire Criteri di restrizione software](#BKMK_Open_SRP)

-   [Per creare nuovi criteri di restrizione software](#BKMK_Create_SRP)

-   [Per aggiungere o eliminare un tipo di file designato](#BKMK_Add_Del)

-   [Per impedire l'applicazione di criteri di restrizione software agli amministratori locali](#BKMK_Prevent_Admin)

-   [Per modificare il livello di sicurezza predefinito dei criteri di restrizione software](#BKMK_Sec_Lvl)

-   [Per applicare i criteri di restrizione software alle dll](#BKMK_Apply_SRP_DLLs)

Per informazioni su come eseguire attività specifiche utilizzando il software di RESTRIzione software, vedere gli argomenti seguenti:

-   [Determinare l'elenco Consenti-Nega e l'inventario delle applicazioni per i criteri di restrizione software](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

-   [Usare le regole dei criteri di restrizione software](work-with-software-restriction-policies-rules.md)

-   [Usare i criteri di restrizione software per proteggere il computer da un virus di posta elettronica](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

## <a name="BKMK_Open_SRP"></a>Per aprire Criteri di restrizione software

-   [Per il computer locale](#BKMK_1)

-   [Per un dominio, un sito o un'unità organizzativa e l'utente si trova in un server membro o in una workstation aggiunta a un dominio](#BKMK_2)

-   [Per un dominio o un'unità organizzativa e si è in un controller di dominio o in una workstation in cui è installato il Strumenti di amministrazione remota del server](#BKMK_3)

-   [Per un sito di e si è in un controller di dominio o in una workstation in cui è installato il Strumenti di amministrazione remota del server](#BKMK_4)

### <a name="BKMK_1"></a>Per il computer locale

1.  Aprire Impostazioni sicurezza locale.

2.  Nell'albero della console fare clic su **criteri di restrizione software**.

    **In cui?**

    -   Impostazioni di sicurezza/Criteri di restrizione software

> [!NOTE]
> Per eseguire questa procedura, è necessario essere membro del gruppo Administrators sul computer locale o aver ricevuto in delega l'autorizzazione appropriata.

### <a name="BKMK_2"></a>Per un dominio, un sito o un'unità organizzativa e l'utente si trova in un server membro o in una workstation aggiunta a un dominio

1.  Aprire Microsoft Management Console (MMC).

2.  Scegliere **Aggiungi/Rimuovi snap-in** dal menu **File** e quindi fare clic su **Aggiungi**.

3.  Fare clic su **Editor oggetti Criteri di gruppo locale** e quindi fare clic su **Aggiungi**.

4.  In **Selezione oggetto Criteri di gruppo**  fare clic su **Sfoglia**.

5.  In **Cerca un oggetto Criteri di gruppo**selezionare un oggetto Criteri di gruppo (GPO) nel dominio appropriato, nel sito o nell'unità organizzativa oppure crearne uno nuovo, quindi fare clic su **fine**.

6.  Fare clic su **Chiudi**e quindi su **OK**.

7.  Nell'albero della console fare clic su **criteri di restrizione software**.

    **In cui?**

    -   *Criteri di gruppo oggetto* [*nomecomputer*] criterio/Configurazione computer o

        Configurazione utente/impostazioni di Windows/impostazioni di sicurezza/Criteri di restrizione software

> [!NOTE]
> Per eseguire questa procedura, è necessario essere un membro del gruppo Domain Admins.

### <a name="BKMK_3"></a>Per un dominio o un'unità organizzativa e si è in un controller di dominio o in una workstation in cui è installato il Strumenti di amministrazione remota del server

1.  Aprire Console Gestione Criteri di gruppo.

2.  Nell'albero della console fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo (GPO) per cui si desidera aprire i criteri di restrizione software.

3.  Fare clic su **Modifica** per aprire l'oggetto Criteri di gruppo che si desidera modificare. È inoltre possibile fare clic su **Nuovo** per creare un nuovo oggetto Criteri di gruppo e quindi fare clic su **Modifica**.

4.  Nell'albero della console fare clic su **criteri di restrizione software**.

    **In cui?**

    -   *Criteri di gruppo oggetto* [*nomecomputer*] criterio/Configurazione computer o

        Configurazione utente/impostazioni di Windows/impostazioni di sicurezza/Criteri di restrizione software

> [!NOTE]
> Per eseguire questa procedura, è necessario essere un membro del gruppo Domain Admins.

### <a name="BKMK_4"></a>Per un sito di e si è in un controller di dominio o in una workstation in cui è installato il Strumenti di amministrazione remota del server

1.  Aprire Console Gestione Criteri di gruppo.

2.  Nell'albero della console fare clic con il pulsante destro del mouse sul sito per il quale si desidera impostare Criteri di gruppo.

    **In cui?**

    -   Siti e servizi di Active Directory [*Domain_Controller_Name. domain_name*]/sites/site

3.  Fare clic su una voce in **criteri di gruppo collegamenti oggetto** per selezionare un oggetto Criteri di gruppo esistente (GPO), quindi fare clic su **modifica**. È inoltre possibile fare clic su **Nuovo** per creare un nuovo oggetto Criteri di gruppo e quindi fare clic su **Modifica**.

4.  Nell'albero della console fare clic su **criteri di restrizione software**.

    **In cui**

    -   *Criteri di gruppo oggetto* [*nomecomputer*] criterio/Configurazione computer o

        Configurazione utente/impostazioni di Windows/impostazioni di sicurezza/Criteri di restrizione software

> [!NOTE]
> -   Per eseguire questa procedura, è necessario essere membro del gruppo Administrators sul computer locale o aver ricevuto in delega l'autorizzazione appropriata. Se il computer appartiene a un dominio, i membri del gruppo Domain Admins potrebbero essere in grado di eseguire questa procedura.
> -   Per impostare le impostazioni dei criteri che verranno applicate ai computer, indipendentemente dall'utente che vi accede, fare clic su **Configurazione computer**.
> -   Per impostare le impostazioni dei criteri che verranno applicate agli utenti, indipendentemente dal computer a cui accedono, fare clic su **Configurazione utente**.

## <a name="BKMK_Create_SRP"></a>Per creare nuovi criteri di restrizione software

1.  Aprire Criteri restrizione software.

2.  Scegliere **Nuovi criteri restrizione software** dal menu **Azione**.

> [!WARNING]
> -   Per eseguire questa procedura sono necessarie credenziali amministrative diverse, a seconda dell'ambiente:
> 
>     -   Se si creano nuovi criteri di restrizione software per il computer locale: l'appartenenza al gruppo **Administrators** locale o a un gruppo equivalente è il requisito minimo necessario per completare questa procedura.
>     -   Per creare nuovi criteri di restrizione software per un computer aggiunto a un dominio, la procedura può essere eseguita dai membri del gruppo Domain Admins.
> -   Se sono già stati creati criteri di restrizione software per un oggetto Criteri di gruppo, il comando **Nuovi criteri restrizione software** non compare nel menu **Azione**. Per eliminare i criteri di restrizione software applicati a un oggetto Criteri di gruppo, nell'albero della console fare clic con il pulsante destro del mouse su **Criteri restrizione software** e quindi scegliere **Elimina criteri restrizione software**. Quando si eliminano i criteri di restrizione software per un oggetto Criteri di gruppo, vengono eliminate anche tutte le regole dei criteri di restrizione software. Dopo aver eliminato i criteri di restrizione software, è possibile creare nuovi criteri per l'oggetto Criteri di gruppo in questione.

## <a name="BKMK_Add_Del"></a>Per aggiungere o eliminare un tipo di file designato

1.  Aprire Criteri restrizione software.

2.  Nel riquadro dei dettagli fare doppio clic su **Tipi di file designati**.

3.  Esegui una delle operazioni seguenti:

    -   Per aggiungere un tipo di file, in **Estensione file** digitare l'estensione del nome di file e quindi fare clic su **Aggiungi**.

    -   Per eliminare un tipo di file, in **Tipi di file designati** fare clic sul tipo di file e quindi fare clic su **Rimuovi**.

> [!NOTE]
> -   Per eseguire questa procedura sono necessarie credenziali amministrative diverse, a seconda dell'ambiente in cui si aggiunge o elimina un tipo di file designato:
> 
>     -   Se si aggiunge o Elimina un tipo di file designato per il computer locale: l'appartenenza al gruppo **Administrators** locale o a un gruppo equivalente è il requisito minimo necessario per completare questa procedura.
>     -   Per creare nuovi criteri di restrizione software per un computer aggiunto a un dominio, la procedura può essere eseguita dai membri del gruppo Domain Admins.
> -   Potrebbe essere necessario creare una nuova impostazione dei criteri di restrizione software per l'oggetto Criteri di gruppo, se non è ancora stato fatto.
> -   L'elenco dei tipi di file designati è condiviso da tutte le regole per la configurazione del computer e la configurazione utente per un oggetto Criteri di gruppo.

## <a name="BKMK_Prevent_Admin"></a>Per impedire l'applicazione di criteri di restrizione software agli amministratori locali

1.  Aprire Criteri restrizione software.

2.  Nel riquadro dei dettagli fare doppio clic su **Imposizione**.

3.  In **Applica criteri restrizione software a** fare clic su **Tutti gli utenti, esclusi gli amministratori locali**.

> [!WARNING]
> -   L'appartenenza al gruppo **Administrators** locale, o equivalente, è il requisito minimo per completare questa procedura.
> -   Potrebbe essere necessario creare una nuova impostazione dei criteri di restrizione software per l'oggetto Criteri di gruppo, se non è ancora stato fatto.
> -   Se è comune per gli utenti essere membri del gruppo Administrators locale nei loro computer nell'organizzazione, potrebbe essere opportuno evitare di abilitare questa opzione.
> -   Per definire un'impostazione dei criteri di restrizione software per il computer locale, utilizzare questa procedura per evitare l'applicazione dei criteri di restrizione software agli amministratori locali. Se si definisce un'impostazione dei criteri di restrizione software per la rete, filtrare le impostazioni dei criteri utente in base all'appartenenza ai gruppi di sicurezza tramite Criteri di gruppo.

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
> -   Le regole dei criteri di restrizione software vengono create per specificare le eccezioni al livello di sicurezza predefinito. Quando il livello di sicurezza predefinito è impostato su **Senza restrizioni**, è possibile utilizzare le regole per specificare il software di cui non è consentita l'esecuzione. Quando il livello di sicurezza predefinito è impostato su **Non consentito**, è possibile utilizzare le regole per specificare il software di cui è consentita l'esecuzione.
> -   Al momento dell'installazione, il livello di sicurezza predefinito per i criteri di restrizione software per tutti i file nel sistema è impostato su **Senza restrizioni**.

## <a name="BKMK_Apply_SRP_DLLs"></a>Per applicare i criteri di restrizione software alle dll

1.  Aprire Criteri restrizione software.

2.  Nel riquadro dei dettagli fare doppio clic su **Imposizione**.

3.  In **Applica criteri restrizione software a** fare clic su **Tutti i file nel software**.

> [!NOTE]
> -   Per eseguire questa procedura, è necessario essere membro del gruppo Administrators sul computer locale o aver ricevuto in delega l'autorizzazione appropriata. Se il computer appartiene a un dominio, i membri del gruppo Domain Admins potrebbero essere in grado di eseguire questa procedura.
> -   Per impostazione predefinita, i criteri di restrizione software non prevedono il controllo di librerie di collegamento dinamico (DLL). Il controllo delle DLL può ridurre le prestazioni del sistema, perché la valutazione dei criteri di restrizione software deve avvenire a ogni caricamento di una DLL. Si potrebbe comunque decidere di controllare le DLL se si teme la ricezione di un virus progettato per le DLL. Se il livello di sicurezza predefinito è impostato su non **consentito**e si Abilita il controllo delle dll, è necessario creare regole dei criteri di restrizione software che consentano l'esecuzione di ogni dll.


