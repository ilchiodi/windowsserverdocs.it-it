---
title: Panoramica dell'infrastruttura sorvegliata e delle macchine virtuali schermate
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: edcf2e011b701904fb8c1b27d4f9b7d13415b2a8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856904"
---
# <a name="guarded-fabric-and-shielded-vms-overview"></a>Panoramica dell'infrastruttura sorvegliata e delle VM schermate

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

## <a name="overview-of-the-guarded-fabric"></a>Panoramica dell'infrastruttura sorvegliata

La protezione della virtualizzazione è un'area di investimento principale in Hyper-V. Oltre a proteggere gli host o le altre macchine virtuali da macchine virtuali che eseguono software dannoso, è necessario anche proteggere le macchine virtuali da host compromessi. Si tratta di un pericolo fondamentale per ogni piattaforma di virtualizzazione, indipendentemente dal fatto che sia Hyper-V, VMware o qualsiasi altro. Una macchina virtuale che esce dall'organizzazione in modo doloso o accidentale può essere infatti eseguita in qualsiasi altro sistema. Proteggere i beni a valore elevato dell'organizzazione, ad esempio i controller di dominio, i file server sensibili e i sistemi di gestione delle risorse umane, è una priorità.

Per garantire la protezione da infrastruttura di virtualizzazione compromessa, Windows Server 2016 Hyper-V ha introdotto VM schermate. Una macchina virtuale schermata è una macchina virtuale di seconda generazione (supportata in Windows Server 2012 e versioni successive) che dispone di un TPM virtuale, viene crittografata con BitLocker e può essere eseguita solo su host integri e approvati nell'infrastruttura. Le macchine virtuali schermate e l'infrastruttura sorvegliata consentono ai provider di servizi cloud o agli amministratori del cloud privato aziendale di offrire un ambiente più sicuro per le macchine virtuali tenant. 

Un'infrastruttura sorvegliata è costituita da:

- 1 servizio Sorveglianza host (HGS, Host Guardian Service), in genere un cluster di 3 nodi
- 1 o più host protetti
- Un set di macchine virtuali schermate. Il diagramma seguente mostra come il servizio Sorveglianza host usa l'attestazione per garantire che le macchine virtuali schermate vengano avviate soltanto da host noti e validi e la protezione con chiave per rilasciare in sicurezza le chiavi per le macchine virtuali schermate.

Quando un tenant crea macchine virtuali schermate che vengono eseguite in un'infrastruttura sorvegliata, anche gli host Hyper-V e le macchine virtuali schermate sono protette dal servizio HGS. HGS offre due servizi: attestazione e protezione con chiave. Il servizio di attestazione garantisce che solo gli host Hyper-V attendibili possano eseguire le macchine virtuali schermate, mentre il servizio di protezione con chiave offre le chiavi necessarie per l'accensione e la migrazione in tempo reale in altri host sorvegliati.

![Infrastruttura di host sorvegliata](../media/Guarded-Fabric-Shielded-VM/Guarded-Host-Overview-Diagram.png)

## <a name="video-introduction-to-shielded-virtual-machines"></a>Video: Introduzione alle macchine virtuali schermate 

<iframe src="https://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016/player" width="650" height="440" allowFullScreen frameBorder="0"></iframe>

## <a name="attestation-modes-in-the-guarded-fabric-solution"></a>Modalità di attestazione nella soluzione di infrastruttura sorvegliata

HGS supporta diverse modalità di attestazione per un'infrastruttura sorvegliata:

- Attestazione TPM attendibile (basata su hardware)
- Attestazione chiave host (basata su coppie di chiavi asimmetriche)

L'attestazione verificata da TPM è consigliata perché offre maggiori garanzie, come illustrato nella tabella seguente, ma richiede che negli host Hyper-V sia installato TPM 2.0. Se al momento non si dispone di TPM 2,0 o di un TPM, è possibile usare l'attestazione chiave host. Se si decide di passare all'attestazione TPM dopo aver acquistato il nuovo hardware, è possibile modificare la modalità di attestazione nel servizio Sorveglianza host con un'interruzione minima o senza alcuna interruzione per l'infrastruttura.

| **Modalità di attestazione scelta per gli host**                                            | **Assicurazioni host** |
|-------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|**Attestazione TPM:** offre la protezione massima ma richiede più passaggi di configurazione. Hardware e firmware host devono includere TPM 2,0 e UEFI 2.3.1 con avvio protetto abilitato. | Gli host sorvegliati vengono approvati in base all'identità del TPM, alla sequenza di avvio misurata e ai criteri di integrità del codice per assicurarsi che eseguano solo codice approvato.| 
| **Attestazione chiave host:** Progettato per supportare l'hardware host esistente in cui TPM 2,0 non è disponibile. Richiede un minor numero di passaggi di configurazione ed è compatibile con l'hardware dei server più comuni. | Gli host sorvegliati vengono approvati in base al possesso della chiave. | 

Un'altra modalità denominata **amministratore-attendibile attestazione** è deprecata a partire da Windows Server 2019. Questa modalità è basata sull'appartenenza a un host sorvegliato in un gruppo di sicurezza designato Active Directory Domain Services (AD DS). L'attestazione della chiave host fornisce un'identificazione host simile ed è più facile da configurare. 

## <a name="assurances-provided-by-the-host-guardian-service"></a>Garanzie offerte dal servizio Sorveglianza host

Il servizio HGS, in associazione ai metodi di creazione di macchine virtuali schermate, consentono di offrire le garanzie seguenti.

| **Tipo di garanzia per le macchine virtuali**                         | **Assicurazioni di VM schermate, dal servizio di protezione delle chiavi e dai metodi di creazione per le macchine virtuali schermate** |
|----------------------------|--------------------------------------------------|
| **Dischi crittografati con BitLocker (dischi del sistema operativo e dischi dati)**   | BitLocker viene usato nelle macchine virtuali schermate per la protezione dei dischi. Le chiavi di BitLocker necessarie per avviare la macchina virtuale e decrittografare i dischi sono protette dal TPM virtuale della VM schermata usando tecnologie collaudate del settore, ad esempio l'avvio con misurazione sicura. Mentre le macchine virtuali schermate eseguono automaticamente la crittografia e proteggono solo il disco del sistema operativo, è possibile eseguire anche la [crittografia delle unità di dati](https://technet.microsoft.com/itpro/windows/keep-secure/bitlocker-overview) collegate alla macchina virtuale schermata. |
| **Distribuzione di nuove macchine virtuali schermate da dischi/immagini modello "Trusted"** | Durante la distribuzione di nuove macchine virtuali schermate, i tenant sono in grado di specificare i dischi di modelli attendibili. I dischi di modelli schermati includono firme che vengono calcolate nel momento in cui il loro contenuto viene considerato attendibile. Le firme dei dischi vengono quindi memorizzate in un catalogo delle firme che i tenant inviano all'infrastruttura durante la creazione delle macchine virtuali schermate. Durante il provisioning delle macchine virtuali schermate, la firma del disco viene ricalcolata e confrontata con le firme attendibili incluse nel catalogo. Se la firma corrispondente, la macchina virtuale schermata viene distribuita. Se la firme non corrisponde, il disco di modello schermato viene considerato non attendibile e la distribuzione non riesce. |
| **Protezione delle password e altri segreti quando viene creata una macchina virtuale schermata** | Quando si creano macchine virtuali, è necessario assicurarsi che i segreti delle macchine virtuali, ad esempio le firme dei dischi attendibili, i certificati RDP e la password dell'account amministratore locale della macchina virtuale, non vengano divulgati all'infrastruttura. Questi segreti vengono memorizzati in un file crittografato chiamato file dei dati di schermatura (file con estensione pdk) che è protetto da chiavi tenant e che viene caricato nell'infrastruttura dal tenant. Quando si crea una macchina virtuale schermata, il tenant seleziona i dati di schermatura da usare per l'invio dei segreti solo ai componenti attendibili all'interno dell'infrastruttura sorvegliata. |
| **Controllo tenant della posizione in cui è possibile avviare la macchina virtuale** | I dati di schermatura includono anche un elenco delle infrastrutture sorvegliate nelle quali è possibile eseguire una determinata macchina virtuale schermata. Ciò risulta utile, ad esempio, nel caso in cui una macchina virtuale schermata che si trova in un cloud privato locale deve essere trasferita in un cloud pubblico o privato per un ripristino di emergenza. È necessario che il cloud o l'infrastruttura di destinazione supportino le macchine virtuali schermate e che la macchina virtuale consenta l'esecuzione da parte dell'infrastruttura. |

## <a name="what-is-shielding-data-and-why-is-it-necessary"></a>Che cosa sono i dati di schermatura e perché sono necessari?

Un file di dati di schermatura (chiamato anche file di dati di provisioning o file PDK) è un file crittografato creato dal tenant o dal proprietario di una macchina virtuale per proteggere le informazioni di configurazione importanti di una macchina virtuale, ad esempio la password dell'amministratore, RDP e altri certificati di identità, le credenziali di aggiunta al dominio e così via. L'amministratore dell'infrastruttura usa il file di dati di schermatura durante la creazione di una macchina virtuale schermata, ma non è in grado di visualizzare o usare le informazioni contenute nel file.

I file di dati di schermatura contengono altri segreti, ad esempio:

- Le credenziali dell'amministratore
- Un file di risposte (unattend.xml)
- Criteri di sicurezza che determinano se le macchine virtuali create con questi dati di schermatura sono configurate come schermate o con crittografia supportata
    - Tenere presente che le macchine virtuali configurate come schermate sono protette dagli amministratori dell'infrastruttura mentre le macchine virtuali con supporto della crittografia non lo sono
- Un certificato RDP per la protezione della comunicazione del desktop remoto con la macchina virtuale
- Un catalogo delle firme del volume che contiene un elenco delle firme attendibili dei dischi modello da cui è possibile creare una nuova macchina virtuale
- Una protezione con chiave che definisce le infrastrutture sorvegliate nelle quali una macchina virtuale schermata può essere eseguita

Il file di dati di schermatura (file con estensione pdk) garantisce che la macchina virtuale venga creata nel modo previsto dal tenant. Ad esempio, quando il tenant inserisce un file di risposte (unattend.xml) nel file di dati di schermatura e lo invia al provider di hosting, il provider non può visualizzare o modificare il file di risposte. Analogamente, il provider di hosting non può sostituire un file VHDX diverso durante la creazione della macchina virtuale schermata poiché il file di dati di schermatura contiene le firme dei dischi attendibili da cui possono essere create le macchine virtuali schermate.

La figura seguente mostra il file di dati di schermatura e gli elementi di configurazione correlati.

![File di dati di schermatura](../media/Guarded-Fabric-Shielded-VM/shielded-vms-shielding-data-file.png)

## <a name="what-are-the-types-of-virtual-machines-that-a-guarded-fabric-can-run"></a>Quali sono i tipi di macchine virtuali che possono essere eseguiti da un'infrastruttura sorvegliata?

Le infrastrutture sorvegliate sono in grado di eseguire tre tipi di macchine virtuali:

1.    Una macchina virtuale normale che non offre protezioni aggiuntive rispetto a quelle delle versioni precedenti di Hyper-V
2.    Una macchina virtuale con supporto della crittografia le cui protezioni possono essere configurate da un amministratore di infrastruttura
3.    Una macchina virtuale schermata con tutte le protezioni abilitate che non possono essere disabilitate da un amministratore di infrastruttura

Le macchine virtuali con supporto della crittografia sono progettate per essere usate nei casi in cui gli amministratori di infrastruttura sono completamente attendibili.  È possibile, ad esempio, che un'azienda distribuisca un'infrastruttura sorvegliata per garantire che i dischi delle macchine virtuali vengano crittografati nell'area di archiviazione per ragioni di conformità. Gli amministratori dell'infrastruttura possono continuare a usare le funzionalità di gestione più adatte, ad esempio le connessioni alla console delle macchine virtuali, PowerShell Direct e altri strumenti di gestione e risoluzioni dei problemi d'uso quotidiano.

Le macchine virtuali schermate sono progettate per essere usate nelle infrastrutture in cui i dati e lo stato della macchina virtuale devono essere protetti dagli amministratori dell'infrastruttura e da software non attendibile che potrebbe essere in esecuzione negli host Hyper-V. Ad esempio, le macchine virtuali schermate non consentono mai una connessione alla console della macchina virtuale, mentre nelle macchine virtuali con supporto della crittografia questa protezione può essere attivata o disattivata da un amministratore dell'infrastruttura.

La tabella seguente riepiloga le differenze tra le macchine virtuali schermate e supportate dalla crittografia.

| Funzionalità        | VM con supporto della crittografia di seconda generazione     | VM schermate di seconda generazione         |
|----------|--------------------|----------------|
|Avvio protetto        | Sì, obbligatorio ma configurabile        | Sì, obbligatorio e imposto    |
|Vtpm               | Sì, obbligatorio ma configurabile        | Sì, obbligatorio e imposto    |
|Crittografia dello stato della macchina virtuale e traffico di migrazione in tempo reale | Sì, obbligatorio ma configurabile |  Sì, obbligatorio e imposto  |
|Componenti di integrazione | Configurabili dall'amministratore dell'infrastruttura      | Alcuni componenti di integrazione bloccati (ad esempio, scambio di dati e PowerShell Direct) |
|Connessione alla macchina virtuale (console), dispositivi HID (ad esempio, tastiera e mouse) | Attivi, non possono essere disabilitati | Abilitato negli host che iniziano con Windows Server versione 1803; Disabilitato negli host precedenti |
|Porte COM/seriali   | Supportato                             | Funzionalità disabilitata, non può essere abilitata |
|Connetti un debugger (al processo della macchina virtuale)<sup>1</sup>| Supportato          | Funzionalità disabilitata, non può essere abilitata |

<sup>1</sup> i debugger tradizionali che si collegano direttamente a un processo, ad esempio WinDbg. exe, sono bloccati per le macchine virtuali schermate, perché il processo di lavoro della macchina virtuale (VMWP. exe) è una libreria PPL (Protected Process Light). Le tecniche di debug alternative, ad esempio quelle usate da LiveKd. exe, non sono bloccate. Diversamente dalle VM schermate, il processo di lavoro per le macchine virtuali supportate dalla crittografia non viene eseguito come PPL, quindi i debugger tradizionali come WinDbg. exe continueranno a funzionare normalmente. 

Le macchine virtuali schermate e le macchine virtuali con supporto della crittografia continuano a supportare le funzionalità di gestione dell'infrastruttura comuni, ad esempio Live Migration, la replica Hyper-V, i checkpoint delle macchine virtuali e così via.

## <a name="the-host-guardian-service-in-action-how-a-shielded-vm-is-powered-on"></a>Servizio Sorveglianza host: come viene accesa una macchina virtuale schermata

![File di dati di schermatura](../media/Guarded-Fabric-Shielded-VM/shielded-vms-how-a-shielded-vm-is-powered-on.png)

1. La VM01 viene accesa.

    Per poter accendere una macchina virtuale schermata è necessario che l'host sorvegliato venga dichiarato integro. Per dimostrare che è integro, deve presentare un certificato di integrità al servizio di protezione delle chiavi (KPS). Il certificato di integrità viene ottenuto tramite il processo di attestazione.

2. L'host richiede l'attestazione.

    L'host sorvegliato richiede l'attestazione. La modalità di attestazione è determinata dal servizio Sorveglianza host:

    **Attestazione TPM**: l'host Hyper-V invia informazioni che includono:

       - Informazioni di identificazione del TPM (la chiave di verifica dell'autenticità)
       - Informazioni sui processi avviati durante l'ultima sequenza di avvio (il log TCG)
       - Informazioni sul criterio di integrità del codice applicato nell'host. 

       Attestation happens when the host starts and every 8 hours thereafter. If for some reason a host doesn't have an attestation certificate when a VM tries to start, this also triggers attestation.

    **Attestazione chiave host**: l'host Hyper-V Invia la metà pubblica della coppia di chiavi. HGS convalida la chiave host registrata. 
    
    **Attestazione amministratore**: l'host Hyper-V invia un ticket Kerberos che identifica i gruppi di sicurezza cui appartiene l'host. HGS verifica che l'host appartenga a un gruppo di sicurezza configurato in precedenza dall'amministratore HGS attendibile.

3. L'attestazione ha esito positivo o non riesce.

    La modalità di attestazione determina quali controlli sono necessari per confermare che l'host è integro. Con l'attestazione TPM, vengono convalidate l'identità del TPM dell'host, le misurazioni di avvio e i criteri di integrità del codice. Con l'attestazione chiave host, viene convalidata solo la registrazione della chiave host. 

4. Il certificato di attestazione viene inviato all'host.

    Supponendo che l'attestazione abbia avuto esito positivo, un certificato di integrità viene inviato all'host e l'host viene considerato "sorvegliato" (autorizzato a eseguire macchine virtuali schermate). L'host usa il certificato di integrità per autorizzare il servizio di protezione delle chiavi a rilasciare in modo sicuro le chiavi necessarie per lavorare con macchine virtuali schermate

5. L'host richiede la chiave della macchina virtuale.

    L'host sorvegliato non dispone delle chiavi necessarie per accendere una macchina virtuale schermata, in questo caso VM01. Per ottenere le chiavi necessarie, l'host sorvegliato deve inviare al servizio di protezione delle chiavi quanto segue:

    - Il certificato di integrità corrente
    - Un segreto crittografato (una protezione con chiave) contenente le chiavi necessarie per l'accessione di VM01. Il segreto viene crittografato mediante altre chiavi note solo al servizio di protezione delle chiavi.

6. Rilascio della chiave.

    Il servizio di protezione delle chiavi esamina il certificato di integrità per determinarne la validità. Il certificato non deve essere scaduto e il servizio di protezione delle chiavi deve considerare attendibile il servizio di attestazione che lo ha emesso.

7. La chiave viene restituita all'host.

    Se il certificato di integrità è valido, il servizio di protezione delle chiavi tenta di decrittografare il segreto e restituire in modo sicuro le chiavi necessarie per accendere la macchina virtuale. Si noti che le chiavi vengono crittografate nel VBS dell'host sorvegliato.

8. L'host accende VM01.

## <a name="guarded-fabric-and-shielded-vm-glossary"></a>Glossario dell'infrastruttura sorvegliata e della macchina virtuale schermata

| Termine              | Definizione           |
|----------|------------|
| Servizio Sorveglianza host (HGS) | Ruolo di Windows Server installato in un cluster protetto di server bare metal senza sistema operativo in grado di misurare l'integrità di un host Hyper-V e di rilasciare le chiavi a host Hyper-V integri durante l'accensione o la migrazione in tempo reale di macchine virtuali schermate. Si tratta di due funzionalità fondamentali di una soluzione di macchina virtuale schermata e sono chiamate rispettivamente **servizio di attestazione** e **servizio di protezione delle chiavi**. |
| host sorvegliato | Host Hyper-V nel quale possono essere eseguite le macchine virtuali schermate. Un host può essere considerato _sorvegliato_ solo quando è stato ritenuto integro dal servizio di attestazione di HGS. Non è possibile accendere o eseguire la migrazione in tempo reale di macchine virtuali schermate in un host Hyper-V non attestato o con attestazione non riuscita. |
| infrastruttura sorvegliata    | Termine collettivo usato per descrivere un'infrastruttura di host Hyper-V e il servizio Sorveglianza host in grado di gestire ed eseguire macchine virtuali schermate. |
| macchina virtuale schermata | Macchina virtuale che può essere eseguita solo su host sorvegliati ed è protetta da ispezione, manomissione e furto da parte di amministratori di infrastrutture dannose e malware degli host. |
| amministratore dell'infrastruttura | Amministratore di cloud pubblico o privato che può gestire le macchine virtuali. Nel contesto di un'infrastruttura sorvegliata, un amministratore dell'infrastruttura non ha accesso alle macchine virtuali schermate o ai criteri che determinano gli host in cui possono essere eseguite le macchine virtuali schermate. |
| Amministratore HGS | Amministratore attendibile nel cloud pubblico o privato che è autorizzato a gestire i criteri e il materiale di crittografia degli host sorvegliati, ovvero degli host in cui è possibile eseguire una macchina virtuale schermata.|
| file di dati di provisioning o file di dati di schermatura (file con estensione PDK) | File crittografato creato da un tenant o da un utente per contenere le informazioni di configurazione importanti della macchina virtuale e per proteggere tali informazioni dall'accesso di altri. Ad esempio, un file di dati di schermatura può contenere la password che verrà assegnata all'account amministratore locale quando viene creata la macchina virtuale. |
| sicurezza basata sulla virtualizzazione (VBS) | Un ambiente di elaborazione e archiviazione basato su Hyper-V protetto dagli amministratori. La modalità protetta virtuale offre al sistema la possibilità di archiviare le chiavi del sistema operativo che non sono visibili a un amministratore del sistema operativo.|
| TPM virtuale | Versione virtualizzata di un TPM (Trusted Platform Module). A partire da Hyper-V in Windows Server 2016, è possibile fornire un dispositivo TPM 2,0 virtuale in modo che le macchine virtuali possano essere crittografate, così come un TPM fisico consente la crittografia di un computer fisico.|

## <a name="see-also"></a>Vedere anche

- [Infrastruttura sorvegliata e macchine virtuali schermate](guarded-fabric-and-shielded-vms-top-node.md)
- Blog: [Blog sulla sicurezza di datacenter e cloud privato](https://blogs.technet.microsoft.com/datacentersecurity/)
- Video: [Introduzione alle macchine virtuali schermate](https://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016)
- Video: [esaminare le VM schermate con Windows Server 2016 Hyper-V](https://channel9.msdn.com/events/Ignite/2016/BRK3124)
