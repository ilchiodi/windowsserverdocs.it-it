---
title: Active Directory Federation Services e proprietà chiave specifica informazioni certificato
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a5307da5-02ff-4c31-80f0-47cb17a87272
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 32b0d08f678e9e612bb0ce9cc38d254564bd9b2f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444097"
---
# <a name="ad-fs-and-certificate-keyspec-property-information"></a>AD FS e proprietà KeySpec informazioni certificato
Chiave specifica ("KeySpec") è una proprietà associata a un certificato e una chiave. Specifica se una chiave privata associata a un certificato può essere utilizzata per la firma, crittografia o entrambi.   

Un valore KeySpec non corretto può causare errori di AD FS e Proxy applicazione Web, ad esempio:


- Tentativo di stabilire una connessione SSL/TLS per AD FS o il Proxy applicazione Web, senza eventi di AD FS registrati (anche se potrebbero essere registrati eventi SChannel 36888 e 36874)
- L'autenticazione basata su pagina, di form di errore all'account di accesso nel server di AD FS o WAP con alcun messaggio di errore visualizzato nella pagina.

Si potrebbe visualizzare quanto segue nel registro eventi:

    Log Name:      AD FS Tracing/Debug
    Source:        AD FS Tracing
    Date:          2/12/2015 9:03:08 AM
    Event ID:      67
    Task Category: None
    Level:         Error
    Keywords:      ADFSProtocol
    User:          S-1-5-21-3723329422-3858836549-556620232-1580884
    Computer:      ADFS1.contoso.com
    Description:
    Ignore corrupted SSO cookie.

## <a name="what-causes-the-problem"></a>Qual è la causa del problema
La proprietà KeySpec identifica come una chiave generata o recuperare da Microsoft CryptoAPI (CAPI), da un Microsoft legacy Storage Provider crittografia (CSP), può essere usata.

Valore KeySpec **1**, o **AT_KEYEXCHANGE**, può essere utilizzato per la firma e crittografia.  Un valore pari **2**, o **AT_SIGNATURE**, viene usato solo per la firma.

La configurazione errata KeySpec più comune utilizza un valore pari a 2 per un certificato diverso dal certificato di firma dei token.  

Per i certificati le cui chiavi sono stati generati usando provider di Cryptography Next Generation (CNG), non prevedono il concetto di specifica della chiave e il valore KeySpec sarà sempre zero.

Informazioni su come verificare la presenza di un valore valido di KeySpec riportato di seguito. 

### <a name="example"></a>Esempio
Un esempio di un CSP legacy è Microsoft Enhanced Cryptographic Provider. 

Formato blob della chiave RSA Microsoft CSP include un identificatore dell'algoritmo, ovvero **CALG_RSA_KEYX** oppure **CALG_RSA_SIGN**rispettivamente, per soddisfare le richieste tra <strong>AT_KEYEXCHANGE * * o * * AT _ FIRMA</strong> chiavi.

Gli identificatori di algoritmo della chiave RSA eseguire il mapping ai valori KeySpec come indicato di seguito

| Algoritmo di provider supportati| Valore di chiave specifica per le chiamate CAPI |
| --- | --- |
|CALG_RSA_KEYX: Chiave RSA che può essere utilizzato per la firma e la decrittografia| AT_KEYEXCHANGE (o KeySpec = 1)|
CALG_RSA_SIGN: Solo chiave di firma RSA |AT_SIGNATURE (o KeySpec = 2)|

## <a name="keyspec-values-and-associated-meanings"></a>KeySpec valori e significati associati
Di seguito sono i significati dei vari valori KeySpec:

|Valore KeySpec|Significa che|Uso consigliato di AD FS|
| --- | --- | --- |
|0|Il certificato è un certificato CNG|Solo certificato SSL|
|1|Per un certificato CAPI (non-CNG) legacy, la chiave può essere utilizzata per la firma e la decrittografia|    SSL, token di accesso, token certificati per le comunicazioni del servizio la decrittografia,|
|2|Per un certificato CAPI (non-CNG) legacy, la chiave può essere utilizzata solo per la firma|Non è consigliata|

## <a name="how-to-check-the-keyspec-value-for-your-certificates--keys"></a>Come controllare il valore KeySpec per i certificati o chiavi
Per visualizzare un valore di certificati è possibile usare la **certutil** strumento da riga di comando.  

Di seguito è riportato un esempio: **certutil – v – archivio my**.  Questo verrà dump le informazioni sul certificato alla schermata.

![KeySpec cert](media/AD-FS-and-KeySpec-Property/keyspec1.png)

In Cerca CERT_KEY_PROV_INFO_PROP_ID per due motivi:


1. **Tipoprovider:** ciò indica se il certificato viene utilizzato un legacy Storage Provider crittografia (CSP) o un Provider di archiviazione chiavi basati su più recente certificato Next Generation (CNG) le API.  Qualsiasi valore diverso da zero indica un provider legacy.
2. **KeySpec:** Di seguito sono valori KeySpec validi per un certificato di AD FS:

   Provider CSP legacy (Tipoprovider non è uguale a 0):

   |Utilizzo generico di AD FS certificato|Valori validi KeySpec|
   | --- | --- |
   |Comunicazione del servizio|1|
   |La decrittografia di token|1|
   |La firma di token|1 e 2|
   |SSL|1|

   Provider CNG (Tipoprovider = 0):

   |Utilizzo generico di AD FS certificato|Valori validi KeySpec|
   | --- | --- |   
   |SSL|0|

## <a name="how-to-change-the-keyspec-for-your-certificate-to-a-supported-value"></a>Come modificare keyspec per il certificato su un valore supportato
La modifica del valore KeySpec non richiede il certificato deve essere nuovamente generati o nuovamente rilasciato dall'autorità di certificazione.  È possibile modificare KeySpec importando nuovamente il certificato completato e la chiave privata da un file PFX nell'archivio certificati attenendosi alla procedura seguente:


1. Innanzitutto, controllare e prendere le autorizzazioni per chiavi private per il certificato esistente, in modo che possano essere riconfigurati se necessario dopo la reimportazione.
2. Esportare il certificato con chiave privata in un file PFX.
3. Eseguire la procedura seguente per ogni server AD FS e WAP
    1. Eliminare il certificato (da AD FS o server WAP)
    2. Aprire un prompt dei comandi di PowerShell con privilegi elevati e importare il file PFX in ogni server AD FS e WAP usando la sintassi di cmdlet seguente, specificando il valore AT_KEYEXCHANGE (che funziona per tutti gli scopi di certificati AD FS):
        1. C:\>certutil-importpfx CertFile AT_KEYEXCHANGE
        2. Immettere la password PFX
    3. Dopo aver completato il codice precedente, seguire questa procedura
        1. Controllare le autorizzazioni per chiavi private
        2. riavviare il servizio ad FS o wap





