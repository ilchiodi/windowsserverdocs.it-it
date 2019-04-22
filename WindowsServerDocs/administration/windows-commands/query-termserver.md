---
title: eseguire una query termserver
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3b89d3b4-236f-4376-90b6-939a0ec4b288
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5baab78f6a7222bad121773e686bacb01dc8872a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817232"
---
# <a name="query-termserver"></a>eseguire una query termserver

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza un elenco di tutti i server Host sessione Desktop remoto (Host sessione rd) nella rete.
Per esempi di come usare questo comando, vedere [esempi](#BKMK_examples).
> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per scoprire quali sono le novità nella versione più recente, vedere [novità in Servizi Desktop remoto in Windows Server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.
## <a name="syntax"></a>Sintassi
```
query termserver [<ServerName>] [/domain:<Domain>] [/address] [/continue]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|<ServerName>|Specifica il nome che identifica il server Host sessione Desktop remoto.|
|/Domain:<Domain>|Specifica il dominio per eseguire query per i server terminal. Non devi specificare un dominio se si esegue una query il dominio in cui si sta lavorando.|
|/address|Consente di visualizzare gli indirizzi di rete e di nodo per ogni server.|
|/continue|Impedisce la sospensione dopo che viene visualizzato ogni schermata di informazioni.|
|/?|Visualizza la guida al prompt dei comandi.|
## <a name="remarks"></a>Note
-   **eseguire una query termserver** esegue la ricerca della rete tutti collegati i server Host sessione Desktop remoto e restituisce le informazioni seguenti:
    -   Il nome del server
    -   La rete (e l'indirizzo del nodo se viene utilizzata l'opzione /address)
## <a name="BKMK_examples"></a>Esempi
-   Per visualizzare informazioni su tutti i server Host sessione Desktop remoto sulla rete, digitare:
    ```
    query termserver
    ```
-   Per visualizzare informazioni sul server Host sessione Desktop remoto denominato Server3, digitare:
    ```
    query termserver Server3
    ```
-   Per visualizzare informazioni su tutti i server Host sessione Desktop remoto nel dominio CONTOSO, digitare:
    ```
    query termserver /domain:CONTOSO
    ```
-   Per visualizzare l'indirizzo di rete e di nodo per il server Host sessione Desktop remoto denominato Server3, digitare:
    ```
    query termserver Server3 /address
    ```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[query](query.md)
[Remote Desktop Services &#40;servizi Terminal&#41; riferimenti ai comandi](remote-desktop-services-terminal-services-command-reference.md)
