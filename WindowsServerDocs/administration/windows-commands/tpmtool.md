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
ms.openlocfilehash: 6f529939304cb3992992d9587c2180f80f8a0f01
ms.sourcegitcommit: 9889f20270e8eb7508d06cbf844cba9159e39697
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/18/2020
ms.locfileid: "83551117"
---
# <a name="tpmtool"></a>tpmtool

Questa utilità può essere utilizzata per ottenere informazioni sulla [Trusted Platform Module (TPM)](https://docs.microsoft.com/windows/security/information-protection/tpm/trusted-platform-module-overview).

>[!IMPORTANT]
>Alcune informazioni sono relative a un prodotto non definitivo che potrebbe subire modifiche sostanziali prima del rilascio sul mercato. Microsoft non riconosce alcuna garanzia, espressa o implicita, in merito alle informazioni qui fornite.

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
Per raccogliere i log TPM e inserirli in `C:\Users\Public` , digitare:
```
tpmtool gatherlogs C:\Users\Public
```
Per raccogliere le tracce del driver TPM, digitare:
```
tpmtool drivertracing start
# Run scenario
tpmtool drivertracing stop
```

## <a name="decoding-error-codes"></a>Decodifica di codici di errore

I codici di errore specifici del TPM sono descritti [qui](https://docs.microsoft.com/windows/desktop/com/com-error-codes-6).
