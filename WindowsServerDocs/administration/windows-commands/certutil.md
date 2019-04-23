---
title: certutil
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c264ccf0-ba1e-412b-9dd3-d77dd9345ad9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8248f5ae540866394169229f0d7cf11497c9dcf2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834722"
---
# <a name="certutil"></a>certutil



Certutil.exe è un programma della riga di comando che viene installato come parte di servizi certificati. È possibile usare Certutil.exe per eseguire il dump e visualizzare le informazioni di configurazione (CA) dell'autorità di certificazione, configurare Servizi certificati, eseguire il backup e ripristinare i componenti di autorità di certificazione e verificare i certificati, coppie di chiavi e catene di certificati.

Quando certutil viene eseguita in un'autorità di certificazione senza parametri aggiuntivi, viene visualizzata la configurazione dell'autorità di certificazione corrente. Quando cerutil viene eseguito in una CA, il comando predefinito è in esecuzione il comando certutil [-dump](#BKMK_dump) verbo.

> [!WARNING]
> Le versioni precedenti di certutil potrebbero non fornire tutte le opzioni descritte in questo documento. È possibile visualizzare tutte le opzioni che fornisce una versione specifica di certutil, eseguire i comandi illustrati nella [notazioni sintassi](#BKMK_notations) sezione.

## <a name="BKMK_menu"></a>Menu

Le sezioni principali in questo documento sono:
-   [Verbi](#BKMK_Verbs)
-   [Notazioni di sintassi](#BKMK_notations)
-   [Opzioni](#BKMK_Options)
-   [Esempi aggiuntivi di certutil](#BKMK_AddedExamples)

## <a name="BKMK_Verbs"></a>Verbi

Nella tabella seguente vengono descritti i verbi che possono essere utilizzati con il comando certutil.

|Verbi|Descrizione|
|-----|-----------|
|[-dump](#BKMK_dump)|Le informazioni di configurazione o i file di dump|
|[-asn](#BKMK_asn)|Analizzare file ASN.1|
|[-decodehex](#BKMK_decodehex)|Decodificare il file con codifica esadecimale|
|[-decode](#BKMK_decode)|Decodificare un file con codifica Base64|
|[-encode](#BKMK_encode)|Codificare un file in base 64|
|[-deny](#BKMK_deny)|Negare una richiesta di certificato in sospeso|
|[-resubmit](#BKMK_resubmit)|Inviare di nuovo una richiesta di certificato in sospeso|
|[-setattributes](#BKMK_setattributes)|Impostare gli attributi per una richiesta di certificato in sospeso|
|[-setextension](#BKMK_setextension)|Impostare un'estensione per una richiesta di certificato in sospeso|
|[-revoke](#BKMK_revoke)|Revoca di un certificato|
|[-isvalid](#BKMK_isvalid)|Visualizzare la disposizione del certificato corrente|
|[-getconfig](#BKMK_getconfig)|Ottenere la stringa di configurazione predefinito|
|[-ping](#BKMK_ping)|Provare a contattare l'interfaccia richiesta servizi certificati di Active Directory|
|-pingadmin|Provare a contattare l'interfaccia di amministrazione di servizi di Active Directory certificato|
|[-CAInfo](#BKMK_CAInfo)|Visualizzare le informazioni sull'autorità di certificazione|
|[-ca.cert](#BKMK_ca.cert)|Recuperare il certificato dell'autorità di certificazione|
|[-ca.chain](#BKMK_ca.chain)|Recuperare la catena di certificati per autorità di certificazione|
|[-GetCRL](#BKMK_GetCRL)|Ottenere un elenco di revoche di certificati (CRL)|
|[-CRL](#BKMK_CRL)|Pubblicazione di nuovi elenchi di revoche di certificati (CRL) [o solo di delta CRL]|
|[-shutdown](#BKMK_shutdown)|Arresta Servizi certificati Active Directory|
|[-installCert](#BKMK_installcert)|Installare un certificato dell'autorità di certificazione|
|[-renewCert](#BKMK_renewcert)|Rinnovare un certificato dell'autorità di certificazione|
|[-schema](#BKMK_schema)|Eseguire il dump lo schema per il certificato|
|[-view](#BKMK_view)|Scarica la visualizzazione di certificato|
|[-db](#BKMK_db)|Eseguire il dump del database non elaborato|
|[-deleterow](#BKMK_deleterow)|Eliminare una riga dal database del server|
|[-backup](#BKMK_backup)|Servizi certificati Active Directory di backup|
|[-backupDB](#BKMK_backupDB)|Il backup del database di servizi certificati Active Directory|
|[-backupKey](#BKMK_backupKey)|Eseguire il backup il certificato di servizi certificati Active Directory e la chiave privata|
|[-restore](#BKMK_restore)|Il ripristino di servizi certificati Active Directory|
|[-restoreDB](#BKMK_restoreDB)|Ripristinare il database di servizi certificati Active Directory|
|[-restoreKey](#BKMK_restorekey)|Ripristinare la chiave privata e il certificato di servizi certificati Active Directory|
|[-importPFX](#BKMK_importPFX)|Importare il certificato e la chiave privata|
|[-dynamicfilelist](#BKMK_dynamicfilelist)|Visualizzare un elenco di file dinamici|
|[-databaselocations](#BKMK_databaselocations)|Visualizzare le posizioni dei database|
|[-hashfile](#BKMK_hashfile)|Generare e visualizzare un hash di crittografia in un file|
|[-store](#BKMK_Store)|Eseguire il dump nell'archivio certificati|
|[-addstore](#BKMK_addstore)|Aggiungere un certificato all'archivio|
|[-delstore](#BKMK_delstore)|Eliminare un certificato dall'archivio|
|[-verifystore](#BKMK_verifystore)|Verificare un certificato nell'archivio|
|[-repairstore](#BKMK_repairstore)|Ripristinare un'associazione di chiavi o aggiorna le proprietà del certificato o il descrittore di sicurezza delle chiavi|
|[-viewstore](#BKMK_viewstore)|Eseguire il dump nell'archivio dei certificati|
|[-viewdelstore](#BKMK_viewdelstore)|Eliminare un certificato dall'archivio|
|[-dsPublish](#BKMK_dsPublish)|Pubblicare un certificato o un elenco di revoche di certificati (CRL) in Active Directory|
|[-ADTemplate](#BKMK_ADTemplate)|Visualizzare i modelli di AD|
|[-Template](#BKMK_template)|Visualizzare i modelli di certificato|
|[-TemplateCAs](#BKMK_TemplateCAs)|Visualizzare le autorità di certificazione (CA) per un modello di certificato|
|[-CATemplates](#BKMK_CATemplates)|Visualizzare i modelli per la CA|
|[-SetCASites](#BKMK_SetCASites)|Gestire i nomi dei siti per le autorità di certificazione|
|[-enrollmentServerURL](#BKMK_enrollmentServerURL)|Visualizzare, aggiungere o eliminare gli URL del server registrazione associati a un'autorità di certificazione|
|[-ADCA](#BKMK_ADCA)|Visualizzare le autorità di certificazione di Active Directory|
|[-CA](#BKMK_CA)|Visualizzare le CA di criteri di registrazione|
|[-Policy](#BKMK_Policy)|Visualizzare i criteri di registrazione|
|[-PolicyCache](#BKMK_PolicyCache)|Visualizzare o eliminare le voci della Cache dei criteri di registrazione|
|[-CredStore](#BKMK_Credstore)|Visualizzare, aggiungere o eliminare le voci di Store delle credenziali|
|[-InstallDefaultTemplates](#BKMK_InstallDefaultTemplates)|Installare i modelli di certificato predefiniti|
|[-URLCache](#BKMK_URLCache)|Visualizzare o eliminare le voci della cache di URL|
|[-pulse](#BKMK_pulse)|Eventi di registrazione automatica di Pulse|
|[-MachineInfo](#BKMK_MachineInfo)|Visualizzare informazioni sull'oggetto computer di Active Directory|
|[-DCInfo](#BKMK_DCInfo)|Visualizzare informazioni sui controller di dominio|
|[-EntInfo](#BKMK_EntInfo)|Visualizzare informazioni su una CA dell'organizzazione|
|[-TCAInfo](#BKMK_TCAInfo)|Visualizzare le informazioni sull'autorità di certificazione|
|[-SCInfo](#BKMK_SCInfo)|Visualizzare informazioni sulle smart card|
|[-SCRoots](#BKMK_SCRoots)|Gestire i certificati radice di smart card|
|[-verifykeys](#BKMK_verifykeys)|Verificare un set di chiavi pubbliche o private|
|[-verify](#BKMK_verify)|Verificare un certificato, elenco di revoche di certificati (CRL) o la catena di certificati|
|[-verifyCTL](#BKMK_verifyCTL)|Verificare AuthRoot o certificati elenco scopi consentiti non consentiti|
|[-sign](#BKMK_sign)|Firmare di nuovo un certificato o un elenco di revoche di certificati (CRL)|
|[-vroot](#BKMK_vroot)|Creare o eliminare le radici virtuali web e le condivisioni file|
|[-vocsproot](#BKMK_vocsproot)|Creare o eliminare le radici virtuali web per un proxy web OCSP|
|[-addEnrollmentServer](#BKMK_addEnrollmentServer)|Aggiungere un'applicazione di registrazione Server|
|[-deleteEnrollmentServer](#BKMK_deleteEnrollmentServer)|Eliminare un'applicazione di registrazione Server|
|[-addPolicyServer](#BKMK_addPolicyServer)|Aggiungere un'applicazione Server dei criteri|
|[-deletePolicyServer](#BKMK_deletePolicyServer)|Eliminazione di un'applicazione Server dei criteri|
|[-oid](#BKMK_oid)|Visualizzare l'identificatore di oggetto o impostare un nome visualizzato|
|[-error](#BKMK_error)|Visualizzare il testo del messaggio associato a un codice di errore|
|[-getreg](#BKMK_getreg)|Visualizzare un valore del Registro di sistema|
|[-setreg](#BKMK_setreg)|Impostare un valore del Registro di sistema|
|[-delreg](#BKMK_delreg)|Eliminare un valore del Registro di sistema|
|[-ImportKMS](#BKMK_ImportKMS)|Importare certificati e chiavi utente nel database del server per l'archiviazione della chiave|
|[-ImportCert](#BKMK_ImportCert)|Importare un file di certificato nel database|
|[-GetKey](#BKMK_GetKey)|Recuperare un blob archiviato recupero delle chiavi private|
|[-RecoverKey](#BKMK_RecoverKey)|Ripristinare una chiave privata archiviata|
|[-MergePFX](#BKMK_MergePFX)|Unire i file PFX|
|[-ConvertEPF](#BKMK_ConvertEPF)|Convertire un file PFX in un file EPF|
|-?|Consente di visualizzare l'elenco dei verbi|
|-*\<verb>* -?|Visualizza la Guida per il verbo specificato.|
|-? -v|Visualizza un elenco completo dei verbi e|

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_notations"></a>Notazioni di sintassi

-   Per la sintassi della riga di comando di base, eseguire `certutil -?`
-   Per informazioni sulla sintassi sull'uso di certutil con un verbo specifico, eseguire **certutil**  *\<verbo >* **-?**
-   Per l'invio di tutta la sintassi certutil in un file di testo, eseguire i comandi seguenti:  
    -   `certutil -v -? > certutilhelp.txt`
    -   `notepad certutilhelp.txt`

Nella tabella seguente vengono descritti la notazione utilizzata per indicare la sintassi della riga di comando.

|Notazione|Descrizione|
|--------|-----------|
|Testo senza parentesi quadre o parentesi graffe|Elementi che come illustrato, è necessario digitare|
|\<Testo all'interno di parentesi angolari >|Segnaposto per il quale è necessario specificare un valore|
|[Testo all'interno delle parentesi quadre]|Elementi facoltativi|
|{Testo tra parentesi graffe}|Set di elementi richiesti; Scegliere una|
|Barra (verticale|)|Separatore per gli elementi si escludono a vicenda; Scegliere una|
|Puntini di sospensione (…)|Elementi che possono essere ripetuti|

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_dump"></a>-dump

CertUtil [Options] [-dump]

CertUtil [Options] [-dump] File

Le informazioni di configurazione o i file di dump

[-f]. [-invisibile all'utente] [-suddividere] [-p Password] [-t Timeout]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_asn"></a>-asn

CertUtil [Options] - asn File [tipo]

Analizzare file ASN.1

tipo: numeric CRYPT_STRING_ * decodifica tipo

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_decodehex"></a>-decodehex

CertUtil [Options] - decodehex FileIn OutFile [tipo]

tipo: numeric CRYPT_STRING_ * il tipo di codifica

[-f]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_decode"></a>-decode

CertUtil [opzioni] - decode FileIn FileOut

Decodificare il file con codifica Base64

[-f]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_encode"></a>-encode

CertUtil [opzioni] - codificare FileIn FileOut

Codificare file in base 64

[-f] [-UnicodeText]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_deny"></a>-Nega

CertUtil [opzioni] - deny RequestId

Negare la richiesta in sospeso

[-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_resubmit"></a>-resubmit

CertUtil [opzioni] - inviare di nuovo RequestId

Inviare di nuovo la richiesta in sospeso

[-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_setattributes"></a>-setattributes

CertUtil [Options] - setattributes RequestId AttributeString

Impostare gli attributi per la richiesta in sospeso

RequestId: Id numerico di una richiesta in sospeso

AttributeString: Richiesta delle coppie nome / valore di attributo
-   I nomi e i valori sono separati da punti.
-   Nome di più, coppie valore sono separati da una nuova riga.
-   Esempio: "CertificateTemplate:User\nEMail:User@Domain.com"
-   Ogni sequenza "\n" viene convertito in un separatore di nuova riga.

[-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_setextension"></a>-setextension

CertUtil [Options] - setextension flag RequestId ExtensionName {lungo | Data | Stringa | @InFile}

Estensione del set per la richiesta in sospeso

RequestId: Id numerico di una richiesta in sospeso

ExtensionName: Stringa ObjectId dell'estensione

Flag: 0 è consigliato.  1 indica un'estensione critica, lo disabilita 2, 3 esegue entrambe le operazioni.

Se l'ultimo parametro è numerico, verrà considerato come un valore Long.

Se può essere analizzato come data, verrà considerato come una data.

Se inizia con ' @', il resto del token è il nome del file contenente dati binari o un dump esadecimale di testo ascii.

Qualsiasi altro elemento verrà considerato come una stringa.

[-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_revoke"></a>-revoke

CertUtil [opzioni] - revocare SerialNumber [Reason]

Revoca di certificato

Numero di serie: Elenco delimitato da virgole di numeri di serie certificato revocare

Motivo: motivo della revoca numerico o simbolico
-   0: CRL_REASON_UNSPECIFIED: Non è specificato (valore predefinito)
-   1: CRL_REASON_KEY_COMPROMISE: Chiave compromessa
-   2: CRL_REASON_CA_COMPROMISE: CA compromessa
-   3: CRL_REASON_AFFILIATION_CHANGED: Affiliazione modificata
-   4: CRL_REASON_SUPERSEDED: Sostituito
-   5: CRL_REASON_CESSATION_OF_OPERATION: Cessazione attività
-   6: CRL_REASON_CERTIFICATE_HOLD: Sospensione certificato
-   8: CRL_REASON_REMOVE_FROM_CRL: Rimuovere dal CRL
-   -1: Annullare la revoca: Annullare la revoca

[-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_isvalid"></a>-isvalid

CertUtil [Options] - isvalid SerialNumber | CertHash

Disposizione di visualizzazione corrente del certificato

[-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_getconfig"></a>-getconfig

CertUtil [Options] - getconfig

Ottenere la stringa di configurazione predefinito

[-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_ping"></a>-ping

CertUtil [opzioni]: ping [MaxSecondsToWait | CAMachineList]

Interfaccia ping Active Directory richiesta di servizi certificati

CAMachineList: Elenco dei nomi computer delimitati da virgole CA
1.  Per un singolo computer, usare una virgola finale
2.  Consente di visualizzare il costo del sito per ogni computer della CA

[-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_CAInfo"></a>-CAInfo

CertUtil [Options] - CAInfo [NomeInfo [indice | Codice di errore]]

Visualizza informazioni sulla CA

NomeInfo: indica la proprietà dell'autorità di certificazione per visualizzare (vedere sotto). Usare "*" per tutte le proprietà.

Indice: indice della proprietà in base zero facoltativo

ErrorCode: codice di errore numerico

[-f]. [-suddividere] [-config nome computer\CA]

Argomento InfoName:
-   File: Versione file
-   prodotto: Versione prodotto
-   exitcount: N. moduli uscita
-   uscita [Index]: Descrizione di modulo uscita
-   Criteri: Descrizione di modulo criteri
-   Nome: Nome della CA
-   sanitizedname: Nome autorità di certificazione puro
-   dsname: Nome breve puro autorità di certificazione (nome di dominio Active Directory)
-   CartellaCondivisa: Cartella condivisa
-   ErrorCode Error1: Testo messaggio di errore
-   ErrorCode Error2: Testo messaggio di errore e codice di errore
-   Tipo: Tipo di CA
-   Info: Informazioni sull'autorità di certificazione
-   Padre: Autorità di certificazione padre
-   certcount: Conteggio di certificati di autorità di certificazione
-   xchgcount: Conteggio di certificati di autorità di certificazione exchange
-   kracount: Conteggio del certificato KRA
-   kraused: Conteggio di certificati usato KRA
-   propidmax: Numero massimo di autorità di certificazione PropId
-   certstate [Index]: Certificato CA
-   certversion [Index]: Versione del certificato della CA
-   certstatuscode [Index]: Verifica dello stato del certificato CA
-   crlstate [Index]: CRL
-   krastate [Index]: Certificato KRA
-   crossstate + [Index]: Certificato incrociato diretto
-   crossstate-[Index]: Certificato incrociato con le versioni precedenti
-   CERT [Index]: Certificato CA
-   certchain [Index]: Catena di certificati di autorità di certificazione
-   certcrlchain [Index]: Catena di certificati di autorità di certificazione con CRL
-   xchg [Index]: Certificato di scambio di CA
-   xchgchain [Index]: Catena di certificati di autorità di certificazione exchange
-   xchgcrlchain [Index]: Catena di certificati di autorità di certificazione exchange con CRL
-   KRA [Index]: Certificato KRA
-   Cross + [Index]: Certificato incrociato diretto
-   Cross-[Index]: Certificato incrociato con le versioni precedenti
-   CRL [Index]: Base CRL
-   deltacrl [Index]: Delta CRL
-   crlstatus [Index]: Stato pubblicazione CRL
-   deltacrlstatus [Index]: Stato di pubblicazione di Delta CRL
-   DNS: Nome DNS
-   Ruolo: Separazione dei ruoli
-   annunci: Advanced Server
-   modelli: Modelli
-   OCSP [Index]: URL OCSP
-   AIA [Index]: URL AIA
-   punti di distribuzione [Index]: URL di punti di distribuzione
-   localename: Nome delle impostazioni locali di autorità di certificazione
-   subjecttemplateoids: OID di modello di oggetto

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_ca.cert"></a>-ca.cert

CertUtil [opzioni] - ca.cert FileCertCaOut [Index]

Recuperare il certificato della CA

FileCertCaOut: file di output

Indice: Indice di rinnovo del certificato della CA (impostazione predefinita è più recente)

[-f]. [-suddividere] [-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_ca.chain"></a>-ca.chain

CertUtil [opzioni] - ca.chain FileCatenaCertCAOut [Index]

Recuperare la catena di certificati della CA

FileCatenaCertCAOut: file di output

Indice: Indice di rinnovo del certificato della CA (impostazione predefinita è più recente)

[-f]. [-suddividere] [-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_GetCRL"></a>-GetCRL

CertUtil [Options] - GetCRL OutFile [Index] [delta]

Ottenere l'elenco CRL

Indice: CRL indice o chiave di indice (impostazione predefinita è CRL per la chiave più recente)

delta: delta CRL (valore predefinito è CRL di base)

[-f]. [-suddividere] [-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_CRL"></a>-CRL

CertUtil [Options] - CRL [gg | ripubblicare] [delta]

Pubblica nuovo CRL [or delta CRL solo]

gg - nuovo periodo di validità CRL in giorni e ore

pubblicare di nuovo, ovvero pubblicare di nuovo CRL più recente

delta, delta CRL solo (valore predefinito è di base e delta CRL)

[-suddividere] [-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_shutdown"></a>-shutdown

CertUtil [opzioni] - arresto del sistema

Arresta Servizi certificati Active Directory

[-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_installcert"></a>-installCert

CertUtil [Options] - installCert [FileCertCA]

Installare il certificato di autorità di certificazione

[-f]. [-invisibile all'utente] [-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_renewcert"></a>-renewCert

CertUtil [Options] - renewCert [ReuseKeys] [Machine\ParentCAName]

Rinnova certificato dell'autorità di certificazione

Usare -f per ignorare una richiesta di rinnovo in sospeso e generare una nuova richiesta.

[-f]. [-invisibile all'utente] [-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_schema"></a>-schema

CertUtil [opzioni] - schema [Ext | Attrib | CRL]

Eseguire il dump dello Schema di certificato

Il valore predefinito è tabella richiesta e il certificato

EXT: Tabella di estensione

Attrib: Tabella di attributi

CRL: Tabella CRL

[-suddividere] [-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_view"></a>-vista

CertUtil [opzioni] - vista [coda | Log | LogFail | Revocato | Ext | Attrib | CRL] [csv]

Visualizzazione di certificati di dump

Coda: Coda di richieste

Registro: Certificati revocati o emessi, più le richieste non riuscite

LogFail: Richieste non riuscite

Per revocare: Certificati revocati

EXT: Tabella di estensione

Attrib: Tabella di attributi

CRL: Tabella CRL

csv: Output come valori delimitati da virgole

Per visualizzare la colonna StatusCode per tutte le voci:-out StatusCode

Per visualizzare tutte le colonne per l'ultima voce:-limitare il "ID richiesta = = $"

Per visualizzare l'ID richiesta e la disposizione per le richieste di tre:-limitare "RequestId > = 37, RequestId\<40"-out "RequestId, Disposition"

Per visualizzare gli ID di riga e i numeri di CRL per tutti i CRL di Base:-limitare "CRLMinBase = 0"-out "CRLRowId, CRLNumber" CRL

Per visualizzare Base CRL numero 3: - v-limitare "CRLMinBase = 0, CRLNumber = 3"-out "CRLRawCRL" CRL

Per visualizzare l'intera tabella CRL: CRL

Usare "data [+ |-gg]" per le restrizioni di date

Usare "ora + gg" per una data rispetto all'ora corrente

[-invisibile all'utente] [-suddividere] [-config nome computer\CA] [-limitare RestrictionList] [-out ColumnList]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_db"></a>-db

CertUtil [Options] - db

Eseguire il dump non elaborato del Database

[-config nome computer\CA] [-limitare RestrictionList] [-out ColumnList]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_deleterow"></a>-deleterow

CertUtil [Options] - deleterow RowId | Date [richiesta | CERT | Ext | Attrib | CRL]

Eliminare la riga del database di server

Richiesta: Non è riuscita e in attesa di richieste (data di invio)

Cert: Certificati scaduti e revocati (data di scadenza)

EXT: Tabella di estensione

Attrib: Tabella di attributi

CRL: Tabella CRL (data di scadenza)

Per eliminare e non in attesa di richieste inviate da 22 gennaio 2001: 1/22/2001 richiesta

Per eliminare tutti i certificati scaduti entro il 22 gennaio 2001: 1/22/2001 cert

Per eliminare la riga del certificato, attributi e le estensioni per 37 RequestId: 37

Per eliminare i CRL scaduti entro il 22 gennaio 2001: 22/1/2001 CRL

[-f]. [-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_backup"></a>-backup

CertUtil [opzioni] - eseguire il backup BackupDirectory [incrementale] [KeepLog]

Servizi certificati Active Directory di backup

DirectoryBackup: directory in cui archiviare i dati di backup

Incrementale: esegue solo il backup incrementale (valore predefinito è il backup completo)

KeepLog: mantenere i file di log database (valore predefinito è troncare i file di log)

[-f]. [-config nome computer\CA] [-p Password]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_backupDB"></a>-backupDB

CertUtil [Options] - backupDB BackupDirectory [incrementale] [KeepLog]

Backup database di servizi certificati Active Directory

DirectoryBackup: directory in cui archiviare il backup i file di database

Incrementale: esegue solo il backup incrementale (valore predefinito è il backup completo)

KeepLog: mantenere i file di log database (valore predefinito è troncare i file di log)

[-f]. [-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_backupKey"></a>-backupKey

CertUtil [Options] - backupKey BackupDirectory

Certificato di servizi certificati Active Directory di backup e la chiave privata

DirectoryBackup: directory in cui archiviare il backup file PFX

[-f]. [-config nome computer\CA] [-p Password] [-t Timeout]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_restore"></a>-ripristino

CertUtil [opzioni] - ripristinare BackupDirectory

Il ripristino di servizi certificati Active Directory

DirectoryBackup: directory contenente i dati da ripristinare

[-f]. [-config nome computer\CA] [-p Password]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_restoreDB"></a>-restoreDB

CertUtil [Options] - restoreDB BackupDirectory

Ripristinare il database di servizi certificati Active Directory

DirectoryBackup: directory che contiene i file di database da ripristinare

[-f]. [-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_restorekey"></a>-restoreKey

CertUtil [Options] - restoreKey BackupDirectory | FilePFX

Ripristinare la chiave privata e certificato di servizi certificati Active Directory

DirectoryBackup: directory contenente il file PFX da ripristinare

PFXFile: File PFX da ripristinare

[-f]. [-config nome computer\CA] [-p Password]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_importPFX"></a>-importPFX

CertUtil [Options] - importPFX [CertificateStoreName] FilePFX [modificatori]

Importare il certificato e la chiave privata

CertificateStoreName: Nome dell'archivio certificati.  Visualizzare [-archiviare](#BKMK_Store).

PFXFile: File PFX da importare

Modificatori di: Elenco delimitato da virgole di uno o più delle operazioni seguenti:
1.  AT_SIGNATURE: Modificare KeySpec alla firma
2.  AT_KEYEXCHANGE: Modificare KeySpec di scambio delle chiavi
3.  NoExport: Rendere non esportabile la chiave privata
4.  NoCert: Non si importa il certificato
5.  NoChain: Non importare la catena di certificati
6.  NoRoot: Non importare il certificato radice
7.  Proteggere: Proteggere le chiavi con la password
8.  NoProtect: Eseguire operazioni di password non proteggere le chiavi

Il valore predefinito è archivio computer personale.

[-f]. [-utente] [-p Password] [-csp Provider]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_dynamicfilelist"></a>-dynamicfilelist

CertUtil [Options] - dynamicfilelist

Visualizza elenco file dinamico

[-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_databaselocations"></a>-databaselocations

CertUtil [Options] - databaselocations

Visualizzare le posizioni dei database

[-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_hashfile"></a>-hashfile

CertUtil [Options] - hashfile FileIn [HashAlgorithm]

Generare e visualizzare l'hash di crittografia in un file

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_Store"></a>-store

CertUtil [opzioni] - archiviare [CertificateStoreName [CertId [FileOutput]]]

Archivio certificati di dump

CertificateStoreName: Nome dell'archivio certificati. Esempi:
-   "My", "CA" (impostazione predefinita), "Root",
-   "ldap: / / / CN autorità di certificazione, CN = = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? cACertificate? uno? objectClass = certificationAuthority" (visualizzazione radice dei certificati)
-   "ldap: / / / CN CAName, CN = = autorità di certificazione, CN = Servizi chiave pubblica, CN = Services, CN = Configuration, DC = cpandl, DC = com? cACertificate? base? objectClass = certificationAuthority" (modificare i certificati radice)
-   "ldap: / / / CN CAName, CN = MachineName, CN = = CDP, CN = Servizi chiave pubblica, CN = Services, CN = Configuration, DC = cpandl, DC = com? certificateRevocationList? base? objectClass = cRLDistributionPoint" (visualizzazione CRL)
-   "ldap: / / / CN = NTAuthCertificates, CN = Servizi chiave pubblica, CN = Services, CN = Configuration, DC = cpandl, DC = com? cACertificate? base? objectClass = certificationAuthority" (certificati di autorità di certificazione dell'organizzazione)
-   LDAP: (Certificati di oggetto computer di Active Directory)
-   -utente ldap: (I certificati oggetto utente di Active Directory)

CertId: Certificati o CRL corrispondenza token.  Ciò può essere un numero di serie, un SHA-1 certificato, CRL, elenco di scopi consentiti o hash di chiave pubblica, un indice numerico cert (0, 1 e così via), un indice numerico CRL (. 0,.1 e così via), un indice numerico CTL (.. 0,... 1 e così via), una chiave pubblica, firma o l'estensione ObjectId, un soggetto certificato nome comune, un indirizzo di posta elettronica, UPN o nome DNS, un nome di contenitore di chiavi o nome CSP, un nome di modello o ObjectId, un EKU o ObjectId dei criteri dell'applicazione o un nome comune dell'autorità di certificazione CRL. Molti di questi può comportare più corrispondenze.

OutputFile: file per salvare il certificato corrisponda

Utilizzare - utente di accedere a un archivio utente invece di un archivio del computer.

Utilizzare - enterprise per accedere a un archivio del computer dell'organizzazione.

Utilizzare - service per accedere a un archivio del computer del servizio.

Utilizzare - grouppolicy per accedere a un archivio di criteri di gruppo di computer.

Esempi:
-   -enterprise NTAuth
-   -enterprise Root 37
-   -user My 26e0aaaf000000000004
-   AUTORITÀ DI CERTIFICAZIONE.11

[-f]. [-enterprise] [-utente] [-GroupPolicy] [-invisibile all'utente] [-suddividere] [-dc DCName]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_addstore"></a>-addstore

CertUtil [Options] - addstore CertificateStoreName FileIn

Aggiungere il certificato all'archivio

CertificateStoreName: Nome dell'archivio certificati.  Visualizzare [-archiviare](#BKMK_Store).

InFile: File di certificato o un CRL da aggiungere all'archivio.

[-f]. [-enterprise] [-utente] [-GroupPolicy] [-dc DCName]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_delstore"></a>-delstore

CertUtil [Options] - delstore CertificateStoreName CertId

Eliminare certificati dall'archivio

CertificateStoreName: Nome dell'archivio certificati.  Visualizzare [-archiviare](#BKMK_Store).

CertId: Certificati o CRL corrispondenza token.  Visualizzare [-archiviare](#BKMK_Store).

[-enterprise] [-utente] [-GroupPolicy] [-dc DCName]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_verifystore"></a>-verifystore

CertUtil [Options] - verifystore CertificateStoreName [CertId]

Verificare il certificato nell'archivio

CertificateStoreName: Nome dell'archivio certificati.  Visualizzare [-archiviare](#BKMK_Store).

CertId: Certificati o CRL corrispondenza token.  Visualizzare [-archiviare](#BKMK_Store).

[-enterprise] [-utente] [-GroupPolicy] [-invisibile all'utente] [-suddividere] [-dc DCName] [-t Timeout]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_repairstore"></a>-repairstore

CertUtil [Options] - repairstore CertificateStoreName CertIdList [PropertyInfFile | SDDLSecurityDescriptor]

Associazione di chiavi di riparazione o aggiornamento di descrittore di sicurezza delle proprietà o una chiave di certificato

CertificateStoreName: Nome dell'archivio certificati.  Visualizzare [-archiviare](#BKMK_Store).

CertIdList: elenco delimitato da virgole dei token di corrispondenza certificati o CRL. Visualizzare [-archiviare](#BKMK_Store) CertId descrizione.

PropertyInfFile - File INF contenente le proprietà esterne:
```
[Properties]
     19 = Empty ; Add archived property, OR:
     19 =       ; Remove archived property

     11 = "{text}Friendly Name" ; Add friendly name property

     127 = "{hex}" ; Add custom hexadecimal property
         _continue_ = "00 01 02 03 04 05 06 07 08 09 0a 0b 0c 0d 0e 0f"
         _continue_ = "10 11 12 13 14 15 16 17 18 19 1a 1b 1c 1d 1e 1f"

     2 = "{text}" ; Add Key Provider Information property
       _continue_ = "Container=Container Name&"
       _continue_ = "Provider=Microsoft Strong Cryptographic Provider&"
       _continue_ = "ProviderType=1&"
       _continue_ = "Flags=0&"
       _continue_ = "KeySpec=2"

     9 = "{text}" ; Add Enhanced Key Usage property
       _continue_ = "1.3.6.1.5.5.7.3.2,"
       _continue_ = "1.3.6.1.5.5.7.3.1,"
```
[-f]. [-enterprise] [-utente] [-GroupPolicy] [-invisibile all'utente] [-suddividere] [-csp Provider]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_viewstore"></a>-viewstore

CertUtil [Options] - viewstore [CertificateStoreName [CertId [FileOutput]]]

Archivio certificati di dump

CertificateStoreName: Nome dell'archivio certificati.  Esempi:
-   "My", "CA" (impostazione predefinita), "Root",
-   "ldap: / / / CN autorità di certificazione, CN = = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? cACertificate? uno? objectClass = certificationAuthority" (visualizzazione radice dei certificati)
-   "ldap: / / / CN CAName, CN = = autorità di certificazione, CN = Servizi chiave pubblica, CN = Services, CN = Configuration, DC = cpandl, DC = com? cACertificate? base? objectClass = certificationAuthority" (modificare i certificati radice)
-   "ldap: / / / CN CAName, CN = MachineName, CN = = CDP, CN = Servizi chiave pubblica, CN = Services, CN = Configuration, DC = cpandl, DC = com? certificateRevocationList? base? objectClass = cRLDistributionPoint" (visualizzazione CRL)
-   "ldap: / / / CN = NTAuthCertificates, CN = Servizi chiave pubblica, CN = Services, CN = Configuration, DC = cpandl, DC = com? cACertificate? base? objectClass = certificationAuthority" (certificati di autorità di certificazione dell'organizzazione)
-   LDAP: (Certificati di oggetto computer di Active Directory)
-   -utente ldap: (I certificati oggetto utente di Active Directory)

CertId: Certificati o CRL corrispondenza token. Ciò può essere un numero di serie, un SHA-1 certificato, CRL, elenco di scopi consentiti o hash di chiave pubblica, un indice numerico cert (0, 1 e così via), un indice numerico CRL (. 0,.1 e così via), un indice numerico CTL (.. 0,... 1 e così via), una chiave pubblica, firma o l'estensione ObjectId, un soggetto certificato nome comune, un indirizzo di posta elettronica, UPN o nome DNS, un nome di contenitore di chiavi o nome CSP, un nome di modello o ObjectId, un EKU o ObjectId dei criteri dell'applicazione o un nome comune dell'autorità di certificazione CRL. Molti di questi può comportare più corrispondenze.

OutputFile: file per salvare il certificato corrisponda

Utilizzare - utente di accedere a un archivio utente invece di un archivio del computer.

Utilizzare - enterprise per accedere a un archivio del computer dell'organizzazione.

Utilizzare - service per accedere a un archivio del computer del servizio.

Utilizzare - grouppolicy per accedere a un archivio di criteri di gruppo di computer.

Esempi:
1.  -enterprise NTAuth
2.  -enterprise Root 37
3.  -user My 26e0aaaf000000000004
4.  AUTORITÀ DI CERTIFICAZIONE.11

[-f]. [-enterprise] [-utente] [-GroupPolicy] [-dc DCName]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_viewdelstore"></a>-viewdelstore

CertUtil [Options] - viewdelstore [CertificateStoreName [CertId [FileOutput]]]

Eliminare certificati dall'archivio

CertificateStoreName: Nome dell'archivio certificati.  Esempi:
-   "My", "CA" (impostazione predefinita), "Root",
-   "ldap: / / / CN autorità di certificazione, CN = = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? cACertificate? uno? objectClass = certificationAuthority" (visualizzazione radice dei certificati)
-   "ldap: / / / CN CAName, CN = = autorità di certificazione, CN = Servizi chiave pubblica, CN = Services, CN = Configuration, DC = cpandl, DC = com? cACertificate? base? objectClass = certificationAuthority" (modificare i certificati radice)
-   "ldap: / / / CN CAName, CN = MachineName, CN = = CDP, CN = Servizi chiave pubblica, CN = Services, CN = Configuration, DC = cpandl, DC = com? certificateRevocationList? base? objectClass = cRLDistributionPoint" (visualizzazione CRL)
-   "ldap: / / / CN = NTAuthCertificates, CN = Servizi chiave pubblica, CN = Services, CN = Configuration, DC = cpandl, DC = com? cACertificate? base? objectClass = certificationAuthority" (certificati di autorità di certificazione dell'organizzazione)
-   LDAP: (Certificati di oggetto computer di Active Directory)
-   -utente ldap: (I certificati oggetto utente di Active Directory)

CertId: Certificati o CRL corrispondenza token. Ciò può essere un numero di serie, un SHA-1 certificato, CRL, elenco di scopi consentiti o hash di chiave pubblica, un indice numerico cert (0, 1 e così via), un indice numerico CRL (. 0,.1 e così via), un indice numerico CTL (.. 0,... 1 e così via), una chiave pubblica, firma o l'estensione ObjectId, un soggetto certificato nome comune, un indirizzo di posta elettronica, UPN o nome DNS, un nome di contenitore di chiavi o nome CSP, un nome di modello o ObjectId, un EKU o ObjectId dei criteri dell'applicazione o un nome comune dell'autorità di certificazione CRL. Molti di questi può comportare più corrispondenze.

OutputFile: file per salvare il certificato corrisponda

Utilizzare - utente di accedere a un archivio utente invece di un archivio del computer.

Utilizzare - enterprise per accedere a un archivio del computer dell'organizzazione.

Utilizzare - service per accedere a un archivio del computer del servizio.

Utilizzare - grouppolicy per accedere a un archivio di criteri di gruppo di computer.

Esempi:
1.  -enterprise NTAuth
2.  -enterprise Root 37
3.  -user My 26e0aaaf000000000004
4.  AUTORITÀ DI CERTIFICAZIONE.11

[-f]. [-enterprise] [-utente] [-GroupPolicy] [-dc DCName]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_dsPublish"></a>-dsPublish

CertUtil [Options] - dsPublish CertFile [NTAuthCA | RootCA | SubCA | CrossCA | KRA | Utente | Macchina]

CertUtil [Options] - dsPublish FileCRL [DSCDPContainer [DSCDPCN]]

Pubblicazione di certificati o CRL in Active Directory

CertFile: file di certificato per la pubblicazione

NTAuthCA: Pubblicare certificato nell'archivio directory Enterprise

RootCA: Pubblica del certificato all'archivio radice attendibile di dominio Active Directory

SubCA: Pubblicare il certificato CA all'oggetto di autorità di certificazione di dominio Active Directory

CrossCA: Pubblicare cross-cert all'oggetto di autorità di certificazione di dominio Active Directory

KRA: Pubblica certificato in oggetti di Key Recovery Agent di dominio Active Directory

Utente: Pubblicare cert all'oggetto utente di dominio Active Directory

Computer: Pubblicare cert all'oggetto computer di dominio Active Directory

CRLFile: File CRL da pubblicare

DSCDPContainer: Contenitori CDP DS CN, in genere il nome del computer della CA

DSCDPCN: DS CDP, CN dell'oggetto in genere in base il nome breve di autorità di certificazione puro e l'indice di chiave

Consente di creare un oggetto DS -f.

[-f]. [-utente] [-dc DCName]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_ADTemplate"></a>-ADTemplate

CertUtil [Options] - ADTemplate [modello]

Visualizzare i modelli di AD

[-f]. [-utente] [-ut] [-mt] [-dc DCName]

## <a name="BKMK_template"></a>-Template

CertUtil [opzioni] - modello [modello]

Visualizzare i modelli di criteri di registrazione

[-f] [-user] [-silent] [-PolicyServer URLOrId] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName UserName] [-p Password]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_TemplateCAs"></a>-TemplateCAs

CertUtil [Options] - TemplateCAs modello

Visualizzare le autorità di certificazione per il modello

[-f]. [-utente] [-dc DCName]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_CATemplates"></a>-CATemplates

CertUtil [Options] - CATemplates [modello]

Visualizzare i modelli per la CA

[-f]. [-utente] [-ut] [-mt] [-config nome computer\CA] [-dc DCName]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_SetCASites"></a>-SetCASites

CertUtil [Options] - SetCASites [impostare] [nome sito]

CertUtil [Options] - SetCASites verificare [nomesito]

CertUtil [Options] - SetCASites delete

Nomi dei siti di set, verificare o eliminare autorità di certificazione
-   Usare l'opzione - config per una singola autorità di certificazione di destinazione (valore predefinito è tutte le CA)
-   *SiteName* è consentita solo quando la destinazione di una singola autorità di certificazione
-   Utilizzare -f per ignorare gli errori di convalida per l'oggetto specificato *SiteName*
-   Usare -f per eliminare tutti i nomi di autorità di certificazione del sito

[-f]. [-config nome computer\CA] [-dc DCName]

> [!NOTE]
> Per altre informazioni su come configurare le autorità di certificazione per il riconoscimento dei siti di Active Directory Domain Services (AD DS), vedere [riconoscimento dei siti di Active Directory per i client Servizi certificati Active Directory e infrastruttura a chiave pubblica](https://social.technet.microsoft.com/wiki/contents/articles/14106.ad-ds-site-awareness-for-ad-cs-and-pki-clients.aspx).

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_enrollmentServerURL"></a>-enrollmentServerURL

CertUtil [Options] - enrollmentServerURL [URL AuthenticationType [priorità] [modificatori]]

Eliminazione di CertUtil [Options] - enrollmentServerURL URL

Visualizzare, aggiungere o eliminare gli URL del server registrazione associati a un'autorità di certificazione

AuthenticationType: Specificare uno dei seguenti metodi di autenticazione client durante l'aggiunta di un URL
1.  Kerberos: Usare le credenziali Kerberos SSL
2.  Nome utente: Utilizzare account denominato credenziali SSL
3.  ClientCertificate: Usare le credenziali di certificato X.509 SSL
4.  Anonymous: Utilizzare credenziali anonime SSL

Delete: Elimina l'URL specificato associato con l'autorità di certificazione

Priorità: il valore predefinito è '1' Se non specificato durante l'aggiunta di un URL

Modificatori: Elenco delimitato da virgole di uno o più delle seguenti operazioni:
1.  AllowRenewalsOnly: Solo le richieste di rinnovo possono essere inviate a questa CA tramite questo URL
2.  AllowKeyBasedRenewal: Consente l'uso di un certificato non con nessun account associato in Active Directory. Questo vale solo con modalità AllowRenewalsOnly e ClientCertificate

[-config nome computer\CA] [-dc DCName]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_ADCA"></a>-ADCA

CertUtil [Options] - ADCA [NomeCA]

Visualizzare le autorità di certificazione di Active Directory

[-f]. [-suddividere] [-dc DCName]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_CA"></a>-CA

CertUtil [Options] -CA [NomeCA | TemplateName]

Visualizzare le CA di criteri di registrazione

[-f] [-user] [-silent] [-split] [-PolicyServer URLOrId] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName UserName] [-p Password]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_Policy"></a>-Policy

Visualizzare i criteri di registrazione

[-f] [-user] [-silent] [-split] [-PolicyServer URLOrId] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName UserName] [-p Password]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_PolicyCache"></a>-PolicyCache

CertUtil [Options] - PolicyCache [delete]

Visualizzare o eliminare le voci della Cache dei criteri di registrazione

Elimina: eliminare le voci della cache di Server dei criteri

-f: utilizzare -f per eliminare tutte le voci della cache

[-f] [-user] [-PolicyServer URLOrId]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_Credstore"></a>-CredStore

CertUtil [Options] - CredStore [URL]

CertUtil [Options] - CredStore URL aggiungere

CertUtil [Options] - CredStore URL delete

Visualizzare, aggiungere o eliminare le voci di Store delle credenziali

URL: l'URL di destinazione.  Usare * per la corrispondenza di tutte le voci. Usare https://machine* in modo che corrisponda un prefisso URL.

Aggiungi: aggiungere una voce di Store delle credenziali. Devono anche specificare le credenziali SSL.

Elimina: eliminare le voci di Store delle credenziali

-f: utilizzare -f per sovrascrivere una voce o eliminare più voci.

[-f]. [-utente] [-invisibile all'utente] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-Nome utente di nome utente] [-p Password]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_InstallDefaultTemplates"></a>-InstallDefaultTemplates

CertUtil [Options] - InstallDefaultTemplates

Installare i modelli di certificato predefiniti

[-dc DCName]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_URLCache"></a>-URLCache

CertUtil [Options] - URLCache [URL | CRL | * [eliminazione]]

Visualizzare o eliminare le voci della cache di URL

URL: URL memorizzato nella cache

CRL: operare su tutte le cache CRL URL solo

*: operano su memorizzato nella cache tutti gli URL

Elimina: eliminare URL rilevanti dalla cache locale dell'utente corrente

Usare -f per forzare il recupero di un URL specifico e l'aggiornamento della cache.

[-f]. [-suddividere]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_pulse"></a>-pulse

CertUtil [opzioni] - pulse

Eventi di registrazione automatica di Pulse

[-user]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_MachineInfo"></a>-MachineInfo

CertUtil [Options] - MachineInfo DomainName\MachineName$

Visualizzare le informazioni di oggetto computer di Active Directory

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_DCInfo"></a>-DCInfo

CertUtil [Options] - DCInfo [Domain] [verificare | DeleteBad | DeleteAll]

Visualizzare le informazioni sul controller di dominio

Valore predefinito è per visualizzare i certificati di controller di dominio senza verifica

[-f]. [-utente] [-urlfetch] [-dc DCName] [-t Timeout]

> [!TIP]
> La possibilità di specificare un dominio di Active Directory Domain Services (AD DS) **[Domain]** e specificare un controller di dominio (**-dc**) è stata aggiunta in Windows Server 2012. Per eseguire il comando, è necessario usare un account che è un membro del **Domain Admins** oppure **Enterprise Admins**. Come indicato di seguito sono riportate le modifiche di comportamento di questo comando:</br>> 1.  Se non viene specificato un dominio e non viene specificato un controller di dominio specifico, questa opzione restituisce un elenco dei controller di dominio per l'elaborazione dal controller di dominio predefinito.</br>> 2.  Se non viene specificato un dominio, ma viene specificato un controller di dominio, viene generato un report dei certificati nel controller di dominio specificato.</br>> 3.  Se viene specificato un dominio, ma non viene specificato un controller di dominio, viene generato un elenco di controller di dominio insieme ai report sui certificati per ogni controller di dominio nell'elenco.</br>> 4.  Se il dominio e il controller di dominio vengono specificati, viene generato un elenco di controller di dominio dal controller di dominio di destinazione. Viene inoltre generato un report dei certificati per ogni controller di dominio nell'elenco.

Si supponga, ad esempio, un dominio denominato CPANDL con un controller di dominio denominato CPANDL-DC1. È possibile eseguire il comando seguente per recuperare un elenco di controller di dominio e i relativi certificati che da CPANDL DC1: certutil -dc cpandl-dc1 - dcinfo cpandl

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_EntInfo"></a>-EntInfo

CertUtil [Options] - EntInfo DomainName\MachineName$

[-f]. [-utente]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_TCAInfo"></a>-TCAInfo

CertUtil [Options] - TCAInfo [DomainDN |-]

Visualizza informazioni sulla CA

[-f]. [-enterprise] [-utente] [-urlfetch] [-dc DCName] [-t Timeout]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_SCInfo"></a>-SCInfo

CertUtil [Options] -SCInfo [ReaderName [CRYPT_DELETEKEYSET]]

Visualizzare informazioni sulle smart card

CRYPT_DELETEKEYSET: Eliminare tutte le chiavi nella smart card

[-invisibile all'utente] [-suddividere] [-urlfetch] [-t Timeout]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_SCRoots"></a>-SCRoots

CertUtil [Options] - SCRoots aggiornare [+] [InputRootFile] [nome lettore]

CertUtil [Options] - SCRoots salvare @OutputRootFile [nome lettore]

CertUtil [Options] - SCRoots visualizzazione [InputRootFile | Nome lettore]

CertUtil [Options] - SCRoots delete [nome lettore]

Gestire i certificati radice di smart card

[-f]. [-suddividere] [-p Password]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_verifykeys"></a>-verifykeys

CertUtil [Options] - verifykeys [KeyContainerName FileCertCA]

Verificare i set di chiavi pubblica/privata

KeyContainerName: il nome di contenitore di chiavi della chiave da verificare. Valore predefinito per le chiavi del computer.  Utilizzare - utente per le chiavi utente.

FileCertCA: file di certificato di firma o crittografia

Se viene specificato alcun argomento, ogni certificato di firma della CA viene confrontata con la relativa chiave privata.

Questa operazione può essere eseguita solo da un'autorità di certificazione locale o delle chiavi locale.

[-f]. [-utente] [-invisibile all'utente] [-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_verify"></a>-verify

CertUtil [opzioni] - verificare CertFile [ApplicationPolicyList |-[IssuancePolicyList]]

CertUtil [opzioni] - verificare CertFile [FileCertCA [viene]]

CertUtil [opzioni] - verificare FileCRL FileCertCA [IssuedCertFile]

CertUtil [opzioni] - verificare FileCRL FileCertCA [FileCertRilasciato]

Verificare i certificati o CRL catena

CertFile: Certificato per la verifica

ApplicationPolicyList: elenco separato da virgole facoltativo ObjectID di criteri di applicazione richiesto

IssuancePolicyList: elenco separato da virgole facoltativo ObjectID di criteri di rilascio necessaria

FileCertCA: facoltativa emissione certificato della CA da verificare

Viene: certificato facoltativo certificazione con certificazione incrociata da CertFile

CRLFile: CRL per verificare

IssuedCertFile: certificato rilasciato facoltativa coperto da FileCRL

FileCertRilasciato: un delta CRL facoltativo

Se viene specificato ApplicationPolicyList, la creazione della catena è limitato a catene valide per i criteri dell'applicazione specificato.

Se viene specificato IssuancePolicyList, la creazione della catena è limitato a catene valide per i criteri di rilascio specificata.

Se viene specificato, viene verificati ElencoCriteriRilascio contro CertFile o FileCRL.

Se non viene specificato CertFile viene utilizzato per compilare e verificare una catena completa.

Se non sono entrambi specificati, i campi in non vengono verificati rispetto CertFile.

Se FileCertCAIncroc, i campi in IssuedCertFile vengono verificati rispetto a FileCRL.

Se viene specificato FileCertRilasciato, campi in FileCertRilasciato vengono verificati rispetto a FileCRL.

[-f]. [-enterprise] [-utente] [-invisibile all'utente] [-suddividere] [-urlfetch] [-t Timeout]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_verifyCTL"></a>-verifyCTL

CertUtil [Options] - verifyCTL CTLObject [CertDir] [CertFile]

Verificare AuthRoot o certificati elenco scopi consentiti non consentiti

CTLObject: Identifica l'elenco di scopi consentiti da verificare:
-   AuthRootWU: leggere file CAB AuthRoot e certificati corrispondenti dalla cache di URL. Usare -f per il download da Windows Update invece.
-   DisallowedWU: leggere file CAB di certificati non consentiti e non consentite di file dell'archivio certificati dalla cache di URL.  Usare -f per il download da Windows Update invece.
-   AuthRoot: lettura Registro di sistema AuthRoot CTL memorizzati nella cache.  Uso con -f e un CertFile che non è già considerato attendibile per forzare l'aggiornamento del Registro di sistema e memorizzati nella cache AuthRoot CTLs certificati non consentiti.
-   Non consentito: lettura Registro di sistema non consentiti dei certificati CTL memorizzati nella cache. -f ha lo stesso comportamento come con AuthRoot.
-   CTLFileName: file o http: percorso di elenco di scopi consentiti o il file CAB

CertDir: cartella contenenti certificati corrispondenti di voci di elenco di scopi consentiti. Http: percorso della cartella deve terminare con un separatore di percorso. Se non è specificata una cartella con AuthRoot o non consentito, verranno cercati più percorsi per la corrispondenza dei certificati: archivi di certificati locali, crypt32.dll risorse e la cache di URL locale. Usare -f per il download da Windows Update quando necessario. In caso contrario, per impostazione predefinita la cartella o del sito web stesso come il CTLObject.

CertFile: file che contiene uno o più certificati per verificare. Certificati verranno confrontati con le voci di elenco di scopi consentiti e corrispondono ai risultati visualizzati. Elimina la maggior parte dell'output predefinito.

[-f]. [-utente] [-suddividere]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_sign"></a>-sign

CertUtil [Options] -sign InFileList|SerialNumber|CRL OutFileList [StartDate+dd:hh] [+SerialNumberList | -SerialNumberList | -ObjectIdList | @ExtensionFile]

CertUtil [opzioni] - accedere InFileList | SerialNumber | CRL OutFileList [#HashAlgorithm] [Alternatesignaturealgorithm=1 + | - Alternatesignaturealgorithm=1]

Firmare nuovamente i certificati o CRL

InFileList: elenco delimitato da virgole di file di certificato o un CRL da modificare e firmare nuovamente

Numero di serie: Numero di serie del certificato da creare. Periodo di validità e altre opzioni non devono essere presente.

CRL: Creare un CRL vuoto. Periodo di validità e altre opzioni non devono essere presente.

OutFileList: elenco delimitato da virgole di file di output di certificati o CRL modificati. Il numero di file deve corrispondere InFileList.

StartDate + gg: periodo di validità: date facoltative più; giorni facoltativi e il periodo di validità di ore; Se vengono specificati entrambi, usare un separatore di segno più (+). Usare "ora [+ gg]" per avviare la fase corrente. Usare "mai" che nessuna data di scadenza (CRL).

ElencoNumeriSerie: elenco delimitato da virgole numero di serie per aggiungere o rimuovere

ElencoIDOggetto: elenco elenco delimitato da virgole estensione ObjectId da rimuovere

@ExtensionFile: File INF contenente le estensioni per aggiornare o rimuovere:
```
[Extensions]
     2.5.29.31 = ; Remove CRL Distribution Points extension
     2.5.29.15 = "{hex}" ; Update Key Usage extension
     _continue_="03 02 01 86"
```
HashAlgorithm: Nome dell'algoritmo hash preceduto da un simbolo #

Alternatesignaturealgorithm=1: identificatore algoritmo di firma alternativo

Un segno meno (-) fa sì che i numeri di serie e le estensioni da rimuovere. Un segno fa in modo che i numeri di serie da aggiungere a un elenco CRL. Quando si rimuove elementi da un elenco CRL, l'elenco può contenere sia numeri di serie e ObjectID. Un segno meno prima Alternatesignaturealgorithm=1 fa sì che il formato della firma legacy da utilizzare. Un segno prima Alternatesignaturealgorithm=1 fa sì che il formato della firma alternature da utilizzare. Se non viene specificato Alternatesignaturealgorithm=1 viene utilizzato il formato della firma nel certificato o CRL.

[-nullsign] [-f]. [-invisibile all'utente] [-Cert CertId]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_vroot"></a>-vroot

CertUtil [Options] - vroot [delete]

Creare o eliminare le radici virtuali web e le condivisioni file

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_vocsproot"></a>-vocsproot

CertUtil [Options] - vocsproot [delete]

Creare o eliminare le radici virtuali web per il proxy web OCSP

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_addEnrollmentServer"></a>-addEnrollmentServer

CertUtil [Options] - addEnrollmentServer Kerberos | Nome utente | ClientCertificate [AllowRenewalsOnly] [AllowKeyBasedRenewal]

Aggiungere un'applicazione di registrazione Server

Aggiungere un'applicazione Server di registrazione e un pool di applicazioni, se necessario, per l'autorità di certificazione specificata. Questo comando non vengono installati i file binari o i pacchetti. Uno dei seguenti metodi di autenticazione con il quale il client si connette a un Server di registrazione certificati.
-   Kerberos: Usare le credenziali Kerberos SSL
-   Nome utente: Utilizzare account denominato credenziali SSL
-   ClientCertificate: Usare le credenziali di certificato X.509 SSL
-   AllowRenewalsOnly: Solo le richieste di rinnovo possono essere inviate a questa CA tramite questo URL
-   AllowKeyBasedRenewal - Consente l'uso di un certificato non con nessun account associato in Active Directory. Questo vale solo con modalità ClientCertificate e AllowRenewalsOnly.

[-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_deleteEnrollmentServer"></a>-deleteEnrollmentServer

CertUtil [Options] - deleteEnrollmentServer Kerberos | Nome utente | ClientCertificate

Eliminare un'applicazione di registrazione Server

Eliminare un'applicazione Server di registrazione e un pool di applicazioni, se necessario, per l'autorità di certificazione specificata. Questo comando non rimuove i file binari o i pacchetti. Uno dei seguenti metodi di autenticazione con il quale il client si connette a un Server di registrazione certificati.
1.  Kerberos: Usare le credenziali Kerberos SSL
2.  Nome utente: Utilizzare account denominato credenziali SSL
3.  ClientCertificate: Usare le credenziali di certificato X.509 SSL

[-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_addPolicyServer"></a>-addPolicyServer

CertUtil [Options] - addPolicyServer Kerberos | Nome utente | ClientCertificate [KeyBasedRenewal]

Aggiungere un'applicazione Server dei criteri

Se necessario, aggiungere un pool di applicazioni e un'applicazione Server dei criteri. Questo comando non vengono installati i file binari o i pacchetti. Uno dei seguenti metodi di autenticazione con il quale il client si connette a un Server criteri di certificato:
-   Kerberos: Usare le credenziali Kerberos SSL
-   Nome utente: Utilizzare account denominato credenziali SSL
-   ClientCertificate: Usare le credenziali di certificato X.509 SSL
-   KeyBasedRenewal: Solo i criteri che contengono i modelli KeyBasedRenewal vengono restituiti al client. Questo flag si applica solo per l'autenticazione di nome utente e ClientCertificate.

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_deletePolicyServer"></a>-deletePolicyServer

CertUtil [Options] - deletePolicyServer Kerberos | Nome utente | ClientCertificate [KeyBasedRenewal]

Eliminazione di un'applicazione Server dei criteri

Se necessario, eliminare un pool di applicazioni e un'applicazione Server dei criteri. Questo comando non rimuove i file binari o i pacchetti. Uno dei seguenti metodi di autenticazione con il quale il client si connette a un Server criteri di certificato:
1.  Kerberos: Usare le credenziali Kerberos SSL
2.  Nome utente: Utilizzare account denominato credenziali SSL
3.  ClientCertificate: Usare le credenziali di certificato X.509 SSL
4.  KeyBasedRenewal: Server dei criteri KeyBasedRenewal

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_oid"></a>-oid

CertUtil [Options] - oid ObjectId [DisplayName | Elimina [LanguageId [tipo]]]

CertUtil [Options] - oid GroupId

CertUtil [Options] - oid AlgId | AlgorithmName [GroupId]

Visualizza ID oggetto o imposta il nome di visualizzazione
-   ObjectId: ObjectId per visualizzare o aggiungere nome visualizzato
-   GroupId: numero di GroupId decimale per ObjectIds enumerare
-   AlgId - AlgId esadecimale per l'ID oggetto cercare
-   AlgorithmName: Nome dell'algoritmo per l'ID oggetto cercare
-   DisplayName: Nome visualizzato per l'archiviazione di dominio Active Directory
-   Delete: eliminazione nome visualizzato
-   LanguageId: Id lingua (valore predefinito è corrente: 1033)
-   Tipo-- Crea tipo di oggetto DS: 1 per il modello (predefinito), 2 per i criteri di rilascio, 3 per i criteri di applicazione
-   Consente di creare un oggetto DS -f.

[-f]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_error"></a>-error

CertUtil [opzioni] - errore ErrorCode

Visualizzazione di testo del messaggio di errore

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_getreg"></a>-getreg

CertUtil [Options] - getreg [{autorità di certificazione | Ripristina | criteri | uscire | modello | registrare | catena | PolicyServers}\[ProgId\]] [RegistryValueName]

Visualizzare il valore del Registro di sistema

ca: Chiave del Registro di sistema dell'autorità di certificazione di utilizzo

Ripristino: Chiave autorità di certificazione ripristino del Registro di sistema uso

Criteri: Usare la chiave di registro del modulo criteri

uscita: Usare prima di tutto chiudere la chiave di registro del modulo

Modello: Usare una chiave del Registro di sistema del modello (usare - utente per i modelli utente)

eseguire la registrazione: Usare una chiave del Registro di sistema di registrazione (usare - utente per il contesto utente)

catena: Usare una chiave del Registro di sistema di configurazione catena

PolicyServers: Usare la chiave del Registro di sistema i server dei criteri

ProgId: Utilizzare i criteri o uscita ProgId del modulo (nome della sottochiave del Registro di sistema)

RegistryValueName: nome del valore del Registro di sistema (usare "Nome *" come prefisso di corrispondenza)

Valore: nuovo numerici, stringa o data di un valore del Registro di sistema o filename. Se un valore numerico inizia con "+" o "-", i bit specificati in quello nuovo vengono impostati o cancellati nel valore del Registro di sistema esistente.

Se un valore stringa inizia con "+" o "-" e il valore esistente è un valore REG_MULTI_SZ, la stringa viene aggiunto o rimosso dal valore del Registro di sistema esistente. Per forzare la creazione di un valore REG_MULTI_SZ, aggiungere un "\n" alla fine del valore della stringa.

Se il valore inizia con "@", il resto del valore è il nome del file contenente la rappresentazione di testo esadecimale di un valore binario. Se non fa riferimento a un file valido, viene invece analizzato come [Date] [+ |-] [gg]: una data facoltativa più o meno facoltativi giorni e ore. Se vengono specificati entrambi, utilizzare un segno più (+) o un separatore di segno di sottrazione (-). Per una data rispetto all'ora corrente, usare "ora + gg".

Usare "chain\ChainCacheResyncFiletime @now" per scaricare in modo efficace i CRL memorizzato nella cache.

[-f]. [-utente] [-GroupPolicy] [-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_setreg"></a>-setreg

CertUtil [Options] - setreg [{autorità di certificazione | Ripristina | criteri | uscire | modello | registrare | catena | PolicyServers}\[ProgId\]] valore RegistryValueName

Valore del Registro di sistema set

ca: Chiave del Registro di sistema dell'autorità di certificazione di utilizzo

Ripristino: Chiave autorità di certificazione ripristino del Registro di sistema uso

Criteri: Usare la chiave di registro del modulo criteri

uscita: Usare prima di tutto chiudere la chiave di registro del modulo

Modello: Usare una chiave del Registro di sistema del modello (usare - utente per i modelli utente)

eseguire la registrazione: Usare una chiave del Registro di sistema di registrazione (usare - utente per il contesto utente)

catena: Usare una chiave del Registro di sistema di configurazione catena

PolicyServers: Usare la chiave del Registro di sistema i server dei criteri

ProgId: Utilizzare i criteri o uscita ProgId del modulo (nome della sottochiave del Registro di sistema)

RegistryValueName: nome del valore del Registro di sistema (usare "Nome *" come prefisso di corrispondenza)

Valore: nuovo numerici, stringa o data di un valore del Registro di sistema o filename. Se un valore numerico inizia con "+" o "-", i bit specificati in quello nuovo vengono impostati o cancellati nel valore del Registro di sistema esistente.

Se un valore stringa inizia con "+" o "-" e il valore esistente è un valore REG_MULTI_SZ, la stringa viene aggiunto o rimosso dal valore del Registro di sistema esistente. Per forzare la creazione di un valore REG_MULTI_SZ, aggiungere un "\n" alla fine del valore della stringa.

Se il valore inizia con "@", il resto del valore è il nome del file contenente la rappresentazione di testo esadecimale di un valore binario. Se non fa riferimento a un file valido, viene invece analizzato come [Date] [+ |-] [gg]: una data facoltativa più o meno facoltativi giorni e ore. Se vengono specificati entrambi, utilizzare un segno più (+) o un separatore di segno di sottrazione (-). Per una data rispetto all'ora corrente, usare "ora + gg".

Usare "chain\ChainCacheResyncFiletime @now" per scaricare in modo efficace i CRL memorizzato nella cache.

[-f]. [-utente] [-GroupPolicy] [-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_delreg"></a>-delreg

CertUtil [Options] - delreg [{autorità di certificazione | Ripristina | criteri | uscire | modello | registrare | catena | PolicyServers}\[ProgId\]] [RegistryValueName]

Eliminare il valore del Registro di sistema

ca: Chiave del Registro di sistema dell'autorità di certificazione di utilizzo

Ripristino: Chiave autorità di certificazione ripristino del Registro di sistema uso

Criteri: Usare la chiave di registro del modulo criteri

uscita: Usare prima di tutto chiudere la chiave di registro del modulo

Modello: Usare una chiave del Registro di sistema del modello (usare - utente per i modelli utente)

eseguire la registrazione: Usare una chiave del Registro di sistema di registrazione (usare - utente per il contesto utente)

catena: Usare una chiave del Registro di sistema di configurazione catena

PolicyServers: Usare la chiave del Registro di sistema i server dei criteri

ProgId: Utilizzare i criteri o uscita ProgId del modulo (nome della sottochiave del Registro di sistema)

RegistryValueName: nome del valore del Registro di sistema (usare "Nome *" come prefisso di corrispondenza)

Valore: nuovo numerici, stringa o data di un valore del Registro di sistema o filename. Se un valore numerico inizia con "+" o "-", i bit specificati in quello nuovo vengono impostati o cancellati nel valore del Registro di sistema esistente.

Se un valore stringa inizia con "+" o "-" e il valore esistente è un valore REG_MULTI_SZ, la stringa viene aggiunto o rimosso dal valore del Registro di sistema esistente. Per forzare la creazione di un valore REG_MULTI_SZ, aggiungere un "\n" alla fine del valore della stringa.

Se il valore inizia con "@", il resto del valore è il nome del file contenente la rappresentazione di testo esadecimale di un valore binario. Se non fa riferimento a un file valido, viene invece analizzato come [Date] [+ |-] [gg]: una data facoltativa più o meno facoltativi giorni e ore. Se vengono specificati entrambi, utilizzare un segno più (+) o un separatore di segno di sottrazione (-). Per una data rispetto all'ora corrente, usare "ora + gg".

Usare "chain\ChainCacheResyncFiletime @now" per scaricare in modo efficace i CRL memorizzato nella cache.

[-f]. [-utente] [-GroupPolicy] [-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_ImportKMS"></a>-ImportKMS

CertUtil [Options] - ImportKMS FileCertEChiaveUtente [CertId]

Importare certificati e chiavi utente nel database del server per l'archiviazione della chiave

FileCertEChiaveUtente-- File di dati che contiene le chiavi private di utente e i certificati per l'archiviazione.  Può trattarsi di uno dei seguenti:
-   File di esportazione di Exchange Server KMS (Key Management)
-   File PFX

CertId: Token corrispondenza certificati di decrittografia file di esportazione KMS.  Visualizzare [-archiviare](#BKMK_Store).

Usare -f per importare i certificati non emessi dall'autorità di certificazione.

[-f] [-silent] [-split] [-config Machine\CAName] [-p Password] [-symkeyalg SymmetricKeyAlgorithm[,KeyLength]]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_ImportCert"></a>-ImportCert

CertUtil [Options] - ImportCert Certfile [ExistingRow]

Importare un file di certificato nel database

Usare ExistingRow per importare il certificato al posto di una richiesta in sospeso per la stessa chiave.

Usare -f per importare i certificati non emessi dall'autorità di certificazione.

L'autorità di certificazione potrebbe essere necessario anche essere configurato per supportare l'importazione del certificato esterna: certutil - setreg ca\KRAFlags + KRAF_ENABLEFOREIGN

[-f]. [-config nome computer\CA]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_GetKey"></a>-GetKey

CertUtil [Options] - Getkeym SearchToken [FileOutBlobRecupero]

CertUtil [Options] - Getkeym script SearchToken OutputScriptFile

CertUtil [Options] - Getkeym SearchToken recuperare | ripristinare OutputFileBaseName

Recuperare i blob archiviati recupero delle chiavi private, generare uno script di ripristino o ripristinare le chiavi archiviate

script: generare uno script per recuperare e ripristinare le chiavi (comportamento predefinito se vengono trovati più candidati di ripristino corrispondente o se il file di output non è specificato).

recuperare: recuperare uno o più BLOB di chiave di ripristino (comportamento predefinito se esattamente un candidato di ripristino corrispondente viene trovato e se viene specificato il file di output)

ripristino: recuperare e ripristinare le chiavi private in un unico passaggio (richiede certificati di Key Recovery Agent e chiavi private)

SearchToken: Consente di selezionare le chiavi e certificati da ripristinare.

può essere uno dei seguenti:
1.  Nome comune del certificato
2.  Numero di serie certificato
3.  Hash SHA-1 del certificato (identificazione personale)
4.  Hash del certificato KeyId SHA-1 (Subject Key Identifier)
5.  Nome del richiedente (Dominio\Utente)
6.  UPN (user@domain)

FileOutBlobRecupero: file di output che contiene una catena di certificati e una chiave privata associata, ancora crittografati per uno o più certificati di Key Recovery Agent.

OutputScriptFile: file di output contenente uno script batch per recuperare e ripristinare le chiavi private.

OutputFileBaseName: nome base file output. Per recuperare, qualsiasi estensione viene troncata e una stringa specifica del certificato e l'estensione. REC vengono accodati per ogni blob di recupero chiavi.  Ogni file contiene una catena di certificati e una chiave privata associata, ancora crittografati per uno o più certificati di Key Recovery Agent. Per il ripristino, qualsiasi estensione viene troncata e viene aggiunta l'estensione. p12.  Contiene le catene di certificati ripristinato e le chiavi private associate, archiviate come file PFX.

[-f] [-UnicodeText] [-silent] [-config Machine\CAName] [-p Password] [-ProtectTo SAMNameAndSIDList] [-csp Provider]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_RecoverKey"></a>-RecoverKey

CertUtil [Options] - RecoverKey RecoveryBlobInFile [FileOutputPFX [RecipientIndex]]

Recuperare la chiave privata archiviata

[-f]. [-utente] [-invisibile all'utente] [-suddividere] [-p Password] [-ProtectTo SAMNameAndSIDList] [-csp Provider] [-t Timeout]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_MergePFX"></a>-MergePFX

CertUtil [Options] - MergePFX ElencoFileInPFX FileOutputPFX [ExtendedProperties]

PFXInFileList: Elenco delimitato da virgole PFX file di input

PFXOutFile: File di output PFX

Proprietà estese: Includere le proprietà estese

La password specificata nella riga di comando è un elenco di password separati da virgole.  Se è specificata più di una password, l'ultima password viene usata per il file di output.  Se solo una viene fornita una password o se l'ultima password è "*", l'utente verrà richiesto per la password del file di output.

[-f]. [-utente] [-suddividere] [-p Password] [-ProtectTo SAMNameAndSIDList] [-csp Provider]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_ConvertEPF"></a>-ConvertEPF

CertUtil [Options] - ConvertEPF ElencoFileInPFX EPFOutFile [cast | cast-] [V3CACertId] [, Salt]

Convertire i file PFX in file EPF

PFXInFileList: Elenco delimitato da virgole PFX file di input

EPF: File di output EPF

cast: Usare la crittografia 64 CAST

cast-: Usare la crittografia 64 CAST (esportazione)

V3CACertId: Token di corrispondenza certificati di autorità di certificazione v3.  Visualizzare [-archiviare](#BKMK_Store) CertId descrizione.

Valore salt: Stringa salt EPF output file

La password specificata nella riga di comando è un elenco di password separati da virgole. Se è specificata più di una password, l'ultima password viene usata per il file di output.  Se solo una viene fornita una password o se l'ultima password è "*", l'utente verrà richiesto per la password del file di output.

[-f]. [-invisibile all'utente] [-suddividere] [-dc DCName] [-p Password] [-csp Provider]

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_Options"></a>Opzioni

In questa sezione definisce le opzioni che è possibile specificare con il comando.

|Opzioni|Descrizione|
|-------|-----------|
|-nullsign|Utilizzare l'hash dei dati come firma|
|-f|Forzare la sovrascrittura|
|-enterprise|Usare l'archivio certificati del computer locale dell'organizzazione del Registro di sistema|
|-utente|Usare l'archivio certificati o chiavi HKEY_CURRENT_USER|
|-GroupPolicy|Archivio certificati di criteri di gruppo utilizzo|
|-ut|Visualizzare i modelli utente|
|-mt|Visualizzare i modelli di computer|
|-Unicode|Scrivere l'output reindirizzato in formato Unicode|
|-UnicodeText|Scrivere il file di output in formato Unicode|
|-ora di Greenwich|Orari di visualizzazione come GMT|
|-secondi|Orari di visualizzazione con secondi e millisecondi|
|-silent|Usare il flag invisibile all'utente per acquisire il contesto|
|-split|Suddividere gli elementi ASN.1 incorporati e Salva su file|
|-v|Operazione dettagliato|
|-privatekey|Visualizzare i dati della chiave privati e password|
|-aggiungere PIN|PIN della Smart Card|
|-urlfetch|Recuperare e verificare i certificati di AIA e CDP CRL|
|-config Machine\CAName|Stringa del nome della CA e il computer|
|-PolicyServer URLOrId|URL del Server dei criteri o ID. Per la selezione U / I, usare - PolicyServer. Per tutti i server dei criteri, usare - PolicyServer *|
|-Anonima|Utilizzare credenziali anonime SSL|
|-Kerberos|Usare le credenziali Kerberos SSL|
|-ClientCertificate ClientCertId|Usare le credenziali di certificato X.509 SSL. Per la selezione U / I, usare - clientCertificate.|
|Nome utente - UserName|Utilizzare account denominato credenziali SSL. Per la selezione U / I, usare - UserName.|
|-CertId cert|Certificato di firma|
|-dc DCName|Destinazione di un Controller di dominio specifico|
|-limitare RestrictionList|Elenco delimitato da virgole restrizione. Ogni restrizione è costituito da un nome di colonna, un operatore relazionale e una costante integer, stringa o Data. Un nome di colonna può essere preceduto da un segno più o meno per indicare l'ordinamento. Esempi:</br>"RequestId = 47"</br>"+ RequesterName > =, RequesterName < b"</br>"-RequesterName > DOMAIN, Disposition = 21"|
|-out ColumnList|Colonna di elenco delimitato da virgole|
|-p Password|Password|
|-ProtectTo SAMNameAndSIDList|Elenco delimitato da virgole SAM nome/SID|
|-csp Provider|Provider|
|-t Timeout|URL recupero timeout in millisecondi|
|-symkeyalg SymmetricKeyAlgorithm[,KeyLength]|Nome dell'algoritmo della chiave simmetrica con lunghezza della chiave facoltativa, esempio: AES, 128 o 3DES|

Tornare a [Menu](#BKMK_menu)

## <a name="BKMK_AddedExamples"></a>Esempi aggiuntivi di certutil

Per alcuni esempi di come usare questo comando, vedere
1.  [Esempi di certutil per la gestione dei servizi certificati Active Directory (AD CS) dalla riga di comando](https://social.technet.microsoft.com/wiki/contents/articles/3063.certutil-examples-for-managing-active-directory-certificate-services-ad-cs-from-the-command-line.aspx)
2.  [Attività di certutil per la gestione dei certificati](https://technet.microsoft.com/library/cc772898.aspx)
3.  [Esportazione richiesta binario mediante la procedura dettagliata dello strumento da riga di comando CertUtil.exe](https://social.technet.microsoft.com/wiki/contents/articles/7573.active-directory-certificate-services-pki-key-archival-and-management.aspx)
4.  [Rinnovo del certificato CA radice](https://social.technet.microsoft.com/wiki/contents/articles/2016.root-ca-certificate-renewal.aspx)
5.  [Certutil](https://msdn.microsoft.com/subscriptions/cc773087.aspx)

Tornare a [Menu](#BKMK_menu)
