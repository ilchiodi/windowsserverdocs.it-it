---
title: "Active Directory Federation Services e certificato proprietà chiave specifica informazioni"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a5307da5-02ff-4c31-80f0-47cb17a87272
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: db58fcce054f34c4b0a3f6725456badae9fd0468
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-and-certificate-keyspec-property-information"></a>AD FS e certificato KeySpec proprietà informazioni
Specifica di chiave ("KeySpec") è una proprietà associata a un certificato e una chiave. Specifica se è possibile utilizzare una chiave privata associata a un certificato per la firma, crittografia o entrambi.   

Un valore non corretto KeySpec può causare errori di ADFS e Proxy applicazione Web, ad esempio:


- Impossibile stabilire una connessione SSL/TLS per ADFS o il Proxy dell'applicazione Web, con nessun evento ADFS registrato (anche se potrebbero essere registrati eventi SChannel 36888 e 36874)
- Errore di accesso in ADFS o WAP costituisce pagina autenticazione basata su, senza alcun messaggio di errore visualizzato nella pagina.

Si possono vedere gli argomenti seguenti nel registro eventi:

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

## <a name="what-causes-the-problem"></a>Qual è la causa il problema
La proprietà KeySpec identifica come una chiave generata o recuperare da Microsoft CryptoAPI (CAPI), da un Microsoft legacy archiviazione Provider crittografia (CSP), è possibile utilizzare.

Il valore KeySpec **1**, o **AT_KEYEXCHANGE**, può essere utilizzato per la firma e crittografia.  Il valore **2**, o **AT_SIGNATURE**, viene utilizzato solo per la firma.

La configurazione errata KeySpec più comune è con un valore pari a 2 per un certificato diverso dal certificato di firma di token.  

Per i certificati il cui chiavi sono state generate tramite provider Cryptography Next Generation (CNG), non è previsto di specifica della chiave e il valore di KeySpec sarà sempre pari a zero.

Scopri come cercare un valore valido di KeySpec riportato di seguito. 

### <a name="example"></a>Esempio
Un esempio di un provider CSP legacy è Microsoft Enhanced Cryptographic Provider. 

Formato del blob chiave RSA Microsoft CSP include un identificatore dell'algoritmo, ovvero **CALG_RSA_KEYX** o **CALG_RSA_SIGN**, rispettivamente, per soddisfare le richieste per una * * AT_KEYEXCHANGE * * o **AT_SIGNATURE** chiavi.
  
Gli identificatori di algoritmo a chiave RSA mapping KeySpec valori come indicato di seguito

| Algoritmo provider supportati| Valore di chiave specifica per le chiamate CAPI |
| --- | --- |
|CALG_RSA_KEYX: Chiave RSA che può essere utilizzato per la firma e la decrittografia| AT_KEYEXCHANGE (o KeySpec = 1)|
CALG_RSA_SIGN: Firma RSA solo chiave |AT_SIGNATURE (o KeySpec = 2)|

## <a name="keyspec-values-and-associated-meanings"></a>Valori KeySpec e significati associati
Di seguito sono il significato dei valori KeySpec diversi:

|Valore KeySpec|Significa|Utilizzo di AD FS consigliato|
| --- | --- | --- |
|0|Il certificato è un certificato CNG|Solo i certificati SSL|
|1|Per un certificato CAPI (non CNG) legacy, è possibile utilizzare la chiave per la firma e la decrittografia|    SSL, la firma, token certificati per le comunicazioni del servizio, decrittografia di token|
|2|Per un certificato CAPI (non CNG) legacy, la chiave può essere utilizzata solo per la firma|Non è consigliata|

## <a name="how-to-check-the-keyspec-value-for-your-certificates--keys"></a>Come controllare il valore KeySpec per i certificati o chiavi
Per visualizzare un valore di certificati, è possibile utilizzare il **certutil** lo strumento da riga di comando.  

Di seguito è riportato un esempio: **certutil – v – store my**.  Questo viene eseguito il dump le informazioni sul certificato sullo schermo.

![KeySpec cert](media/AD-FS-and-KeySpec-Property/keyspec1.png)

In Cerca CERT_KEY_PROV_INFO_PROP_ID per due scopi:


1. **ProviderType:** indica se il certificato viene utilizzato un legacy archiviazione Provider crittografia (CSP) o un Provider di archiviazione chiavi basati su più recenti certificato Next Generation (CNG) API.  Qualsiasi valore diverso da zero indica un provider legacy.
2.  **KeySpec:** valori KeySpec valido per un certificato ADFS:

    Provider CSP legacy (ProviderType non è uguale a 0):
    
    |Scopo del certificato di ADFS di Active Directory|Valori validi KeySpec|
    | --- | --- |
    |Comunicazione dei servizi|1|
    |La decrittografia di token|1|
    |La firma di token|1 e 2|
    |SSL|1|

    Provider di CNG (ProviderType = 0):
    |Scopo del certificato di ADFS di Active Directory|Valori validi KeySpec|
    | --- | --- |   
    |SSL|0|

## <a name="how-to-change-the-keyspec-for-your-certificate-to-a-supported-value"></a>Come modificare il keyspec per il certificato a un valore supportato
La modifica del valore KeySpec non richiede il certificato deve essere nuovamente generato o nuovamente rilasciato dall'autorità di certificazione.  È possibile modificare il KeySpec da importare nuovamente il certificato completo e la chiave privata da un file PFX nell'archivio certificati tramite la procedura seguente:


1. Prima di tutto, controllare e registrare le autorizzazioni di chiave private per il certificato esistente, in modo che possano essere riconfigurati se necessario dopo importare nuovamente.
2. Esportare il certificato, tra cui la chiave privata in un file PFX.
3. Eseguire i passaggi seguenti per ogni server ADFS e WAP
    1. Eliminare il certificato (da AD FS / server WAP)
    2. Aprire un prompt dei comandi di PowerShell con privilegi elevati e importare il file PFX in ogni server ADFS e WAP utilizzando la seguente sintassi del cmdlet, specificando il valore AT_KEYEXCHANGE (che funziona per tutti gli scopi di certificati AD FS):
        1. C:\ > certutil – importpfx certfile.pfx AT_KEYEXCHANGE
        2. Immettere la password PFX
    3. Una volta completata la precedente, eseguire le operazioni seguenti
        1. Controllare le autorizzazioni di chiave private
        2. Riavviare il servizio adfs o wap





