---
title: bootcfg
description: Windows Commands argomento per bootcfg, che consente di configurare, eseguire query o modificare le impostazioni del file Boot. ini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3deb354c-5717-4066-bc79-b9323d559e44
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a977b857242c030515a09a67eb0d284ade7a0beb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848384"
---
# <a name="bootcfg"></a>bootcfg

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura, interroga o modifica le impostazioni del file Boot.ini.

## <a name="syntax"></a>Sintassi

```  
bootcfg <parameter> [arguments...]  
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|  
|-------|--------|  
|[bootcfg addsw](bootcfg-addsw.md)|aggiunge le opzioni di caricamento del sistema operativo per una voce del sistema operativo specificata.|  
|[bootcfg copy](bootcfg-copy.md)|Crea una copia di una voce di avvio esistente, a cui è possibile aggiungere opzioni della riga di comando.|  
|[bootcfg dbg1394](bootcfg-dbg1394.md)|Consente di configurare il debug 1394 porta per una voce del sistema operativo specificato.|  
|[bootcfg debug](bootcfg-debug.md)|aggiunge o modifica le impostazioni di debug per una voce del sistema operativo specificata.|  
|[bootcfg default](bootcfg-default.md)|Specifica la voce del sistema operativo per specificare che il valore predefinito.|  
|[bootcfg delete](bootcfg-delete.md)|Elimina una voce del sistema operativo nella sezione **[Operating Systems]** del file Boot. ini.|  
|[bootcfg ems](bootcfg-ems.md)|Consente all'utente di aggiungere o modificare le impostazioni per il reindirizzamento della console di servizi di gestione emergenze a un computer remoto.|  
|[bootcfg query](bootcfg-query.md)|Esegue query e Visualizza [caricatore di avvio] e **[i sistemi operativi]** sezione voci da Boot. ini.|  
|[bootcfg raw](bootcfg-raw.md)|aggiunge le opzioni di caricamento del sistema operativo specificate come stringa a una voce del sistema operativo nella sezione **[Operating Systems]** del file Boot. ini.|  
|[bootcfg rmsw](bootcfg-rmsw.md)|rimuove le opzioni di caricamento del sistema operativo per una voce del sistema operativo specificata.|  
|[bootcfg timeout](bootcfg-timeout.md)|modifica il valore di timeout del sistema operativo.|  
