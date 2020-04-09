---
title: Controlli delle risorse della macchina virtuale
description: Uso di Gruppi CPU della macchina virtuale
author: allenma
ms.date: 06/18/2018
ms.topic: article
ms.prod: windows-server
ms.service: windows-10-hyperv
ms.assetid: cc7bb88e-ae75-4a54-9fb4-fc7c14964d67
ms.openlocfilehash: fcf61c22a24abb6b16baf75b4846cc188dcecd49
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860794"
---
>Si applica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019 Microsoft Hyper-V Server 2019

# <a name="virtual-machine-resource-controls"></a>Controlli delle risorse della macchina virtuale

Questo articolo descrive i controlli delle risorse e dell'isolamento di Hyper-V per le macchine virtuali.  Queste funzionalità, che verranno usate come gruppi di CPU delle macchine virtuali o semplicemente "gruppi di CPU", sono state introdotte in Windows Server 2016.  I gruppi di CPU consentono agli amministratori di Hyper-V di gestire e allocare in modo più efficiente le risorse della CPU dell'host tra macchine virtuali guest.  Utilizzando gruppi di CPU, gli amministratori di Hyper-V possono:

* Creare gruppi di macchine virtuali, in cui ogni gruppo presenta allocazioni diverse delle risorse totali della CPU dell'host di virtualizzazione, condivise nell'intero gruppo. Questo consente all'amministratore host di implementare classi di servizio per diversi tipi di macchine virtuali.

* Impostare i limiti delle risorse della CPU su gruppi specifici. Questo "gruppo Cap" imposta il limite superiore per le risorse della CPU dell'host che l'intero gruppo può utilizzare, applicando in modo efficace la classe di servizio desiderata per quel gruppo.

* Vincolare un gruppo di CPU per l'esecuzione solo in un set specifico di processori del sistema host. Questa operazione può essere usata per isolare le macchine virtuali appartenenti a diversi gruppi di CPU.

## <a name="managing-cpu-groups"></a>Gestione dei gruppi di CPU

I gruppi di CPU vengono gestiti tramite il servizio di calcolo host Hyper-V o HCS. Un'ottima descrizione di HCS, i relativi Genesis, i collegamenti alle API HCS e altro ancora sono disponibili nel Blog del team di virtualizzazione Microsoft della pubblicazione che [introduce il servizio di calcolo host (HCS)](https://blogs.technet.microsoft.com/virtualization/2017/01/27/introducing-the-host-compute-service-hcs/).

>[!NOTE] 
>Solo HCS può essere usato per creare e gestire gruppi di CPU; le interfacce di gestione di Hyper-V, WMI e PowerShell non supportano i gruppi di CPU.

Microsoft fornisce un'utilità della riga di comando, cpugroups. exe, nell' [area download Microsoft](https://go.microsoft.com/fwlink/?linkid=865968) che usa l'interfaccia HCS per gestire i gruppi di CPU.  Questa utilità consente inoltre di visualizzare la topologia della CPU di un host.

## <a name="how-cpu-groups-work"></a>Funzionamento di gruppi CPU

L'allocazione delle risorse di calcolo host tra gruppi di CPU viene applicata dall'hypervisor Hyper-V, usando un limite del gruppo di CPU calcolato. Il limite del gruppo di CPU è una frazione della capacità totale della CPU per un gruppo di CPU. Il valore del limite del gruppo dipende dalla classe gruppo o dal livello di priorità assegnato. Il limite del gruppo calcolato può essere considerato come "un numero di LP che vale il tempo della CPU". Questo budget del gruppo è condiviso, quindi, se era attiva una sola macchina virtuale, poteva usare l'allocazione della CPU dell'intero gruppo.

Il limite del gruppo di CPU viene calcolato come G = *n* x *C*, dove:

    *G* is the amount of host LP we'd like to assign to the group
    *n* is the total number of logical processors (LPs) in the group
    *C* is the maximum CPU allocation — that is, the class of service desired for the group, expressed as a percentage of the system's total compute capacity

Si consideri, ad esempio, un gruppo di CPU configurato con 4 processori logici (LPs) e un limite del 50%.

    G = n * C
    G = 4 * 50%
    G = 2 LP's worth of CPU time for the entire group

In questo esempio, il gruppo di CPU G viene allocato per 2 LP di tempo di CPU.  

Si noti che il limite di gruppo si applica indipendentemente dal numero di macchine virtuali o processori virtuali associati al gruppo e indipendentemente dallo stato (ad esempio, Shutdown o Started) delle macchine virtuali assegnate al gruppo di CPU. Ogni macchina virtuale associata allo stesso gruppo di CPU riceverà pertanto una frazione dell'allocazione totale della CPU del gruppo, che cambierà con il numero di macchine virtuali associate al gruppo di CPU. Di conseguenza, poiché le macchine virtuali sono associate o non associate a un gruppo di CPU, è necessario rimodificare il limite complessivo della CPU e impostare in modo da mantenere il limite risultante per macchina virtuale desiderato. L'amministratore host VM o il livello software di gestione della virtualizzazione è responsabile della gestione dei limiti di gruppo in base alle esigenze per ottenere l'allocazione delle risorse della CPU per macchina virtuale desiderata.

## <a name="example-classes-of-service"></a>Classi di esempio di servizio

Esaminiamo alcuni semplici esempi. Per iniziare, si supponga che l'amministratore dell'host Hyper-V desideri supportare due livelli di servizio per le macchine virtuali guest:

1. Livello "C" di fascia bassa. Questo livello verrà fornito al 10% delle risorse di calcolo dell'intero host.

1. Livello "B" di intervallo intermedio. Questo livello è allocato al 50% delle risorse di calcolo dell'intero host.

A questo punto, nell'esempio si asserirà che non sono in uso altri controlli delle risorse della CPU, ad esempio singoli tappi delle macchine virtuali, pesi e riserve.
Tuttavia, i tappi delle singole macchine virtuali sono importanti, perché in seguito verrà visualizzato un po'.

Per semplicità, si supponga che ogni macchina virtuale abbia 1 VP e che l'host abbia 8 LPs. Si inizierà con un host vuoto.

Per creare il livello "B", l'host adminstartor imposta il limite del gruppo su 50%:

    G = n * C
    G = 8 * 50%
    G = 4 LP's worth of CPU time for the entire group

L'amministratore host aggiunge una singola macchina virtuale di livello "B".
A questo punto, la macchina virtuale di livello "B" può usare al massimo il 50% della CPU dell'host o l'equivalente di 4 LPs nel sistema di esempio.

A questo punto, l'amministratore aggiunge una seconda macchina virtuale "Tier B". L'allocazione del gruppo di CPU è divisa in modo uniforme tra tutte le macchine virtuali. Nel gruppo B sono presenti un totale di 2 macchine virtuali, quindi ogni macchina virtuale ora ottiene la metà del totale del gruppo B del 50%, il 25% ciascuno o l'equivalente di 2 LPs di tempo di calcolo.

## <a name="setting-cpu-caps-on-individual-vms"></a>Impostazione delle protezioni CPU sulle singole macchine virtuali

Oltre al limite di gruppo, ogni macchina virtuale può avere anche un singolo "Cap della macchina virtuale". I controlli delle risorse della CPU per macchina virtuale, inclusi un limite di CPU, un peso e una riserva, fanno parte di Hyper-V fin dall'introduzione.
In combinazione con un limite di gruppo, un limite di VM specifica la quantità massima di CPU che ciascun VP può ottenere, anche se nel gruppo sono disponibili risorse della CPU.

Ad esempio, l'amministratore host potrebbe voler inserire un limite di VM del 10% sulle macchine virtuali "C".
In questo modo, anche se la maggior parte del VPs "C" è inattiva, ogni VP non può mai ottenere più del 10%.
Senza un tappo della macchina virtuale, le macchine virtuali "C" potrebbero opportunisticamente ottenere prestazioni superiori ai livelli consentiti dal livello.

## <a name="isolating-vm-groups-to-specific-host-processors"></a>Isolamento di gruppi di macchine virtuali in processori host specifici

Gli amministratori di host Hyper-V possono anche voler dedicare le risorse di calcolo a una macchina virtuale.
Si supponga, ad esempio, che l'amministratore volesse offrire una macchina virtuale "A" Premium con un limite di classe pari al 100%.
Queste VM Premium richiedono anche la latenza di pianificazione più bassa e l'instabilità possibile; ovvero, non possono essere depianificati da altre macchine virtuali.
Per ottenere questa separazione, un gruppo di CPU può essere configurato anche con un mapping di affinità LP specifico.

Ad esempio, per adattare una macchina virtuale "A" nell'host in questo esempio, l'amministratore crea un nuovo gruppo di CPU e imposta l'affinità del processore del gruppo su un subset di LPs dell'host.
I gruppi B e C verrebbero creata un'affinità ai restanti LPs.
L'amministratore potrebbe creare una singola macchina virtuale nel gruppo A, che avrà quindi accesso esclusivo a tutti gli LPs del gruppo A, mentre i gruppi di livelli B e C che presumibilmente riducono gli LPs rimanenti.

## <a name="segregating-root-vps-from-guest-vps"></a>Separazione del VPs radice dal VPs Guest

Per impostazione predefinita, Hyper-V creerà un VP radice in ogni LP fisico sottostante.
Il VPs radice viene mappato in modo rigoroso a 1:1 con gli LPs del sistema e non viene eseguita la migrazione, ovvero ogni VP radice viene sempre eseguito sullo stesso LP fisico.
Il VPs Guest può essere eseguito su qualsiasi LP disponibile e condividerà l'esecuzione con il VPs radice.

Tuttavia, potrebbe essere utile separare completamente l'attività VP radice dal VPs Guest.
Si consideri l'esempio precedente in cui viene implementata una macchina virtuale di livello Premium "A".
Per assicurarsi che il VPs della macchina virtuale "A" disponga della latenza e della "jitter" più bassa possibile o della variazione di pianificazione, è opportuno eseguirli in un set di LPs dedicato e assicurarsi che la radice non venga eseguita su questi LPs.

Questa operazione può essere eseguita usando una combinazione della configurazione "minroot", che limita la partizione del sistema operativo host a essere in esecuzione in un subset dei processori logici di sistema totali, insieme a uno o più gruppi di CPU creata un'affinità.

L'host di virtualizzazione può essere configurato in modo da limitare la partizione host a LPs specifici, con uno o più gruppi di CPU creata un'affinità ai restanti LPs.
In questo modo, le partizioni radice e Guest possono essere eseguite su risorse della CPU dedicate e completamente isolate senza alcuna condivisione della CPU.

Per ulteriori informazioni sulla configurazione "minroot", vedere [gestione delle risorse della CPU dell'host Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-minroot-2016).

## <a name="using-the-cpugroups-tool"></a>Uso dello strumento CpuGroups

Esaminiamo alcuni esempi di come usare lo strumento CpuGroups.

>[!NOTE] 
>I parametri della riga di comando per lo strumento CpuGroups vengono passati usando solo spazi come delimitatori. Nessun carattere '/' o '-' deve continuare con l'opzione della riga di comando desiderata.

### <a name="discovering-the-cpu-topology"></a>Individuazione della topologia della CPU

L'esecuzione di CpuGroups con GetCpuTopology restituisce informazioni sul sistema corrente, come illustrato di seguito, tra cui l'indice LP, il nodo NUMA a cui appartiene il LP, il pacchetto e gli ID core e l'indice VP radice.

Nell'esempio seguente viene illustrato un sistema con 2 socket CPU e nodi NUMA, un totale di 32 LPs e un multithreading abilitato e configurato per abilitare Minroot con 8 VPs radice, 4 da ogni nodo NUMA.
Gli LPs con VPs radice hanno una RootVpIndex > = 0; Gli LPs con RootVpIndex di-1 non sono disponibili per la partizione radice, ma sono comunque gestiti dall'hypervisor e eseguono il VPs Guest come consentito da altre impostazioni di configurazione.

```console
C:\vm\tools>CpuGroups.exe GetCpuTopology

LpIndex NodeNumber PackageId CoreId RootVpIndex
------- ---------- --------- ------ -----------
      0          0         0      0           0
      1          0         0      0           1
      2          0         0      1           2
      3          0         0      1           3
      4          0         0      2          -1
      5          0         0      2          -1
      6          0         0      3          -1
      7          0         0      3          -1
      8          0         0      4          -1
      9          0         0      4          -1
     10          0         0      5          -1
     11          0         0      5          -1
     12          0         0      6          -1
     13          0         0      6          -1
     14          0         0      7          -1
     15          0         0      7          -1
     16          1         1     16           4
     17          1         1     16           5
     18          1         1     17           6
     19          1         1     17           7
     20          1         1     18          -1
     21          1         1     18          -1
     22          1         1     19          -1
     23          1         1     19          -1
     24          1         1     20          -1
     25          1         1     20          -1
     26          1         1     21          -1
     27          1         1     21          -1
     28          1         1     22          -1
     29          1         1     22          -1
     30          1         1     23          -1
     31          1         1     23          -1
```

### <a name="example-2--print-all-cpu-groups-on-the-host"></a>Esempio 2: stampare tutti i gruppi di CPU nell'host

Qui verranno elencati tutti i gruppi di CPU nell'host corrente, i relativi GroupId, il limite della CPU del gruppo e il indici di LPs assegnati a tale gruppo.

Si noti che i valori limite della CPU validi sono compresi nell'intervallo [0, 65536] e questi valori esprimono il limite del gruppo in percentuale (ad esempio, 32768 = 50%).

```console
C:\vm\tools>CpuGroups.exe GetGroups

CpuGroupId                          CpuCap  LpIndexes
------------------------------------ ------ --------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536  24,25,26,27,28,29,30,31
```

### <a name="example-3--print-a-single-cpu-group"></a>Esempio 3: stampare un singolo gruppo di CPU

In questo esempio verrà eseguita una query su un singolo gruppo di CPU usando GroupId come filtro.

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000003
CpuGroupId                          CpuCap   LpIndexes
------------------------------------ ------ ----------
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
```

### <a name="example-4--create-a-new-cpu-group"></a>Esempio 4: creare un nuovo gruppo di CPU

In questo caso verrà creato un nuovo gruppo di CPU, che specifica l'ID del gruppo e il set di LPs da assegnare al gruppo.

```console
C:\vm\tools>CpuGroups.exe CreateGroup /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /GroupAffinity:0,1,16,17
```

A questo punto, visualizzare il gruppo appena aggiunto.

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 65536 0,1,16,17
36AB08CB-3A76-4B38-992E-000000000002 32768 4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536 12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-5--set-the-cpu-group-cap-to-50"></a>Esempio 5: impostare il limite del gruppo di CPU su 50%

Impostiamo il limite del gruppo di CPU su 50%.

```console
C:\vm\tools>CpuGroups.exe SetGroupProperty /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /CpuCap:32768
```

A questo punto, è possibile confermare l'impostazione visualizzando il gruppo appena aggiornato.

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000001

CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 32768 0,1,16,17
```

### <a name="example-6--print-cpu-group-ids-for-all-vms-on-the-host"></a>Esempio 6: stampare gli ID dei gruppi di CPU per tutte le macchine virtuali nell'host

```console
C:\vm\tools>CpuGroups.exe GetVmGroup

VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G2 4ABCFC2F-6C22-498C-BB38-7151CE678758 36ab08cb-3a76-4b38-992e-000000000002
    P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC 36ab08cb-3a76-4b38-992e-000000000003
    P2 A593D93A-3A5F-48AB-8862-A4350E3459E8 36ab08cb-3a76-4b38-992e-000000000004
    G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200 36ab08cb-3a76-4b38-992e-000000000002
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 36ab08cb-3a76-4b38-992e-000000000002
```

### <a name="example-7--unbind-a-vm-from-the-cpu-group"></a>Esempio 7: separare una macchina virtuale dal gruppo di CPU

Per rimuovere una macchina virtuale da un gruppo di CPU, impostare su CpuGroupId della macchina virtuale sul GUID NULL. Questa operazione Annulla l'associazione della macchina virtuale dal gruppo di CPU.

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:00000000-0000-0000-0000-000000000000

C:\vm\tools>CpuGroups.exe GetVmGroup
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G2 4ABCFC2F-6C22-498C-BB38-7151CE678758 36ab08cb-3a76-4b38-992e-000000000002
    P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC 36ab08cb-3a76-4b38-992e-000000000003
    P2 A593D93A-3A5F-48AB-8862-A4350E3459E8 36ab08cb-3a76-4b38-992e-000000000004
    G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200 36ab08cb-3a76-4b38-992e-000000000002
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 00000000-0000-0000-0000-000000000000
```

### <a name="example-8--bind-a-vm-to-an-existing-cpu-group"></a>Esempio 8: associare una macchina virtuale a un gruppo di CPU esistente

In questo caso verrà aggiunta una macchina virtuale a un gruppo di CPU esistente.
Si noti che la macchina virtuale non deve essere associata a un gruppo di CPU esistente o l'impostazione dell'ID gruppo CPU avrà esito negativo.

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:36AB08CB-3A76-4B38-992E-000000000001
```

A questo punto, verificare che la macchina virtuale G1 si trovi nel gruppo CPU desiderato.

```console
C:\vm\tools>CpuGroups.exe GetVmGroup
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G2 4ABCFC2F-6C22-498C-BB38-7151CE678758 36ab08cb-3a76-4b38-992e-000000000002
    P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC 36ab08cb-3a76-4b38-992e-000000000003
    P2 A593D93A-3A5F-48AB-8862-A4350E3459E8 36ab08cb-3a76-4b38-992e-000000000004
    G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200 36ab08cb-3a76-4b38-992e-000000000002
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 36AB08CB-3A76-4B38-992E-000000000001
```

### <a name="example-9--print-all-vms-grouped-by-cpu-group-id"></a>Esempio 9: stampare tutte le macchine virtuali raggruppate in base all'ID gruppo CPU

```console
C:\vm\tools>CpuGroups.exe GetGroupVms
CpuGroupId                           VmName                                 VmId
------------------------------------ ------ ------------------------------------
36AB08CB-3A76-4B38-992E-000000000001     G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
36ab08cb-3a76-4b38-992e-000000000002     G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002     G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
36ab08cb-3a76-4b38-992e-000000000003     P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC
36ab08cb-3a76-4b38-992e-000000000004     P2 A593D93A-3A5F-48AB-8862-A4350E3459E8
```

### <a name="example-10--print-all-vms-for-a-single-cpu-group"></a>Esempio 10: stampare tutte le macchine virtuali per un singolo gruppo di CPU

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36ab08cb-3a76-4b38-992e-000000000002

CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36ab08cb-3a76-4b38-992e-000000000002     G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002     G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
```

### <a name="example-11--attempting-to-delete-a-non-empty-cpu-group"></a>Esempio 11: tentativo di eliminazione di un gruppo di CPU non vuoto

Solo i gruppi di CPU vuoti, ovvero i gruppi di CPU senza macchine virtuali associate, possono essere eliminati.
Il tentativo di eliminare un gruppo di CPU non vuoto avrà esito negativo.

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
(null)
Failed with error 0xc0350070
```

### <a name="example-12--unbind-the-only-vm-from-a-cpu-group-and-delete-the-group"></a>Esempio 12: separare l'unica VM da un gruppo di CPU ed eliminare il gruppo

In questo esempio verranno usati diversi comandi per esaminare un gruppo di CPU, rimuovere la singola macchina virtuale appartenente a tale gruppo, quindi eliminare il gruppo.

Prima di tutto, è necessario enumerare le macchine virtuali nel gruppo.

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36AB08CB-3A76-4B38-992E-000000000001
CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36AB08CB-3A76-4B38-992E-000000000001     G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
```

Si noterà che solo una singola VM, denominata G1, appartiene a questo gruppo.
Rimuovere la VM G1 dal gruppo impostando l'ID gruppo della macchina virtuale su NULL.

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:00000000-0000-0000-0000-000000000000
```

E verifica la modifica...

```console
C:\vm\tools>CpuGroups.exe GetVmGroup /VmName:g1
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 00000000-0000-0000-0000-000000000000
```

Ora che il gruppo è vuoto, è possibile eliminarlo in modo sicuro.

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
```

E verificare che il gruppo sia scomparso.

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap                     LpIndexes
------------------------------------ ------ -----------------------------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-13--bind-a-vm-back-to-its-original-cpu-group"></a>Esempio 13: associare una macchina virtuale al gruppo di CPU originale

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:36AB08CB-3A76-4B38-992E-000000000002

C:\vm\tools>CpuGroups.exe GetGroupVms
CpuGroupId VmName VmId
------------------------------------ -------------------------------- ------------------------------------
36ab08cb-3a76-4b38-992e-000000000002 G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002 G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
36AB08CB-3A76-4B38-992E-000000000002 G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
36ab08cb-3a76-4b38-992e-000000000003 P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC
36ab08cb-3a76-4b38-992e-000000000004 P2 A593D93A-3A5F-48AB-8862-A4350E3459E8
```
