---
title: Limiti di scalabilità di server di destinazione iSCSI
TOCTitle: iSCSI Target Server Scalability Limits
ms.prod: windows-server
ms.technology: storage-iscsi
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: d92ed347288bc9a0dd3893148a31152ae8b8a313
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393985"
---
# <a name="iscsi-target-server-scalability-limits"></a>Limiti di scalabilità di server di destinazione iSCSI

Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento fornisce i limiti dei server di destinazione iSCSI Microsoft supportati e testati in Windows Server. Nelle tabelle seguenti vengono visualizzati i limiti di supporto testati e, se applicabile, i limiti vengono applicati.

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
<th><p>Elemento</p></th>
<th><p>Limite supporto</p></th>
<th><p>Applicati?</p></th>
<th><p>Commento</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>istanze di destinazione iSCSI per server di destinazione iSCSI</p></td>
<td><p>256</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>unità logiche iSCSI (unità logiche) o dischi virtuali per server di destinazione iSCSI</p></td>
<td><p>512</p></td>
<td><p>No</p></td>
<td><p>Test delle configurazioni incluse: 8 unità logiche per istanza di destinazione con una media di oltre 64 destinazioni e 256 istanze di destinazione con una LU per destinazione.</p></td>
</tr>
<tr class="odd">
<td><p>Unità logiche iSCSI o dischi virtuali per istanza di destinazione iSCSI</p></td>
<td><p>256 (128 su Windows Server 2012)</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Sessioni che possono connettersi contemporaneamente a un'istanza di destinazione iSCSI</p></td>
<td><p>544 (512 su Windows Server 2012)</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Snapshot per LU</p></td>
<td><p>512</p></td>
<td><p>Yes</p></td>
<td><p>È previsto un limite di 512 snapshot per ogni volume dell'applicazione iSCSI indipendente.</p></td>
</tr>
<tr class="even">
<td><p>Dischi virtuali o snapshot montati localmente per ogni appliance di archiviazione</p></td>
<td><p>32</p></td>
<td><p>Yes</p></td>
<td><p>I dischi&#39;virtuali montati localmente non offrono funzionalità specifiche per iSCSI e sono deprecati. per altre informazioni, vedere <a href="https://technet.microsoft.com/library/dn303411.aspx">funzionalità rimosse o deprecate in Windows Server 2012 R2</a>.</p></td>
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
<th><p>Elemento</p></th>
<th><p>Limite supporto</p></th>
<th><p>Applicati?</p></th>
<th><p>Commento</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Nodi del cluster di failover</p></td>
<td><p>8 (5 su Windows Server 2012)</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Più nodi cluster attivi</p></td>
<td><p>Supportato</p></td>
<td> 
<p>N/D</p></td>
<td><p>Ogni nodo attivo nel cluster di failover è proprietario di un'istanza cluster di server di destinazione iSCSI diversa con altri nodi che fungono da possibili nodi del proprietario.</p></td>
</tr>
<tr class="odd">
<td><p>Livello di recupero errori (elfi)</p></td>
<td><p>0</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Connessioni per sessione</p></td>
<td><p>1</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Sessioni che possono connettersi contemporaneamente a un'istanza di destinazione iSCSI</p></td>
<td><p>544 (512 su Windows Server 2012)</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Input/output a percorsi multipli (MPIO)</p></td>
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
<td><p>Conversione di un server di destinazione iSCSI autonomo in un server di destinazione iSCSI in cluster o viceversa</p></td>
<td><p>Non supportate</p></td>
<td><p>No</p></td>
<td><p>L'istanza di destinazione iSCSI e i dati di configurazione del disco virtuale, inclusi i metadati dello snapshot, vengono persi durante la conversione.</p></td>
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
<th><p>Elemento</p></th>
<th><p>Limite supporto</p></th>
<th><p>Applicati?</p></th>
<th><p>Commento</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Numero massimo di schede di rete attive</p></td>
<td><p>8</p></td>
<td><p>No</p></td>
<td><p>Si applica alle schede di rete dedicate al traffico iSCSI, anziché al numero totale di schede di rete nell'appliance.</p></td>
</tr>
<tr class="even">
<td><p>Portale (indirizzi IP) supportati</p></td>
<td><p>64</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Velocità della porta di rete</p></td>
<td><p>1 Gbps, 10 Gbps, 40Gbps, 56 Gbps (solo Windows Server 2012 R2 e versioni successive)</p></td>
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
<td><p>Usare invii di grandi dimensioni (segmentazione), checksum, moderazione interrupt e offload RSS</p></td>
</tr>
<tr class="odd">
<td><p>offload iSCSI</p></td>
<td><p>Non supportate</p></td>
<td><br/><p>N/D</p></td>
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

## <a name="iscsi-virtual-disk-limits"></a>limiti dei dischi virtuali iSCSI

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Elemento</p></th>
<th><p>Limite supporto</p></th>
<th><p>Applicati?</p></th>
<th><p>Commento</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Da un iniziatore iSCSI che converte il disco virtuale da un disco di base a un disco dinamico </p></td>
<td><p>Yes</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Formato del disco rigido virtuale</p></td>
<td><p>. vhdx (solo Windows Server 2012 R2 e versioni successive)</p>
<p>vhd</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Dimensioni minime del formato VHD</p></td>
<td><p>VHDX 3 MB</p>
<p>VHD 8 MB</p></td>
<td><p>Yes</p></td>
<td><p>Si applica a tutti i tipi VHD supportati: Parent, differenze e fixed.</p></td>
</tr>
<tr class="even">
<td><p>Dimensioni massime VHD padre</p></td>
<td><p>VHDX 64 TB</p>
<p>VHD 2 TB</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Dimensioni massime VHD fisse</p></td>
<td><p>VHDX 64 TB</p>
<p>VHD 16 TB</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Dimensioni massime VHD differenze</p></td>
<td><p>VHDX 64 TB</p>
<p>VHD 2 TB</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Formato fisso VHD</p></td>
<td><p>Supportato</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Formato differenze VHD</p></td>
<td><p>Supportato</p></td>
<td><p>No</p></td>
<td><p>Non è possibile considerare gli snapshot dei dischi virtuali iSCSI basati su VHD differenze.</p></td>
</tr>
<tr class="odd">
<td><p>Numero di dischi rigidi virtuali differenze per ogni VHD padre</p></td>
<td><p>256</p></td>
<td><p>No (Sì in Windows Server 2012)</p></td>
<td><p>Due livelli di profondità (file nipotis. vhdx) sono i valori massimi per i file VHDX; un livello di profondità (file con estensione VHD figlio) è il valore massimo per i file con estensione vhd.</p></td>
</tr>
<tr class="even">
<td><p>Formato dinamico VHD</p></td>
<td><p>VHDX Yes</p>
<p>VHD Sì (no in Windows Server 2012)</p></td>
<td><p>Yes</p></td>
<td><p>Annullare isn&#39;t supportato.</p></td>
</tr>
<tr class="odd">
<td><p>exFAT/FAT32/FAT (volume di hosting del disco rigido virtuale)</p></td>
<td><p>Non supportate</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>CSV v2</p></td>
<td><p>Non supportate</p></td>
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
<td><p>CFS non Microsoft</p></td>
<td><p>Non supportate</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Thin provisioning</p></td>
<td><p>No</p></td>
<td><p>N/D</p></td>
<td><p>I VHD dinamici sono supportati, ma annullare&#39;non è supportato.</p></td>
</tr>
<tr class="odd">
<td><p>Compattazione unità logica</p></td>
<td><p>Sì (solo Windows Server 2012 R2 e versioni successive)</p></td>
<td><p>N/D</p></td>
<td><p>Usare <a href="https://docs.microsoft.com/powershell/module/iscsitarget/resize-iscsivirtualdisk">Resize-iSCSIVirtualDisk</a> per compattare un lun.</p></td>
</tr>
<tr class="even">
<td><p>Clonazione di unità logiche</p></td>
<td><p>Non supportate</p></td>
<td><p>N/D</p></td>
<td><p>È possibile clonare rapidamente i dati del disco utilizzando dischi rigidi virtuali differenze.</p></td>
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
<th><p>Elemento</p></th>
<th><p>Limite supporto</p></th>
<th><p>Commento</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Creazione Snapshot</p></td>
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
<td><p>Non supportate</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Snapshot: conversione a completa</p></td>
<td><p>Non supportate</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Snapshot-rollback online</p></td>
<td><p>Non supportate</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Snapshot: conversione in scrittura</p></td>
<td><p>Non supportate</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Snapshot-Reindirizzamento</p></td>
<td><p>Non supportate</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Aggiunta di snapshot</p></td>
<td><p>Non supportate</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Montaggio locale</p></td>
<td><p>Supportato</p></td>
<td><p>I dischi virtuali iSCSI montati localmente sono deprecati. per altre informazioni, vedere <a href="https://technet.microsoft.com/library/dn303411.aspx">funzionalità rimosse o deprecate in Windows Server 2012 R2</a>. Gli snapshot del disco dinamico non possono essere montati localmente.</p></td>
</tr>
</tbody>
</table>

## <a name="iscsi-target-server-manageability-and-backup"></a>gestione e backup del server di destinazione iSCSI

Se si desidera creare copie shadow del volume (snapshot di file aperti VSS) di dati in dischi virtuali iSCSI da un server applicazioni o si desidera gestire i dischi virtuali iSCSI con un'app precedente (ad esempio il comando Diskraid) che richiede un hardware servizio dischi virtuali (VDS) , installare il provider di archiviazione di destinazione iSCSI nel server da cui si desidera creare uno snapshot o utilizzare un'app di gestione VDS.

Il provider di archiviazione di destinazione iSCSI è un servizio ruolo in Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012; è anche possibile scaricare e installare i [provider di archiviazione di destinazione iSCSI (VDS/VSS) per i server applicazioni](http://www.microsoft.com/download/details.aspx?id=34759) legacy nei sistemi operativi seguenti, purché il server di destinazione iSCSI sia in esecuzione in Windows Server 2012:

  - Windows Storage Server 2008 R2

  - Windows Server 2008 R2

  - Windows HPC Server 2008 R2

  - Windows HPC Server 2008

Si noti che se il server di destinazione iSCSI è ospitato da un server che esegue Windows Server 2012 R2 o versione successiva e si desidera utilizzare VSS o VDS da un server remoto, il server remoto deve eseguire anche la stessa versione di Windows Server e disporre del servizio ruolo provider di archiviazione destinazioni iSCSI e installato. Si noti inoltre che in tutte le versioni di Windows è necessario installare solo una versione del servizio ruolo provider di archiviazione destinazioni iSCSI.

Per ulteriori informazioni sul provider di archiviazione di destinazione iSCSI, vedere [provider di archiviazione di destinazione iSCSI (VDS/VSS)](http://blogs.technet.com/b/filecab/archive/2012/10/08/iscsi-target-storage-vds-vss-provider.aspx).

## <a name="tested-compatibility-with-iscsi-initiators"></a>Verifica della compatibilità con iniziatori iSCSI

Il software server di destinazione iSCSI è stato testato con i seguenti iniziatori iSCSI:

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
<td><p>Convalidato</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003</p></td>
<td><p>Convalidato</p></td>
<td><p>Convalidato</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>VMWare vSphere 5</p></td>
<td></td>
<td><p>Convalidato</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>VMWare ESXi 5,0</p></td>
<td><p>Convalidato</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>VMWare ESX 4,1</p></td>
<td><p>Convalidato</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>CentOS 6. x</p></td>
<td><p>Convalidato</p></td>
<td></td>
<td><p>È necessario disconnettere una sessione ed eseguire nuovamente l'accesso per rilevare un disco virtuale ridimensionato.</p></td>
</tr>
<tr class="even">
<td><p>Red Hat Enterprise Linux 6</p></td>
<td><p>Convalidato</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>RedHat Enterprise Linux 5 e 5</p></td>
<td><p>Convalidato</p></td>
<td><p>Convalidato</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>SUSE Linux Enterprise Server 10</p></td>
<td></td>
<td><p>Convalidato</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Oracle Solaris 11. x</p></td>
<td><p>Convalidato</p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

Sono stati anche testati i seguenti iniziatori iSCSI che eseguono un avvio senza dischi da dischi virtuali ospitati da server di destinazione iSCSI:

  - Windows Server 2012 R2

  - Windows Server 2012

  - NIC PCIe con iPXE

  - CD o disco USB con iPXE

## <a name="see-also"></a>Vedere anche

L'elenco seguente include risorse aggiuntive su Server di destinazione iSCSI e tecnologie correlate.

- [Panoramica dell'archiviazione a blocchi di destinazione iSCSI](iscsi-target-server.md)

- [Panoramica dell'avvio di destinazione iSCSI](iscsi-boot-overview.md)

- [Archiviazione in Windows Server](../storage.md)

