---
title: Infrastruttura sorvegliata e macchine Virtuali schermate Guida alla pianificazione di hosting
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 854defc8-99f8-4573-82c0-f484e0785859
manager: dongill
author: nirb-ms
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: f280fbe682ebf706ce6ea5b53ea8af5e6f39d75d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857812"
---
# <a name="guarded-fabric-and-shielded-vm-planning-guide-for-hosters"></a>Infrastruttura sorvegliata e macchine Virtuali schermate Guida alla pianificazione di hosting

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

In questo argomento vengono illustrate le decisioni di pianificazione che dovranno essere apportate alle macchine virtuali schermate di abilitare per l'esecuzione nell'infrastruttura di. Se si aggiorna un'infrastruttura Hyper-V esistente o crea una nuova infrastruttura, in esecuzione schermato macchine virtuali costituito da due componenti principali:

- Il servizio sorveglianza Host (HGS) offre l'attestazione e protezione con chiave in modo che è possibile assicurarsi che le macchine virtuali schermate verranno eseguito solo su host Hyper-V integri e approvati. 
- Gli host Hyper-V integri e approvati in cui è possono eseguire le macchine virtuali schermate (e normali macchine virtuali), si parla di host sorvegliati.

![HGS e un host sorvegliato](../media/Guarded-Fabric-Shielded-VM/guarded-host-hgs-plus-host-diagram-basic.png)

## <a name="decision-1-trust-level-in-the-fabric"></a>Decisione di #1: Nell'infrastruttura di livello di attendibilità

Come si implementa il servizio sorveglianza Host e host Hyper-V sorvegliati dipenderà principalmente forza delle trust che si sta cercando di ottenere nell'infrastruttura. Il livello di attendibilità è disciplinato dalla modalità di attestazione. Sono disponibili due opzioni si escludono a vicenda:

1. Attestazione TPM

    Se l'obiettivo consiste nel proteggere le macchine virtuali da amministratori malintenzionati o un'infrastruttura compromessa, quindi si utilizzerà attestazione TPM. Questa opzione funziona bene per scenari multi-tenant hosting anche per quanto riguarda gli asset di valore elevato in ambienti aziendali, ad esempio i controller di dominio o server di contenuti, ad esempio SQL o SharePoint.
    Vengono valutati i criteri di integrità (HVCI) codice protetto da hypervisor e la loro validità applicata dal servizio HGS prima che l'host sia consentita l'esecuzione di macchine virtuali schermate. 

2. Attestazione chiave host

    Se i requisiti sono principalmente basati su conformità che richiede virtuali macchine crittografati sia quando sono inattivi che in transito, si userà l'attestazione chiave host. Questa opzione funziona bene per i Data Center generico in cui si ha familiarità con gli amministratori di host e dell'infrastruttura Hyper-V che possono accedere ai sistemi operativi guest delle macchine virtuali per le operazioni e manutenzione quotidiana. 

    In questa modalità, l'amministratore dell'infrastruttura è responsabile esclusivamente per garantire l'integrità degli host Hyper-V. 
    Poiché HGS non svolge alcuna parte decidere cosa è o non è consentita per l'esecuzione, malware e i debugger funzioneranno come previsto. 
    
    Tuttavia, i debugger che tentano di connettersi direttamente a un processo (ad esempio WinDbg.exe) vengono bloccati per le macchine virtuali schermate perché il processo di lavoro della macchina virtuale (VMWP.exe) è una luce processo protetto (PPL). 
    Le tecniche di debug alternative, ad esempio quelle usate da LiveKd.exe, non vengono bloccate. 
    A differenza delle macchine virtuali schermate, il processo di lavoro per le macchine virtuali di supporto della crittografia non viene eseguito come una libreria PPL in modo che i debugger tradizionali, ad esempio WinDbg.exe continueranno a funzionare normalmente.

    Una modalità di attestazione simile denominato attestazione amministratore (basata su Active Directory) è deprecata a partire da Windows Server 2019. 

Il livello di attendibilità che scelto è determineranno i requisiti hardware per gli host Hyper-V, nonché i criteri che si applicano a infrastruttura. Se necessario, è possibile distribuire l'infrastruttura sorvegliata con attestazione amministratore e dall'hardware esistente e convertirlo in attestazione TPM quando l'hardware è stato aggiornato ed è necessario aumentare la protezione dell'infrastruttura.

## <a name="decision-2-existing-hyper-v-fabric-versus-a-new-separate-hyper-v-fabric"></a>Decisioni #2: Infrastruttura Hyper-V esistente rispetto a una nuova infrastruttura Hyper-V separata

Se si dispone di un'infrastruttura esistente (Hyper-V o meno), è molto probabile che è possibile usare per eseguire le macchine virtuali schermate insieme ai normali macchine virtuali. Alcuni clienti scelgono di integrare le macchine virtuali schermate negli strumenti esistenti e le infrastrutture e ad altri utenti per separare l'infrastruttura per motivi aziendali.

## <a name="hgs-admin-planning-for-the-host-guardian-service"></a>Amministratore HGS pianificazione per il servizio sorveglianza Host

Distribuire il servizio sorveglianza Host (HGS) in un ambiente a sicurezza elevata, che può essere in un server fisico dedicato, una macchina virtuale schermata, una macchina virtuale in un host Hyper-V isolato (separati dall'infrastruttura che protetti) oppure uno logicamente separati, usando un Azure diversi sottoscrizione.   

| Area | Dettagli |
|------|---------|
| Requisiti di installazione | <ul><li>Un server (cluster a tre nodi consigliato per la disponibilità elevata)</li><li>Per il fallback, sono necessari almeno due server HGS</li><li>I server possono essere fisici o virtuali (server fisico con TPM 2.0 consigliato; TPM 1.2 di anche supportati)</li><li>Installazione Server Core di Windows Server 2016 o versione successiva</li><li>Rete comunichi con l'infrastruttura che consente HTTP o [configurazione del fallback](guarded-fabric-manage-branch-office.md#fallback-configuration)</li><li>Consigliato per la convalida dell'accesso al certificato HTTPS</li></ul> |
| Ridimensionamento | Ogni nodo server HGS di medie dimensioni (8 core e 4 GB) può gestire 1.000 host Hyper-V. |
| Management | Designare utenti specifici che gestiscono il servizio HGS. Devono essere distinti dagli amministratori dell'infrastruttura. Per il confronto, HGS cluster può essere considerato nello stesso modo come un'autorità di certificazione (CA) in termini di isolamento amministrativo, distribuzione fisica e a livello complessivo del livello di riservatezza. |
| Host Guardian Service Active Directory | Per impostazione predefinita, HGS installa la propria Active Directory per la gestione interna. Questa è una foresta indipendente, Auto-gestita e la configurazione consigliata per agevolare l'isolamento di HGS dall'infrastruttura.<br><br>Se si dispone già di una foresta di Active Directory con privilegiata elevati usato per l'isolamento, è possibile utilizzare tale foresta anziché la foresta predefiniti HGS. È importante che HGS non appartenga a un dominio nella stessa foresta come gli host Hyper-V o gli strumenti di gestione dell'infrastruttura. In questo modo potrebbe consentire un amministratore dell'infrastruttura di assumere il controllo servizio HGS. |
| Ripristino di emergenza | Sono disponibili tre opzioni:<br><ol><li>Installare un cluster del servizio HGS separato in ogni Data Center e autorizza le macchine virtuali schermate per l'esecuzione in Data Center di backup sia principale. Questo evita la necessità di estendere il cluster in una rete WAN e consente di isolare le macchine virtuali in modo che vengano eseguiti solo nel sito loro designato.</li><li>Installare servizio HGS in un cluster esteso tra due (o più) Data Center. Questo offre la resilienza, se la rete WAN si arresta, ma inserisce i limiti del clustering di failover. Non è possibile isolare i carichi di lavoro a un sito. una VM autorizzata per l'esecuzione in un sito può essere eseguita su un altro.</li><li>Registrare l'host Hyper-V con un altro servizio HGS come failover.</li></ol>È inoltre consigliabile eseguire il backup ogni HGS esportando la relativa configurazione in modo che sia sempre possibile recuperare in locale. Per altre informazioni, vedere [Export-HgsServerState](https://technet.microsoft.com/library/mt652164.aspx) e [importazione HgsServerState](https://technet.microsoft.com/library/mt652168.aspx). |
| Chiavi del servizio sorveglianza host | Servizio sorveglianza Host usa due coppie di chiavi asimmetriche, ovvero una chiave di crittografia e una chiave di firma, ognuno rappresentato da un certificato SSL. Sono disponibili due opzioni di generazione delle chiavi:<br><ol><li>Autorità di certificazione interna, è possibile generare le chiavi usando l'infrastruttura PKI interna. Ciò è adatto per un ambiente di Data Center.</li><li>Autorità di certificazione pubblicamente attendibile, usare un set di chiavi ottenuti da un'autorità di certificazione pubblicamente attendibile. Questo è l'opzione di hosting deve usare.</li></ol>Si noti che sebbene sia possibile usare certificati autofirmati, non è consigliabile per gli scenari di distribuzione diversi da labs proof-of-concept.<br><br>Oltre a usare le chiavi HGS, un provider di servizi di hosting può usare "bring your own key," in cui i tenant possono fornire le proprie chiavi in modo che i tenant di alcune (o all) possono avere la propria chiave del servizio HGS specifica. Questa opzione è adatta per gli host in grado di fornire un processo fuori banda per i tenant caricare le proprie chiavi. |
| Archiviazione chiavi del servizio sorveglianza host | Per la massima protezione possibile, è consigliabile che le chiavi HGS vengono create e archiviate esclusivamente nel modulo Security un Hardware (HSM). Se non si usa moduli di protezione hardware, BitLocker nei server HGS consigliano di applicare. |

## <a name="fabric-admin-planning-for-guarded-hosts"></a>Fabric admin pianificazione per host sorvegliati

| Area | Dettagli |
|------|---------|
| Hardware | <ul><li>Attestazione chiave host: È possibile usare qualsiasi hardware esistente come host sorvegliato. Esistono alcune eccezioni (per assicurarsi che l'host può utilizzare nuovi meccanismi di sicurezza, a partire da Windows Server 2016, vedere [componenti hardware compatibili con protezione basata sulla virtualizzazione di Windows Server 2016 di integrità del codice](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).</li><li>Attestazione TPM: È possibile usare qualsiasi hardware con le [qualificazione aggiuntiva di sicurezza Hardware](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/systems#system-server-assurance) purché sia configurato in modo appropriato (vedere [le configurazioni del Server che sono conformi con macchine virtuali schermate e basata su virtualizzazione protezione dell'integrità del codice](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md) per la configurazione specifica). Ad esempio TPM 2.0 e versione UEFI 2.3.1c e versioni successive.</li></ul> |
| Sistema operativo | È consigliabile usare l'opzione Server Core per il sistema operativo host Hyper-V. |
| Implicazioni sulle prestazioni | Basato su test delle prestazioni, prevediamo un approssimativamente il 5% densità-differenza tra esecuzione schermato macchine virtuali e macchine virtuali non schermate. Ciò significa che se un host Hyper-V specificato può essere eseguito 20 macchine virtuali non schermate, prevediamo che è possibile eseguire le macchine virtuali schermate 19.<br><br>Assicurarsi di verificare il ridimensionamento con carichi di lavoro tipici. Ad esempio, potrebbero esserci alcuni outlier con orientati alla scrittura i/o carichi di lavoro intensivi che influiranno ulteriormente la differenza di densità. |
| Considerazioni sulla succursale | A partire da Windows Server versione 1709, è possibile specificare un URL di fallback per un server HGS virtualizzato in esecuzione in locale come una macchina virtuale schermata nella succursale. L'URL di fallback può essere utilizzato quando la succursale si perde la connettività ai server HGS nel Data Center. Nelle versioni precedenti di Windows Server, un host Hyper-V in esecuzione in una connettività alle esigenze delle filiali al servizio sorveglianza Host per accendere o eseguire la migrazione in tempo reale di macchine virtuali schermate. Per altre informazioni, vedere [ramo considerazioni su office](guarded-fabric-manage-branch-office.md). |
