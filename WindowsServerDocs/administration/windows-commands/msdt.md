---
title: msdt
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 09f3f738d5a7ba7f7c40cb4eb2ce043856b378cc
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/24/2019
ms.locfileid: "63718262"
---
# <a name="msdt"></a>msdt



Richiama un pacchetto di risoluzione dei problemi nella riga di comando o come parte di uno script automatizzato e abilita le opzioni aggiuntive senza l'intervento dell'utente.

## <a name="syntax"></a>Sintassi

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

## <a name="parameters"></a>Parametri

La tabella seguente include i parametri e opzioni supportate da msdt.exe.

|Parametro|Descrizione|
|---------|-----------|
|/ID \<nome pacchetto >|Specifica il pacchetto di diagnostica per l'esecuzione. Per un elenco dei pacchetti disponibili, vedere l'ID di pacchetto di risoluzione dei problemi in "disponibile risoluzione dei problemi relativi a pacchetti? sezione più avanti in questo argomento.|
|/Path \<directory | file .diagpkg | file .diagcfg >|Specifica il percorso completo di un pacchetto di diagnostica. Se si specifica una directory, la directory deve contenere un pacchetto di diagnostica. È possibile usare il parametro /path in combinazione con il */id*, */dci*, o */cab* parametro.|
|/dci \<passkey>|Vengono precompilate da per il campo passkey per il supporto tecnico Microsoft. Questo parametro viene utilizzato solo quando un servizio di supporto tecnico è fornito una passkey.|
|/dt \<directory>|Consente di visualizzare la cronologia di risoluzione dei problemi nella directory specificata. I risultati di diagnostica vengono archiviati dell'utente **%LOCALAPPDATA%\Diagnostics** oppure **%LOCALAPPDATA%\ElevatedDiagnostics** le directory.|
|visualizzare \<file di risposte >|Specifica un file di risposte in formato XML che contiene le risposte a uno o più interazioni di diagnostica.|
