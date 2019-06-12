---
title: bootcfg rmsw
description: Argomento i comandi di Windows per **bootcfg rmsw** - rimuove operativo opzioni di caricamento del sistema per una voce del sistema operativo specificato.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fd7e4248-880e-4e2b-929e-87f8d44b9a63
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f3d873cffbdb386b5f4f564801a4f4b815c6987a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434685"
---
# <a name="bootcfg-rmsw"></a>bootcfg rmsw

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rimuove opzioni di caricamento del sistema operativo per una voce del sistema operativo specificato.

## <a name="syntax"></a>Sintassi
```
bootcfg /rmsw [/s <computer> [/u <Domain>\<User> [/p <Password>]]] [/mm] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Parametri

|      Parametro       |                                                                                                      Descrizione                                                                                                       |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                   Specifica il nome o indirizzo IP di un computer remoto (non utilizzare le barre rovesciate). Il valore predefinito è il computer locale.                                                   |
| /u <Domain>\\<User>  |          Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User> oppure <Domain> \\ <User>. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso.          |
|    /p <Password>     |                                                                 Specifica la password dell'account utente specificato nella **/u** parametro.                                                                  |
|         /mm          |           Rimuove l'opzione /maxmem e il relativo valore di memoria massimo associato specificato <OSEntryLineNum>. L'opzione /maxmem specifica la quantità massima di RAM che è possibile utilizzare il sistema operativo.            |
|         /BV          |                     Rimuove l'opzione /basevideo specificato <OSEntryLineNum>. L'opzione /basevideo indica al sistema operativo di utilizzare la modalità VGA standard per il driver video installato.                     |
|         /SO          |                         Rimuove l'opzione /sos specificato <OSEntryLineNum>. L'opzione /sos indica al sistema operativo di visualizzare i nomi dei driver di dispositivo durante il caricamento.                          |
|         /NG          |                         Rimuove l'opzione /noguiboot specificato <OSEntryLineNum>. L'opzione /noguiboot disabilita l'indicatore di stato che precede il CTRL + ALT + CANC prompt di accesso.                          |
| /id <OSEntryLineNum> | Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini da cui vengono rimosse le opzioni di caricamento del sistema operativo. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1. |
|          /?          |                                                                                          Visualizza la guida al prompt dei comandi.                                                                                          |

## <a name="BKMK_examples"></a>Esempi
Gli esempi seguenti illustrano come utilizzare il **bootcfg /rmsw**comando:
```
bootcfg /rmsw /mm 64 /id 2 
bootcfg /rmsw /so /id 3 
bootcfg /rmsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /rmsw /ng /id 2 
bootcfg /rmsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2       
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
