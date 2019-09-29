---
title: Usare le regole dei criteri di restrizione software
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: bb5e56fe541a06b1100de2f25fc10f4db46b8d24
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407159"
---
# <a name="work-with-software-restriction-policies-rules"></a>Usare le regole dei criteri di restrizione software

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento vengono descritte le procedure per l'utilizzo di certificati, percorsi, aree Internet e regole hash mediante i criteri di restrizione software.

## <a name="introduction"></a>Introduzione
I criteri di restrizione software consentono di proteggere l'ambiente informatico da software non attendibile identificando e specificando il software che è autorizzato a eseguire. È possibile definire un livello di sicurezza predefinito **senza restrizioni** o non **consentito** per un oggetto Criteri di gruppo (GPO) in modo che il software sia consentito o meno per l'esecuzione per impostazione predefinita. È possibile creare eccezioni a questo livello di sicurezza predefinito creando regole dei criteri di restrizione software per software specifico. Se il livello di sicurezza predefinito è impostato su **Non consentito**, ad esempio, è possibile creare regole per consentire l'esecuzione di software specifico. I tipi di regole sono i seguenti:

-   **Regole certificato**

    Per le procedure, vedere [Utilizzo delle regole certificati](#BKMK_Cert_Rules).

-   **Regole hash**

    Per le procedure, vedere [Utilizzo delle regole hash](#BKMK_Hash_Rules).

-   **Regole dell'area Internet**

    Per le procedure, vedere [utilizzo delle regole dell'area Internet](#BKMK_Internet_Zone_Rules).

-   **Regole percorso**

    Per le procedure, vedere [Utilizzo delle regole di percorso](#BKMK_Path_Rules).

Per informazioni su altre attività per gestire i criteri di restrizione software, vedere [amministrare i criteri di restrizione software](administer-software-restriction-policies.md).

## <a name="BKMK_Cert_Rules"></a>Utilizzo delle regole dei certificati
I criteri di restrizione software possono inoltre identificare il software tramite il certificato di firma. È possibile creare una regola certificato per identificare il software e quindi consentirne o meno l'esecuzione a seconda del livello di sicurezza. È possibile utilizzare le regole certificati, ad esempio, per impostare automaticamente come attendibile un'applicazione proveniente da una fonte attendibile in un dominio, senza richiedere conferma all'utente. È inoltre possibile utilizzare le regole certificati per eseguire file nelle aree non consentite del sistema operativo. Le regole certificati non sono abilitate per impostazione predefinita.

Quando vengono create regole per il dominio utilizzando Criteri di gruppo, è necessario disporre delle autorizzazioni per creare o modificare un oggetto Criteri di gruppo. Se si creano regole per il computer locale, è necessario disporre di credenziali amministrative per tale computer.

#### <a name="to-create-a-certificate-rule"></a>Per creare una regola certificato

1.  Aprire Criteri restrizione software.

2.  Nell'albero della console o nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **regole aggiuntive**e quindi scegliere **nuova regola certificato**.

3.  Fare clic su **Sfoglia** e quindi selezionare un file di certificato o un file firmato.

4.  In **livello di sicurezza**fare clic su non **consentito** o **senza restrizioni**.

5.  In **Descrizione** digitare una descrizione per la regola e quindi fare clic su **OK**.

> [!NOTE]
> -   Potrebbe essere necessario creare una nuova impostazione dei criteri di restrizione software per l'oggetto Criteri di gruppo, se non è già stato fatto.
> -   Le regole certificati non sono abilitate per impostazione predefinita.
> -   Gli unici tipi di file interessati dalle regole certificati sono quelli elencati in **Tipi di file designati** nel riquadro dei dettagli di Criteri restrizione software. È disponibile un elenco dei tipi di file designati condivisi da tutte le regole.
> -   Per rendere effettive i criteri di restrizione software, gli utenti devono aggiornare le impostazioni dei criteri disconnettendosi da e accedendo ai propri computer.
> -   Quando più di una regola dei criteri di restrizione software viene applicata alle impostazioni di criteri, esiste la precedenza delle regole per la gestione dei conflitti.

### <a name="enabling-certificate-rules"></a>Abilitazione delle regole certificati
Esistono procedure diverse per l'abilitazione delle regole certificati a seconda dell'ambiente:

-   [Per il computer locale](#BKMK_1)

-   [Per un oggetto Criteri di gruppo e si è in un server aggiunto a un dominio](#BKMK_2)

-   [Per un oggetto Criteri di gruppo e si è in un controller di dominio o in una workstation in cui è installato il Strumenti di amministrazione remota del server](#BKMK_3)

-   [Solo per i controller di dominio e si è in un controller di dominio o in una workstation in cui è installato il Strumenti di amministrazione remota del server Pack](#BKMK_4)

#### <a name="BKMK_1"></a>Per abilitare le regole dei certificati per il computer locale

1.  Aprire Impostazioni sicurezza locale.

2.  Nell'albero della console fare clic su **Opzioni di sicurezza** disponibili in impostazioni di sicurezza/Criteri locali.

3.  Nel riquadro dei dettagli fare doppio clic su impostazioni **System: Usare le regole dei certificati nei file eseguibili di Windows per i criteri di restrizione software @ no__t-0.

4.  Eseguire una delle operazioni seguenti e quindi fare clic su **OK**:

    -   Per abilitare le regole certificati, fare clic su **Attivato**.

    -   Per disabilitare le regole certificati, fare clic su **Disattivato**.

#### <a name="BKMK_2"></a>Per abilitare le regole dei certificati per un oggetto Criteri di gruppo e si utilizza un server aggiunto a un dominio

1.  Aprire Microsoft Management Console (MMC).

2.  Scegliere **Aggiungi/Rimuovi snap-in** dal menu **File** e quindi fare clic su **Aggiungi**.

3.  Fare clic su **Editor oggetti Criteri di gruppo locale** e quindi fare clic su **Aggiungi**.

4.  In **Selezione oggetto Criteri di gruppo** fare clic su **Sfoglia**.

5.  In **Cerca un oggetto Criteri di gruppo**selezionare un oggetto Criteri di gruppo (GPO) nel dominio appropriato, nel sito o nell'unità organizzativa oppure crearne uno nuovo, quindi fare clic su **fine**.

6.  Fare clic su **Chiudi**e quindi su **OK**.

7.  Nell'albero della console fare clic su **Opzioni di sicurezza** in *criteri OggettoCriteridiGruppo* [*nomecomputer*] criterio/Configurazione computer/impostazioni di Windows/impostazioni di sicurezza/Criteri locali/.

8.  Nel riquadro dei dettagli fare doppio clic su impostazioni **System: Usare le regole dei certificati nei file eseguibili di Windows per i criteri di restrizione software @ no__t-0.

9. Se questa impostazione dei criteri non è ancora stata definita, selezionare la casella di controllo **Definisci le impostazioni relative ai criteri**.

10. Eseguire una delle operazioni seguenti e quindi fare clic su **OK**:

    -   Per abilitare le regole certificati, fare clic su **Attivato**.

    -   Per disabilitare le regole certificati, fare clic su **Disattivato**.

#### <a name="BKMK_3"></a>Per abilitare le regole dei certificati per un oggetto Criteri di gruppo e si è in un controller di dominio o in una workstation in cui è installato il Strumenti di amministrazione remota del server

1.  Aprire Utenti e computer di Active Directory.

2.  Nell'albero della console fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo per cui si desidera abilitare le regole certificati.

3.  Scegliere **Proprietà** e quindi fare clic sulla scheda **Criteri di gruppo**.

4.  Fare clic su **Modifica** per aprire l'oggetto Criteri di gruppo che si desidera modificare. È inoltre possibile fare clic su **Nuovo** per creare un nuovo oggetto Criteri di gruppo e quindi fare clic su **Modifica**.

5.  Nell'albero della console fare clic su **Opzioni di sicurezza** in *criteri OggettoCriteridiGruppo*[*nomecomputer*] criterio/computer configurazione/impostazioni di Windows/impostazioni di sicurezza/Criteri locali.

6.  Nel riquadro dei dettagli fare doppio clic su impostazioni **System: Usare le regole dei certificati nei file eseguibili di Windows per i criteri di restrizione software @ no__t-0.

7.  Se questa impostazione dei criteri non è ancora stata definita, selezionare la casella di controllo **Definisci le impostazioni relative ai criteri**.

8.  Eseguire una delle operazioni seguenti e quindi fare clic su **OK**:

    -   Per abilitare le regole certificati, fare clic su **Attivato**.

    -   Per disabilitare le regole certificati, fare clic su **Disattivato**.

#### <a name="BKMK_4"></a>Per abilitare le regole dei certificati solo per i controller di dominio e si è in un controller di dominio o in una workstation in cui è installato il Strumenti di amministrazione remota del server

1.  Aprire Impostazioni di sicurezza controller di dominio.

2.  Nell'albero della console fare clic su **Opzioni di sicurezza** in Criteri *OggettoCriteriDiGruppo* [*Nome computer*]/Configurazione computer/Impostazioni di Windows/Impostazioni sicurezza/Criteri locali.

3.  Nel riquadro dei dettagli fare doppio clic su impostazioni **System: Usare le regole dei certificati nei file eseguibili di Windows per i criteri di restrizione software @ no__t-0.

4.  Se questa impostazione dei criteri non è ancora stata definita, selezionare la casella di controllo **Definisci le impostazioni relative ai criteri**.

5.  Eseguire una delle operazioni seguenti e quindi fare clic su **OK**:

    -   Per abilitare le regole certificati, fare clic su **Attivato**.

    -   Per disabilitare le regole certificati, fare clic su **Disattivato**.

> [!NOTE]
> È necessario eseguire questa procedura prima che le regole certificati diventino effettive.

### <a name="set-trusted-publisher-options"></a>Imposta opzioni autori attendibili
La firma del software viene utilizzata da un numero crescente di autori e sviluppatori di applicazioni per verificare che i rispettivi prodotti provengono da una fonte attendibile. Molti utenti tuttavia non sono a conoscenza o non prestano grande attenzione ai certificati di firma associati alle applicazioni che installano.

Le impostazioni dei criteri disponibili nella scheda **Autori attendibili** dei criteri di convalida del percorso di certificazione consentono agli amministratori di determinare quali certificati possono essere accettati in quanto provenienti da un autore attendibile.

##### <a name="to-configure-the-trusted-publishers-policy-settings-for-a-local-computer"></a>Per configurare le impostazioni dei criteri Autori attendibili per un computer locale

1.  Nella schermata **Start** Digitare**gpedit. msc** , quindi premere INVIO.

2.  Nell'albero della console, in **Criteri del computer locale\Configurazione computer\Impostazioni di Windows\Impostazioni sicurezza**, fare clic su **Criteri chiave pubblica**.

3.  Fare doppio clic su **Impostazioni di convalida percorso certificati** e quindi fare clic sulla scheda **Autori attendibili**.

4.  Selezionare la casella di controllo **Definisci le impostazioni relative ai criteri**, selezionare le impostazioni dei criteri che si desidera applicare e quindi fare clic su **OK** per applicare le nuove impostazioni.

##### <a name="to-configure-the-trusted-publishers-policy-settings-for-a-domain"></a>Per configurare le impostazioni dei criteri Autori attendibili per un dominio

1.  Aprire **gestione criteri di gruppo**.

2.  Nell'albero della console fare doppio clic su **criteri di gruppo oggetti** nella foresta e nel dominio che contengono l'oggetto **criteri di dominio predefinito** criteri di gruppo (GPO) che si desidera modificare.

3.  Fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo **Criterio dominio predefinito** e quindi scegliere **Modifica**.

4.  Nell'albero della console, in **Configurazione computer\Impostazioni di Windows\Impostazioni sicurezza**, fare clic su **Criteri chiave pubblica**.

5.  Fare doppio clic su **Impostazioni di convalida percorso certificati** e quindi fare clic sulla scheda **Autori attendibili**.

6.  Selezionare la casella di controllo **Definisci le impostazioni relative ai criteri**, selezionare le impostazioni dei criteri che si desidera applicare e quindi fare clic su **OK** per applicare le nuove impostazioni.

##### <a name="to-allow-only-administrators-to-manage-certificates-used-for-code-signing-for-a-local-computer"></a>Per consentire soltanto agli amministratori di gestire i certificati utilizzati per la firma del codice per un computer locale

1.  Nella schermata **Start** Digitare **gpedit. msc** nei **programmi e nei file di ricerca** o in Windows 8 sul desktop, quindi premere INVIO.

2.  Nell'albero della console in **Criterio dominio predefinito** o **Criteri del computer locale**, fare doppio clic su **Configurazione computer**, **Impostazioni di Windows**, **Impostazioni sicurezza** e quindi fare clic su **Criteri chiave pubblica**.

3.  Fare doppio clic su **Impostazioni di convalida percorso certificati** e quindi fare clic sulla scheda **Autori attendibili**.

4.  Selezionare la casella di controllo **Definisci le impostazioni relative ai criteri**.

5.  In **Gestione autori attendibili** fare clic su **Consenti solo agli amministratori di gestire gli autori attendibili** e quindi fare clic su **OK** per applicare le nuove impostazioni.

##### <a name="to-allow-only-administrators-to-manage-certificates-used-for-code-signing-for-a-domain"></a>Per consentire soltanto agli amministratori di gestire i certificati utilizzati per la firma del codice per un dominio

1.  Aprire **gestione criteri di gruppo**.

2.  Nell'albero della console fare doppio clic su **criteri di gruppo oggetti** nella foresta e nel dominio che contengono l'oggetto Criteri di gruppo **criterio dominio predefinito** che si desidera modificare.

3.  Fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo **Criterio dominio predefinito** e quindi scegliere **Modifica**.

4.  Nell'albero della console, in **Configurazione computer\Impostazioni di Windows\Impostazioni sicurezza**, fare clic su **Criteri chiave pubblica**.

5.  Fare doppio clic su **Impostazioni di convalida percorso certificati** e quindi fare clic sulla scheda **Autori attendibili**.

6.  Selezionare la casella di controllo **Definisci le impostazioni relative ai criteri**, apportare le modifiche desiderate e quindi fare clic su **OK** per applicare le nuove impostazioni.

## <a name="BKMK_Hash_Rules"></a>Utilizzo delle regole hash
Un hash è una serie di byte con lunghezza fissa che identifica in modo univoco un programma software o un file. L'hash viene calcolato da un algoritmo hash. Quando si crea una regola hash per un programma software, i criteri di restrizione software calcolano un hash del programma. Quando un utente tenta di aprire un programma software, un hash del programma viene confrontato con le regole hash esistenti per i criteri di restrizione software. L'hash del programma software è sempre lo stesso, indipendentemente dalla posizione del programma nel computer. Se un programma software viene modificato in qualche modo, tuttavia, anche il relativo hash cambia e non corrisponde più all'hash nella regola hash per i criteri di restrizione software.

È ad esempio possibile creare una regola hash e impostare il livello di sicurezza su **Non consentito** per impedire agli utenti di eseguire un determinato file. Un file può essere rinominato o spostato in un'altra cartella e mantenere comunque lo stesso hash. Qualsiasi modifica apportata al file, tuttavia, ne modifica il valore hash e consente al file di evitare le restrizioni.

#### <a name="to-create-a-hash-rule"></a>Per creare una regola hash

1.  Aprire Criteri restrizione software.

2.  Nell'albero della console o nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **regole aggiuntive**e quindi scegliere **nuova regola hash**.

3.  Fare clic su **Sfoglia** per trovare un file.

    > [!NOTE]
    > In Windows XP è possibile incollare un hash pre-calcolato nell'hash del **file**. In Windows Server 2008 R2, Windows 7 e versioni successive questa opzione non è disponibile.

4.  In **livello di sicurezza**fare clic su non **consentito** o **senza restrizioni**.

5.  In **Descrizione** digitare una descrizione per la regola e quindi fare clic su **OK**.

> [!NOTE]
> -   Potrebbe essere necessario creare una nuova impostazione dei criteri di restrizione software per l'oggetto Criteri di gruppo, se non è ancora stato fatto.
> -   È possibile creare una regola hash per un virus o un trojan horse per impedirne l'esecuzione.
> -   Se si vuole che altri utenti usino una regola hash in modo da non poter eseguire un virus, calcolare l'hash del virus usando i criteri di restrizione software e quindi inviare tramite posta elettronica il valore hash agli altri utenti. Non inviare mai il virus.
> -   Se un virus è stato inviato tramite posta elettronica, è anche possibile creare una regola di percorso per impedire l'esecuzione di allegati di posta elettronica.
> -   Un file rinominato o spostato in un'altra cartella mantiene lo stesso hash. Qualsiasi modifica apportata al file restituisce un hash diverso.
> -   Gli unici tipi di file interessati dalle regole hash sono quelli elencati in **Tipi di file designati** nel riquadro dei dettagli di Criteri restrizione software. È disponibile un elenco dei tipi di file designati condivisi da tutte le regole.
> -   Per rendere effettive i criteri di restrizione software, gli utenti devono aggiornare le impostazioni dei criteri disconnettendosi da e accedendo ai propri computer.
> -   Quando più di una regola dei criteri di restrizione software viene applicata alle impostazioni di criteri, esiste la precedenza delle regole per la gestione dei conflitti.

## <a name="BKMK_Internet_Zone_Rules"></a>Uso delle regole dell'area Internet
Le regole area Internet si applicano solo ai pacchetti di Windows Installer. Una regola di area consente di identificare il software proveniente da un'area specificata tramite Internet Explorer. Queste aree sono Internet, Intranet locale, Siti con restrizioni, Siti attendibili e Computer locale. Una regola dell'area Internet è progettata per impedire agli utenti di scaricare e installare software.

#### <a name="to-create-an-internet-zone-rule"></a>Per creare una regola area Internet

1.  Aprire Criteri restrizione software.

2.  Nell'albero della console o nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **regole aggiuntive**e quindi scegliere **nuova regola area Internet**.

3.  In **Area Internet** fare clic su un'area Internet.

4.  In **livello di sicurezza**fare clic su non **consentito** o **senza restrizioni**, quindi fare clic su **OK**.

> [!NOTE]
> -   Potrebbe essere necessario creare una nuova impostazione dei criteri di restrizione software per l'oggetto Criteri di gruppo, se non è ancora stato fatto.
> -   Le regole di area si applicano solo ai file con estensione MSI, ovvero pacchetti di Windows Installer.
> -   Per rendere effettive i criteri di restrizione software, gli utenti devono aggiornare le impostazioni dei criteri disconnettendosi da e accedendo ai propri computer.
> -   Quando più di una regola dei criteri di restrizione software viene applicata alle impostazioni di criteri, esiste la precedenza delle regole per la gestione dei conflitti.

## <a name="BKMK_Path_Rules"></a>Utilizzo delle regole di percorso
Una regola di percorso identifica il software in base al relativo percorso. Nel caso di un computer con livello di sicurezza predefinito **Non consentito**, ad esempio, è comunque possibile concedere l'accesso senza restrizioni a una cartella specifica per ogni utente. È possibile creare una regola di percorso utilizzando il percorso di file e impostando il livello di sicurezza della regola di percorso su **Senza restrizioni**. Alcuni percorsi comuni per questo tipo di regola sono %userprofile%, %windir%, %appdata%, %programfiles% e %temp%. È inoltre possibile creare regole di percorso del Registro di sistema che utilizzano la chiave del Registro di sistema del programma software come percorso.

Dato che queste regole sono specificate in base al percorso, se un programma software viene spostato la regola di percorso non è più applicabile.

#### <a name="to-create-a-path-rule"></a>Per creare una regola di percorso

1.  Aprire Criteri restrizione software.

2.  Nell'albero della console o nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **regole aggiuntive**e quindi scegliere **nuova regola percorso**.

3.  In **Percorso** digitare un percorso oppure fare clic su **Sfoglia** per trovare un file o una cartella.

4.  In **livello di sicurezza**fare clic su non **consentito** o **senza restrizioni**.

5.  In **Descrizione** digitare una descrizione per la regola e quindi fare clic su **OK**.

> [!CAUTION]
> -   In determinate cartelle, ad esempio la cartella Windows, l'impostazione del livello di sicurezza su non **consentito** può influire negativamente sul funzionamento del sistema operativo. Assicurarsi di non bloccare un componente cruciale del sistema operativo o uno dei programmi dipendenti.

> [!NOTE]
> -   Potrebbe essere necessario creare nuovi criteri di restrizione software per l'oggetto Criteri di gruppo, se non è ancora stato fatto.
> -   Se si crea una regola di percorso per un programma software con livello di sicurezza **Non consentito**, gli utenti potranno comunque eseguire il software copiandolo in un'altra posizione.
> -   I caratteri jolly supportati dalla regola di percorso sono * e ?.
> -   Nella regola di percorso è possibile utilizzare variabili di ambiente, come %programfiles% o %systemroot%.
> -   Se si desidera creare una regola di percorso per un programma software e non si conosce il percorso di archiviazione in un computer, ma è nota la chiave del Registro di sistema, è possibile creare una regola di percorso del Registro di sistema.
> -   Per impedire agli utenti di eseguire allegati di posta elettronica, è possibile creare una regola di percorso per la directory degli allegati del programma di posta elettronica che impedisce agli utenti di eseguire allegati di posta elettronica.
> -   Gli unici tipi di file interessati dalle regole di percorso sono quelli elencati in **Tipi di file designati** nel riquadro dei dettagli di Criteri restrizione software. È disponibile un elenco dei tipi di file designati condivisi da tutte le regole.
> -   Per rendere effettive i criteri di restrizione software, gli utenti devono aggiornare le impostazioni dei criteri disconnettendosi da e accedendo ai propri computer.
> -   Quando più di una regola dei criteri di restrizione software viene applicata alle impostazioni di criteri, esiste la precedenza delle regole per la gestione dei conflitti.

#### <a name="to-create-a-registry-path-rule"></a>Per creare una regola di percorso del Registro di sistema

1.  Nella schermata **Start** Digitare regedit.

2.  Nell'albero della console fare clic con il pulsante destro del mouse sulla chiave del Registro di sistema per cui si desidera creare una regola e quindi scegliere **Copia nome chiave**. Prendere nota del nome del valore nel riquadro dei dettagli.

3.  Aprire Criteri restrizione software.

4.  Nell'albero della console o nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **regole aggiuntive**e quindi scegliere **nuova regola percorso**.

5.  In **percorso**incollare il nome della chiave del registro di sistema, seguito dal nome del valore.

6.  Racchiudere il percorso del registro di sistema in segni di percentuale (%), ad esempio%HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PlatformSDK\Directories\InstallDir%.

7.  In **livello di sicurezza**fare clic su non **consentito** o **senza restrizioni**.

8.  In **Descrizione** digitare una descrizione per la regola e quindi fare clic su **OK**.


