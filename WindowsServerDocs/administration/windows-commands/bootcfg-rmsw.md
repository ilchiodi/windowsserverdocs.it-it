---
title: bootcfg rmsw
description: Argomento di riferimento per il comando bootcfg rmsw, che consente di rimuovere le opzioni di caricamento del sistema operativo per una voce del sistema operativo specificata.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fd7e4248-880e-4e2b-929e-87f8d44b9a63
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 41c9819fb3d669b24a5918077bef960869625a15
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82708909"
---
# <a name="bootcfg-rmsw"></a>bootcfg rmsw

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rimuove opzioni di caricamento del sistema operativo per una voce del sistema operativo specificato.

## <a name="syntax"></a>Sintassi

```
bootcfg /rmsw [/s <computer> [/u <domain>\<user> /p <password>]] [/mm] [/bv] [/so] [/ng] /id <osentrylinenum>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `/s <computer>` | Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale. |
| `/u <domain>\<user>`  | Esegue il comando con le autorizzazioni dell'account dell'utente specificato da `<user>` o `<domain>\<user>`. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
| `/p <password>` | Specifica la password dell'account utente specificato nella **/u** parametro. |
| /mm | Rimuove l'opzione /maxmem e il valore di memoria massimo associato dal percorso specificato `<osentrylinenum>`. L'opzione /maxmem specifica la quantità massima di RAM che è possibile utilizzare il sistema operativo. |
| /BV | Rimuove l'opzione /basevideo specificato `<osentrylinenum>`. L'opzione /basevideo indica al sistema operativo di utilizzare la modalità VGA standard per il driver video installato. |
| /SO | Rimuove l'opzione /sos specificato `<osentrylinenum>`. L'opzione /sos indica al sistema operativo di visualizzare i nomi dei driver di dispositivo durante il caricamento. |
| /NG | Rimuove l'opzione /noguiboot specificato `<osentrylinenum>`. L'opzione /noguiboot disabilita l'indicatore di stato che precede il prompt di accesso CTRL + ALT + CANC. |
| `/id <osentrylinenum>` | Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini alla quale aggiungere opzioni di caricamento del sistema operativo. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

Per usare il comando **bootcfg/rmsw** :

```
bootcfg /rmsw /mm 64 /id 2
bootcfg /rmsw /so /id 3
bootcfg /rmsw /so /ng /s srvmain /u hiropln /id 2
bootcfg /rmsw /ng /id 2
bootcfg /rmsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando bootcfg](bootcfg.md)
