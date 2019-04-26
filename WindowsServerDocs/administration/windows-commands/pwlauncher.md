---
title: pwlauncher
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1aa2e22f74323bb6cabfc644ca67e17a7fcbd3fc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851022"
---
# <a name="pwlauncher"></a>pwlauncher



Abilita o disabilita il Windows To Go Startup Options (pwlauncher). Il **pwlauncher** lo strumento da riga di comando consente di configurare il computer per l'avvio in un'area di lavoro Windows To Go automaticamente (supponendo che uno è presente), senza che sia necessario immettere il firmware o modificare le opzioni di avvio.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
Pwlauncher {/enable | /disable}
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/Enable|Abilita le opzioni di avvio di Windows To Go in modo che il computer verrà automaticamente avviato da un dispositivo USB, quando è presente|
|/Disable|Disabilita le opzioni di avvio di Windows To Go in modo che il computer non può essere avviato da un dispositivo USB a meno che non configurate manualmente nel firmware.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

Il principale ostacolo per un utente che desiderano utilizzare Windows To Go è ottenere i computer per l'avvio da USB. Questa operazione viene in genere eseguita immettendo il firmware e provare diverse opzioni di configurazione fino a quando il computer è configurato correttamente. Ciò non è un'impresa di semplice per la maggior parte degli utenti ed è estremamente rischiosa perché il firmware contiene opzioni che è possano eseguire il rendering di un sistema inutilizzabile se usato in modo errato. Per risolvere questo problema, Windows 8and nei sistemi operativi successivi includono una funzionalità denominata "Windows per passare opzioni di avvio? che consente di configurare i propri computer per l'avvio da USB all'interno di Windows, ovvero senza mai immesso il firmware, purché il firmware supporta l'avvio da USB. L'abilitazione di un sistema eseguire sempre l'avvio da USB prima di tutto ha implicazioni che è opportuno considerare. Ad esempio, un dispositivo USB che include il malware può essere avviato inavvertitamente per compromettere il sistema o potrebbe essere collegata più unità USB per provocare un conflitto di avvio. Per questo motivo, la configurazione predefinita è di Windows To Go Startup Options disabilitata per impostazione predefinita. Inoltre, sono necessari privilegi di amministratore per configurare Windows To Go Startup Options. Se si abilita le opzioni di avvio di Windows To Go utilizzando lo strumento da riga di comando pwlauncher o la **modifica Windows To Go Startup Options** app il computer tenterà di eseguire l'avvio da qualsiasi dispositivo USB che viene inserita nel computer prima che venga avviato.

## <a name="BKMK_examples"></a>Esempi

L'esempio seguente illustra come usare il **pwlauncher** comando per abilitare l'avvio da USB:
```
Pwlauncher /enable
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)