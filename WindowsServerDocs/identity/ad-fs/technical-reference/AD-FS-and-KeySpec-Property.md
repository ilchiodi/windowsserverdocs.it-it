---
title: Informazioni sulle proprietà di specifica della chiave Active Directory Federation Services e del certificato
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.assetid: a5307da5-02ff-4c31-80f0-47cb17a87272
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: e3ddc427d84a79d831c61cad8087dbfa1d3fb564
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860244"
---
# <a name="ad-fs-and-certificate-keyspec-property-information"></a>Informazioni sulle proprietà delle specifiche della AD FS e del certificato
La specifica della chiave ("chiave specifica") è una proprietà associata a un certificato e una chiave. Specifica se è possibile usare una chiave privata associata a un certificato per la firma, la crittografia o entrambi.   

Un valore di una specifica di un valore non corretto può causare errori di AD FS e proxy dell'applicazione Web, ad esempio:


- Errore durante il tentativo di stabilire una connessione SSL/TLS a AD FS o al proxy dell'applicazione Web, senza AD FS eventi registrati (sebbene sia possibile registrare gli eventi SChannel 36888 e 36874)
- Impossibile eseguire l'accesso alla pagina di autenticazione basata su form AD FS o WAP, senza visualizzare alcun messaggio di errore nella pagina.

Nel registro eventi potrebbe essere visualizzato quanto segue:

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

## <a name="what-causes-the-problem"></a>Causa del problema
La proprietà chiave specifica identifica il modo in cui può essere usata una chiave generata o recuperata da Microsoft CryptoAPI (CAPI), da un provider di archiviazione di crittografia (CSP) legacy Microsoft.

Per la firma e la crittografia è possibile usare un valore di una specifica di una specifica di un valore pari a **1**o **AT_KEYEXCHANGE**.  Il valore **2**, o **AT_SIGNATURE**, viene utilizzato solo per la firma.

La configurazione errata della specifica di più comune sta usando un valore di 2 per un certificato diverso dal certificato per la firma di token.  

Per i certificati le cui chiavi sono state generate usando i provider CNG (Cryptography Next Generation), non esiste alcun concetto di specifica della chiave e il valore della chiave specifica sarà sempre zero.

Vedere come verificare la presenza di un valore di una specifica di un valore valido. 

### <a name="example"></a>Esempio
Un esempio di CSP legacy è Microsoft Enhanced Cryptographic Provider. 

Il formato BLOB della chiave Microsoft RSA CSP include un identificatore di algoritmo, **CALG_RSA_KEYX** o **CALG_RSA_SIGN**rispettivamente, per soddisfare le richieste per le chiavi di <strong>AT_KEYEXCHANGE * * o * * AT_SIGNATURE</strong> .

Gli identificatori dell'algoritmo della chiave RSA vengono mappati ai valori delle specifiche chiave come indicato di seguito

| Algoritmo supportato dal provider| Valore della specifica chiave per le chiamate CAPI |
| --- | --- |
|CALG_RSA_KEYX: chiave RSA che può essere usata per la firma e la decrittografia| AT_KEYEXCHANGE o la specifica di una scheda|
CALG_RSA_SIGN: chiave solo firma RSA |AT_SIGNATURE (o la specifica di una delle due)|

## <a name="keyspec-values-and-associated-meanings"></a>Valori delle specifiche e significati associati
Di seguito sono riportati i significati dei diversi valori delle specifiche di codice:

|Valore della specifica|Significa|Uso consigliato AD FS|
| --- | --- | --- |
|0|Il certificato è un certificato CNG|Solo certificato SSL|
|1|Per un certificato CAPI legacy (non CNG), la chiave può essere usata per la firma e la decrittografia|    SSL, firma di token, decrittografia di token, certificati di comunicazione del servizio|
|2|Per un certificato CAPI legacy (non CNG), la chiave può essere usata solo per la firma|non consigliato|

## <a name="how-to-check-the-keyspec-value-for-your-certificates--keys"></a>Come controllare il valore della specifica della chiave per i certificati o le chiavi
Per visualizzare un valore di certificati, è possibile usare lo strumento da riga di comando **certutil** .  

Di seguito è riportato un esempio: **certutil – v – Store My**.  Il dump delle informazioni del certificato verrà eseguito sullo schermo.

![Certificato di specifica della scheda](media/AD-FS-and-KeySpec-Property/keyspec1.png)

In CERT_KEY_PROV_INFO_PROP_ID cercare due elementi:


1. **ProviderType:** indica se il certificato usa un provider di archiviazione di crittografia (CSP) legacy o un provider di archiviazione chiavi basato sulle API CNG (certificate Next Generation) più recenti.  Qualsiasi valore diverso da zero indica un provider Legacy.
2. **Specifica della scheda:** Di seguito sono riportati i valori validi delle specifiche per un certificato AD FS:

   Provider CSP legacy (ProviderType diverso da 0):

   |Scopo del certificato AD FS|Valori validi per le specifiche|
   | --- | --- |
   |Comunicazione del servizio|1|
   |Decrittografia di token|1|
   |Firma di token|1 e 2|
   |SSL|1|

   Provider CNG (ProviderType = 0):

   |Scopo del certificato AD FS|Valori validi per le specifiche|
   | --- | --- |   
   |SSL|0|

## <a name="how-to-change-the-keyspec-for-your-certificate-to-a-supported-value"></a>Come modificare la specifica di una scheda per il certificato in un valore supportato
Per modificare il valore di una specifica di base non è necessario che il certificato venga rigenerato o riemesso dall'autorità di certificazione.  È possibile modificare la specifica della chiave importando nuovamente il certificato completo e la chiave privata da un file PFX nell'archivio certificati usando la procedura seguente:


1. Prima di tutto, controllare e registrare le autorizzazioni della chiave privata per il certificato esistente in modo che possano essere riconfigurate, se necessario, dopo la reimportazione.
2. Esportare il certificato, inclusa la chiave privata, in un file PFX.
3. Per ogni AD FS e server WAP, seguire questa procedura.
    1. Eliminare il certificato (dal server AD FS/WAP)
    2. Aprire un prompt dei comandi di PowerShell con privilegi elevati e importare il file PFX in ogni AD FS e server WAP usando la sintassi del cmdlet riportata di seguito, specificando il valore di AT_KEYEXCHANGE (che funziona per tutti gli scopi di AD FS certificate):
        1. C:\>certutil – importpfx CertFile. pfx AT_KEYEXCHANGE
        2. Immettere la password PFX
    3. Al termine dell'operazione, eseguire le operazioni seguenti
        1. controllare le autorizzazioni per la chiave privata
        2. riavviare il servizio ADFS o WAP





