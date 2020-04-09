---
title: 'secedit: importazione'
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1dd59d4c-9d48-444a-871b-b957eb682597
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47cd3b74a52af149706c6e6d238a076a9bc61f98
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834884"
---
# <a name="seceditimport"></a>secedit: importazione



Importa le impostazioni di protezione memorizzate in un file inf esportato in precedenza dal database configurato con modelli di protezione. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
Secedit /import /db <database file name> /cfg <configuration file name> [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|DB|Obbligatoria.</br>Specifica il percorso e il nome di un database che contiene la configurazione memorizzata in cui verrà eseguita l'importazione.</br>Se il nome di file specifica un database che non ha un modello di sicurezza (come indicato dal file di configurazione) associato, il `/cfg \<configuration file name>` essere specificata anche l'opzione della riga di comando.|
|overwrite|Facoltativa.</br>Specifica se il modello di sicurezza nel parametro /cfg sovrascriverà qualsiasi modello o modello composito, che viene archiviato nel database anziché aggiungere i risultati al modello archiviato.</br>Questa opzione della riga di comando è valido solo quando il `/cfg \<configuration file name>` parametro viene utilizzato anche. Se non viene specificato, il modello nel parametro /cfg viene aggiunto al modello archiviato.|
|config.|Obbligatoria.</br>Specifica il percorso e il nome del modello di protezione che verrà importato nel database per l'analisi.</br>Questa opzione /cfg è valida solo quando utilizzato con il `/db \<database file name>` parametro. Se non viene specificato, l'analisi viene eseguita con le configurazioni già memorizzate nel database.|
|overwrite|Facoltativa.</br>Specifica se il modello di sicurezza nel parametro /cfg sovrascriverà qualsiasi modello o modello composito, che viene archiviato nel database anziché aggiungere i risultati al modello archiviato.</br>Questa opzione della riga di comando è valido solo quando il `/cfg \<configuration file name>` parametro viene utilizzato anche. Se non viene specificato, il modello nel parametro /cfg viene aggiunto al modello archiviato.|
|aree|Facoltativa.</br>Specifica le aree di sicurezza da applicare al sistema. Se questo parametro viene omesso, tutte le impostazioni di sicurezza definite nel database vengono applicate al sistema. Per configurare più aree, separarle con uno spazio. Sono supportate le seguenti aree di sicurezza:</br>-SecurityPolicy</br>    Criteri locali e criteri di dominio per il sistema, compresi i criteri di account, controllare i criteri, le opzioni di protezione e così via.</br>-Group_Mgmt</br>    Impostazioni di gruppo per tutti i gruppi specificati nel modello di sicurezza con restrizioni.</br>-User_Rights</br>    Diritti di accesso e concessione di privilegi.</br>-RegKeys</br>    Sicurezza nelle chiavi del Registro di sistema locale.</br>-Cache dei file</br>    Protezione archivio file locale.</br>-Servizi</br>    Sicurezza per tutti i servizi definiti.|
|log|Facoltativa.</br>Specifica il percorso e il nome del file di log per il processo.|
|quiet|Facoltativa.</br>Disattiva l'output dello schermo e di log. È possibile visualizzare i risultati di analisi tramite l'analisi e configurazione della protezione snap-in a Microsoft Management Console (MMC).|

## <a name="remarks"></a>Note

Prima di importare un file. inf in un altro computer, eseguire il /generaterollback secedit comando sul database in cui verrà eseguita l'importazione e secedit /validate nel file di importazione per verificarne l'integrità.

Se il percorso del file di log non viene specificato, viene usato il file di log predefinito, ovvero*systemroot*\Documents and Settings\*AccountUtente<em>\My Documents\Security\Logs\*DatabaseName</em>. log).

In Windows Server 2008, `Secedit /refreshpolicy` è stato sostituito con `gpupdate`. Per informazioni su come aggiornare le impostazioni di sicurezza, vedere [Gpupdate](gpupdate.md).

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi

Esportare il database di protezione e i criteri di sicurezza di dominio in un file inf e quindi importare tale file a un database diverso per replicare le impostazioni di criteri di sicurezza in un altro computer.
```
Secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg NetworkShare\Policies\SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```
Importare solo la parte di criteri di sicurezza del file a un database diverso in un altro computer.
```
Secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg NetworkShare\Policies\SecContoso.inf /areas securitypolicy /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

## <a name="additional-references"></a>Altre informazioni di riferimento

-   [Secedit:export](secedit-export.md)
-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit:validate](secedit-validate.md)
-   [Secedit](secedit.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)