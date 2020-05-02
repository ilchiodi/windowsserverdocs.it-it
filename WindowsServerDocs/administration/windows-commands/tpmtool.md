---
title: tpmtool
description: Argomento di riferimento per tpmtool, che ottiene informazioni sul Trusted Platform Module.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: ashleytqy
ms.author: ashleytqy
manager: ronaldai
ms.date: 05/07/2019
ms.openlocfilehash: f2e283dd20d22418416958686d77605976923eaf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721335"
---
# <a name="tpmtool"></a>tpmtool

Questa utilità può essere utilizzata per ottenere informazioni sulla [Trusted Platform Module (TPM)](https://docs.microsoft.com/windows/security/information-protection/tpm/trusted-platform-module-overview).

>[!IMPORTANT]
>Alcune informazioni sono relative a un prodotto non definitivo che potrebbe subire modifiche sostanziali prima del rilascio sul mercato. Microsoft non offre alcuna garanzia, esplicita o implicita, riguardo alle informazioni fornite in questo documento.

Per esempi di utilizzo di questo comando, vedere [Esempi](#tpmtool_examples).

## <a name="syntax"></a>Sintassi

```
tpmtool /parameter [<arguments>]
```
### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|getdeviceinformation|Visualizza le informazioni di base del TPM. Il significato dei valori del flag informazioni è disponibile [qui](https://docs.microsoft.com/windows/desktop/SecProv/win32-tpm-isreadyinformation#parameters).|
|GatherLogs [percorso della directory di output]|Raccoglie i log TPM e li inserisce nella directory specificata. Se la directory non esiste, viene creata. Per impostazione predefinita, vengono inseriti nella directory corrente. I file possibili generati sono: </br>-TpmEvents. evtx</br>-TpmInformation. txt</br>-SRTMBoot. dat</br>-SRTMResume. dat</br>-DRTMBoot. dat</br>-DRTMResume. dat</br>|
|drivertracing [Start/Stop]|Avviare/arrestare la raccolta delle tracce del driver TPM. Il log di traccia, TPMTRACE. ETL, verrà generato e inserito nella directory corrente.|
|parsetcglogs [-Validate (-v)]|Consente di visualizzare il log TCG analizzato, noto anche come log di configurazione di avvio di Windows (WBCL). Le descrizioni degli eventi più aggiornati sono reperibili nel [sito Web TCG](https://trustedcomputinggroup.org/resource/pc-client-specific-platform-firmware-profile-specification/), in **descrizioni degli eventi**. Se il `-validate` flag è impostato, verifica che i valori del registro di configurazione della piattaforma (PCR) nel TPM corrispondano ai valori nel log.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="examples"></a><a name=tpmtool_examples></a>Esempi

Per visualizzare le informazioni di base del TPM, digitare:
```
tpmtool getdeviceinformation
```
Per raccogliere i log TPM e inserirli nella directory corrente, digitare:
```
tpmtool gatherlogs
```
Per raccogliere i log TPM e inserirli in `C:\Users\Public`, digitare:
```
tpmtool gatherlogs C:\Users\Public
```
Per raccogliere le tracce del driver TPM, digitare:
```
tpmtool drivertracing start
# Run scenario
tpmtool drivertracing stop
```
Per analizzare il registro TCG:
```
tpmtool parsetcglogs
```
Per analizzare il registro TCG e convalidare il PCRs:
```
tpmtool parsetcglogs -validate
```

## <a name="decoding-error-codes"></a>Decodifica di codici di errore

I codici di errore specifici del TPM sono descritti [qui](https://docs.microsoft.com/windows/desktop/com/com-error-codes-6).
