---
title: bootcfg
description: Argomento di riferimento per il comando bootcfg, che consente di configurare, eseguire query o modificare le impostazioni del file Boot. ini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3deb354c-5717-4066-bc79-b9323d559e44
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aca24cfbf47586ae1d7d4262c232be47a056f7ae
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82708861"
---
# <a name="bootcfg"></a>bootcfg

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura, interroga o modifica le impostazioni del file Boot.ini.

## <a name="syntax"></a>Sintassi

```  
bootcfg <parameter> [arguments...]  
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| [bootcfg addsw](bootcfg-addsw.md) | Consente di aggiungere opzioni di caricamento del sistema operativo per una voce del sistema operativo specificato. |
| [bootcfg copy](bootcfg-copy.md) | Crea una copia di una voce di avvio esistente, a cui è possibile aggiungere opzioni della riga di comando. |
| [bootcfg dbg1394](bootcfg-dbg1394.md) | Consente di configurare il debug 1394 porta per una voce del sistema operativo specificato. |
| [bootcfg debug](bootcfg-debug.md) | Aggiunge o modifica le impostazioni di debug per una voce del sistema operativo specificato. |
| [bootcfg default](bootcfg-default.md) | Specifica la voce del sistema operativo per specificare che il valore predefinito. |
| [bootcfg delete](bootcfg-delete.md) | Elimina una voce del sistema operativo della sezione [operating systems] del file Boot. ini. |
| [bootcfg ems](bootcfg-ems.md) | Consente all'utente di aggiungere o modificare le impostazioni per il reindirizzamento della console di servizi di gestione emergenze a un computer remoto. |
| [bootcfg query](bootcfg-query.md) | Esegue query e Visualizza [caricatore di avvio] e le voci da Boot. ini di sezione [operating systems]. |
| [bootcfg raw](bootcfg-raw.md) | Aggiunge le opzioni di caricamento del sistema operativo specificate come stringa a una voce del sistema operativo di [i sistemi operativi] sezione del file Boot. ini. |
| [bootcfg rmsw](bootcfg-rmsw.md) | Rimuove opzioni di caricamento del sistema operativo per una voce del sistema operativo specificato. |
| [bootcfg timeout](bootcfg-timeout.md) | Modifica il valore di timeout del sistema operativo. |
