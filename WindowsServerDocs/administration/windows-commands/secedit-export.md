---
title: secedit:export
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 49a8b241-aa8c-45b7-844d-67a29fab708e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 398d2fa47f2418aec910569c2eb85aec408ad482
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441596"
---
# <a name="seceditexport"></a>secedit:export



Esporta le impostazioni di protezione memorizzate in un database configurato con modelli di protezione. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
Secedit /export /db <database file name> [/mergedpolicy] /cfg <configuration file name> [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|db|Obbligatorio.</br>Specifica il percorso e il nome di un database che contiene la configurazione memorizzata con cui verrà eseguita l'analisi.</br>Se il nome di file specifica un database che non ha un modello di sicurezza (come indicato dal file di configurazione) associato, il `/cfg \<configuration file name>` essere specificata anche l'opzione della riga di comando.|
|mergedpolicy|Facoltativo.</br>Unisce ed Esporta impostazioni di sicurezza Criteri locali e di dominio.|
|config.|Obbligatorio.</br>Specifica il percorso e il nome del modello di protezione che verrà importato nel database per l'analisi.</br>Questa opzione /cfg è valida solo quando utilizzato con il `/db \<database file name>` parametro. Se non viene specificato, l'analisi viene eseguita con le configurazioni già memorizzate nel database.|
|aree|Facoltativo.</br>Specifica le aree di sicurezza da applicare al sistema. Se questo parametro viene omesso, tutte le impostazioni di sicurezza definite nel database vengono applicate al sistema. Per configurare più aree, separarle con uno spazio. Sono supportate le seguenti aree di sicurezza:</br>-SecurityPolicy</br>    Criteri locali e criteri di dominio per il sistema, compresi i criteri di account, controllare i criteri, le opzioni di protezione e così via.</br>-Group_Mgmt</br>    Impostazioni di gruppo per tutti i gruppi specificati nel modello di sicurezza con restrizioni.</br>-User_Rights</br>    Diritti di accesso e concessione di privilegi.</br>-RegKeys</br>    Sicurezza nelle chiavi del Registro di sistema locale.</br>-Cache dei file</br>    Protezione archivio file locale.</br>-Servizi</br>    Sicurezza per tutti i servizi definiti.|
|log|Facoltativo.</br>Specifica il percorso e il nome del file di log per il processo.|
|quiet|Facoltativo.</br>Disattiva l'output dello schermo e di log. È possibile visualizzare i risultati di analisi tramite l'analisi e configurazione della protezione snap-in a Microsoft Management Console (MMC).|

## <a name="remarks"></a>Note

È possibile utilizzare questo comando per eseguire il backup i criteri di protezione in un computer locale, oltre a importare le impostazioni in un altro computer.

Se il percorso del file di log non viene specificato, il file di registro predefinito (*systemroot*\Documents and Settings\*UserAccount<em>\My Documents\Security\Logs\*NomeDatabase</em>. viene usato log).

In Windows Server 2008, `Secedit /refreshpolicy` è stato sostituito con `gpupdate`. Per informazioni su come aggiornare le impostazioni di sicurezza, vedere [Gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Esempi

Esportare il database di protezione e i criteri di sicurezza di dominio in un file inf e quindi importare tale file a un database diverso per replicare le impostazioni di criteri di sicurezza in un altro computer.
```
Secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```
Importare il file a un database diverso in un altro computer.
```
Secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Secedit:import](secedit-import.md)
-   [Secedit](secedit.md)
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)