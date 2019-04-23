---
title: secedit:import
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1dd59d4c-9d48-444a-871b-b957eb682597
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f24cf173d1bacd70d92b325bfe7b342d0589a490
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874282"
---
# <a name="seceditimport"></a>secedit:import



Importa le impostazioni di protezione memorizzate in un file inf esportato in precedenza dal database configurato con modelli di protezione. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
Secedit /import /db <database file name> /cfg <configuration file name> [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]

```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|db|Obbligatorio.</br>Specifica il percorso e il nome di un database che contiene la configurazione memorizzata in cui verrà eseguita l'importazione.</br>Se il nome di file specifica un database che non ha un modello di sicurezza (come indicato dal file di configurazione) associato, il `/cfg \<configuration file name>` essere specificata anche l'opzione della riga di comando.|
|overwrite|Facoltativo.</br>Specifica se il modello di sicurezza nel parametro /cfg sovrascriverà qualsiasi modello o modello composito, che viene archiviato nel database anziché aggiungere i risultati al modello archiviato.</br>Questa opzione della riga di comando è valido solo quando il `/cfg \<configuration file name>` parametro viene utilizzato anche. Se non viene specificato, il modello nel parametro /cfg viene aggiunto al modello archiviato.|
|config.|Obbligatorio.</br>Specifica il percorso e il nome del modello di protezione che verrà importato nel database per l'analisi.</br>Questa opzione /cfg è valida solo quando utilizzato con il `/db \<database file name>` parametro. Se non viene specificato, l'analisi viene eseguita con le configurazioni già memorizzate nel database.|
|overwrite|Facoltativo.</br>Specifica se il modello di sicurezza nel parametro /cfg sovrascriverà qualsiasi modello o modello composito, che viene archiviato nel database anziché aggiungere i risultati al modello archiviato.</br>Questa opzione della riga di comando è valido solo quando il `/cfg \<configuration file name>` parametro viene utilizzato anche. Se non viene specificato, il modello nel parametro /cfg viene aggiunto al modello archiviato.|
|aree|Facoltativo.</br>Specifica le aree di sicurezza da applicare al sistema. Se questo parametro viene omesso, tutte le impostazioni di sicurezza definite nel database vengono applicate al sistema. Per configurare più aree, separarle con uno spazio. Sono supportate le seguenti aree di sicurezza:</br>-SecurityPolicy</br>    Criteri locali e criteri di dominio per il sistema, compresi i criteri di account, controllare i criteri, le opzioni di protezione e così via.</br>-Group_Mgmt</br>    Impostazioni di gruppo per tutti i gruppi specificati nel modello di sicurezza con restrizioni.</br>-User_Rights</br>    Diritti di accesso e concessione di privilegi.</br>-RegKeys</br>    Sicurezza nelle chiavi del Registro di sistema locale.</br>-Cache dei file</br>    Protezione archivio file locale.</br>-Servizi</br>    Sicurezza per tutti i servizi definiti.|
|log|Facoltativo.</br>Specifica il percorso e il nome del file di log per il processo.|
|quiet|Facoltativo.</br>Disattiva l'output dello schermo e di log. È possibile visualizzare i risultati di analisi tramite l'analisi e configurazione della protezione snap-in a Microsoft Management Console (MMC).|

## <a name="remarks"></a>Note

Prima di importare un file. inf in un altro computer, eseguire il /generaterollback secedit comando sul database in cui verrà eseguita l'importazione e secedit /validate nel file di importazione per verificarne l'integrità.

Se il percorso del file di log non viene specificato, il file di registro predefinito (*systemroot*\Documents and Settings\*UserAccount*\My Documents\Security\Logs\*NomeDatabase*. viene usato log).

In Windows Server 2008, `Secedit /refreshpolicy` è stato sostituito con `gpupdate`. Per informazioni su come aggiornare le impostazioni di sicurezza, vedere [Gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Esempi

Esportare il database di protezione e i criteri di sicurezza di dominio in un file inf e quindi importare tale file a un database diverso per replicare le impostazioni di criteri di sicurezza in un altro computer.
```
Secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg NetworkShare\Policies\SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```
Importare solo la parte di criteri di sicurezza del file a un database diverso in un altro computer.
```
Secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg NetworkShare\Policies\SecContoso.inf /areas securitypolicy /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Secedit:export](secedit-export.md)
-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit:validate](secedit-validate.md)
-   [Secedit](secedit.md)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)