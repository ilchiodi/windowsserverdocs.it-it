---
title: volume attributi
description: Argomento di riferimento per il comando volume attributi, che Visualizza, imposta o cancella gli attributi di un volume.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e40e8284-3d57-4de8-a46c-e4ade34a0d53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbe1d66584216875daa82a7e250f3d2f525c2280
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719188"
---
# <a name="attributes-volume"></a>volume attributi

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza, imposta o cancella gli attributi di un volume.

## <a name="syntax"></a>Sintassi  

```
attributes volume [{set | clear}] [{hidden | readonly | nodefaultdriveletter | shadowcopy}] [noerr]  
```  
  
### <a name="parameters"></a>Parametri  
  
| Parametro | Descrizione |  
| ------- | -------- |  
| set | Imposta l'attributo specificato del volume con lo stato attivo. |  
| deselezionare | Cancella l'attributo specificato del volume con lo stato attivo. |  
| readonly | Specifica che il volume è di sola lettura. |  
| hidden | Specifica che il volume è nascosto. |  
| NODEFAULTDRIVELETTER | Specifica che il volume non riceve una lettera di unità per impostazione predefinita. |  
| ShadowCopy | Specifica che il volume è un volume di copia shadow. |  
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |  
  
### <a name="remarks"></a>Osservazioni  
  
- Nei dischi MBR (master boot record) Basic i parametri **Hidden**, **ReadOnly**e **nodefaultdriveletter** si applicano a tutti i volumi sul disco.  
  
- Nei dischi di base della tabella di partizione GUID (GPT) e nei dischi MBR e GPT dinamici i parametri **Hidden**, **ReadOnly**e **nodefaultdriveletter** si applicano solo al volume selezionato.  
  
- È necessario selezionare un volume affinché il comando **volume attributi** abbia esito positivo. Utilizzare il **Selezionare volume** comando per selezionare un volume e spostare lo stato attivo a esso.  
  
## <a name="examples"></a>Esempi

Per visualizzare gli attributi correnti sul volume selezionato, digitare:  
  
```
attributes volume  
```  
  
Per impostare il volume selezionato come nascosto e in sola lettura, digitare:  
  
```
attributes volume set hidden readonly  
```  
  
Per rimuovere gli attributi nascosti e di sola lettura nel volume selezionato, digitare:  
  
```
attributes volume clear hidden readonly  
```  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando select volume](select-volume.md)
