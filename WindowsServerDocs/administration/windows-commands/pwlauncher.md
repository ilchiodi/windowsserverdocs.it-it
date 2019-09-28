---
title: pwlauncher
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0917bb7b-408a-40f7-a1c5-20e94c10d38b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4cf65643e0c5a4b28e06619a8156792b6e5456ea
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371970"
---
# <a name="pwlauncher"></a>pwlauncher



Abilita o Disabilita le opzioni di avvio di Windows to go (pwlauncher). Lo strumento da riga di comando **pwlauncher** consente di configurare il computer per l'avvio automatico in un'area di lavoro Windows to go (presupponendo che ne sia presente uno), senza che sia necessario immettere il firmware o modificare le opzioni di avvio.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
Pwlauncher {/enable | /disable}
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/Enable|Abilita le opzioni di avvio di Windows to go in modo che il computer venga avviato automaticamente da un dispositivo USB quando presente|
|/Disable|Disabilita le opzioni di avvio di Windows to go in modo che il computer non possa essere avviato da un dispositivo USB, a meno che non sia configurato manualmente nel firmware.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

Il più grande ostacolo per un utente che desidera utilizzare Windows to go consiste nel far avviare il computer da USB. Questa operazione viene eseguita tradizionalmente immettendo il firmware e provando diverse opzioni di configurazione fino a quando il computer non è configurato correttamente. Non si tratta di un'impresa semplice per la maggior parte degli utenti ed è estremamente rischiosa perché il firmware contiene opzioni che possono rendere inutilizzabile un sistema se usato in modo non corretto. Per risolvere questo problema, Windows 8and sistemi operativi successivi include una funzionalità denominata "Windows to go Startup Options", che consente a un utente di configurare il computer per l'avvio da USB da Windows senza mai immettere il firmware, purché i relativi il firmware supporta l'avvio da USB. Per consentire a un sistema di eseguire sempre l'avvio da USB, è necessario prendere in considerazione le implicazioni. Ad esempio, un dispositivo USB che include malware potrebbe essere avviato inavvertitamente per compromettere il sistema oppure è possibile collegare più unità USB per provocare un conflitto di avvio. Per questo motivo, la configurazione predefinita include le opzioni di avvio di Windows to go disabilitate per impostazione predefinita. Inoltre, sono necessari i privilegi di amministratore per configurare le opzioni di avvio di Windows to go. Se si abilitano le opzioni di avvio di Windows to go usando lo strumento da riga di comando pwlauncher o l'app **Change Windows to go Startup Options** , il computer tenterà di eseguire l'avvio da qualsiasi dispositivo USB inserito nel computer prima di avviarlo.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente viene illustrato come è possibile utilizzare il comando **pwlauncher** per abilitare l'avvio da USB:
```
Pwlauncher /enable
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)