---
title: 'secedit: configura'
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a92e68ca-003c-4219-8655-0e7734f5fab3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d659429f4aa60d7c12ae0ba93e068cc0c99c093
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722082"
---
# <a name="seceditconfigure"></a>secedit: configura



Consente di configurare le impostazioni di sistema corrente utilizzando impostazioni di protezione memorizzate in un database.

## <a name="syntax"></a>Sintassi

```
Secedit /configure /db <database file name> [/cfg <configuration file name>] [/overwrite] [/areas SECURITYPOLICY | GROUP_MGMT | USER_RIGHTS | REGKEYS | FILESTORE | SERVICES] [/log <log file name>] [/quiet]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|db|Obbligatorio.</br>Specifica il percorso e il nome di un database che contiene la configurazione archiviata.</br>Se il nome di file specifica un database che non ha un modello di sicurezza (come indicato dal file di configurazione) associato, il `/cfg \<configuration file name>` essere specificata anche l'opzione della riga di comando.|
|config.|Facoltativa.</br>Specifica il percorso e il nome del modello di protezione che verrà importato nel database per l'analisi.</br>Questa opzione /cfg è valida solo quando utilizzato con il `/db \<database file name>` parametro. Se non viene specificato, l'analisi viene eseguita con le configurazioni già memorizzate nel database.|
|overwrite|Facoltativa.</br>Specifica se il modello di sicurezza nel parametro /cfg sovrascriverà qualsiasi modello o modello composito, che viene archiviato nel database anziché aggiungere i risultati al modello archiviato.</br>Questa opzione della riga di comando è valido solo quando il `/cfg \<configuration file name>` parametro viene utilizzato anche. Se non viene specificato, il modello nel parametro /cfg viene aggiunto al modello archiviato.|
|aree|Facoltativa.</br>Specifica le aree di sicurezza da applicare al sistema. Se questo parametro viene omesso, tutte le impostazioni di sicurezza definite nel database vengono applicate al sistema. Per configurare più aree, separarle con uno spazio. Sono supportate le seguenti aree di sicurezza:</br>-SecurityPolicy</br>    Criteri locali e criteri di dominio per il sistema, compresi i criteri di account, controllare i criteri, le opzioni di protezione e così via.</br>-Group_Mgmt</br>    Impostazioni di gruppo per tutti i gruppi specificati nel modello di sicurezza con restrizioni.</br>-User_Rights</br>    Diritti di accesso e concessione di privilegi.</br>-RegKeys</br>    Sicurezza nelle chiavi del Registro di sistema locale.</br>-Cache dei file</br>    Protezione archivio file locale.</br>-Servizi</br>    Sicurezza per tutti i servizi definiti.|
|log|Facoltativa.</br>Specifica il percorso e il nome del file di log per il processo.|
|tranquillo|Facoltativa.</br>Disattiva l'output dello schermo e di log. È possibile visualizzare i risultati di analisi tramite l'analisi e configurazione della protezione snap-in a Microsoft Management Console (MMC).|

## <a name="remarks"></a>Osservazioni

Se il percorso del file di log non viene specificato, il file di registro predefinito (*systemroot*\Users \*UserAccount<em>\My Documents\Security\Logs\*DatabaseName</em>. log) viene utilizzato.

A partire da Windows Server 2008, `Secedit /refreshpolicy` è stato sostituito con `gpupdate`. Per informazioni su come aggiornare le impostazioni di sicurezza, vedere [Gpupdate](gpupdate.md).

## <a name="examples"></a>Esempi

Eseguire l'analisi per i parametri di sicurezza nel database di protezione, SecDbContoso.sdb, creato utilizzando la configurazione della protezione e snap-in analisi. Indirizzare l'output al file SecAnalysisContosoFY11 con cui viene richiesto, consente di verificare il comando eseguito correttamente.
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /log C:\Security\FY11\SecAnalysisContosoFY11.log
```
Si direbbe che l'analisi ha rivelato alcune carenze, quindi il modello di sicurezza SecContoso. inf è stato modificato. Eseguire nuovamente il comando per rendere effettive le modifiche, indirizzando l'output al file esistente SecAnalysisContosoFY11 senza richiesta.
```
Secedit /configure /db C:\Security\FY11\SecDbContoso.sdb /cfg SecContoso.inf /overwrite /log C:\Security\FY11\SecAnalysisContosoFY11.xml /quiet
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   [Secedit](secedit.md)
-   [Secedit:analyze](secedit-analyze.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)