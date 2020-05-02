---
title: tsecimp
description: Argomento di riferimento per TSecImp, che importa le informazioni di assegnazione da un file Extensible Markup Language (XML) nel file di sicurezza del server TAPI (Tsec. ini).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d7488ec6-0eff-45ff-89ee-9cbe752416bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: afd38f7081a9b4674eb6cac26f52849794b8d5e6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721248"
---
# <a name="tsecimp"></a>tsecimp

Importa le informazioni di assegnazione da un file Extensible Markup Language (XML) nel file di sicurezza del server TAPI (Tsec. ini). È anche possibile usare questo comando per visualizzare l'elenco di provider TAPI e i dispositivi di linea associati a ognuno di essi, convalidare la struttura del file XML senza importare il contenuto e controllare l'appartenenza al dominio.

## <a name="syntax"></a>Sintassi

```
tsecimp /f <Filename> [{/v | /u}]
tsecimp /d
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/f \<> nomefile|Obbligatorio. Specifica il nome del file XML che contiene le informazioni di assegnazione che si desidera importare.|
|/v|Convalida la struttura del file XML senza importare le informazioni nel file Tsec. ini.|
|/U|Verifica se ogni utente è un membro del dominio specificato nel file XML. Il computer in cui si utilizza questo parametro deve essere connesso alla rete. Questo parametro potrebbe rallentare significativamente le prestazioni se si elabora una grande quantità di informazioni sull'assegnazione dell'utente.|
|/d|Visualizza un elenco di provider di telefonia installati. Per ogni provider di telefonia vengono elencati i dispositivi linea associati, nonché gli indirizzi e gli utenti associati a ogni dispositivo di linea.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

-   Il file XML da cui si desidera importare le informazioni di assegnazione deve seguire la struttura descritta di seguito.  
    -   **Utente** (elemento)

        L' **utente** è l'elemento principale del file XML.
    -   Elemento **User**

        Ogni elemento **utente** contiene informazioni su un utente membro di un dominio. A ogni utente potrebbero essere assegnati uno o più dispositivi line.

        Inoltre, ogni elemento **utente** potrebbe avere un attributo denominato **NoMerge**. Quando si specifica questo attributo, tutte le assegnazioni di dispositivi linea correnti per l'utente vengono rimosse prima che vengano eseguite nuove. È possibile utilizzare questo attributo per rimuovere facilmente le assegnazioni utente indesiderate. Per impostazione predefinita, questo attributo non è impostato.

        L'elemento **User** deve contenere un solo elemento **DomainUserName** , che specifica il dominio e il nome utente dell'utente. L'elemento **User** potrebbe contenere anche un elemento **FriendlyName** , che specifica un nome descrittivo per l'utente.

        L'elemento **User** potrebbe contenere un elemento line-of- **line** . Se non è presente un elemento di **linea** , vengono rimossi tutti i dispositivi linea per questo utente.
    -   Elemento **line** -of

        L'elemento **line** -of contiene informazioni su ogni riga o dispositivo che potrebbe essere assegnato all'utente. Ogni elemento **Linet** può contenere più di un elemento **line** .
    -   Elemento **line**

        Ogni elemento **linea** specifica un dispositivo a linee. È necessario identificare ogni dispositivo a linee aggiungendo un elemento **Address** o un elemento **PermanentID** sotto l'elemento **line** .

        Per ogni elemento **linea** è possibile impostare l'attributo **Remove** . Se si imposta questo attributo, l'utente non è più assegnato a tale dispositivo di linea. Se questo attributo non è impostato, l'utente ottiene l'accesso a tale dispositivo della riga. Non viene specificato alcun errore se il dispositivo a linee non è disponibile per l'utente.

## <a name="examples"></a>Esempi
- I segmenti di codice XML di esempio seguenti illustrano l'utilizzo corretto degli elementi definiti in precedenza.  
  - Il codice seguente rimuove tutti i dispositivi a linee assegnati a User1.  
    ```
    <UserList>
      <User NoMerge=1>
        <DomainUser>domain1\user1</DomainUser>
      </User>
    </UserList>
    ```  
  - Il codice seguente rimuove tutti i dispositivi a linee assegnati a User1 prima di assegnare una riga con l'indirizzo 99999. User1 non avrà alcun altro dispositivo a linee assegnato, indipendentemente dal fatto che i dispositivi a linee siano stati assegnati in precedenza.  
    ```
    <UserList>
      <User NoMerge=1>
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
  - Il codice seguente aggiunge un dispositivo di linea per User1 senza eliminare i dispositivi linea precedentemente assegnati.  
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
  - Il codice seguente aggiunge l'indirizzo della riga 99999 e rimuove l'indirizzo della riga 88888 dall'accesso utente1.  
    ```
    <UserList>
      <User>
        <DomainUser>domain1\user1</DomainUser>
        <FriendlyName>User1</FriendlyName>
        <LineList>
          <Line>
            <Address>99999</Address>
          </Line>
          <Line Remove=1>
            <Address>88888</Address>
          </Line>
        </LineList>
      </User>
    </UserList>
    ```  
  - Il codice seguente aggiunge il dispositivo permanente 1000 e rimuove la riga 88888 dall'accesso a Utente1.  
    ```
    <UserList>
      <User>
        <DomainUser>domain1\user1</DomainUser>
        <FriendlyName>User1</FriendlyName>
        <LineList>
          <Line>
            <PermanentID>1000</PermanentID>
          </Line>
          <Line Remove=1>
            <Address>88888</Address>
          </Line>
        </LineList>
      </User>
    </UserList>
    ```

-   L'output di esempio seguente viene visualizzato dopo che è stata specificata l'opzione della riga di comando **/d** per visualizzare la configurazione TAPI corrente. Per ogni provider di telefonia vengono elencati i dispositivi linea associati, nonché gli indirizzi e gli utenti associati a ogni dispositivo di linea.  
    ```
    NDIS Proxy TAPI Service Provider
            Line: WAN Miniport (L2TP)
                    Permanent ID: 12345678910

    NDIS Proxy TAPI Service Provider
            Line: LPT1DOMAIN1\User1
                    Permanent ID: 12345678910

    Microsoft H.323 Telephony Service Provider
            Line: H323 Line
                    Permanent ID: 123456
                    Addresses:
                            BLDG1-TAPI32

    ```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Cenni preliminari sulla shell comandi](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)
