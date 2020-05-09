---
title: create partition primary
description: Argomento di riferimento per il comando create partition primary, che consente di creare una partizione primaria nel disco di base con lo stato attivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6d652d8e-3935-4a91-8ced-b17c0e7937be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ac1763d52d8caf90112fb371aeac5e6b501bd2ea
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993236"
---
# <a name="create-partition-primary"></a>create partition primary

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea una partizione primaria del disco di base con lo stato attivo. Dopo che la partizione è stata creata, lo stato attivo passa automaticamente alla nuova partizione.

> [!IMPORTANT]
> Per eseguire questa operazione, è necessario selezionare un disco di base. È necessario usare il comando [Seleziona disco](select-disk.md) per selezionare un disco di base e spostare lo stato attivo a esso.

## <a name="syntax"></a>Sintassi

```
create partition primary [size=<n>] [offset=<n>] [id={ <byte> | <guid> }] [align=<n>] [noerr]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| dimensioni =`<n>` | Specifica le dimensioni della partizione in megabyte (MB). Se si specifica alcuna dimensione, la partizione continua fino a quando è disponibile spazio nell'area corrente. |
| offset =`<n>` | Offset in kilobyte (KB), in cui viene creata la partizione. Se l'offset non è specificato, la partizione inizierà all'inizio dell'extent del disco più grande di dimensioni sufficienti. |
| Allinea =`<n>` | Consente di allineare tutti gli extent di partizione per il limite di allineamento più vicino. In genere utilizzata con le matrici numero RAID unità logica (LUN) hardware per migliorare le prestazioni. `<n>`numero di kilobyte (KB) dall'inizio del disco al limite di allineamento più vicino. |
| ID = { `<byte>  | <guid>` } | Specifica il tipo di partizione. Questo parametro è destinato esclusivamente all'uso OEM (Original Equipment Manufacturer). Con questo parametro è possibile specificare qualsiasi tipo di partizione o GUID. DiskPart non controlla la validità del tipo di partizione, tranne per assicurarsi che sia un byte in formato esadecimale o GUID. **Attenzione:** la creazione di partizioni con questo parametro può causare l'esito negativo o non essere in grado di avvio del computer. A meno che non si sia un OEM o un professionista IT esperto di dischi GPT, non creare partizioni in dischi GPT con questo parametro. Usare sempre il comando [Crea partizione EFI](create-partition-efi.md) per creare partizioni di sistema EFI, il comando [Crea partizione MSR](create-partition-msr.md) per creare partizioni riservate Microsoft e il comando [Crea partizione primaria](create-partition-primary.md)(senza il `id={ <byte>  | <guid>` parametro) per creare partizioni primarie nei dischi GPT.<p>**Per i dischi MBR (record di avvio principale)** è necessario specificare un tipo di partizione byte, in formato esadecimale, per la partizione. Se questo parametro non è specificato, il comando crea una partizione di `0x06`tipo, che specifica che un file System non è installato. Tra gli esempi sono inclusi:<ul><li>**Partizione dati LDM:** 0x42</li><li>**Partizione di ripristino:** 0x27</li><li>**Partizione OEM riconosciuta:** 0x12, 0X84, 0XDE, 0xFE, messaggi 0XA0</li></ul><p>**Per i dischi della tabella di partizione GUID (GPT)**, è possibile specificare un GUID del tipo di partizione per la partizione che si desidera creare. GUID riconosciuto includono:<ul><li>**Partizione di sistema EFI:** C12A7328-F81F-11D2-BA4B-00A0C93EC93B</li><li>**Partizione riservata Microsoft:** e3c9e316-0b5c-4db8-817d-f92df00215ae</li><li>**Partizione dati di base:** ebd0a0a2-b9e5-4433-87C0-68b6b72699c7</li><li>**Partizione di metadati LDM (disco dinamico):** 5808c8aa-7e8f-42e0-85d2-e1e90434cfb3</li><li>**Partizione dati LDM (disco dinamico):** af9b60a0-1431-4f62-bc68-3311714a69ad</li><li>**Partizione di ripristino:** de94bba4-06d1-4d40-a16a-bfd50179d6ac<p>Se questo parametro non è specificato per un disco GPT, il comando crea una partizione di dati di base.</li></ul> |
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza il parametro noerr, un errore causa DiskPart viene interrotto con un codice di errore. |

## <a name="examples"></a>Esempi

Per creare una partizione primaria di 1000 MB di dimensioni, digitare:

```
create partition primary size=1000
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [assegna comando](assign.md)

- [Crea comando](create.md)

- [select disk](select-disk.md)
