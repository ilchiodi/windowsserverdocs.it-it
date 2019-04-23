---
title: Scwcmd register
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe4d126a-9f27-4076-b7b1-fbefa45f378a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cc8b4a06af519b0da01dfcab8de0139b12cc68f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834972"
---
# <a name="scwcmd-register"></a>Scwcmd: registrare

> Si applica a: Windows Server 2012 R2, Windows Server 2012

Estendere o personalizzare il Database di configurazione di protezione configurazione guidata impostazioni di Sicurezza tramite la registrazione di un file di Database di configurazione di sicurezza che contiene i ruoli, attività, servizio o definizioni di porte.

## <a name="syntax"></a>Sintassi

```
scwcmd register /kbname:<MyApp> [/kbfile:<kb.xml>] [/kb:<path>] [/d]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/kbname.:\<MyApp >|Specifica il nome con cui verrà registrato l'estensione del Database di configurazione di sicurezza. Questo parametro deve essere specificato.|
|/kbfile:\<Kb.xml>|Specifica il percorso e il nome del file di Database di configurazione di sicurezza che verrà utilizzato per estendere o personalizzare il Database di configurazione di sicurezza base. Per verificare che il file di Database di configurazione di protezione sia conforme con lo schema di Sicurezza, utilizzare il file di definizione dello schema %windir%\security\KBRegistrationInfo.xsd. È necessario specificare questa opzione a meno che il **/d** parametro specificato.|
|/kb:\<Path>|Specifica il percorso della directory che contiene i file di Database di configurazione Guidata sicurezza da aggiornare. Se questa opzione non è specificata, viene utilizzato %windir%\security\msscw\kbs.|
|/d|Annulla la registrazione di un'estensione di Database di configurazione di sicurezza dal Database di configurazione di sicurezza. L'estensione per annullare la registrazione è specificato dal parametro /kbname. (Il **/kbfile** parametro non deve essere specificato.) Viene specificato il Database di configurazione di protezione per annullare la registrazione dell'estensione per il **/kb** parametro.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

Scwcmd.exe è disponibile solo nei computer che eseguono Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="BKMK_Examples"></a>Esempi

Per registrare il file di Database di configurazione di sicurezza denominato scwkbformyapp. XML sotto il nome MyApp nel percorso \\ \\kbserver\kb, tipo:
```
scwcmd register /kbfile:d:\SCWKBForMyApp.xml /kbname:MyApp /kb:\\kbserver\kb
```
Per annullare la registrazione che si trova in Security Configuration Database MyApp \\ \\kbserver\kb, tipo:
```
scwcmd register /d /kbname:MyApp /kb:\\kbserver\kb
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)