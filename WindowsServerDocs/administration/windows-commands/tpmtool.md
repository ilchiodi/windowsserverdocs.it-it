---
title: tpmtool
description: Argomento i comandi di Windows per tpmtool - Ottiene informazioni su Trusted Platform Module.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: ashleytqy
ms.author: ashleytqy
manager: ronaldai
ms.date: 05/07/2019
ms.openlocfilehash: e125dbf6127b92c91e041c431f1e462e1f884168
ms.sourcegitcommit: 0ff812a80f654fa2c35b1632524e27841eca75c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/16/2019
ms.locfileid: "68230859"
---
# <a name="tpmtool"></a>tpmtool

Questa utilità può essere utilizzata per ottenere le informazioni di [modulo TPM (Trusted Platform)](https://docs.microsoft.com/windows/security/information-protection/tpm/trusted-platform-module-overview).

>[!IMPORTANT]
>Alcune informazioni sono relative a un prodotto non definitivo che potrebbe subire modifiche sostanziali prima del rilascio sul mercato. Microsoft non riconosce alcuna garanzia, espressa o implicita, in merito alle informazioni qui fornite.

Per esempi di utilizzo di questo comando, vedere [Esempi](#tpmtool_examples).

## <a name="syntax"></a>Sintassi

```
tpmtool /parameter [<arguments>]
```
## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|getdeviceinformation|Visualizza le informazioni di base del TPM. Il significato dei valori di flag informazioni sono reperibili [qui](https://docs.microsoft.com/windows/desktop/SecProv/win32-tpm-isreadyinformation#parameters).|
|stesse [percorso di directory di output]|Raccoglie i log di TPM e li inserisce nella directory specificata. Se tale directory non esiste, viene creato. Per impostazione predefinita, vengono inseriti nella directory corrente. I possibili file generati sono: </br>-TpmEvents.evtx</br>-TpmInformation.txt</br>-SRTMBoot.dat</br>-SRTMResume.dat</br>-DRTMBoot.dat</br>-DRTMResume.dat</br>|
|drivertracing [Avvia / Arresta]|Avvia / Arresta la raccolta di tracce di driver TPM. Il log di traccia, TPMTRACE.etl, verrà generato e inserito nella directory corrente.|
|parsetcglogs [-convalidare (-v)]|Consente di visualizzare il log TCG analizzato, noto anche come il Windows Boot Configuration Log (WBCL). Le descrizioni dell'evento più aggiornate sono reperibile nella [sito Web TCG](https://trustedcomputinggroup.org/resource/pc-client-specific-platform-firmware-profile-specification/), in **descrizioni degli eventi**. Se il `-validate` impostato, verifica che i valori di configurazione registra di piattaforma (PCR) del TPM corrispondano ai valori nel registro.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="tpmtool_examples"></a>Esempi

Per visualizzare le informazioni di base del TPM, digitare:
```
tpmtool getdeviceinformation
```
Per raccogliere i registri TPM e inserirli nella directory corrente, digitare:
```
tpmtool gatherlogs
```
Per raccogliere i registri TPM e inserirli in `C:\Users\Public`, tipo:
```
tpmtool gatherlogs C:\Users\Public
```
Per raccogliere le tracce del driver TPM, digitare:
```
tpmtool drivertracing start
# Run scenario
tpmtool drivertracing stop
```
Analizzare il log TCG:
```
tpmtool parsetcglogs
```
Per analizzare il log TCG e convalidare PCRs:
```
tpmtool parsetcglogs -validate
```

## <a name="decoding-error-codes"></a>Decodifica dei codici di errore

Codici di errore specifiche TPM sono documentati [qui](https://docs.microsoft.com/windows/desktop/com/com-error-codes-6).
