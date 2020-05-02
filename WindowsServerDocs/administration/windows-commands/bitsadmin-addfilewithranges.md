---
title: bitsadmin addfilewithranges
description: Argomento di riferimento per il comando Bitsadmin ADDFILEWITHRANGES, che aggiunge un file al processo specificato. BITS scarica gli intervalli specificati dal file remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: df0ce0bf-dff1-4a48-a16f-fd2f4d5f7189
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b878b4f48441808bf971c051397d3af9bd975fe
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718464"
---
# <a name="bitsadmin-addfilewithranges"></a>bitsadmin addfilewithranges

Aggiunge un file al processo specificato. BITS scarica gli intervalli specificati dal file remoto. Questa opzione è valida solo per i processi di download.

## <a name="syntax"></a>Sintassi

```
bitsadmin /addfilewithranges <job> <remoteURL> <localname> <rangelist>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| processo | Nome visualizzato o GUID del processo. |
| remoteURL | URL del file nel server. |
| localname | Nome del file nel computer locale. Deve contenere un percorso assoluto del file. |
| intervallo di valori | Elenco delimitato da virgole di coppie offset: lunghezza. Usare i due punti per separare il valore di offset dal valore length. Ad esempio, un valore `0:100,2000:100,5000:eof` indica a bits di trasferire 100 byte dall'offset 0, 100 byte dall'offset 2000 e i byte rimanenti dall'offset 5000 alla fine del file. |

## <a name="remarks"></a>Osservazioni

- Il token **EOF** è un valore di lunghezza valido all'interno delle coppie di offset e `<rangelist>`lunghezza in. Indica al servizio di leggere fino alla fine del file specificato.

- Il `addfilewithranges` comando avrà esito negativo con codice di errore 0x8020002c, se viene specificato un intervallo di lunghezza zero insieme a un altro intervallo con lo stesso offset, ad esempio:

    `c:\bits>bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:0,100:5`

    **Messaggio di errore:** Non è possibile aggiungere il file al processo 0x8020002c. L'elenco di intervalli di byte contiene alcuni intervalli sovrapposti, che non sono supportati.

    **Soluzione alternativa:** Non specificare prima l'intervallo di lunghezza zero. Ad esempio, usare `bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:5,100:0`

## <a name="examples"></a>Esempi

Per trasferire 100 byte dall'offset 0, 100 byte dall'offset 2000 e i byte rimanenti dall'offset 5000 alla fine del file:

```
bitsadmin /addfilewithranges http://downloadsrv/10mb.zip c:\10mb.zip 0:100,2000:100,5000:eof
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
