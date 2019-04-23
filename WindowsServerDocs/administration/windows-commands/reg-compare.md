---
title: Confronto Reg
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 177dc6a3-034e-4846-a394-330d03c14e0b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9f4a8c51e7add429aab326804af698fed7007999
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887222"
---
# <a name="reg-compare"></a>Confronto Reg



Consente di confrontare specificati sottochiavi del Registro di sistema o le voci.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
reg compare <KeyName1> <KeyName2> [{/v ValueName | /ve}] [{/oa | /od | /os | on}] [/s]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Nomechiave1 >|Specifica il percorso completo della sottochiave del primo da confrontare. Per specificare un computer remoto, includere il nome del computer (nel formato \\ \\nomecomputer\) come parte del *KeyName*. L'omissione \\ \\ComputerName\ fa sì che l'operazione per impostazione predefinita nel computer locale. Il *KeyName* deve includere una chiave radice valido. Le chiavi principali valide per il computer locale sono: HKLM, HKCU, HKCR, HKU e HKCC. Se viene specificato un computer remoto, le chiavi principali valide sono: HKLM e HKU.|
|\<KeyName2>|Specifica il percorso completo della seconda sottochiave da confrontare. Per specificare un computer remoto, includere il nome del computer (nel formato \\ \\nomecomputer\) come parte del *KeyName*. L'omissione \\ \\ComputerName\ fa sì che l'operazione per impostazione predefinita nel computer locale. Specifica il nome del computer in *Nomechiave2* fa sì che l'operazione utilizzare il percorso per la sottochiave specificata *Nomechiave1*. Il *KeyName* deve includere una chiave radice valido. Le chiavi principali valide per il computer locale sono: HKLM, HKCU, HKCR, HKU e HKCC. Se viene specificato un computer remoto, le chiavi principali valide sono: HKLM e HKU.|
|/v \<ValueName>|Specifica il nome del valore da confrontare nella sottochiave.|
|/ve|Specifica che solo le voci con il nome di un valore null devono essere confrontate.|
|[{/oa | /od | /OS | on}]|Specifica la modalità visualizzare i risultati dell'operazione di confronto. Il valore predefinito è **/od**. Vedere le opzioni seguenti.|
|/Oa|Specifica che vengono visualizzate tutte le differenze e le corrispondenze. Per impostazione predefinita, vengono elencate solo le differenze.|
|/od|Specifica che vengono visualizzate solo le differenze. Questo è il comportamento predefinito.|
|/OS|Specifica che vengono visualizzate solo le corrispondenze. Per impostazione predefinita, vengono elencate solo le differenze.|
|/on|Specifica che verrà visualizzato nulla. Per impostazione predefinita, vengono elencate solo le differenze.|
|/s|Confronta tutti in modo ricorsivo le sottochiavi e voci.|
|/?|Visualizza la Guida per **reg compare** al prompt dei comandi.|

## <a name="remarks"></a>Note

Nella tabella seguente sono elencati i valori restituiti per **reg compare**.

|Value|Descrizione|
|-----|-----------|
|0|Il confronto ha esito positivo e il risultato è identico.|
|1|Il confronto non riuscito.|
|2|Il confronto ha avuto esito positivo e sono state rilevate differenze.|

Nella tabella seguente sono elencati i simboli visualizzati nei risultati.

|Simbolo|Descrizione|
|------|-----------|
|=|*Nomechiave1* dati sono uguali a *Nomechiave2* dati.|
|<|*Nomechiave1* dati sono minore di *Nomechiave2* dati.|
|>|*Nomechiave1* dati sono maggiori di *Nomechiave2* dati.|

## <a name="BKMK_examples"></a>Esempi

Per confrontare tutti i valori nella chiave **MyApp** con tutti i valori nella chiave **SalvaMiaApp**, tipo:

REG COMPARE HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp

Per confrontare il valore per la versione nella chiave **MyCo** e il valore per la versione nella chiave **MyCo1**, tipo:

REG COMPARE HKLM\Software\MyCo HKLM\Software\MyCo1 /v Version

Per confrontare tutte le sottochiavi e valori sotto HKLM\Software\MyCo nel computer denominato ZODIAC con tutte le sottochiavi e valori sotto HKLM\Software\MyCo nel computer locale, digitare:

REG COMPARE \\\\ZODIAC\HKLM\Software\MyCo \\\\. /s

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)