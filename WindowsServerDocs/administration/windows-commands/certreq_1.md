---
title: certreq
description: Argomento di riferimento per il comando certreq, che richiede certificati da un'autorità di certificazione (CA), recupera una risposta a una richiesta precedente da una CA, crea una nuova richiesta da un file con estensione inf, accetta e installa una risposta a una richiesta, costruisce una richiesta di certificazione incrociata o di subordinazione qualificata da una richiesta o da un certificato CA esistente e firma una richiesta di certificazione incrociata o di
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7a04e51f-f395-4bff-b57a-0e9efcadf973
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 14fc717ad49a676387206692af32842f212c4296
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719651"
---
# <a name="certreq"></a>certreq

Il comando certreq può essere usato per richiedere certificati da un'autorità di certificazione (CA), per recuperare una risposta a una richiesta precedente da una CA, per creare una nuova richiesta da un file inf, per accettare e installare una risposta a una richiesta, per costruire una richiesta di certificazione incrociata o di subordinazione qualificata da una richiesta o da un certificato CA esistente e per firmare una richiesta di certificazione incrociata o di subordinazione qualificata.

> [!IMPORTANT]
> Le versioni precedenti del comando certreq potrebbero non fornire tutte le opzioni descritte di seguito. Per visualizzare le opzioni supportate in base a versioni specifiche di certreq, eseguire l'opzione della guida della riga `certreq -v -?`di comando.
>
> Il comando certreq non supporta la creazione di una nuova richiesta di certificato basata su un modello di attestazione chiave in un ambiente CEP/CES.

> [!WARNING]
> Il contenuto di questo argomento è basato sulle impostazioni predefinite per Windows Server; ad esempio, impostando la lunghezza della chiave su 2048, selezionando il provider di archiviazione chiavi del software Microsoft come CSP e usando Secure Hash Algorithm 1 (SHA1). Valutare queste selezioni in base ai requisiti dei criteri di sicurezza della società.

## <a name="syntax"></a>Sintassi

```
certreq [-submit] [options] [requestfilein [certfileout [certchainfileout [fullresponsefileOut]]]]
certreq -retrieve [options] requestid [certfileout [certchainfileout [fullresponsefileOut]]]
certreq -new [options] [policyfilein [requestfileout]]
certreq -accept [options] [certchainfilein | fullresponsefilein | certfilein]
certreq -sign [options] [requestfilein [requestfileout]]
certreq –enroll [options] templatename
certreq –enroll –cert certId [options] renew [reusekeys]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------- | ----------- |
| -Invia | Invia una richiesta a un'autorità di certificazione. |
| -recupero`<requestid>` | Recupera una risposta a una richiesta precedente da un'autorità di certificazione. |
| -nuovo | Crea una nuova richiesta da un file con estensione inf. |
| -accetta | Accetta e installa una risposta a una richiesta di certificato. |
| -criterio | Imposta i criteri per una richiesta. |
| -firma | Firma una richiesta di certificazione incrociata o di subordinazione qualificata. |
| -registrazione | Registra o rinnova un certificato. |
| -? | Visualizza un elenco di sintassi, opzioni e descrizioni di certreq. |
| `<parameter>` -? | Visualizza la guida per il parametro specificato. |
| -v-? | Visualizza un elenco dettagliato della sintassi, delle opzioni e delle descrizioni di certreq. |

## <a name="examples"></a>Esempi

### <a name="certreq--submit"></a>certreq-Invia

Per inviare una richiesta di certificato semplice:

```
certreq –submit certrequest.req certnew.cer certnew.pfx
```

#### <a name="remarks"></a>Osservazioni

- Si tratta del parametro predefinito certreq. exe. Se non viene specificata alcuna opzione al prompt della riga di comando, certreq. exe tenta di inviare una richiesta di certificato a un'autorità di certificazione. Quando si usa l'opzione **– Submit** , è necessario specificare un file di richiesta di certificato. Se questo parametro viene omesso, viene visualizzata la finestra **Apri file** comune, che consente di selezionare il file di richiesta di certificato appropriato.

- Per richiedere un certificato specificando l'attributo SAN, vedere la sezione *come utilizzare l'utilità Certreq. exe per creare e inviare una richiesta di certificato* dell'articolo 931351 della Microsoft Knowledge base [come aggiungere un nome alternativo del soggetto a un certificato LDAP sicuro](https://support.microsoft.com/kb/931351).

### <a name="certreq--retrieve"></a>certreq-recupera

Per recuperare l'ID certificato 20 e creare un file di certificato (. cer), denominato il *certificato*:

```
certreq -retrieve 20 MyCertificate.cer
```

#### <a name="remarks"></a>Osservazioni

- Usare certreq-retrieve *RequestId* per recuperare il certificato dopo che è stato emesso dall'autorità di certificazione. La PKC *RequestId* può essere un valore decimale o esadecimale con prefisso 0x e può essere un numero di serie del certificato senza prefisso 0x. È anche possibile usarlo per recuperare tutti i certificati emessi dall'autorità di certificazione, inclusi i certificati revocati o scaduti, a prescindere dal fatto che la richiesta del certificato sia sempre nello stato in sospeso.

- Se si invia una richiesta all'autorità di certificazione, il modulo criteri dell'autorità di certificazione potrebbe lasciare la richiesta in uno stato in sospeso e restituire il *RequestId* al chiamante Certreq per la visualizzazione. Infine, l'amministratore dell'autorità di certificazione emetterà il certificato o rifiuterà la richiesta.

### <a name="certreq--new"></a>certreq-nuovo

Per creare una nuova richiesta:

```
[newrequest]
; At least one value must be set in this section
subject = CN=W2K8-BO-DC.contoso2.com
```

Di seguito sono riportate alcune delle possibili sezioni che possono essere aggiunte al file INF:

#### <a name="newrequest"></a>[newrequest]

Questa area del file INF è obbligatoria per tutti i nuovi modelli di richiesta di certificato e deve includere almeno un parametro con un valore.

| Chiave<sup>1</sup> | Descrizione | Valore<sup>2</sup> | Esempio |
| --- | ---------- | ----- | ------- |
| Oggetto | Diverse app si basano sulle informazioni sul soggetto in un certificato. È consigliabile specificare un valore per questa chiave. Se l'oggetto non è impostato qui, è consigliabile includere un nome soggetto come parte dell'estensione del certificato nome alternativo oggetto. | Valori stringa del nome distinto relativo | Subject = CN = computer1. contoso. com Subject = CN = John Smith, CN = Users, DC = contoso, DC = com |
| Exportable | Se impostato su TRUE, la chiave privata può essere esportata con il certificato. Per garantire un livello elevato di sicurezza, le chiavi private non devono essere esportabili. in alcuni casi, tuttavia, potrebbe essere necessario se più computer o utenti devono condividere la stessa chiave privata. | `true | false` | `Exportable = TRUE`. Le chiavi CNG possono distinguere tra questo e il testo non crittografato esportabile. Le chiavi CAPI1 non possono. |
| ExportableEncrypted | Specifica se la chiave privata deve essere impostata per essere esportabile. | `true | false` | `ExportableEncrypted = true`<p>**Suggerimento:** Non tutti gli algoritmi e le dimensioni delle chiavi pubbliche funzioneranno con tutti gli algoritmi hash. Il CSP specificato deve supportare anche l'algoritmo hash specificato. Per visualizzare l'elenco degli algoritmi hash supportati, è possibile eseguire il comando:`certutil -oid 1 | findstr pwszCNGAlgid | findstr /v CryptOIDInfo` |
| HashAlgorithm | Algoritmo hash da utilizzare per la richiesta. | `Sha256, sha384, sha512, sha1, md5, md4, md2` | `HashAlgorithm = sha1`. Per visualizzare l'elenco degli algoritmi hash supportati, usare certutil-oid 1 | pwszCNGAlgid findstr | findstr/v CryptOIDInfo|
| KeyAlgorithm| Algoritmo che verrà utilizzato dal provider di servizi per generare una coppia di chiavi pubblica e privata.| `RSA, DH, DSA, ECDH_P256, ECDH_P521, ECDSA_P256, ECDSA_P384, ECDSA_P521` | `KeyAlgorithm = RSA` |
| KeyContainer | Non è consigliabile impostare questo parametro per le nuove richieste in cui viene generato un nuovo materiale della chiave. Il contenitore di chiavi viene generato e gestito automaticamente dal sistema.<p>Per le richieste in cui deve essere usato il materiale della chiave esistente, questo valore può essere impostato sul nome del contenitore delle chiavi esistente. Usare il `certutil –key` comando per visualizzare l'elenco dei contenitori di chiavi disponibili per il contesto del computer. Usare il `certutil –key –user` comando per il contesto dell'utente corrente.| Valore stringa casuale<p>**Suggerimento:** Utilizzare le virgolette doppie intorno a qualsiasi valore di chiave INF con spazi vuoti o caratteri speciali per evitare potenziali problemi di analisi INF. | `KeyContainer = {C347BD28-7F69-4090-AA16-BC58CF4D749C}` |
| KeyLength | Definisce la lunghezza della chiave pubblica e privata. La lunghezza della chiave ha un effetto sul livello di sicurezza del certificato. Una lunghezza della chiave maggiore in genere fornisce un livello di sicurezza più elevato; Tuttavia, alcune applicazioni possono avere limitazioni relative alla lunghezza della chiave. | Qualsiasi lunghezza di chiave valida supportata dal provider del servizio di crittografia. | `KeyLength = 2048` |
| KeySpec | Determina se la chiave può essere utilizzata per le firme, per Exchange (crittografia) o per entrambe. | `AT_NONE, AT_SIGNATURE, AT_KEYEXCHANGE` | `KeySpec = AT_KEYEXCHANGE` |
| KeyUsage | Definisce la chiave del certificato da utilizzare per. | <ul><li>`CERT_DIGITAL_SIGNATURE_KEY_USAGE -- 80 (128)`</li><li>`CERT_NON_REPUDIATION_KEY_USAGE -- 40 (64)`</li><li>`CERT_KEY_ENCIPHERMENT_KEY_USAGE -- 20 (32)`</li><li>`CERT_DATA_ENCIPHERMENT_KEY_USAGE -- 10 (16)`</li><li>`CERT_KEY_AGREEMENT_KEY_USAGE -- 8`</li><li>`CERT_KEY_CERT_SIGN_KEY_USAGE -- 4`</li><li>`CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_ENCIPHER_ONLY_KEY_USAGE -- 1`</li><li>`CERT_DECIPHER_ONLY_KEY_USAGE -- 8000 (32768)`</li></ul> | `KeyUsage = CERT_DIGITAL_SIGNATURE_KEY_USAGE | CERT_KEY_ENCIPHERMENT_KEY_USAGE`<p>**Suggerimento:** Per più valori viene utilizzata una pipe (|) separatore di simboli. Assicurarsi di usare le virgolette doppie quando si usano più valori per evitare problemi di analisi INF. I valori visualizzati sono valori esadecimali (decimali) per ogni definizione di bit. È inoltre possibile utilizzare una sintassi precedente, ovvero un singolo valore esadecimale con più bit impostati anziché la rappresentazione simbolica. Ad esempio: `KeyUsage = 0xa0`. |
| KeyUsageProperty | Recupera un valore che identifica lo scopo specifico per il quale è possibile utilizzare una chiave privata. | <ul><li>`NCRYPT_ALLOW_DECRYPT_FLAG -- 1`</li><li>`NCRYPT_ALLOW_SIGNING_FLAG -- 2`</li><li>`NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4`</li><li>`NCRYPT_ALLOW_ALL_USAGES -- ffffff (16777215)`</li></ul> | `KeyUsageProperty = NCRYPT_ALLOW_DECRYPT_FLAG | NCRYPT_ALLOW_SIGNING_FLAG` |
| MachineKeySet | Questa chiave è importante quando è necessario creare certificati di proprietà del computer e non di un utente. Il materiale della chiave generato viene mantenuto nel contesto di sicurezza dell'entità di sicurezza (utente o account computer) che ha creato la richiesta. Quando un amministratore crea una richiesta di certificato per conto di un computer, il materiale della chiave deve essere creato nel contesto di sicurezza del computer e non nel contesto di sicurezza dell'amministratore. In caso contrario, il computer non è riuscito ad accedere alla chiave privata perché si trova nel contesto di sicurezza dell'amministratore. | `true | false`. Il valore predefinito è false. | `MachineKeySet = true` |
| NotBefore | Specifica una data o una data e un'ora prima delle quali non è possibile emettere la richiesta. `NotBefore`può essere usato con `ValidityPeriod` e `ValidityPeriodUnits`. | Data, data e ora | `NotBefore = 7/24/2012 10:31 AM`<p>**Suggerimento:** `NotBefore` e `NotAfter` sono solo per`equestType=cert` R. L'analisi della data tenta di essere dipendente dalle impostazioni locali. L'utilizzo dei nomi dei mesi eliminerà le ambiguità e funzionerà in tutte le impostazioni locali. |
| NotAfter | Specifica una data o una data e un'ora dopo le quali non è possibile emettere la richiesta. `NotAfter`non può essere usato `ValidityPeriod` con `ValidityPeriodUnits`o. | Data, data e ora | `NotAfter = 9/23/2014 10:31 AM`<p>**Suggerimento:** `NotBefore` e `NotAfter` sono solo `RequestType=cert` per. L'analisi della data tenta di essere dipendente dalle impostazioni locali. L'utilizzo dei nomi dei mesi eliminerà le ambiguità e funzionerà in tutte le impostazioni locali. |
| PrivateKeyArchive | L'impostazione PrivateKeyArchive funziona solo se il RequestType corrispondente è impostato su CMC, perché solo il formato di richiesta dei messaggi di gestione certificati per CMS (CMC) consente di trasferire in modo sicuro la chiave privata del richiedente all'autorità di certificazione per l'archiviazione delle chiavi. | `true | false` | `PrivateKeyArchive = true` |
| EncryptionAlgorithm | Algoritmo di crittografia da utilizzare. | Le opzioni possibili variano a seconda della versione del sistema operativo e del set di provider del servizio di crittografia installati. Per visualizzare l'elenco degli algoritmi disponibili, eseguire il comando: `certutil -oid 2 | findstr pwszCNGAlgid`. Il CSP specificato utilizzato deve supportare anche l'algoritmo di crittografia simmetrica e la lunghezza specificati. | `EncryptionAlgorithm = 3des` |
| EncryptionLength | Lunghezza dell'algoritmo di crittografia da utilizzare. | Lunghezza consentita dal EncryptionAlgorithm specificato. | `EncryptionLength = 128` |
| ProviderName | Il nome del provider è il nome visualizzato del CSP. | Se non si conosce il nome del provider CSP in uso, eseguire `certutil –csplist` da una riga di comando. Il comando visualizzerà i nomi di tutti i CSP disponibili nel sistema locale | `ProviderName = Microsoft RSA SChannel Cryptographic Provider` |
| ProviderType | Il tipo di provider viene utilizzato per selezionare provider specifici in base a funzionalità specifiche dell'algoritmo, ad esempio RSA Full. | Se non si conosce il tipo di provider del CSP in uso, eseguire `certutil –csplist` da un prompt della riga di comando. Il comando visualizzerà il tipo di provider di tutti i CSP disponibili nel sistema locale. | `ProviderType = 1` |
| RenewalCert | Se è necessario rinnovare un certificato presente nel sistema in cui viene generata la richiesta di certificato, è necessario specificare l'hash del certificato come valore per questa chiave. | Hash del certificato di qualsiasi certificato disponibile nel computer in cui viene creata la richiesta di certificato. Se non si conosce l'hash del certificato, utilizzare lo snap-in MMC certificati ed esaminare il certificato da rinnovare. Aprire le proprietà del certificato e vedere `Thumbprint` l'attributo del certificato. Il rinnovo del certificato richiede `PKCS#7` un formato `CMC` di richiesta o. | `RenewalCert = 4EDF274BD2919C6E9EC6A522F0F3B153E9B1582D` |
| RequesterName | Consente di eseguire la registrazione per conto di un'altra richiesta dell'utente. La richiesta deve anche essere firmata con un certificato dell'agente di registrazione o la CA rifiuterà la richiesta. Utilizzare l' `-cert` opzione per specificare il certificato dell'agente di registrazione. Il nome del richiedente può essere specificato per le richieste di `RequestType` certificato se è `PKCS#7` impostato `CMC`su o. Se `RequestType` è impostato su `PKCS#10`, questa chiave verrà ignorata. L' `Requestername` oggetto può essere impostato solo come parte della richiesta. Non è possibile modificare `Requestername` in una richiesta in sospeso. | `Domain\User` | `Requestername = Contoso\BSmith` |
| RequestType | Determina lo standard utilizzato per generare e inviare la richiesta di certificato. | <ul><li>`PKCS10 -- 1`</li><li>`PKCS7 -- 2`</li><li>`CMC -- 3`</li><li>`Cert -- 4`</li><li>`SCEP -- fd00 (64768)`</li></ul>**Suggerimento:** Questa opzione indica un certificato autofirmato o autocertificato. Non genera una richiesta, bensì un nuovo certificato, quindi installa il certificato. Il valore predefinito è autofirmato. Specificare un certificato di firma utilizzando l'opzione – CERT per creare un certificato autocertificato non autofirmato. | `RequestType = CMC` |
| SecurityDescriptor | Contiene le informazioni di sicurezza associate agli oggetti a protezione diretta. Per la maggior parte degli oggetti a protezione diretta, è possibile specificare un descrittore di sicurezza di un oggetto nella chiamata di funzione che crea l'oggetto. Stringhe basate sul [linguaggio di definizione del descrittore di sicurezza](https://msdn.microsoft.com/library/aa379567(v=vs.85).aspx).<p>**Suggerimento:** Questo è pertinente solo per le chiavi non smart card del contesto del computer. | `SecurityDescriptor = D:P(A;;GA;;;SY)(A;;GA;;;BA)` |
| AlternateSignatureAlgorithm | Specifica e recupera un valore booleano che indica se l'identificatore di oggetto (OID) dell'algoritmo di firma per una richiesta PKCS # 10 o una firma del certificato è discreto o combinato. | `true | false` | `AlternateSignatureAlgorithm = false`<p>Per una firma RSA, `false` indica un `Pkcs1 v1.5`oggetto, `true` mentre indica `v2.1` una firma. |
| Invisibile all'utente | Per impostazione predefinita, questa opzione consente al CSP di accedere al desktop utente interattivo e di richiedere informazioni come un PIN della smart card dall'utente. Se questa chiave è impostata su TRUE, il CSP non deve interagire con il desktop e non verrà visualizzata alcuna interfaccia utente per l'utente. | `true | false` | `Silent = true` |
| SMIME | Se questo parametro è impostato su TRUE, alla richiesta verrà aggiunta un'estensione con il valore dell'identificatore di oggetto 1.2.840.113549.1.9.15. Il numero di identificatori di oggetto dipende dalla versione del sistema operativo installata e dalla funzionalità CSP, che fanno riferimento a algoritmi di crittografia simmetrica che possono essere utilizzati da applicazioni Secure Multipurpose Internet Mail Extensions (S/MIME), ad esempio Outlook. | `true | false` | `SMIME = true` |
| UseExistingKeySet | Questo parametro viene usato per specificare che è necessario usare una coppia di chiavi esistente nella compilazione di una richiesta di certificato. Se questa chiave è impostata su TRUE, è necessario specificare anche un valore per la chiave RenewalCert o il nome del contenitore. Non è necessario impostare la chiave esportabile perché non è possibile modificare le proprietà di una chiave esistente. In questo caso, non viene generato alcun materiale della chiave quando viene compilata la richiesta di certificato. | `true | false` | `UseExistingKeySet = true` |
| Protezione di dati | Specifica un valore che indica il modo in cui una chiave privata viene protetta prima dell'utilizzo. | <ul><li>`XCN_NCRYPT_UI_NO_PROTCTION_FLAG -- 0`</li><li>`XCN_NCRYPT_UI_PROTECT_KEY_FLAG -- 1`</li><li>`XCN_NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2`</li></ul> | `KeyProtection = NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG` |
| SuppressDefaults | Specifica un valore booleano che indica se le estensioni e gli attributi predefiniti sono inclusi nella richiesta. Le impostazioni predefinite sono rappresentate dai rispettivi identificatori di oggetto (OID). | `true | false` | `SuppressDefaults = true` |
| FriendlyName | Nome descrittivo per il nuovo certificato. | Testo | `FriendlyName = Server1` |
| ValidityPeriodUnits | Specifica un numero di unità da usare con ValidityPeriod. Nota: questa operazione viene usata solo quando `request type=cert`il. | Numeric | `ValidityPeriodUnits = 3` |
| ValidityPeriod | ValidityPeriod deve essere un periodo di tempo plurale in inglese (Stati Uniti). Nota: questa operazione viene usata solo quando il tipo di richiesta = cert. | `Years |  Months | Weeks | Days | Hours | Minutes | Seconds` | `ValidityPeriod = Years` |

<sup>1</sup> Parametro a sinistra del segno di uguale (=)

<sup>2</sup> Il parametro a destra del segno di uguale (=)

#### <a name="extensions"></a>estensioni

Questa sezione è facoltativa.

| OID estensione | Definizione | Esempio |
| ------------- | ---------- | ----- | ------- |
| 2.5.29.17 | | 2.5.29.17 = {Text} |
| *continuare* | | `continue = UPN=User@Domain.com&` |
| *continuare* | | `continue = EMail=User@Domain.com&` |
| *continuare* | | `continue = DNS=host.domain.com&` |
| *continuare* | | `continue = DirectoryName=CN=Name,DC=Domain,DC=com&` |
| *continuare* | | `continue = URL=<http://host.domain.com/default.html&>` |
| *continuare* | | `continue = IPAddress=10.0.0.1&` |
| *continuare* | | `continue = RegisteredId=1.2.3.4.5&` |
| *continuare* | | `continue = 1.2.3.4.6.1={utf8}String&` |
| *continuare* | | `continue = 1.2.3.4.6.2={octet}AAECAwQFBgc=&` |
| *continuare* | | `continue = 1.2.3.4.6.2={octet}{hex}00 01 02 03 04 05 06 07&` |
| *continuare* | | `continue = 1.2.3.4.6.3={asn}BAgAAQIDBAUGBw==&` |
| *continuare* | | `continue = 1.2.3.4.6.3={hex}04 08 00 01 02 03 04 05 06 07` |
| 2.5.29.37 | | `2.5.29.37={text}` |
| *continuare* | | `continue = 1.3.6.1.5.5.7` |
| *continuare* | | `continue = 1.3.6.1.5.5.7.3.1` |
| 2.5.29.19 | | `{text}ca=0pathlength=3` |
| Critico | | `Critical=2.5.29.19` |
| KeySpec | | <ul><li>`AT_NONE -- 0`</li><li>`AT_SIGNATURE -- 2`</li><li>`AT_KEYEXCHANGE -- 1`</ul></li> |
| RequestType | | <ul><li>`PKCS10 -- 1`</li><li>`PKCS7 -- 2`</li><li>`CMC -- 3`</li><li>`Cert -- 4`</li><li>`SCEP -- fd00 (64768)`</li></ul> |
| KeyUsage | | <ul><li>`CERT_DIGITAL_SIGNATURE_KEY_USAGE -- 80 (128)`</li><li>`CERT_NON_REPUDIATION_KEY_USAGE -- 40 (64)`</li><li>`CERT_KEY_ENCIPHERMENT_KEY_USAGE -- 20 (32)`</li><li>`CERT_DATA_ENCIPHERMENT_KEY_USAGE -- 10 (16)`</li><li>`CERT_KEY_AGREEMENT_KEY_USAGE -- 8`</li><li>`CERT_KEY_CERT_SIGN_KEY_USAGE -- 4`</li><li>`CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_ENCIPHER_ONLY_KEY_USAGE -- 1`</li><li>`CERT_DECIPHER_ONLY_KEY_USAGE -- 8000 (32768)`</li></ul> |
| KeyUsageProperty | | <ul><li>`NCRYPT_ALLOW_DECRYPT_FLAG -- 1`</li><li>`NCRYPT_ALLOW_SIGNING_FLAG -- 2`</li><li>`NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4`</li><li>`NCRYPT_ALLOW_ALL_USAGES -- ffffff (16777215)`</li></ul> |
| Protezione di dati | | <ul><li>`NCRYPT_UI_NO_PROTECTION_FLAG -- 0`</li><li>`NCRYPT_UI_PROTECT_KEY_FLAG -- 1`</li><li>`NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2`</li></ul> |
| SubjectNameFlags | template | <ul><li>`CT_FLAG_SUBJECT_REQUIRE_COMMON_NAME -- 40000000 (1073741824)`</li><li>`CT_FLAG_SUBJECT_REQUIRE_DIRECTORY_PATH -- 80000000 (2147483648)`</li><li>`CT_FLAG_SUBJECT_REQUIRE_DNS_AS_CN -- 10000000 (268435456)`</li><li>`CT_FLAG_SUBJECT_REQUIRE_EMAIL -- 20000000 (536870912)`</li><li>`CT_FLAG_OLD_CERT_SUPPLIES_SUBJECT_AND_ALT_NAME -- 8`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_DIRECTORY_GUID -- 1000000 (16777216)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_DNS -- 8000000 (134217728)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_DOMAIN_DNS -- 400000 (4194304)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_EMAIL -- 4000000 (67108864)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_SPN -- 800000 (8388608)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_UPN -- 2000000 (33554432)`</li></ul> |
| X500NameFlags | | <ul><li>`CERT_NAME_STR_NONE -- 0`</li><li>`CERT_OID_NAME_STR -- 2`</li><li>`CERT_X500_NAME_STR -- 3`</li><li>`CERT_NAME_STR_SEMICOLON_FLAG -- 40000000 (1073741824)`</li><li>`CERT_NAME_STR_NO_PLUS_FLAG -- 20000000 (536870912)`</li><li>`CERT_NAME_STR_NO_QUOTING_FLAG -- 10000000 (268435456)`</li><li>`CERT_NAME_STR_CRLF_FLAG -- 8000000 (134217728)`</li><li>`CERT_NAME_STR_COMMA_FLAG -- 4000000 (67108864)`</li><li>`CERT_NAME_STR_REVERSE_FLAG -- 2000000 (33554432)`</li><li>`CERT_NAME_STR_FORWARD_FLAG -- 1000000 (16777216)`</li><li>`CERT_NAME_STR_DISABLE_IE4_UTF8_FLAG -- 10000 (65536)`</li><li>`CERT_NAME_STR_ENABLE_T61_UNICODE_FLAG -- 20000 (131072)`</li><li>`CERT_NAME_STR_ENABLE_UTF8_UNICODE_FLAG -- 40000 (262144)`</li><li>`CERT_NAME_STR_FORCE_UTF8_DIR_STR_FLAG -- 80000 (524288)`</li><li>`CERT_NAME_STR_DISABLE_UTF8_DIR_STR_FLAG -- 100000 (1048576)`</li><li>`CERT_NAME_STR_ENABLE_PUNYCODE_FLAG -- 200000 (2097152)`</li></ul> |

> [!NOTE]
> `SubjectNameFlags`consente al file INF di specificare quali campi dell'estensione **Subject** e **SubjectAltName** devono essere popolati automaticamente da certreq in base alle proprietà correnti dell'utente o del computer: nome DNS, UPN e così via. L'utilizzo del modello letterale indica che vengono utilizzati i flag del nome del modello. Questo consente l'uso di un singolo file INF in più contesti per generare richieste con informazioni sul soggetto specifiche del contesto.
>
> `X500NameFlags`Specifica i flag da passare direttamente all' `CertStrToName` API quando il `Subject INF keys` valore viene convertito in un **nome**distinto con codifica ASN. 1.

#### <a name="example"></a>Esempio

Per creare un file di criteri (inf) nel blocco note e salvarlo come *RequestConfig. inf*:

```
[NewRequest]
Subject = CN=<FQDN of computer you are creating the certificate>
Exportable = TRUE
KeyLength = 2048
KeySpec = 1
KeyUsage = 0xf0
MachineKeySet = TRUE
[RequestAttributes]
CertificateTemplate=WebServer
[Extensions]
OID = 1.3.6.1.5.5.7.3.1
OID = 1.3.6.1.5.5.7.3.2
```

Nel computer per cui si richiede un certificato:

```
certreq –new requestconfig.inf certrequest.req
```

Per utilizzare la sintassi della sezione [Strings] per OID e altri dati difficili da interpretare. Nuovo esempio di sintassi {text} per l'estensione EKU, che usa un elenco delimitato da virgole di OID:

```
[Version]
Signature=$Windows NT$

[Strings]
szOID_ENHANCED_KEY_USAGE = 2.5.29.37
szOID_PKIX_KP_SERVER_AUTH = 1.3.6.1.5.5.7.3.1
szOID_PKIX_KP_CLIENT_AUTH = 1.3.6.1.5.5.7.3.2

[NewRequest]
Subject = CN=TestSelfSignedCert
Requesttype = Cert

[Extensions]
%szOID_ENHANCED_KEY_USAGE%={text}%szOID_PKIX_KP_SERVER_AUTH%,
_continue_ = %szOID_PKIX_KP_CLIENT_AUTH%
```

### <a name="certreq--accept"></a>certreq-accetta

Il `–accept` parametro collega la chiave privata generata in precedenza con il certificato emesso e rimuove la richiesta di certificato in sospeso dal sistema in cui è richiesto il certificato (se è presente una richiesta corrispondente).

Per accettare manualmente un certificato:

```
certreq -accept certnew.cer
```

> [!WARNING]
> L'utilizzo `-accept` del parametro con `-user` le `–machine` opzioni e indica se il certificato di installazione deve essere installato in un contesto **utente** o **computer** . Se è presente una richiesta in attesa in uno dei due contesti che corrisponde alla chiave pubblica installata, queste opzioni non sono necessarie. Se non sono presenti richieste in attesa, è necessario specificare una di queste.

### <a name="certreq--policy"></a>certreq-criteri

Il file Policy. inf è un file di configurazione che definisce i vincoli applicati a una certificazione della CA, quando viene definita una subordinazione qualificata.

Per compilare una richiesta di certificato incrociato:

```
certreq -policy certsrv.req policy.inf newcertsrv.req
```

Se `certreq -policy` si utilizza senza parametri aggiuntivi, viene visualizzata una finestra di dialogo che consente di selezionare il valore fie (. req,. CMC,. txt,. der,. cer o. CRT) richiesto. Dopo aver selezionato il file richiesto e fatto clic su **Apri**, viene visualizzata un'altra finestra di dialogo che consente di selezionare il file Policy. inf.

#### <a name="examples"></a>Esempi

Trovare un esempio del file Policy. inf nella [sintassi CAPolicy. inf](https://docs.microsoft.com/windows-server/networking/core-network-guide/cncg/server-certs/prepare-the-capolicy-inf-file).

### <a name="certreq--sign"></a>certreq-Sign

Per creare una nuova richiesta di certificato, firmarla e inviarla:

```
certreq -new policyfile.inf myrequest.req
certreq -sign myrequest.req myrequest.req
certreq -submit myrequest_sign.req myrequest_cert.cer
```

#### <a name="remarks"></a>Osservazioni

- Se `certreq -sign` si usa senza parametri aggiuntivi, verrà aperta una finestra di dialogo in cui è possibile selezionare il file richiesto (req, CMC, txt, der, CER o CRT).

- La firma della richiesta di subordinazione qualificata può richiedere credenziali di **amministratore dell'organizzazione** . Si tratta di una procedura consigliata per emettere i certificati di firma per la subordinazione qualificata.

- Il certificato usato per firmare la richiesta di subordinazione qualificata usa il modello di subordinazione qualificato. Gli amministratori dell'organizzazione dovranno firmare la richiesta o concedere le autorizzazioni utente agli utenti che firmano il certificato.

- Potrebbe essere necessario che il personale aggiuntivo firmi la richiesta CMC dopo l'utente. Questo dipenderà dal livello di garanzia associato alla subordinazione qualificata.

- Se la CA padre della CA subordinata che si sta installando è offline, è necessario ottenere il certificato della CA per la CA subordinata qualificata dal padre offline. Se la CA padre è online, specificare il certificato della CA per la CA subordinata qualificata durante l'installazione guidata di **Servizi certificati** .

### <a name="certreq--enroll"></a>certreq-registrazione

È possibile usare questo commento per registrare o rinnovare i certificati.

#### <a name="examples"></a>Esempi

Per registrare un certificato usando il modello *webserver* e selezionando il server dei criteri usando U/I:

```
certreq -enroll –machine –policyserver * WebServer
```

Per rinnovare un certificato utilizzando un numero di serie:

```
certreq –enroll -machine –cert 61 2d 3c fe 00 00 00 00 00 05 renew
```

È possibile rinnovare solo i certificati validi. I certificati scaduti non possono essere rinnovati e devono essere sostituiti con un nuovo certificato.

## <a name="options"></a>Opzioni

| Opzioni | Descrizione |
| ------- | ----------- |
| -any | `Force ICertRequest::Submit`per determinare il tipo di codifica.|
| -attrib`<attributestring>` | Specifica le coppie di stringhe di **nome** e **valore** , separate da due punti.<p>Separa **Name** le coppie di stringhe di nome `\n` e **valore** usando (ad esempio, name1: value1\nName2: value2). |
| -binario | Formatta i file di output come binari anziché con codifica Base64. |
| -policyserver`<policyserver>` | LDAP`<path>`<br>Inserire l'URI o l'ID univoco per un computer in cui è in esecuzione il servizio Web di criteri di registrazione certificati.<p>Per specificare che si desidera utilizzare un file di richiesta esplorando, è sufficiente utilizzare un segno meno (-) per `<policyserver>`. |
| -config`<ConfigString>` | Elabora l'operazione tramite l'autorità di certificazione specificata nella stringa di configurazione, ovvero **CAHostName\CAName**. Per una connessione HTTPS\\: \ specificare l'URI del server di registrazione. Per la CA dell'archivio del computer locale, usare un segno meno (-). |
| -Anonimo | Usare le credenziali anonime per i servizi Web di registrazione certificati. |
| -Kerberos | Usare le credenziali Kerberos (dominio) per i servizi Web di registrazione certificati. |
| -ClientCertificate`<ClientCertId>` | È possibile sostituire con `<ClientCertId>` un'identificazione personale del certificato, un CN, un EKU, un modello, un messaggio di `name=value` posta elettronica, un UPN o la nuova sintassi. |
| -username`<username>` | Utilizzato con i servizi Web di registrazione certificati. È possibile sostituire `<username>` con il nome del Sam o il valore **dominio\utente** . Questa opzione è utilizzabile con l' `-p` opzione. |
| -p`<password>` | Utilizzato con i servizi Web di registrazione certificati. Sostituire `<password>` con la password dell'utente effettivo. Questa opzione è utilizzabile con l' `-username` opzione. |
| -utente | Configura il `-user` contesto per una nuova richiesta di certificato o specifica il contesto per un'accettazione del certificato. Si tratta del contesto predefinito, se non ne viene specificato alcuno nel INF o nel modello. |
| -machine | Configura una nuova richiesta di certificato o specifica il contesto per un'accettazione del certificato per il contesto del computer. Per le nuove richieste è necessario che sia coerente con la chiave INF MachineKeyset e il contesto del modello. Se questa opzione non è specificata e il modello non imposta un contesto, il valore predefinito è il contesto utente. |
| -CRL | Include gli elenchi di revoche di certificati (CRL) nell'output per il file PKCS #7 con codifica Base64 specificato da `certchainfileout` o nel file con codifica Base64 specificato da. `requestfileout` |
| -RPC | Indica a Active Directory Servizi certificati (AD CS) di utilizzare una connessione al server RPC (Remote Procedure Call) invece di COM distribuito. |
| -adminforcemachine | Usare il servizio chiave o la rappresentazione per inviare la richiesta dal contesto del sistema locale. Richiede che l'utente che richiama questa opzione sia un membro del gruppo Administrators locale. |
| -renewonbehalfof | Inviare un rinnovo per conto dell'oggetto identificato nel certificato di firma. Imposta CR_IN_ROBO quando viene chiamato il [Metodo ICertRequest:: Submit](https://docs.microsoft.com/windows/win32/api/certcli/nf-certcli-icertrequest-submit) |
| -f | Forza la sovrascrittura dei file esistenti. Ignora inoltre i modelli e i criteri di memorizzazione nella cache. |
| -Q | Usa modalità invisibile all'utente; non visualizzare tutti i prompt interattivi. |
| -Unicode | Scrive l'output Unicode quando l'output standard viene reindirizzato o reindirizzato a un altro comando, che consente di richiamare gli script di Windows PowerShell. |
| -UnicodeText | Invia l'output Unicode durante la scrittura di BLOB di dati codificati in testo Base64 nei file. |

## <a name="formats"></a>Formati

| Formati | Descrizione |
| ------- | ----------- |
| RequestFileIn | Nome del file di input con codifica Base64 o binario: la richiesta di certificato PKCS #10, la richiesta di certificato CMS, la richiesta di rinnovo del certificato PKCS #7, il certificato X. 509 da certificare o la richiesta di certificato in formato tag KeyGen. |
| RequestFileOut | Nome del file di output con codifica Base64. |
| certfileout | Nome file X-509 con codifica Base64. |
| PKCS10fileout | Da usare solo con `certreq -policy` il parametro. Nome del file di output PKCS10 con codifica Base64. |
| CertChainFileOut | Nome file #7 PKCS con codifica Base64. |
| fullresponsefileout | Nome del file di risposta completo con codifica Base64. |
| PolicyFileIn | Da usare solo con `certreq -policy` il parametro. File INF contenente una rappresentazione testuale delle estensioni utilizzate per qualificare una richiesta. |

## <a name="additional-resources"></a>Risorse aggiuntive

Gli articoli seguenti contengono esempi di utilizzo di certreq:

- [Come aggiungere un nome alternativo del soggetto a un certificato LDAP sicuro](https://support.microsoft.com/help/931351/how-to-add-a-subject-alternative-name-to-a-secure-ldap-certificate)

- [Test Lab Guide: Deploying an AD CS Two-Tier PKI Hierarchy](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831348(v=ws.11))

- [Appendice 3: sintassi di certreq. exe](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc736326(v=ws.10))

- [Come creare manualmente un certificato SSL del server Web](https://techcommunity.microsoft.com/t5/core-infrastructure-and-security/how-to-create-a-web-server-ssl-certificate-manually/ba-p/1128529)

- [Richiedere un certificato di provisioning AMT usando una CA di Windows Server 2008](https://social.technet.microsoft.com/wiki/contents/articles/548.request-an-amt-provisioning-certificate-using-a-windows-server-2008-ca.aspx)

- [Registrazione del certificato per System Center Operations Manager Agent](https://social.technet.microsoft.com/wiki/contents/articles/2017.certificate-enrollment-for-system-center-operations-manager-agent.aspx)

- [Guida dettagliata di Servizi certificati Active Directory: distribuzione della gerarchia PKI a due livelli](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)

- [Come abilitare LDAP su SSL con un'autorità di certificazione di terze parti](https://support.microsoft.com/help/321051/how-to-enable-ldap-over-ssl-with-a-third-party-certification-authority)
