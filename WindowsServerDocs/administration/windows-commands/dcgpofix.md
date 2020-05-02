---
title: dcgpofix
description: Argomento di riferimento per Dcgpofix, che ricrea gli oggetti Criteri di gruppo predefiniti (GPO) per un dominio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 81d5fa65-2aea-49d3-b353-357441846c00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7b30190f06e5e38031c8d205d8ccee9c573a8d84
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716783"
---
# <a name="dcgpofix"></a>dcgpofix

Ricrea il valore predefinito dei criteri di gruppo (GPO) per un dominio.

## <a name="syntax"></a>Sintassi

```
DCGPOFix [/ignoreschema] [/target: {Domain | DC | Both}] [/?]
```

#### <a name="parameters"></a>Parametri

|    Parametro    |                                                                                                 Descrizione                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  /IgnoreSchema  | Ignora la versione di mc schema Active Directory®</br>Quando si esegue questo comando. In caso contrario, il comando funziona solo nella stessa versione di schema come la versione di Windows in cui è stato fornito il comando. |
| /target {dominio |                                                                                                     DC                                                                                                      |
|       /?        |                                                                                    Visualizza la Guida al prompt dei comandi.                                                                                     |

## <a name="remarks"></a>Osservazioni

-   Il comando **Dcgpofix** è disponibile in windows Server 2008 R2 e windows Server 2008, tranne che nelle installazioni dei componenti di base del server.
-   Sebbene la Console Gestione Criteri di gruppo (GPMC) venga distribuita con Windows Server 2008 R2 e Windows Server 2008, è necessario installare Criteri di gruppo gestione come funzionalità tramite Server Manager.

## <a name="examples"></a>Esempi

Ripristinare il criterio dominio predefinito per lo stato originale. Si perderanno tutte le modifiche apportate a questo oggetto Criteri di gruppo. Come procedura consigliata, è necessario configurare l'oggetto criterio dominio predefinito solo per gestire le impostazioni di criteri di Account, criteri Password, criterio di blocco Account e criterio Kerberos. In questo esempio, si ignora la versione dello schema di Active Directory in modo che il **dcgpofix** comando non è limitato allo stesso schema della versione di Windows in cui è stato fornito il comando.
```
dcgpofix /ignoreschema /target:Domain
```
Ripristinare il controller criterio dominio predefinito per lo stato originale. Si perderanno tutte le modifiche apportate a questo oggetto Criteri di gruppo. Come procedura consigliata, è necessario configurare il controller criterio dominio predefinito solo per impostare i diritti utente e criteri di controllo. In questo esempio, si ignora la versione dello schema di Active Directory in modo che il **dcgpofix** comando non è limitato allo stesso schema della versione di Windows in cui è stato fornito il comando.
```
dcgpofix /ignoreschema /target:DC
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   [TechCenter di criteri di gruppo](https://go.microsoft.com/fwlink/?LinkID=145531)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)