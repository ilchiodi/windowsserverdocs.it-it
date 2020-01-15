---
title: certreq
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8e3c98fd965fc6923c6af7893261bd1e0eee7d8b
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947597"
---
# <a name="certreq"></a>certreq



Certreq può essere usato per richiedere certificati da un'autorità di certificazione (CA), per recuperare una risposta a una richiesta precedente da una CA, per creare una nuova richiesta da un file inf, per accettare e installare una risposta a una richiesta, per costruire una certificazione incrociata o richiesta di subordinazione qualificata da un certificato o da una richiesta CA esistente e per firmare una richiesta di certificazione incrociata o di subordinazione qualificata.

> [!WARNING]
> - Le versioni precedenti di certreq potrebbero non fornire tutte le opzioni descritte in questo documento. È possibile visualizzare tutte le opzioni fornite da una versione specifica di certreq eseguendo i comandi illustrati nella sezione relativa alla notazione della sintassi.
> - Certreq non supporta la creazione di una nuova richiesta di certificato basata su un modello di attestazione chiave in un ambiente CEP/CES

## <a name="BKMK_Contents"></a>Sommario

Le sezioni principali di questo articolo sono le seguenti:
1.  [Verbi](#BKMK_Verbs)
2.  [Notazione della sintassi](#BKMK_notation)
3.  [Opzioni](#BKMK_Options)
4.  [Formati](#BKMK_Formats)
5.  [Esempi di certreq aggiuntivi](#BKMK_Examples)

## <a name="BKMK_Verbs"></a>Verbi

La tabella seguente descrive i verbi che possono essere usati con il comando certreq

|Cambia la funzione|Descrizione|
|------|-----------|
|-Invia|Invia una richiesta a una CA. Per ulteriori informazioni, vedere [certreq-submit](#BKMK_Submit).|
|-recuperare *RequestId*|Recupera una risposta a una richiesta precedente da un'autorità di certificazione. Per ulteriori informazioni, vedere [certreq-retrieve](#BKMK_Retrieve).|
|-Nuovo|Crea una nuova richiesta da un file con estensione inf. Per ulteriori informazioni, vedere [certreq-new](#BKMK_New).|
|-Accetta|Accetta e installa una risposta a una richiesta di certificato. Per ulteriori informazioni, vedere [certreq-accept](#BKMK_accept).|
|-Criterio|Imposta i criteri per una richiesta. Per ulteriori informazioni, vedere [certreq-policy](#BKMK_policy).|
|-Firma|Firma una richiesta di certificazione incrociata o di subordinazione qualificata. Per ulteriori informazioni, vedere [certreq-Sign](#BKMK_sign).|
|-Registrazione|Registra o rinnova un certificato. Per ulteriori informazioni, vedere [certreq-registra](#BKMK_enroll).|
|-?|Visualizza un elenco di sintassi, opzioni e descrizioni di certreq.|
|*\<verbo >* -?|Visualizza la guida per il verbo specificato.|
|-v-?|Visualizza un elenco dettagliato della sintassi, delle opzioni e delle descrizioni di certreq.|

Torna al [contenuto](#BKMK_Contents)

## <a name="BKMK_notation"></a>Notazione della sintassi

-   Per la sintassi della riga di comando di base, eseguire `certreq -?`
-   Per la sintassi sull'uso di certutil con un verbo specifico, eseguire **certreq** *\<verbo >* **-?**
-   Per inviare tutta la sintassi certutil in un file di testo, eseguire i comandi seguenti:  
    -   `certreq -v -? > certreqhelp.txt`
    -   `notepad certreqhelp.txt`

Nella tabella seguente vengono descritti la notazione utilizzata per indicare la sintassi della riga di comando.

|Notation|Descrizione|
|--------|-----------|
|Testo senza parentesi quadre o parentesi graffe|Elementi che devono essere digitati come illustrato|
|\<testo racchiuso tra parentesi angolari >|Segnaposto per il quale è necessario fornire un valore|
|[Testo racchiuso tra parentesi quadre]|Elementi facoltativi|
|{Testo racchiuso tra parentesi graffe}|Set di elementi obbligatori; scegliere una|
|Barra verticale (&#124;)|Separatore per gli elementi che si escludono a vicenda; scegliere una|
|Puntini di sospensione (…)|Elementi che possono essere ripetuti|

Torna al [contenuto](#BKMK_Contents)

## <a name="BKMK_Submit"></a>Certreq-Invia

Si tratta del parametro certreq. exe predefinito, se nessuna opzione viene specificata in modo esplicito al prompt della riga di comando, certreq. exe tenta di inviare una richiesta di certificato a una CA.
```
CertReq [-Submit] [Options] [RequestFileIn [CertFileOut [CertChainFileOut [FullResponseFileOut]]]]
```
Quando si usa l'opzione – Submit, è necessario specificare un file di richiesta di certificato. Se questo parametro viene omesso, viene visualizzata una finestra di apertura file comune in cui è possibile selezionare il file di richiesta di certificato appropriato.

È possibile usare questi esempi come punto di partenza per compilare la richiesta di invio del certificato:

Per inviare una richiesta di certificato semplice, usare l'esempio seguente:
```
certreq –submit certRequest.req certnew.cer certnew.pfx
```
Per richiedere un certificato specificando l'attributo SAN, vedere la procedura dettagliata nell'articolo 931351 della Microsoft Knowledge base [come aggiungere un nome alternativo del soggetto a un certificato LDAP sicuro](https://support.microsoft.com/kb/931351) nella sezione "come utilizzare l'utilità Certreq. exe per creare e inviare una richiesta di certificato che includa una San".

Torna al [contenuto](#BKMK_Contents)

## <a name="BKMK_Retrieve"></a>Certreq-recupera

```
certreq -retrieve [Options] RequestId [CertFileOut [CertChainFileOut [FullResponseFileOut]]]
```
-   Se non si specifica la finestra di dialogo CAComputerName o nome utente nella configurazione di CAComputerName\CANamea, viene visualizzato un elenco di tutte le CA disponibili.
-   Se si utilizza -config - anziché -config NomeComputerCA\NomeCa, l'operazione viene elaborata tramite l'Autorità di certificazione predefinita.
-   È possibile usare certreq-retrieve *RequestId* per recuperare il certificato dopo che è stato effettivamente emesso dall'autorità di certificazione. La PKC *RequestId*può essere un valore decimale o esadecimale con prefisso 0x e può essere un numero di serie del certificato senza prefisso 0x. È anche possibile usarlo per recuperare tutti i certificati che sono stati rilasciati dall'autorità di certificazione, inclusi i certificati revocati o scaduti, a prescindere dal fatto che la richiesta del certificato sia sempre nello stato in sospeso.
-   Se si invia una richiesta alla CA, il modulo criteri della CA potrebbe lasciare la richiesta in uno stato in sospeso e restituire il *RequestId* al chiamante Certreq per la visualizzazione. Infine, l'amministratore della CA emetterà il certificato o rifiuterà la richiesta.

Il comando seguente recupera l'ID certificato 20 e crea il file di certificato (. cer):
```
certreq -retrieve 20 MyCertificate.cer 
```
Torna al [contenuto](#BKMK_Contents)

## <a name="BKMK_New"></a>Certreq-nuovo

```
certreq -new [Options] [PolicyFileIn [RequestFileOut]]
```
Poiché il file INF consente di specificare un set completo di parametri e opzioni, è difficile definire un modello predefinito che gli amministratori devono usare per tutti gli scopi. Pertanto, in questa sezione vengono descritte tutte le opzioni che consentono di creare un file INF personalizzato in base alle specifiche esigenze. Per descrivere la struttura dei file INF vengono usate le parole chiave seguenti.
1.  Una *sezione* è un'area del file inf che copre un gruppo logico di chiavi. Una sezione viene sempre visualizzata tra parentesi quadre nel file INF.
2.  Una *chiave* è il parametro che si trova a sinistra del segno di uguale.
3.  Un *valore* è il parametro a destra del segno di uguale.

Ad esempio, un file INF minimo avrà un aspetto simile al seguente:
```
[NewRequest] 
; At least one value must be set in this section 
Subject = "CN=W2K8-BO-DC.contoso2.com"
```
Di seguito sono riportate alcune delle possibili sezioni che possono essere aggiunte al file INF:

**[NewRequest]**

Questa sezione è obbligatoria per un file INF che funge da modello per una nuova richiesta di certificato. Questa sezione richiede almeno una chiave con un valore.

|Codice|Definizione|Value|Esempio|
|---|----------|-----|-------|
|Soggetto|Diverse applicazioni si basano sulle informazioni sul soggetto in un certificato. Pertanto, è consigliabile specificare un valore per questa chiave. Se l'oggetto non è impostato qui, è consigliabile includere un nome soggetto come parte dell'estensione del certificato del nome alternativo del soggetto.|Valori stringa del nome distinto relativo|Subject = "CN = computer1. contoso. com" Subject = "CN = John Smith, CN = Users, DC = contoso, DC = com"|
|Exportable|Se questo attributo è impostato su TRUE, la chiave privata può essere esportata con il certificato. Per garantire un livello elevato di sicurezza, le chiavi private non devono essere esportabili. in alcuni casi, tuttavia, potrebbe essere necessario rendere esportabile la chiave privata se più computer o utenti devono condividere la stessa chiave privata.|true, false|Exportable = TRUE. Le chiavi CNG possono distinguere tra questo e il testo non crittografato esportabile. Le chiavi CAPI1 non possono.|
|ExportableEncrypted|Specifica se la chiave privata deve essere impostata per essere esportabile.|true, false|ExportableEncrypted = true</br>Suggerimento: non tutti gli algoritmi e le dimensioni delle chiavi pubbliche funzioneranno con tutti gli algoritmi hash. Il CSP specificato di Tamehe deve supportare anche l'algoritmo hash specificato. Per visualizzare l'elenco degli algoritmi hash supportati, è possibile eseguire il comando <code>certutil -oid 1 &#124; findstr pwszCNGAlgid &#124; findstr /v CryptOIDInfo</code>|
|HashAlgorithm|Algoritmo hash da utilizzare per la richiesta.|SHA256, SHA384, SHA512, SHA1, MD5, MD4, MD2|HashAlgorithm = SHA1. Per visualizzare l'elenco degli algoritmi hash supportati, usare certutil-oid 1 &#124; findstr pwszCNGAlgid &#124; findstr/v CryptOIDInfo|
|KeyAlgorithm|Algoritmo che verrà utilizzato dal provider di servizi per generare una coppia di chiavi pubblica e privata.|RSA, DH, DSA, ECDH_P256, ECDH_P521, ECDSA_P256, ECDSA_P384, ECDSA_P521|Algoritmo di fotoalgoritmo = RSA|
|KeyContainer|Non è consigliabile impostare questo parametro per le nuove richieste in cui viene generato un nuovo materiale della chiave. Il contenitore di chiavi viene generato e gestito automaticamente dal sistema. Per le richieste in cui deve essere usato il materiale della chiave esistente, questo valore può essere impostato sul nome del contenitore delle chiavi esistente. Usare il comando certutil – Key per visualizzare l'elenco dei contenitori di chiavi disponibili per il contesto del computer. Usare il comando certutil – Key-User per il contesto dell'utente corrente.|Valore stringa casuale</br>Suggerimento: è consigliabile usare le virgolette doppie intorno a qualsiasi valore di chiave INF con spazi vuoti o caratteri speciali per evitare potenziali problemi di analisi INF.|Contenitore di contenitori = {C347BD28-7F69-4090-AA16-BC58CF4D749C}|
|KeyLength|Definisce la lunghezza della chiave pubblica e privata. La lunghezza della chiave ha un effetto sul livello di sicurezza del certificato. Una lunghezza della chiave maggiore in genere fornisce un livello di sicurezza più elevato; Tuttavia, alcune applicazioni possono avere limitazioni relative alla lunghezza della chiave.|Qualsiasi lunghezza di chiave valida supportata dal provider del servizio di crittografia.|KeyLength = 2048|
|KeySpec|Determina se la chiave può essere utilizzata per le firme, per Exchange (crittografia) o per entrambe.|AT_NONE, AT_SIGNATURE, AT_KEYEXCHANGE|Specifica di AT_KEYEXCHANGE|
|KeyUsage|Definisce la chiave del certificato da utilizzare per.|CERT_DIGITAL_SIGNATURE_KEY_USAGE--80 (128)</br>Suggerimento: i valori visualizzati sono valori esadecimali (decimali) per ogni definizione di bit. È inoltre possibile utilizzare una sintassi precedente, ovvero un singolo valore esadecimale con più bit impostati anziché la rappresentazione simbolica. Ad esempio, DataUsage = messaggi 0XA0.</br>CERT_NON_REPUDIATION_KEY_USAGE--40 (64)</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE--20 (32)</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE--10 (16)</br>CERT_KEY_AGREEMENT_KEY_USAGE--8</br>CERT_KEY_CERT_SIGN_KEY_USAGE--4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE--2</br>CERT_CRL_SIGN_KEY_USAGE--2</br>CERT_ENCIPHER_ONLY_KEY_USAGE--1</br>CERT_DECIPHER_ONLY_KEY_USAGE--8000 (32768)|DataUsage = "CERT_DIGITAL_SIGNATURE_KEY_USAGE &#124; CERT_KEY_ENCIPHERMENT_KEY_USAGE"</br>Suggerimento: più valori usano un separatore&#124;di simboli pipe (). Assicurarsi di usare le virgolette doppie quando si usano più valori per evitare problemi di analisi INF.|
|KeyUsageProperty|Recupera un valore che identifica lo scopo specifico per il quale è possibile utilizzare una chiave privata.|NCRYPT_ALLOW_DECRYPT_FLAG--1</br>NCRYPT_ALLOW_SIGNING_FLAG--2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG--4</br>NCRYPT_ALLOW_ALL_USAGES--FFFFFF (16777215)|KeyUsageProperty = "NCRYPT_ALLOW_DECRYPT_FLAG &#124; NCRYPT_ALLOW_SIGNING_FLAG"|
|MachineKeySet|Questa chiave è importante quando è necessario creare certificati di proprietà del computer e non di un utente. Il materiale della chiave generato viene mantenuto nel contesto di sicurezza dell'entità di sicurezza (utente o account computer) che ha creato la richiesta. Quando un amministratore crea una richiesta di certificato per conto di un computer, il materiale della chiave deve essere creato nel contesto di sicurezza del computer e non nel contesto di sicurezza dell'amministratore. In caso contrario, il computer non è riuscito ad accedere alla chiave privata perché si trova nel contesto di sicurezza dell'amministratore.|true, false|MachineKeySet = TRUE</br>Suggerimento: il valore predefinito è false.|
|NotBefore|Specifica una data o una data e un'ora prima delle quali non è possibile emettere la richiesta. NotBefore può essere usato con ValidityPeriod e ValidityPeriodUnits.|Data, data e ora|NotBefore = "7/24/2012 10:31 AM"</br>Suggerimento: NotBefore e NotAfter sono per RequestType = CERT only. L'analisi della data tenta di essere dipendente dalle impostazioni locali. L'utilizzo dei nomi dei mesi eliminerà le ambiguità e funzionerà in tutte le impostazioni locali.|
|NotAfter|Specifica una data o una data e un'ora dopo le quali non è possibile emettere la richiesta. Non è possibile usare NotAfter con ValidityPeriod o ValidityPeriodUnits.|Data, data e ora|NotAfter = "9/23/2014 10:31 AM"</br>Suggerimento: NotBefore e NotAfter sono per RequestType = CERT only. L'analisi della data tenta di essere dipendente dalle impostazioni locali. L'utilizzo dei nomi dei mesi eliminerà le ambiguità e funzionerà in tutte le impostazioni locali.|
|PrivateKeyArchive|L'impostazione PrivateKeyArchive funziona solo se il RequestType corrispondente è impostato su "CMC" perché solo il formato di richiesta dei messaggi di gestione certificati su CMS (CMC) consente di trasferire in modo sicuro la chiave privata del richiedente all'autorità di certificazione per l'archiviazione delle chiavi.|true, false|PrivateKeyArchive = true|
|EncryptionAlgorithm|Algoritmo di crittografia da utilizzare.|Le opzioni possibili variano a seconda della versione del sistema operativo e del set di provider del servizio di crittografia installati. Per visualizzare l'elenco degli algoritmi disponibili, eseguire il comando <code>certutil -oid 2 &#124; findstr pwszCNGAlgid</code> il CSP specificato utilizzato deve supportare anche l'algoritmo di crittografia simmetrica e la lunghezza specificati.|EncryptionAlgorithm = 3DES|
|EncryptionLength|Lunghezza dell'algoritmo di crittografia da utilizzare.|Lunghezza consentita dal EncryptionAlgorithm specificato.|EncryptionLength = 128|
|ProviderName|Il nome del provider è il nome visualizzato del CSP.|Se non si conosce il nome del provider CSP in uso, eseguire certutil – csplist dalla riga di comando. Il comando visualizzerà i nomi di tutti i CSP disponibili nel sistema locale|ProviderName = "Microsoft RSA SChannel Cryptographic Provider"|
|ProviderType|Il tipo di provider viene utilizzato per selezionare provider specifici in base a funzionalità specifiche dell'algoritmo, ad esempio "RSA full".|Se non si conosce il tipo di provider del CSP in uso, eseguire certutil – csplist da un prompt della riga di comando. Il comando visualizzerà il tipo di provider di tutti i CSP disponibili nel sistema locale.|ProviderType = 1|
|RenewalCert|Se è necessario rinnovare un certificato presente nel sistema in cui viene generata la richiesta di certificato, è necessario specificare l'hash del certificato come valore per questa chiave.|Hash del certificato di qualsiasi certificato disponibile nel computer in cui viene creata la richiesta di certificato. Se non si conosce l'hash del certificato, utilizzare lo snap-in MMC certificati ed esaminare il certificato da rinnovare. Aprire le proprietà del certificato e vedere l'attributo "identificazione personale" del certificato. Il rinnovo del certificato richiede un formato di richiesta PKCS # 7 o CMC.|RenewalCert = 4EDF274BD2919C6E9EC6A522F0F3B153E9B1582D|
|RequesterName</br>Nota: questa operazione consente di eseguire la registrazione per conto di un'altra richiesta dell'utente. La richiesta deve anche essere firmata con un certificato dell'agente di registrazione o la CA rifiuterà la richiesta. Usare l'opzione-CERT per specificare il certificato dell'agente di registrazione.|Il nome del richiedente può essere specificato per le richieste di certificato se RequestType è impostato su PKCS # 7 o CMC. Se RequestType è impostato su PKCS # 10, questa chiave verrà ignorata. Requestername può essere impostato solo come parte della richiesta. Non è possibile modificare Requestername in una richiesta in sospeso.|Dominio\utente|Requestername = "Contoso\BSmith"|
|RequestType|Determina lo standard utilizzato per generare e inviare la richiesta di certificato.|PKCS10--1</br>PKCS7--2</br>CMC--3</br>Certificato--4</br>SCEP--fd00 (64768)</br>Suggerimento: questa opzione indica un certificato autofirmato o rilasciato autonomamente. Non genera una richiesta, bensì un nuovo certificato, quindi installa il certificato. Il valore predefinito è autofirmato. Specificare un certificato di firma utilizzando l'opzione – CERT per creare un certificato autocertificato non autofirmato.|RequestType = CMC|
|SecurityDescriptor</br>Suggerimento: questo è pertinente solo per le chiavi non smart card del contesto del computer.|Contenere le informazioni di sicurezza associate agli oggetti a protezione diretta. Per la maggior parte degli oggetti a protezione diretta, è possibile specificare un descrittore di sicurezza di un oggetto nella chiamata di funzione che crea l'oggetto.|Stringhe basate sul [linguaggio di definizione del descrittore di sicurezza](https://msdn.microsoft.com/library/aa379567(v=vs.85).aspx).|SecurityDescriptor = "D:P (A;; GA;;; SY) (A;; GA;;; BA) "|
|AlternateSignatureAlgorithm|Specifica e recupera un valore booleano che indica se l'identificatore di oggetto (OID) dell'algoritmo di firma per una richiesta PKCS # 10 o una firma del certificato è discreto o combinato.|true, false|AlternateSignatureAlgorithm = false</br>Suggerimento: per una firma RSA, false indica Pkcs1 v 1.5. True indica una firma v 2.1.|
|Silenzioso|Per impostazione predefinita, questa opzione consente al CSP di accedere al desktop utente interattivo e di richiedere informazioni come un PIN della smart card dall'utente. Se questa chiave è impostata su TRUE, il CSP non deve interagire con il desktop e non verrà visualizzata alcuna interfaccia utente per l'utente.|true, false|Invisibile all'utente = true|
|SMIME|Se questo parametro è impostato su TRUE, alla richiesta verrà aggiunta un'estensione con il valore dell'identificatore di oggetto 1.2.840.113549.1.9.15. Il numero di identificatori di oggetto dipende dalla versione del sistema operativo installata e dalla funzionalità CSP, che fanno riferimento a algoritmi di crittografia simmetrica che possono essere utilizzati da applicazioni Secure Multipurpose Internet Mail Extensions (S/MIME), ad esempio Outlook.|true, false|SMIME = true|
|UseExistingKeySet|Questo parametro viene usato per specificare che è necessario usare una coppia di chiavi esistente nella compilazione di una richiesta di certificato. Se questa chiave è impostata su TRUE, è necessario specificare anche un valore per la chiave RenewalCert o il nome del contenitore. Non è necessario impostare la chiave esportabile perché non è possibile modificare le proprietà di una chiave esistente. In questo caso, non viene generato alcun materiale della chiave quando viene compilata la richiesta di certificato.|true, false|UseExistingKeySet = true|
|Protezione di dati|Specifica un valore che indica il modo in cui una chiave privata viene protetta prima dell'utilizzo.|XCN_NCRYPT_UI_NO_PROTCTION_FLAG--0</br>XCN_NCRYPT_UI_PROTECT_KEY_FLAG--1</br>XCN_NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG--2|Protezione dati = NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG|
|SuppressDefaults|Specifica un valore booleano che indica se le estensioni e gli attributi predefiniti sono inclusi nella richiesta. Le impostazioni predefinite sono rappresentate dai rispettivi identificatori di oggetto (OID).|true, false|SuppressDefaults = true|
|FriendlyName|Nome descrittivo per il nuovo certificato.|Testo|FriendlyName = "server1"|
|ValidityPeriodUnits</br>Nota: questa operazione viene usata solo quando il tipo di richiesta = cert.|Specifica un numero di unità da usare con ValidityPeriod.|Numerico|ValidityPeriodUnits = 3|
|ValidityPeriod</br>Nota: questa operazione viene usata solo quando il tipo di richiesta = cert.|VValidityPeriod deve essere un periodo di tempo plurale in inglese (Stati Uniti).|Years, months, weeks, Days, hours, minutes, seconds|ValidityPeriod = anni|

Torna al [contenuto](#BKMK_Contents)

**Estensioni**

Questa sezione è facoltativa.


|  OID estensione   | Definizione | Value |                                                                                                                                                                                                                                                                                                                                                                                                                      Esempio                                                                                                                                                                                                                                                                                                                                                                                                                       |
|------------------|------------|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    2.5.29.17     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                2.5.29.17 = "{Text}"                                                                                                                                                                                                                                                                                                                                                                                                                |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                        *continue* = "UPN =User@Domain.com&"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                       *continue* = "EMail =User@Domain.com&"                                                                                                                                                                                                                                                                                                                                                                                                        |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                        *continue* = "DNS = host. Domain. com &"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                               *continue* = "DirectoryName = CN = Name, DC = Domain, DC = com &"                                                                                                                                                                                                                                                                                                                                                                                               |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                             *continue* = "URL =<http://host.domain.com/default.html&>"                                                                                                                                                                                                                                                                                                                                                                                              |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                         *continue* = "IPAddress = 10.0.0.1 &"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                       *continue* = "RegisteredId = 1.2.3.4.5 &"                                                                                                                                                                                                                                                                                                                                                                                                       |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                      *continue* = "1.2.3.4.6.1 = {Utf8} stringa &"                                                                                                                                                                                                                                                                                                                                                                                                      |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                  *continue* = "1.2.3.4.6.2 = {ottetto} AAECAwQFBgc = &"                                                                                                                                                                                                                                                                                                                                                                                                   |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                          *continue* = "1.2.3.4.6.2 = {ottetto} {hex} 00 01 02 03 04 05 06 07 &"                                                                                                                                                                                                                                                                                                                                                                                           |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                 *continue* = "1.2.3.4.6.3 = {ASN} BAgAAQIDBAUGBw = = &"                                                                                                                                                                                                                                                                                                                                                                                                  |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                           *continue* = "1.2.3.4.6.3 = {Hex} 04 08 00 01 02 03 04 05 06 07"                                                                                                                                                                                                                                                                                                                                                                                            |
|    2.5.29.37     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                 2.5.29.37 = "{Text}"                                                                                                                                                                                                                                                                                                                                                                                                                 |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                            *continue* = "1.3.6.1.5.5.7.                                                                                                                                                                                                                                                                                                                                                                                                            |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                          *continue* = "1.3.6.1.5.5.7.3.1"                                                                                                                                                                                                                                                                                                                                                                                                          |
|    2.5.29.19     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                              "{text} CA = 0pathlength = 3"                                                                                                                                                                                                                                                                                                                                                                                                              |
|     Critical     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                 Critico = 2.5.29.19                                                                                                                                                                                                                                                                                                                                                                                                                 |
|     KeySpec      |            |       |                                                                                                                                                                                                                                                                                                                                                                                             AT_NONE--0</br>AT_SIGNATURE--2</br>AT_KEYEXCHANGE--1                                                                                                                                                                                                                                                                                                                                                                                             |
|   RequestType    |            |       |                                                                                                                                                                                                                                                                                                                                                                                   PKCS10--1</br>PKCS7--2</br>CMC--3</br>Certificato--4</br>SCEP--fd00 (64768)                                                                                                                                                                                                                                                                                                                                                                                   |
|     KeyUsage     |            |       |                                                                                                                                                                                                       CERT_DIGITAL_SIGNATURE_KEY_USAGE--80 (128)</br>CERT_NON_REPUDIATION_KEY_USAGE--40 (64)</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE--20 (32)</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE--10 (16)</br>CERT_KEY_AGREEMENT_KEY_USAGE--8</br>CERT_KEY_CERT_SIGN_KEY_USAGE--4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE--2</br>CERT_CRL_SIGN_KEY_USAGE--2</br>CERT_ENCIPHER_ONLY_KEY_USAGE--1</br>CERT_DECIPHER_ONLY_KEY_USAGE--8000 (32768)                                                                                                                                                                                                       |
| KeyUsageProperty |            |       |                                                                                                                                                                                                                                                                                                                                            NCRYPT_ALLOW_DECRYPT_FLAG--1</br>NCRYPT_ALLOW_SIGNING_FLAG--2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG--4</br>NCRYPT_ALLOW_ALL_USAGES--FFFFFF (16777215)                                                                                                                                                                                                                                                                                                                                             |
|  Protezione di dati   |            |       |                                                                                                                                                                                                                                                                                                                                                                NCRYPT_UI_NO_PROTECTION_FLAG--0</br>NCRYPT_UI_PROTECT_KEY_FLAG--1</br>NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG--2                                                                                                                                                                                                                                                                                                                                                                 |
| SubjectNameFlags |  modello  |       |                                                                           CT_FLAG_SUBJECT_REQUIRE_COMMON_NAME--40 MILIONI (1073741824)</br>CT_FLAG_SUBJECT_REQUIRE_DIRECTORY_PATH--80 MILIONI (2147483648)</br>CT_FLAG_SUBJECT_REQUIRE_DNS_AS_CN--10 MILIONI (268435456)</br>CT_FLAG_SUBJECT_REQUIRE_EMAIL--20 MILIONI (536870912)</br>CT_FLAG_OLD_CERT_SUPPLIES_SUBJECT_AND_ALT_NAME--8</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DIRECTORY_GUID--1 MILIONE (16777216)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DNS--8 MILIONI (134217728)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DOMAIN_DNS--400000 (4194304)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_EMAIL--4 MILIONI (67108864)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_SPN--800000 (8388608)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_UPN--2 MILIONI (33554432)                                                                            |
|  X500NameFlags   |            |       | CERT_NAME_STR_NONE--0</br>CERT_OID_NAME_STR--2</br>CERT_X500_NAME_STR--3</br>CERT_NAME_STR_SEMICOLON_FLAG--40 MILIONI (1073741824)</br>CERT_NAME_STR_NO_PLUS_FLAG--20 MILIONI (536870912)</br>CERT_NAME_STR_NO_QUOTING_FLAG--10 MILIONI (268435456)</br>CERT_NAME_STR_CRLF_FLAG--8 MILIONI (134217728)</br>CERT_NAME_STR_COMMA_FLAG--4 MILIONI (67108864)</br>CERT_NAME_STR_REVERSE_FLAG--2 MILIONI (33554432)</br>CERT_NAME_STR_FORWARD_FLAG--1 MILIONE (16777216)</br>CERT_NAME_STR_DISABLE_IE4_UTF8_FLAG--10000 (65536)</br>CERT_NAME_STR_ENABLE_T61_UNICODE_FLAG--20000 (131072)</br>CERT_NAME_STR_ENABLE_UTF8_UNICODE_FLAG--40000 (262144)</br>CERT_NAME_STR_FORCE_UTF8_DIR_STR_FLAG--80000 (524288)</br>CERT_NAME_STR_DISABLE_UTF8_DIR_STR_FLAG--100000 (1048576)</br>CERT_NAME_STR_ENABLE_PUNYCODE_FLAG--200000 (2097152) |

Torna al [contenuto](#BKMK_Contents)

> [!NOTE]
> SubjectNameFlags consente al file INF di specificare quali campi dell'estensione Subject e SubjectAltName devono essere popolati automaticamente da certreq in base alle proprietà correnti dell'utente o del computer corrente: nome DNS, UPN e così via. Se si usa il valore letterale "template", vengono invece usati i flag del nome del modello. Questo consente l'uso di un singolo file INF in più contesti per generare richieste con informazioni sul soggetto specifiche del contesto.
>
> X500NameFlags specifica i flag da passare direttamente all'API CertStrToName quando il valore delle chiavi INF del soggetto viene convertito in un nome distinto con codifica ASN. 1.

Per richiedere un certificato basato su certreq-new, usare i passaggi dell'esempio seguente:

> [!WARNING]
> Il contenuto di questo argomento si basa sulle impostazioni predefinite per Servizi certificati Active Directory di Windows Server 2008. ad esempio, impostando la lunghezza della chiave su 2048, selezionando il provider di archiviazione chiavi del software Microsoft come CSP e usando Secure Hash Algorithm 1 (SHA1). Valutare queste selezioni in base ai requisiti dei criteri di sicurezza della società.

Per creare una copia del file dei criteri (. inf) e salvare l'esempio seguente nel blocco note e salvarlo come RequestConfig. inf:
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
Nel computer per cui si richiede un certificato digitare il comando seguente:
```
CertReq –New RequestConfig.inf CertRequest.req 
```
Nell'esempio seguente viene illustrata l'implementazione della sintassi della sezione [Strings] per OID e altri difficili da interpretare. Nuovo esempio di sintassi {text} per l'estensione EKU, che usa un elenco delimitato da virgole di OID:
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
Torna al [contenuto](#BKMK_Contents)

## <a name="BKMK_accept"></a>Certreq-accetta

```
CertReq -accept [Options] [CertChainFileIn | FullResponseFileIn | CertFileIn]
```
Il parametro – Accept collega la chiave privata generata in precedenza al certificato emesso e rimuove la richiesta di certificato in sospeso dal sistema in cui è richiesto il certificato (se è presente una richiesta corrispondente).

Questo esempio può essere usato per accettare manualmente un certificato:
```
certreq -accept certnew.cer 
```

> [!WARNING]
> Il verbo-Accept, le opzioni-user e-Machine indicano se il certificato da installare deve essere installato in un contesto utente o computer. Se è presente una richiesta in attesa in uno dei due contesti che corrisponde alla chiave pubblica installata, queste opzioni non sono necessarie. Se non sono presenti richieste in attesa, è necessario specificare una di queste.

Torna al [contenuto](#BKMK_Contents)

## <a name="BKMK_policy"></a>Certreq-criteri

```
certreq -policy [-attrib AttributeString] [-binary] [-cert CertID] [RequestFileIn [PolicyFileIn [RequestFileOut [PKCS10FileOut]]]]
```
-   Il file di configurazione che definisce i vincoli applicati a un certificato della CA quando viene definita subordinazione qualificata è denominato Policy. inf.
-   Per un esempio del file Policy. inf, vedere l' [appendice a della pianificazione e implementazione della certificazione incrociata e della subordinazione qualificata](https://technet.microsoft.com/library/cc738878(WS.10).aspx) white paper.
-   Se si digita certreq-policy senza parametri aggiuntivi, verrà aperta una finestra di dialogo in cui è possibile selezionare il valore Fie richiesto (req, CMC, txt, der, CER o CRT). Quando si seleziona il file richiesto e si fa clic sul pulsante Apri, viene aperta un'altra finestra di dialogo per selezionare il file INF.

È possibile usare questo esempio per compilare una richiesta di certificato incrociato:
```
certreq -policy Certsrv.req Policy.inf newcertsrv.req 
```
Torna al [contenuto](#BKMK_Contents)

## <a name="BKMK_sign"></a>Certreq-Sign

```
certreq -sign [Options] [RequestFileIn [RequestFileOut]]
```
-   Se si digita certreq-Sign senza parametri aggiuntivi, verrà aperta una finestra di dialogo in cui è possibile selezionare il file richiesto (req, CMC, txt, der, CER o CRT).
-   La firma della richiesta di subordinazione qualificata può richiedere credenziali di amministratore dell'organizzazione. Si tratta di una procedura consigliata per emettere i certificati di firma per la subordinazione qualificata.
-   Il certificato usato per firmare la richiesta di subordinazione qualificata viene creato usando il modello di subordinazione qualificato. Enterprise Admins dovrà firmare la richiesta o concedere le autorizzazioni utente per gli utenti che firmano il certificato.
-   Quando si firma la richiesta CMC, potrebbe essere necessario che più personale firmi questa richiesta, a seconda del livello di garanzia associato alla subordinazione qualificata.
-   Se la CA padre della CA subordinata che si sta installando è offline, è necessario ottenere il certificato della CA per la CA subordinata qualificata dal padre offline. Se la CA padre è online, specificare il certificato della CA per la CA subordinata qualificata durante l'installazione guidata di Servizi certificati.

La sequenza di comandi seguente mostrerà come creare una nuova richiesta di certificato, firmarla e inviarla:
```
certreq -new policyfile.inf MyRequest.req
certreq -sign MyRequest.req MyRequest_Sign.req
certreq -submit MyRequest_Sign.req MyRequest_cert.cer 
```
Torna al [contenuto](#BKMK_Contents)

## <a name="BKMK_enroll"></a>Certreq-registrazione

Per eseguire la registrazione a un certificato
```
certreq –enroll [Options] TemplateName
```
Per rinnovare un certificato esistente
```
certreq –enroll –cert CertId [Options] Renew [ReuseKeys]
```
È possibile rinnovare solo i certificati che sono ora validi. I certificati scaduti non possono essere rinnovati e devono essere sostituiti con un nuovo certificato.

Di seguito è riportato un esempio di rinnovo di un certificato con il numero di serie:
```
certreq –enroll -machine –cert "61 2d 3c fe 00 00 00 00 00 05" Renew
```
Di seguito è riportato un esempio di registrazione in un modello di certificato denominato WebServer usando un asterisco (*) per selezionare il server dei criteri tramite U/I:
```
certreq -enroll –machine –policyserver * "WebServer"
```
Torna al [contenuto](#BKMK_Contents)

## <a name="BKMK_Options"></a>Opzioni

|Opzioni|Descrizione|
|-------|-----------|
|-any|Forzare ICertRequest:: Submit per determinare il tipo di codifica.|
|-attrib \<AttributeString >|Specifica le coppie di stringhe di nome e valore, separate da due punti.</br>Separa le coppie di stringhe di nome e valore con \n (ad esempio, name1: Value1\nName2: value2).|
|-binario|Formatta i file di output come binari anziché con codifica Base64.|
|-PolicyServer *\<PolicyServer >*|"LDAP: *\<percorso >* "</br>Inserire l'URI o l'ID univoco per un computer che esegue il Servizio Web di informazioni sulle registrazioni di certificati.</br>Per specificare che si desidera utilizzare un file di richiesta esplorando, è sufficiente utilizzare un segno meno (-) per *\<> PolicyServer*.|
|-config \<ConfigString >|Elabora l'operazione tramite l'autorità di certificazione specificata nella stringa di configurazione, che è CAHostName\CAName. Per una connessione HTTPS, specificare l'URI del server di registrazione. Per la CA dell'archivio del computer locale, usare un segno meno (-).|
|-Anonymous|Usare le credenziali anonime per i servizi Web di registrazione certificati.|
|-Kerberos|Usare le credenziali Kerberos (dominio) per i servizi Web di registrazione certificati.|
|-ClientCertificate *\<ClientCertId >*|È possibile sostituire il *\<ClientCertID >* con un'identificazione personale del certificato, CN, EKU, template, email, UPN e la nuova sintassi nome = valore.|
|-UserName *\<username >*|Utilizzato con i servizi Web di registrazione certificati. È possibile sostituire *\<nome utente >* con il nome Sam o dominio\utente. Questa opzione è utilizzabile con l'opzione-p.|
|-p *\<Password >*|Utilizzato con i servizi Web di registrazione certificati. Sostituire *\<password >* con la password dell'utente effettivo. Questa opzione è utilizzabile con l'opzione-UserName.|
|-utente|Configura il contesto utente per una nuova richiesta di certificato o specifica il contesto per un'accettazione del certificato. Si tratta del contesto predefinito, se non ne viene specificato alcuno nel INF o nel modello.|
|-machine|Configura una nuova richiesta di certificato o specifica il contesto per un'accettazione del certificato per il contesto del computer. Per le nuove richieste è necessario che sia coerente con la chiave INF MachineKeyset e il contesto del modello. Se questa opzione non è specificata e il modello non imposta un contesto, il valore predefinito è il contesto utente.|
|-CRL|Include gli elenchi di revoche di certificati (CRL) nell'output per il file PKCS #7 con codifica Base64 specificato da CertChainFileOut o nel file con codifica Base64 specificato da RequestFileOut.|
|-RPC|Indica a Active Directory Servizi certificati (AD CS) di utilizzare una connessione al server RPC (Remote Procedure Call) invece di COM distribuito.|
|-AdminForceMachine|Usare il servizio chiave o la rappresentazione per inviare la richiesta dal contesto del sistema locale. Richiede che l'utente che richiama questa opzione sia un membro del gruppo Administrators locale.|
|-RenewOnBehalfOf|Inviare un rinnovo per conto dell'oggetto identificato nel certificato di firma. Questa impostazione CR_IN_ROBO quando si chiama [ICertRequest:: Submit](https://technet.microsoft.com/subscriptions/aa385040.aspx)|
|-f|Forza la sovrascrittura dei file esistenti. Ignora inoltre i modelli e i criteri di memorizzazione nella cache.|
|-q|Usa modalità invisibile all'utente; non visualizzare tutti i prompt interattivi.|
|-Unicode|Scrive l'output Unicode quando l'output standard viene reindirizzato o inoltrato tramite pipe a un altro comando, che può essere utile quando viene richiamato da script® Windows PowerShell).|
|-UnicodeText|Invia l'output Unicode durante la scrittura di BLOB di dati codificati in testo Base64 nei file.|

Torna al [contenuto](#BKMK_Contents)

## <a name="BKMK_Formats"></a>Formati

|Formati|Descrizione|
|-------|-----------|
|RequestFileIn|Nome del file di input con codifica Base64 o binario: la richiesta di certificato PKCS #10, la richiesta di certificato CMS, la richiesta di rinnovo del certificato PKCS #7, il certificato X. 509 da certificare o la richiesta di certificato in formato tag KeyGen.|
|RequestFileOut|Nome file di output con codifica Base64|
|CertFileOut|Nome file X-509 con codifica Base64.|
|PKCS10FileOut|Da usare solo con il verbo [certreq-policy](#BKMK_policy) . Nome del file di output PKCS10 con codifica Base64.|
|CertChainFileOut|Nome file #7 PKCS con codifica Base64.|
|FullResponseFileOut|Nome del file di risposta completo con codifica Base64.|
|PolicyFileIn|Da usare solo con il verbo [certreq-policy](#BKMK_policy) . File INF contenente una rappresentazione testuale delle estensioni utilizzate per qualificare una richiesta.|

## <a name="BKMK_Examples"></a>Esempi di certreq aggiuntivi

Gli articoli seguenti contengono esempi di utilizzo di certreq:
-   [Come richiedere un certificato con un nome alternativo oggetto personalizzato](https://technet.microsoft.com/library/ff625722.aspx)
-   [Guida al Lab di test: distribuzione di una gerarchia PKI a due livelli di Servizi certificati Active Directory](https://technet.microsoft.com/library/hh831348.aspx)
-   [Appendice 3: sintassi di certreq. exe](https://technet.microsoft.com/library/cc736326.aspx)
-   [Come creare manualmente un certificato SSL del server Web](https://blogs.technet.com/b/pki/archive/2009/08/05/how-to-create-a-web-server-ssl-certificate-manually.aspx)
-   [Richiedere un certificato di provisioning AMT usando una CA di Windows Server 2008](https://social.technet.microsoft.com/wiki/contents/articles/request-an-amt-provisioning-certificate-using-a-windows-server-2008-ca.aspx)
-   [Registrazione del certificato per System Center Operations Manager Agent](https://social.technet.microsoft.com/wiki/contents/articles/certificate-enrollment-for-system-center-operations-manager-agent.aspx)
-   [Guida dettagliata di Servizi certificati Active Directory: distribuzione della gerarchia PKI a due livelli](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)
-   [Come abilitare LDAP su SSL con un'autorità di certificazione di terze parti](https://support.microsoft.com/kb/321051)

Torna al [contenuto](#BKMK_Contents)
