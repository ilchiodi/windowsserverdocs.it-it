---
title: bootcfg addsw
description: Windows Commands Topic for bootcfg Addsw, che aggiunge le opzioni di caricamento del sistema operativo per una voce del sistema operativo specificata.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d8389293-ecd9-42f0-b84b-b9ead4cf52e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a9ae5175dfc3b068276f6ab95d6823699c96b2b5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848714"
---
# <a name="bootcfg-addsw"></a>bootcfg addsw

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di aggiungere opzioni di caricamento del sistema operativo per una voce del sistema operativo specificato.

## <a name="syntax"></a>Sintassi
```
bootcfg /addsw [/s <computer> [/u <Domain>\<User> /p <Password>]] [/mm <MaximumRAM>] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
### <a name="parameters"></a>Parametri

|         Termine         |                                                                                                            Definizione                                                                                                            |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                        Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale.                                                        |
| /u <Domain>\\<User>  |               Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User> o <Domain>\\<User>. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso.               |
|    /p <Password>     |                                                                      Specifica la password dell'account utente specificato nella **/u** parametro.                                                                       |
|   <MaximumRAM>/mm   |                                          Specifica la quantità massima di RAM, in megabyte, che è possibile utilizzare il sistema operativo. Il valore deve essere uguale o maggiore di 32 MB.                                          |
|         /BV          |                                    aggiunge l'opzione **/basevideo** al <OSEntryLineNum>specificato, indicando al sistema operativo di utilizzare la modalità VGA standard per il driver video installato.                                     |
|         /SO          |                                      aggiunge l'opzione **/SOS** al *NumRigaVoceSO*specificato, indirizzando il sistema operativo in modo da visualizzare i nomi dei driver di dispositivo durante il caricamento.                                      |
|         /NG          |                                         aggiunge l'opzione **/noguiboot** al <OSEntryLineNum>specificato, disabilitando la barra di stato visualizzata prima del prompt di accesso CTRL + ALT + CANC.                                          |
| <OSEntryLineNum>/ID | Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini alla quale aggiungere opzioni di caricamento del sistema operativo. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1. |
|          /?          |                                                                                               Visualizza la guida al prompt dei comandi.                                                                                               |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Gli esempi seguenti illustrano come utilizzare il **bootcfg /addsw** comando:
```
bootcfg /addsw /mm 64 /id 2 
bootcfg /addsw /so /id 3 
bootcfg /addsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /addsw /ng /id 2 
bootcfg /addsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
