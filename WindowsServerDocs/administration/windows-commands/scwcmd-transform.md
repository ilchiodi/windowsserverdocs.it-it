---
title: Trasformazione scwcmd
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 640dd892-0bb9-416d-8318-60a26605bcf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1fed9ff6369e6c966d9d1f5295db7db6648a1ab1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835124"
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

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/p:\<policyFile. XML >|Specifica il percorso e il nome del file di criteri XML che deve essere applicato. Questo parametro deve essere specificato.|
|/g:\<GPODisplayName >|Specifica il nome visualizzato dell'oggetto Criteri di gruppo. Questo parametro deve essere specificato.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

Scwcmd.exe è disponibile solo nei computer che eseguono Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi

Per creare un oggetto Criteri di gruppo denominato FileServerSecurity da un file denominato FileServerPolicy.xml, digitare:
```
scwcmd transform /p:FileServerPolicy.xml /g:FileServerSecurity
```

## <a name="additional-references"></a>Altre informazioni di riferimento

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)