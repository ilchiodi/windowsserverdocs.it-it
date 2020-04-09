---
title: Dfsutil
description: Windows Commands argomento per Dfsutil, che gestisce spazi dei nomi DFS, server e client. i comandi dfsutil utilizzano la terminologia file system distribuito originale, con la terminologia degli spazi dei nomi DFS aggiornata fornita come spiegazione per la maggior parte dei comandi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 30415bc85fd8a4a4804946a3d4a168d6a7d1433a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845584"
---
# <a name="dfsutil"></a>Dfsutil

>Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il comando dfsutil gestisce spazi dei nomi DFS, server e client. Nella maggior parte dei casi è possibile usare i cmdlet di PowerShell per gli spazi dei nomi DFS più recenti, anche se sono disponibili alcuni comandi che richiedono ancora Dfsutil.

## <a name="parameters-available-in-powershell"></a>Parametri disponibili in PowerShell

Da PowerShell è possibile usare i parametri seguenti:

| Parametro | Descrizione |
| --------- | ----------- |
| radice | Visualizza, crea, rimuove, importa ed Esporta le radici dello spazio dei nomi. |
| collegamento | Consente di visualizzare, creare, rimuovere o spostare cartelle (collegamenti). |
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

Per altre informazioni su ognuno di questi comandi, aprire un prompt dei comandi in un server con gli strumenti di gestione degli spazi dei nomi DFS installati, quindi digitare `dfsutil client /?`, `dfsutil diag /?`o `dfsutil cache /?`.

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)