---
title: reg Aggiungi
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d9ad143e-dc10-4e2e-a229-408393c40079
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: df59477c980169699dac897e36836e5226b6a0fa
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836594"
---
# <a name="reg-add"></a>reg Aggiungi


Aggiunge una nuova sottochiave o voce nel Registro di sistema.

## <a name="syntax"></a>Sintassi

```
reg add <KeyName> [{/v ValueName | /ve}] [/t DataType] [/s Separator] [/d Data] [/f]
```
Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

### <a name="parameters"></a>Parametri

|      Parametro      |                                                                                                                                                                                                                                                                   Descrizione                                                                                                                                                                                                                                                                   |
|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \<nome della<em>></em> | Specifica il percorso completo della sottochiave o voce da aggiungere. Per specificare un computer remoto, includere il nome del computer (nel formato \\\\\<nomecomputer >\) come parte del *nome*della pagina. Se si omette \\\\nomecomputer \ l'operazione viene impostata sul computer locale per impostazione predefinita. Il *KeyName* deve includere una chiave radice valido. Le chiavi principali valide per il computer locale sono: HKLM, HKCU, HKCR, HKU e HKCC. Se viene specificato un computer remoto, le chiavi principali valide sono: HKLM e HKU. Se il nome della chiave del registro di sistema contiene uno spazio, racchiudere il nome della chiave tra virgolette. |
|   /v \<valore >   |                                                                                                                                                                                                                                Specifica il nome della voce del Registro di sistema da aggiungere nella sottochiave specificata.                                                                                                                                                                                                                                 |
|         /ve         |                                                                                                                                                                                                                                Specifica che la voce del Registro di sistema che viene aggiunto al Registro di sistema ha un valore null.                                                                                                                                                                                                                                |
|     /t \<tipo >      |                                                                                                                                          Specifica il tipo per la voce del Registro di sistema. *Tipo* deve essere uno dei seguenti:</br>REG_SZ</br>REG_MULTI_SZ</br>REG_DWORD_BIG_ENDIAN</br>REG_DWORD</br>REG_BINARY</br>REG_DWORD_LITTLE_ENDIAN</br>REG_LINK</br>REG_FULL_RESOURCE_DESCRIPTOR</br>REG_EXPAND_SZ                                                                                                                                          |
|   /s \<separatore >   |                                                                                                                                                              Specifica il carattere da utilizzare per separare più istanze di dati quando viene specificato il tipo di dati REG_MULTI_SZ e deve essere elencato più di una voce. Se non specificato, il separatore predefinito è **\0**.                                                                                                                                                              |
|     /d \<> dati      |                                                                                                                                                                                                                                                 Specifica i dati per la nuova voce del Registro di sistema.                                                                                                                                                                                                                                                  |
|         /f          |                                                                                                                                                                                                                                           Aggiunge la voce del Registro di sistema senza chiedere conferma.                                                                                                                                                                                                                                           |
|         /?          |                                                                                                                                                                                                                                              Visualizza la Guida per **reg aggiungere** al prompt dei comandi.                                                                                                                                                                                                                                               |

## <a name="remarks"></a>Note

-   Impossibile aggiungere sottostrutture con questa operazione. Questa versione di **reg** chiedere conferma quando si aggiunge una sottochiave.
-   Nella tabella seguente sono elencati i valori restituiti per il **reg aggiungere** operazione.

| Valore | Descrizione |
|-------|-------------|
|   0   |   Success   |
|   1   |   Operazione non riuscita   |

-   Per il tipo di chiave REG_EXPAND_SZ usare il simbolo del cursore ( **^** ) con **%** all'interno del parametro/d

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per aggiungere la chiave HKLM\Software\MyCo sul computer remoto ABC, digitare:
```
REG ADD \\ABC\HKLM\Software\MyCo
```
Per aggiungere una voce del Registro di sistema a HKLM\Software\MiaSoc con un valore denominato **dati** di tipo REG_BINARY e i dati di **fe340ead**, tipo:
```
REG ADD HKLM\Software\MyCo /v Data /t REG_BINARY /d fe340ead
```
Per aggiungere una voce del Registro di sistema multivalore a HKLM\Software\MiaSoc con il nome di un valore di **MRU** di tipo REG_MULTI_SZ e i dati di **fax\0mail\0\0**, tipo:
```
REG ADD HKLM\Software\MyCo /v MRU /t REG_MULTI_SZ /d fax\0mail\0\0
```
Per aggiungere una voce del Registro di sistema espansa a HKLM\Software\MiaSoc con il nome di un valore di **percorso** di tipo REG_EXPAND_SZ e i dati di **% systemroot %** , tipo:
```
REG ADD HKLM\Software\MyCo /v Path /t REG_EXPAND_SZ /d ^%systemroot^%
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
