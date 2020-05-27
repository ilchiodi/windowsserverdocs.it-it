---
title: gpfixup
description: Argomento di riferimento per il comando gpfixup, che corregge le dipendenze dei nomi di dominio negli oggetti Criteri di gruppo e Criteri di gruppo i collegamenti dopo un'operazione di ridenominazione del dominio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2b145410-fc75-4526-932d-f16b7ee3aaef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 14a225209e719707093ddd87918587e24e4572a6
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818871"
---
# <a name="gpfixup"></a>gpfixup

Corregge le dipendenze dei nomi di dominio negli oggetti Criteri di gruppo e nei collegamenti Criteri di gruppo dopo un'operazione di ridenominazione del dominio. Per usare questo comando, è necessario installare Criteri di gruppo Management come funzionalità tramite Server Manager.

## <a name="syntax"></a>Sintassi

```
gpfixup [/v]
[/olddns:<olddnsname> /newdns:<newdnsname>]
[/oldnb:<oldflatname> /newnb:<newflatname>]
[/dc:<dcname>] [/sionly]
[/user:<username> [/pwd:{<password>|*}]] [/?]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- |------------ |
| /v | Consente di visualizzare messaggi di stato dettagliati. Se questo parametro non viene utilizzato, vengono visualizzati solo i messaggi di errore o un messaggio di stato di riepilogo che indica, **esito positivo** o **negativo** . |
| /olddns:`<olddnsname>` | Specifica il vecchio nome DNS del dominio rinominato come `<olddnsname>` quando l'operazione di ridenominazione del dominio viene modificato il nome DNS di un dominio. È possibile utilizzare questo parametro solo se si utilizza anche il **/newdns** parametro per specificare un nuovo nome di dominio DNS. |
| /newdns:`<newdnsname>` | Specifica il nuovo nome DNS del dominio rinominato come `<newdnsname>` quando l'operazione di ridenominazione del dominio viene modificato il nome DNS di un dominio. È possibile utilizzare questo parametro solo se si utilizza anche il **/olddns** parametro per specificare il nome di dominio DNS precedente. |
| /oldnb:`<oldflatname>` | Specifica il vecchio nome NetBIOS del dominio rinominato come `<oldflatname>` quando l'operazione di ridenominazione del dominio viene modificato il nome NetBIOS di un dominio. È possibile utilizzare questo parametro solo se si utilizza il **/newnb** parametro per specificare un nuovo nome NetBIOS del dominio. |
| /newnb:`<newflatname>` | Specifica il nuovo nome NetBIOS del dominio rinominato come `<newflatname>` quando l'operazione di ridenominazione del dominio viene modificato il nome NetBIOS di un dominio. È possibile utilizzare questo parametro solo se si utilizza il **/oldnb** parametro per specificare il nome NetBIOS del dominio precedente. |
| /DC`<dcname>` | Connettersi al controller di dominio denominato `<dcname>` (un nome DNS o un nome NetBIOS). `<dcname>` è necessario ospitare una replica scrivibile della partizione di directory dominio come indicato da uno dei seguenti:<ul><li>Nome DNS `<newdnsname>` tramite **/newdns**</li><li>Nome NetBIOS `<newflatname>` tramite **/newnb**</br>Se questo parametro non viene utilizzato, è possibile connettersi a qualsiasi controller di dominio nel dominio rinominato indicato da `<newdnsname>` o `<newflatname>` .</li></ul> |
| /sionly | Esegue solo la correzione di criteri di gruppo che si riferisce all'installazione del software gestito (l'estensione Installazione Software basata su criteri di gruppo). Ignorare le azioni che consentono di risolvere i collegamenti di criteri di gruppo e i percorsi SYSVOL in oggetti Criteri di gruppo. |
| /User`<username>` |Esegue il comando nel contesto di sicurezza dell'utente `<username>`, dove `<username>` è nel formato dominio\utente. Se questo parametro non viene usato, questo comando viene eseguito come utente connesso. |
| /pwd`{<password> | *}` | Specifica la password per l'utente. |
| /? | Visualizza la Guida al prompt dei comandi. |

### <a name="examples"></a>Esempi

Questo esempio si presuppone di avere già eseguito un'operazione di ridenominazione del dominio in cui è stato modificato il nome DNS da **MyOldDnsName** a **MyNewDnsName**, e il nome NetBIOS da **MyOldNetBIOSName** a **MyNewNetBIOSName**.

In questo esempio, utilizziamo il **gpfixup** comando per connettersi al controller di dominio denominato **MyDcDnsName** e riparare oggetti Criteri di gruppo e criteri di gruppo e collegamenti aggiornando il vecchio nome di dominio incorporati in oggetti Criteri di gruppo e collegamenti. Output di errore e di stato viene salvato in un file denominato **gpfixup.log**.

```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /oldnb:MyOldNetBIOSName /newnb:MyNewNetBIOSName /dc:MyDcDnsName 2>&1 >gpfixup.log
```

In questo esempio è identico a quello precedente, ad eccezione del fatto che assume che il nome NetBIOS del dominio non è stato modificato durante il dominio di operazione di ridenominazione.

```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /dc:MyDcDnsName 2>&1 >gpfixup.log
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [L'amministrazione della ridenominazione del dominio Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc794869(v=ws.10))
