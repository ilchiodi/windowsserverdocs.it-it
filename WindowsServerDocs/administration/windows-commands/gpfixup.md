---
title: gpfixup
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2b145410-fc75-4526-932d-f16b7ee3aaef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e32427369f1664476c81a81353ae8869ec0c2ff3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375675"
---
# <a name="gpfixup"></a>gpfixup



Correggere le dipendenze di nome di dominio nei collegamenti oggetti Criteri di gruppo e criteri di gruppo dopo l'operazione di ridenominazione di un dominio. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
Gpfixup [/v] 
[/olddns:<OLDDNSNAME> /newdns:<NEWDNSNAME>] 
[/oldnb:<OLDFLATNAME> /newnb:<NEWFLATNAME>] 
[/dc:<DCNAME>] [/sionly] 
[/user:<USERNAME> [/pwd:{<PASSWORD>|*}]] [/?]
```

### <a name="parameters"></a>Parametri

|       Parametro       |                                                                                                                                                                                                                               Descrizione                                                                                                                                                                                                                               |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          /v           |                                                                                                                                                      Consente di visualizzare messaggi di stato dettagliati.</br>Se si omette questo parametro, solo i messaggi di errore o un messaggio di riepilogo dello stato di **SUCCESSO** o **ERRORE** viene visualizzata.                                                                                                                                                       |
| /olddns: > \<OLDDNSNAME |                                                                                                           Specifica il nome DNS precedente del dominio rinominato come *\<OLDDNSNAME >* quando l'operazione di ridenominazione del dominio modifica il nome DNS di un dominio. È possibile utilizzare questo parametro solo se si utilizza anche il **/newdns** parametro per specificare un nuovo nome di dominio DNS.                                                                                                            |
| /newdns: > \<NEWDNSNAME |                                                                                                          Specifica il nuovo nome DNS del dominio rinominato come *\<NEWDNSNAME >* quando l'operazione di ridenominazione del dominio modifica il nome DNS di un dominio. È possibile utilizzare questo parametro solo se si utilizza anche il **/olddns** parametro per specificare il nome di dominio DNS precedente.                                                                                                           |
| /oldnb: > \<OLDFLATNAME |                                                                                                        Specifica il nome NetBIOS precedente del dominio rinominato come *\<OLDFLATNAME >* quando l'operazione di ridenominazione del dominio modifica il nome NetBIOS di un dominio. È possibile utilizzare questo parametro solo se si utilizza il **/newnb** parametro per specificare un nuovo nome NetBIOS del dominio.                                                                                                        |
| /newnb: > \<NEWFLATNAME |                                                                                                       Specifica il nuovo nome NetBIOS del dominio rinominato come *\<NEWFLATNAME >* quando l'operazione di ridenominazione del dominio modifica il nome NetBIOS di un dominio. È possibile utilizzare questo parametro solo se si utilizza il **/oldnb** parametro per specificare il nome NetBIOS del dominio precedente.                                                                                                       |
|     /DC: > \<DCNAME     | Connettersi al controller di dominio denominato *\<DCNAME >* , ovvero un nome DNS o un nome NetBIOS. *\<DCNAME >* deve ospitare una replica scrivibile della partizione di directory del dominio come indicato da uno dei seguenti elementi:</br>-Il nome DNS *\<NEWDNSNAME >* tramite **/newdns**</br>-Il nome NetBIOS *\<NEWFLATNAME >* tramite **/newnb**</br>Se questo parametro non viene utilizzato, connettersi a qualsiasi controller di dominio nel dominio rinominato indicato da *\<NEWDNSNAME >* o *\<NEWFLATNAME >* . |
|        /sionly        |                                                                                                                           Esegue solo la correzione di criteri di gruppo che si riferisce all'installazione del software gestito (l'estensione Installazione Software basata su criteri di gruppo). Ignorare le azioni che consentono di risolvere i collegamenti di criteri di gruppo e i percorsi SYSVOL in oggetti Criteri di gruppo.                                                                                                                           |
|   /User: \<USERNAME >   |                                                                                                                                   Esegue questo comando nel contesto di sicurezza dell'utente *\<USERNAME >* , in cui *\<USERNAME >* è nel formato dominio\utente.</br>Se si omette questo parametro, questo comando viene eseguito come utente connesso.                                                                                                                                    |
|   /pwd: {\<PASSWORD >   |                                                                                                                                                                                                                                   \*}                                                                                                                                                                                                                                   |
|          /?           |                                                                                                                                                                                                                  Visualizza la Guida al prompt dei comandi.                                                                                                                                                                                                                   |

## <a name="remarks"></a>Note

-   Il comando **gpfixup** è disponibile in windows Server 2008 R2 e windows Server 2008, tranne che nelle installazioni dei componenti di base del server.
-   Sebbene la Console Gestione Criteri di gruppo (GPMC) venga distribuita con Windows Server 2008 R2 e Windows Server 2008, è necessario installare Criteri di gruppo gestione come funzionalità tramite Server Manager.

## <a name="BKMK_Examples"></a>Esempi

Questo esempio si presuppone di avere già eseguito un'operazione di ridenominazione del dominio in cui è stato modificato il nome DNS da **MyOldDnsName** a **MyNewDnsName**, e il nome NetBIOS da **MyOldNetBIOSName** a **MyNewNetBIOSName**. In questo esempio, utilizziamo il **gpfixup** comando per connettersi al controller di dominio denominato **MyDcDnsName** e riparare oggetti Criteri di gruppo e criteri di gruppo e collegamenti aggiornando il vecchio nome di dominio incorporati in oggetti Criteri di gruppo e collegamenti. Output di errore e di stato viene salvato in un file denominato **gpfixup.log**.
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /oldnb:MyOldNetBIOSName /newnb:MyNewNetBIOSName /dc:MyDcDnsName 2>&1 >gpfixup.log
```
In questo esempio è identico a quello precedente, ad eccezione del fatto che assume che il nome NetBIOS del dominio non è stato modificato durante il dominio di operazione di ridenominazione.
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /dc:MyDcDnsName 2>&1 >gpfixup.log
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Amministrazione della ridenominazione Dominio di Active Directory](https://go.microsoft.com/fwlink/?LinkId=198385)
-   [Criteri di gruppo TechCenter](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)