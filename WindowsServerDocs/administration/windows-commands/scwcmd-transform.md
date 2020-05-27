---
title: Trasformazione scwcmd
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 640dd892-0bb9-416d-8318-60a26605bcf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 86c88f0207a53da813f5d4eaed399375bfc5bc02
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820951"
---
# <a name="scwcmd-transform"></a>Scwcmd: trasformare

> Si applica a: Windows Server 2012 R2, Windows Server 2012

Trasforma un file di criteri di sicurezza generato utilizzando la configurazione guidata impostazioni di Sicurezza in un nuovo oggetto (criteri di gruppo) in servizi di dominio Active Directory. L'operazione di trasformazione non modifica le impostazioni nel server in cui viene eseguito. Una volta completata l'operazione di trasformazione, un amministratore è necessario collegare l'oggetto Criteri di gruppo alle unità organizzative per distribuire i criteri di server desiderate.

Per completare l'operazione di trasformazione sono necessarie le credenziali di amministratore di dominio.

> [!IMPORTANT]
> Impostazioni di protezione di Internet Information Services (IIS) non possono essere distribuite utilizzando criteri di gruppo.</br>> criteri firewall che elencano le applicazioni approvate non devono essere distribuiti ai server a meno che il servizio Windows Firewall non venga avviato automaticamente all'ultimo avvio del server.



## <a name="syntax"></a>Sintassi

```
scwcmd transform /p:<Policyfile.xml> /g:<GPODisplayName>
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/p: \< policyFile. xml>|Specifica il percorso e il nome del file di criteri XML che deve essere applicato. Questo parametro deve essere specificato.|
|/g: \< GPODisplayName>|Specifica il nome visualizzato dell'oggetto Criteri di gruppo. Questo parametro deve essere specificato.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

Scwcmd.exe è disponibile solo nei computer che eseguono Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="examples"></a>Esempi

Per creare un oggetto Criteri di gruppo denominato FileServerSecurity da un file denominato FileServerPolicy.xml, digitare:
```
scwcmd transform /p:FileServerPolicy.xml /g:FileServerSecurity
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)