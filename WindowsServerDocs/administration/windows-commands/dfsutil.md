---
title: Dfsutil
description: Argomento di riferimento per Dfsutil, che gestisce gli spazi dei nomi DFS, i server e i client. i comandi dfsutil utilizzano la terminologia file system distribuito originale, con la terminologia degli spazi dei nomi DFS aggiornata fornita come spiegazione per la maggior parte dei comandi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 999eef79227d4531ba724c9cac40127297ea38a0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719518"
---
# <a name="dfsutil"></a>Dfsutil

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il comando dfsutil gestisce spazi dei nomi DFS, server e client.

>[!NOTE]
>Il **modulo di PowerShell per gli spazi dei nomi DFS** fornisce sostituzioni per alcuni parametri di Dfsutil, mentre altri ancora richiedono l'uso di Dfsutil. Per ulteriori informazioni sugli equivalenti di PowerShell aggiornati, vedere [DFSN](https://docs.microsoft.com/powershell/module/dfsn/?view=win10-ps).

## <a name="parameters-available-in-powershell"></a>Parametri disponibili in PowerShell

Da PowerShell è possibile usare i parametri seguenti:

| Parametro | Descrizione |
| --------- | ----------- |
| root | Visualizza, crea, rimuove, importa ed Esporta le radici dello spazio dei nomi. |
| link | Consente di visualizzare, creare, rimuovere o spostare cartelle (collegamenti). |
| target | Visualizza, crea, Rimuovi la cartella di destinazione o il server dello spazio dei nomi. |
| proprietà | Visualizza o modifica di un server di destinazione o spazio dei nomi di cartella. |
| server | Visualizza o modifica di configurazione dello spazio dei nomi. |
| dominio | Visualizza tutti gli spazi dei nomi basati su dominio in un dominio. |

## <a name="parameters-only-available-in-dfsutil"></a>Parametri disponibili solo in Dfsutil

È possibile usare i parametri seguenti solo da Dfsutil.

| Parametro | Descrizione |
| --------- | ----------- |
| client | Visualizza o modifica le chiavi del Registro di sistema o informazioni client. |
| diag | Eseguire la diagnostica o visualizzare dfsdirs/dfspath. |
| cache | Visualizza o scarica la cache del client. |

Per ulteriori informazioni su ognuno di questi comandi, aprire un prompt dei comandi in un server in cui sono installati gli strumenti di gestione degli spazi dei `dfsutil client /?`nomi `dfsutil diag /?`DFS, `dfsutil cache /?`quindi digitare, o.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)