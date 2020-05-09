---
title: Dfsutil
description: Argomento di riferimento per il comando Dfsutil, che gestisce gli spazi dei nomi DFS, i server e i client.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6905d90ee42958e47dfed4869520000a4fd3ddf
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992617"
---
# <a name="dfsutil"></a>Dfsutil

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il comando Dfsutil gestisce gli spazi dei nomi DFS, i server e i client.

## <a name="functionality-available-in-powershell"></a>Funzionalità disponibili in PowerShell

Il modulo [DFSN](https://docs.microsoft.com/powershell/module/dfsn/?view=win10-ps) di PowerShell fornisce funzionalità equivalenti ai seguenti parametri Dfsutil.

| Parametro | Descrizione |
| --------- | ----------- |
| root | Visualizza, crea, rimuove, importa ed Esporta le radici dello spazio dei nomi. |
| link | Consente di visualizzare, creare, rimuovere o spostare cartelle (collegamenti). |
| target | Visualizza, crea, Rimuovi la cartella di destinazione o il server dello spazio dei nomi. |
| proprietà | Visualizza o modifica di un server di destinazione o spazio dei nomi di cartella. |
| server | Visualizza o modifica di configurazione dello spazio dei nomi. |
| dominio | Visualizza tutti gli spazi dei nomi basati su dominio in un dominio. |

## <a name="functionality-available-only-in-dfsutil"></a>Funzionalità disponibili solo in Dfsutil

Le funzionalità seguenti sono disponibili solo come parametri Dfsutil:

| Parametro | Descrizione |
| --------- | ----------- |
| client | Visualizza o modifica le chiavi del Registro di sistema o informazioni client. |
| diag | Eseguire la diagnostica o visualizzare dfsdirs/dfspath. |
| cache | Visualizza o scarica la cache del client. |

Per ulteriori informazioni su ognuno di questi comandi, aprire un prompt dei comandi in un server in cui sono installati gli strumenti di gestione degli spazi dei `dfsutil client /?`nomi `dfsutil diag /?`DFS, `dfsutil cache /?`quindi digitare, o.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)