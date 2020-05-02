---
title: certutil
description: Argomento di riferimento per il comando certutil, ovvero un programma da riga di comando che consente di eseguire il dump e visualizzare le informazioni di configurazione dell'autorità di certificazione (CA), configurare i servizi certificati, eseguire il backup e ripristinare i componenti CA e verificare i certificati, le coppie di chiavi e le catene di certificati.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c264ccf0-ba1e-412b-9dd3-d77dd9345ad9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3848c5493247e7e2d5e5b57be6d5d6e4015708b4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716247"
---
# <a name="certutil"></a>certutil

Certutil. exe è un programma da riga di comando, installato come parte di Servizi certificati. È possibile utilizzare certutil. exe per eseguire il dump e visualizzare le informazioni di configurazione dell'autorità di certificazione (CA), configurare i servizi certificati, eseguire il backup e ripristinare i componenti CA e verificare i certificati, le coppie di chiavi e le catene di certificati.

Se certutil viene eseguito in un'autorità di certificazione senza parametri aggiuntivi, Visualizza la configurazione dell'autorità di certificazione corrente. Se certutil viene eseguito in un'autorità non di certificazione, per impostazione predefinita viene eseguito il `certutil [-dump]` comando.

> [!IMPORTANT]
> Le versioni precedenti di certutil potrebbero non fornire tutte le opzioni descritte in questo documento. È possibile visualizzare tutte le opzioni fornite da una versione specifica di certutil eseguendo `certutil -?` o. `certutil <parameter> -?`

## <a name="parameters"></a>Parametri

### <a name="-dump"></a>-dump

Dump di informazioni di configurazione o file.

```
certutil [options] [-dump]
certutil [options] [-dump] file
```

```
[-f] [-silent] [-split] [-p password] [-t timeout]
```

### <a name="-asn"></a>-ASN

Analizzare il file ASN. 1.

```
certutil [options] -asn file [type]
```

`[type]`: Numeric CRYPT_STRING_ * tipo di decodifica

### <a name="-decodehex"></a>-decodehex

Decodifica un file con codifica esadecimale.

```
certutil [options] -decodehex infile outfile [type]
```

`[type]`: Numeric CRYPT_STRING_ * tipo di codifica

```
[-f]
```

### <a name="-decode"></a>-decodifica

Decodifica un file con codifica Base64.

```
certutil [options] -decode infile outfile
```

```
[-f]
```

### <a name="-encode"></a>-codifica

Codificare un file in Base64.

```
certutil [options] -encode infile outfile
```

```
[-f] [-unicodetext]
```

### <a name="-deny"></a>-nega

Negare una richiesta in sospeso.

```
certutil [options] -deny requestID
```

```
[-config Machine\CAName]
```

### <a name="-resubmit"></a>-Invia di più

Inviare nuovamente una richiesta in sospeso.

```
certutil [options] -resubmit requestId
```

```
[-config Machine\CAName]
```

### <a name="-setattributes"></a>-SetAttributes

Impostare gli attributi per una richiesta di certificato in sospeso.

```
certutil [options] -setattributes RequestID attributestring
```

Dove:

- **RequestId** è l'ID della richiesta numerica per la richiesta in sospeso.

- **AttributeString** sono le coppie nome e valore dell'attributo della richiesta.

```
[-config Machine\CAName]
```

#### <a name="remarks"></a>Osservazioni

- I nomi e i valori devono essere separati da due punti, mentre più coppie nome-valore devono essere separate da una nuova riga. Ad esempio, `CertificateTemplate:User\nEMail:User@Domain.com` in cui `\n` la sequenza viene convertita in un separatore di nuova riga.

### <a name="-setextension"></a>-seextension

Impostare un'estensione per una richiesta di certificato in sospeso.

```
certutil [options] -setextension requestID extensionname flags {long | date | string | \@infile}
```

Dove:

- **RequestId** è l'ID della richiesta numerica per la richiesta in sospeso.

- **ExtensionName** è la stringa ObjectID per l'estensione.

- **Flags** imposta la priorità dell'estensione. `0`si consiglia di `1` impostare l'estensione su critico, `2` di disabilitare l'estensione ed `3` eseguire entrambe le impostazioni.

```
[-config Machine\CAName]
```

#### <a name="remarks"></a>Osservazioni

- Se l'ultimo parametro è numerico, viene considerato **lungo**.

- Se l'ultimo parametro può essere analizzato come una data, viene considerato come una **Data**.

- Se l'ultimo parametro inizia con `\@`, il resto del token viene considerato come filename con dati binari o un dump esadecimale di testo ASCII.

- Se l'ultimo parametro è qualsiasi altro elemento, viene considerato come una stringa.

### <a name="-revoke"></a>-revoca

Revocare un certificato.

```
certutil [options] -revoke serialnumber [reason]
```

Dove:

- **serialNumber** è un elenco delimitato da virgole di numeri di serie del certificato da revocare.

- **reason** è la rappresentazione numerica o simbolica del motivo della revoca, tra cui:

  - **0. CRL_REASON_UNSPECIFIED** -non specificato (impostazione predefinita)

  - **1.** compromissione della chiave CRL_REASON_KEY_COMPROMISE

  - **2. CRL_REASON_CA_COMPROMISE** -compromissione dell'autorità di certificazione

  - **3. CRL_REASON_AFFILIATION_CHANGED** -affiliazione modificata

  - **4. CRL_REASON_SUPERSEDED** -sostituito

  - **5. CRL_REASON_CESSATION_OF_OPERATION** -cessazione dell'operazione

  - **6. CRL_REASON_CERTIFICATE_HOLD** -Mantieni il certificato

  - **8. CRL_REASON_REMOVE_FROM_CRL** -Rimuovi da CRL

  - **1. unrevoke** -Annulla revoca

```
[-config Machine\CAName]
```

### <a name="-isvalid"></a>-IsValid

Visualizza la disposizione del certificato corrente.

```
certutil [options] -isvalid serialnumber | certhash
```

```
[-config Machine\CAName]
```

### <a name="-getconfig"></a>-GetConfig

Ottiene la stringa di configurazione predefinita.

```
certutil [options] -getconfig
```

```
[-config Machine\CAName]
```

### <a name="-ping"></a>-ping

Tentare di contattare l'interfaccia richiesta di Active Directory Servizi certificati.

```
certutil [options] -ping [maxsecondstowait | camachinelist]
```

Dove:

- **camachine** list è un elenco delimitato da virgole di nomi di computer della CA. Per un singolo computer, usare una virgola di terminazione. Questa opzione Visualizza anche il costo del sito per ogni computer della CA.

```
[-config Machine\CAName]
```

### <a name="-cainfo"></a>-cainfo

Visualizzare informazioni sull'autorità di certificazione.

```
certutil [options] -cainfo [infoname [index | errorcode]]
```

Dove:

- **InfoName** indica la proprietà CA da visualizzare, in base alla sintassi dell'argomento InfoName seguente:

  - versione **file-file**
  
  - **prodotto** -versione prodotto
  
  - **exitcount** -numero di moduli di uscita
  
  - Descrizione del modulo **Exit `[index]` ** -Exit
  
  - **policy** -Descrizione modulo criteri
  
  - **nome** -nome CA
  
  - **sanitizedname** -nome CA purificata
  
  - **dsName** -nome breve CA purificato (nome DS)
  
  - **CartellaCondivisa** -cartella condivisa
  
  - **Error1 ErrorCode** -testo del messaggio di errore
  
  - **Error2 ErrorCode** -codice di errore e testo del messaggio di errore
  
  - **tipo** : tipo CA
  
  - **info** -info CA
  
  - CA **padre-padre**
  
  - **certcount** -conteggio certificati CA
  
  - **xchgcount** -conteggio certificati Exchange CA
  
  - conteggio certificati **kracount** -Kra
  
  - conteggio utilizzato del certificato **kraused** -Kra
  
  - **propidmax** -PropId CA massima
  
  - **certstate `[index]` ** -certificato CA
  
  - **certversion `[index]` ** -versione certificato CA
  
  - **certstatuscode `[index]` ** -stato verifica certificato CA
  
  - **crlstate `[index]` ** -CRL
  
  - **certificato `[index]` krastate** -Kra
  
  - **crossstate + `[index]` ** -diretta del certificato incrociato
  
  - **crossstate- `[index]` ** -certificato incrociato a ritroso
  
  - **certificato `[index]` certificato** CA
  
  - **certchain `[index]` ** -catena di certificati CA
  
  - **certcrlchain `[index]` ** -catena di certificati CA con CRL
  
  - **xchg `[index]` ** -certificato di scambio CA
  
  - **xchgchain `[index]` ** -catena di certificati di scambio CA

  - **xchgcrlchain `[index]` ** -catena di certificati di scambio CA con CRL
  
  - **Kra `[index]` ** : certificato Kra
  
  - Cross **+ `[index]` ** -inoltr (certificato incrociato)
  
  - certificato incrociato tra le versioni precedenti ** `[index]` **
  
  - CRL-base CRL ** `[index]` **
  
  - **deltacrl `[index]` ** -Delta CRL
  
  - **crlstatus `[index]` ** -stato pubblicazione CRL
  
  - **deltacrlstatus `[index]` ** -stato pubblicazione Delta CRL
  
  - **DNS** -nome DNS
  
  - Divisione **ruolo-ruolo**
  
  - **ADS** -server avanzato
  
  - **modelli-modelli**
  
  - **CSP `[index]` ** -URL OCSP
  
  - **URL `[index]` Aia** -Aia
  
  - **URL `[index]` CDP** -CDP
  
  - **localename** -nome delle impostazioni locali della CA
  
  - **subjecttemplateoids** -modello oggetto Oid
  
  - **&#42;** -Visualizza tutte le proprietà

- **index** è l'indice di proprietà facoltativo in base zero.

- **ErrorCode** è il codice di errore numerico.

```
[-f] [-split] [-config Machine\CAName]
```

### <a name="-cacert"></a>-CA. cert

Recuperare il certificato per l'autorità di certificazione.

```
certutil [options] -ca.cert outcacertfile [index]
```

Dove:

- **outcacertfile** è il file di output.

- **index** è l'indice di rinnovo del certificato dell'autorità di certificazione (il valore predefinito è quello più recente).

```
[-f] [-split] [-config Machine\CAName]
```

### <a name="-cachain"></a>-CA. Chain

Recuperare la catena di certificati per l'autorità di certificazione.

```
certutil [options] -ca.chain outcacertchainfile [index]
```

Dove:

- **outcacertchainfile** è il file di output.

- **index** è l'indice di rinnovo del certificato dell'autorità di certificazione (il valore predefinito è quello più recente).

```
[-f] [-split] [-config Machine\CAName]
```

### <a name="-getcrl"></a>-getcrl

Ottiene un elenco di revoche di certificati (CRL).

certutil [opzioni]-getcrl outfile [Indice] [Delta]

Dove:

- **index** è l'indice CRL o Key index (il valore predefinito è CRL per la chiave più recente).

- **Delta** CRL (il valore predefinito è base CRL).

```
[-f] [-split] [-config Machine\CAName]
```

### <a name="-crl"></a>-CRL

Pubblicare nuovi elenchi di revoche di certificati (CRL) o Delta CRL.

```
certutil [options] -crl [dd:hh | republish] [delta]
```

Dove:

- **DD: HH** è il nuovo periodo di validità CRL in giorni e ore.

- **ripubblicare** nuovamente i CRL più recenti.

- **Delta** pubblica solo Delta CRL (il valore predefinito è base e Delta CRL).

```
[-split] [-config Machine\CAName]
```

### <a name="-shutdown"></a>-arresta

Arresta il Active Directory Servizi certificati.

```
certutil [options] -shutdown
```

```
[-config Machine\CAName]
```

### <a name="-installcert"></a>-installCert

Installa un certificato dell'autorità di certificazione.

```
certutil [options] -installcert [cacertfile]
```

```
[-f] [-silent] [-config Machine\CAName]
```

### <a name="-renewcert"></a>-renewcert

Rinnova un certificato dell'autorità di certificazione.

```
certutil [options] -renewcert [reusekeys] [Machine\ParentCAName]
```

- Usare `-f` per ignorare una richiesta di rinnovo in attesa e per generare una nuova richiesta.

```
[-f] [-silent] [-config Machine\CAName]
```

### <a name="-schema"></a>-Schema

Dump dello schema per il certificato.

```
certutil [options] -schema [ext | attrib | cRL]
```
Dove:

- Per impostazione predefinita, il comando è la tabella Request e certificate.

- **ext** è la tabella di estensione.
  
- **attribute** è la tabella degli attributi.

- **CRL** è la tabella CRL.

```
[-split] [-config Machine\CAName]
```

### <a name="-view"></a>-Visualizza

Dump della visualizzazione del certificato.

```
certutil [options] -view [queue | log | logfail | revoked | ext | attrib | crl] [csv]
```

Dove:

- **queue** viene eseguito il dump di una coda di richieste specifica.

- **log** Scarica i certificati emessi o revocati, oltre a eventuali richieste non riuscite.

- **LogFail** dump delle richieste non riuscite.

- **revoca** i certificati revocati.

- **ext** dump della tabella di estensione.
  
- l' **attributo** Esegui il dump della tabella degli attributi.

- **CRL** Esegui il dump della tabella CRL.

- **CSV** fornisce l'output usando valori delimitati da virgole.

```
[-silent] [-split] [-config Machine\CAName] [-restrict RestrictionList] [-out ColumnList]
```

#### <a name="remarks"></a>Osservazioni

- Per visualizzare la colonna **statusCode** per tutte le voci, digitare`-out StatusCode`

- Per visualizzare tutte le colonne per l'ultima voce, digitare:`-restrict RequestId==$`

- Per visualizzare i **RequestId** e la **disposizione** per tre richieste, digitare:`-restrict requestID>37,requestID<40 -out requestID,disposition`

- Per visualizzare gli**ID di riga** degli ID di riga e i **numeri CRL** per tutti i CRL di base, digitare:`-restrict crlminbase=0 -out crlrowID,crlnumber crl`

- Per visualizzare, digitare:`-v -restrict crlminbase=0,crlnumber=3 -out crlrawcrl crl`

- Per visualizzare l'intera tabella CRL, digitare:`CRL`

- Usare `Date[+|-dd:hh]` per le restrizioni relative alla data.

- Utilizzare `now+dd:hh` per una data relativa all'ora corrente.

### <a name="-db"></a>-DB

Dump del database non elaborato.

```
certutil [options] -db
```

```
[-config Machine\CAName] [-restrict RestrictionList] [-out ColumnList]
```

### <a name="-deleterow"></a>-DeleteRow

Elimina una riga dal database del server.

```
certutil [options] -deleterow rowID | date [request | cert | ext | attrib | crl]
```

Dove:

- **Request** Elimina le richieste non riuscite e in sospeso, in base alla data di invio.

- **CERT** Elimina i certificati scaduti e revocati, in base alla data di scadenza.

- **ext** Elimina la tabella di estensione.
  
- l' **attributo** Elimina la tabella degli attributi.

- **CRL** Elimina la tabella CRL.

```
[-f] [-config Machine\CAName]
```

#### <a name="examples"></a>Esempi

- Per eliminare le richieste non riuscite e in sospeso inviate dal 22 gennaio 2001, digitare:`1/22/2001 request`

- Per eliminare tutti i certificati scaduti entro il 22 gennaio 2001, digitare:`1/22/2001 cert`

- Per eliminare la riga, gli attributi e le estensioni del certificato per RequestId 37, digitare:`37`

- Per eliminare i CRL scaduti entro il 22 gennaio 2001, digitare:`1/22/2001 crl`

### <a name="-backup"></a>-backup

Esegue il backup dei servizi certificati Active Directory.

```
certutil [options] -backup backupdirectory [incremental] [keeplog]
```

Dove:

- **BackupDirectory** è la directory in cui archiviare i dati di cui è stato eseguito il backup.

- **Incremental** esegue solo un backup incrementale (l'impostazione predefinita è backup completo).

- **keeplog** consente di mantenere i file di log del database (il valore predefinito è il troncamento dei file di log).

```
[-f] [-config Machine\CAName] [-p Password]
```

### <a name="-backupdb"></a>-backupdb

Esegue il backup del database dei servizi certificati Active Directory.

```
certutil [options] -backupdb backupdirectory [incremental] [keeplog]
```

Dove:

- **BackupDirectory** è la directory in cui archiviare i file di database di cui è stato eseguito il backup.

- **Incremental** esegue solo un backup incrementale (l'impostazione predefinita è backup completo).

- **keeplog** consente di mantenere i file di log del database (il valore predefinito è il troncamento dei file di log).

```
[-f] [-config Machine\CAName]
```

### <a name="-backupkey"></a>-backupkey

Esegue il backup del certificato e della chiave privata dei servizi certificati Active Directory.

```
certutil [options] -backupkey backupdirectory
```

Dove:

- **BackupDirectory** è la directory in cui archiviare il file PFX di cui è stato eseguito il backup.

```
[-f] [-config Machine\CAName] [-p password] [-t timeout]
```

### <a name="-restore"></a>-restore

Ripristina i servizi certificati Active Directory.

```
certutil [options] -restore backupdirectory
```

Dove:

- **BackupDirectory** è la directory che contiene i dati da ripristinare.

```
[-f] [-config Machine\CAName] [-p password]
```

### <a name="-restoredb"></a>-RestoreDB

Ripristina il Active Directory database dei servizi certificati.

```
certutil [options] -restoredb backupdirectory
```

Dove:

- **BackupDirectory** è la directory che contiene i file di database da ripristinare.

```
[-f] [-config Machine\CAName]
```

### <a name="-restorekey"></a>-restorekey

Ripristina il certificato di Servizi certificati Active Directory e la chiave privata.

```
certutil [options] -restorekey backupdirectory | pfxfile
```

Dove:

- **BackupDirectory** è la directory che contiene il file PFX da ripristinare.

```
[-f] [-config Machine\CAName] [-p password]
```

### <a name="-importpfx"></a>-importpfx

Importare il certificato e la chiave privata. Per altre informazioni, vedere il `-store` parametro in questo articolo.

```
certutil [options] -importpfx [certificatestorename] pfxfile [modifiers]
```

Dove:

- **NomeArchivioCertificati** è il nome dell'archivio certificati.

- i **modificatori** sono l'elenco delimitato da virgole, che può includere uno o più degli elementi seguenti:

  1. **AT_SIGNATURE** -modifica la specifica di una scheda in firma
  
  2. **AT_KEYEXCHANGE** : modifica la specifica della chiave in scambio di chiave
  
  3. **Noexport** : rende la chiave privata non esportabile
  
  4. **NoCert** -non importa il certificato
  
  5. **NoChain** : non importa la catena di certificati
  
  6. **Noroot** : non importa il certificato radice
  
  7. **Proteggi** : protegge le chiavi usando una password
  
  8. **Noprotect** -non protegge le chiavi tramite password

```
[-f] [-user] [-p password] [-csp provider]
```

#### <a name="remarks"></a>Osservazioni

- Il valore predefinito è Personal Machine Store.

### <a name="-dynamicfilelist"></a>-dynamicfilelist

Visualizza un elenco di file dinamici.

```
certutil [options] -dynamicfilelist
```

```
[-config Machine\CAName]
```

### <a name="-databaselocations"></a>-databaselocations

Visualizza i percorsi di database.

```
certutil [options] -databaselocations
```

```
[-config Machine\CAName]
```

### <a name="-hashfile"></a>-hashfile

Genera e visualizza un hash crittografico su un file.

```
certutil [options] -hashfile infile [hashalgorithm]
```

### <a name="-store"></a>-Archivio

Esegui il dump dell'archivio certificati.

```
certutil [options] -store [certificatestorename [certID [outputfile]]]
```

Dove:

- **NomeArchivioCertificati** è il nome dell'archivio certificati. Ad esempio:

  - `My, CA (default), Root,`
  
  - `ldap:///CN=Certification Authorities,CN=Public Key Services,CN=Services,CN=Configuration,DC=cpandl,DC=com?cACertificate?one?objectClass=certificationAuthority (View Root Certificates)`
  
  - `ldap:///CN=CAName,CN=Certification Authorities,CN=Public Key Services,CN=Services,CN=Configuration,DC=cpandl,DC=com?cACertificate?base?objectClass=certificationAuthority (Modify Root Certificates)`
  
  - `ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services,CN=Services,CN=Configuration,DC=cpandl,DC=com?certificateRevocationList?base?objectClass=cRLDistributionPoint (View CRLs)`
  
  - `ldap:///CN=NTAuthCertificates,CN=Public Key Services,CN=Services,CN=Configuration,DC=cpandl,DC=com?cACertificate?base?objectClass=certificationAuthority (Enterprise CA Certificates)`
  
  - `ldap: (AD computer object certificates)`
  
  - `-user ldap: (AD user object certificates)`

- **certID** è il token di corrispondenza del certificato o del CRL. Può trattarsi di un numero di serie, un certificato SHA-1, un elenco di revoche di certificati, un elenco di scopi consentiti o un hash di chiave pubblica, un indice numerico (0, 1 e così via), un indice CRL numerico (. 0, 1 e così via), un indice CTL numerico (.. 0... 1, e così via), una chiave pubblica, una firma o un ObjectId di estensione, un nome comune del soggetto del certificato, un indirizzo di posta elettronica, un nome UPN o DNS, un nome del contenitore di chiavi o un nome CSP, un nome di modello o un ObjectId, un valore EKU o un ObjectId di criteri di applicazione o un nome comune dell'autorità di certificazione CRL. Molti di questi possono comportare più corrispondenze.

- **outputfile** è il file usato per salvare i certificati corrispondenti.

```
[-f] [-user] [-enterprise] [-service] [-grouppolicy] [-silent] [-split] [-dc DCName]
```

#### <a name="options"></a>Opzioni

- L' `-user` opzione accede a un archivio utenti anziché a un archivio del computer.

- L' `-enterprise` opzione consente di accedere a un archivio aziendale del computer.

- L' `-service` opzione consente di accedere a un archivio del servizio computer.

- L' `-grouppolicy` opzione consente di accedere a un archivio criteri di gruppo del computer.

Ad esempio:

- `-enterprise NTAuth`

- `-enterprise Root 37`

- `-user My 26e0aaaf000000000004`

- `CA .11`

### <a name="-addstore"></a>-addstore

Aggiunge un certificato all'archivio. Per altre informazioni, vedere il `-store` parametro in questo articolo.

```
certutil [options] -addstore certificatestorename infile
```

Dove:

- **NomeArchivioCertificati** è il nome dell'archivio certificati.

- **infile** è il certificato o il file CRL che si desidera aggiungere all'archivio.

```
[-f] [-user] [-enterprise] [-grouppolicy] [-dc DCName]
```

### <a name="-delstore"></a>-delstore

Elimina un certificato dall'archivio. Per altre informazioni, vedere il `-store` parametro in questo articolo.

```
certutil [options] -delstore certificatestorename certID
```

Dove:

- **NomeArchivioCertificati** è il nome dell'archivio certificati.

- **certID** è il token di corrispondenza del certificato o del CRL.

```
[-enterprise] [-user] [-grouppolicy] [-dc DCName]
```

### <a name="-verifystore"></a>-verifystore

Verifica un certificato nell'archivio. Per altre informazioni, vedere il `-store` parametro in questo articolo.

```
certutil [options] -verifystore certificatestorename [certID]
```

Dove:

- **NomeArchivioCertificati** è il nome dell'archivio certificati.

- **certID** è il token di corrispondenza del certificato o del CRL.

```
[-enterprise] [-user] [-grouppolicy] [-silent] [-split] [-dc DCName] [-t timeout]
```

### <a name="-repairstore"></a>-repairstore

Ripristina un'associazione di chiavi o aggiorna le proprietà del certificato o il descrittore di sicurezza della chiave. Per altre informazioni, vedere il `-store` parametro in questo articolo.

```
certutil [options] -repairstore certificatestorename certIDlist [propertyinffile | SDDLsecuritydescriptor]
```

Dove:

- **NomeArchivioCertificati** è il nome dell'archivio certificati.

- **certIDlist** è l'elenco delimitato da virgole di token di corrispondenza con certificati o CRL. Per altre informazioni, vedere la `-store certID` descrizione in questo articolo.

- **propertyinffile** è il file inf contenente le proprietà esterne, tra cui:

  ```
  [Properties]
      19 = Empty ; Add archived property, OR:
      19 =       ; Remove archived property

      11 = {text}Friendly Name ; Add friendly name property

      127 = {hex} ; Add custom hexadecimal property
          _continue_ = 00 01 02 03 04 05 06 07 08 09 0a 0b 0c 0d 0e 0f
          _continue_ = 10 11 12 13 14 15 16 17 18 19 1a 1b 1c 1d 1e 1f

      2 = {text} ; Add Key Provider Information property
        _continue_ = Container=Container Name&
        _continue_ = Provider=Microsoft Strong Cryptographic Provider&
        _continue_ = ProviderType=1&
        _continue_ = Flags=0&
        _continue_ = KeySpec=2

      9 = {text} ; Add Enhanced Key Usage property
        _continue_ = 1.3.6.1.5.5.7.3.2,
        _continue_ = 1.3.6.1.5.5.7.3.1,
  ```

```
[-f] [-enterprise] [-user] [-grouppolicy] [-silent] [-split] [-csp provider]
```

### <a name="-viewstore"></a>-viewstore

Esegui il dump dell'archivio certificati. Per altre informazioni, vedere il `-store` parametro in questo articolo.

```
certutil [options] -viewstore [certificatestorename [certID [outputfile]]]
```

Dove:

- **NomeArchivioCertificati** è il nome dell'archivio certificati.

- **certID** è il token di corrispondenza del certificato o del CRL.

- **outputfile** è il file usato per salvare i certificati corrispondenti.

```
[-f] [-user] [-enterprise] [-service] [-grouppolicy] [-dc DCName]
```

#### <a name="options"></a>Opzioni

- L' `-user` opzione accede a un archivio utenti anziché a un archivio del computer.

- L' `-enterprise` opzione consente di accedere a un archivio aziendale del computer.

- L' `-service` opzione consente di accedere a un archivio del servizio computer.

- L' `-grouppolicy` opzione consente di accedere a un archivio criteri di gruppo del computer.

Ad esempio:

- `-enterprise NTAuth`

- `-enterprise Root 37`

- `-user My 26e0aaaf000000000004`

- `CA .11`

### <a name="-viewdelstore"></a>-viewdelstore

Elimina un certificato dall'archivio.

```
certutil [options] -viewdelstore [certificatestorename [certID [outputfile]]]
```

Dove:

- **NomeArchivioCertificati** è il nome dell'archivio certificati.

- **certID** è il token di corrispondenza del certificato o del CRL.

- **outputfile** è il file usato per salvare i certificati corrispondenti.

```
[-f] [-user] [-enterprise] [-service] [-grouppolicy] [-dc DCName]
```

#### <a name="options"></a>Opzioni

- L' `-user` opzione accede a un archivio utenti anziché a un archivio del computer.

- L' `-enterprise` opzione consente di accedere a un archivio aziendale del computer.

- L' `-service` opzione consente di accedere a un archivio del servizio computer.

- L' `-grouppolicy` opzione consente di accedere a un archivio criteri di gruppo del computer.

Ad esempio:

- `-enterprise NTAuth`

- `-enterprise Root 37`

- `-user My 26e0aaaf000000000004`

- `CA .11`

### <a name="-dspublish"></a>-dspublish

Pubblica un certificato o un elenco di revoche di certificati (CRL) in Active Directory.

```
certutil [options] -dspublish certfile [NTAuthCA | RootCA | SubCA | CrossCA | KRA | User | Machine]
```

```
certutil [options] -dspublish CRLfile [DSCDPContainer [DSCDPCN]]
```

Dove:

- **CertFile** è il nome del file di certificato da pubblicare.

- **Ntauthca** pubblica il certificato in DS Enterprise Store.

- **Rootca** pubblica il certificato nell'archivio radice attendibile DS.

- **SubCA** pubblica il certificato della CA nell'oggetto CA DS.

- **Crossca** pubblica il certificato incrociato nell'oggetto CA DS.

- **Kra** pubblica il certificato nell'oggetto dell'agente di recupero chiavi DS.

- L' **utente** pubblica il certificato nell'oggetto DS utente.

- Il **computer** pubblica il certificato nell'oggetto Machine DS.

- **FileCRL** è il nome del file CRL da pubblicare.

- **DSCDPContainer** è il CN del contenitore CDP DS, in genere il nome del computer della CA.

- **DSCDPCN** è l'oggetto CDP di DS, in genere basato sul nome breve e sull'indice della chiave della CA purificata.

- Utilizzare `-f` per creare un nuovo oggetto DS.

```
[-f] [-user] [-dc DCName]
```

### <a name="-adtemplate"></a>-adtemplate

Visualizza i modelli Active Directory.

```
certutil [options] -adtemplate [template]
```

```
[-f] [-user] [-ut] [-mt] [-dc DCName]
```

### <a name="-template"></a>-modello

Consente di visualizzare i modelli di certificato.

```
certutil [options] -template [template]
```

```
[-f] [-user] [-silent] [-policyserver URLorID] [-anonymous] [-kerberos] [-clientcertificate clientcertID] [-username username] [-p password]
```

### <a name="-templatecas"></a>-templatecas

Visualizza le autorità di certificazione (CAs) per un modello di certificato.

```
certutil [options] -templatecas template
```

```
[-f] [-user] [-dc DCName]
```

### <a name="-catemplates"></a>-catemplates

Visualizza i modelli per l'autorità di certificazione.

```
certutil [options] -catemplates [template]
```

```
[-f] [-user] [-ut] [-mt] [-config Machine\CAName] [-dc DCName]
```

### <a name="-setcasites"></a>-setcasites

Gestisce i nomi dei siti, inclusa l'impostazione, la verifica e l'eliminazione dei nomi dei siti dell'autorità di certificazione

```
certutil [options] -setcasites [set] [sitename]
certutil [options] -setcasites verify [sitename]
certutil [options] -setcasites delete
```

Dove:

- il nome **sito è consentito** solo quando la destinazione è una singola autorità di certificazione.

```
[-f] [-config Machine\CAName] [-dc DCName]
```

#### <a name="remarks"></a>Osservazioni

- L' `-config` opzione è destinata a una singola autorità di certificazione (l'impostazione predefinita è tutte le CA).

- L' `-f` opzione può essere usata per eseguire l'override degli errori di convalida per il nome sito specificato o per eliminare tutti **i SiteName** della CA.

> [!NOTE]
> Per ulteriori informazioni sulla configurazione di autorità di certificazione per il riconoscimento dei siti Active Directory Domain Services (AD DS), vedere [riconoscimento del sito di servizi di dominio Active Directory per i client ad CS e PKI](https://social.technet.microsoft.com/wiki/contents/articles/14106.ad-ds-site-awareness-for-ad-cs-and-pki-clients.aspx).

### <a name="-enrollmentserverurl"></a>-enrollmentserverURL

Visualizza, aggiunge o Elimina gli URL del server di registrazione associati a una CA.

```
certutil [options] -enrollmentServerURL [URL authenticationtype [priority] [modifiers]]
certutil [options] -enrollmentserverURL URL delete
```

Dove:

- **AuthenticationType** specifica uno dei seguenti metodi di autenticazione client, durante l'aggiunta di un URL:

  1. **Kerberos** : usare le credenziali SSL Kerberos.

  2. **nome utente** : usare un account denominato per le credenziali SSL.

  3. **ClientCertificate**: usare le credenziali SSL del certificato X. 509.

  4. **Anonimo** : Usa credenziali SSL anonime.

- **Elimina** consente di eliminare l'URL specificato associato all'autorità di certificazione.

- **priority** per impostazione predefinita, `1` la priorità è se non viene specificato quando si aggiunge un URL.

- i **modificatori** sono un elenco delimitato da virgole, che include uno o più degli elementi seguenti:

1. **allowrenewalsonly** : solo le richieste di rinnovo possono essere inviate a questa CA tramite questo URL.

2. **allowkeybasedrenewal** : consente l'uso di un certificato a cui non è associato alcun account in Active Directory. Si applica solo alla modalità ClientCertificate e allowrenewalsonly

```
[-config Machine\CAName] [-dc DCName]
```

### <a name="-adca"></a>-ADCA

Visualizza Active Directory autorità di certificazione.

```
certutil [options] -adca [CAName]
```

```
[-f] [-split] [-dc DCName]
```

### <a name="-ca"></a>CustomScript.fr

Visualizza le autorità di certificazione dei criteri di registrazione.

```
certutil [options] -CA [CAName | templatename]
```

```
[-f] [-user] [-silent] [-split] [-policyserver URLorID] [-anonymous] [-kerberos] [-clientcertificate clientcertID] [-username username] [-p password]
```

### <a name="-policy"></a>-criterio

Consente di visualizzare i criteri di registrazione.

```
[-f] [-user] [-silent] [-split] [-policyserver URLorID] [-anonymous] [-kerberos] [-clientcertificate clientcertID] [-username username] [-p password]
```

### <a name="-policycache"></a>-PolicyCache

Consente di visualizzare o eliminare le voci della cache dei criteri di registrazione.

```
certutil [options] -policycache [delete]
```

Dove:

- **Elimina** Elimina le voci della cache del server dei criteri.

- **-f** Elimina tutte le voci della cache

```
[-f] [-user] [-policyserver URLorID]
```

### <a name="-credstore"></a>-credstore

Consente di visualizzare, aggiungere o eliminare le voci dell'archivio credenziali.

```
certutil [options] -credstore [URL]
certutil [options] -credstore URL add
certutil [options] -credstore URL delete
```

Dove:

- **URL** è l'URL di destinazione. È anche possibile usare `*` per trovare la corrispondenza con `https://machine*` tutte le voci o per trovare la corrispondenza con un prefisso URL.

- **Aggiungi** aggiunge una voce dell'archivio credenziali. L'utilizzo di questa opzione richiede anche l'utilizzo di credenziali SSL.

- **Delete** Elimina le voci dell'archivio credenziali.

- **-f** sovrascrive una singola voce o Elimina più voci.

```
[-f] [-user] [-silent] [-anonymous] [-kerberos] [-clientcertificate clientcertID] [-username username] [-p password]
```

### <a name="-installdefaulttemplates"></a>-installdefaulttemplates

Installa i modelli di certificato predefiniti.

```
certutil [options] -installdefaulttemplates
```

```
[-dc DCName]
```

### <a name="-urlcache"></a>-URLcache

Consente di visualizzare o eliminare le voci della cache URL.

```
certutil [options] -URLcache [URL | CRL | * [delete]]
```

Dove:

- **URL** è l'URL memorizzato nella cache.

- **CRL** viene eseguito solo su tutti gli URL CRL memorizzati nella cache.

- **&#42;** funziona su tutti gli URL memorizzati nella cache.

- **Elimina** Elimina gli URL pertinenti dalla cache locale dell'utente corrente.

- **-f** forza il recupero di un URL specifico e l'aggiornamento della cache.

```
[-f] [-split]
```

### <a name="-pulse"></a>-impulso

Gli eventi di registrazione automatica degli impulsi.

```
certutil [options] -pulse
```

```
[-user]
```

### <a name="-machineinfo"></a>-machineinfo

Visualizza informazioni sull'oggetto computer Active Directory.

```
certutil [options] -machineinfo domainname\machinename$
```

### <a name="-dcinfo"></a>-DCInfo

Visualizza informazioni sul controller di dominio. Il valore predefinito Visualizza i certificati DC senza verifica.

```
certutil [options] -DCInfo [domain] [verify | deletebad | deleteall]
```

```
[-f] [-user] [-urlfetch] [-dc DCName] [-t timeout]
```

> [!TIP]
> La possibilità di specificare un dominio di Active Directory Domain Services (AD DS) **[dominio]** e di specificare un controller di dominio (**-DC**) è stato aggiunto in Windows Server 2012. Per eseguire correttamente il comando, è necessario usare un account membro di **Domain Admins** o **Enterprise Admins**. Di seguito sono riportate le modifiche del comportamento di questo comando:<ol><li>1. se non si specifica un dominio e non si specifica un controller di dominio specifico, questa opzione restituisce un elenco di controller di dominio da elaborare dal controller di dominio predefinito.</li><li>2. se non viene specificato un dominio, ma viene specificato un controller di dominio, viene generato un report dei certificati sul controller di dominio specificato.</li><li>3. Se si specifica un dominio, ma non si specifica un controller di dominio, viene generato un elenco di controller di dominio insieme ai report sui certificati per ogni controller di dominio nell'elenco.</li><li>4. Se si specifica il dominio e il controller di dominio, viene generato un elenco di controller di dominio dal controller di dominio di destinazione. Viene generato anche un report dei certificati per ogni controller di dominio nell'elenco.</li></ol>
>
>Si supponga, ad esempio, che esista un dominio denominato CPANDL con un controller di dominio denominato CPANDL-DC1. È possibile eseguire il comando seguente per recuperare un elenco di controller di dominio e i relativi certificati da CPANDL-DC1:`certutil -dc cpandl-dc1 -DCInfo cpandl`

### <a name="-entinfo"></a>-entinfo

Visualizza informazioni su un'autorità di certificazione dell'organizzazione.

```
certutil [options] -entinfo domainname\machinename$
```

```
[-f] [-user]
```

### <a name="-tcainfo"></a>-tcainfo

Visualizza informazioni sull'autorità di certificazione.

```
certutil [options] -tcainfo [domainDN | -]
```

```
[-f] [-enterprise] [-user] [-urlfetch] [-dc DCName] [-t timeout]
```

### <a name="-scinfo"></a>-scinfo

Visualizza le informazioni sulla smart card.

```
certutil [options] -scinfo [readername [CRYPT_DELETEKEYSET]]
```

Dove:

- **CRYPT_DELETEKEYSET** Elimina tutte le chiavi della smart card.

```
[-silent] [-split] [-urlfetch] [-t timeout]
```

### <a name="-scroots"></a>-scroots

Gestisce i certificati radice della smart card.

```
certutil [options] -scroots update [+][inputrootfile] [readername]
certutil [options] -scroots save \@in\\outputrootfile [readername]
certutil [options] -scroots view [inputrootfile | readername]
certutil [options] -scroots delete [readername]
```

```
[-f] [-split] [-p Password]
```

### <a name="-verifykeys"></a>-verifykeys

Verifica un set di chiavi pubbliche o private.

```
certutil [options] -verifykeys [keycontainername cacertfile]
```

Dove:

- **datacontainername** è il nome del contenitore di chiavi per la chiave da verificare. Per impostazione predefinita, questa opzione è impostata su chiavi computer. Per passare alle chiavi utente, usare `-user`.

- **FileCertificatoCA** firma o Crittografa i file di certificato.

```
[-f] [-user] [-silent] [-config Machine\CAName]
```

#### <a name="remarks"></a>Osservazioni

- Se non viene specificato alcun argomento, ogni certificato della CA di firma viene verificato rispetto alla relativa chiave privata.

- Questa operazione può essere eseguita solo su una CA locale o su chiavi locali.

### <a name="-verify"></a>-Verifica

Verifica un certificato, un elenco di revoche di certificati (CRL) o una catena di certificati.

```
certutil [options] -verify certfile [applicationpolicylist | - [issuancepolicylist]]
certutil [options] -verify certfile [cacertfile [crossedcacertfile]]
certutil [options] -verify CRLfile cacertfile [issuedcertfile]
certutil [options] -verify CRLfile cacertfile [deltaCRLfile]
```

Dove:

- **CertFile** è il nome del certificato da verificare.

- **applicationpolicylist** è l'elenco facoltativo delimitato da virgole dei criteri dell'applicazione ObjectID richiesti.

- **issuancepolicylist** è l'elenco facoltativo delimitato da virgole dei criteri di rilascio ObjectID richiesti.

- **FileCertificatoCA** è il certificato della CA emittente facoltativo da verificare.

- **crossedcacertfile** è il certificato facoltativo con certificazione incrociata di **CertFile**.

- **FileCRL** è il file CRL usato per verificare il **FileCertificatoCA**.

- **issuedcertfile** è il certificato emesso facoltativo incluso in FileCRL.

- **deltaCRLfile** è il file Delta CRL facoltativo.

```
[-f] [-enterprise] [-user] [-silent] [-split] [-urlfetch] [-t timeout]
```

#### <a name="remarks"></a>Osservazioni

- L'uso di **applicationpolicylist** limita la creazione della catena a solo catene valide per i criteri dell'applicazione specificati.

- L'uso di **issuancepolicylist** limita la creazione della catena a solo catene valide per i criteri di rilascio specificati.

- L'uso di **FileCertificatoCA** verifica i campi nel file rispetto a **CertFile** o **FileCRL**.

- L'uso di **issuedcertfile** verifica i campi nel file in **FileCRL**.

- L'uso di deltaCRLfile verifica i campi nel file in **CertFile**.

- Se **FileCertificatoCA** non è specificato, la catena completa viene compilata e verificata in base a **CertFile**.

- Se **FileCertificatoCA** e **crossedcacertfile** sono entrambi specificati, i campi in entrambi i file vengono verificati in base a **CertFile**.

### <a name="-verifyctl"></a>-verifyCTL

Verifica il AuthRoot o i certificati non consentiti CTL.

```
certutil [options] -verifyCTL CTLobject [certdir] [certfile]
```

Dove:

- **CTLobject** identifica l'elenco di scopi consentiti da verificare, tra cui:

  - **AuthRootWU** : legge il CAB AuthRoot e i certificati corrispondenti dalla cache URL. Usare `-f` per scaricare invece da Windows Update.

  - **DisallowedWU** : legge i certificati non consentiti CAB e il file dell'archivio certificati non consentito dalla cache degli URL. Usare `-f` per scaricare invece da Windows Update.

  - **AuthRoot** : legge il registro di sistema AuthRoot CTL memorizzato nella cache. Usare con `-f` e un **CertFile** non attendibile per forzare la memorizzazione nella cache del registro di sistema AuthRoot e non consentire l'aggiornamento del certificato scopi consentiti.

  - Non **consentito** : legge i certificati non consentiti memorizzati nella cache del registro di sistema (CTL). Usare con `-f` e un **CertFile** non attendibile per forzare la memorizzazione nella cache del registro di sistema AuthRoot e non consentire l'aggiornamento del certificato scopi consentiti.

- **CTLfilename** specifica il file o il percorso http del file CTL o CAB.

- **certdir** specifica la cartella che contiene i certificati corrispondenti alle voci CTL. Il valore predefinito è la stessa cartella o sito Web del **CTLobject**. L'uso di un percorso di cartella http richiede un separatore di percorso alla fine. Se non si specifica **AuthRoot** o non è **consentito**, verranno cercati più percorsi per i certificati corrispondenti, inclusi gli archivi certificati locali, le risorse crypt32. dll e la cache URL locale. Usare `-f` per scaricare da Windows Update, in base alle esigenze.

- **CertFile** specifica i certificati da verificare. I certificati vengono confrontati con le voci CTL, visualizzando i risultati. Questa opzione consente di disattivare la maggior parte dell'output predefinito.

```
[-f] [-user] [-split]
```

### <a name="-sign"></a>-firma

Firma nuovamente un elenco di revoche di certificati (CRL) o un certificato.

```
certutil [options] -sign infilelist | serialnumber | CRL outfilelist [startdate+dd:hh] [+serialnumberlist | -serialnumberlist | -objectIDlist | \@extensionfile]
certutil [options] -sign infilelist | serialnumber | CRL outfilelist [#hashalgorithm] [+alternatesignaturealgorithm | -alternatesignaturealgorithm]
```

Dove:

- **FileList è l'** elenco delimitato da virgole dei file di certificato o CRL da modificare e firmare nuovamente.

- **serialNumber** è il numero di serie del certificato da creare. Non è possibile presentare il periodo di validità e le altre opzioni.

- **CRL** crea un CRL vuoto. Non è possibile presentare il periodo di validità e le altre opzioni.

- **outlisting** è l'elenco delimitato da virgole di file di output di certificati o CRL modificati. Il numero di file deve corrispondere a un file con estensione.

- **StartDate + GG: HH** è il nuovo periodo di validità per il certificato o i file CRL, tra cui:

  - data facoltativa più

  - periodo di validità di giorni e ore facoltativo
  
  Se vengono specificati entrambi, è necessario usare un separatore di segno più (+). Utilizzare `now[+dd:hh]` per avviare l'ora corrente. Utilizzare `never` per non avere una data di scadenza (solo per i CRL).

- **serialnumberlist** è l'elenco di numeri di serie separati da virgole dei file da aggiungere o rimuovere.

- **objectIDlist** è l'elenco di file con estensione delimitato da virgole da rimuovere.

- extensionfile è il file inf che contiene le estensioni da aggiornare o rimuovere. ** \@** Ad esempio:

  ```
  [Extensions]
      2.5.29.31 = ; Remove CRL Distribution Points extension
      2.5.29.15 = {hex} ; Update Key Usage extension
      _continue_=03 02 01 86
  ```

- **HashAlgorithm** è il nome dell'algoritmo hash. Deve essere solo il testo preceduto dal `#` segno.

- **alternatesignaturealgorithm** è l'identificatore dell'algoritmo di firma alternativo.

```
[-nullsign] [-f] [-silent] [-cert certID]
```

#### <a name="remarks"></a>Osservazioni

- Utilizzando il segno meno (-) vengono rimossi i numeri di serie e le estensioni.

- Utilizzando il segno più (+), i numeri di serie vengono aggiunti a un CRL.

- È possibile utilizzare un elenco per rimuovere contemporaneamente i numeri di serie e ObjectID da un CRL.

- L'uso del segno meno prima di **alternatesignaturealgorithm** consente di usare il formato di firma legacy. L'uso del segno più consente di usare il formato di firma alternativo. Se non si specifica **alternatesignaturealgorithm**, viene usato il formato della firma nel certificato o nel CRL.

### <a name="-vroot"></a>-vroot

Crea o Elimina le radici virtuali Web e le condivisioni file.

```
certutil [options] -vroot [delete]
```

### <a name="-vocsproot"></a>-vocsproot

Crea o Elimina le radici virtuali Web per un proxy Web OCSP.

```
certutil [options] -vocsproot [delete]
```

### <a name="-addenrollmentserver"></a>-addenrollmentserver

Aggiungere un'applicazione del server di iscrizione e un pool di applicazioni, se necessario, per l'autorità di certificazione specificata. Questo comando non installa i file binari o i pacchetti.

```
certutil [options] -addenrollmentserver kerberos | username | clientcertificate [allowrenewalsonly] [allowkeybasedrenewal]
```

Dove:

- per **addenrollmentserver** è necessario usare un metodo di autenticazione per la connessione client al server di registrazione certificati, tra cui:

  - **Kerberos** usa le credenziali SSL Kerberos.
  
  - **nome utente** utilizza l'account denominato per le credenziali SSL.

  - **ClientCertificate** usa le credenziali SSL del certificato X. 509.

- **allowrenewalsonly** consente solo invii di richieste di rinnovo all'autorità di certificazione tramite l'URL.

- **allowkeybasedrenewal** consente l'uso di un certificato senza account associato in Active Directory. Questo vale quando viene usato con la modalità **ClientCertificate** e **allowrenewalsonly** .

```
[-config Machine\CAName]
```

### <a name="-deleteenrollmentserver"></a>-deleteenrollmentserver

Elimina un'applicazione del server di iscrizione e un pool di applicazioni, se necessario, per l'autorità di certificazione specificata. Questo comando non installa i file binari o i pacchetti.

```
certutil [options] -deleteenrollmentserver kerberos | username | clientcertificate
```

Dove:

- per **deleteenrollmentserver** è necessario usare un metodo di autenticazione per la connessione client al server di registrazione certificati, tra cui:

  - **Kerberos** usa le credenziali SSL Kerberos.
  
  - **nome utente** utilizza l'account denominato per le credenziali SSL.

  - **ClientCertificate** usa le credenziali SSL del certificato X. 509.

```
[-config Machine\CAName]
```

### <a name="-addpolicyserver"></a>-addpolicyserver

Aggiungere un'applicazione server dei criteri e un pool di applicazioni, se necessario. Questo comando non installa i file binari o i pacchetti.

```
certutil [options] -addpolicyserver kerberos | username | clientcertificate [keybasedrenewal]
```

Dove:

- per **addpolicyserver** è necessario usare un metodo di autenticazione per la connessione client al server dei criteri di certificato, tra cui:

  - **Kerberos** usa le credenziali SSL Kerberos.
  
  - **nome utente** utilizza l'account denominato per le credenziali SSL.

  - **ClientCertificate** usa le credenziali SSL del certificato X. 509.

- **keybasedrenewal** consente l'uso dei criteri restituiti al client che contiene i modelli di keybasedrenewal. Questa opzione è valida solo per il **nome utente** e l'autenticazione **ClientCertificate** .

### <a name="-deletepolicyserver"></a>-deletepolicyserver

Elimina un'applicazione server dei criteri e un pool di applicazioni, se necessario. Questo comando non rimuove i file binari o i pacchetti.

certutil [opzioni]-deletePolicyServer Kerberos | nome utente | ClientCertificate [keybasedrenewal]

Dove:

- per **deletepolicyserver** è necessario usare un metodo di autenticazione per la connessione client al server dei criteri di certificato, tra cui:

  - **Kerberos** usa le credenziali SSL Kerberos.
  
  - **nome utente** utilizza l'account denominato per le credenziali SSL.

  - **ClientCertificate** usa le credenziali SSL del certificato X. 509.

- **keybasedrenewal** consente l'uso di un server dei criteri di keybasedrenewal.

### <a name="-oid"></a>-OID

Consente di visualizzare l'identificatore dell'oggetto o di impostare un nome visualizzato.

```
certutil [options] -oid objectID [displayname | delete [languageID [type]]]
certutil [options] -oid groupID
certutil [options] -oid agID | algorithmname [groupID]
```

Dove:

- **ObjectID** Visualizza o per aggiungere il nome visualizzato.

- **GroupID** è il numero GroupID (decimale) enumerato da ObjectId.

- **algido** è l'ID esadecimale ricercato da ObjectId.

- **algorithmName** è il nome dell'algoritmo ricercato da ObjectId.

- **DisplayName** consente di visualizzare il nome da archiviare in DS.

- **Elimina** consente di eliminare il nome visualizzato.

- **LanguageID** è il valore ID della lingua (l'impostazione predefinita è current: 1033).

- **Tipo** è il tipo di oggetto DS da creare, tra cui:

  - `1`-Template (impostazione predefinita)
  
  - `2`-Criterio di rilascio

  - `3`-Criteri di applicazione

- `-f`Crea un oggetto DS.

### <a name="-error"></a>-errore

Consente di visualizzare il testo del messaggio associato a un codice di errore.

```
certutil [options] -error errorcode
```

### <a name="-getreg"></a>-getreg

Visualizza un valore del registro di sistema.

```
certutil [options] -getreg [{ca | restore | policy | exit | template | enroll |chain | policyservers}\[progID\]][registryvaluename]
```

Dove:

- la **CA** usa una chiave del registro di sistema dell'autorità di certificazione.

- il **ripristino** usa la chiave del registro di sistema Restore dell'autorità di certificazione.

- il **criterio** usa la chiave del registro di sistema del modulo criteri.

- **Exit** usa la chiave del registro di sistema del primo modulo di uscita.

- il **modello** usa la chiave del registro di `-user` sistema template (usare per i modelli utente).

- la **registrazione** usa la chiave del registro di sistema di `-user` registrazione (usare per il contesto utente).

- **Chain** usa la chiave del registro di sistema Chain Configuration.

- **policyservers** utilizza la chiave del registro di sistema server dei criteri.

- **ProgID** usa il ProgID del modulo dei criteri o di uscita (nome della sottochiave del registro di sistema).

- **RegistryValueName** usa il nome del valore del registro `Name*` di sistema (usare per trovare la corrispondenza con prefisso).

- **value** utilizza il nuovo valore del registro di sistema numeric, String o date o filename. Se un valore numerico inizia con `+` o `-`, i bit specificati nel nuovo valore vengono impostati o cancellati nel valore del registro di sistema esistente.

```
[-f] [-user] [-grouppolicy] [-config Machine\CAName]
```

#### <a name="remarks"></a>Osservazioni

- Se un valore stringa inizia con `+` o `-`e il valore esistente è un `REG_MULTI_SZ` valore, la stringa viene aggiunta o rimossa dal valore del registro di sistema esistente. Per forzare la creazione `REG_MULTI_SZ` di un valore `\n` , aggiungere alla fine del valore di stringa.

- Se il valore inizia con `\@`, il resto del valore è il nome del file che contiene la rappresentazione in formato testo esadecimale di un valore binario. Se non fa riferimento a un file valido, viene invece analizzato come `[Date][+|-][dd:hh]` una data facoltativa più o meno giorni e ore facoltativi. Se vengono specificati entrambi, usare un separatore di segno più (+) o meno (-). Utilizzare `now+dd:hh` per una data relativa all'ora corrente.

- Utilizzare `chain\chaincacheresyncfiletime \@now` per scaricare effettivamente i CRL memorizzati nella cache.

### <a name="-setreg"></a>-setreg

Imposta un valore del registro di sistema.

```
certutil [options] -setreg [{ca | restore | policy | exit | template | enroll |chain | policyservers}\[progID\]]registryvaluename value
```

Dove:

- la **CA** usa una chiave del registro di sistema dell'autorità di certificazione.

- il **ripristino** usa la chiave del registro di sistema Restore dell'autorità di certificazione.

- il **criterio** usa la chiave del registro di sistema del modulo criteri.

- **Exit** usa la chiave del registro di sistema del primo modulo di uscita.

- il **modello** usa la chiave del registro di `-user` sistema template (usare per i modelli utente).

- la **registrazione** usa la chiave del registro di sistema di `-user` registrazione (usare per il contesto utente).

- **Chain** usa la chiave del registro di sistema Chain Configuration.

- **policyservers** utilizza la chiave del registro di sistema server dei criteri.

- **ProgID** usa il ProgID del modulo dei criteri o di uscita (nome della sottochiave del registro di sistema).

- **RegistryValueName** usa il nome del valore del registro `Name*` di sistema (usare per trovare la corrispondenza con prefisso).

- **value** utilizza il nuovo valore del registro di sistema numeric, String o date o filename. Se un valore numerico inizia con `+` o `-`, i bit specificati nel nuovo valore vengono impostati o cancellati nel valore del registro di sistema esistente.

```
[-f] [-user] [-grouppolicy] [-config Machine\CAName]
```

#### <a name="remarks"></a>Osservazioni

- Se un valore stringa inizia con `+` o `-`e il valore esistente è un `REG_MULTI_SZ` valore, la stringa viene aggiunta o rimossa dal valore del registro di sistema esistente. Per forzare la creazione `REG_MULTI_SZ` di un valore `\n` , aggiungere alla fine del valore di stringa.

- Se il valore inizia con `\@`, il resto del valore è il nome del file che contiene la rappresentazione in formato testo esadecimale di un valore binario. Se non fa riferimento a un file valido, viene invece analizzato come `[Date][+|-][dd:hh]` una data facoltativa più o meno giorni e ore facoltativi. Se vengono specificati entrambi, usare un separatore di segno più (+) o meno (-). Utilizzare `now+dd:hh` per una data relativa all'ora corrente.

- Utilizzare `chain\chaincacheresyncfiletime \@now` per scaricare effettivamente i CRL memorizzati nella cache.

### <a name="-delreg"></a>-delreg

Elimina un valore del registro di sistema.

```
certutil [options] -delreg [{ca | restore | policy | exit | template | enroll |chain | policyservers}\[progID\]][registryvaluename]
```

Dove:

- la **CA** usa una chiave del registro di sistema dell'autorità di certificazione.

- il **ripristino** usa la chiave del registro di sistema Restore dell'autorità di certificazione.

- il **criterio** usa la chiave del registro di sistema del modulo criteri.

- **Exit** usa la chiave del registro di sistema del primo modulo di uscita.

- il **modello** usa la chiave del registro di `-user` sistema template (usare per i modelli utente).

- la **registrazione** usa la chiave del registro di sistema di `-user` registrazione (usare per il contesto utente).

- **Chain** usa la chiave del registro di sistema Chain Configuration.

- **policyservers** utilizza la chiave del registro di sistema server dei criteri.

- **ProgID** usa il ProgID del modulo dei criteri o di uscita (nome della sottochiave del registro di sistema).

- **RegistryValueName** usa il nome del valore del registro `Name*` di sistema (usare per trovare la corrispondenza con prefisso).

- **value** utilizza il nuovo valore del registro di sistema numeric, String o date o filename. Se un valore numerico inizia con `+` o `-`, i bit specificati nel nuovo valore vengono impostati o cancellati nel valore del registro di sistema esistente.

```
[-f] [-user] [-grouppolicy] [-config Machine\CAName]
```

#### <a name="remarks"></a>Osservazioni

- Se un valore stringa inizia con `+` o `-`e il valore esistente è un `REG_MULTI_SZ` valore, la stringa viene aggiunta o rimossa dal valore del registro di sistema esistente. Per forzare la creazione `REG_MULTI_SZ` di un valore `\n` , aggiungere alla fine del valore di stringa.

- Se il valore inizia con `\@`, il resto del valore è il nome del file che contiene la rappresentazione in formato testo esadecimale di un valore binario. Se non fa riferimento a un file valido, viene invece analizzato come `[Date][+|-][dd:hh]` una data facoltativa più o meno giorni e ore facoltativi. Se vengono specificati entrambi, usare un separatore di segno più (+) o meno (-). Utilizzare `now+dd:hh` per una data relativa all'ora corrente.

- Utilizzare `chain\chaincacheresyncfiletime \@now` per scaricare effettivamente i CRL memorizzati nella cache.

### <a name="-importkms"></a>-importKMS

Importa le chiavi utente e i certificati nel database del server per l'archiviazione delle chiavi.

```
certutil [options] -importKMS userkeyandcertfile [certID]
```

Dove:

- **userkeyandcertfile** è un file di dati con chiavi private utente e certificati da archiviare. Il file può essere:

  - Un file di esportazione del server di gestione delle chiavi (KMS) di Exchange.

  - Un file PFX.

- certID è un token di corrispondenza del certificato di decrittografia del file di esportazione del KMS. Per altre informazioni, vedere il `-store` parametro in questo articolo.

- `-f`Importa i certificati non rilasciati dall'autorità di certificazione.

```
[-f] [-silent] [-split] [-config Machine\CAName] [-p password] [-symkeyalg symmetrickeyalgorithm[,keylength]]
```

### <a name="-importcert"></a>-importcert

Importa un file di certificato nel database.

```
certutil [options] -importcert certfile [existingrow]
```

Dove:

- **existingrow** importa il certificato al posto di una richiesta in sospeso per la stessa chiave.

- `-f`Importa i certificati non rilasciati dall'autorità di certificazione.

```
[-f] [-config Machine\CAName]
```

#### <a name="remarks"></a>Osservazioni

Potrebbe inoltre essere necessario configurare l'autorità di certificazione per supportare i certificati esterni. A tale scopo, digitare `import - certutil -setreg ca\KRAFlags +KRAF_ENABLEFOREIGN`.

### <a name="-getkey"></a>-GetKey

Recupera un BLOB di recupero della chiave privata archiviato, genera uno script di ripristino o Ripristina le chiavi archiviate.

```
certutil [options] -getkey searchtoken [recoverybloboutfile]
certutil [options] -getkey searchtoken script outputscriptfile
certutil [options] -getkey searchtoken retrieve | recover outputfilebasename
```

Dove:

- **script** genera uno script per recuperare e recuperare le chiavi (comportamento predefinito se vengono trovati più candidati di recupero corrispondenti o se il file di output non è specificato).

- il **recupero recupera uno** o più BLOB di recupero chiave (comportamento predefinito se viene trovato esattamente un candidato di recupero corrispondente e se viene specificato il file di output). L'uso di questa opzione tronca qualsiasi estensione e aggiunge la stringa specifica del certificato e l'estensione REC per ogni BLOB di recupero della chiave.  Ogni file contiene una catena di certificati e una chiave privata associata, ancora crittografati a uno o più certificati dell'agente di recupero chiavi.

- il **ripristino** recupera e ripristina le chiavi private in un unico passaggio (richiede certificati dell'agente di recupero chiavi e chiavi private). Se si usa questa opzione, viene troncata un'estensione e viene aggiunta l'estensione P12.  Ogni file contiene le catene di certificati ripristinati e le chiavi private associate, archiviate come file PFX.

- **searchtoken** seleziona le chiavi e i certificati da ripristinare, tra cui:

  - 1. Nome comune certificato
  
  - 2. Numero di serie del certificato
  
  - 3. Hash SHA-1 certificato (identificazione personale)
  
  - 4. Hash SHA-1 del certificato KeyId (identificatore della chiave del soggetto)
  
  - 5. Nome del richiedente (dominio\utente)
  
  - 6. UPN (dominio\@utente)

- **recoverybloboutfile** restituisce un file con una catena di certificati e una chiave privata associata, ancora crittografati in uno o più certificati dell'agente di recupero chiavi.

- **outputScriptFile** genera un file con uno script batch per recuperare e recuperare le chiavi private.

- **outputfilebasename** restituisce un nome di base del file.

```
[-f] [-unicodetext] [-silent] [-config Machine\CAName] [-p password] [-protectto SAMnameandSIDlist] [-csp provider]
```

### <a name="-recoverkey"></a>-recoverkey

Ripristinare una chiave privata archiviata.

```
certutil [options] -recoverkey recoveryblobinfile [PFXoutfile [recipientindex]]
```

```
[-f] [-user] [-silent] [-split] [-p password] [-protectto SAMnameandSIDlist] [-csp provider] [-t timeout]
```

### <a name="-mergepfx"></a>-mergePFX

Unisce i file PFX.

```
certutil [options] -mergePFX PFXinfilelist PFXoutfile [extendedproperties]
```

Dove:

- **ElencoFileInPFX** è un elenco delimitato da virgole di file di input PFX.

- **PFXoutfile** è il nome del file di output PFX.

- in **ExtendedProperties** sono incluse eventuali proprietà estese.

```
[-f] [-user] [-split] [-p password] [-protectto SAMnameAndSIDlist] [-csp provider]
```

#### <a name="remarks"></a>Osservazioni

- La password specificata nella riga di comando deve essere un elenco di password separate da virgole.

- Se viene specificata più di una password, per il file di output viene utilizzata l'ultima password. Se viene fornita una sola password o se l'ultima password è `*`, all'utente verrà richiesto di immettere la password del file di output.

### <a name="-convertepf"></a>-convertEPF

Converte un file PFX in un file EPF.

```
certutil [options] -convertEPF PFXinfilelist PFXoutfile [cast | cast-] [V3CAcertID][,salt]
```

Dove:


- **ElencoFileInPFX** è un elenco delimitato da virgole di file di input PFX.

- **PFXoutfile** è il nome del file di output PFX.

- **EPF** è il nome del file di output di EPF.

- **cast** usa la crittografia 64 di cast.

- **Cast:** usa la crittografia 64 (esportazione)

- **V3CAcertID** è il token di corrispondenza del certificato CA V3. Per altre informazioni, vedere il `-store` parametro in questo articolo.

- **Salt** è la stringa Salt del file di output EPF.

```
[-f] [-silent] [-split] [-dc DCName] [-p password] [-csp provider]
```

#### <a name="remarks"></a>Osservazioni

- La password specificata nella riga di comando deve essere un elenco di password separate da virgole.

- Se viene specificata più di una password, per il file di output viene utilizzata l'ultima password. Se viene fornita una sola password o se l'ultima password è `*`, all'utente verrà richiesto di immettere la password del file di output.

### <a name="-"></a>-?

Visualizza l'elenco dei parametri.

``` 
certutil -?
certutil <name_of_parameter> -?
certutil -? -v
```

Dove:

- **-?** Visualizza l'elenco completo dei parametri

- **-`<name_of_parameter>` -?** Visualizza il contenuto della Guida per il parametro specificato.

- **-?-v** Visualizza un elenco completo dei parametri e delle opzioni.

## <a name="options"></a>Opzioni

In questa sezione vengono definite tutte le opzioni che è possibile specificare, in base al comando. Ogni parametro include informazioni sulle opzioni valide per l'utilizzo.

| Opzioni | Descrizione |
| ------- | ----------- |
| -nullsign | Usare l'hash dei dati come firma. |
| -f | Forza sovrascrittura. |
| -enterprise | Usare l'archivio certificati del registro di sistema dell'organizzazione locale del computer. |
| -utente | Usare le chiavi HKEY_CURRENT_USER o l'archivio certificati. |
| -GroupPolicy | Utilizzare l'archivio certificati criteri di gruppo. |
| -UT | Visualizzare i modelli utente. |
| -Mt | Visualizzare i modelli di computer. |
| -Unicode | Scrivi output reindirizzato in Unicode. |
| -UnicodeText | Scrivere il file di output in formato Unicode. |
| -ora di Greenwich | Visualizza orari con GMT. |
| -secondi | Visualizza gli orari utilizzando secondi e millisecondi. |
| -invisibile all'utente | Usare il `silent` flag per acquisire il contesto di crittografia. |
| -Divisione | Suddividere gli elementi ASN. 1 incorporati e salvarli in file. |
| -v | Fornire informazioni dettagliate (dettagliato). |
| -PrivateKey | Visualizzare i dati della password e della chiave privata. |
| -pin pin | PIN della smart card. |
| -UrlFetch | Recuperare e verificare i certificati AIA e i CRL CDP. |
| -config Machine\CAName | Autorità di certificazione e stringa nome computer. |
| -policyserver URLorID | ID o URL del server dei criteri. Per la selezione di U/I `-policyserver`, usare. Per tutti i server dei criteri, utilizzare`-policyserver *`|
| -Anonimo | Usare credenziali SSL anonime. |
| -Kerberos | Utilizzare le credenziali SSL Kerberos. |
| -ClientCertificate clientcertID | Usare le credenziali SSL del certificato X. 509. Per la selezione di U/I `-clientcertificate`, usare. |
| -username nomeutente | Usare l'account denominato per le credenziali SSL. Per la selezione di U/I `-username`, usare. |
| -CERT certID | Certificato di firma. |
| -DC DCName | Specificare come destinazione un controller di dominio specifico. |
| -limita restrizione | Elenco di restrizioni separate da virgole. Ogni restrizione è costituita da un nome di colonna, un operatore relazionale e un numero intero costante, una stringa o una data. Un nome di colonna può essere preceduto da un segno più o meno per indicare l'ordinamento. Ad esempio: `requestID = 47`, `+requestername >= a, requestername` o `-requestername > DOMAIN, Disposition = 21` |
| -out (istogramma) | Elenco di colonne delimitate da virgole. |
| -p password | Password |
| -protectto SAMnameandSIDlist | Elenco di SID o nomi SAM delimitati da virgole. |
| -provider CSP | Provider |
| -t timeout | Timeout di recupero URL in millisecondi. |
| -symkeyalg symmetrickeyalgorithm [, lunghezza] | Nome dell'algoritmo di chiave simmetrica con lunghezza della chiave facoltativa. Ad esempio: `AES,128` o`3DES` |

### <a name="additional-references"></a>Riferimenti aggiuntivi

Per altri esempi sull'uso di questo comando, vedere

- [Esempi di certutil per la gestione di Servizi certificati Active Directory (AD CS) dalla riga di comando](https://social.technet.microsoft.com/wiki/contents/articles/3063.certutil-examples-for-managing-active-directory-certificate-services-ad-cs-from-the-command-line.aspx)

- [Attività certutil per la gestione dei certificati](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc772898(v=ws.10))

- [Esportazione della richiesta binaria tramite la procedura dettagliata dello strumento da riga di comando certutil. exe](https://social.technet.microsoft.com/wiki/contents/articles/7573.active-directory-certificate-services-pki-key-archival-and-management.aspx)

- [Rinnovo del certificato CA radice](https://social.technet.microsoft.com/wiki/contents/articles/2016.root-ca-certificate-renewal.aspx)

- [certutil (comando)](certutil.md)
