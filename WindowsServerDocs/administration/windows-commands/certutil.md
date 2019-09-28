---
title: certutil
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 45c9946cc53fe3a901c3f6ee53f082a5b3d086c0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379646"
---
# <a name="certutil"></a>certutil

Certutil. exe è un programma da riga di comando che viene installato come parte di Servizi certificati. È possibile utilizzare certutil. exe per eseguire il dump e visualizzare le informazioni di configurazione dell'autorità di certificazione (CA), configurare i servizi certificati, eseguire il backup e ripristinare i componenti CA e verificare i certificati, le coppie di chiavi e le catene di certificati.

Quando certutil viene eseguito in un'autorità di certificazione senza parametri aggiuntivi, Visualizza la configurazione dell'autorità di certificazione corrente. Quando cerutil viene eseguito in un'autorità non di certificazione, per impostazione predefinita il comando esegue il verbo certutil [-dump](#-dump) .

> [!WARNING]
> Le versioni precedenti di certutil potrebbero non fornire tutte le opzioni descritte in questo documento. È possibile visualizzare tutte le opzioni fornite da una versione specifica di certutil eseguendo i comandi illustrati nella sezione relativa alla [notazione della sintassi](#syntax-notations) .

## <a name="menu"></a>Menu

Le sezioni principali di questo documento sono:

- [Verbi](#verbs)
- [Notazione della sintassi](#syntax-notations)
- [Opzioni](#options)
- [Esempi di certutil aggiuntivi](#additional-certutil-examples)

## <a name="verbs"></a>Verbi

La tabella seguente descrive i verbi che possono essere usati con il comando certutil.

|Verbi|Descrizione|
|-----|-----------|
|[-dump](#-dump)|Dump di informazioni o file di configurazione|
|[-ASN](#-asn)|Analizza file ASN. 1|
|[-decodehex](#-decodehex)|Decodifica file con codifica esadecimale|
|[-decodifica](#-decode)|Decodifica di un file con codifica Base64|
|[-codifica](#-encode)|Codificare un file in Base64|
|[-nega](#-deny)|Negare una richiesta di certificato in sospeso|
|[-Invia di più](#-resubmit)|Invia nuovamente una richiesta di certificato in sospeso|
|[-SetAttributes](#-setattributes)|Impostare gli attributi per una richiesta di certificato in sospeso|
|[-seextension](#-setextension)|Imposta un'estensione per una richiesta di certificato in sospeso|
|[-revoca](#-revoke)|Revoca di un certificato|
|[-IsValid](#-isvalid)|Visualizza la disposizione del certificato corrente|
|[-GetConfig](#-getconfig)|Ottenere la stringa di configurazione predefinita|
|[-ping](#-ping)|Tentativo di contattare il Active Directory interfaccia richiesta di Servizi certificati|
|-pingadmin|Tentativo di contattare l'interfaccia di amministrazione di Servizi certificati Active Directory|
|[-Cainfo](#-cainfo)|Visualizzare informazioni sull'autorità di certificazione|
|[-CA. cert](#-cacert)|Recuperare il certificato per l'autorità di certificazione|
|[-CA. Chain](#-cachain)|Recuperare la catena di certificati per l'autorità di certificazione|
|[-GetCRL](#-getcrl)|Ottenere un elenco di revoche di certificati (CRL)|
|[-CRL](#-crl)|Pubblicare nuovi elenchi di revoche di certificati (CRL) [o solo Delta CRL]|
|[-arresta](#-shutdown)|Arrestare Active Directory Servizi certificati|
|[-installCert](#-installcert)|Installare un certificato dell'autorità di certificazione|
|[-renewCert](#-renewcert)|Rinnovare un certificato dell'autorità di certificazione|
|[-Schema](#-schema)|Dump dello schema per il certificato|
|[-Visualizza](#-view)|Dump della visualizzazione certificati|
|[-DB](#-db)|Dump del database non elaborato|
|[-DeleteRow](#-deleterow)|Eliminare una riga dal database del server|
|[-backup](#-backup)|Servizi certificati Active Directory di backup|
|[-backupDB](#-backupdb)|Eseguire il backup del database di Servizi certificati Active Directory|
|[-backupKey](#-backupkey)|Eseguire il backup del certificato di Servizi certificati Active Directory e della chiave privata|
|[-ripristino](#-restore)|Ripristinare Servizi certificati Active Directory|
|[-restoreDB](#-restoredb)|Ripristinare il database di Servizi certificati Active Directory|
|[-restoreKey](#-restorekey)|Ripristinare il certificato di Servizi certificati Active Directory e la chiave privata|
|[-importPFX](#-importpfx)|Importa certificato e chiave privata|
|[-dynamicfilelist](#-dynamicfilelist)|Visualizzare un elenco di file dinamici|
|[-databaselocations](#-databaselocations)|Visualizza percorsi database|
|[-hashfile](#-hashfile)|Generare e visualizzare un hash crittografico su un file|
|[-Archivio](#-store)|Dump dell'archivio certificati|
|[-addstore](#-addstore)|Aggiungere un certificato all'archivio|
|[-delstore](#-delstore)|Elimina un certificato dall'archivio|
|[-verifystore](#-verifystore)|Verificare un certificato nell'archivio|
|[-repairstore](#-repairstore)|Ripristinare un'associazione di chiavi o aggiornare le proprietà del certificato o il descrittore di sicurezza della chiave|
|[-viewstore](#-viewstore)|Dump dell'archivio certificati|
|[-viewdelstore](#-viewdelstore)|Elimina un certificato dall'archivio|
|[-dsPublish](#-dspublish)|Pubblicare un certificato o un elenco di revoche di certificati (CRL) in Active Directory|
|[-ADTemplate](#-adtemplate)|Visualizzare i modelli AD|
|[-Modello](#-template)|Visualizzazione di modelli di certificato|
|[-TemplateCAs](#-templatecas)|Visualizzare le autorità di certificazione (CAs) per un modello di certificato|
|[-Catemplates](#-catemplates)|Visualizza modelli per CA|
|[-SetCASites](#-setcasites)|Gestire i nomi dei siti per le autorità di certificazione|
|[-enrollmentServerURL](#-enrollmentserverurl)|Visualizzare, aggiungere o eliminare gli URL del server di registrazione associati a una CA|
|[-ADCA](#-adca)|Visualizza CA AD|
|[-CA](#-ca)|Visualizzare le autorità di certificazione per i criteri di registrazione|
|[-Criterio](#-policy)|Visualizzare i criteri di registrazione|
|[-PolicyCache](#-policycache)|Visualizzare o eliminare le voci della cache dei criteri di registrazione|
|[-CredStore](#-credstore)|Visualizzare, aggiungere o eliminare le voci dell'archivio credenziali|
|[-InstallDefaultTemplates](#-installdefaulttemplates)|Installare i modelli di certificato predefiniti|
|[-URLCache](#-urlcache)|Visualizzare o eliminare le voci della cache URL|
|[-impulso](#-pulse)|Eventi di registrazione automatica a impulsi|
|[-MachineInfo](#-machineinfo)|Visualizza informazioni sull'oggetto computer Active Directory|
|[-DCInfo](#-dcinfo)|Visualizzare informazioni sul controller di dominio|
|[-EntInfo](#-entinfo)|Visualizzare informazioni su una CA globale (Enterprise)|
|[-TCAInfo](#-tcainfo)|Visualizzare informazioni sulla CA|
|[-SCInfo](#-scinfo)|Visualizzare informazioni sulla smart card|
|[-SCRoots](#-scroots)|Gestire i certificati radice della smart card|
|[-verifykeys](#-verifykeys)|Verificare un set di chiavi pubbliche o private|
|[-Verifica](#-verify)|Verificare un certificato, un elenco di revoche di certificati (CRL) o una catena di certificati|
|[-verifyCTL](#-verifyctl)|Verificare AuthRoot o i certificati non consentiti CTL|
|[-firma](#-sign)|Firma di nuovo un elenco di revoche di certificati (CRL) o un certificato|
|[-vroot](#-vroot)|Creare o eliminare le radici virtuali Web e le condivisioni file|
|[-vocsproot](#-vocsproot)|Creare o eliminare radici virtuali Web per un proxy Web OCSP|
|[-addEnrollmentServer](#-addenrollmentserver)|Aggiungere un'applicazione del server di iscrizione|
|[-deleteEnrollmentServer](#-deleteenrollmentserver)|Eliminare un'applicazione del server di iscrizione|
|[-addPolicyServer](#-addpolicyserver)|Aggiungere un'applicazione server dei criteri|
|[-deletePolicyServer](#-deletepolicyserver)|Eliminare un'applicazione server dei criteri|
|[-OID](#-oid)|Visualizzare l'identificatore dell'oggetto o impostare un nome visualizzato|
|[-errore](#-error)|Visualizzare il testo del messaggio associato a un codice di errore|
|[-getreg](#-getreg)|Visualizzare un valore del registro di sistema|
|[-setreg](#-setreg)|Imposta un valore del registro di sistema|
|[-delreg](#-delreg)|Eliminare un valore del registro di sistema|
|[-ImportKMS](#-importkms)|Importazione di chiavi utente e certificati nel database del server per l'archiviazione delle chiavi|
|[-ImportCert](#-importcert)|Importare un file di certificato nel database|
|[-GetKey](#-getkey)|Recuperare un BLOB di recupero della chiave privata archiviata|
|[-RecoverKey](#-recoverkey)|Ripristinare una chiave privata archiviata|
|[-MergePFX](#-mergepfx)|Unisci file PFX|
|[-ConvertEPF](#-convertepf)|Convertire un file PFX in un file EPF|
|-?|Visualizza l'elenco dei verbi|
|-verbo >-?  *\<*|Visualizza la guida per il verbo specificato.|
|-? -v|Visualizza un elenco completo di verbi e|

Torna al [menu](#menu)

## <a name="syntax-notations"></a>Notazione della sintassi

- Per la sintassi della riga di comando di base, eseguire`certutil -?`
- Per la sintassi sull'uso di certutil con un verbo specifico, eseguire **certutil**  *\<verb >* **-?**
- Per inviare tutta la sintassi certutil in un file di testo, eseguire i comandi seguenti:  
  - `certutil -v -? > certutilhelp.txt`
  - `notepad certutilhelp.txt`

Nella tabella seguente vengono descritti la notazione utilizzata per indicare la sintassi della riga di comando.


|            Notation             |                  Descrizione                  |
|---------------------------------|-----------------------------------------------|
| Testo senza parentesi quadre o parentesi graffe |         Elementi che devono essere digitati come illustrato          |
|  \<Testo racchiuso tra parentesi angolari >  | Segnaposto per il quale è necessario fornire un valore |
|  [Testo racchiuso tra parentesi quadre]  |                Elementi facoltativi                 |
|      {Testo racchiuso tra parentesi graffe}       |       Set di elementi obbligatori; scegliere una       |
|         Barra verticale (          |                       )                       |
|          Puntini di sospensione (…)           |          Elementi che possono essere ripetuti           |

Torna al [menu](#menu)

## <a name="-dump"></a>-dump

CertUtil [opzioni] [-dump]

CertUtil [opzioni] file [-dump]

Dump di informazioni o file di configurazione

[-f] [-invisibile all'utente] [-split] [-p password] [-t timeout]

Torna al [menu](#menu)

## <a name="-asn"></a>-ASN

CertUtil [opzioni]-file ASN [tipo]

Analizza file ASN. 1

Tipo: tipo di\_decodifica\_ stringa\* di crittografia numerica

Torna al [menu](#menu)

## <a name="-decodehex"></a>-decodehex

CertUtil [opzioni]-decodehex outfile outfile [tipo]

Tipo: tipo di\_codifica\_ stringa\* di crittografia numerica

[-f]

Torna al [menu](#menu)

## <a name="-decode"></a>-decodifica

CertUtil [opzioni]-decodifica file in outfile

Decodifica file con codifica Base64

[-f]

Torna al [menu](#menu)

## <a name="-encode"></a>-codifica

CertUtil [opzioni]-codificare i file in outfile

Codifica file in Base64

[-f] [-UnicodeText]

Torna al [menu](#menu)

## <a name="-deny"></a>-nega

CertUtil [opzioni]-nega RequestId

Nega richiesta in sospeso

[-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-resubmit"></a>-Invia di più

CertUtil [opzioni]-invia nuovamente RequestId

Invia nuovamente la richiesta in sospeso

[-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-setattributes"></a>-SetAttributes

CertUtil [Options]-SetAttributes RequestId AttributeString

Imposta attributi per la richiesta in sospeso

RequestId-ID richiesta numerica della richiesta in sospeso

AttributeString--coppie nome e valore dell'attributo della richiesta

- I nomi e i valori sono separati da due punti.
- Più coppie nome/valore sono separate da una nuova riga.
- Esempio: "CertificateTemplate:User\nEMail:User@Domain.com"
- Ogni sequenza "\n" viene convertita in un separatore di nuova riga.

[-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-setextension"></a>-seextension

CertUtil [Options]-disextension RequestId ExtensionName flag {Long | Data | Stringa | \@InFile}

Imposta l'estensione per la richiesta in sospeso

RequestId-ID richiesta numerica di una richiesta in sospeso

ExtensionName--ObjectId stringa dell'estensione

Flags: è consigliato 0.  1 rende l'estensione critica, 2 la Disabilita, 3 esegue entrambe.

Se l'ultimo parametro è numerico, viene considerato lungo.

Se può essere analizzata come data, viene considerata come data.

Se inizia con '\@', il resto del token è il nome file contenente i dati binari o un dump esadecimale di testo ASCII.

Qualsiasi altro elemento viene considerato come una stringa.

[-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-revoke"></a>-revoca

CertUtil [opzioni]-Revoke SerialNumber [motivo]

Revoca certificato

SerialNumber Elenco delimitato da virgole di numeri di serie del certificato da revocare

Motivo: motivo di revoca numerico o simbolico

- 0: CRL_REASON_UNSPECIFIED: Non specificato (impostazione predefinita)
- 1: CRL_REASON_KEY_COMPROMISE: Compromissione chiave
- 2: CRL_REASON_CA_COMPROMISE: Compromissione CA
- 3: CRL_REASON_AFFILIATION_CHANGED: affiliazione modificata
- 4: CRL_REASON_SUPERSEDED: Sostituito
- 5: CRL_REASON_CESSATION_OF_OPERATION: Cessazione dell'operazione
- 6: CRL_REASON_CERTIFICATE_HOLD: Possesso certificato
- 8: CRL_REASON_REMOVE_FROM_CRL: Rimuovi da CRL
- -1: Revoca Revoca

[-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-isvalid"></a>-IsValid

CertUtil [Options]-IsValid SerialNumber | CertHash

Visualizza la disposizione corrente del certificato

[-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-getconfig"></a>-GetConfig

CertUtil [opzioni]-GetConfig

Ottenere la stringa di configurazione predefinita

[-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-ping"></a>-ping

CertUtil [opzioni]-ping [MaxSecondsToWait | Cacomputer]

Ping Active Directory interfaccia richiesta servizi certificati

Camachine list: elenco di nomi di computer CA delimitati da virgole

1. Per un singolo computer, usare una virgola di terminazione
2. Visualizza il costo del sito per ogni computer della CA

[-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-cainfo"></a>-Cainfo

CertUtil [opzioni]-cainfo [InfoName [indice | ErrorCode]]

Visualizza informazioni CA

InfoName: indica la proprietà CA da visualizzare (vedere di seguito). Usare "\*" per tutte le proprietà.

Index: Indice di proprietà in base zero facoltativo

ErrorCode--codice di errore numerico

[-f] [-split] [-config Machine\CAName]

Sintassi dell'argomento InfoName:

- file Versione file
- prodotto Versione prodotto
- exitcount: Numero di moduli di uscita
- Exit [index]: Descrizione del modulo di uscita
- politica Descrizione modulo criteri
- nome Nome CA
- sanitizedname: Nome CA purificato
- dsname Nome breve CA purificato (nome DS)
- CartellaCondivisa Cartella condivisa
- ErrorCode Error1: Testo del messaggio di errore
- ErrorCode Error2: Testo del messaggio di errore e codice di errore
- tipo Tipo CA
- informazioni Informazioni CA
- padre CA padre
- certcount: Conteggio certificati CA
- xchgcount: Conteggio certificati Exchange CA
- kracount: Conteggio certificati KRA
- kraused: Conteggio utilizzato del certificato KRA
- propidmax: Numero massimo di PropId CA
- certstate [Indice]: Certificato CA
- certversion [Indice]: Versione certificato CA
- certstatuscode [Indice]: Stato verifica certificato CA
- crlstate [Indice]: CRL
- krastate [Indice]: Certificato KRA
- crossstate + [Indice]: Certificato incrociato diretto
- crossstate-[Indice]: Certificato incrociato a ritroso
- cert [Indice]: Certificato CA
- certchain [Indice]: Catena di certificati CA
- certcrlchain [Indice]: Catena di certificati CA con CRL
- xchg [Indice]: Certificato di scambio CA
- xchgchain [Indice]: Catena di certificati di scambio CA
- xchgcrlchain [Indice]: Catena di certificati di scambio CA con CRL
- Kra [Indice]: Certificato KRA
- Cross + [index]: Certificato incrociato diretto
- Cross-[Indice]: Certificato incrociato a ritroso
- CRL [Indice]: Base CRL
- deltacrl [Indice]: Delta CRL
- crlstatus [Indice]: Stato pubblicazione CRL
- deltacrlstatus [Indice]: Stato pubblicazione Delta CRL
- DNS Nome DNS
- ruolo Separazione dei ruoli
- annunci Advanced Server
- modelli Modelli
- CSP [Indice]: URL OCSP
- Aia [Indice]: URL AIA
- CDP [Indice]: URL CDP
- localename Nome delle impostazioni locali della CA
- subjecttemplateoids: Modello di oggetto Oid

Torna al [menu](#menu)

## <a name="-cacert"></a>-CA. cert

CertUtil [opzioni]-CA. cert OutCACertFile [Indice]

Recuperare il certificato della CA

OutCACertFile: file di output

Indice Indice di rinnovo del certificato CA (il valore predefinito è quello più recente)

[-f] [-split] [-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-cachain"></a>-CA. Chain

CertUtil [opzioni]-CA. Chain OutCACertChainFile [Indice]

Recuperare la catena di certificati della CA

OutCACertChainFile: file di output

Indice Indice di rinnovo del certificato CA (il valore predefinito è quello più recente)

[-f] [-split] [-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-getcrl"></a>-GetCRL

CertUtil [opzioni]-GetCRL outfile [Indice] [Delta]

Ottieni CRL

Indice Indice CRL o indice chiave (il valore predefinito è CRL per la chiave più recente)

Delta: Delta CRL (il valore predefinito è base CRL)

[-f] [-split] [-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-crl"></a>-CRL

CertUtil [opzioni]-CRL [DD: HH | Ripubblica] [Delta]

Pubblica nuovi CRL [solo Delta CRL]

GG: HH--nuovo periodo di validità CRL in giorni e ore

ripubblicare: ripubblicare i CRL più recenti

Delta: solo Delta CRL (il valore predefinito è CRL di base e Delta)

[-split] [-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-shutdown"></a>-arresta

CertUtil [opzioni]-arresto

Arrestare Active Directory Servizi certificati

[-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-installcert"></a>-installCert

CertUtil [opzioni]-installCert [FileCertificatoCA]

Installare il certificato dell'autorità di certificazione

[-f] [-invisibile all'utente] [-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-renewcert"></a>-renewCert

CertUtil [opzioni]-renewCert [ReuseKeys] [Machine\ParentCAName]

Rinnova certificato dell'autorità di certificazione

Usare-f per ignorare una richiesta di rinnovo in attesa e generare una nuova richiesta.

[-f] [-invisibile all'utente] [-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-schema"></a>-Schema

CertUtil [opzioni]-schema [EXT | Attrib | CRL

Dump dello schema del certificato

Il valore predefinito è tabella richiesta e certificato

EXT Tabella di estensione

Attrib Tabella di attributi

CRL Tabella CRL

[-split] [-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-view"></a>-Visualizza

CertUtil [opzioni]-Visualizza [coda | Log | LogFail | Revocato | EXT | Attrib | CRL] [CSV]

Dump della visualizzazione certificati

Coda: Coda di richieste

Registro: Certificati emessi o revocati, più richieste non riuscite

LogFail: Richieste non riuscite

Revocato Certificati revocati

EXT Tabella di estensione

Attrib Tabella di attributi

CRL Tabella CRL

CSV Output come valori delimitati da virgole

Per visualizzare la colonna StatusCode per tutte le voci:-out StatusCode

Per visualizzare tutte le colonne per l'ultima voce:-Restrict "RequestId = = $"

Per visualizzare RequestId e Disposition per tre richieste:-Restrict "RequestId >\<= 37, RequestId 40"-out "RequestId, Disposition"

Per visualizzare gli ID di riga e i numeri CRL per tutti i CRL di base:-Restrict "CRLMinBase = 0"-out "CRLRowId, CRLNumber" CRL

Per visualizzare il CRL base numero 3:-v-Restrict "CRLMinBase = 0, CRLNumber = 3"-out "CRLRawCRL" CRL

Per visualizzare l'intera tabella CRL: CRL

Usare "date [+ |-DD: HH]" per le restrizioni relative alla data

Usare "Now + DD: HH" per una data relativa all'ora corrente

[-invisibile all'utente] [-split] [-config Machine\CAName] [-restrict restrizione] [-out ColumnName]

Torna al [menu](#menu)

## <a name="-db"></a>-DB

CertUtil [opzioni]-DB

Dump del database non elaborato

[-config Machine\CAName] [-restrict restrizione] [-out ColumnName]

Torna al [menu](#menu)

## <a name="-deleterow"></a>-DeleteRow

CertUtil [opzioni]-DeleteRow RowId | Data [richiesta | Certificato | EXT | Attrib | CRL

Elimina riga database server

Richiesta Richieste non riuscite e in sospeso (data di invio)

Cert: Certificati scaduti e revocati (data di scadenza)

EXT Tabella di estensione

Attrib Tabella di attributi

CRL Tabella CRL (data di scadenza)

Per eliminare le richieste non riuscite e in sospeso inviate dal 22 gennaio 2001: richiesta 1/22/2001

Per eliminare tutti i certificati scaduti entro il 22 gennaio 2001: certificato 1/22/2001

Per eliminare la riga, gli attributi e le estensioni del certificato per RequestId 37: 37

Per eliminare i CRL scaduti entro il 22 gennaio 2001: 1/22/2001 CRL

[-f] [-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-backup"></a>-backup

CertUtil [opzioni]-backup BackupDirectory [incrementale] [KeepLog]

Servizi certificati Active Directory di backup

BackupDirectory: directory per archiviare i dati di cui è stato eseguito il backup

Incrementale: Esegui solo backup incrementale (l'impostazione predefinita è backup completo)

KeepLog: mantiene i file di log del database (il valore predefinito è il troncamento dei file di log)

[-f] [-config Machine\CAName] [-p password]

Torna al [menu](#menu)

## <a name="-backupdb"></a>-backupDB

CertUtil [opzioni]-backupDB BackupDirectory [incrementale] [KeepLog]

Backup Active Directory database di Servizi certificati

BackupDirectory: directory per archiviare i file di database di cui è stato eseguito il backup

Incrementale: Esegui solo backup incrementale (l'impostazione predefinita è backup completo)

KeepLog: mantiene i file di log del database (il valore predefinito è il troncamento dei file di log)

[-f] [-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-backupkey"></a>-backupKey

CertUtil [opzioni]-backupKey BackupDirectory

Backup Active Directory certificato di Servizi certificati e la chiave privata

BackupDirectory: directory per archiviare il file PFX sottoposto a backup

[-f] [-config Machine\CAName] [-p password] [-t timeout]

Torna al [menu](#menu)

## <a name="-restore"></a>-ripristino

CertUtil [opzioni]-Restore BackupDirectory

Ripristinare Servizi certificati Active Directory

BackupDirectory: directory contenente i dati da ripristinare

[-f] [-config Machine\CAName] [-p password]

Torna al [menu](#menu)

## <a name="-restoredb"></a>-restoreDB

CertUtil [opzioni]-restoreDB BackupDirectory

Ripristinare Active Directory database di Servizi certificati

BackupDirectory: directory contenente i file di database da ripristinare

[-f] [-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-restorekey"></a>-restoreKey

CertUtil [opzioni]-restoreKey BackupDirectory | FilePFX

Ripristinare Active Directory certificato di Servizi certificati e la chiave privata

BackupDirectory: directory che contiene il file PFX da ripristinare

FilePFX File PFX da ripristinare

[-f] [-config Machine\CAName] [-p password]

Torna al [menu](#menu)

## <a name="-importpfx"></a>-importPFX

CertUtil [opzioni]-importPFX [NomeArchivioCertificati] FilePFX [modificatori]

Importa certificato e chiave privata

NomeArchivioCertificati Nome dell'archivio certificati.  Vedere [-Store](#-store).

FilePFX File PFX da importare

Modificatori Elenco delimitato da virgole di uno o più degli elementi seguenti:

1. AT_SIGNATURE: Modificare la specifica della scheda in firma
2. AT_KEYEXCHANGE: Modificare la specifica della chiave in scambio di chiave
3. NoExport Rendere la chiave privata non esportabile
4. NoCert: Non importare il certificato
5. NoChain: Non importare la catena di certificati
6. Noroot: Non importare il certificato radice
7. Proteggere Proteggi le chiavi con la password
8. Noprotect: Non proteggere le chiavi da password

Il valore predefinito è Personal Machine Store.

[-f] [-utente] [-p password] [-provider CSP]

Torna al [menu](#menu)

## <a name="-dynamicfilelist"></a>-dynamicfilelist

CertUtil [opzioni]-dynamicfilelist

Visualizza elenco file dinamici

[-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-databaselocations"></a>-databaselocations

CertUtil [opzioni]-databaselocations

Visualizza percorsi database

[-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-hashfile"></a>-hashfile

CertUtil [opzioni]-hashfile infile [HashAlgorithm]

Generare e visualizzare hash crittografico su un file

Torna al [menu](#menu)

## <a name="-store"></a>-Archivio

CertUtil [opzioni]-Store [NomeArchivioCertificati [CertId [OutputFile]]]

Dump archivio certificati

NomeArchivioCertificati Nome dell'archivio certificati. Esempi:

- "My", "CA" (impostazione predefinita), "root",
- "autorità ldap:///CN=Certification, CN = Servizi chiave pubblica, CN = Services, CN = Configuration, DC = cpandl, DC = com? Cacertificate? One? objectClass = certificationAuthority" (Visualizza certificati radice)
- "autorità ldap:///CN=CAName,CN=Certification, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? Cacertificate? base? objectClass = certificationAuthority" (modificare i certificati radice)
- "ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? certificateRevocationList? base? objectClass = cRLDistributionPoint" (View CRL)
- "ldap:///CN=NTAuthCertificates,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? Cacertificate? base? objectClass = certificationAuthority" (certificati della CA globale (Enterprise))
- LDAP (Certificati oggetto computer Active Directory)
- -utente LDAP: (Certificati oggetto utente Active Directory)

CertId Token di corrispondenza del certificato o del CRL.  Può trattarsi di un numero di serie, un certificato SHA-1, un elenco di revoche di certificati, un elenco a discesa CTL o una chiave pubblica, un indice numerico (0, 1 e così via), un indice CRL numerico (. 0, 1 e così via), un indice CTL numerico (.. 0... 1, e così via), una chiave pubblica, una firma o un ObjectId di estensione, un nome comune del soggetto del certificato, un indirizzo di posta elettronica, un nome UPN o DNS, un nome del contenitore di chiavi o un nome CSP, un nome di modello o un ObjectId, un valore EKU o un ObjectId di criteri di applicazione o un nome comune dell'autorità di certificazione CRL. Molti di questi possono comportare più corrispondenze.

OutputFile: file per salvare il certificato corrispondente

Usare-User per accedere a un archivio utenti anziché a un archivio del computer.

Usare-Enterprise per accedere a un archivio aziendale del computer.

Usare-Service per accedere a un archivio del servizio computer.

Usare-GroupPolicy per accedere a un archivio criteri di gruppo del computer.

Esempi:

- -NTAuth Enterprise
- -Radice aziendale 37
- -utente 26e0aaaf000000000004
- CA. 11

[-f] [-Enterprise] [-utente] [-GroupPolicy] [-invisibile all'utente] [-split] [-DC DCName]

Torna al [menu](#menu)

## <a name="-addstore"></a>-addstore

CertUtil [opzioni]-addstore NomeArchivioCertificati infile

Aggiungi certificato all'archivio

NomeArchivioCertificati Nome dell'archivio certificati.  Vedere [-Store](#-store).

InFile Certificato o file CRL da aggiungere all'archivio.

[-f] [-Enterprise] [-utente] [-GroupPolicy] [-DC DCName]

Torna al [menu](#menu)

## <a name="-delstore"></a>-delstore

CertUtil [opzioni]-delstore NomeArchivioCertificati CertId

Elimina certificato dall'archivio

NomeArchivioCertificati Nome dell'archivio certificati.  Vedere [-Store](#-store).

CertId Token di corrispondenza del certificato o del CRL.  Vedere [-Store](#-store).

[-Enterprise] [-utente] [-GroupPolicy] [-DC DCName]

Torna al [menu](#menu)

## <a name="-verifystore"></a>-verifystore

CertUtil [opzioni]-verifystore NomeArchivioCertificati [CertId]

Verificare il certificato nell'archivio

NomeArchivioCertificati Nome dell'archivio certificati.  Vedere [-Store](#-store).

CertId Token di corrispondenza del certificato o del CRL.  Vedere [-Store](#-store).

[-Enterprise] [-utente] [-GroupPolicy] [-invisibile all'utente] [-split] [-DC DCName] [-t timeout]

Torna al [menu](#menu)

## <a name="-repairstore"></a>-repairstore

CertUtil [opzioni]-repairstore NomeArchivioCertificati CertIdList [PropertyInfFile | SDDLSecurityDescriptor]

Ripristinare le proprietà dell'associazione di chiavi o aggiornare il certificato o la chiave di sicurezza

NomeArchivioCertificati Nome dell'archivio certificati.  Vedere [-Store](#-store).

CertIdList: elenco delimitato da virgole di token di certificato o di corrispondenza CRL. Vedere [: Store](#-store) CertId Description.

PropertyInfFile--file INF contenente le proprietà esterne:

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

[-f] [-Enterprise] [-utente] [-GroupPolicy] [-invisibile all'utente] [-split] [-provider CSP]

Torna al [menu](#menu)

## <a name="-viewstore"></a>-viewstore

CertUtil [opzioni]-viewstore [NomeArchivioCertificati [CertId [OutputFile]]]

Dump archivio certificati

NomeArchivioCertificati Nome dell'archivio certificati. Esempi:

- "My", "CA" (impostazione predefinita), "root",
- "autorità ldap:///CN=Certification, CN = Servizi chiave pubblica, CN = Services, CN = Configuration, DC = cpandl, DC = com? Cacertificate? One? objectClass = certificationAuthority" (Visualizza certificati radice)
- "autorità ldap:///CN=CAName,CN=Certification, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? Cacertificate? base? objectClass = certificationAuthority" (modificare i certificati radice)
- "ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? certificateRevocationList? base? objectClass = cRLDistributionPoint" (View CRL)
- "ldap:///CN=NTAuthCertificates,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? Cacertificate? base? objectClass = certificationAuthority" (certificati della CA globale (Enterprise))
- LDAP (Certificati oggetto computer Active Directory)
- -utente LDAP: (Certificati oggetto utente Active Directory)

CertId Token di corrispondenza del certificato o del CRL. Può trattarsi di un numero di serie, un certificato SHA-1, un elenco di revoche di certificati, un elenco a discesa CTL o una chiave pubblica, un indice numerico (0, 1 e così via), un indice CRL numerico (. 0, 1 e così via), un indice CTL numerico (.. 0... 1, e così via), una chiave pubblica, una firma o un ObjectId di estensione, un nome comune del soggetto del certificato, un indirizzo di posta elettronica, un nome UPN o DNS, un nome del contenitore di chiavi o un nome CSP, un nome di modello o un ObjectId, un valore EKU o un ObjectId di criteri di applicazione o un nome comune dell'autorità di certificazione CRL. Molti di questi possono comportare più corrispondenze.

OutputFile: file per salvare il certificato corrispondente

Usare-User per accedere a un archivio utenti anziché a un archivio del computer.

Usare-Enterprise per accedere a un archivio aziendale del computer.

Usare-Service per accedere a un archivio del servizio computer.

Usare-GroupPolicy per accedere a un archivio criteri di gruppo del computer.

Esempi:

1. -NTAuth Enterprise
2. -Radice aziendale 37
3. -utente 26e0aaaf000000000004
4. CA. 11

[-f] [-Enterprise] [-utente] [-GroupPolicy] [-DC DCName]

Torna al [menu](#menu)

## <a name="-viewdelstore"></a>-viewdelstore

CertUtil [opzioni]-viewdelstore [NomeArchivioCertificati [CertId [OutputFile]]]

Elimina certificato dall'archivio

NomeArchivioCertificati Nome dell'archivio certificati. Esempi:

- "My", "CA" (impostazione predefinita), "root",
- "autorità ldap:///CN=Certification, CN = Servizi chiave pubblica, CN = Services, CN = Configuration, DC = cpandl, DC = com? Cacertificate? One? objectClass = certificationAuthority" (Visualizza certificati radice)
- "autorità ldap:///CN=CAName,CN=Certification, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? Cacertificate? base? objectClass = certificationAuthority" (modificare i certificati radice)
- "ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? certificateRevocationList? base? objectClass = cRLDistributionPoint" (View CRL)
- "ldap:///CN=NTAuthCertificates,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? Cacertificate? base? objectClass = certificationAuthority" (certificati della CA globale (Enterprise))
- LDAP (Certificati oggetto computer Active Directory)
- -utente LDAP: (Certificati oggetto utente Active Directory)

CertId Token di corrispondenza del certificato o del CRL. Può trattarsi di un numero di serie, un certificato SHA-1, un elenco di revoche di certificati, un elenco a discesa CTL o una chiave pubblica, un indice numerico (0, 1 e così via), un indice CRL numerico (. 0, 1 e così via), un indice CTL numerico (.. 0... 1, e così via), una chiave pubblica, una firma o un ObjectId di estensione, un nome comune del soggetto del certificato, un indirizzo di posta elettronica, un nome UPN o DNS, un nome del contenitore di chiavi o un nome CSP, un nome di modello o un ObjectId, un valore EKU o un ObjectId di criteri di applicazione o un nome comune dell'autorità di certificazione CRL. Molti di questi possono comportare più corrispondenze.

OutputFile: file per salvare il certificato corrispondente

Usare-User per accedere a un archivio utenti anziché a un archivio del computer.

Usare-Enterprise per accedere a un archivio aziendale del computer.

Usare-Service per accedere a un archivio del servizio computer.

Usare-GroupPolicy per accedere a un archivio criteri di gruppo del computer.

Esempi:

1. -NTAuth Enterprise
2. -Radice aziendale 37
3. -utente 26e0aaaf000000000004
4. CA. 11

[-f] [-Enterprise] [-utente] [-GroupPolicy] [-DC DCName]

Torna al [menu](#menu)

## <a name="-dspublish"></a>-dsPublish

CertUtil [opzioni]-dsPublish CertFile [NTAuthCA | RootCA | SubCA | CrossCA | KRA | Utente | Macchina

CertUtil [opzioni]-dsPublish FileCRL [DSCDPContainer [DSCDPCN]]

Pubblicare un certificato o un CRL per Active Directory

CertFile: file di certificato da pubblicare

NTAuthCA: Pubblica il certificato in DS Enterprise Store

RootCA Pubblicare il certificato nell'archivio radice attendibile DS

SubCA Pubblica certificato CA nell'oggetto CA DS

CrossCA: Pubblica il certificato incrociato nell'oggetto CA DS

KRA Oggetto Publish certificate to DS Key Recovery Agent

Utente: Pubblica certificato nell'oggetto DS utente

Computer: Pubblicare un certificato in un oggetto Machine DS

FileCRL File CRL da pubblicare

DSCDPContainer: DS CDP container CN, in genere il nome del computer CA

DSCDPCN: Oggetto CDP DS, in genere basato sul nome breve e sull'indice della chiave della CA purificata

Usare-f per creare un oggetto DS.

[-f] [-utente] [-DC DCName]

Torna al [menu](#menu)

## <a name="-adtemplate"></a>-ADTemplate

CertUtil [opzioni]-ADTemplate [modello]

Visualizzare i modelli AD

[-f] [-utente] [-ut] [-MT] [-DC DCName]

## <a name="-template"></a>-Modello

CertUtil [opzioni]-modello [modello]

Visualizzare i modelli di criteri di registrazione

[-f] [-utente] [-invisibile all'utente] [-PolicyServer URLOrId] [-Anonimo] [-Kerberos] [-ClientCertificate ClientCertId] [-Nomeutente nomeutente] [-p password]

Torna al [menu](#menu)

## <a name="-templatecas"></a>-TemplateCAs

CertUtil [opzioni]-modello TemplateCAs

Visualizzare le autorità di certificazione per il modello

[-f] [-utente] [-DC DCName]

Torna al [menu](#menu)

## <a name="-catemplates"></a>-Catemplates

CertUtil [opzioni]-catemplates [modello]

Visualizza modelli per CA

[-f] [-utente] [-ut] [-MT] [-config Machine\CAName] [-DC DCName]

Torna al [menu](#menu)

## <a name="-setcasites"></a>-SetCASites

CertUtil [opzioni]-SetCASites [set] [SiteName]

CertUtil [opzioni]-SetCASites verify [SiteName]

CertUtil [opzioni]-SetCASites Delete

Impostare, verificare o eliminare i nomi dei siti CA

- Usare l'opzione-config per fare riferimento a una singola CA (il valore predefinito è tutte le autorità di certificazione)
- Il nome *sito è consentito* solo quando la destinazione è un'unica CA
- Usare-f per eseguire l'override degli errori di convalida per il nome *sito specificato*
- Usare-f per eliminare tutti i nomi dei siti CA

[-f] [-config Machine\CAName] [-DC DCName]

> [!NOTE]
> Per ulteriori informazioni sulla configurazione di autorità di certificazione per il riconoscimento dei siti Active Directory Domain Services (AD DS), vedere [riconoscimento del sito di servizi di dominio Active Directory per i client ad CS e PKI](https://social.technet.microsoft.com/wiki/contents/articles/14106.ad-ds-site-awareness-for-ad-cs-and-pki-clients.aspx).

Torna al [menu](#menu)

## <a name="-enrollmentserverurl"></a>-enrollmentServerURL

CertUtil [opzioni]-enrollmentServerURL [URL AuthenticationType [priorità] [modificatori]]

CertUtil [opzioni]-eliminazione URL enrollmentServerURL

Visualizzare, aggiungere o eliminare gli URL del server di registrazione associati a una CA

AuthenticationType Specificare uno dei seguenti metodi di autenticazione client durante l'aggiunta di un URL

1. Kerberos Usa credenziali SSL Kerberos
2. Nome utente Usa account denominato per le credenziali SSL
3. ClientCertificate Usare le credenziali SSL del certificato X. 509
4. Anonymous: Usa credenziali SSL anonime

Delete: Elimina l'URL specificato associato alla CA

Priority: il valore predefinito è' 1' se non è specificato durante l'aggiunta di un URL

Modificatori: elenco delimitato da virgole di uno o più degli elementi seguenti:

1. AllowRenewalsOnly: Solo le richieste di rinnovo possono essere inviate a questa CA tramite questo URL
2. AllowKeyBasedRenewal: Consente l'uso di un certificato a cui non è associato alcun account in Active Directory. Si applica solo alla modalità ClientCertificate e AllowRenewalsOnly

[-config Machine\CAName] [-DC DCName]

Torna al [menu](#menu)

## <a name="-adca"></a>-ADCA

CertUtil [opzioni]-ADCA [nome file]

Visualizza CA AD

[-f] [-split] [-DC DCName]

Torna al [menu](#menu)

## <a name="-ca"></a>-CA

CertUtil [opzioni]-CA [nome file | TemplateName

Visualizzare le autorità di certificazione per i criteri di registrazione

[-f] [-utente] [-invisibile all'utente] [-split] [-PolicyServer URLOrId] [-Anonimo] [-Kerberos] [-ClientCertificate ClientCertId] [-Nomeutente nomeutente] [-p password]

Torna al [menu](#menu)

## <a name="-policy"></a>-Criterio

Visualizzare i criteri di registrazione

[-f] [-utente] [-invisibile all'utente] [-split] [-PolicyServer URLOrId] [-Anonimo] [-Kerberos] [-ClientCertificate ClientCertId] [-Nomeutente nomeutente] [-p password]

Torna al [menu](#menu)

## <a name="-policycache"></a>-PolicyCache

CertUtil [opzioni]-PolicyCache [Delete]

Visualizzare o eliminare le voci della cache dei criteri di registrazione

Elimina: Elimina voci della cache del server dei criteri

-f: usare-f per eliminare tutte le voci della cache

[-f] [-utente] [-PolicyServer URLOrId]

Torna al [menu](#menu)

## <a name="-credstore"></a>-CredStore

CertUtil [opzioni]-CredStore [URL]

CertUtil [opzioni]-Aggiungi URL CredStore

CertUtil [opzioni]-eliminazione URL CredStore

Visualizzare, aggiungere o eliminare le voci dell'archivio credenziali

URL: URL di destinazione.  Usare \* per trovare la corrispondenza con tutte le voci. Usare https://machine\ * per trovare la corrispondenza con un prefisso URL.

Aggiungi: aggiungere una voce dell'archivio credenziali. È necessario specificare anche le credenziali SSL.

Elimina: Elimina voci dell'archivio credenziali

-f: usare-f per sovrascrivere una voce o per eliminare più voci.

[-f] [-utente] [-invisibile all'utente] [-Anonimo] [-Kerberos] [-ClientCertificate ClientCertId] [-Nomeutente nomeutente] [-p password]

Torna al [menu](#menu)

## <a name="-installdefaulttemplates"></a>-InstallDefaultTemplates

CertUtil [opzioni]-InstallDefaultTemplates

Installare i modelli di certificato predefiniti

[-DC DCName]

Torna al [menu](#menu)

## <a name="-urlcache"></a>-URLCache

CertUtil [opzioni]-URLCache [URL | CRL | \* [Delete]]

Visualizzare o eliminare le voci della cache URL

URL: URL memorizzato nella cache

CRL: operare su tutti gli URL CRL memorizzati nella cache

\*: opera su tutti gli URL memorizzati nella cache

Elimina: Elimina gli URL pertinenti dalla cache locale dell'utente corrente

Usare-f per forzare il recupero di un URL specifico e l'aggiornamento della cache.

[-f] [-split]

Torna al [menu](#menu)

## <a name="-pulse"></a>-impulso

CertUtil [opzioni]-Pulse

Eventi di registrazione automatica impulsi

[-utente]

Torna al [menu](#menu)

## <a name="-machineinfo"></a>-MachineInfo

CertUtil [opzioni]-MachineInfo DomainName\MachineName $

Visualizza informazioni Active Directory oggetto computer

Torna al [menu](#menu)

## <a name="-dcinfo"></a>-DCInfo

CertUtil [opzioni]-DCInfo [dominio] [verify | DeleteBad | DeleteAll

Visualizzare le informazioni sul controller di dominio

Il valore predefinito prevede la visualizzazione di certificati DC senza verifica

[-f] [-utente] [-UrlFetch] [-DC DCName] [-t timeout]

> [!TIP]
> La possibilità di specificare un dominio di Active Directory Domain Services (AD DS) **[dominio]** e di specificare un controller di dominio ( **-DC**) è stato aggiunto in Windows Server 2012. Per eseguire correttamente il comando, è necessario usare un account membro di **Domain Admins** o **Enterprise Admins**. Di seguito sono riportate le modifiche del comportamento di questo comando:</br>> 1.  Se non si specifica un dominio e non viene specificato un controller di dominio specifico, questa opzione restituisce un elenco di controller di dominio da elaborare dal controller di dominio predefinito.</br>> 2.  Se non si specifica un dominio, ma viene specificato un controller di dominio, viene generato un report dei certificati sul controller di dominio specificato.</br>> 3.  Se si specifica un dominio, ma non si specifica un controller di dominio, viene generato un elenco di controller di dominio insieme ai report sui certificati per ogni controller di dominio nell'elenco.</br>> 4.  Se vengono specificati il dominio e il controller di dominio, viene generato un elenco di controller di dominio dal controller di dominio di destinazione. Viene generato anche un report dei certificati per ogni controller di dominio nell'elenco.

Si supponga, ad esempio, che esista un dominio denominato CPANDL con un controller di dominio denominato CPANDL-DC1. È possibile eseguire il comando seguente per recuperare un elenco di controller di dominio e i relativi certificati da CPANDL-DC1: certutil-DC cpandl-DC1-dcinfo cpandl

Torna al [menu](#menu)

## <a name="-entinfo"></a>-EntInfo

CertUtil [opzioni]-EntInfo DomainName\MachineName $

[-f] [-utente]

Torna al [menu](#menu)

## <a name="-tcainfo"></a>-TCAInfo

CertUtil [opzioni]-TCAInfo [DomainDN |-]

Visualizza informazioni CA

[-f] [-Enterprise] [-utente] [-UrlFetch] [-DC DCName] [-t timeout]

Torna al [menu](#menu)

## <a name="-scinfo"></a>-SCInfo

CertUtil [opzioni]-SCInfo [Readername [CRYPT_DELETEKEYSET]]

Visualizza informazioni smart card

CRYPT_DELETEKEYSET: Elimina tutte le chiavi della smart card

[-invisibile all'utente] [-split] [-UrlFetch] [-t timeout]

Torna al [menu](#menu)

## <a name="-scroots"></a>-SCRoots

CertUtil [opzioni]-aggiornamento SCRoots [+] [InputRootFile] [Readername]

CertUtil [opzioni]-SCRoots Save \@OutputRootFile [Readername]

CertUtil [opzioni]-Visualizzazione SCRoots [InputRootFile | Readername]

CertUtil [opzioni]-SCRoots delete [Readername]

Gestire i certificati radice della smart card

[-f] [-split] [-p password]

Torna al [menu](#menu)

## <a name="-verifykeys"></a>-verifykeys

CertUtil [opzioni]-verifykeys [FileCertificatoCA di contenitori]

Verificare il set di chiavi pubbliche/private

Chiave ContainerName: nome del contenitore di chiavi da verificare. Il valore predefinito è chiavi computer.  Use-User per le chiavi utente.

FileCertificatoCA: file di certificato di firma o di crittografia

Se non viene specificato alcun argomento, ogni certificato CA di firma viene verificato rispetto alla relativa chiave privata.

Questa operazione può essere eseguita solo su una CA locale o su chiavi locali.

[-f] [-utente] [-invisibile all'utente] [-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-verify"></a>-Verifica

CertUtil [opzioni]-Verify CertFile [ApplicationPolicyList |-[IssuancePolicyList]]

CertUtil [opzioni]-Verify CertFile [FileCertificatoCA [CrossedCACertFile]]

CertUtil [opzioni]-Verify FileCRL FileCertificatoCA [IssuedCertFile]

CertUtil [opzioni]-Verify FileCRL FileCertificatoCA [DeltaCRLFile]

Verificare il certificato, il CRL o la catena

CertFile Certificato da verificare

ApplicationPolicyList: elenco facoltativo delimitato da virgole dei criteri dell'applicazione richiesti ObjectId

IssuancePolicyList: elenco facoltativo delimitato da virgole dei criteri di rilascio richiesti ObjectId

FileCertificatoCA: certificato CA emittente facoltativo per la verifica

CrossedCACertFile: certificato facoltativo con certificazione incrociata da CertFile

FileCRL CRL da verificare

IssuedCertFile: certificato emesso facoltativo coperto da FileCRL

DeltaCRLFile: Delta CRL facoltativo

Se si specifica ApplicationPolicyList, la creazione della catena è limitata alle catene valide per i criteri dell'applicazione specificati.

Se si specifica IssuancePolicyList, la creazione della catena è limitata alle catene valide per i criteri di rilascio specificati.

Se viene specificato FileCertificatoCA, i campi in FileCertificatoCA vengono verificati in base a CertFile o FileCRL.

Se FileCertificatoCA non è specificato, viene usato CertFile per compilare e verificare una catena completa.

Se FileCertificatoCA e CrossedCACertFile sono entrambi specificati, i campi in FileCertificatoCA e CrossedCACertFile vengono verificati in base a CertFile.

Se viene specificato IssuedCertFile, i campi in IssuedCertFile vengono verificati rispetto a FileCRL.

Se viene specificato DeltaCRLFile, i campi in DeltaCRLFile vengono verificati rispetto a FileCRL.

[-f] [-Enterprise] [-utente] [-invisibile all'utente] [-split] [-UrlFetch] [-t timeout]

Torna al [menu](#menu)

## <a name="-verifyctl"></a>-verifyCTL

CertUtil [opzioni]-verifyCTL CTLObject [CertDir] [CertFile]

Verificare AuthRoot o i certificati non consentiti CTL

CTLObject: Identifica l'elenco di scopi consentiti da verificare:

- AuthRootWU: legge AuthRoot CAB e i certificati corrispondenti dalla cache degli URL. Usare-f per scaricare invece dal Windows Update.
- DisallowedWU: leggere i certificati non consentiti CAB e il file dell'archivio certificati non consentito dalla cache degli URL.  Usare-f per scaricare invece dal Windows Update.
- AuthRoot: lettura del registro di sistema AuthRoot CTL.  Usare con-f e un CertFile non ancora attendibile per forzare l'aggiornamento del registro di sistema AuthRoot e scopi consentiti del certificato non consentito.
- Non consentito: lettura del registro di sistema per i certificati non consentiti CTL. -f ha lo stesso comportamento di con AuthRoot.
- CTLFileName: file o http: percorso di CTL o CAB

CertDir: cartella contenente i certificati corrispondenti alle voci CTL. Un percorso http: Folder deve terminare con un separatore di percorso. Se una cartella non è specificata con AuthRoot o non è consentita, verranno cercati più percorsi per i certificati corrispondenti: archivi certificati locali, risorse crypt32. dll e cache URL locale. Usare-f per scaricare da Windows Update quando necessario. In caso contrario, il valore predefinito è la stessa cartella o sito Web del CTLObject.

CertFile: file contenente i certificati da verificare. I certificati verranno confrontati con le voci CTL e i risultati delle corrispondenze verranno visualizzati. Evita la maggior parte dell'output predefinito.

[-f] [-utente] [-split]

Torna al [menu](#menu)

## <a name="-sign"></a>-firma

CertUtil [opzioni]-segno di infiler | SerialNumber | Elenco di file CRL [StartDate + GG: HH] [+ SerialNumberList |-SerialNumberList |-ObjectIdList | \@ExtensionFile]

CertUtil [opzioni]-segno di infiler | SerialNumber | Elenco di file CRL [#HashAlgorithm] [+ AlternateSignatureAlgorithm |-AlternateSignatureAlgorithm]

Firma di nuovo CRL o certificato

Filelist: elenco delimitato da virgole di file di certificato o CRL da modificare e firmare di nuovo

SerialNumber Numero di serie del certificato da creare. Il periodo di validità e le altre opzioni non devono essere presenti.

CRL Creare un CRL vuoto. Il periodo di validità e le altre opzioni non devono essere presenti.

Outlisting: elenco delimitato da virgole di file di output di certificati o CRL modificati. Il numero di file deve corrispondere a un file con estensione.

StartDate + GG: HH: nuovo periodo di validità: data facoltativa più; periodo di validità di giorni e ore facoltativo; Se vengono specificati entrambi, usare un separatore di segno più (+). Usare "Now [+ GG: HH]" per iniziare dall'ora corrente. Usare "mai" per non avere una data di scadenza (solo per i CRL).

SerialNumberList: elenco di numeri di serie separati da virgole da aggiungere o rimuovere

ObjectIdList: elenco ObjectId con estensione delimitato da virgole da rimuovere

\@ExtensionFile: File INF contenente le estensioni da aggiornare o rimuovere:

```
[Extensions]
     2.5.29.31 = ; Remove CRL Distribution Points extension
     2.5.29.15 = "{hex}" ; Update Key Usage extension
     _continue_="03 02 01 86"
```

HashAlgorithm Nome dell'algoritmo hash preceduto da un segno #

AlternateSignatureAlgorithm: identificatore dell'algoritmo di firma alternativo

Un segno meno causa la rimozione di numeri di serie ed estensioni. Un segno più comporta l'aggiunta di numeri di serie a un CRL. Quando si rimuovono gli elementi da un CRL, l'elenco può contenere sia i numeri di serie che ObjectId. Un segno meno prima di AlternateSignatureAlgorithm causa l'utilizzo del formato di firma legacy. Un segno più prima di AlternateSignatureAlgorithm causa l'uso del formato della firma alternature. Se AlternateSignatureAlgorithm non è specificato, viene usato il formato della firma nel certificato o nel CRL.

[-nullsign] [-f] [-invisibile all'utente] [-Cert CertId]

Torna al [menu](#menu)

## <a name="-vroot"></a>-vroot

CertUtil [opzioni]-vroot [Delete]

Creare/eliminare radici virtuali Web e condivisioni file

Torna al [menu](#menu)

## <a name="-vocsproot"></a>-vocsproot

CertUtil [opzioni]-vocsproot [Delete]

Creare/eliminare radici virtuali Web per il proxy Web OCSP

Torna al [menu](#menu)

## <a name="-addenrollmentserver"></a>-addEnrollmentServer

CertUtil [opzioni]-addEnrollmentServer Kerberos | Nome utente | ClientCertificate [AllowRenewalsOnly] [AllowKeyBasedRenewal]

Aggiungere un'applicazione del server di iscrizione

Aggiungere un'applicazione del server di iscrizione e un pool di applicazioni, se necessario, per la CA specificata. Questo comando non installa i file binari o i pacchetti. Uno dei seguenti metodi di autenticazione con cui il client si connette a un server di registrazione certificati.

- Kerberos Usa credenziali SSL Kerberos
- Nome utente Usa account denominato per le credenziali SSL
- ClientCertificate Usare le credenziali SSL del certificato X. 509
- AllowRenewalsOnly: Solo le richieste di rinnovo possono essere inviate a questa CA tramite questo URL
- AllowKeyBasedRenewal: consente l'uso di un certificato a cui non è associato alcun account in Active Directory. Si applica solo alla modalità ClientCertificate e AllowRenewalsOnly.

[-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-deleteenrollmentserver"></a>-deleteEnrollmentServer

CertUtil [opzioni]-deleteEnrollmentServer Kerberos | Nome utente | ClientCertificate

Eliminare un'applicazione del server di iscrizione

Eliminare un'applicazione del server di iscrizione e un pool di applicazioni, se necessario, per la CA specificata. Questo comando non rimuove i file binari o i pacchetti. Uno dei seguenti metodi di autenticazione con cui il client si connette a un server di registrazione certificati.

1. Kerberos Usa credenziali SSL Kerberos
2. Nome utente Usa account denominato per le credenziali SSL
3. ClientCertificate Usare le credenziali SSL del certificato X. 509

[-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-addpolicyserver"></a>-addPolicyServer

CertUtil [opzioni]-addPolicyServer Kerberos | Nome utente | ClientCertificate [KeyBasedRenewal]

Aggiungere un'applicazione server dei criteri

Aggiungere un'applicazione server dei criteri e un pool di applicazioni, se necessario. Questo comando non installa i file binari o i pacchetti. Uno dei seguenti metodi di autenticazione con cui il client si connette a un server dei criteri di certificazione:

- Kerberos Usa credenziali SSL Kerberos
- Nome utente Usa account denominato per le credenziali SSL
- ClientCertificate Usare le credenziali SSL del certificato X. 509
- KeyBasedRenewal: Solo i criteri che contengono modelli KeyBasedRenewal vengono restituiti al client. Questo flag è valido solo per il nome utente e l'autenticazione ClientCertificate.

Torna al [menu](#menu)

## <a name="-deletepolicyserver"></a>-deletePolicyServer

CertUtil [opzioni]-deletePolicyServer Kerberos | Nome utente | ClientCertificate [KeyBasedRenewal]

Eliminare un'applicazione server dei criteri

Eliminare un'applicazione server dei criteri e un pool di applicazioni, se necessario. Questo comando non rimuove i file binari o i pacchetti. Uno dei seguenti metodi di autenticazione con cui il client si connette a un server dei criteri di certificazione:

1. Kerberos Usa credenziali SSL Kerberos
2. Nome utente Usa account denominato per le credenziali SSL
3. ClientCertificate Usare le credenziali SSL del certificato X. 509
4. KeyBasedRenewal: Server dei criteri di KeyBasedRenewal

Torna al [menu](#menu)

## <a name="-oid"></a>-OID

CertUtil [opzioni]-OID ObjectId [DisplayName | delete [LanguageId [tipo]]]

CertUtil [opzioni]-OID GroupId

CertUtil [opzioni]-OID algido | AlgorithmName [GroupId]

Visualizza ObjectId o imposta il nome visualizzato

- ObjectId--ObjectId da visualizzare o per aggiungere il nome visualizzato
- GroupId--numero GroupId decimale per ObjectId da enumerare
- Algido--algido esadecimale per la ricerca di ObjectId
- AlgorithmName--nome algoritmo per ObjectId da ricercare
- DisplayName--nome visualizzato da archiviare in DS
- Elimina--Elimina nome visualizzato
- LanguageId--ID lingua (l'impostazione predefinita è Current: 1033)
- Tipo di oggetto DS da creare: 1 per il modello (impostazione predefinita), 2 per i criteri di rilascio, 3 per i criteri di applicazione
- Usare-f per creare un oggetto DS.

[-f]

Torna al [menu](#menu)

## <a name="-error"></a>-errore

CertUtil [opzioni]-errore ErrorCode

Visualizza il testo del messaggio del codice di errore

Torna al [menu](#menu)

## <a name="-getreg"></a>-getreg

CertUtil [opzioni]-getreg [{CA | Restore | Policy | Exit | template | registra | Chain | PolicyServers} \[ProgId @ no__t-1] [RegistryValueName]

Visualizza valore del registro di sistema

CA Usa la chiave del registro di sistema della CA

ripristinare Usa la chiave di ripristino del registro di sistema della CA

politica Usa la chiave del registro di sistema del modulo criteri

uscita Usa la chiave del registro di sistema del primo modulo di uscita

modello Usare la chiave del registro di sistema template (Use-User per i modelli utente)

iscriversi Usare la chiave del registro di sistema di registrazione (Use-User per il contesto utente)

catena Usare la chiave del registro di sistema Chain Configuration

PolicyServers: Usare la chiave del registro di sistema server dei criteri

ProgID Usa il ProgId del modulo criteri o Esci (nome della sottochiave del registro di sistema)

RegistryValueName: nome del valore del registro di sistema\*(usare "Name" per la corrispondenza con prefisso)

Valore: nuovo valore del registro di sistema numeric, String o date o filename. Se un valore numerico inizia con "+" o "-", i bit specificati nel nuovo valore vengono impostati o cancellati nel valore del registro di sistema esistente.

Se un valore stringa inizia con "+" o "-" e il valore esistente è un valore REG_MULTI_SZ, la stringa viene aggiunta o rimossa dal valore del registro di sistema esistente. Per forzare la creazione di un valore REG_MULTI_SZ, aggiungere "\n" alla fine del valore stringa.

Se il valore inizia con "\@", il resto del valore è il nome del file che contiene la rappresentazione in formato testo esadecimale di un valore binario. Se non fa riferimento a un file valido, viene invece analizzato come [DATE] [+ |-] [DD: HH]--una data facoltativa più o meno giorni e ore facoltativi. Se vengono specificati entrambi, usare un separatore di segno più (+) o meno (-). Usare "Now + DD: HH" per una data relativa all'ora corrente.

Usare "chain\ChainCacheResyncFiletime \@Now" per scaricare effettivamente i CRL memorizzati nella cache.

[-f] [-utente] [-GroupPolicy] [-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-setreg"></a>-setreg

CertUtil [opzioni]-setreg [{CA | Restore | Policy | Exit | template | registra | Chain | PolicyServers} \[ProgId @ no__t-1] Valore RegistryValueName

Imposta valore del registro di sistema

CA Usa la chiave del registro di sistema della CA

ripristinare Usa la chiave di ripristino del registro di sistema della CA

politica Usa la chiave del registro di sistema del modulo criteri

uscita Usa la chiave del registro di sistema del primo modulo di uscita

modello Usare la chiave del registro di sistema template (Use-User per i modelli utente)

iscriversi Usare la chiave del registro di sistema di registrazione (Use-User per il contesto utente)

catena Usare la chiave del registro di sistema Chain Configuration

PolicyServers: Usare la chiave del registro di sistema server dei criteri

ProgID Usa il ProgId del modulo criteri o Esci (nome della sottochiave del registro di sistema)

RegistryValueName: nome del valore del registro di sistema\*(usare "Name" per la corrispondenza con prefisso)

Valore: nuovo valore del registro di sistema numeric, String o date o filename. Se un valore numerico inizia con "+" o "-", i bit specificati nel nuovo valore vengono impostati o cancellati nel valore del registro di sistema esistente.

Se un valore stringa inizia con "+" o "-" e il valore esistente è un valore REG_MULTI_SZ, la stringa viene aggiunta o rimossa dal valore del registro di sistema esistente. Per forzare la creazione di un valore REG_MULTI_SZ, aggiungere "\n" alla fine del valore stringa.

Se il valore inizia con "\@", il resto del valore è il nome del file che contiene la rappresentazione in formato testo esadecimale di un valore binario. Se non fa riferimento a un file valido, viene invece analizzato come [DATE] [+ |-] [DD: HH]--una data facoltativa più o meno giorni e ore facoltativi. Se vengono specificati entrambi, usare un separatore di segno più (+) o meno (-). Usare "Now + DD: HH" per una data relativa all'ora corrente.

Usare "chain\ChainCacheResyncFiletime \@Now" per scaricare effettivamente i CRL memorizzati nella cache.

[-f] [-utente] [-GroupPolicy] [-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-delreg"></a>-delreg

CertUtil [opzioni]-DelReg [{CA | Restore | Policy | Exit | template | registra | Chain | PolicyServers} \[ProgId @ no__t-1] [RegistryValueName]

Elimina valore del registro di sistema

CA Usa la chiave del registro di sistema della CA

ripristinare Usa la chiave di ripristino del registro di sistema della CA

politica Usa la chiave del registro di sistema del modulo criteri

uscita Usa la chiave del registro di sistema del primo modulo di uscita

modello Usare la chiave del registro di sistema template (Use-User per i modelli utente)

iscriversi Usare la chiave del registro di sistema di registrazione (Use-User per il contesto utente)

catena Usare la chiave del registro di sistema Chain Configuration

PolicyServers: Usare la chiave del registro di sistema server dei criteri

ProgID Usa il ProgId del modulo criteri o Esci (nome della sottochiave del registro di sistema)

RegistryValueName: nome del valore del registro di sistema\*(usare "Name" per la corrispondenza con prefisso)

Valore: nuovo valore del registro di sistema numeric, String o date o filename. Se un valore numerico inizia con "+" o "-", i bit specificati nel nuovo valore vengono impostati o cancellati nel valore del registro di sistema esistente.

Se un valore stringa inizia con "+" o "-" e il valore esistente è un valore REG_MULTI_SZ, la stringa viene aggiunta o rimossa dal valore del registro di sistema esistente. Per forzare la creazione di un valore REG_MULTI_SZ, aggiungere "\n" alla fine del valore stringa.

Se il valore inizia con "\@", il resto del valore è il nome del file che contiene la rappresentazione in formato testo esadecimale di un valore binario. Se non fa riferimento a un file valido, viene invece analizzato come [DATE] [+ |-] [DD: HH]--una data facoltativa più o meno giorni e ore facoltativi. Se vengono specificati entrambi, usare un separatore di segno più (+) o meno (-). Usare "Now + DD: HH" per una data relativa all'ora corrente.

Usare "chain\ChainCacheResyncFiletime \@Now" per scaricare effettivamente i CRL memorizzati nella cache.

[-f] [-utente] [-GroupPolicy] [-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-importkms"></a>-ImportKMS

CertUtil [opzioni]-ImportKMS UserKeyAndCertFile [CertId]

Importare chiavi utente e certificati nel database del server per l'archiviazione delle chiavi

UserKeyAndCertFile: file di dati contenente le chiavi private utente e i certificati da archiviare.  Può essere uno dei seguenti:

- File di esportazione del server di gestione delle chiavi (KMS) di Exchange
- File PFX

CertId Token di corrispondenza del certificato di decrittografia del file di esportazione KMS.  Vedere [-Store](#-store).

Usare-f per importare i certificati non rilasciati dalla CA.

[-f] [-invisibile all'utente] [-split] [-config Machine\CAName] [-p password] [-symkeyalg SymmetricKeyAlgorithm [, lunghezza del tempo]]

Torna al [menu](#menu)

## <a name="-importcert"></a>-ImportCert

CertUtil [opzioni]-ImportCert CertFile [ExistingRow]

Importare un file di certificato nel database

Usare ExistingRow per importare il certificato al posto di una richiesta in sospeso per la stessa chiave.

Usare-f per importare i certificati non rilasciati dalla CA.

Potrebbe inoltre essere necessario configurare la CA per supportare l'importazione di certificati esterni: certutil-setreg ca\KRAFlags + KRAF_ENABLEFOREIGN

[-f] [-config Machine\CAName]

Torna al [menu](#menu)

## <a name="-getkey"></a>-GetKey

CertUtil [opzioni]-GetKey SearchToken [RecoveryBlobOutFile]

CertUtil [Options]-GetKey SearchToken script OutputScriptFile

CertUtil [Options]-GetKey SearchToken Retrieve | ripristino OutputFileBaseName

Recuperare il BLOB di recupero della chiave privata archiviata, generare uno script di ripristino o recuperare le chiavi archiviate

script: genera uno script per recuperare e recuperare le chiavi (comportamento predefinito se vengono trovati più candidati di recupero corrispondenti o se il file di output non è specificato).

Recupera: recupera uno o più BLOB di recupero chiave (comportamento predefinito se viene trovato esattamente un candidato di recupero corrispondente e se viene specificato il file di output)

ripristino: recuperare e recuperare le chiavi private in un unico passaggio (richiede certificati dell'agente di recupero chiavi e chiavi private)

SearchToken: Consente di selezionare le chiavi e i certificati da ripristinare.

Può essere uno dei seguenti:

1. Nome comune del certificato
2. Numero di serie del certificato
3. Hash SHA-1 certificato (identificazione personale)
4. Hash SHA-1 del certificato KeyId (identificatore della chiave del soggetto)
5. Nome del richiedente (dominio\utente)
6. UPN (dominio\@utente)

RecoveryBlobOutFile: file di output contenente una catena di certificati e una chiave privata associata, ancora crittografati in uno o più certificati dell'agente di recupero chiavi.

OutputScriptFile: file di output contenente uno script batch per recuperare e recuperare le chiavi private.

OutputFileBaseName: nome di base del file di output. Per il recupero, qualsiasi estensione viene troncata e vengono accodate una stringa specifica del certificato e l'estensione REC per ogni BLOB di recupero della chiave.  Ogni file contiene una catena di certificati e una chiave privata associata, ancora crittografati a uno o più certificati dell'agente di recupero chiavi. Per il ripristino, qualsiasi estensione viene troncata e viene aggiunta l'estensione. P12.  Contiene le catene di certificati ripristinati e le chiavi private associate, archiviate come file PFX.

[-f] [-UnicodeText] [-invisibile all'utente] [-config Machine\CAName] [-p password] [-ProtectTo SAMNameAndSIDList] [-provider CSP]

Torna al [menu](#menu)

## <a name="-recoverkey"></a>-RecoverKey

CertUtil [opzioni]-RecoverKey RecoveryBlobInFile [PFXOutFile [RecipientIndex]]

Ripristinare la chiave privata archiviata

[-f] [-utente] [-invisibile all'utente] [-split] [-p password] [-ProtectTo SAMNameAndSIDList] [-provider CSP] [-t timeout]

Torna al [menu](#menu)

## <a name="-mergepfx"></a>-MergePFX

CertUtil [opzioni]-MergePFX ElencoFileInPFX PFXOutFile [ExtendedProperties]

ElencoFileInPFX Elenco di file di input PFX separati da virgole

PFXOutFile: File di output PFX

ExtendedProperties Includi proprietà estese

La password specificata nella riga di comando è un elenco di password separate da virgole.  Se viene specificata più di una password, per il file di output viene utilizzata l'ultima password.  Se viene fornita una sola password o se l'ultima password è "\*", all'utente verrà richiesto di immettere la password del file di output.

[-f] [-utente] [-split] [-p password] [-ProtectTo SAMNameAndSIDList] [-provider CSP]

Torna al [menu](#menu)

## <a name="-convertepf"></a>-ConvertEPF

CertUtil [opzioni]-ConvertEPF ElencoFileInPFX EPFOutFile [Cast | Cast-] [V3CACertId] [, Salt]

Converti i file PFX in un file EPF

ElencoFileInPFX Elenco di file di input PFX separati da virgole

EPF File di output EPF

Cast Usare la crittografia 64 di CAST

Cast-: Usare la crittografia 64 di CAST (esportazione)

V3CACertId: Token di corrispondenza certificato CA V3.  Vedere [: Store](#-store) CertId Description.

Sale Stringa Salt del file di output EPF

La password specificata nella riga di comando è un elenco di password separate da virgole. Se viene specificata più di una password, per il file di output viene utilizzata l'ultima password.  Se viene fornita una sola password o se l'ultima password è "\*", all'utente verrà richiesto di immettere la password del file di output.

[-f] [-invisibile all'utente] [-split] [-DC DCName] [-p password] [-provider CSP]

Torna al [menu](#menu)

## <a name="options"></a>Opzioni

Questa sezione definisce le opzioni che è possibile specificare con il comando.

|Opzioni|Descrizione|
|-------|-----------|
|-nullsign|USA hash di dati come firma|
|-f|Forza sovrascrittura|
|-Enterprise|Usa archivio certificati del registro di sistema aziendale del computer locale|
|-utente|Usare le chiavi HKEY_CURRENT_USER o l'archivio certificati|
|-GroupPolicy|USA Criteri di gruppo archivio certificati|
|-UT|Visualizzare i modelli utente|
|-Mt|Visualizza modelli di computer|
|-Unicode|Scrivi output reindirizzato in Unicode|
|-UnicodeText|Scrivi file di output in formato Unicode|
|-ora di Greenwich|Visualizza orari come GMT|
|-secondi|Visualizza orari con secondi e millisecondi|
|-invisibile all'utente|Usare il flag invisibile all'utente per acquisire il contesto della crittografia|
|-Divisione|Suddivide gli elementi ASN. 1 incorporati e Salva nei file|
|-v|Operazione dettagliata|
|-PrivateKey|Visualizzare i dati della password e della chiave privata|
|-pin pin|PIN della smart card|
|-UrlFetch|Recuperare e verificare i certificati AIA e i CRL CDP|
|-config Machine\CAName|Stringa nome computer e CA|
|-PolicyServer URLOrId|ID o URL del server dei criteri. Per la selezione di U/I, usare-PolicyServer. Per tutti i server dei criteri, usare-PolicyServer\*|
|-Anonimo|Usa credenziali SSL anonime|
|-Kerberos|Usa credenziali SSL Kerberos|
|-ClientCertificate ClientCertId|Usare le credenziali SSL del certificato X. 509. Per la selezione di U/I, usare-clientCertificate.|
|-UserName nomeutente|Usare l'account denominato per le credenziali SSL. Per la selezione U/I, use-UserName.|
|-CERT CertId|Certificato di firma|
|-DC DCName|Destinazione di un controller di dominio specifico|
|-limita restrizione|Elenco di restrizioni separate da virgole. Ogni restrizione è costituita da un nome di colonna, un operatore relazionale e un numero intero costante, una stringa o una data. Un nome di colonna può essere preceduto da un segno più o meno per indicare l'ordinamento. Esempi:</br>"RequestId = 47"</br>"+ RequesterName > = a, RequesterName < b"</br>"-RequesterName > dominio, Disposition = 21"|
|-out (istogramma)|Elenco di colonne delimitate da virgole|
|-p password|Password|
|-ProtectTo SAMNameAndSIDList|Elenco di SID e nomi SAM separati da virgole|
|-Provider CSP|Provider|
|-t timeout|Timeout recupero URL in millisecondi|
|-symkeyalg SymmetricKeyAlgorithm [, lunghezza]|Nome dell'algoritmo a chiave simmetrica con lunghezza della chiave facoltativa, ad esempio: AES, 128 o 3DES|

Torna al [menu](#menu)

## <a name="additional-certutil-examples"></a>Esempi di certutil aggiuntivi

Per alcuni esempi sull'uso di questo comando, vedere

1. [Esempi di certutil per la gestione di Servizi certificati Active Directory (AD CS) dalla riga di comando](https://social.technet.microsoft.com/wiki/contents/articles/3063.certutil-examples-for-managing-active-directory-certificate-services-ad-cs-from-the-command-line.aspx)
2. [Attività certutil per la gestione dei certificati](https://technet.microsoft.com/library/cc772898.aspx)
3. [Esportazione della richiesta binaria tramite la procedura dettagliata dello strumento da riga di comando CertUtil. exe](https://social.technet.microsoft.com/wiki/contents/articles/7573.active-directory-certificate-services-pki-key-archival-and-management.aspx)
4. [Rinnovo del certificato CA radice](https://social.technet.microsoft.com/wiki/contents/articles/2016.root-ca-certificate-renewal.aspx)
5. [Certutil](https://msdn.microsoft.com/subscriptions/cc773087.aspx)

Torna al [menu](#menu)
