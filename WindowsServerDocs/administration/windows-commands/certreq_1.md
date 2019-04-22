---
title: certreq
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7a04e51f-f395-4bff-b57a-0e9efcadf973
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9e48682cee40c00e187ca674bd8019b136a2c3f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818142"
---
# <a name="certreq"></a>certreq



CertReq può essere usato per richiedere certificati da un'autorità di certificazione (CA), per recuperare una risposta a una precedente richiesta da un'autorità di certificazione, per creare una nuova richiesta da un file. inf, per accettare e installare una risposta a una richiesta, per costruire una certificazione incrociata o richiesta di subordinazione qualificata da una richiesta o il certificato della CA esistente, quindi effettuare una richiesta di certificazione incrociata o di subordinazione qualificata.

> [!WARNING]
> - Le versioni precedenti di certreq potrebbero non fornire tutte le opzioni descritte in questo documento. È possibile visualizzare tutte le opzioni che fornisce una versione specifica di certreq eseguendo i comandi illustrati nella sezione relativa alla sintassi notazioni.
> - CertReq non supporta la creazione di una nuova richiesta di certificato basata su un modello di attestazione chiave in un ambiente di CEP/CES

## <a name="BKMK_Contents"></a>Contenuto

Le sezioni principali in questo articolo sono i seguenti:
1.  [Verbi](#BKMK_Verbs)
2.  [Notazioni di sintassi](#BKMK_notation)
3.  [Opzioni](#BKMK_Options)
4.  [Formati](#BKMK_Formats)
5.  [Esempi aggiuntivi certreq](#BKMK_Examples)

## <a name="BKMK_Verbs"></a>Verbi

Non esiste nella tabella seguente vengono descritti i verbi che possono essere utilizzati con il comando certreq

|Opzione|Descrizione|
|------|-----------|
|-Submit|Invia una richiesta all'autorità di certificazione. Per altre informazioni, vedere [Certreq-submit](#BKMK_Submit).|
|-recuperare *RequestID*|Recupera una risposta a una precedente richiesta da un'autorità di certificazione. Per altre informazioni, vedere [Certreq-recuperare](#BKMK_Retrieve).|
|-Nuovo|Crea una nuova richiesta da un file. inf. Per altre informazioni, vedere [Certreq-nuova](#BKMK_New).|
|-Accept|Accetta e installa una risposta a una richiesta di certificato. Per altre informazioni, vedere [Certreq-accettare](#BKMK_accept).|
|-Policy|Imposta i criteri per una richiesta. Per altre informazioni, vedere [Certreq-criteri](#BKMK_policy).|
|-Sign|Effettua una richiesta di certificazione incrociata o di subordinazione qualificata. Per altre informazioni, vedere [Certreq-sign](#BKMK_sign).|
|-Registrazione|Si registra o rinnova un certificato. Per altre informazioni, vedere [Certreq-registrare](#BKMK_enroll).|
|-?|Visualizza un elenco di descrizioni, opzioni e sintassi certreq.|
|*\<verb>* -?|Visualizza la Guida per il verbo specificato.|
|-v -?|Visualizza un elenco dettagliato di certreq sintassi, opzioni e le descrizioni.|

Tornare a [contenuto](#BKMK_Contents)

## <a name="BKMK_notation"></a>Notazioni di sintassi

-   Per la sintassi della riga di comando di base, eseguire `certreq -?`
-   Per informazioni sulla sintassi sull'uso di certutil con un verbo specifico, eseguire **certreq**  *\<verbo >* **-?**
-   Per l'invio di tutta la sintassi certutil in un file di testo, eseguire i comandi seguenti:  
    -   `certreq -v -? > certreqhelp.txt`
    -   `notepad certreqhelp.txt`

Nella tabella seguente vengono descritti la notazione utilizzata per indicare la sintassi della riga di comando.

|Notazione|Descrizione|
|--------|-----------|
|Testo senza parentesi quadre o parentesi graffe|Elementi che come illustrato, è necessario digitare|
|\<Testo all'interno di parentesi angolari >|Segnaposto per il quale è necessario specificare un valore|
|[Testo all'interno delle parentesi quadre]|Elementi facoltativi|
|{Testo tra parentesi graffe}|Set di elementi richiesti; Scegliere una|
|Barra verticale (&#124;)|Separatore per gli elementi si escludono a vicenda; Scegliere una|
|Puntini di sospensione (…)|Elementi che possono essere ripetuti|

Tornare a [contenuto](#BKMK_Contents)

## <a name="BKMK_Submit"></a>CertReq-submit

Si tratta del parametro di certreq.exe predefinito, se non vengono specificate opzioni in modo esplicito al prompt della riga di comando, certreq.exe tenti di inviare una richiesta di certificato da un'autorità di certificazione.
```
CertReq [-Submit] [Options] [RequestFileIn [CertFileOut [CertChainFileOut [FullResponseFileOut]]]]
```
È necessario specificare un file di richiesta di certificato quando si usa-opzione di invio. Se questo parametro viene omesso, verrà visualizzata una finestra Apri File comune in cui è possibile selezionare il file di richiesta di certificato appropriato.

È possibile usare questi esempi come punto di partenza per creare la richiesta di invio del certificato:

Per inviare una richiesta di certificato semplice usare l'esempio seguente:
```
certreq –submit certRequest.req certnew.cer certnew.pfx
```
Per richiedere un certificato, specificando l'attributo SAN, vedere i passaggi dettagliati nell'articolo della Microsoft Knowledge Base 931351 [come aggiungere un nome alternativo soggetto a un certificato LDAP sicuro](https://support.microsoft.com/kb/931351) nella procedura"usare l'utilità Certreq.exe per sezione "creare e inviare una richiesta di certificato che include una rete SAN.

Tornare a [contenuto](#BKMK_Contents)

## <a name="BKMK_Retrieve"></a>CertReq-recuperare

```
certreq -retrieve [Options] RequestId [CertFileOut [CertChainFileOut [FullResponseFileOut]]]
```
-   Se non si specifica il NomeComputerCA o NomeCA nella finestra di dialogo CAComputerName\CANamea - configurazione viene visualizzata e viene visualizzato un elenco di tutte le CA disponibili.
-   Se si usa - config - invece - config CAComputerName\CAName, l'operazione viene elaborata tramite l'autorità di certificazione predefinita.
-   È possibile usare certreq-recuperare *RequestID* per recuperare il certificato dopo che l'autorità di certificazione è stato rilasciato. Il *RequestID*PKC può essere un numero decimale o esadecimale con 0x prefisso e può essere un numero di serie del certificato senza il prefisso 0x. È anche possibile usarlo per recuperare qualsiasi certificato rilasciato dall'autorità di certificazione, inclusi i certificati revocati o scaduti, indipendentemente dal fatto che la richiesta di certificato era sempre nello stato in sospeso.
-   Se si invia una richiesta all'autorità di certificazione, il modulo criterio dell'autorità di certificazione potrebbe lasciare la richiesta in uno stato in sospeso e restituiscono il *RequestID* al chiamante per la visualizzazione. Infine, l'amministratore della CA verrà emettere il certificato o negare la richiesta.

Il comando seguente recupera l'id certificato 20 e crea il file del certificato (con estensione cer):
```
certreq -retrieve 20 MyCertificate.cer 

```
Tornare a [contenuto](#BKMK_Contents)

## <a name="BKMK_New"></a>CertReq-nuovo

```
certreq -new [Options] [PolicyFileIn [RequestFileOut]]
```
Poiché il file INF consente un set completo di parametri e di specificare opzioni, è difficile da definire un modello predefinito che gli amministratori devono utilizzare per tutti gli scopi. Pertanto, in questa sezione vengono descritte tutte le opzioni che consentono di creare un file INF personalizzate in base a esigenze specifiche. Le parole chiave seguenti vengono usate per descrivere la struttura dei file INF.
1.  Oggetto *sezione* è un'area nel file INF che copre un gruppo logico di chiavi. Una sezione viene sempre visualizzato tra parentesi nel file INF.
2.  Oggetto *chiave* parametro che è a sinistra del segno di uguale.
3.  Oggetto *valore* parametro che a destra del segno di uguale.

Ad esempio, un file INF minimo avrebbe un aspetto simile al seguente:
```
[NewRequest] 
; At least one value must be set in this section 
Subject = "CN=W2K8-BO-DC.contoso2.com"
```
Di seguito sono alcune delle sezioni possibili che possono essere aggiunti al file INF:

**[NewRequest]**

In questa sezione è obbligatoria per un file INF che funge da modello per una nuova richiesta di certificato. In questa sezione è necessaria almeno una chiave con un valore.

|Chiave|Definizione|Value|Esempio|
|---|----------|-----|-------|
|Subject|Molte applicazioni si basano su informazioni del soggetto nel certificato. Di conseguenza, è consigliabile che è possibile specificare un valore per questa chiave. Se l'oggetto non è impostato in questo caso, è consigliabile che un nome soggetto da includere come parte dell'estensione del certificato nome alternativo del soggetto.|Valori di stringa di nome distinto relativi|Subject = Subject "CN=computer1.contoso.com" = "CN = John Smith, CN = Users, DC = Contoso, DC = com"|
|Exportable|Se questo attributo è impostato su TRUE, la chiave privata può essere esportata con il certificato. Per garantire un livello elevato di sicurezza, le chiavi private non devono essere esportabile; Tuttavia, in alcuni casi, si potrebbe essere necessario per rendere la chiave privata esportabile se diversi computer o gli utenti devono condividere la stessa chiave privata.|true, false|Può essere esportata = TRUE. Chiavi CNG in grado di distinguere tra questa e plaintext esportabili. Non è possibile CAPI1 chiavi.|
|ExportableEncrypted|Specifica se la chiave privata deve essere impostata per essere esportabile.|true, false|ExportableEncrypted = true</br>Suggerimento: Non tutte le dimensioni della chiave pubbliche e gli algoritmi funziona con tutti gli algoritmi di hash. Tamehe specificato che CSP deve anche supportare l'algoritmo hash specificato. Per visualizzare l'elenco degli algoritmi hash supportati, è possibile eseguire il comando <code>certutil -oid 1 &#124; findstr pwszCNGAlgid &#124; findstr /v CryptOIDInfo</code>|
|HashAlgorithm|Algoritmo hash da usare per questa richiesta.|Sha256, sha384, sha512, sha1, md5, md4, md2|HashAlgorithm = sha1. Per visualizzare l'elenco degli algoritmi hash supportati, usare: certutil - oid 1 &#124; findstr pwszCNGAlgid &#124; findstr /v CryptOIDInfo|
|KeyAlgorithm|L'algoritmo che verrà usato dal provider del servizio per generare una coppia di chiavi pubblica e privata.|RSA, DH, DSA, ECDH_P256, ECDH_P521, ECDSA_P256, ECDSA_P384, ECDSA_P521|KeyAlgorithm = RSA|
|KeyContainer|È consigliabile non impostare questo parametro per le nuove richieste in cui viene generato il materiale della chiave nuova. Il contenitore di chiavi viene automaticamente generato e gestito dal sistema. Per le richieste in cui deve essere utilizzato il materiale della chiave esistente, questo valore può essere impostato sul nome di contenitore di chiavi di chiave esistente. Usare il comando certutil: comando per visualizzare l'elenco dei contenitori chiave disponibili per il contesto del computer della chiave. Usare il comando certutil – chiave del comando dell'utente per il contesto dell'utente corrente.|Valore stringa casuale</br>Suggerimento: È consigliabile usare le virgolette doppie intorno a qualsiasi valore chiave INF con spazi vuoti o caratteri speciali per evitare potenziali problemi di analisi INF.|KeyContainer = {C347BD28-7F69-4090-AA16-BC58CF4D749C}|
|KeyLength|Definisce la lunghezza della chiave pubblica e privata. La lunghezza della chiave ha un impatto sul livello di sicurezza del certificato. Lunghezza della chiave supera fornisce in genere un maggiore livello di sicurezza; Tuttavia, alcune applicazioni possono presentare limitazioni relative alla lunghezza della chiave.|Qualsiasi lunghezza chiave valido che è supportato dal provider del servizio di crittografia.|KeyLength = 2048|
|KeySpec|Determina se la chiave può essere utilizzata per le firme, per Exchange (crittografia) o per entrambi.|AT_NONE, AT_SIGNATURE, AT_KEYEXCHANGE|KeySpec = AT_KEYEXCHANGE|
|KeyUsage|Definisce ciò che la chiave del certificato deve essere utilizzata per.|CERT_DIGITAL_SIGNATURE_KEY_USAGE -- 80 (128)</br>Suggerimento: I valori riportati sono valori (decimali) esadecimali per ogni definizione di bit. È anche possibile usare sintassi meno recente: un singolo valore esadecimale con più bit di set, anziché la rappresentazione simbolica. Ad esempio, KeyUsage = 0xa0.</br>CERT_NON_REPUDIATION_KEY_USAGE - 40 (64)</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE - 20 (32)</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE - 10 (16)</br>CERT_KEY_AGREEMENT_KEY_USAGE - 8</br>CERT_KEY_CERT_SIGN_KEY_USAGE -- 4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2</br>CERT_CRL_SIGN_KEY_USAGE -- 2</br>CERT_ENCIPHER_ONLY_KEY_USAGE - 1</br>CERT_DECIPHER_ONLY_KEY_USAGE -- 8000 (32768)|KeyUsage = "CERT_DIGITAL_SIGNATURE_KEY_USAGE &#124; CERT_KEY_ENCIPHERMENT_KEY_USAGE"</br>Suggerimento: Più valori utilizzano una pipe (&#124;) il separatore di simboli. Assicurarsi di usare le virgolette doppie quando si usano più valori per evitare problemi di analisi INF.|
|KeyUsageProperty|Recupera un valore che identifica lo scopo specifico per cui è possibile utilizzare una chiave privata.|NCRYPT_ALLOW_DECRYPT_FLAG -- 1</br>NCRYPT_ALLOW_SIGNING_FLAG -- 2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4</br>NCRYPT_ALLOW_ALL_USAGES - ffffff (16777215)|KeyUsageProperty = "NCRYPT_ALLOW_DECRYPT_FLAG &#124; NCRYPT_ALLOW_SIGNING_FLAG"|
|MachineKeySet|Questa chiave è importante quando è necessario creare i certificati appartenenti al computer e non un utente. Il materiale della chiave generato viene mantenuto nel contesto di sicurezza dell'entità di sicurezza (account utente o computer) che ha creato la richiesta. Quando un amministratore crea una richiesta di certificato per conto di un computer, il materiale della chiave deve essere creato nel contesto di sicurezza del computer e non il contesto di sicurezza dell'amministratore. Impossibile accedere in caso contrario, la relativa chiave privata della macchina perché sarebbe nel contesto di sicurezza dell'amministratore.|true, false|MachineKeySet = true</br>Suggerimento: Il valore predefinito è false.|
|NotBefore|Specifica una data o data e ora prima del quale non può essere inviata la richiesta. NotBefore è utilizzabile con ValidityPeriod e ValidityPeriodUnits.|Data o data e ora|NotBefore = "7/24 o 2012 10 31 AM"</br>Suggerimento: NotBefore e NotAfter sono relative a RequestType = solo cert. Data analisi tenta di essere dipendente dalla lingua. Utilizzo di mese verranno risolvere l'ambiguità di nomi e dovrebbero funzionare in tutte le impostazioni locali.|
|NotAfter|Specifica una data o data e ora dopo il quale non può essere inviata la richiesta. NotAfter non può essere utilizzato con ValidityPeriod o ValidityPeriodUnits.|Data o data e ora|NotAfter = "9 al 23 maggio 2014 10 31 AM"</br>Suggerimento: NotBefore e NotAfter sono relative a RequestType = solo cert. Data analisi tenta di essere dipendente dalla lingua. Utilizzo di mese verranno risolvere l'ambiguità di nomi e dovrebbero funzionare in tutte le impostazioni locali.|
|PrivateKeyArchive|L'impostazione PrivateKeyArchive funziona solo se RequestType corrispondenti è impostata su "CMC" perché solo i messaggi di gestione di certificati nel formato della richiesta CMS (CMC) consente di trasferire in modo sicuro la chiave privata del richiedente all'autorità di certificazione per l'archiviazione della chiave.|true, false|PrivateKeyArchive = True|
|EncryptionAlgorithm|L'algoritmo di crittografia da usare.|Le opzioni possibili variano, a seconda della versione del sistema operativo e il set di provider di crittografia installati. Per visualizzare l'elenco degli algoritmi disponibili, eseguire il comando <code>certutil -oid 2 &#124; findstr pwszCNGAlgid</code> del CSP specificato usato deve supportare anche l'algoritmo di crittografia simmetrica e lunghezza.|EncryptionAlgorithm = 3des|
|EncryptionLength|Lunghezza dell'algoritmo di crittografia da usare.|Qualsiasi lunghezza consentita per i parametri EncryptionAlgorithm specificato.|EncryptionLength = 128|
|ProviderName|Il nome del provider è il nome visualizzato del CSP...|Se non si conosce il nome del provider del CSP in uso, eseguire certutil – csplist dalla riga di comando. Il comando visualizzerà i nomi di tutti i CSP che sono disponibili nel sistema locale|ProviderName = "Microsoft RSA SChannel Cryptographic Provider"|
|ProviderType|Il tipo di provider consente di selezionare specifici provider basati su capacità di algoritmo specifico, ad esempio "RSA Full".|Se non si conosce il tipo di provider del CSP in uso, eseguire certutil – csplist da un prompt della riga di comando. Il comando verrà visualizzato il tipo di provider di tutti i CSP che sono disponibili nel sistema locale.|ProviderType = 1|
|RenewalCert|Se è necessario rinnovare un certificato che è presente nel sistema in cui viene generata la richiesta di certificato, è necessario specificare il relativo hash certificato come valore per questa chiave.|L'hash del certificato di qualsiasi certificato che è disponibile nel computer in cui viene creata la richiesta di certificato. Se non si conosce l'hash del certificato, utilizzare lo Snap-In MMC certificati ed esaminare il certificato che deve essere rinnovato. Aprire le proprietà del certificato e vedere l'attributo "Identificazione personale" del certificato. Rinnovo del certificato richiede un PKCS #7 o un formato della richiesta CMC.|RenewalCert = 4EDF274BD2919C6E9EC6A522F0F3B153E9B1582D|
|RequesterName</br>Nota: In questo modo la richiesta di registrazione per conto di un'altra richiesta dell'utente. La richiesta deve essere firmata anche con un certificato di Enrollment Agent o l'autorità di certificazione rifiuterà la richiesta. Usare l'opzione - opzione cert per specificare il certificato di enrollment agent.|Se RequestType è impostato su CMC o PKCS #7, è possibile specificare il nome del richiedente delle richieste di certificato. Se RequestType è impostato su PKCS #10, questa chiave verrà ignorata. Il Requestername può essere impostato solo come parte della richiesta. Non è possibile manipolare Requestername in una richiesta in sospeso.|Domain\User|Requestername = "Contoso\BSmith"|
|RequestType|Determina lo standard che consente di generare e inviare la richiesta di certificato.|PKCS10 -- 1</br>PKCS7 -- 2</br>CMC -- 3</br>CERT - 4</br>SCEP -- fd00 (64768)</br>Suggerimento: Questa opzione indica un certificato autofirmato o autocertificato. Non genera una richiesta, ma piuttosto un nuovo certificato e quindi installa il certificato. Auto-firmato è quello predefinito. Specificare un certificato di firma con opzione – cert per creare un certificato autocertificato che non è autofirmato.|RequestType = CMC|
|SecurityDescriptor</br>Suggerimento: In questo è rilevante solo per le chiavi del computer contesto senza smart card.|Contengono le informazioni di sicurezza associate agli oggetti a protezione diretta. Per gli oggetti più entità a protezione diretta, è possibile specificare il descrittore di sicurezza di un oggetto nella chiamata di funzione che crea l'oggetto.|Le stringhe di base [linguaggio security descriptor definition](https://msdn.microsoft.com/library/aa379567(v=vs.85).aspx).|SecurityDescriptor = "D:P(A;; DISPONIBILITÀ GENERALE;; SY) (A; DISPONIBILITÀ GENERALE;; BA)"|
|AlternateSignatureAlgorithm|Specifica e recupera un valore booleano che indica se l'identificatore di oggetto di algoritmo di firma (OID) per una firma di richiesta o un certificato PKCS #10 è discreti o combinati.|true, false|Alternatesignaturealgorithm=1 = false</br>Suggerimento: Per una firma RSA, false indica un v1.5 Pkcs1. True indica che una firma v2.1.|
|Invisibile all'utente|Per impostazione predefinita, questa opzione consente l'accesso CSP per le informazioni di richiesta e desktop utente interattivo, ad esempio un PIN della smart card da parte dell'utente. Se questa chiave è impostata su TRUE, il CSP non deve interagire con il desktop e non potranno visualizzare l'interfaccia utente per l'utente.|true, false|Invisibile all'utente = true|
|SMIME|Se questo parametro è impostato su TRUE, un'estensione con il valore dell'identificatore oggetto 1.2.840.113549.1.9.15 viene aggiunto alla richiesta. Il numero di identificatori di oggetto dipende il la versione del sistema operativo installato e della funzionalità CSP, che fanno riferimento agli algoritmi di crittografia simmetrica che possono essere usati da applicazioni Secure Multipurpose Internet Mail Extensions (S/MIME), ad esempio Outlook.|true, false|SMIME = true|
|UseExistingKeySet|Questo parametro viene utilizzato per specificare che una coppia di chiavi esistente deve essere utilizzata nella creazione di una richiesta di certificato. Se questa chiave è impostata su TRUE, è necessario specificare anche un valore per la chiave RenewalCert o il nome KeyContainer. È necessario non impostare la chiave può essere esportata perché non è possibile modificare le proprietà di una chiave esistente. In questo caso, non il materiale della chiave viene generato quando viene compilata la richiesta di certificato.|true, false|UseExistingKeySet = true|
|KeyProtection|Specifica un valore che indica come una chiave privata è protetta prima dell'uso.|XCN_NCRYPT_UI_NO_PROTCTION_FLAG -- 0</br>XCN_NCRYPT_UI_PROTECT_KEY_FLAG -- 1</br>XCN_NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2|KeyProtection = NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG|
|SuppressDefaults|Specifica un valore booleano che indica se le estensioni predefinite e gli attributi sono inclusi nella richiesta. I valori predefiniti sono rappresentati tramite i relativi identificatori di oggetto (OID).|true, false|SuppressDefaults = true|
|FriendlyName|Nome descrittivo per il nuovo certificato.|Testo|FriendlyName = "Server1"|
|ValidityPeriodUnits</br>Nota: Questo valore viene usato solo quando la richiesta di tipo = cert.|Specifica un numero di unità che deve essere utilizzato con ValidityPeriod.|Numerico|ValidityPeriodUnits = 3|
|ValidityPeriod</br>Nota: Questo valore viene usato solo quando la richiesta di tipo = cert.|VValidityPeriod deve essere un inglese Americano plurale periodo di tempo.|Anni, mesi, settimane, giorni, ore, minuti, secondi|ValidityPeriod = anni|

Tornare a [contenuto](#BKMK_Contents)

**[Estensioni]**

Questa sezione è facoltativa.

|Estensione OID|Definizione|Value|Esempio|
|-------------|----------|-----|-------|
|2.5.29.17|||2.5.29.17 = "{text}"|
|_continue_|||_continuare_ = "UPN =User@Domain.com&"|
|_continue_|||_continuare_ = "messaggio di posta elettronica =User@Domain.com&"|
|_continue_|||_continue_ = "DNS=host.domain.com&"|
|_continue_|||_continue_ = "DirectoryName=CN=Name,DC=Domain,DC=com&"|
|_continue_|||_continue_ = "URL=http://host.domain.com/default.html&"|
|_continue_|||_continue_ = "IPAddress=10.0.0.1&"|
|_continue_|||_continue_ = "RegisteredId=1.2.3.4.5&"|
|_continue_|||_continue_ = "1.2.3.4.6.1={utf8}String&"|
|_continue_|||_continue_ = "1.2.3.4.6.2={octet}AAECAwQFBgc=&"|
|_continue_|||_continue_ = "1.2.3.4.6.2={octet}{hex}00 01 02 03 04 05 06 07&"|
|_continue_|||_continue_ = "1.2.3.4.6.3={asn}BAgAAQIDBAUGBw==&"|
|_continue_|||_continue_ = "1.2.3.4.6.3={hex}04 08 00 01 02 03 04 05 06 07"|
|2.5.29.37|||2.5.29.37="{text}"|
|_continue_|||_continue_ = "1.3.6.1.5.5.7.|
|_continue_|||_continue_ = "1.3.6.1.5.5.7.3.1"|
|2.5.29.19|||"{text}ca=0pathlength=3"|
|Critico|||Critical=2.5.29.19|
|KeySpec|||AT_NONE -- 0</br>AT_SIGNATURE -- 2</br>AT_KEYEXCHANGE - 1|
|RequestType|||PKCS10 -- 1</br>PKCS7 -- 2</br>CMC -- 3</br>CERT - 4</br>SCEP -- fd00 (64768)|
|KeyUsage|||CERT_DIGITAL_SIGNATURE_KEY_USAGE -- 80 (128)</br>CERT_NON_REPUDIATION_KEY_USAGE - 40 (64)</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE - 20 (32)</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE - 10 (16)</br>CERT_KEY_AGREEMENT_KEY_USAGE - 8</br>CERT_KEY_CERT_SIGN_KEY_USAGE -- 4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2</br>CERT_CRL_SIGN_KEY_USAGE -- 2</br>CERT_ENCIPHER_ONLY_KEY_USAGE - 1</br>CERT_DECIPHER_ONLY_KEY_USAGE -- 8000 (32768)|
|KeyUsageProperty|||NCRYPT_ALLOW_DECRYPT_FLAG -- 1</br>NCRYPT_ALLOW_SIGNING_FLAG -- 2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4</br>NCRYPT_ALLOW_ALL_USAGES - ffffff (16777215)|
|KeyProtection|||NCRYPT_UI_NO_PROTECTION_FLAG -- 0</br>NCRYPT_UI_PROTECT_KEY_FLAG -- 1</br>NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2|
|SubjectNameFlags|modello||CT_FLAG_SUBJECT_REQUIRE_COMMON_NAME - 40000000 (1073741824)</br>CT_FLAG_SUBJECT_REQUIRE_DIRECTORY_PATH -- 80000000 (2147483648)</br>CT_FLAG_SUBJECT_REQUIRE_DNS_AS_CN -- 10000000 (268435456)</br>CT_FLAG_SUBJECT_REQUIRE_EMAIL - 20000000 (536870912)</br>CT_FLAG_OLD_CERT_SUPPLIES_SUBJECT_AND_ALT_NAME -- 8</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DIRECTORY_GUID -- 1000000 (16777216)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DNS - 8000000 (134217728)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DOMAIN_DNS -- 400000 (4194304)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_EMAIL - 4000000 (67108864)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_SPN -- 800000 (8388608)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_UPN -- 2000000 (33554432)|
|X500NameFlags|||CERT_NAME_STR_NONE -- 0</br>CERT_OID_NAME_STR -- 2</br>CERT_X500_NAME_STR -- 3</br>CERT_NAME_STR_SEMICOLON_FLAG -- 40000000 (1073741824)</br>CERT_NAME_STR_NO_PLUS_FLAG -- 20000000 (536870912)</br>CERT_NAME_STR_NO_QUOTING_FLAG -- 10000000 (268435456)</br>CERT_NAME_STR_CRLF_FLAG -- 8000000 (134217728)</br>CERT_NAME_STR_COMMA_FLAG -- 4000000 (67108864)</br>CERT_NAME_STR_REVERSE_FLAG -- 2000000 (33554432)</br>CERT_NAME_STR_FORWARD_FLAG -- 1000000 (16777216)</br>CERT_NAME_STR_DISABLE_IE4_UTF8_FLAG -- 10000 (65536)</br>CERT_NAME_STR_ENABLE_T61_UNICODE_FLAG -- 20000 (131072)</br>CERT_NAME_STR_ENABLE_UTF8_UNICODE_FLAG -- 40000 (262144)</br>CERT_NAME_STR_FORCE_UTF8_DIR_STR_FLAG -- 80000 (524288)</br>CERT_NAME_STR_DISABLE_UTF8_DIR_STR_FLAG -- 100000 (1048576)</br>CERT_NAME_STR_ENABLE_PUNYCODE_FLAG -- 200000 (2097152)|

Tornare a [contenuto](#BKMK_Contents)

> [!NOTE]
> SubjectNameFlags consente il file INF specificare quali campi di estensione SubjectAltName e soggetto devono essere popolato automaticamente per certreq in base all'utente corrente o proprietà della macchina corrente: Nome DNS, UPN e così via. Usando il valore letterale "template" significa che vengono invece utilizzati i flag di nome di modello. In questo modo un singolo file INF da utilizzare in più contesti per generare le richieste con informazioni sul soggetto specifici del contesto.
>
> X500NameFlags specifica i flag da passare direttamente all'API CertStrToName quando il valore della chiave INF soggetto viene convertito in un ASN.1 codifica del nome distinto.

Per richiedere un certificato in base alla certreq-new attenersi alla procedura nell'esempio riportato di seguito:

> [!WARNING]
> Il contenuto di questo argomento si basa sulle impostazioni predefinite per Windows Server 2008 Servizi certificati Active Directory; ad esempio si imposta la lunghezza della chiave a 2048, selezione di Microsoft Software Key Storage Provider come CSP e l'utilizzo di Secure Hash Algorithm 1 (SHA1). Valutare queste selezioni in base ai requisiti dei criteri di sicurezza dell'azienda.

Per creare un File dei criteri (con estensione inf) copiare e salvare l'esempio seguente nel blocco note e salvarlo come RequestConfig.inf:
```
[NewRequest] 
Subject = "CN=<FQDN of computer you are creating the certificate>" 
Exportable = TRUE 
KeyLength = 2048 
KeySpec = 1 
KeyUsage = 0xf0 
MachineKeySet = TRUE 
[RequestAttributes]
CertificateTemplate="WebServer"
[Extensions] 
OID = 1.3.6.1.5.5.7.3.1 
OID = 1.3.6.1.5.5.7.3.2  

```
Nel computer per il quale si richiede un certificato, digitare il comando seguente:
```
CertReq –New RequestConfig.inf CertRequest.req 

```
Nell'esempio seguente viene illustrato come implementare la sintassi di sezione [Strings] per OID e altri difficili da interpretare i dati. Nuovo esempio della sintassi {text} per l'estensione utilizzo chiavi avanzato, che usa un elenco delimitato da virgole di OID:
```
[Version]
Signature="$Windows NT$

[Strings]
szOID_ENHANCED_KEY_USAGE = "2.5.29.37"
szOID_PKIX_KP_SERVER_AUTH = "1.3.6.1.5.5.7.3.1"
szOID_PKIX_KP_CLIENT_AUTH = "1.3.6.1.5.5.7.3.2"

[NewRequest]
Subject = "CN=TestSelfSignedCert"
Requesttype = Cert

[Extensions]
%szOID_ENHANCED_KEY_USAGE%="{text}%szOID_PKIX_KP_SERVER_AUTH%,"
_continue_ = "%szOID_PKIX_KP_CLIENT_AUTH%"
```
Tornare a [contenuto](#BKMK_Contents)

## <a name="BKMK_accept"></a>CertReq-accettare

```
CertReq -accept [Options] [CertChainFileIn | FullResponseFileIn | CertFileIn]
```
– Accettare parametri collega la chiave privata generata in precedenza con il certificato emesso e rimuove la richiesta di certificato in sospeso dal sistema in cui viene richiesto il certificato (se è presente una richiesta corrispondente).

È possibile usare questo esempio per accettare manualmente un certificato:
```
certreq -accept certnew.cer 

```

> [!WARNING]
> -Accettare verbo, -opzioni utente e – computer indicano se il certificato in corso l'installazione deve essere installato nel contesto del computer o utente. Se è una richiesta in sospeso in entrambi contesto corrispondente alla chiave pubblica in corso l'installazione, queste opzioni non sono necessari. Se è presente alcuna richiesta in sospeso, quindi uno di questi deve essere specificato.

Tornare a [contenuto](#BKMK_Contents)

## <a name="BKMK_policy"></a>CertReq-criterio

```
certreq -policy [-attrib AttributeString] [-binary] [-cert CertID] [RequestFileIn [PolicyFileIn [RequestFileOut [PKCS10FileOut]]]]
```
-   Il file di configurazione che definisce i vincoli applicati a un certificato CA quando viene definita subordinazione qualificata viene chiamato inf...
-   È possibile trovare un esempio del file inf nel [appendice A di pianificazione e implementazione della certificazione incrociata e della subordinazione qualificata](https://technet.microsoft.com/library/cc738878(WS.10).aspx) white paper.
-   Se si digita il certreq-criteri senza alcun parametro aggiuntivo si aprirà una finestra di dialogo che consente di selezionare la richiesta file (req, cmc, txt, der, area a esecuzione vincolata o crt). Quando si fa clic sul pulsante Apri selezionare il file richiesto, verrà aperto un'altra finestra di dialogo per selezionare il file INF.

È possibile usare questo esempio per creare una richiesta di certificato incrociato:
```
certreq -policy Certsrv.req Policy.inf newcertsrv.req 

```
Tornare a [contenuto](#BKMK_Contents)

## <a name="BKMK_sign"></a>CertReq-accesso

```
certreq -sign [Options] [RequestFileIn [RequestFileOut]]
```
-   Se si digita il certreq-accesso senza alcun parametro aggiuntivo si aprirà una finestra di dialogo che consente di selezionare il file di richiesto (req, cmc, txt, der, area a esecuzione vincolata o crt).
-   Firma la richiesta di subordinazione qualificata può richiedere le credenziali di amministratore dell'organizzazione. Si tratta di una procedura consigliata per il rilascio di certificati di firma per la subordinazione qualificata.
-   Il certificato usato per firmare la richiesta di subordinazione qualificata viene creato usando il modello di subordinazione qualificata. Enterprise Admins deve firmare la richiesta o concedere le autorizzazioni utente per utenti singoli che firmeranno il certificato.
-   Quando si effettua la richiesta CMC, potrebbe essere necessario avere più personale firmare la richiesta, a seconda del livello di garanzia che è associato il subordinazione qualificata.
-   Se la CA padre dell'autorità di certificazione subordinata completo si sta installando è offline, è necessario ottenere il certificato della CA per la CA subordinata completo dall'elemento padre non in linea. Se la CA padre è online, specificare il certificato della CA per la CA subordinata completo durante l'installazione guidata di servizi certificati.

La sequenza di comandi seguente mostrerà come creare una nuova richiesta di certificato, firmarlo e inviarlo:
```
certreq -new policyfile.inf MyRequest.req
certreq -sign MyRequest.req MyRequest_Sign.req
certreq -submit MyRequest_Sign.req MyRequest_cert.cer 

```
Tornare a [contenuto](#BKMK_Contents)

## <a name="BKMK_enroll"></a>CertReq-registrazione

Eseguire la registrazione a un certificato
```
certreq –enroll [Options] TemplateName
```
Per rinnovare un certificato esistente
```
certreq –enroll –cert CertId [Options] Renew [ReuseKeys]
```
È solo possibile rinnovare i certificati che sono ora valida. I certificati scaduti non possono essere rinnovati e devono essere sostituiti con un nuovo certificato.

Ecco un esempio di rinnovo di un certificato utilizzando il relativo numero di serie:
```
certreq –enroll -machine –cert "61 2d 3c fe 00 00 00 00 00 05" Renew
```
Di seguito un esempio di registrazione di un modello di certificato chiamato server Web utilizzando l'asterisco (*) per selezionare il server dei criteri tramite U/ricerca per categorie:
```
certreq -enroll –machine –policyserver * "WebServer"
```
Tornare a [contenuto](#BKMK_Contents)

## <a name="BKMK_Options"></a>Opzioni

|Opzioni|Descrizione|
|-------|-----------|
|-qualsiasi|Forzare ICertRequest:: Submit per determinare il tipo di codifica.|
|attrib - \<AttributeString >|Specifica le coppie nome / valore stringa, separate da punti.</br>Stringa di nome e valore separata coppie con \n (ad esempio, Name1:Value1\nName2:Value2).|
|-binario|Formati di file di output in formato binario anziché con codifica base64.|
|-PolicyServer *\<PolicyServer>*|"ldap:  *\<path >*"</br>Inserire l'URI o un ID univoco per un computer che esegue il servizio Web di registrazione certificati.</br>Per specificare che si desidera utilizzare un file di richiesta esplorando, usare semplicemente un segno meno (-) Iscriviti  *\<policyserver >*.|
|-config \<ConfigString>|Elabora l'operazione utilizzando l'autorità di certificazione specificata nella stringa di configurazione, ovvero CAHostName\CAName. Per una connessione https, specificare l'URI del server di registrazione. Archivio Autorità di certificazione per il computer locale, usare un segno meno (-) sign.|
|-Anonima|Usare credenziali anonime per servizi Web di registrazione certificati.|
|-Kerberos|Usare le credenziali Kerberos (dominio) per servizi Web di registrazione certificati.|
|-ClientCertificate *\<ClientCertId>*|È possibile sostituire il  *\<ClientCertID >* con un'identificazione personale del certificato, CN, EKU, modello, messaggio di posta elettronica, UPN e il nuovo nome = sintassi valori.|
|-UserName  *\<UserName >*|Utilizzato con servizi Web di registrazione certificati. È possibile sostituire  *\<UserName >* con il nome SAM o dominio\utente. Questa opzione è per l'uso con l'opzione -p.|
|-p *\<Password>*|Utilizzato con servizi Web di registrazione certificati. Substitute  *\<Password >* con la password dell'utente effettivo. Questa opzione è per l'uso con l'opzione - UserName.|
|-utente|Configura il - contesto utente per una nuova richiesta di certificato o specifica il contesto per un accettazione di un certificato. Questo è il contesto predefinito, se non viene specificato nell'INF o modello.|
|-machine|Consente di configurare una nuova richiesta di certificato o specifica il contesto per un'accettazione un certificato per il contesto del computer. Per le nuove richieste deve essere coerenza con la chiave INF MachineKeyset e con contesto del modello. Se questa opzione non è specificata e il modello non impostare un contesto, il valore predefinito è il contesto dell'utente.|
|-crl|Include gli elenchi di revoche di certificati (CRL) nell'output nel file PKCS #7 con codifica base 64 specificato da CertChainFileOut o il file con codifica base 64 specificato da RequestFileOut.|
|-rpc|Indica a Servizi certificati Active Directory (AD CS) per usare una connessione del server remote procedure call (RPC) anziché Distributed COM.|
|-AdminForceMachine|Usare la chiave di servizio o la rappresentazione per inviare la richiesta dal contesto di sistema locale. Richiede che l'utente richiama questa opzione sia un membro del gruppo Administrators locale.|
|-RenewOnBehalfOf|Inviare un rinnovo per conto dell'oggetto identificato nel certificato di firma. Questo imposta CR_IN_ROBO quando si chiama [ICertRequest:: Submit](https://technet.microsoft.com/subscriptions/aa385040.aspx)|
|-f|Forzare i file esistenti verranno sovrascritti. In questo modo si anche i modelli e criteri di memorizzazione nella cache.|
|-q|Usare la modalità invisibile all'utente; eliminare tutti i prompt interattivi.|
|-Unicode|Scrive output in formato Unicode quando output standard viene reindirizzato o inviati tramite pipe a un altro comando, è utile quando viene richiamato da script Windows PowerShell®).|
|-UnicodeText|Invia l'output Unicode durante la scrittura di testo base64 codificati BLOB dei dati ai file.|

Tornare a [contenuto](#BKMK_Contents)

## <a name="BKMK_Formats"></a>Formati

|Formati|Descrizione|
|-------|-----------|
|RequestFileIn|Nome del file di input con codifica Base64 o binarie: Richiesta di certificato PKCS #10, richiesta di certificato CMS, richiesta di rinnovo certificato PKCS #7, il certificato X.509 sia certificazione con certificazione incrociata o richiesta di certificato in formato KeyGen tag.|
|RequestFileOut|Nome file di output con codifica Base64|
|CertFileOut|Nome del file x-509 con codifica Base64.|
|PKCS10FileOut|Per l'uso con il [Certreq-criteri](#BKMK_policy) solo verbo. Con codifica Base64 PKCS10 nome file di output.|
|CertChainFileOut|Nome del file PKCS #7 con codifica Base64.|
|FullResponseFileOut|Nome del file con codifica Base64 risposta completa.|
|PolicyFileIn|Per l'uso con il [Certreq-criteri](#BKMK_policy) solo verbo. File INF contenente una rappresentazione testuale delle estensioni utilizzate per qualificare una richiesta.|

## <a name="BKMK_Examples"></a>Esempi aggiuntivi certreq

Gli articoli seguenti contengono esempi di utilizzo certreq:
-   [Come richiedere un certificato con un nome alternativo del soggetto personalizzato](https://technet.microsoft.com/library/ff625722.aspx)
-   [Guida al Lab di test: Distribuzione di una gerarchia infrastruttura a chiave pubblica a due livelli CS](https://technet.microsoft.com/library/hh831348.aspx)
-   [Appendice 3: Certreq.exe Syntax](https://technet.microsoft.com/library/cc736326.aspx)
-   [Come creare manualmente un certificato SSL del server web](http://blogs.technet.com/b/pki/archive/2009/08/05/how-to-create-a-web-server-ssl-certificate-manually.aspx)
-   [Richiedere un certificato utilizzando un'autorità di certificazione di Windows Server 2008 di Provisioning AMT](https://social.technet.microsoft.com/wiki/contents/articles/request-an-amt-provisioning-certificate-using-a-windows-server-2008-ca.aspx)
-   [Registrazione del certificato per l'agente di System Center Operations Manager](https://social.technet.microsoft.com/wiki/contents/articles/certificate-enrollment-for-system-center-operations-manager-agent.aspx)
-   [Guida dettagliata di AD CS: Distribuzione di gerarchia infrastruttura a chiave pubblica a due livelli](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)
-   [Come abilitare l'accesso LDAP tramite SSL con un'autorità di certificazione di terze parti](https://support.microsoft.com/kb/321051)

Tornare a [contenuto](#BKMK_Contents)
