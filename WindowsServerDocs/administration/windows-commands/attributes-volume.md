---
title: volume attributi
description: Windows Commands argomento for **Attributes volume**, che Visualizza, imposta o cancella gli attributi di un volume.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e40e8284-3d57-4de8-a46c-e4ade34a0d53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 00991cdba57f0728cfa348dea2b0916ad758b34a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851234"
---
# <a name="attributes-volume"></a>volume attributi

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza, imposta o cancella gli attributi di un volume.

## <a name="syntax"></a>Sintassi  

```
attributes volume [{set | clear}] [{hidden | readonly | nodefaultdriveletter | shadowcopy}] [noerr]  
```  
  
### <a name="parameters"></a>Parametri  
  
| Parametro | Descrizione |  
| ------- | -------- |  
| set | Imposta l'attributo specificato del volume con lo stato attivo. |  
| deselezionato | Cancella l'attributo specificato del volume con lo stato attivo. |  
| readonly | Specifica che il volume è di sola lettura. |  
| nascosto | Specifica che il volume è nascosto. |  
| NODEFAULTDRIVELETTER | Specifica che il volume non riceve una lettera di unità per impostazione predefinita. |  
| ShadowCopy | Specifica che il volume è un volume di copia shadow. |  
| NOERR | solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |  
  
## <a name="remarks"></a>Note  
  
- Nei dischi MBR (master boot record) Basic i parametri **Hidden**, **ReadOnly**e **nodefaultdriveletter** si applicano a tutti i volumi sul disco.  
  
- Nei dischi di base della tabella di partizione GUID (GPT) e nei dischi MBR e GPT dinamici i parametri **Hidden**, **ReadOnly**e **nodefaultdriveletter** si applicano solo al volume selezionato.  
  
- È necessario selezionare un volume affinché il comando **volume attributi** abbia esito positivo. Utilizzare il **Selezionare volume** comando per selezionare un volume e spostare lo stato attivo a esso.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Esempi

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
  
## <a name="additional-references"></a>Altre informazioni di riferimento  

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)