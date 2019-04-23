---
title: Controlli delle risorse macchina virtuale
description: Uso di Gruppi CPU della macchina virtuale
keywords: windows 10, hyper-v
author: allenma
ms.date: 06/18/2018
ms.topic: article
ms.prod: windows-10-hyperv
ms.service: windows-10-hyperv
ms.assetid: cc7bb88e-ae75-4a54-9fb4-fc7c14964d67
ms.openlocfilehash: 7c4ddf3e5d2ff58eef844c50960327c27a3e0a3d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854762"
---
>Si applica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

# <a name="virtual-machine-resource-controls"></a>Controlli delle risorse macchina virtuale

Questo articolo descrive i controlli di risorse e l'isolamento Hyper-V per le macchine virtuali.  Queste funzionalità, che si farà riferimento a gruppi di CPU macchina virtuale, o semplicemente "gruppi di CPU", sono stati introdotti in Windows Server 2016.  Gruppi di CPU consentono agli amministratori di Hyper-V per una migliore gestione e allocano le risorse della CPU dell'host tra più macchine virtuali guest.  Utilizza gruppi di CPU, gli amministratori di Hyper-V possono:

* Creare gruppi di macchine virtuali, con ciascun gruppo può essere diversa delle allocazioni di risorse della CPU totale dell'host di virtualizzazione, condivise nell'intero gruppo. Ciò consente all'amministratore di host implementare le classi del servizio per diversi tipi di macchine virtuali.

* Impostare i limiti delle risorse di CPU a gruppi specifici. Questo "gruppo limite di utilizzo" imposta il limite superiore per host le risorse di CPU che potrebbe usare l'intero gruppo, l'applicazione in modo efficace la classe del servizio per il gruppo desiderata.

* Vincolare un gruppo di CPU per l'esecuzione solo su un set specifico di processori del sistema host. Ciò consente di isolare le macchine virtuali appartenenti a gruppi di CPU diversi tra loro.

## <a name="managing-cpu-groups"></a>Gestione dei gruppi di CPU

Gruppi di CPU vengono gestiti tramite il servizio di calcolo Hyper-V Host o HCS. Una descrizione del HCS, la creazione, i collegamenti per le API HCS e altro ancora sono disponibili nel blog del team Microsoft Virtualization sulla registrazione [Introducing Host Compute Service (HCS)](https://blogs.technet.microsoft.com/virtualization/2017/01/27/introducing-the-host-compute-service-hcs/).

>[!NOTE] 
>Solo il HCS può essere utilizzato per creare e gestire gruppi di CPU; l'applet di gestione di Hyper-V, le interfacce di gestione WMI e PowerShell non supportano i gruppi di CPU.

Microsoft fornisce una riga di comando utilità, cpugroups.exe, sul [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=865968) che usa l'interfaccia HCS per gestire i gruppi di CPU.  Questa utilità è inoltre possibile visualizzare la topologia della CPU di un host.

## <a name="how-cpu-groups-work"></a>La modalità di gruppi di lavoro della CPU

Allocazione delle risorse di calcolo di host in gruppi di CPU viene applicata dall'hypervisor Hyper-V, utilizzando un delimitatore di gruppo CPU calcolato. Il limite di utilizzo della CPU gruppo è una frazione della capacità totale della CPU per un gruppo di CPU. Il valore del limite di gruppo dipende dalla classe gruppo o assegnate a livello di priorità. Il limite di utilizzo di gruppo calcolata può essere considerato come "numero di LP's vale la pena di tempo della CPU". Il budget di gruppo è condivisa, pertanto se solo una singola VM erano attivi, è possibile utilizzare l'allocazione della CPU dell'intero gruppo per se stesso.

Il limite di utilizzo della CPU gruppo viene calcolata come G = *n* x *C*, dove:

    *G* is the amount of host LP we'd like to assign to the group
    *n* is the total number of logical processors (LPs) in the group
    *C* is the maximum CPU allocation — that is, the class of service desired for the group, expressed as a percentage of the system’s total compute capacity

Ad esempio, si consideri un gruppo di CPU configurato con 4 processori logici (LPs) e un limite del 50%.

    G = n * C
    G = 4 * 50%
    G = 2 LP's worth of CPU time for the entire group

In questo esempio, il gruppo di CPU G viene allocato 2 LP's vale la pena di tempo della CPU.  

Si noti che il limite di utilizzo di gruppo si applica indipendentemente dal numero di macchine virtuali o associati al gruppo di processori virtuali e indipendentemente dallo stato (ad esempio, arresto o avvio) delle macchine virtuali assegnate al gruppo di CPU. Pertanto, ogni macchina virtuale associata allo stesso gruppo di CPU riceveranno una frazione di allocazione della CPU complessivo del gruppo e verrà modificato con il numero di macchine virtuali associate al gruppo di CPU. Pertanto, quando le macchine virtuali sono associate o non associate le macchine virtuali da un gruppo di CPU, il limite di gruppo complessivo della CPU deve essere regolato e impostato per mantenere il limite di utilizzo per ogni VM risultante desiderato. La gestione della macchina virtuale host virtualizzazione o amministratore di livello di software è responsabile della gestione dei criteri di autorizzazione connessioni gruppo in base alle esigenze per ottenere l'allocazione delle risorse della CPU desiderate per ogni VM.

## <a name="example-classes-of-service"></a>Classi di esempio di servizio

Esaminiamo alcuni semplici esempi. Per iniziare, si supponga che l'amministratore di host Hyper-V da supportare due livelli di servizio per le macchine virtuali guest:

1. Un livello "C" low-end. Questo livello si assegnerà 10% delle risorse di calcolo dell'intero host.

1. Un livello di livello intermedio "B". Questo livello verrà allocato 50% delle risorse di calcolo dell'intero host.

A questo punto nel nostro esempio abbiamo sarà asserire che nessun altro controlli delle risorse della CPU sono in uso, ad esempio singoli limiti della macchina virtuale, i pesi e si riserva.
Tuttavia, singoli limiti della macchina virtuale sono importanti, come vedremo più avanti.

Per semplicità, si supponga che ogni macchina virtuale è 1, Vice presidente del settore e che l'host abbia 8 LPs. Si inizierà con un host vuoto.

Per creare il livello "B", l'host adminstartor imposta il limite di utilizzo di gruppo al 50%:

    G = n * C
    G = 8 * 50%
    G = 4 LP's worth of CPU time for the entire group

L'amministratore di host aggiunge un singolo livello "B" macchina virtuale.
A questo punto, il nostro livello "B" macchina virtuale può usare al massimo 50%, vale la pena di CPU dell'host o l'equivalente di 4 LPs nel nostro sistema di esempio.

A questo punto, l'amministratore aggiunge un secondo "livello B" macchina virtuale. Allocazione del gruppo della CPU, ovvero viene suddiviso equamente tra tutte le macchine virtuali. Abbiamo un totale di 2 macchine virtuali nel gruppo B, in modo che ogni macchina virtuale Ottiene ora la metà del totale del gruppo B del 50%, 25% ogni o pari a 2 LPs vale la pena di tempo di calcolo.

## <a name="setting-cpu-caps-on-individual-vms"></a>Impostare limiti massimi della CPU nelle singole macchine virtuali

Oltre il limite di gruppo, ogni macchina virtuale può avere anche un singolo delimitatore"macchina virtuale". I controlli delle risorse della CPU per macchina virtuale, inclusi un limite di utilizzo della CPU, peso e riserva, sono stati una parte di Hyper-V rispetto alla sua introduzione.
Se combinato con un delimitatore di gruppo, un limite di utilizzo della macchina virtuale specifica la quantità massima di CPU che possono ottenere ogni Vice presidente del settore, anche se il gruppo dispone di risorse di CPU disponibili.

Ad esempio, l'amministratore di host possibile inserire un limite di 10% della macchina virtuale nelle macchine virtuali "C".
In questo modo, anche se la maggior parte delle VPs "C" sono inattive, ogni VP non viveva mai più del 10%.
Senza un limite di utilizzo della macchina virtuale, "C" le macchine virtuali in base alle esigenze è stato possibile ottenere prestazioni oltre ai livelli consentiti per i livelli.

## <a name="isolating-vm-groups-to-specific-host-processors"></a>Isolare i gruppi di macchine Virtuali ai processori Host specifico

Gli amministratori di host Hyper-V potrebbero essere anche la possibilità di dedicare le risorse di calcolo a una macchina virtuale.
Si supponga ad esempio l'amministratore desidera offrire un piano premium "A" macchina virtuale che presenta un limite di classe del 100%.
Queste macchine virtuali premium di instabilità possibili; e richiedono la latenza più bassa pianificazione anche vale a dire, si potrebbe non essere deprovisioning pianificate da qualsiasi altra macchina virtuale.
Per ottenere questa separazione, un gruppo di CPU può anche essere configurato con un mapping di affinità LP specifico.

Ad esempio, per adattare una "A" macchina virtuale nell'host in questo esempio, l'amministratore potrebbe creare un nuovo gruppo di CPU e impostare l'affinità processori del gruppo a un sottoinsieme di LPs dell'host.
I rimanenti LPs sarebbero affinità gruppi B e C.
L'amministratore può creare una singola macchina virtuale nel gruppo A, che potrebbe quindi avere accesso esclusivo LPs tutti in un gruppo, mentre i gruppi di livello inferiori presumibilmente B e C condividerebbero la LPs rimanenti.

## <a name="segregating-root-vps-from-guest-vps"></a>Memorizzando VPs radice da VPs Guest

Per impostazione predefinita, Hyper-V creerà una radice Vice presidente del settore su ogni LP fisica sottostante.
Questi VPs radice sono strettamente mappati 1:1 con il sistema LPs e non possono essere migrati, vale a dire, Vice presidente del settore ogni radice venga sempre eseguita sulla stessa LP fisico.
VPs Guest può essere eseguito su qualsiasi LP disponibili e condividono l'esecuzione con radice VPs.

Tuttavia, potrebbe essere auspicabile radice completamente separato attività Vice presidente del settore da VPs guest.
Si consideri l'esempio precedente in cui viene implementato un livello di "A" macchina virtuale premium.
Per garantire che VPS nostro "A" della macchina virtuale abbiano la latenza più bassa possibile e "variazione" o pianificazione variazione, desideriamo eseguirli in un set dedicato di LPs e verificare che la directory principale non viene eseguito in questi LPs.

Ciò è possibile usare una combinazione della configurazione "minroot", che limita la partizione del sistema operativo host in esecuzione su un subset dei processori logici complessiva del sistema, insieme a uno o più affinità con gruppi di CPU.

L'host di virtualizzazione può essere configurato per limitare la partizione host a LPs specifico, con uno o più gruppi di CPU, creare un'affinità con i rimanenti LPs.
In questo modo, le partizioni radice e guest eseguibili su risorse dedicate di CPU e completamente isolati, senza condivisione della CPU.

Per altre informazioni sulla configurazione di "minroot", vedere [gestione delle risorse CPU Host Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-minroot-2016).

## <a name="using-the-cpugroups-tool"></a>Utilizzo dello strumento CpuGroups

Esaminiamo alcuni esempi di come usare lo strumento CpuGroups.

>[!NOTE] 
>Parametri della riga di comando per lo strumento CpuGroups vengono passati usando spazi solo come delimitatori. Non '/' o '-' caratteri devono procedere l'opzione della riga di comando desiderata.

### <a name="discovering-the-cpu-topology"></a>L'individuazione della topologia di CPU

L'esecuzione CpuGroups con il GetCpuTopology restituisce informazioni relative al sistema corrente, come illustrato di seguito, tra cui l'indice LP, del nodo NUMA a cui appartiene il log, il pacchetto e gli ID di Core e l'indice radice Vice presidente del settore.

Nell'esempio seguente viene illustrato un sistema con 2 CPU/socket e nodi NUMA, un totale di 32 LPs e multithreading, abilitate e configurate per abilitare Minroot con radice 8 VPs, 4 da ogni nodo NUMA.
LPs con radice VPs hanno un RootVpIndex > = 0. LPs con un RootVpIndex-1 non sono disponibili nella partizione radice, ma vengono ancora gestite dall'hypervisor e verranno eseguiti VPs guest come consentito dalle altre impostazioni di configurazione.

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

In questo caso, verranno elencati tutti i gruppi di CPU in host corrente, i GroupId, limite di utilizzo della CPU del gruppo e di indici di LPs assegnati a quel gruppo.

Si noti che i valori limite di utilizzo della CPU validi sono compresi nell'intervallo [0, 65536] e questi valori express il limite di utilizzo di gruppo in percentuale (ad esempio, 32768 = 50%).

```console
C:\vm\tools>CpuGroups.exe GetGroups

CpuGroupId                          CpuCap  LpIndexes
------------------------------------ ------ --------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536  24,25,26,27,28,29,30,31
```

### <a name="example-3--print-a-single-cpu-group"></a>Esempio 3: stampa di un singolo gruppo di CPU

In questo esempio, abbiamo query verranno eseguite su un singolo gruppo di CPU utilizzando GroupId come filtro.

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000003
CpuGroupId                          CpuCap   LpIndexes
------------------------------------ ------ ----------
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
```

### <a name="example-4--create-a-new-cpu-group"></a>Esempio 4: creare un nuovo gruppo di CPU

In questo caso, si creerà un nuovo gruppo di CPU, che specifica l'ID del gruppo e il set di LPs da assegnare al gruppo.

```console
C:\vm\tools>CpuGroups.exe CreateGroup /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /GroupAffinity:0,1,16,17
```

Viene ora visualizzato il gruppo appena aggiunto.

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 65536 0,1,16,17
36AB08CB-3A76-4B38-992E-000000000002 32768 4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536 12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-5--set-the-cpu-group-cap-to-50"></a>Esempio 5: impostare il limite di gruppo della CPU al 50%

In questo caso, il limite di utilizzo della CPU gruppo verrà impostato su 50%.

```console
C:\vm\tools>CpuGroups.exe SetGroupProperty /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /CpuCap:32768
```

A questo punto è possibile confermare l'impostazione visualizzando il gruppo è appena aggiornati.

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000001

CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 32768 0,1,16,17
```

### <a name="example-6--print-cpu-group-ids-for-all-vms-on-the-host"></a>Esempio 6: gli ID gruppo di stampa della CPU per tutte le macchine virtuali nell'host

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

### <a name="example-7--unbind-a-vm-from-the-cpu-group"></a>Esempio 7: a disassocia una macchina virtuale dal gruppo di CPU

Per rimuovere una macchina virtuale da un gruppo di CPU, impostare su CpuGroupId della macchina virtuale con il GUID NULL. Rimuove il binding della VM dal gruppo di CPU.

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

### <a name="example-8--bind-a-vm-to-an-existing-cpu-group"></a>Esempio 8: eseguire l'associazione di una macchina virtuale a un gruppo esistente di CPU

In questo caso, si aggiungerà una macchina virtuale a un gruppo esistente di CPU.
Si noti che la macchina virtuale non deve essere associata ad alcun gruppo di CPU esistente oppure id del gruppo di impostazione della CPU non riuscirà.

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:36AB08CB-3A76-4B38-992E-000000000001
```

A questo punto, verificare che la macchina virtuale G1 sia nel gruppo di CPU desiderato.

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

### <a name="example-9--print-all-vms-grouped-by-cpu-group-id"></a>Esempio 9 – stampare tutte le macchine virtuali raggruppate per id del gruppo di CPU

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

### <a name="example-11--attempting-to-delete-a-non-empty-cpu-group"></a>Esempio 11: tentativo di eliminare un gruppo di CPU non vuoto

Solo svuotare i gruppi di CPU, vale a dire, i gruppi di CPU senza alcun associati macchine virtuali, può essere eliminato.
Tentativo di eliminare un gruppo di CPU non vuoto avrà esito negativo.

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
(null)
Failed with error 0xc0350070
```

### <a name="example-12--unbind-the-only-vm-from-a-cpu-group-and-delete-the-group"></a>Esempio 12: disassociare la macchina virtuale sola da un gruppo di CPU ed eliminare il gruppo

In questo esempio, si sarà usare diversi comandi per esaminare un gruppo di CPU, rimuovere la macchina virtuale singola appartenenti al gruppo di, quindi eliminare il gruppo.

In primo luogo, è possibile enumerare le macchine virtuali nel gruppo di.

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36AB08CB-3A76-4B38-992E-000000000001
CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36AB08CB-3A76-4B38-992E-000000000001     G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
```

Noteremo che solo una singola macchina virtuale, denominata G1, appartiene a questo gruppo.
È possibile rimuovere la macchina virtuale G1 dal nostro gruppo impostando l'ID del gruppo della macchina virtuale su NULL.

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:00000000-0000-0000-0000-000000000000
```

E verificare le modifiche...

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

E confermare che il gruppo non è più presente.

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap                     LpIndexes
------------------------------------ ------ -----------------------------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-13--bind-a-vm-back-to-its-original-cpu-group"></a>Esempio 13: associare una macchina virtuale al relativo gruppo di CPU originale

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
