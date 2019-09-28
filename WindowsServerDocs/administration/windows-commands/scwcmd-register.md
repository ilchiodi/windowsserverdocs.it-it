---
title: Registro scwcmd
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2e892f7c08461e88d12a072dfb171f9523558ef7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371220"
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
|/kbname: > \<MyApp|Specifica il nome con cui verrà registrato l'estensione del Database di configurazione di sicurezza. Questo parametro deve essere specificato.|
|/kbfile: @no__t -0Kb. XML >|Specifica il percorso e il nome del file di Database di configurazione di sicurezza che verrà utilizzato per estendere o personalizzare il Database di configurazione di sicurezza base. Per verificare che il file di Database di configurazione di protezione sia conforme con lo schema di Sicurezza, utilizzare il file di definizione dello schema %windir%\security\KBRegistrationInfo.xsd. È necessario specificare questa opzione a meno che il **/d** parametro specificato.|
|/KB: > \<Path|Specifica il percorso della directory che contiene i file di Database di configurazione Guidata sicurezza da aggiornare. Se questa opzione non è specificata, viene utilizzato %windir%\security\msscw\kbs.|
|/d|Annulla la registrazione di un'estensione di Database di configurazione di sicurezza dal Database di configurazione di sicurezza. L'estensione per annullare la registrazione è specificato dal parametro /kbname. (Il **/kbfile** parametro non deve essere specificato.) Viene specificato il Database di configurazione di protezione per annullare la registrazione dell'estensione per il **/kb** parametro.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

Scwcmd.exe è disponibile solo nei computer che eseguono Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="BKMK_Examples"></a>Esempi

Per registrare il file di database di configurazione di sicurezza denominato SCWKBForMyApp. XML con il nome MyApp nel percorso \\ @ no__t-1kbserver\kb, digitare:
```
scwcmd register /kbfile:d:\SCWKBForMyApp.xml /kbname:MyApp /kb:\\kbserver\kb
```
Per annullare la registrazione del database di configurazione di sicurezza MyApp disponibile in \\ @ no__t-1kbserver\kb, digitare:
```
scwcmd register /d /kbname:MyApp /kb:\\kbserver\kb
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)