---
title: Limiti di scalabilità di Server di destinazione iSCSI
TOCTitle: iSCSI Target Server Scalability Limits
ms.prod: windows-server-threshold
ms.technology: storage-iscsi
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: 9514392da133911c900f68fc8f1be260b6c91138
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873032"
---
# <a name="iscsi-target-server-scalability-limits"></a>Limiti di scalabilità di Server di destinazione iSCSI

Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento fornisce limiti per i Server di destinazione iSCSI Microsoft testata e supportata in Windows Server. Nelle tabelle seguenti vengono visualizzati i limiti di supporto testati ed, eventualmente, se i limiti vengono applicati.

## <a name="general-limits"></a>Limiti generali

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Item</p></th>
<th><p>Limite di supporto</p></th>
<th><p>Applicate?</p></th>
<th><p>Commento</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>istanze di destinazione iSCSI per ogni Server di destinazione iSCSI</p></td>
<td><p>256</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>iSCSI unità logiche (lu) o i dischi virtuali per ogni Server di destinazione iSCSI</p></td>
<td><p>512</p></td>
<td><p>No</p></td>
<td><p>Testare le configurazioni incluse: 8 unità logiche per ogni istanza di destinazione con un Media destinazioni di oltre 64 e 256 istanze di destinazione con una unità LOGICA per ogni destinazione.</p></td>
</tr>
<tr class="odd">
<td><p>istanza di destinazione iSCSI unità logiche o dischi virtuali per iSCSI</p></td>
<td><p>256 (128 in Windows Server 2012)</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Sessioni in grado di connettersi contemporaneamente a un'istanza di destinazione iSCSI</p></td>
<td><p>544 (512 in Windows Server 2012)</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Snapshot per ogni unità LOGICA</p></td>
<td><p>512</p></td>
<td><p>Yes</p></td>
<td><p>È previsto un limite di 512 snapshot per volume di applicazione iSCSI indipendenti.</p></td>
</tr>
<tr class="even">
<td><p>I dischi virtuali montati in locale o snapshot per ogni dispositivo di archiviazione</p></td>
<td><p>32</p></td>
<td><p>Yes</p></td>
<td><p>I dischi virtuali montati in locale non offrono le funzionalità specifiche di iSCSI e vengono deprecati - per altre informazioni, vedere <a href="https://technet.microsoft.com/library/dn303411.aspx">Features Removed or Deprecated in Windows Server 2012 R2</a>.</p></td>
</tr>
</tbody>
</table>

## <a name="fault-tolerance-limits"></a>Limiti di tolleranza di errore

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Item</p></th>
<th><p>Limite di supporto</p></th>
<th><p>Applicate?</p></th>
<th><p>Commento</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Nodi del cluster di failover</p></td>
<td><p>8 (5 in Windows Server 2012)</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Più nodi del cluster attivo</p></td>
<td><p>Supportato</p></td>
<td> 
<p>N/D</p></td>
<td><p>Un'istanza cluster di Server di destinazione iSCSI diversi è proprietario ogni nodo attivo del cluster di failover con gli altri nodi che agisce come possibili nodi proprietari.</p></td>
</tr>
<tr class="odd">
<td><p>Livello ripristino errore (ERL)</p></td>
<td><p>0</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Connessioni per ogni sessione</p></td>
<td><p>1</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Sessioni in grado di connettersi contemporaneamente a un'istanza di destinazione iSCSI</p></td>
<td><p>544 (512 in Windows Server 2012)</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Input/Output a percorsi multipli (MPIO)</p></td>
<td><p>Supportato</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Percorsi MPIO</p></td>
<td><p>4</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Conversione di un Server di destinazione iSCSI autonomo in un cluster iSCSI Target Server o viceversa</p></td>
<td><p>Non supportato</p></td>
<td><p>No</p></td>
<td><p>L'istanza di destinazione iSCSI e un disco virtuale dati di configurazione, inclusi i metadati dello snapshot, vengono persi durante la conversione.</p></td>
</tr>
</tbody>
</table>

## <a name="network-limits"></a>Limiti di rete

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Item</p></th>
<th><p>Limite di supporto</p></th>
<th><p>Applicate?</p></th>
<th><p>Commento</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Numero massimo di schede di rete attive</p></td>
<td><p>8</p></td>
<td><p>No</p></td>
<td><p>Si applica alle schede di rete dedicate per il traffico iSCSI, anziché il numero totale di schede di rete nell'appliance.</p></td>
</tr>
<tr class="even">
<td><p>Portale (indirizzi IP) è supportato</p></td>
<td><p>64</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Velocità di porta di rete</p></td>
<td><p>1 Gbps, 10 Gbps, 40Gbps, 56 Gbps (Windows Server 2012 R2 e versioni successive solo)</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>IPv4</p></td>
<td><p>Supportato</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>IPv6</p></td>
<td><p>Supportato</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Offload TCP</p></td>
<td><p>Supportato</p></td>
<td><p>N/D</p></td>
<td><p>Sfruttare (segmentazione) invio di grandi dimensioni, checksum, regolazione di interrupt e l'offload RSS</p></td>
</tr>
<tr class="odd">
<td><p>offload di iSCSI</p></td>
<td><p>Non supportato</p></td>
<td>              
<p>N/D</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Frame jumbo</p></td>
<td><p>Supportato</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>IPSec</p></td>
<td><p>Supportato</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Offload CRC</p></td>
<td><p>Supportato</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
</tbody>
</table>

## <a name="iscsi-virtual-disk-limits"></a>limiti di disco virtuale iSCSI

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Item</p></th>
<th><p>Limite di supporto</p></th>
<th><p>Applicate?</p></th>
<th><p>Commento</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Da un iniziatore iSCSI conversione del disco virtuale da un disco di base a un disco dinamico </p></td>
<td><p>Yes</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Formato del disco rigido virtuale</p></td>
<td><p>vhdx (Windows Server 2012 R2 e versioni successive solo)</p>
<p>vhd</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Dimensioni minime formato disco rigido virtuale</p></td>
<td><p>.vhdx: 3 MB</p>
<p>.vhd: 8 MB</p></td>
<td><p>Yes</p></td>
<td><p>Si applica a tutti i tipi di disco rigido virtuale supportati: padre, differenziazione e risolto.</p></td>
</tr>
<tr class="even">
<td><p>Dimensioni massime dei dischi rigidi Virtuali padre</p></td>
<td><p>.vhdx: 64 TB</p>
<p>.vhd: 2 TB</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Dimensioni max disco rigido virtuale fisse</p></td>
<td><p>.vhdx: 64 TB</p>
<p>.vhd: 16 TB</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Dimensioni massime di disco rigido virtuale differenze</p></td>
<td><p>.vhdx: 64 TB</p>
<p>.vhd: 2 TB</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>File VHD con formato fissato</p></td>
<td><p>Supportato</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Formato di disco rigido virtuale differenze</p></td>
<td><p>Supportato</p></td>
<td><p>No</p></td>
<td><p>Impossibile creare snapshot dei dischi virtuali iSCSI basate su disco rigido virtuale differenze.</p></td>
</tr>
<tr class="odd">
<td><p>Numero di VHD differenze per ogni disco rigido virtuale padre</p></td>
<td><p>256</p></td>
<td><p>No (Sì in Windows Server 2012)</p></td>
<td><p>Due livelli di profondità (file con estensione vhdx nipoti) è il valore massimo per i file vhdx; un livello di profondità (file con estensione vhd figlio) è il numero massimo di file con estensione vhd.</p></td>
</tr>
<tr class="even">
<td><p>Formato di disco rigido virtuale dinamico</p></td>
<td><p>.vhdx: Yes</p>
<p>.vhd: Sì (No in Windows Server 2012)</p></td>
<td><p>Yes</p></td>
<td><p>Annullare il mapping non è supportato.</p></td>
</tr>
<tr class="odd">
<td><p>exFAT/FAT32/file system FAT (volume del disco rigido virtuale di hosting)</p></td>
<td><p>Non supportato</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>CSV v2</p></td>
<td><p>Non supportato</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>ReFS</p></td>
<td><p>Supportato</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>NTFS</p></td>
<td><p>Supportato</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Non-Microsoft CFS</p></td>
<td><p>Non supportato</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Thin provisioning</p></td>
<td><p>No</p></td>
<td><p>N/D</p></td>
<td><p>Sono supportati dischi rigidi virtuali dinamici, ma non è supportato l'annullamento del mapping.</p></td>
</tr>
<tr class="odd">
<td><p>Compattazione di unità logica</p></td>
<td><p>Sì (Windows Server 2012 R2 e versioni successive solo)</p></td>
<td><p>N/D</p></td>
<td><p>Uso <a href="https://docs.microsoft.com/powershell/module/iscsitarget/resize-iscsivirtualdisk">Resize-iSCSIVirtualDisk</a> per compattare un LUN.</p></td>
</tr>
<tr class="even">
<td><p>La clonazione di unità logica</p></td>
<td><p>Non supportato</p></td>
<td><p>N/D</p></td>
<td><p>È possibile clonare rapidamente i dati su disco usando dischi rigidi virtuali differenze.</p></td>
</tr>
</tbody>
</table>

## <a name="snapshot-limits"></a>Limiti di snapshot

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Item</p></th>
<th><p>Limite di supporto</p></th>
<th><p>Commento</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Creazione di snapshot</p></td>
<td><p>Supportato</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Ripristino di snapshot</p></td>
<td><p>Supportato</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Snapshot scrivibili</p></td>
<td><p>Non supportato</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Snapshot: convert a full</p></td>
<td><p>Non supportato</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Snapshot: ripristino online</p></td>
<td><p>Non supportato</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Per gli snapshot: convertire scrivibile</p></td>
<td><p>Non supportato</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Snapshot - reindirizzamento</p></td>
<td><p>Non supportato</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Snapshot - aggiunta</p></td>
<td><p>Non supportato</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Montaggio locale</p></td>
<td><p>Supportato</p></td>
<td><p>I dischi virtuali iSCSI montato localmente sono deprecati: per altre informazioni, vedere <a href="https://technet.microsoft.com/library/dn303411.aspx">Features Removed or Deprecated in Windows Server 2012 R2</a>. Gli snapshot dei dischi dinamici non possono essere montati localmente.</p></td>
</tr>
</tbody>
</table>

## <a name="iscsi-target-server-manageability-and-backup"></a>backup e la gestibilità di Server di destinazione iSCSI

Se si desidera creare volumi copie shadow (snapshot VSS open-file) dei dati nei dischi virtuali iSCSI da un server applicazioni o si desidera gestire i dischi virtuali iSCSI con un'app meno recente (ad esempio, il comando Diskraid) che richiede un hardware servizio dischi virtuali (VDS) provider, installare il Provider di archiviazione destinazioni iSCSI nel server da cui si desidera creare uno snapshot oppure usare un'app di gestione VDS.

Il Provider di archiviazione destinazioni iSCSI è un servizio ruolo in Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012; è anche possibile scaricare e installare [iSCSI i provider di archiviazione di destinazione (VDS/VSS) per i server applicazioni di livello inferiore](http://www.microsoft.com/download/details.aspx?id=34759) nei seguenti sistemi operativi fino a quando il Server di destinazione iSCSI è in esecuzione in Windows Server 2012:

  - Windows Storage Server 2008 R2

  - Windows Server 2008 R2

  - Windows HPC Server 2008 R2

  - Windows HPC Server 2008

Si noti che se il Server di destinazione iSCSI è ospitato da un server che esegue Windows Server 2012 R2 o versione successiva e si desidera utilizzare VSS o VDS da un server remoto, il server remoto ha anche eseguire la stessa versione di Windows Server e disporre del ruolo di Provider di archiviazione destinazioni iSCSI servic e installato. Si noti inoltre che tutte le versioni di Windows è necessario installare solo una versione del servizio ruolo Provider di archiviazione destinazioni iSCSI.

Per altre informazioni sui Provider di archiviazione destinazioni iSCSI, vedere [Provider di archiviazione di destinazione (VDS/VSS) iSCSI](http://blogs.technet.com/b/filecab/archive/2012/10/08/iscsi-target-storage-vds-vss-provider.aspx).

## <a name="tested-compatibility-with-iscsi-initiators"></a>Testata la compatibilità con gli iniziatori iSCSI

È stato testato il software Server di destinazione con gli iniziatori iSCSI seguente iSCSI:

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Iniziatore</p></td>
<td><p>Windows Server 2012 R2</p></td>
<td><p>Windows Server 2012</p></td>
<td><p>Commenti</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2012 R2</p></td>
<td><p>Convalidare</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003</p></td>
<td><p>Convalidare</p></td>
<td><p>Convalidare</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>VMWare vSphere 5</p></td>
<td></td>
<td><p>Convalidare</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>VMWare ESXi 5.0</p></td>
<td><p>Convalidare</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>VMWare ESX 4.1</p></td>
<td><p>Convalidare</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>CentOS 6.x</p></td>
<td><p>Convalidare</p></td>
<td></td>
<td><p>È necessario disconnettere una sessione e accedere di nuovo di rilevare un disco virtuale ridimensionato.</p></td>
</tr>
<tr class="even">
<td><p>Red Hat Enterprise Linux 6</p></td>
<td><p>Convalidare</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>RedHat Enterprise Linux 5 and 5</p></td>
<td><p>Convalidare</p></td>
<td><p>Convalidare</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>SUSE Linux Enterprise Server 10</p></td>
<td></td>
<td><p>Convalidare</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Oracle Solaris 11.x</p></td>
<td><p>Convalidare</p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

È stato testato anche gli iniziatori iSCSI seguente esegue un avvio senza dischi da dischi virtuali ospitati dal Server di destinazione iSCSI:

  - Windows Server 2012 R2

  - Windows Server 2012

  - PCIe NIC con iPXE

  - CD o un disco USB con iPXE

## <a name="see-also"></a>Vedere anche

L'elenco seguente include risorse aggiuntive su Server di destinazione iSCSI e tecnologie correlate.

  - [Panoramica di archiviazione blocchi di destinazione iSCSI](iscsi-target-server.md)

  - [Panoramica di avvio di destinazioni iSCSI](iscsi-boot-overview.md)

  - [Archiviazione in Windows Server](..\storage.md)

