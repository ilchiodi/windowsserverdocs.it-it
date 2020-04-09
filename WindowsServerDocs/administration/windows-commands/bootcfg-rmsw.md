---
title: bootcfg rmsw
description: Windows Commands Topic for bootcfg rmsw, che rimuove le opzioni di caricamento del sistema operativo per una voce del sistema operativo specificata.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fd7e4248-880e-4e2b-929e-87f8d44b9a63
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 956732f396e0fa353a8acd55953e46605a5c4200
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848444"
---
# <a name="bootcfg-rmsw"></a>bootcfg rmsw

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rimuove opzioni di caricamento del sistema operativo per una voce del sistema operativo specificato.

## <a name="syntax"></a>Sintassi
```
bootcfg /rmsw [/s <computer> [/u <Domain>\<User> [/p <Password>]]] [/mm] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
### <a name="parameters"></a>Parametri

|      Parametro       |                                                                                                      Descrizione                                                                                                       |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                   Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale.                                                   |
| /u <Domain>\\<User>  |          Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User> o <Domain>\\<User>. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso.          |
|    /p <Password>     |                                                                 Specifica la password dell'account utente specificato nella **/u** parametro.                                                                  |
|         /mm          |           Rimuove l'opzione/maxmem e il valore di memoria massimo associato dal <OSEntryLineNum>specificato. L'opzione /maxmem specifica la quantità massima di RAM che è possibile utilizzare il sistema operativo.            |
|         /BV          |                     Rimuove l'opzione/basevideo dall'<OSEntryLineNum>specificato. L'opzione /basevideo indica al sistema operativo di utilizzare la modalità VGA standard per il driver video installato.                     |
|         /SO          |                         Rimuove l'opzione/SOS dall'<OSEntryLineNum>specificato. L'opzione /sos indica al sistema operativo di visualizzare i nomi dei driver di dispositivo durante il caricamento.                          |
|         /NG          |                         Rimuove l'opzione/noguiboot dall'<OSEntryLineNum>specificato. L'opzione/noguiboot Disabilita l'indicatore di stato visualizzato prima del prompt di accesso CTRL + ALT + CANC.                          |
| <OSEntryLineNum>/ID | Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini da cui vengono rimosse le opzioni di caricamento del sistema operativo. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1. |
|          /?          |                                                                                          Visualizza la guida al prompt dei comandi.                                                                                          |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Gli esempi seguenti illustrano come utilizzare il **bootcfg /rmsw**comando:
```
bootcfg /rmsw /mm 64 /id 2 
bootcfg /rmsw /so /id 3 
bootcfg /rmsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /rmsw /ng /id 2 
bootcfg /rmsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2       
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
