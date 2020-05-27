---
title: Scwcmd
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 188ae881-c7d4-4a7a-b967-8fdc79f5f345
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9a8c002aa735b188fdd9ad75b0db7dbe06053516
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821161"
---
# <a name="scwcmd"></a>Scwcmd

> Si applica a: Windows Server 2012 R2, Windows Server 2012

Lo strumento da riga di comando Scwcmd. exe incluso con la configurazione guidata impostazioni di sicurezza può essere utilizzato per eseguire le attività seguenti:
-   Configurare uno o più server con un criterio di Sicurezza generato.
-   Analizzare uno o più server con un criterio di Sicurezza generato.
-   Consente di visualizzare i risultati dell'analisi in formato HTML.
-   Rollback dei criteri di Sicurezza.
-   Trasformare un criterio di Sicurezza generato in file nativi supportati da criteri di gruppo.
-   Registrare un'estensione di sicurezza Database di configurazione guidata impostazioni di Sicurezza.

Quando si utilizza **scwcmd** per configurare, analizzare o eseguire il rollback un criterio in un server remoto, SCW deve essere installato nel server remoto.

## <a name="syntax"></a>Sintassi

```
scwcmd <command> [<subcommand>]
```

### <a name="parameters"></a>Parametri

|Sottocomando|Descrizione|
|----------|-----------|
|/analyze|Determina se un computer è in conformità con i criteri.</br>Vedere [Scwcmd: analizzare](scwcmd-analyze.md) per la sintassi e le opzioni.|
|configurano|Si applica un criterio di sicurezza generato Sicurezza a un computer.</br>Vedere [Scwcmd: configurare](scwcmd-configure.md) per la sintassi e le opzioni.|
|/Register|Estendere o personalizzare il Database di configurazione Guidata sicurezza tramite la registrazione di un file di Database di configurazione di sicurezza che contiene i ruoli, attività, servizio o definizioni di porte.</br>Vedere [Scwcmd: registrare](scwcmd-register.md) per la sintassi e le opzioni.|
|/rollback|Applica i criteri di ripristino più recente disponibili e quindi Elimina il criterio di ripristino.</br>Vedere [Scwcmd: rollback](scwcmd-rollback.md) per la sintassi e le opzioni.|
|/Transform|Trasforma un file di criteri di sicurezza generato tramite la configurazione Guidata in un nuovo oggetto Criteri di gruppo (GPO) in servizi di dominio Active Directory.</br>Vedere [Scwcmd: trasformare](scwcmd-transform.md) sintassi e le opzioni.|
|/View|Esegue il rendering di un file XML utilizzando una trasformazione XSL specificato.</br>Vedere [Scwcmd: vista](scwcmd-view.md) per la sintassi e le opzioni.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
