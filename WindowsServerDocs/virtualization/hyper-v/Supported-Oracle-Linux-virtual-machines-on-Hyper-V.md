---
title: Macchine virtuali Oracle Linux supportate in Hyper-V
description: Elenca i servizi di integrazione Linux e le funzionalità incluse in ogni versione
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: c02fdb5b-62f3-43cb-a190-ab74b3ebcf77
author: shirgall
ms.author: kathydav
ms.date: 06/05/2020
ms.openlocfilehash: 67f38d11c032e9eb0b98da14c25e01a5f67cabae
ms.sourcegitcommit: 76a3b5f66e47e08e8235e2d152185b304d03b68b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/10/2020
ms.locfileid: "84663174"
---
# <a name="supported-oracle-linux-virtual-machines-on-hyper-v"></a>Macchine virtuali Oracle Linux supportate in Hyper-V

>Si applica a: Windows Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows 10, Windows 8.1

La mappa di distribuzione di funzionalità seguente indica le funzionalità presenti in ogni versione. Dopo la tabella sono elencate i problemi noti e soluzioni alternative per ogni distribuzione.

Contenuto della sezione:

* [Serie Oracle Linux 8. x](#oracle-linux-8x-series)
* [Serie Oracle Linux 7. x](#oracle-linux-7x-series)
* [Oracle Linux serie 6. x](#oracle-linux-6x-series)
 
   
## <a name="table-legend"></a>Legenda tabella

* **Incorporata** -LIS sono inclusi come parte di questa distribuzione Linux. I numeri di versione del modulo del kernel per incorporato LIS (come illustrato da **lsmod**, ad esempio) sono diversi dal numero di versione del pacchetto di download LIS fornita da Microsoft. Una mancata corrispondenza non indica che incorporato LIS è scaduto.

* & #10004; -Funzionalità disponibili
* (*vuoto*)-funzionalità non disponibile
* **RHCK** -kernel compatibile con Red Hat
* **UEK** -Unbreakable Enterprise kernel (UEK) 
   * UEK4-basato sulla versione kernel Linux upstream 4.1.12
   * UEK5-basato sulla versione kernel Linux upstream 4,14
   * UEK6-basato sulla versione kernel Linux upstream 5,4

## <a name="oracle-linux-8x-series"></a>Serie Oracle Linux 8. x

|       **Funzionalità**     |       **Versione di Windows Server**      |       **8.0-8.1 (RHCK)** |
|-----------------------|---------------------------------------|-------------------|
|       **Disponibilità**        |   |
|       **[Base](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**      | 2019, 2016, 2012 R2 | &#10004; | 
|       Ora esatta di Windows Server 2016       | 2019, 2016 | &#10004; | 
|       **[Rete](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**      |   | 
|       Frame jumbo        | 2019, 2016, 2012 R2 | &#10004; | 
|       Assegnazione di tag e trunking VLAN       | 2019, 2016, 2012 R2 | &#10004;  | 
|       Migrazione in tempo reale      | 2019, 2016, 2012 R2 | &#10004; |
|       Inserimento IP statico     |  2019, 2016, 2012 R2 | & #10004; Nota 2 | 
|       RSS virtuale     | 2019, 2016, 2012 R2 | &#10004; |
|       Offload di segmentazione e checksum TCP | 2019, 2016, 2012 R2 | &#10004;|
|       SR-IOV  | 2019, 2016 |  &#10004;   |
|       **[Archiviazione](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)** |  | 
|       Ridimensionamento VHDX  | 2019, 2016, 2012 R2 | &#10004; |
|       Fibre Channel virtuale | 2019, 2016, 2012 R2 | & #10004; Nota 3  |
|       Backup della macchina virtuale in tempo reale  | 2019, 2016, 2012 R2 | & #10004; Nota 5 |
|       Supporto TRIM | 2019, 2016, 2012 R2 | &#10004;  |
|       WWN SCSI | 2019, 2016, 2012 R2 | &#10004;  |
|       **[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)** | |
|       Supporto del kernel PAE  | 2019, 2016, 2012 R2 |  N/D |
|       Configurazione di MMIO Gap  | 2019, 2016, 2012 R2 | &#10004; | 
|       Memoria dinamica - aggiunta a caldo | 2019, 2016, 2012 R2  | & #10004; Si noti 7, 8, 9 |
|       Memoria dinamica - Ballooning | 2019, 2016, 2012 R2 | & #10004; Si noti 7, 8, 9 |
|       Ridimensionamento della memoria di runtime | 2019, 2016  | &#10004;  |
|       **[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)** | |
|       Dispositivo video specifico di Hyper-V | 2019, 2016, 2012 R2 | &#10004;   | 
|       **[Varie](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)** | |
|       Coppia chiave-valore  | 2019, 2016, 2012 R2 | &#10004;   | 
|       Interrupt non mascherabile | 2019, 2016, 2012 R2 | &#10004;  | 
|       Copia di file da host a Guest | 2019, 2016, 2012 R2 | &#10004;  | 
|       comando lsvmbus | 2019, 2016, 2012 R2 | &#10004;  | 
|       Socket di Hyper-V | 2019, 2016 | &#10004;  | 
|       Pass-through/DDA PCI | 2019, 2016 | &#10004; | 
| **[Macchine virtuali di seconda generazione](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** | |  |
|       Avvio tramite UEFI | 2019, 2016, 2012 R2 |  & #10004; Nota 12  |   
|       Avvio protetto | 2019, 2016 |  &#10004; | 

## <a name="oracle-linux-7x-series"></a>Serie Oracle Linux 7. x

Questa serie dispone solo di kernel a 64 bit.

<table width="100%">
<tr height="50px">
<td width="20%" rowspan="2">

Funzionalità
</td>
<td width="20%" rowspan="2">

Versione di Windows Server
</td>
<td width="30%" colspan="3">

7.5-7,8
</td>
<td width="30%" colspan="3">

7.3-7.4
</td>
</tr>
<tr>
<td width="20%" colspan="2">

RHCK
</td>
<td width="10%">

UEK5
</td>
<td width="20%" colspan="2">

RHCK
</td>
<td width="10%">

UEK4
</td>
</tr>
<tr>
<td width="20%">

Disponibilità
</td>
<td width="20%">


</td>
<td width="10%">

LIS 4,3
</td>
<td width="10%">

Integrato
</td>
<td width="10%">

Integrato
</td>
<td width="10%">

LIS 4,3
</td>
<td width="10%">

Integrato
</td>
<td width="10%">

Integrato
</td>
</tr>
<tr height="50px">
<td width="20%">

**[Base](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Ora esatta di Windows Server 2016
</td>
<td width="20%">

2019, 2016
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

 **[Rete](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)** 
</td>
<td width="20%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

Frame jumbo
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">
Assegnazione di tag e trunking VLAN
</td>
<td width="20%">
2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Migrazione in tempo reale
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Inserimento IP statico
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

& #10004; Nota 2
</td>
<td width="10%">

& #10004; Nota 2
</td>
<td width="10%">

& #10004; Nota 2
</td>
<td width="10%">

& #10004; Nota 2
</td>
<td width="10%">

& #10004; Nota 2
</td>
<td width="10%">

& #10004; Nota 2
</td>
</tr>
<tr height="50px">
<td width="20%">

RSS virtuale
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Offload di segmentazione e checksum TCP
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

SR-IOV
</td>
<td width="20%">

2019, 2016
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

**[Archiviazione](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**
</td>
<td width="20%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

Ridimensionamento VHDX
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Fibre Channel virtuale
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

& #10004; Nota 3
</td>
<td width="10%">

& #10004; Nota 3
</td>
<td width="10%">

& #10004; Nota 3
</td>
<td width="10%">

& #10004; Nota 3
</td>
<td width="10%">

& #10004; Nota 3
</td>
<td width="10%">

& #10004; Nota 3
</td>
</tr>
<tr height="50px">
<td width="20%">

Backup della macchina virtuale in tempo reale
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

& #10004; Nota 5
</td>
<td width="10%">

& #10004; Nota 4,5
</td>
<td width="10%">

& #10004; Nota 5
</td>
<td width="10%">

& #10004; Nota 5
</td>
<td width="10%">

& #10004; Nota 4,5
</td>
<td width="10%">

& #10004; Nota 5
</td>
</tr>
<tr height="50px">
<td width="20%">

Supporto TRIM
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

WWN SCSI
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

**[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**
</td>
<td width="20%">


</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

Supporto del kernel PAE
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

N/D
</td>
<td width="10%">

N/D
</td>
<td width="10%">

N/D
</td>
<td width="10%">

N/D
</td>
<td width="10%">

N/D
</td>
<td width="10%">

N/D
</td>
</tr>
<tr height="50px">
<td width="20%">

Configurazione di MMIO Gap
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Aggiunta a caldo memoria dinamica
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004; nota 7, 8, 9
</td>
<td width="10%">

&#10004; nota 8, 9
</td>
<td width="10%">

&#10004; nota 8, 9
</td>
<td width="10%">

&#10004; nota 8, 9
</td>
<td width="10%">

&#10004; nota 8, 9
</td>
<td width="10%">

&#10004; nota 8, 9
</td>
</tr>
<tr height="50px">
<td width="20%">

memoria dinamica Balloon
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004; nota 7, 8, 9
</td>
<td width="10%">

&#10004; nota 8, 9
</td>
<td width="10%">

&#10004; nota 8, 9
</td>
<td width="10%">

&#10004; nota 8, 9
</td>
<td width="10%">

&#10004; nota 8, 9
</td>
<td width="10%">

&#10004; nota 8, 9
</td>
</tr>
<tr height="50px">
<td width="20%">

Ridimensionamento della memoria di runtime
</td>
<td width="20%">

2019, 2016
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**
</td>
<td width="20%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

Video specifico di Hyper-V
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

**[Varie](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**
</td>
<td width="20%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

Coppia chiave-valore
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Interrupt non mascherabile
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Copia di file da host a Guest
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

comando lsvmbus
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Socket di Hyper-V
</td>
<td width="20%">

2019, 2016
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Pass-through/DDA PCI
</td>
<td width="20%">

2019, 2016
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

**[Macchine virtuali di seconda generazione](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**
</td>
<td width="20%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

Avvio tramite UEFI
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

& #10004; Nota 12
</td>
<td width="10%">

& #10004; Nota 12
</td>
<td width="10%">

& #10004; Nota 12
</td>
<td width="10%">

& #10004; Nota 12
</td>
<td width="10%">

& #10004; Nota 12
</td>
<td width="10%">

& #10004; Nota 12
</td>
</tr>
<tr height="50px">
<td width="20%">

Avvio protetto
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
</table>


## <a name="oracle-linux-6x-series"></a>Oracle Linux serie 6. x

Questa serie dispone solo di kernel a 64 bit.

|       **Funzionalità**     |       **Versione di Windows Server**      |       **6.8-6.10 (RHCK)** |       **6.8-6.10 (UEK4)**     | 
|-----------------------|---------------------------------------|-------------------|-------------------|
|       **Disponibilità**     |   | LIS 4,3  | Integrato  |
|       **[Base](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**      | 2019, 2016, 2012 R2 | &#10004; | &#10004;
|       Ora esatta di Windows Server 2016       | 2019, 2016 | | 
|       **[Rete](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**      |   |  |
|       Frame jumbo        | 2019, 2016, 2012 R2 | &#10004; | &#10004;|
|       Assegnazione di tag e trunking VLAN       | 2019, 2016, 2012 R2 | & #10004; Nota 1 | & #10004; Nota 1 |
|       Migrazione in tempo reale      | 2019, 2016, 2012 R2 | &#10004; | &#10004;|
|       Inserimento IP statico     |  2019, 2016, 2012 R2 | & #10004; Nota 2 | &#10004;|
|       RSS virtuale     | 2019, 2016, 2012 R2 | &#10004; | &#10004;|
|       Offload di segmentazione e checksum TCP | 2019, 2016, 2012 R2 | &#10004;|  &#10004; |
|       SR-IOV  | 2019, 2016 |    |  |
|       **[Archiviazione](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)** |  |  |
|       Ridimensionamento VHDX  | 2019, 2016, 2012 R2 | &#10004; | &#10004; |
|       Fibre Channel virtuale | 2019, 2016, 2012 R2 | & #10004; Nota 3  | & #10004; Nota 3 |
|       Backup della macchina virtuale in tempo reale  | 2019, 2016, 2012 R2 | & #10004; Nota 5 | & #10004; Nota 5|
|       Supporto TRIM | 2019, 2016, 2012 R2 | &#10004;  | &#10004; |
|       WWN SCSI | 2019, 2016, 2012 R2 | &#10004;  | &#10004; |
|       **[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)** | |  |
|       Supporto del kernel PAE  | 2019, 2016, 2012 R2 |  N/D | N/D
|       Configurazione di MMIO Gap  | 2019, 2016, 2012 R2 | &#10004; | &#10004;  |
|       Memoria dinamica - aggiunta a caldo | 2019, 2016, 2012 R2  | & #10004; Nota 6, 8, 9 | & #10004; Nota 6, 8, 9 |
|       Memoria dinamica - Ballooning | 2019, 2016, 2012 R2 | & #10004; Nota 6, 8, 9 | & #10004; Nota 6, 8, 9 |
|       Ridimensionamento della memoria di runtime | 2019, 2016  |  | |
|       **[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)** | | |
|       Dispositivo video specifico di Hyper-V | 2019, 2016, 2012 R2 | &#10004;   | &#10004; |
|       **[Varie](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)** | | |
|       Coppia chiave-valore  | 2019, 2016, 2012 R2 | Nota &#10004; 10, 11   | Nota &#10004; 10, 11  |
|       Interrupt non mascherabile | 2019, 2016, 2012 R2 | &#10004;  | &#10004; |
|       Copia di file da host a Guest | 2019, 2016, 2012 R2 | &#10004;  | &#10004; |
|       comando lsvmbus | 2019, 2016, 2012 R2 | &#10004;  | &#10004; |
|       Socket di Hyper-V | 2019, 2016 | &#10004;  | &#10004; |
|       Pass-through/DDA PCI | 2019, 2016 | &#10004; | &#10004; |
| **[Macchine virtuali di seconda generazione](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** | |  |
|       Avvio tramite UEFI | 2019, 2016, 2012 R2 |  & #10004; Nota 12  | & #10004; Nota 12   
|       Avvio protetto | 2019, 2016 |  |  |



## <a name="notes"></a><a name="BKMK_notes"></a>Note

1. Per questa versione di Oracle Linux, VLAN tag funziona ma trunking VLAN non sono disponibili.

2. Inserimento di IP statico potrebbe non funzionare se Gestione di rete è stata configurata per una scheda di rete sintetiche sulla macchina virtuale. Per garantire il corretto funzionamento di IP statico injection assicurarsi che l'amministratore di sistema è spento completamente o è stato disattivato per una scheda di rete specifico tramite il file ifcfg-ethX.

3.  In Windows Server 2012 R2 quando si usano dispositivi Fibre Channel virtuali, verificare che sia stato popolato il numero di unità logica 0 (LUN 0). Se non è stato popolato LUN 0, una macchina virtuale potrebbe non essere in grado di installare dispositivi di fibre channel in modo nativo.

4. Per la LIS predefinita, per questa funzionalità è necessario installare il pacchetto "HyperV-daemons".

5.  Se sono presenti handle di file aperti durante un'operazione di backup di macchina virtuale, quindi in alcuni casi estremi, i dischi rigidi virtuali backup potrebbero essere necessario essere sottoposto a un controllo di coerenza di sistema di file (fsck) su ripristino. Operazioni di backup Live possano un esito negativo se la macchina virtuale dispone di un dispositivo iSCSI collegati o DAS (noto anche come disco pass-through).

6. Supporto della memoria dinamica è disponibile solo nelle macchine virtuali a 64 bit.

7. Aggiunta a caldo Supporto non è abilitato per impostazione predefinita in questa distribuzione. Per abilitare il supporto aggiunto a caldo che è necessario aggiungere una regola di udev in /etc/udev/rules.d/ come indicato di seguito:

   1. Creare un file **/etc/udev/rules.d/100-balloon.rules**. È possibile utilizzare qualsiasi altro nome per il file desiderato.

   2. Aggiungere il contenuto seguente al file:`SUBSYSTEM=="memory", ACTION=="add", ATTR{state}="online"`

   3. Riavviare il sistema per consentire l'aggiunta a caldo.

   Durante il download di Linux Integration Services crea questa regola su installazione, la regola è rimossi anche quando LIS viene disinstallato, la regola deve essere ricreato se è necessaria una quantità di memoria dinamica dopo la disinstallazione.

8. Operazioni di memoria dinamica possono non riuscire se il sistema operativo guest è troppo memoria insufficiente. Di seguito sono le procedure consigliate:

   * Memoria di avvio e di memoria minimo deve essere uguale o maggiore rispetto alla quantità di memoria in cui si consiglia il fornitore.

   * Le applicazioni che tendono a consumare l'intera memoria disponibile in un sistema sono limitate all'utilizzo di fino a 80% della RAM disponibile.

9. Se si utilizza la memoria dinamica in un sistema operativo Windows Server 2012 R2 o Windows Server 2016, specificare **memoria di avvio**, **memoria minima**, e **quantità massima di memoria** parametri in multipli di 128 megabyte (MB). In caso contrario, può comportare errori a caldo e non sarà possibile visualizzare qualsiasi memoria aumenta in un sistema operativo guest.

10. Per abilitare l'infrastruttura delle coppie chiave/valore (KVP), installare il pacchetto RPM hypervkvpd o HyperV-daemons dalla Oracle Linux ISO. In alternativa, il pacchetto può essere installato direttamente da Oracle Linux i repository yum.

11. L'infrastruttura (coppia chiave-VALORE) di coppia chiave/valore potrebbe non funzionare correttamente senza un aggiornamento software Linux. Contattare il fornitore di distribuzione per ottenere l'aggiornamento software nel caso in cui noterete problemi con questa funzionalità.

12. Nelle macchine virtuali Windows Server 2012 R2 di seconda generazione l'avvio protetto è abilitato per impostazione predefinita e alcune macchine virtuali Linux non vengono avviate a meno che l'opzione di avvio protetto non sia disabilitata. È possibile disabilitare l'avvio protetto nella sezione **firmware** delle impostazioni per la macchina virtuale nella console di **gestione di Hyper-V** oppure è possibile disabilitarlo con PowerShell:

    ```Powershell
    Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off
    ```

    Il download di Linux Integration Services può essere applicato alle macchine virtuali di 2 generazione esistente ma non applicare la funzionalità di generazione 2.


Vedere anche

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [CentOS è supportato e Red Hat Enterprise Linux macchine virtuali in Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Debian supportate in Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali SUSE supportate in Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Ubuntu supportate in Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali FreeBSD supportate in Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descrizioni delle funzionalità per le macchine virtuali Linux e FreeBSD in Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Procedure consigliate per l'esecuzione di Linux in Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)
