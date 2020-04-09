---
title: bitsadmin addfilewithranges
description: Windows Commands Topic for **BITSAdmin ADDFILEWITHRANGES**, che aggiunge un file al processo specificato. BITS scarica gli intervalli specificati dal file remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: df0ce0bf-dff1-4a48-a16f-fd2f4d5f7189
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60b448550473691bbbaf573f99f00609e14e0f42
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850954"
---
# <a name="bitsadmin-addfilewithranges"></a>bitsadmin addfilewithranges

Aggiunge un file al processo specificato. BITS scarica gli intervalli specificati dal file remoto. Questa opzione è valida solo per i processi di download.

## <a name="syntax"></a>Sintassi

```
bitsadmin /AddFileWithRanges <Job> <RemoteURL> <LocalName> <RangeList>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| Job | Nome visualizzato o GUID del processo. |
| RemoteURL | URL del file nel server. |
| LocalName | Nome del file nel computer locale. Deve contenere un percorso assoluto del file. |
| Intervallo di valori | Elenco delimitato da virgole di coppie offset: lunghezza. Usare i due punti per separare il valore di offset dal valore length. Ad esempio, un valore di `0:100,2000:100,5000:eof` indica a BITS di trasferire 100 byte dall'offset 0, 100 byte dall'offset 2000 e i byte rimanenti dall'offset 5000 alla fine del file. |

## <a name="remarks"></a>Note

- Il token **EOF** è un valore di lunghezza valido all'interno delle coppie di offset e lunghezza nell' *intervallo di\<>* . Indica al servizio di leggere fino alla fine del file specificato.

- AddFileWithRanges avrà esito negativo con codice di errore 0x8020002c, quando viene specificato un intervallo di lunghezza zero insieme a un altro intervallo con lo stesso offset, ad esempio:

    `C:\bits>bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:0,100:5`

    **Messaggio di errore:** Non è possibile aggiungere il file al processo 0x8020002c. L'elenco di intervalli di byte contiene alcuni intervalli sovrapposti, che non sono supportati.

    **Soluzione alternativa:** Non specificare prima l'intervallo di lunghezza zero. Ad esempio, usare: 

    `bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:5,100:0.`

## <a name="examples"></a>Esempi

L'esempio seguente indica a BITS di trasferire 100 byte dall'offset 0, 100 byte dall'offset 2000 e i byte rimanenti dall'offset 5000 alla fine del file.

```
C:\>bitsadmin /addfilewithranges http://downloadsrv/10mb.zip c:\10mb.zip 0:100,2000:100,5000:eof
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)