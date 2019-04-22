---
title: tsecimp
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7488ec6-0eff-45ff-89ee-9cbe752416bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 38582706dfa5db2b5069415b81dafc533c8a89b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822102"
---
# <a name="tsecimp"></a>tsecimp



Importa le informazioni sulle assegnazioni da un file Extensible Markup Language (XML) in file di sicurezza del server TAPI (Tsec). È anche possibile usare questo comando per visualizzare l'elenco dei provider TAPI e i dispositivi di righe associato a ognuna di esse, convalidare la struttura del file XML senza l'importazione del contenuto e controllare l'appartenenza al dominio.

## <a name="syntax"></a>Sintassi

```
tsecimp /f <Filename> [{/v | /u}]
tsecimp /d
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/f \<nomefile >|Obbligatorio. Specifica il nome del file XML che contiene le informazioni di assegnazione che si desidera importare.|
|/v|Convalida la struttura del file XML senza l'importazione delle informazioni nel file Tsec.|
|/u|Verifica se ogni utente è un membro del dominio specificato nel file XML. Il computer in cui si utilizza questo parametro deve essere connesso alla rete. Questo parametro può rallentare in modo significativo le prestazioni se si stanno elaborando una grande quantità di informazioni sull'assegnazione di utente.|
|/d|Visualizza un elenco di provider di telefonia installati. Per ogni provider di telefonia, sono elencati i dispositivi di riga associato, nonché gli indirizzi e gli utenti associati a ogni dispositivo di riga.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Il file XML da cui si desidera importare le informazioni sulle assegnazioni deve seguire la struttura descritta di seguito.  
    -   **Elencoutenti** elemento

        Il **UserList** è l'elemento principale del file XML.
    -   **Utente** elemento

        Ciascuna **utente** elemento contiene le informazioni relative a un utente membro di un dominio. Ogni utente potrebbe essere assegnato uno o più dispositivi di riga.

        Inoltre, ogni **utente** elemento potrebbe avere un attributo denominato **NoMerge**. Quando questo attributo viene specificato, vengono rimosse tutte le assegnazioni di dispositivo riga corrente per l'utente prima che vengano apportate nuove. È possibile usare questo attributo per rimuovere facilmente le assegnazioni utente indesiderate. Per impostazione predefinita, questo attributo non è impostato.

        Il **utente** l'elemento deve contenere una singola **DomainUserName** elemento che specifica il nome utente e dominio dell'utente. Il **utente** potrebbe anche contenere un elemento **FriendlyName** elemento che specifica un nome descrittivo per l'utente.

        Il **utente** potrebbe contenere un elemento **LineList** elemento. Se un **LineList** elemento non è presente, vengono rimossi tutti i dispositivi di riga per l'utente corrente.
    -   **LineList** elemento

        Il **LineList** elemento contiene informazioni su ogni riga o un dispositivo che è possibile assegnare all'utente. Ciascuna **LineList** può contenere più di un elemento **riga** elemento.
    -   **Riga** elemento

        Ciascuna **riga** elemento specifica un dispositivo di riga. È necessario identificare ogni dispositivo riga aggiungendo una un' **indirizzi** elemento o un **PermanentID** elemento sotto il **riga** elemento.

        Per ognuno **Line** elemento, è possibile impostare il **rimuovere** attributo. Se si imposta questo attributo, l'utente non è più assegnata la periferica di tale linea. Se questo attributo non è impostato, l'utente accede a tale dispositivo di riga. Se il dispositivo di riga non è disponibile per l'utente, viene restituito alcun errore.

## <a name="examples"></a>Esempi
-   I segmenti di codice XML di esempio seguenti illustrano l'uso corretto degli elementi definiti in precedenza.  
    -   Il codice seguente rimuove tutti i dispositivi di riga assegnati a User1.  
        ```
        <UserList>
          <User NoMerge="1">
            <DomainUser>domain1\user1</DomainUser>
          </User>
        </UserList>
        ```  
    -   Il codice seguente rimuove tutti i dispositivi di riga assegnati a User1 prima di assegnare una sola riga con indirizzo 99999. User1 non disporrebbe altri righe dispositivi assegnati, indipendentemente dal fatto che tutti i dispositivi di riga sono stati assegnati in precedenza.  
        ```
        <UserList>
          <User NoMerge="1">
            <DomainUser>domain1\user1</DomainUser>
            <FriendlyName>User1</FriendlyName>
            <LineList>
              <Line>
                <Address>99999</Address>
              </Line>
            </LineList>
          </User>
        </UserList>
        
        ```  
    -   Il codice seguente aggiunge un dispositivo di riga per l'utente1 senza eliminare tutti i dispositivi riga assegnato in precedenza.  
        ```
        <UserList>
          <User>
            <DomainUser>domain1\user1</DomainUser>
            <FriendlyName>User1</FriendlyName>
            <LineList>
              <Line>
                <Address>99999</Address>
              </Line>
            </LineList>
          </User>
        </UserList>
        
        ```  
    -   Il codice seguente aggiunge riga indirizzo 99999 e rimuove l'indirizzo di riga 88888 dall'accesso di User1.  
        ```
        <UserList>
          <User>
            <DomainUser>domain1\user1</DomainUser>
            <FriendlyName>User1</FriendlyName>
            <LineList>
              <Line>
                <Address>99999</Address>
              </Line>
              <Line Remove="1">
                <Address>88888</Address>
              </Line>
            </LineList>
          </User>
        </UserList>
        
        ```  
    -   Il codice seguente aggiunge il dispositivo di permanente 1000 e rimuove riga 88888 da accesso dell'User1.  
        ```
        <UserList>
          <User>
            <DomainUser>domain1\user1</DomainUser>
            <FriendlyName>User1</FriendlyName>
            <LineList>
              <Line>
                <PermanentID>1000</PermanentID>
              </Line>
              <Line Remove="1">
                <Address>88888</Address>
              </Line>
            </LineList>
          </User>
        </UserList>
        
        
        ```  
-   L'output di esempio seguente viene visualizzato dopo il **/d** opzione della riga di comando è specificata per visualizzare la configurazione TAPI corrente. Per ogni provider di telefonia, sono elencati i dispositivi di riga associato, nonché gli indirizzi e gli utenti associati a ogni dispositivo di riga.  
    ```
    NDIS Proxy TAPI Service Provider
            Line: "WAN Miniport (L2TP)"
                    Permanent ID: 12345678910
    
    NDIS Proxy TAPI Service Provider
            Line: "LPT1DOMAIN1\User1"
                    Permanent ID: 12345678910
    
    Microsoft H.323 Telephony Service Provider
            Line: "H323 Line"
                    Permanent ID: 123456
                    Addresses:
                            BLDG1-TAPI32
    
    ```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

[Cenni preliminari sulla shell comandi](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)