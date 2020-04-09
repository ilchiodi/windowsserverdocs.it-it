---
title: Registro scwcmd
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe4d126a-9f27-4076-b7b1-fbefa45f378a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5390b7d81efe8d807dd0b7d7a8c136a1d7092af3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835154"
---
# <a name="scwcmd-register"></a>Scwcmd: registrare

> Si applica a: Windows Server 2012 R2, Windows Server 2012

Estendere o personalizzare il Database di configurazione di protezione configurazione guidata impostazioni di Sicurezza tramite la registrazione di un file di Database di configurazione di sicurezza che contiene i ruoli, attività, servizio o definizioni di porte.

## <a name="syntax"></a>Sintassi

```
scwcmd register /kbname:<MyApp> [/kbfile:<kb.xml>] [/kb:<path>] [/d]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/kbname:\<MyApp >|Specifica il nome con cui verrà registrato l'estensione del Database di configurazione di sicurezza. Questo parametro deve essere specificato.|
|/kbfile:\<KB. XML >|Specifica il percorso e il nome del file di Database di configurazione di sicurezza che verrà utilizzato per estendere o personalizzare il Database di configurazione di sicurezza base. Per verificare che il file di Database di configurazione di protezione sia conforme con lo schema di Sicurezza, utilizzare il file di definizione dello schema %windir%\security\KBRegistrationInfo.xsd. È necessario specificare questa opzione a meno che il **/d** parametro specificato.|
|/KB: > percorso\<|Specifica il percorso della directory che contiene i file di Database di configurazione Guidata sicurezza da aggiornare. Se questa opzione non è specificata, viene utilizzato %windir%\security\msscw\kbs.|
|/d|Annulla la registrazione di un'estensione di Database di configurazione di sicurezza dal Database di configurazione di sicurezza. L'estensione per annullare la registrazione è specificato dal parametro /kbname. (Il parametro **/kbfile** non deve essere specificato). Il database di configurazione della sicurezza per cui annullare la registrazione dell'estensione viene specificato dal parametro **/KB** .|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

Scwcmd.exe è disponibile solo nei computer che eseguono Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi

Per registrare il file di database di configurazione di sicurezza denominato SCWKBForMyApp. XML con il nome MyApp nel percorso \\\\kbserver\kb, digitare:
```
scwcmd register /kbfile:d:\SCWKBForMyApp.xml /kbname:MyApp /kb:\\kbserver\kb
```
Per annullare la registrazione del database di configurazione di sicurezza MyApp disponibile in \\\\kbserver\kb, digitare:
```
scwcmd register /d /kbname:MyApp /kb:\\kbserver\kb
```

## <a name="additional-references"></a>Altre informazioni di riferimento

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)