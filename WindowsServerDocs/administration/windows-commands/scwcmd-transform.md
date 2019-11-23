---
title: Trasformazione scwcmd
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 640dd892-0bb9-416d-8318-60a26605bcf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 36ee3a99828c7fdd9d4fc0ca14cbc0e203b01ea0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384315"
---
# <a name="scwcmd-transform"></a>Scwcmd: trasformare

> Si applica a: Windows Server 2012 R2, Windows Server 2012

Trasforma un file di criteri di sicurezza generato utilizzando la configurazione guidata impostazioni di Sicurezza in un nuovo oggetto (criteri di gruppo) in servizi di dominio Active Directory. L'operazione di trasformazione non modifica le impostazioni nel server in cui viene eseguito. Una volta completata l'operazione di trasformazione, un amministratore è necessario collegare l'oggetto Criteri di gruppo alle unità organizzative per distribuire i criteri di server desiderate.

Per completare l'operazione di trasformazione sono necessarie le credenziali di amministratore di dominio.

> [!IMPORTANT]
> Impostazioni di protezione di Internet Information Services (IIS) non possono essere distribuite utilizzando criteri di gruppo.</br>> Criteri firewall che elencano le applicazioni approvate non devono essere distribuiti ai server a meno che il servizio Windows Firewall non venga avviato automaticamente all'ultimo avvio del server.

Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
scwcmd transform /p:<Policyfile.xml> /g:<GPODisplayName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/p:\<policyFile. XML >|Specifica il percorso e il nome del file di criteri XML che deve essere applicato. Questo parametro deve essere specificato.|
|/g:\<GPODisplayName >|Specifica il nome visualizzato dell'oggetto Criteri di gruppo. Questo parametro deve essere specificato.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

Scwcmd.exe è disponibile solo nei computer che eseguono Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="BKMK_Examples"></a>Esempi

Per creare un oggetto Criteri di gruppo denominato FileServerSecurity da un file denominato FileServerPolicy.xml, digitare:
```
scwcmd transform /p:FileServerPolicy.xml /g:FileServerSecurity
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)