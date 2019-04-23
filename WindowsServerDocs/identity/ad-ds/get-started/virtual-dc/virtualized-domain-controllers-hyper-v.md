---
Title: La virtualizzazione dei controller di dominio con Hyper-V
description: Considerazioni relative alla virtualizzazione di Windows Server Active Directory controller di dominio in Hyper-V
author: MicrosoftGuyJFlo
ms.author: joflore
ms.date: 04/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.openlocfilehash: 684f3418bf336af4959282e7a8c2088d22a8c8dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865132"
---
# <a name="virtualizing-domain-controllers-using-hyper-v"></a>La virtualizzazione dei controller di dominio con Hyper-V

> Si applica a: Windows Server 2016

In questo argomento verrà aggiornato per rendere le linee guida applicabili a Windows Server 2016. Windows Server 2012 introduce numerosi miglioramenti per i controller di dominio virtualizzato (DC), tra cui le misure di sicurezza per impedire il rollback degli USN sul controller di dominio virtuali e la possibilità di clonare i controller di dominio virtuale. Per altre informazioni su questi miglioramenti, vedere [Introduction to Active Directory Domain Services (AD DS) Virtualization (Level 100)](../../introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100.md).

Hyper-V consente di consolidare ruoli server diversi in un singolo computer fisico. Questa guida descrive l'esecuzione di controller di dominio come sistemi operativi guest a 32 o 64 bit.

## <a name="planning-to-virtualize-domain-controllers"></a>Considerazioni sulla pianificazione dei controller di dominio virtualizzati

Questa sezione vengono illustrati i requisiti hardware per server Hyper-v, come evitare singoli punti di guasto, selezione del tipo appropriato di configurazione per il server Hyper-V e macchine virtuali e le decisioni di sicurezza e prestazioni.

## <a name="hyper-v-requirements"></a>Requisiti di Hyper-V

Per installare e usare il ruolo Hyper-V, è necessario quanto segue:

   - **X64 processore**
      - Hyper-V è disponibile nelle versioni basate su x64 di Windows Server 2008 o versione successiva.  
   - **Virtualizzazione assistita mediante hardware**
      - Questa funzionalità è disponibile nei processori che includono un'opzione di virtualizzazione, in particolare Intel Virtualization Technology (Intel VT) o AMD Virtualization (AMD-V).  
   - **Protezione esecuzione dati hardware (DEP)**
      - La protezione esecuzione dati hardware deve essere disponibile e abilitata. In particolare, è necessario abilitare Intel XD bit (execute disable bit) o AMD NX bit (no execute).  

## <a name="avoid-creating-single-points-of-failure"></a>Evitare di creare singoli punti di errore

È necessario tentare di evitare di creare potenziali punti di errore quando si pianifica la distribuzione del controller di dominio virtuale. È possibile evitare di introdurre potenziali punti singoli di errore implementando la ridondanza del sistema. Si considerino, ad esempio, le raccomandazioni seguenti tenendo in mente i potenziali aumenti nel costo di amministrazione:

1. Eseguire almeno due controller di dominio virtualizzati per dominio in differenti host di virtualizzazione. In questo modo si riduce il rischio di perdere tutti i controller di dominio se si verificano dei problemi in un host di virtualizzazione.  
2. Come consigliato per altre tecnologie, diversificare l'hardware (utilizzo di CPU, schede madri, schede di rete differenti o altro hardware) nel quale vengono eseguiti i controller di dominio. La diversificazione dell'hardware limita il danno che potrebbe essere causato da un malfunzionamento specifico di una configurazione del fornitore, un driver o un solo pezzo o un tipo di hardware.  
3. Se possibile, i controller di dominio devono essere eseguiti su hardware ubicato in aree diverse del mondo. In questo modo, viene ridotto l'impatto che un disastro o un errore ha su un sito presso il quale sono ospitati i controller di dominio.  
4. Gestire i controller di dominio fisici in ognuno dei domini. In questo modo, si riduce il rischio di un malfunzionamento della piattaforma di virtualizzazione che influisce su tutti i sistemi host che utilizzano tale piattaforma.  

## <a name="security-considerations"></a>Considerazioni sulla sicurezza

Il computer host sul quale sono in esecuzione controller di dominio virtuali deve essere gestito con la stessa attenzione con cui vengono gestiti i controller di dominio scrivibili, anche se si tratta solo di un computer appartenente a un dominio o del gruppo di lavoro. Questo punto è essenziale ai fini della sicurezza. Un host gestito impropriamente è vulnerabile a un attacco di elevazione dei privilegi, che si verifica quando un utente malintenzionato ottiene accesso e privilegi di sistema non autorizzati né assegnati in modo legittimo. Un utente malintenzionato può utilizzare questo tipo di attacco per compromettere macchine virtuali, domini e foreste ospitate da tale computer.

Quando si pianifica la virtualizzazione dei controller di dominio, tenere sempre presenti le considerazioni sulla sicurezza seguenti:

   - L'amministratore locale di un computer che ospita controller di dominio scrivibili virtuali deve essere considerato alla pari dell'amministratore di dominio predefinito di tutti i domini e di tutte le foreste alle quali appartengono tali controller di dominio.  
   - La configurazione consigliata per evitare problemi di sicurezza e le prestazioni è un host che esegue un'installazione Server Core di Windows Server 2008 o versioni successive, senza applicazioni diverse da Hyper-V. Questa configurazione limita il numero di applicazioni e servizi installati nel server, che dovrebbero comportare un miglioramento delle prestazioni e un minor numero di applicazioni e servizi potenzialmente sfruttabili per attaccare il computer o rete. L'effetto di questo tipo di configurazione è quello di produrre una superficie di attacco ridotta. In una succursale o in altre posizioni che non possono essere protette in modo soddisfacente è consigliabile utilizzare controller di dominio di sola lettura. Se è disponibile una rete di gestione separata, è consigliabile connettere l'host solo a tale rete.  
   - È possibile usare Bitlocker con i controller di dominio, poiché Windows Server 2016 è possibile usare la funzionalità TPM virtuale offrono inoltre il materiale della chiave di guest per sbloccare il volume di sistema.
   - [Infrastruttura protetta e macchine virtuali schermate](/it-server/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md) può fornire controlli aggiuntivi per proteggere i controller di dominio.

Per informazioni sui controller, vedere [Guida alla distribuzione e pianificazione del Controller di dominio di sola lettura](../../deploy/rodc/read-only-domain-controller-updates.md).

Per altre informazioni sulla sicurezza dei controller di dominio, vedere [migliore Guida alle procedure consigliate per la sicurezza delle installazioni Active Directory](../../plan/security-best-practices/best-practices-for-securing-active-directory.md).

## <a name="security-boundaries-for-different-host-and-guest-configurations"></a>Limiti di sicurezza per le configurazioni guest e host diverso

Grazie all'utilizzo delle macchine virtuali è possibile disporre di diversi tipi di configurazione per i controller di dominio. È necessario considerare con molta attenzione in che modo le macchine virtuali influiscano sui limiti e sui trust della propria topologia Active Directory. Nella tabella che segue vengono descritte le configurazioni possibili per un controller di dominio e un host (server Hyper-V) Active Directory e i relativi computer guest (macchine virtuali eseguite sul server Hyper-V).

|Computer|Configurazione 1|Configuration 2|
|-------|---------------|---------------|
|Hyper-V|Computer membro o del gruppo di lavoro|Computer membro o del gruppo di lavoro|
|Guest|Controller di dominio|Computer membro o del gruppo di lavoro|

![](media/virtualized-domain-controller-architecture/Dd363553.f44706fd-317e-4f0b-9578-4243f4db225f(WS.10).gif)

   - L'amministratore del computer host dispone dello stesso tipo di accesso di cui dispone un amministratore di dominio sui guest dei controller di dominio scrivibili e deve essere considerato in base a tale presupposto. In presenza di un guest di controller di dominio di sola lettura, l'amministratore del computer host dispone dello stesso tipo di accesso di cui dispone un amministratore locale sul controller di dominio di sola lettura guest.   
   - Un controller di dominio di una macchina virtuale dispone di diritti amministrativi sull'host se questo appartiene allo stesso dominio. È un'opportunità per un utente malintenzionato comprometta tutte le macchine virtuali se l'utente malintenzionato ottiene innanzitutto l'accesso alla macchina virtuale 1. Questo è noto come vettore di attacco. In presenza di controller di dominio per più domini o foreste, tali domini devono disporre di un'amministrazione centralizzata in cui l'amministratore di un domino viene considerato attendibile su tutti i domini.  
   - La possibilità di un attacco dalla macchina virtuale 1 esiste anche se tale macchina viene installata come controller di dominio di sola lettura. Sebbene un amministratore di un controller di dominio di sola lettura non disponga esplicitamente dei diritti di amministratore di dominio, è possibile utilizzare il controller di dominio di sola lettura per inviare criteri al computer host. Tali criteri possono includere script di avvio. Se questa operazione riesce, il computer host potrebbe essere danneggiato e quindi utilizzato per compromettere le altre macchine virtuali ospitate.  

## <a name="security-of-vhd-files"></a>Sicurezza dei file VHD

Un file VHD di un controller di dominio virtuale equivale all'unità disco rigido fisica di un controller di dominio fisico. Per questo motivo è necessario proteggerla con la stessa cautela con cui si protegge l'unità disco rigido di un controller di dominio fisico. Verificare che l'accesso ai file VHD del controller di dominio sia consentito solo ad amministratori affidabili e attendibili.

## <a name="rodcs"></a>Controller di dominio di sola lettura

Un vantaggio offerto dai controller di dominio di sola lettura è la possibilità di collocarli in posizioni in cui non è possibile garantire la sicurezza fisica, ad esempio le succursali. È possibile usare crittografia unità BitLocker di Windows per proteggere i file di disco rigido virtuale (non i file System al suo interno) da eventuali compromissioni sull'host mediante furto del disco fisico. 
<!-- Removed link to Windows Server 2008 Hyper-V and BitLocker Drive Encryption (http://go.microsoft.com/fwlink/?linkid=123534). Link is dead. -->

## <a name="performance"></a>Prestazioni

La nuova architettura microkernel a 64 bit offre prestazioni Hyper-V di gran lunga superiori rispetto alle piattaforme di virtualizzazione precedenti. Per prestazioni host migliori, l'host deve essere un'installazione Server Core di Windows Server 2008 o versione successiva e non deve avere i ruoli server diversi da Hyper-V installato.

Le prestazioni delle macchine virtuali dipendono in modo specifico dal carico di lavoro. Per garantire prestazioni Active Directory soddisfacenti, testare topologie specifiche. Valutare il carico di lavoro corrente in un periodo di tempo mediante uno strumento, ad esempio l'affidabilità e Performance Monitor (PerfMon. msc) o la [Microsoft Assessment and Planning (MAP) toolkit](https://go.microsoft.com/fwlink/?linkid=137077). Lo strumento Microsoft Assessment and Planning è anche utile per gestire un inventario di tutti i server e ruoli del server disponibili nella rete.

Per ottenere un'idea generale delle prestazioni dei controller di dominio virtualizzati, i test delle prestazioni seguenti sono stato effettuati con il [Active Directory Performance Testing Tool (ADTest.exe)](https://go.microsoft.com/fwlink/?linkid=137088).

I test LDAP (Lightweight Directory Access Protocol) sono stati eseguiti su un controller di dominio fisico mediante ADTest.exe e quindi su una macchina virtuale ospitata su un server identico al controller di dominio fisico. È stato utilizzato un solo processore logico per il computer fisico e un solo processore virtuale per la macchina virtuale per raggiungere facilmente un utilizzo della CPU del 100%. Nella tabella seguente, la lettera e il numero tra parentesi dopo ogni test indicano il test specifico in ADTest.exe. Come i dati, le prestazioni del controller di dominio virtualizzati è stata 88 al 98% delle prestazioni dei controller di dominio fisico.

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Misura</th>
<th>Testa</th>
<th>Fisico</th>
<th>Virtuale</th>
<th>Delta</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Ricerche/sec</p></td>
<td><p>Ricerca di un nome comune nell'ambito di base (L1)</p></td>
<td><p>11508</p></td>
<td><p>10276</p></td>
<td><p>-10.71%</p></td>
</tr>
<tr class="even">
<td><p>Ricerche/sec</p></td>
<td><p>Ricerca di un insieme di attributi nell'ambito di base (L2)</p></td>
<td><p>10123</p></td>
<td><p>9005</p></td>
<td><p>-11.04%</p></td>
</tr>
<tr class="odd">
<td><p>Ricerche/sec</p></td>
<td><p>Ricerca di tutti gli attributi nell'ambito di base (L3)</p></td>
<td><p>1284</p></td>
<td><p>1242</p></td>
<td><p>-3.27%</p></td>
</tr>
<tr class="even">
<td><p>Ricerche/sec</p></td>
<td><p>Ricerca di un nome comune nell'ambito del sottoalbero (L6)</p></td>
<td><p>8613</p></td>
<td><p>7904</p></td>
<td><p>-8.23%</p></td>
</tr>
<tr class="odd">
<td><p>Binding riusciti/sec</p></td>
<td><p>Esecuzione di binding veloci (B1)</p></td>
<td><p>1438</p></td>
<td><p>1374</p></td>
<td><p>-4.45%</p></td>
</tr>
<tr class="even">
<td><p>Binding riusciti/sec</p></td>
<td><p>Esecuzione di binding semplici (B2)</p></td>
<td><p>611</p></td>
<td><p>550</p></td>
<td><p>-9.98%</p></td>
</tr>
<tr class="odd">
<td><p>Binding riusciti/sec</p></td>
<td><p>Utilizzo di NTLM per l'esecuzione dei binding (B5)</p></td>
<td><p>1068</p></td>
<td><p>1056</p></td>
<td><p>-1.12%</p></td>
</tr>
<tr class="even">
<td><p>Scritture/sec</p></td>
<td><p>Scrittura di più attributi (W2)</p></td>
<td><p>6467</p></td>
<td><p>5885</p></td>
<td><p>-9.00%</p></td>
</tr>
</tbody>
</table>

Per garantire prestazioni soddisfacenti, sono stati installati componenti di integrazione per consentire al sistema operativo guest di utilizzare miglioramenti o driver sintetici in grado di riconoscere l'hypervisor. Durante il processo di installazione, potrebbe essere necessario utilizzare driver IDE o di scheda di rete emulati. Negli ambienti di produzione è necessario sostituire tali driver emulati con driver sintetici per migliorare le prestazioni.

Quando si monitorano le prestazioni delle macchine virtuali con Reliability and Performance Manager (Perfmon.msc), nella macchina virtuale le informazioni sulla CPU non saranno del tutto corrette a causa della modalità di pianificazione della CPU virtuale sul processore fisico. Se si desidera ottenere informazioni sulla CPU per una macchina virtuale eseguita su un server Hyper-V, utilizzare i contatori del processore logico hypervisor Hyper-V nella partizione host.

Per altre informazioni sulle prestazioni di ottimizzazione di Active Directory Domain Services e Hyper-V, vedere [Performance Tuning linee guida per Windows Server 2016](../../../../administration/performance-tuning/index.md).
<!-- Updated to 2016 perf guidance -->

Inoltre, non pianificare l'utilizzo di un VHD per un disco differenze su una macchina virtuale configurata come controller di dominio in quanto tale VHD può ridurre le prestazioni. Per altre informazioni sui tipi di dischi Hyper-V, inclusi i dischi differenze, vedere [Creazione guidata disco rigido virtuale](http://go.microsoft.com/fwlink/?linkid=137279).
<!-- Couldn't find an equivalent WS 2016 Hyper-V article. -->

Per altre informazioni su Active Directory Domain Services in ambienti host virtuali, vedere [aspetti da considerare quando si ospitano controller di dominio di Active Directory negli ambienti host virtuali](https://go.microsoft.com/fwlink/?linkid=141292) nella Microsoft Knowledge Base.

## <a name="deployment-considerations-for-virtualized-domain-controllers"></a>Considerazioni sulla distribuzione dei controller di dominio virtualizzati

Esistono diverse procedure consigliate di macchina virtuale comuni che è consigliabile evitare quando si distribuiscono controller di dominio e considerazioni speciali per l'archiviazione e sincronizzazione dell'ora.

## <a name="virtualization-deployment-practices-to-avoid"></a>Procedure per la distribuzione della virtualizzazione da evitare

Le piattaforme di virtualizzazione, come Hyper-V, offrono alcune comode funzionalità che semplificano la gestione, la manutenzione, il backup e la migrazione dei computer. Tuttavia, le seguenti procedure consigliate di distribuzione comuni e le funzionalità non devono essere utilizzate per i controller di dominio virtuale:

   - Per garantire durabilità delle scritture di Active Directory, non vengono distribuiti i file di database del controller di dominio virtuale (database di Active Directory (NTDS. DIT), i log e SYSVOL) nei dischi IDE virtuali. In alternativa, creare un secondo disco rigido virtuale collegato a un controller SCSI virtuali e garantire che il database, log e SYSVOL vengono posizionati nel disco SCSI della macchina virtuale durante l'installazione del controller di dominio.  
   - Non implementare file VHD per dischi differenze su una macchina virtuale configurata come controller di dominio. Questa operazione rende eccessivamente semplice il ripristino di una versione precedente e comporta inoltre un calo delle prestazioni. Per altre informazioni sui tipi di disco rigido virtuale, vedere [Creazione guidata disco rigido virtuale](https://go.microsoft.com/fwlink/?linkid=137279).  
   - Non distribuire nuovi domini di Active Directory e gli insiemi di strutture in una copia di un sistema operativo Windows Server che non è stato innanzitutto preparato con Utilità preparazione sistema (Sysprep). Per altre informazioni sull'esecuzione di Sysprep, vedere [Panoramica su Sysprep (Utilità preparazione sistema)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)

      > [!WARNING]
      > Esecuzione di Sysprep su un controller di dominio non è supportato.

   - Per evitare una potenziale situazione di rollback dei numeri di sequenza di aggiornamento (USN), non utilizzare copie di un file VHD che rappresenta un controller di dominio già distribuito per distribuire altri controller di dominio. Per altre informazioni sul rollback degli USN, vedere [USN e Rollback degli USN](#usn-and-usn-rollback).
      - Windows Server 2012 e versioni successive consente agli amministratori di clonare le immagini di controller di dominio se preparato correttamente quando si vogliono distribuire i controller di dominio aggiuntivo
   - Non utilizzare la funzionalità di esportazione di Hyper-V per esportare una macchina virtuale che esegue un controller di dominio.
      - Con Windows Server 2012 e versioni successive, un'esportazione e importazione di un guest virtuale Controller di dominio viene gestito come un ripristino non autorevole quando rileva una modifica dell'ID di generazione e non è configurato per la clonazione.
      - Assicurarsi che non si usa il guest che è stata esportata più.
  - È possibile usare replica Hyper-V per mantenere una seconda copia inattiva di un Controller di dominio. Se si avvia l'immagine replicato, è necessario anche eseguire la pulizia appropriata, per lo stesso motivo non si utilizza l'origine dopo l'esportazione di un'immagine guest di controller di dominio.

## <a name="physical-to-virtual-migration"></a>Migrazione da macchina fisica a virtuale

System Center Virtual Machine Manager (VMM) 2008 offre una gestione unificata delle macchine fisiche e virtuali e consente inoltre di migrare una macchina fisica in una macchina virtuale. Questo processo è noto come conversione da macchina fisica a virtuale (conversione P2V). Durante il processo di conversione P2V, la nuova macchina virtuale e il controller di dominio fisico che viene eseguita la migrazione non deve essere in esecuzione nello stesso momento, per evitare una situazione di rollback degli USN come descritto in [USN e Rollback degli USN](#usn-and-usn-rollback).

È consigliabile eseguire la conversione P2V utilizzando la modalità offline in modo che i dati di directory siano coerenti quando il controller di dominio viene riattivato. L'opzione della modalità offline è disponibile e consigliata nella Conversione guidata server fisico. Per una descrizione della differenza tra modalità online e offline, vedere [P2V: conversione di computer fisici in macchine virtuali in VMM](https://go.microsoft.com/fwlink/?linkid=155072). Durante la conversione P2V, la macchina virtuale non deve essere connessa alla rete. La scheda di rete della macchina virtuale deve essere abilitata solo dopo che il processo di conversione P2V viene completato e verificato. A questo punto, la macchina di origine fisica verrà disattivata. Non riconnettere la macchina di origine fisica alla rete prima di aver riformattato il disco rigido.

> [!NOTE]
> Sono disponibili le opzioni più sicure per creare nuovi controller di dominio virtuale che non vengono eseguiti i rischi di creazione di un Rollback degli USN. È possibile impostare un nuovo controller di dominio virtuale da regolare innalzamento di livello, la promozione da installa da supporto e tramite la clonazione del Controller di dominio, se si dispone già di almeno un controller di dominio virtuale.
Questo contribuirà a evitare i problemi hardware o potrebbero verificarsi problemi relativi alla piattaforma sottoposti a conversione P2V guest virtuali.

> [!WARNING]
> Per evitare problemi relativi alla replica di Active Directory, assicurarsi che solo un'istanza (fisica o virtuale) di un dato dominio controller esiste in una rete specifica in qualsiasi punto nel tempo.
> È possibile ridurre la probabilità che il clone precedente in corso di un problema:
> 
> - Quando il nuovo controller di dominio virtuale è in esecuzione, modificare la password dell'account computer utilizzando due volte: resetpwd netdom. exe /Server: < controller di dominio >...
> - Esportare e importare il nuovo guest di virtualizzazione per far sì che diventi un nuovo ID di generazione e pertanto una chiamata a un database all'ID.
> 

## <a name="using-p2v-migration-to-create-test-environments"></a>Utilizzo della migrazione P2V per creare ambienti di prova

È possibile utilizzare la migrazione P2V mediante VMM per creare ambienti di prova. L'utente può migrare i controller del dominio di produzione dalle macchine fisiche alle macchine virtuali per creare un ambiente di prova senza disconnettere permanentemente tali controller. L'ambiente di prova, tuttavia, deve trovarsi su una rete diversa da quella dell'ambiente di produzione se è richiesta la presenza di due istanze dello stesso controller di dominio. Quando si creano ambienti di prova mediante la migrazione P2V, è necessario prestare particolare attenzione onde evitare situazioni di rollback degli USN che possono influire sugli ambienti di prova e di produzione. Di seguito viene descritto un metodo utilizzabile per la creazione di ambienti di prova con P2V.

Un controller di dominio nell'ambiente di produzione da ogni dominio viene eseguita la migrazione a una macchina virtuale di prova utilizzando P2V in base alle linee guida indicate nella sezione migrazione fisica a virtuale. Le macchine fisiche di produzione e le macchine virtuali di prova devono trovarsi in reti diverse quando vengono riconnesse. Per evitare i rollback degli USN nell'ambiente di prova, è necessario disconnettere tutti i controller di dominio che devono essere migrati dalle macchine fisiche nelle macchine virtuali. A tale scopo, arrestare il servizio ntds o riavviare il computer in modalità ripristino servizi directory (DSRM).) Quando i controller di dominio sono offline, non introdurre nuovi aggiornamenti nell'ambiente. I computer devono rimanere in modalità offline durante la migrazione P2V. Nessuno dei computer dovrà essere riconnesso finché tutti i computer non saranno stati migrati completamente. Per altre informazioni sul rollback degli USN, vedere USN e Rollback degli USN.

I successivi controller di dominio di prova devono essere innalzati al livello di repliche nell'ambiente di prova.

## <a name="time-service"></a>Servizio Ora

Per le macchine virtuali configurate come controller di dominio, è consigliabile disabilitare la sincronizzazione dell'ora tra il sistema e il guest sistema operativo host che agisce come un controller di dominio. In questo modo il controller di dominio guest sincronizzare l'ora dalla gerarchia di dominio.

Per disabilitare il provider di sincronizzazione ora Hyper-V, arrestare la macchina virtuale e deselezionare la casella di controllo di sincronizzazione ora in Integration Services.

> [!NOTE]
> Questo materiale sussidiario è stato aggiornato di recente in modo da riflettere la raccomandazione corrente per sincronizzare l'ora per il controller di dominio guest da solo la gerarchia dei domini, anziché l'indicazione di disattivare parzialmente la sincronizzazione dell'ora tra l'host precedente controller di dominio di sistema e guest.

## <a name="storage"></a>Archiviazione

Per ottimizzare le prestazioni della macchina virtuale controller di dominio e garantire la durabilità delle scritture di Active Directory, seguire questi suggerimenti per l'archiviazione del sistema operativo, Active Directory e file di disco rigido virtuale:

   - **Archiviazione guest**. Archiviare il file di database di Active Directory (Ntds.dit), i file di registro e i file SYSVOL su un disco virtuale separato dai file del sistema operativo. Creare un secondo disco rigido virtuale collegato a un controller SCSI virtuali e archiviare il database, log e SYSVOL su disco SCSI virtuale della macchina virtuale. Dischi SCSI virtuali offrono un miglioramento delle prestazioni rispetto all'IDE virtuale e supportano forzato Unit Access (FUA). FUA garantisce che il sistema operativo scrive e legge i dati direttamente dal supporto di ignorare tutti i meccanismi di memorizzazione nella cache.

   > [!NOTE]
   > Se si prevede di utilizzare Bitlocker per il guest di controller di dominio virtuale, è necessario assicurarsi che i volumi aggiuntivi configurati per "sbloccare automatica".
   > Altre informazioni sulla configurazione automatica sblocco è reperibile [Enable-BitLockerAutoUnlock](https://docs.microsoft.com/powershell/module/bitlocker/enable-bitlockerautounlock)

   - **Archiviazione host dei file VHD**. Consigli: i consigli per l'archiviazione host si riferiscono all'archiviazione dei file VHD. Per ottenere prestazioni ottimali, non archiviare i file VHD su un disco utilizzato di frequente da altri servizi o altre applicazioni, ad esempio il disco di sistema in cui è installato il sistema operativo Windows host. Archiviare ogni file VHD in una partizione separata dal sistema operativo host e dagli altri file VHD. La configurazione ideale prevede l'archiviazione di ogni VHD in un'unità fisica distinta.  

    Il sistema del disco fisico host deve inoltre soddisfare **almeno una** i criteri seguenti per soddisfare i requisiti di integrità dei dati del carico di lavoro virtualizzato:  

      - Il sistema Usa dischi di classe server (SCSI, Fibre Channel).  
      - Il sistema garantisce che i dischi siano connesse a una batteria memorizzazione nella cache scheda bus host (HBA).  
      - Il sistema utilizza un controller di archiviazione (ad esempio, un sistema RAID) come dispositivo di archiviazione.  
      - Il sistema garantisce che power sul disco sia protetta da un gruppo di continuità (UPS).  
      - Il sistema garantisce che funzionalità di memorizzazione nella cache in scrittura del disco è disabilitata.  

   - **Dischi VHD fissi e dischi pass-through**. È possibile configurare l'archiviazione per le macchine virtuali in diversi modi. Quando si utilizzano file VHD, i dischi VHD a dimensione fissa sono più efficienti dei dischi VHD dinamici in quanto la memoria per i dischi VHD a dimensione fissa viene allocata al momento della creazione dei dischi. I dischi pass-through, utilizzabili sulle macchine virtuali per accedere ai supporti di archiviazione fisici, offrono prestazioni persino migliori. Si tratta in pratica di dischi fisici o numeri di unità logica (LUN) collegati a una macchina virtuale, che non supportano la funzionalità di snapshot. I dischi pass-through rappresentano pertanto la configurazione preferita per i dischi rigidi in quanto l'uso degli snapshot con i controller di dominio non è consigliato.  

Per ridurre il rischio di danneggiamento dei dati di Active Directory, utilizzare controller SCSI virtuali:

   - Utilizzare unità fisiche SCSI (invece delle unità IDE/ATA) sui server Hyper-V che ospitano controller di dominio virtuali. Se non è possibile utilizzare tali unità, verificare che la cache in scrittura sia disabilitata sulle unità ATA/IDE che ospitano controller di dominio virtuali. Per altre informazioni, vedere [1539 ID evento – integrità del Database](https://go.microsoft.com/fwlink/?linkid=162419).
   - Per garantire la durabilità di Active Directory scrive, il database di Active Directory, registri e SYSVOL deve trovarsi su un disco virtuale SCSI. Dischi SCSI virtuali supportano forzato Unit Access (FUA). FUA garantisce che il sistema operativo scrive e legge i dati direttamente dal supporto di ignorare tutti i meccanismi di memorizzazione nella cache.  

## <a name="operational-considerations-for-virtualized-domain-controllers"></a>Considerazioni operative per i controller di dominio virtualizzati

I controller di dominio in esecuzione sulle macchine virtuali presentano restrizioni operative che non sono applicabili ai controller di dominio in esecuzione sulle macchine fisiche. Quando si utilizza un controller di dominio virtualizzato, è consigliabile non utilizzare alcune procedure e funzionalità software di virtualizzazione:

   - Non sospendere, arrestare o archiviare lo stato salvato di un controller di dominio in una macchina virtuale per periodi di tempo maggiori della durata per la rimozione definitiva della foresta e quindi eseguire la ripresa dallo stato salvato o di sospensione. In caso contrario, potrebbero verificarsi interferenze con la replica. Per informazioni su come determinare la durata di rimozione definitiva della foresta, vedere [determinare la durata di rimozione definitiva della foresta](https://go.microsoft.com/fwlink/?linkid=137177).  
   - Non copiare o clonare dischi rigidi virtuali (VHD). Anche con le misure di sicurezza in atto per la macchina virtuale guest, dischi rigidi virtuali singoli possono essere copiati e causano il rollback degli USN.
   - Non eseguire né utilizzare uno snapshot di un controller di dominio virtuale. Tecnicamente è supportato con Windows Server 2012 e versioni più recenti, non è una sostituzione per una buona strategia di backup. Esistono alcuni motivi per la creazione di snapshot di controller di dominio o il ripristino di snapshot.
   - Non utilizzare un VHD per un disco differenze su una macchina virtuale configurata come controller di dominio. Questa operazione rende eccessivamente semplice il ripristino di una versione precedente e comporta inoltre un calo delle prestazioni.  
   - Non utilizzare la funzionalità di esportazione su una macchina virtuale che esegue un controller di dominio.  
   - Non ripristinare un controller di dominio né tentare di eseguire il rollback del contenuto di un database di Active Directory con strumenti diversi da una soluzione di backup supportata. Per altre informazioni, vedere [Backup e ripristino di considerazioni per i controller di dominio virtualizzati](#backup-and-restore-practices-to-avoid).  

Tutti questi consigli sono utili per evitare il rischio di un rollback degli USN (Update Sequence Number). Per altre informazioni sul rollback degli USN, vedere USN e Rollback degli USN.

## <a name="backup-and-restore-considerations-for-virtualized-domain-controllers"></a>Considerazioni sul backup e il ripristino dei controller di dominio virtualizzati

Il backup dei controller di dominio rappresenta un requisito di importanza critica per qualsiasi ambiente. I backup proteggono da perdite di dati in caso di errore amministrativo o del controller di dominio. Se si verifica un errore di questo tipo, è necessario eseguire il rollback dello stato del sistema del controller di dominio a un momento precedente all'errore. Il metodo supportato per il ripristino di uno stato integro di un controller di dominio si basa sull'utilizzo di un'applicazione di backup compatibile con Active Directory, quale Windows Server Backup, per ripristinare il backup di uno stato del sistema originato dall'installazione corrente del controller di dominio. Per altre informazioni sull'uso di Windows Server Backup con servizi di dominio Active Directory (AD DS), vedere la [AD DS Guida al Backup e ripristino dettagliata](https://go.microsoft.com/fwlink/?LinkId=138501).

Grazie alla tecnologia delle macchine virtuali, determinati requisiti delle operazioni di ripristino di Active Directory assumono un ulteriore significato. Se, ad esempio, si ripristina un controller di dominio utilizzando una copia del file del disco rigido virtuale (VHD), si evita il passaggio critico di aggiornamento della versione del database di un controller di dominio in seguito al relativo ripristino. La replica procederà con numeri di registrazione non corretti, generando un database incoerente tra le repliche dei controller di dominio. Nella maggior parte dei casi questo problema non viene rilevato dal sistema di replica, né vengono segnalati errori nonostante le incoerenze tra i vari controller di dominio.

C'è un modo supportato per eseguire il backup e ripristino di un controller di dominio virtualizzato:

1. Eseguire Windows Server Backup nel sistema operativo guest.  

Con Windows Server 2012 e gli host Hyper-V più recenti e gli utenti Guest, è possibile sfruttare supportati i backup dei controller di dominio tramite gli snapshot, esportazione di macchine Virtuali guest e l'importazione e anche la replica Hyper-V. Tutti questi componenti non sono tuttavia una scelta ideale per la creazione di una cronologia di backup corretta, con l'eccezione leggera di esportazione della macchina virtuale guest.

Con Windows Server 2016 Hyper-V è il supporto per "snapshot di ambiente di produzione" in cui il server Hyper-V attiva un backup basato su VSS del guest e quando l'utente guest ha terminato con lo snapshot, l'host recupera i dischi rigidi virtuali e li archivia nel percorso di backup.

Questa impostazione con Windows Server 2012 e versioni successive, c'è un problema di incompatibilità con Bitlocker:

- Quando si esegue una cattura Blocca VSS, si desidera eseguire un'attività di post-snapshot per contrassegnare il database come proveniente da un backup o nel caso di preparazione di un'origine di installazione da supporto per controller di dominio, rimuovere le credenziali dal database AD.
- Quando Hyper-V monta il volume in snapshot per questa attività, non sono disponibili funzionalità che verrebbe sbloccare il Volume per l'accesso non crittografato. In modo che il motore di database di Active Directory ha esito negativo l'accesso al database e ha esito negativo dello snapshot.

> [!NOTE]
> Il progetto VM schermato menzionato in precedenza ha un host Hyper-V basato sui backup come obiettivo non-per la massima protezione dei dati della macchina virtuale guest.

## <a name="backup-and-restore-practices-to-avoid"></a>Procedure di backup e ripristino da evitare

Come già osservato, i controller di dominio in esecuzione sulle macchine virtuali presentano restrizioni non applicabili ai controller di dominio in esecuzione sulle macchine fisiche. Quando si esegue il backup o il ripristino di un controller di dominio virtuale, è consigliabile non utilizzare determinate procedure e funzionalità software di virtualizzazione:

   - Non copiare o clonare file VHD dei controller di dominio anziché eseguire backup regolari. Se il file VHD viene copiato o clonato, diventa obsoleto. Quindi, se il disco rigido virtuale viene avviato in modalità normale, si verificherà un Rollback degli USN. È consigliabile eseguire operazioni di backup appropriate supportate da Servizi di dominio Active Directory, come l'utilizzo della funzionalità Windows Server Backup.  
   - Non utilizzare la funzionalità Snapshot come backup per ripristinare una macchina virtuale configurata come controller di dominio. Si verifichino problemi con la replica quando si ripristina la macchina virtuale a uno stato precedente con Windows Server 2008 R2 e versioni precedenti. Per altre informazioni, vedere [USN e Rollback degli USN](#usn-and-usn-rollback). Sebbene l'utilizzo di uno snapshot per il ripristino di un controller di dominio di sola lettura non determini problemi di replica, è comunque consigliabile evitare tale metodo.  

## <a name="restoring-a-virtual-domain-controller"></a>Ripristino di un controller di dominio virtuale

Per ripristinare un controller di dominio in caso di errore, è necessario eseguire regolarmente il backup dello stato del sistema. Tale stato include i dati di Active Directory e i file di registro, il Registro di sistema, il volume di sistema (cartella SYSVOL) e vari elementi del sistema operativo. Questo requisito è lo stesso per i controller di dominio fisici e virtuali. Le procedure di ripristino dello stato del sistema eseguite mediante applicazioni di backup compatibili con Active Directory sono progettate per garantire la coerenza tra i database locali e replicati di Active Directory in seguito a un processo di ripristino e includono la notifica ai partner di replica delle reimpostazioni degli ID di chiamata. L'utilizzo di ambienti host virtuali e di applicazioni di acquisizione delle immagini del sistema operativo o del disco, tuttavia, consente agli amministratori di evitare le procedure di controllo e convalida che generalmente vengono eseguite quando si ripristina uno stato del sistema del controller di dominio.

Quando si verifica un errore in una macchina virtuale del controller di dominio e non viene eseguito un rollback dei numeri di sequenza di aggiornamento (USN), è possibile ripristinare la macchina virtuale in due modi diversi:

   - Se esiste un backup valido dei dati di stato del sistema antecedente all'errore, è possibile ripristinare lo stato del sistema mediante l'opzione di ripristino dell'utilità di backup utilizzata per creare il backup. È necessario che il backup dei dati di stato del sistema sia stato creato utilizzando un'utilità di backup compatibile con Active Directory entro l'intervallo di durata per la rimozione definitiva che, per impostazione predefinita, non è superiore ai 180 giorni. Il backup dei controller di dominio dovrebbe essere eseguito con una cadenza pari ad almeno la metà della durata per la rimozione definitiva. Per istruzioni su come determinare la durata di rimozione definitiva specifici per la foresta, vedere [determinare la durata di rimozione definitiva della foresta](https://go.microsoft.com/fwlink/?linkid=137177).  
   - Se è disponibile una copia di lavoro del file VHD, ma non è disponibile alcun backup dello stato del sistema, è possibile rimuovere la macchina virtuale esistente. Ripristinare la macchina virtuale esistente utilizzando una copia precedente del VHD, ma assicurarsi di avviarla in modalità ripristino servizi directory (DSRM) e configurare il Registro di sistema correttamente, come descritto nella sezione che segue. Riavviare quindi il controller di dominio in modalità normale.

Utilizzare il processo mostrato nell'illustrazione seguente per stabilire la modalità migliore per il ripristino del controller di dominio virtualizzato.

![](media\virtualized-domain-controller-architecture\Dd363553.85c97481-7b95-4705-92a7-006e48bc29d0(WS.10).gif)

Per i controller di dominio di sola lettura, il processo di ripristino e le decisioni da prendere sono più semplici.

![](media\virtualized-domain-controller-architecture\Dd363553.4c5c5eda-df95-4c6b-84e0-d84661434e5d(WS.10).gif)

## <a name="restoring-the-system-state-backup-of-a-virtual-domain-controller"></a>Ripristino del backup dello stato del sistema di un controller di dominio virtuale

Se è disponibile un backup dello stato del sistema valido per la macchina virtuale del controller di dominio, è possibile ripristinarlo seguendo la procedura di ripristino indicata dallo strumento di backup utilizzato per il backup del file VHD.

> [!IMPORTANT]
> Per ripristinare correttamente il controller di dominio, è necessario avviarlo in modalità ripristino servizi directory. Non consentire l'avvio del controller di dominio in modalità normale. Se si perde questa opportunità di attivare la modalità ripristino servizi directory durante l'avvio del sistema, spegnere la macchina virtuale del controller di dominio prima che l'avvio in modalità normale venga completato. È importante avviare il controller di dominio in modalità ripristino servizi directory in quanto l'avvio di un controller di dominio in modalità normale ne incrementa gli USN, anche se il controller di dominio viene disconnesso dalla rete. Per altre informazioni sul rollback degli USN, vedere USN e Rollback degli USN. 

## <a name="to-restore-the-system-state-backup-of-a-virtual-domain-controller"></a>Per ripristinare il backup dello stato del sistema di un controller di dominio virtuale

1. Avviare la macchina virtuale del controller di dominio e premere F5 per accedere alla schermata di Windows Boot Manager. Se è necessario immettere le credenziali di connessione, fare subito clic sul pulsante **Sospendi** sulla macchina virtuale in modo da interrompere l'avvio. Immettere quindi le credenziali di connessione e fare clic sul pulsante **Riproduci** sulla macchina virtuale. Fare clic nella finestra della macchina virtuale e premere F5.

   Se la schermata di Windows Boot Manager non è visualizzata e il controller di dominio inizia la procedura di avvio in modalità normale, spegnere la macchina virtuale per impedire che l'avvio venga completato. Ripetere questo passaggio il numero di volte necessario per accedere alla schermata di Windows Boot Manager. Non è possibile accedere alla modalità ripristino servizi directory dal menu Ripristino da errori di Windows. Se tale menu viene visualizzato, è necessario spegnere la macchina virtuale e riprovare.

2. Nella schermata di Windows Boot Manager premere F8 per accedere alle opzioni di avvio avanzate.
3. Nella schermata **Opzioni di avvio avanzate** selezionare **Modalità ripristino servizi directory** e quindi premere INVIO. Il controller di dominio verrà avviato in modalità ripristino servizi directory.
4. Applicare il metodo di ripristino appropriato per lo strumento utilizzato per creare il backup dello stato del sistema. Se si usa Windows Server Backup, vedere [esecuzione di un ripristino non autorevole di Active Directory Domain Services](https://go.microsoft.com/fwlink/?linkid=132637).

## <a name="restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available"></a>Ripristino di un controller di dominio virtuale quando non è disponibile un backup appropriato dei dati di stato del sistema

Se non è disponibile un backup dei dati di stato del sistema antecedente all'errore della macchina virtuale, è possibile utilizzare un file VHD precedente per ripristinare un controller di dominio in esecuzione su una macchina virtuale. Se possibile, effettuare una copia del VHD, in modo che se si incontra un problema durante la procedura o si verifica un errore in un passaggio, è possibile tentare nuovamente con la copia del VHD.

> [!IMPORTANT]
> - Non utilizzare la procedura riportata di seguito come sostituzione dei backup regolarmente pianificati e pianificati.
> - **I ripristini eseguiti con la procedura seguente non sono supportati da Microsoft e devono essere utilizzati solo quando è presente alcuna altra alternativa.**
> - Non utilizzare questa procedura se la copia del VHD che si sta per ripristinare è stata avviata in modalità normale da una macchina virtuale.

## <a name="to-restore-a-previous-version-of-a-virtual-domain-controller-vhd-without-system-state-data-backup"></a>Per ripristinare una versione precedente di un VHD del controller di dominio virtuale senza un backup dei dati di stato del sistema

1. Utilizzando il VHD precedente, avviare il controller di dominio virtuale in modalità ripristino servizi directory, come descritto nella sezione precedente. Non consentire l'avvio del controller di dominio in modalità normale. Se non si nota la schermata di Windows Boot Manager e il controller di dominio inizia la procedura di avvio in modalità normale, spegnere la macchina virtuale per impedire che l'avvio venga completato. Per informazioni dettagliate sull'attivazione della modalità ripristino servizi directory, vedere la sezione precedente.
2. Apri l'editor del Registro di sistema. A tale scopo, fare clic sul pulsante **Start**, scegliere **Esegui**, digitare **regedit** e quindi fare clic su OK. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**. Nell'Editor del Registro di sistema espandere il percorso seguente: **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\NTDS\\Parameters**. Cercare il valore **DSA Previous Restore Count**. Se il valore esiste, annotarne l'impostazione. Se il valore non esiste, l'impostazione corrisponde a quella predefinita, ovvero zero. Se non è visualizzato alcun valore, non aggiungerlo.
3. Fare clic con il pulsante destro del mouse sulla chiave **Parametri**, scegliere **Nuovo** e quindi **Valore DWORD (32 bit)**.
4. Digitare il nuovo nome **Database restored from backup** e quindi premere INVIO.
5. Fare doppio clic sul valore appena creato per aprire la finestra di dialogo **Modifica valore DWORD (32 bit)** e quindi digitare **1** nella casella **Dati valore**. Il **Database ripristinati da voce di backup** opzione è disponibile nei controller di dominio che eseguono Windows 2000 Server con Service Pack 4 (SP4), Windows Server 2003 con gli aggiornamenti inclusi in [come rilevare e il ripristino da un rollback degli USN in Windows Server 2003, Windows Server 2008 e Windows Server 2008 R2](https://go.microsoft.com/fwlink/?linkid=137182) nella Microsoft Knowledge Base installato e Windows Server 2008.
6. Riavviare il controller di dominio in modalità normale.
7. Quando il controller di dominio viene riavviato, aprire Visualizzatore eventi. Per aprire il Visualizzatore eventi, fare clic su **Start**, scegliere **Pannello di controllo**, fare doppio clic su **Strumenti di amministrazione** e quindi su **Visualizzatore eventi**.
8. Espandere **Registri applicazioni e servizi** e quindi fare clic sul registro **Servizi directory**. Verificare che gli eventi vengano visualizzati nel riquadro dei dettagli.
9. Fare clic con il pulsante destro del mouse sul registro **Servizi directory** e quindi scegliere **Trova**. In **Trova** digitare **1109** e quindi fare clic su **Trova successivo**.
10. Dovrebbe essere visualizzato almeno l'ID evento 1109. In caso contrario, continuare con il passaggio successivo. In alternativa, fare doppio clic sulla voce e quindi controllare il testo che conferma che l'attributo InvocationID è stato aggiornato:

   ```
   Active Directory has been restored from backup media, or has been configured to host an application partition. 
   The invocationID attribute for this directory server has been changed. 
   The highest update sequence number at the time the backup was created is <time>

   InvocationID attribute (old value):<Previous InvocationID value>
   InvocationID attribute (new value):<New InvocationID value>
   Update sequence number:<USN>

   The InvocationID is changed when a directory server is restored from backup media or is configured to host a writeable application directory partition.
   ```

11. Chiudere il Visualizzatore eventi.
12. Utilizzare l'Editor del Registro di sistema per verificare che il valore di **DSA Previous Restore Count** sia uguale al valore precedente più uno. Se non è il valore corretto e non è possibile trovare una voce per l'ID evento 1109 nel Visualizzatore eventi, verificare che i service pack del controller di dominio siano aggiornati. Non è possibile provare nuovamente questa procedura sullo stesso VHD. È possibile ritentare la procedura su una copia del VHD o un differente VHD che non è stato avviato in modalità normale ricominciando dal passaggio 1.
13. Chiudi l'Editor del Registro di sistema.

## <a name="usn-and-usn-rollback"></a>USN e Rollback degli USN

In questa sezione vengono descritti i problemi di replica che possono verificarsi in seguito a un ripristino non corretto del database di Active Directory con una versione precedente di una macchina virtuale. Per altri dettagli sul processo di replica di Active Directory, vedere [concetti di replica di Active Directory](../replication/active-directory-replication-concepts.md)

<!-- Replaced this link with 2016 article: [How the Active Directory Replication Model Works](http://go.microsoft.com/fwlink/?linkid=27636) (http://go.microsoft.com/fwlink/?LinkID=27636). -->

## <a name="usns"></a>USN

Servizi di dominio Active Directory utilizza i numeri di sequenza di aggiornamento (USN) per tenere traccia della replica dei dati tra i controller di dominio. Ogni volta che viene apportata una modifica ai dati contenuti nella directory, il numero USN viene incrementato per segnalare tale modifica.

Per ogni partizione di directory che archivia un controller di dominio di destinazione, vengono utilizzati numeri USN per tenere traccia di ultimo aggiornamento di origine che un controller di dominio introdotto al relativo database, nonché lo stato di tutti gli altri controller di dominio che viene archiviata una replica del partizione di directory. Quando i controller di dominio replicano le modifiche tra loro, essi query relativi partner di replica per le modifiche con USN maggiori dell'USN dell'ultima modifica che il controller di dominio ha ricevuto da ogni partner.

Le tabelle di metadati della replica due seguenti contengono USN. I controller di dominio di origine e destinazione li utilizzano per filtrare gli aggiornamenti richiesti dal controller di dominio di destinazione.

1. **Vettore di aggiornamento**: Una tabella che gestisce il controller di dominio di destinazione per il rilevamento degli aggiornamenti di origine vengono ricevuti da tutti i controller di dominio di origine. Quando un controller di dominio di destinazione richiede modifiche per una partizione di directory, offre il proprio vettore di aggiornamento al controller di dominio di origine. Il controller di dominio di origine utilizza quindi questo valore per filtrare gli aggiornamenti che invia al controller di dominio di destinazione. Il controller di dominio di origine invia il vettore di aggiornamento alla destinazione al completamento di un ciclo di replica completata per garantire che il controller di dominio di destinazione a conoscenza che sia sincronizzato con ogni controller di dominio aggiornamenti di origine e gli aggiornamenti sono allo stesso livello dell'origine.  
2. **Limite massimo**: Valore memorizzato dal controller di dominio di destinazione per tenere traccia delle modifiche più recenti che ha ricevuto da un controller di dominio di origine specifica per una partizione specifica. Il limite massimo impedisce che il controller di dominio di origine invii modifiche dal controller di dominio di destinazione ha già ricevuto da quest'ultimo.  

## <a name="directory-database-identity"></a>Identità database di directory

Oltre agli USN, i controller di dominio tengono traccia del database di directory dei partner di replica di origine. L'identità del database di directory in esecuzione sul server viene gestita separatamente dall'identità dell'oggetto server stesso. L'identità del database di directory in ogni controller di dominio viene archiviata nel **invocationID** attributo dell'oggetto impostazioni NTDS, che si trova nel percorso seguente Lightweight Directory Access Protocol (LDAP): cn = NTDS Settings, cn = ServerName, cn = Servers, cn =*nomesito*, cn = Sites, cn = Configuration, dc =*DominioRadiceForesta*. L'identità dell'oggetto server viene archiviata nell'attributo **objectGUID** dell'oggetto Impostazioni NTDS e non cambia mai. Tuttavia, l'identità delle modifiche del database di directory quando si verifica una procedura di ripristino dello stato di sistema sul server o quando viene aggiunta una partizione di directory applicativa, quindi la rimozione e successivamente aggiunte nuovamente dal server. (altro scenario: quando il writer VSS in una partizione contenente disco rigido virtuale del controller di dominio virtuale viene attivata un'istanza di Hyper-v, l'utente guest a sua volta attiva propri VSS Writer (lo stesso meccanismo utilizzato da un backup/ripristino) risultante in un altro metodo per cui l'attributo invocationID è reimpostazione)

Di conseguenza, **invocationID** mette in correlazione un insieme di aggiornamenti di origine su un controller di dominio con una specifica versione del database di directory. Il vettore di aggiornamento e il limite massimo contrassegnare le tabelle Usa il **invocationID** e proviene GUID controller di dominio rispettivamente in modo che le informazioni di replica di database di tale dominio controller sapere da quale copia di Active Directory.

**invocationID** è un valore di identificatore univoco globale (GUID) visibile nella parte superiore dell'output dopo l'esecuzione del comando **repadmin /showrepl**. Il testo seguente rappresenta un esempio di output del comando:

   ```
   Repadmin: running command /showrepl against full DC local host
   Default-First-Site-Name\VDC1
   DSA Options: IS_GC
   DSA object GUID: 966651f3-a544-461f-9f2c-c30c91d17818
   DSA invocationID: b0d9208b-8eb6-4205-863d-d50801b325a9
   ```

Quando Active Directory Domain Services viene ripristinato correttamente su un controller di dominio, il **invocationID** viene reimpostato. In seguito a questa modifica, si verificherà un incremento in traffico di replica, la cui durata è relativo alle dimensioni della partizione in fase di replica

Si supponga, ad esempio, che VDC1 e DC2 siano due controller dello stesso dominio. Nella figura seguente viene illustrata la percezione che DC2 ha di VDC1 quando il valore di invocationID viene reimpostato in una situazione di ripristino appropriata.

![](media\virtualized-domain-controller-architecture\Dd363553.ca71fc12-b484-47fb-991c-5a0b7f516366(WS.10).gif)

## <a name="usn-rollback"></a>Rollback degli USN

Il rollback degli USN viene eseguito quando gli aggiornamenti normali degli USN vengono aggirati e un controller di dominio prova a utilizzare un USN più basso rispetto all'ultimo aggiornamento. Verrà rilevato rollback degli USN e la replica viene arrestata prima che venga creato divergenza nella foresta, nella maggior parte dei casi. 

Il rollback degli USN può verificarsi in diverse circostanze, ad esempio quando si utilizzano file del disco rigido virtuale (VHD) precedenti o si esegue la conversione da macchina fisica a virtuale (conversione P2V) senza verificare che la macchina fisica non resti in modalità non in linea in modo permanente dopo la conversione. Per assicurarsi che il rollback degli USN non venga eseguito, adottare le precauzioni seguenti:

   - Quando non è in esecuzione Windows Server 2012 o versioni successive, non eseguire né utilizzare uno snapshot di una macchina virtuale controller di dominio.
   - Non copiare il file VHD del controller di dominio.  
   - Quando non è in esecuzione Windows Server 2012 o versioni successive, non esportare la macchina virtuale che esegue un controller di dominio.  
   - Non ripristinare un controller di dominio né tentare di eseguire il rollback del contenuto di un database di Active Directory con strumenti diversi da una soluzione di backup supportata, quale Windows Server Backup.  

In alcuni casi il rollback degli USN non viene rilevato, mentre in altri può causare ulteriori errori di replica. In questi casi, è necessario valutare l'entità del problema e provare a risolverlo in tempo utile. Per informazioni su come rimuovere oggetti residui che possono verificarsi in seguito a rollback degli USN, vedere [oggetti Active Directory obsoleti generano ID evento 1988 in Windows Server 2003](https://go.microsoft.com/fwlink/?linkid=137185) nella Microsoft Knowledge Base.

## <a name="usn-rollback-detection"></a>Rilevamento del rollback degli USN

Nella maggior parte dei casi, i rollback degli USN senza una corrispondente reimpostazione dell'attributo **invocationID**, dovuta a procedure di ripristino inadeguate, vengono rilevati. Windows Server 2008 offre protezioni dalla replica non appropriata eseguita dopo un'operazione di ripristino del controller di dominio non corretta. Tali protezioni vengono attivate in seguito a un'operazione di ripristino errata che restituisce numeri USN più bassi che rappresentano modifiche di origine già ricevute dai partner di replica.

In Windows Server 2008 e Windows Server 2003 SP1 quando un controller di dominio di destinazione richiede modifiche mediante un USN già utilizzato in precedenza, la risposta dal relativo partner di replica di origine viene interpretata dal controller di dominio di destinazione come indicazione che i metadati di replica sono obsoleti. Questo indica che è stato eseguito il rollback a uno stato precedente del database di Active Directory sul controller di dominio di origine, ad esempio il rollback a una versione precedente del file VHD di una macchina virtuale. In questo caso, il controller di dominio di destinazione avvia le misure di quarantena seguenti sul controller di dominio sul quale è stata identificata una procedura di ripristino errata:

   - Servizi di dominio Active Directory sospende il servizio Accesso rete che impedisce che gli account utente e gli account computer modifichino le password degli account. In questo modo è possibile evitare la perdita di tali modifiche quando vengono apportate dopo un ripristino inadeguato.  
   - Servizi di dominio Active Directory disabilita la replica di Active Directory in entrata e in uscita.  
   - Servizi di dominio Active Directory genera l'ID evento 2095 nel registro eventi di Directory Service per indicare la condizione.  

Nell'illustrazione seguente viene mostrata la sequenza di eventi che si verifica quando il rollback degli USN viene rilevato su VDC2, ovvero il controller di dominio di destinazione in esecuzione su una macchina virtuale. Nell'illustrazione il rilevamento del rollback degli USN si verifica su VDC2 quando un partner di replica rileva che VDC2 ha inviato un valore USN di aggiornamento già ricevuto in precedenza dal controller di dominio di destinazione, a indicare che il database di VDC2 ha eseguito il rollback in tempo ma in modo errato.

![](media\virtualized-domain-controller-architecture\\Dd363553.373b0504-43fc-40d0-9908-13fdeb7b3f14(WS.10).gif)

Se il registro eventi di Directory Service segnala l'ID evento 2095, completare la procedura che segue immediatamente.

## <a name="to-resolve-event-id2095"></a>Per risolvere l'ID evento 2095

1. Isolare la macchina virtuale che registrato l'errore dalla rete.
2. Tentare di stabilire se le modifiche sono state originate dal controller di dominio e propagate ad altri controller di dominio. Se l'evento è il risultato di uno snapshot o di una copia di una macchina virtuale avviata, provare a stabilire l'ora in cui si è verificato il rollback degli USN. È quindi possibile controllare i partner di replica del controller di dominio per stabilire se da quel momento è stata effettuata una replica.

   Per stabilirlo è possibile utilizzare lo strumento Repadmin. Per informazioni su come utilizzare Repadmin, vedere [il monitoraggio e la risoluzione dei problemi di replica di Active Directory mediante Repadmin](https://go.microsoft.com/fwlink/?linkid=122830). Se non si è in grado di determinare manualmente, contattare [supporto tecnico Microsoft](https://support.microsoft.com) per assistenza.

3. Imporre un abbassamento dei livello del controller di dominio, effettuando una pulizia dei metadati di tale controller e assegnando i ruoli di master operazioni, anche noti come ruoli FSMO (Flexible Single Master Operations). Per altre informazioni, vedere la sezione "Ripristino da un rollback degli USN" del [come rilevare e ripristinare da un rollback degli USN in Windows Server 2003, Windows Server 2008 e Windows Server 2008 R2](https://go.microsoft.com/fwlink/?linkid=137182) nella Microsoft Knowledge Base.
4. Eliminare tutti i file VHD precedenti per il controller di dominio.

## <a name="undetected-usn-rollback"></a>Rollback degli USN non rilevato

In una delle due circostanze seguenti è possibile che il rollback degli USN non venga rilevato:

1. Il file VHD è collegato ad altre macchine virtuali eseguite in più posizioni contemporaneamente.  
2. Il numero USN sul controller di dominio ripristinato è aumentato dopo l'ultimo USN ricevuto dall'altro controller di dominio.  

Nella prima circostanza altri controller di dominio potrebbero eseguire la replica con una delle macchine virtuali e potrebbero essere apportate modifiche su una delle due macchine virtuali che non vengono replicate sull'altra. Questa divergenza nella foresta può essere rilevata difficilmente e causerà risposte di directory imprevedibili. Una situazione del genere può verificarsi dopo una migrazione P2V quando sia la macchina virtuale che quella fisica vengono eseguite nella stessa rete, ma anche quando più controller di dominio virtuali vengono creati dallo stesso controller di dominio fisico e quindi eseguiti nella stessa rete.

Nella seconda circostanza un intervallo di numeri USN viene applicato a due insiemi di modifiche diversi. Tale operazione può continuare per periodi di tempo prolungati senza essere rilevata. Ogni volta che si modifica un oggetto creato durante tale intervallo di tempo, un oggetto residuo viene rilevato e segnalato come ID evento 1988 nel Visualizzatore eventi. Nell'illustrazione seguente viene mostrata una circostanza in cui il rollback degli USN non viene rilevato.

![](media\virtualized-domain-controller-architecture\Dd363553.63565fe0-d970-4b4e-b5f3-9c76bc77e2d4(WS.10).gif)

## <a name="read-only-domain-controllers"></a>Controller di dominio di sola lettura

Ovvero controller di dominio che ospitano copie di sola lettura delle partizioni in un database di Active Directory. Grazie a tali controller è possibile evitare la maggior parte dei problemi relativi ai rollback degli USN in quanto non replicano le modifiche agli altri controller di dominio. Se, tuttavia, un controller di dominio di sola lettura esegue una replica da un controller di dominio scrivibile interessato dal rollback degli USN, tale rollback influisce anche sul controller di dominio di sola lettura.

Non è consigliabile eseguire il ripristino di un controller di dominio di sola lettura utilizzando uno snapshot. A tale scopo, utilizzare un'applicazione di backup compatibile con Active Directory. Come per i controller di dominio scrivibili, è inoltre opportuno evitare che un controller di dominio di sola lettura resti in modalità non in linea per un periodo di tempo superiore alla durata per la rimozione definitiva. Questa condizione può generare oggetti residui nel controller di dominio di sola lettura.

Per ulteriori informazioni sui controller, vedere la [Guida alla distribuzione e pianificazione del Controller di dominio di sola lettura](../../deploy/rodc/read-only-domain-controller-updates.md).
