---
title: Utilizzo delle regole dei criteri di restrizione Software
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a8047d5-9bb9-4bed-bc8f-583a237731e2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 2dd1810b50f4f02be99eb2e2c0893501f99d1e93
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="work-with-software-restriction-policies-rules"></a>Utilizzo delle regole dei criteri di restrizione Software

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento descrive le procedure di utilizzo di certificato, percorso, internet zona e hash regole utilizzando criteri di restrizione Software.

## <a name="introduction"></a>Introduzione
I criteri di restrizione software, è possibile proteggere l'ambiente informatico da software non attendibile identificando e specificando quale software è consentita l'esecuzione. È possibile definire un livello di sicurezza predefinito **senza restrizioni** o **non consentito** per un oggetto Criteri di gruppo (GPO) in modo da essere concessa software o non consentito per impostazione predefinita. È possibile creare eccezioni per questo livello di sicurezza predefinito mediante la creazione di regole per uno specifico software dei criteri di restrizione software. Ad esempio, se il livello di sicurezza predefinito è impostato su **non consentito**, è possibile creare regole che consentono l'esecuzione di software specifico. I tipi di regole sono i seguenti:

-   **Regole certificati**

    Per le procedure, vedere [utilizzo delle regole certificati](#BKMK_Cert_Rules).

-   **Regole hash**

    Per le procedure, vedere [utilizzo delle regole hash](#BKMK_Hash_Rules).

-   **Regole area Internet**

    Per le procedure, vedere [utilizzo delle regole area Internet](#BKMK_Internet_Zone_Rules).

-   **Regole di percorso**

    Per le procedure, vedere [utilizzo delle regole di percorso](#BKMK_Path_Rules).

Per informazioni su altre attività per gestire criteri di restrizione Software, vedere [amministrare i criteri di restrizione Software](administer-software-restriction-policies.md).

## <a name="BKMK_Cert_Rules"></a>Utilizzo delle regole certificati
Criteri di restrizione software possono anche identificare il software per il certificato di firma. È possibile creare una regola certificato che identifica il software e quindi consente o non consentire l'esecuzione, a seconda del livello di sicurezza del software. Ad esempio, è possibile utilizzare le regole certificati possono considerare automaticamente attendibile software da una fonte attendibile in un dominio senza chiedere conferma all'utente. È anche possibile utilizzare le regole certificati di eseguire i file nelle aree non consentite del sistema operativo. Le regole certificati non abilitate per impostazione predefinita.

Quando si creano regole per il dominio mediante criteri di gruppo, è necessario disporre delle autorizzazioni per creare o modificare un oggetto Criteri di gruppo. Se si creano regole per il computer locale, è necessario disporre di credenziali amministrative nel computer.

#### <a name="to-create-a-certificate-rule"></a>Per creare una regola certificato

1.  Aprire criteri di restrizione Software.

2.  Nell'albero della console o il riquadro dei dettagli, fare doppio clic su **regole aggiuntive**, quindi fare clic su **nuova regola certificato**.

3.  Fare clic su **Sfoglia**, quindi selezionare un certificato o un file firmato.

4.  In **livello di sicurezza**, fare clic su **non consentito** o **senza restrizioni**.

5.  In **descrizione**, digitare una descrizione per questa regola e quindi fare clic su **OK**.

> [!NOTE]
> -   Potrebbe essere necessario creare una nuova impostazione di criteri di restrizione software per l'oggetto Criteri di gruppo (GPO) se non è già stato fatto.
> -   Le regole certificati non abilitate per impostazione predefinita.
> -   Gli unici tipi di file interessati dalle regole certificati sono quelli elencati in **tipi di file designati** nel riquadro dei dettagli di criteri restrizione Software. È disponibile un elenco di tipi di file designati condivisi da tutte le regole.
> -   Per i criteri di restrizione software rendere effettiva, gli utenti devono aggiornare le impostazioni di criteri disconnettersi e accedere ai computer.
> -   Quando più di una regola dei criteri di restrizione software viene applicata alle impostazioni dei criteri, esiste una precedenza delle regole per gestire i conflitti.

### <a name="enabling-certificate-rules"></a>Abilitazione delle regole certificati
Esistono procedure diverse per l'abilitazione delle regole certificati a seconda dell'ambiente:

-   [Per il computer locale](#BKMK_1)

-   [Per un oggetto Criteri di gruppo e sono in un server che fa parte di un dominio](#BKMK_2)

-   [Per un oggetto Criteri di gruppo e sono in un controller di dominio o una workstation con strumenti di amministrazione Server remota installato su](#BKMK_3)

-   [Per solo dominio controller e sono in un controller di dominio o in una workstation con il pacchetto di strumenti di amministrazione Server remota installato](#BKMK_4)

#### <a name="BKMK_1"></a>Per abilitare le regole certificati per il computer locale

1.  Apri le impostazioni di sicurezza locali.

2.  Nell'albero della console, fare clic su **opzioni di protezione** in sicurezza impostazioni/criteri locali.

3.  Nel riquadro dei dettagli fare doppio clic su **le impostazioni di sistema: utilizza regole certificati con file eseguibili di Windows per i criteri di restrizione Software**.

4.  Eseguire una delle operazioni seguenti e quindi fare clic su **OK**:

    -   Per abilitare le regole certificati, fare clic su **abilitato**.

    -   Per disabilitare le regole certificati, fare clic su **disabilitato**.

#### <a name="BKMK_2"></a>Per abilitare le regole certificati per un oggetto Criteri di gruppo e sono in un server che fa parte di un dominio

1.  Aprire Microsoft Management Console (MMC).

2.  Nel **File** menu, fare clic su **Aggiungi/Rimuovi snap-in**, quindi fare clic su **Aggiungi**.

3.  Fare clic su **Editor oggetti Criteri di gruppo locali**, quindi fare clic su **Aggiungi**.

4.  In **Selezione oggetto Criteri di gruppo**, fare clic su **Sfoglia**.

5.  In **cercare un oggetto Criteri di gruppo**, selezionare un oggetto Criteri di gruppo (GPO) nel dominio appropriato, sito o unità organizzativa- o crearne uno nuovo e quindi fare clic su **fine**.

6.  Fare clic su **Chiudi**, quindi fare clic su **OK**.

7.  Nell'albero della console, fare clic su **opzioni di protezione** in *Oggettocriteridigruppo* [*ComputerName*] criteri Computer Configuration/Windows/protezione impostazioni impostazioni/criteri locali /.

8.  Nel riquadro dei dettagli fare doppio clic su **le impostazioni di sistema: utilizza regole certificati con file eseguibili di Windows per i criteri di restrizione Software**.

9. Se questa impostazione non è ancora stata definita, selezionare il **definire queste impostazioni dei criteri** casella di controllo.

10. Eseguire una delle operazioni seguenti e quindi fare clic su **OK**:

    -   Per abilitare le regole certificati, fare clic su **abilitato**.

    -   Per disabilitare le regole certificati, fare clic su **disabilitato**.

#### <a name="BKMK_3"></a>Per abilitare certificato regole per un oggetto Criteri di gruppo e si utilizza un controller di dominio o in una workstation con strumenti di amministrazione Server remota installato

1.  Aprire utenti e computer Active Directory.

2.  Nell'albero della console fare doppio clic sull'oggetto di criteri di gruppo (GPO) per il quale si desidera abilitare le regole certificati.

3.  Fare clic su **proprietà**, quindi fare clic su di **criteri di gruppo** scheda.

4.  Fare clic su **modifica** per aprire l'oggetto Criteri di gruppo che si desidera modificare. È anche possibile fare clic su **New** per creare un nuovo oggetto Criteri di gruppo e quindi fare clic su **modifica**.

5.  Nell'albero della console, fare clic su **opzioni di protezione** in *Oggettocriteridigruppo*[*ComputerName*] criteri Computer Configuration/Windows/protezione impostazioni impostazioni/criteri locali.

6.  Nel riquadro dei dettagli fare doppio clic su **le impostazioni di sistema: utilizza regole certificati con file eseguibili di Windows per i criteri di restrizione Software**.

7.  Se questa impostazione non è ancora stata definita, selezionare il **definire queste impostazioni dei criteri** casella di controllo.

8.  Eseguire una delle operazioni seguenti e quindi fare clic su **OK**:

    -   Per abilitare le regole certificati, fare clic su **abilitato**.

    -   Per disabilitare le regole certificati, fare clic su **disabilitato**.

#### <a name="BKMK_4"></a>Per abilitare le regole certificati solo per i controller di dominio e si presenti in un controller di dominio o in una workstation con strumenti di amministrazione Server remota installato

1.  Aprire impostazioni di protezione Controller di dominio.

2.  Nell'albero della console, fare clic su **opzioni di protezione** in *Oggettocriteridigruppo* [*ComputerName*] criteri Computer Configuration/Windows/protezione impostazioni impostazioni/criteri locali.

3.  Nel riquadro dei dettagli fare doppio clic su **le impostazioni di sistema: utilizza regole certificati con file eseguibili di Windows per i criteri di restrizione Software**.

4.  Se questa impostazione non è ancora stata definita, selezionare il **definire queste impostazioni dei criteri** casella di controllo.

5.  Eseguire una delle operazioni seguenti e quindi fare clic su **OK**:

    -   Per abilitare le regole certificati, fare clic su **abilitato**.

    -   Per disabilitare le regole certificati, fare clic su **disabilitato**.

> [!NOTE]
> È necessario eseguire questa procedura prima che le regole certificati diventino effettive.

### <a name="set-trusted-publisher-options"></a>Impostare le opzioni degli autori attendibili
Firma del software viene utilizzata da un numero crescente di autori di software e sviluppatori di applicazioni per verificare che le loro applicazioni provengono da una fonte attendibile. Molti utenti tuttavia non comprendere o prestano grande attenzione ai certificati di firma associati alle applicazioni che installano.

Le impostazioni dei criteri nel **autori attendibili** scheda dei criteri di convalida percorso del certificato consente agli amministratori di determinare quali certificati possono essere accettati in quanto provenienti da un autore attendibile.

##### <a name="to-configure-the-trusted-publishers-policy-settings-for-a-local-computer"></a>Per configurare le impostazioni dei criteri autori attendibili per un computer locale

1.  Nel **Start** digitare**gpedit. msc** e quindi premere INVIO.

2.  Nell'albero della console in **locale Computer locale\Configurazione computer\Impostazioni di Windows\impostazioni**, fare clic su **criteri chiave pubblica**.

3.  Fare doppio clic su **impostazioni di convalida percorso certificati**, quindi fare clic su di **autori attendibili** scheda.

4.  Selezionare il **definire queste impostazioni dei criteri** casella di controllo, seleziona le impostazioni dei criteri che si desidera applicare, quindi fare clic su **OK** per applicare le nuove impostazioni.

##### <a name="to-configure-the-trusted-publishers-policy-settings-for-a-domain"></a>Per configurare le impostazioni dei criteri autori attendibili per un dominio

1.  Apri **Gestione criteri di gruppo**.

2.  Nell'albero della console, fare doppio clic su **oggetti Criteri di gruppo** nella foresta e nel dominio contenente il **criterio dominio predefinito** oggetto Criteri di gruppo (GPO) che si desidera modificare.

3.  Fare doppio clic su di **criterio dominio predefinito** oggetto Criteri di gruppo, quindi fare clic su **modifica**.

4.  Nell'albero della console in **Configurazione computer\Impostazioni di Windows\impostazioni**, fare clic su **criteri chiave pubblica**.

5.  Fare doppio clic su **impostazioni di convalida percorso certificati**, quindi fare clic su di **autori attendibili** scheda.

6.  Selezionare il **definire queste impostazioni dei criteri** casella di controllo, seleziona le impostazioni dei criteri che si desidera applicare, quindi fare clic su **OK** per applicare le nuove impostazioni.

##### <a name="to-allow-only-administrators-to-manage-certificates-used-for-code-signing-for-a-local-computer"></a>Per consentire soltanto agli amministratori di gestire i certificati utilizzati per la firma del codice per un computer locale

1.  Nel **Start** dello schermo, tipo, **gpedit. msc** nel **Cerca programmi e file** o in Windows 8, sul Desktop e quindi premere INVIO.

2.  Nell'albero della console in **criterio dominio predefinito** o **criteri del Computer locale**, fare doppio clic su **configurazione Computer**, **le impostazioni di Windows**, e **le impostazioni di sicurezza**, quindi fare clic su **criteri chiave pubblica**.

3.  Fare doppio clic su **impostazioni di convalida percorso certificati**, quindi fare clic su di **autori attendibili** scheda.

4.  Selezionare il **definire queste impostazioni dei criteri** casella di controllo.

5.  In **gestione autori attendibili**, fare clic su **Consenti solo agli amministratori di gestire autori attendibili**, quindi fare clic su **OK** per applicare le nuove impostazioni.

##### <a name="to-allow-only-administrators-to-manage-certificates-used-for-code-signing-for-a-domain"></a>Per consentire soltanto agli amministratori di gestire i certificati utilizzati per la firma del codice per un dominio

1.  Apri **Gestione criteri di gruppo**.

2.  Nell'albero della console, fare doppio clic su **oggetti Criteri di gruppo** nella foresta e nel dominio contenente il **criterio dominio predefinito** oggetto Criteri di gruppo che si desidera modificare.

3.  Fare doppio clic su di **criterio dominio predefinito** oggetto Criteri di gruppo, quindi fare clic su **modifica**.

4.  Nell'albero della console in **Configurazione computer\Impostazioni di Windows\impostazioni**, fare clic su **criteri chiave pubblica**.

5.  Fare doppio clic su **impostazioni di convalida percorso certificati**, quindi fare clic su di **autori attendibili** scheda.

6.  Selezionare il **definire queste impostazioni dei criteri** casella di controllo, implementare le modifiche e quindi fare clic su **OK** per applicare le nuove impostazioni.

## <a name="BKMK_Hash_Rules"></a>Utilizzo delle regole hash
Un hash è una serie di byte con lunghezza fissa che identifica in modo univoco un programma software o un file. L'hash viene calcolato un algoritmo hash. Quando viene creata una regola di hash per un programma software, criteri di restrizione software calcolano un hash del programma. Quando un utente tenta di aprire un programma software, un hash del programma viene confrontato con le regole hash esistenti per i criteri di restrizione software. L'hash di un programma software è sempre lo stesso, indipendentemente dalla posizione del programma nel computer. Tuttavia, se un programma software viene modificato in alcun modo, il relativo hash cambia anche e non corrisponde più all'hash nella regola hash per i criteri di restrizione software.

Ad esempio, è possibile creare una regola hash e impostare la protezione di livello **non consentito** impedire agli utenti di eseguire un determinato file. Un file può essere rinominato o spostato in un'altra cartella e che comunque risultato lo stesso hash. Tuttavia, anche le modifiche al file stesso modifica il valore hash e consentono al file di eludere le restrizioni.

#### <a name="to-create-a-hash-rule"></a>Per creare una regola hash

1.  Aprire criteri di restrizione Software.

2.  Nell'albero della console o il riquadro dei dettagli, fare doppio clic su **regole aggiuntive**, quindi fare clic su **nuova regola Hash**.

3.  Fare clic su **Sfoglia** per trovare un file.

    > [!NOTE]
    > In Windows XP è possibile incollare un hash pre-calcolato in **hash File**. In Windows Server 2008 R2, Windows 7 e versioni successive, questa opzione non è disponibile.

4.  In **livello di sicurezza**, fare clic su **non consentito** o **senza restrizioni**.

5.  In **descrizione**, digitare una descrizione per questa regola e quindi fare clic su **OK**.

> [!NOTE]
> -   Potrebbe essere necessario creare una nuova impostazione di criteri di restrizione software per l'oggetto Criteri di gruppo (GPO) se non è già stato fatto.
> -   È possibile creare una regola hash per un virus o un Trojan horse per impedire l'esecuzione.
> -   Se si desidera che altri utenti di usare una regola hash in modo che non è possibile eseguire un virus, calcolare l'hash del virus tramite criteri di restrizione software e quindi il valore hash agli altri utenti di posta elettronica. Mai tramite posta elettronica il virus stesso.
> -   Se un virus è stato inviato tramite posta elettronica, è anche possibile creare una regola di percorso per impedire l'esecuzione degli allegati di posta elettronica.
> -   Un file che viene rinominato o spostato in un'altra cartella mantiene lo stesso hash. Qualsiasi modifica al file stesso determina un hash diverso.
> -   Gli unici tipi di file interessati dalle regole hash sono quelli elencati in **tipi di File designati** nel riquadro dei dettagli di criteri restrizione Software. È disponibile un elenco di tipi di file designati condivisi da tutte le regole.
> -   Per i criteri di restrizione software rendere effettiva, gli utenti devono aggiornare le impostazioni di criteri disconnettersi e accedere ai computer.
> -   Quando più di una regola dei criteri di restrizione software viene applicata alle impostazioni dei criteri, esiste una precedenza delle regole per gestire i conflitti.

## <a name="BKMK_Internet_Zone_Rules"></a>Utilizzo delle regole area Internet
Regole area Internet si applicano solo ai pacchetti di Windows Installer. Una regola di area può identificare il software proveniente da un'area specificata tramite Internet Explorer. Queste aree sono Internet, intranet locale, siti con restrizioni, siti attendibili e risorse del Computer. Una regola area Internet è progettata per impedire agli utenti di scaricare e installare software.

#### <a name="to-create-an-internet-zone-rule"></a>Per creare una regola area Internet

1.  Aprire criteri di restrizione Software.

2.  Nell'albero della console o il riquadro dei dettagli, fare doppio clic su **regole aggiuntive**, quindi fare clic su **nuova regola area Internet**.

3.  In **area Internet**, fare clic su un'area Internet.

4.  In **livello di sicurezza**, fare clic su **non consentito** o **senza restrizioni**, quindi fare clic su **OK**.

> [!NOTE]
> -   Potrebbe essere necessario creare una nuova impostazione di criteri di restrizione software per l'oggetto Criteri di gruppo (GPO) se non è già stato fatto.
> -   Regole area si applicano solo ai file con estensione msi, che sono i pacchetti di Windows Installer.
> -   Per i criteri di restrizione software rendere effettiva, gli utenti devono aggiornare le impostazioni di criteri disconnettersi e accedere ai computer.
> -   Quando più di una regola dei criteri di restrizione software viene applicata alle impostazioni dei criteri, esiste una precedenza delle regole per gestire i conflitti.

## <a name="BKMK_Path_Rules"></a>Utilizzo delle regole di percorso
Una regola di percorso identifica il software in base al relativo percorso. Ad esempio, se si dispone di un computer che dispone di un livello di sicurezza predefinito **non consentito**, è comunque possibile concedere l'accesso senza restrizioni a una cartella specifica per ogni utente. È possibile creare una regola di percorso utilizzando il percorso del file e impostando il livello di sicurezza della regola di percorso per **senza restrizioni**. Alcuni percorsi comuni per questo tipo di regola sono % userprofile %, % windir %, % appdata %, % programfiles % e % temp %. È inoltre possibile creare del Registro di sistema delle regole di percorso che utilizzano la chiave del Registro di sistema del software come percorso.

Poiché queste regole sono specificate nel percorso, se un programma software viene spostato, la regola di percorso non è più applicabile.

#### <a name="to-create-a-path-rule"></a>Per creare una regola di percorso

1.  Aprire criteri di restrizione Software.

2.  Nell'albero della console o il riquadro dei dettagli, fare doppio clic su **regole aggiuntive**, quindi fare clic su **nuova regola percorso**.

3.  In **percorso**, digitare un percorso o fare clic su **Sfoglia** per trovare un file o una cartella.

4.  In **livello di sicurezza**, fare clic su **non consentito** o **senza restrizioni**.

5.  In **descrizione**, digitare una descrizione per questa regola e quindi fare clic su **OK**.

> [!CAUTION]
> -   In determinate cartelle, ad esempio la cartella di Windows, impostazione il livello di sicurezza per **non consentito** può influire negativamente sul funzionamento del sistema operativo. Assicurarsi di non disattivare un componente cruciale del sistema operativo o uno dei programmi dipendenti.

> [!NOTE]
> -   Potrebbe essere necessario creare nuovi criteri di restrizione software per l'oggetto Criteri di gruppo (GPO) se non è già stato fatto.
> -   Se si crea una regola di percorso per il software con una protezione livello di **non consentito**, gli utenti possono comunque eseguire il software copiandolo in un'altra posizione.
> -   I caratteri jolly sono supportati dalla regola di percorso sono * e?.
> -   È possibile utilizzare variabili di ambiente, ad esempio % programfiles % o % systemroot %, nella regola di percorso.
> -   Se si desidera creare una regola di percorso per il software quando non si conosce in cui viene archiviato in un computer, ma hai la chiave del Registro di sistema, è possibile creare una regola di percorso del Registro di sistema.
> -   Per impedire agli utenti di eseguire gli allegati di posta elettronica, è possibile creare una regola di percorso per directory degli allegati del programma di posta elettronica che impedisce agli utenti di eseguire gli allegati di posta elettronica.
> -   Gli unici tipi di file interessati dalle regole di percorso sono quelli elencati in **tipi di File designati** nel riquadro dei dettagli di criteri restrizione Software. È disponibile un elenco di tipi di file designati condivisi da tutte le regole.
> -   Per i criteri di restrizione software rendere effettiva, gli utenti devono aggiornare le impostazioni di criteri disconnettersi e accedere ai computer.
> -   Quando più di una regola dei criteri di restrizione software viene applicata alle impostazioni dei criteri, esiste una precedenza delle regole per gestire i conflitti.

#### <a name="to-create-a-registry-path-rule"></a>Per creare una regola di percorso del Registro di sistema

1.  Nel **Start** digitare regedit.

2.  Nell'albero della console fare doppio clic sulla chiave del Registro di sistema che si desidera creare una regola per e quindi fare clic su **Copia nome chiave**. Annotare il nome del valore nel riquadro dei dettagli.

3.  Aprire criteri di restrizione Software.

4.  Nell'albero della console o il riquadro dei dettagli, fare doppio clic su **regole aggiuntive**, quindi fare clic su **nuova regola percorso**.

5.  In **percorso**, incollare il nome della chiave del Registro di sistema, seguito dal nome del valore.

6.  Racchiudere il percorso del Registro di sistema tra segni di percentuale (%)), ad esempio, % HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PlatformSDK\Directories\InstallDir%.

7.  In **livello di sicurezza**, fare clic su **non consentito** o **senza restrizioni**.

8.  In **descrizione**, digitare una descrizione per questa regola e quindi fare clic su **OK**.


