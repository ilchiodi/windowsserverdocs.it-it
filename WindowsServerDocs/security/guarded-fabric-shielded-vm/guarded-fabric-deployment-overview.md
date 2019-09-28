---
title: Guida introduttiva per la distribuzione di infrastruttura sorvegliata
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: e060e052-39a0-4154-90bb-b97cc6dde68e
manager: dongill
author: justinha
ms.author: justinha
ms.technology: security-guarded-fabric
ms.date: 01/30/2019
ms.openlocfilehash: 8359532113e04e2247b4af34effc7f5b89d36f34
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402414"
---
# <a name="quick-start-for-guarded-fabric-deployment"></a>Guida introduttiva per la distribuzione di infrastruttura sorvegliata

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento viene illustrata un'infrastruttura sorvegliata, i relativi requisiti e un riepilogo del processo di distribuzione. Per informazioni dettagliate sulla procedura di distribuzione, vedere [distribuzione del servizio sorveglianza host per host sorvegliati e macchine virtuali schermate](https://technet.microsoft.com/windows-server-docs/security/guarded-fabric-shielded-vm/guarded-fabric-deploying-hgs-overview).

Preferisci il video? Vedere il corso Microsoft Virtual Academy [per la distribuzione di VM schermate e un'infrastruttura sorvegliata con Windows Server 2016](https://mva.microsoft.com/en-US/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474).

## <a name="what-is-a-guarded-fabric"></a>Che cos'è un'infrastruttura sorvegliata

Un' _infrastruttura sorvegliata_ è un'infrastruttura di Windows Server 2016 Hyper-V in grado di proteggere i carichi di lavoro dei tenant da controlli, furti e manomissioni da malware in esecuzione nell'host, oltre che dagli amministratori di sistema. Questi carichi di lavoro di tenant virtualizzati, protetti sia inattivi che in transito, sono denominati _VM schermate_. 

## <a name="what-are-the-requirements-for-a-guarded-fabric"></a>Quali sono i requisiti per un'infrastruttura sorvegliata

I requisiti per un'infrastruttura sorvegliata includono:

- **Una posizione in cui eseguire macchine virtuali schermate gratuite da software dannoso.**

    Questi sono denominati _host sorvegliati_. 
    Gli host sorvegliati sono gli host Hyper-V di Windows Server 2016 Datacenter Edition che possono eseguire macchine virtuali schermate solo se possono dimostrare che sono in esecuzione in uno stato noto e attendibile per un'autorità esterna denominata servizio sorveglianza host (HGS). 
    HGS è un nuovo ruolo del server in Windows Server 2016 e viene in genere distribuito come cluster a tre nodi. 

- **Un modo per verificare un host si trova in uno stato integro.**

    Il HGS esegue l' _attestazione_, dove misura l'integrità degli host sorvegliati.

- **Un processo per rilasciare in modo sicuro le chiavi in host integri.**

    Il HGS esegue la protezione delle chiavi _e il rilascio della chiave_, in cui rilascia le chiavi in host integri.

- **Strumenti di gestione per automatizzare il provisioning protetto e l'hosting di VM schermate.**

    Facoltativamente, è possibile aggiungere questi strumenti di gestione a un'infrastruttura sorvegliata:

    - System Center 2016 Virtual Machine Manager (VMM). VMM è consigliato perché offre strumenti di gestione aggiuntivi oltre a quello che si ottiene usando solo i cmdlet di PowerShell forniti con Hyper-V e i carichi di lavoro dell'infrastruttura sorvegliata.
    - System Center 2016 Service Provider Foundation (SPF). Si tratta di un livello API tra Windows Azure Pack e VMM e un prerequisito per l'utilizzo di Windows Azure Pack.
    - Windows Azure Pack offre un'interfaccia Web grafica efficace per gestire un'infrastruttura sorvegliata e macchine virtuali schermate. 

In pratica, è necessario prendere una decisione prima di tutto: la [modalità di attestazione](guarded-fabric-and-shielded-vms.md#attestation-modes-in-the-guarded-fabric-solution) usata dall'infrastruttura sorvegliata. Ci sono due mezzi, due modalità che si escludono a vicenda, grazie ai quali HGS può misurare che un host Hyper-V è integro. Quando si inizializza HGS, è necessario scegliere la modalità:  

- L'attestazione della chiave host, o la modalità chiave, è meno sicura ma più semplice da adottare  
- L'attestazione basata su TPM, o la modalità TPM, è più sicura, ma richiede una configurazione e hardware specifico

Se necessario, è possibile eseguire la distribuzione in modalità chiave utilizzando gli host Hyper-V esistenti che sono stati aggiornati a Windows Server 2019 Datacenter Edition e quindi eseguire la conversione in modalità TPM più protetta quando è disponibile il supporto dell'hardware del server (incluso TPM 2,0). 

Ora che si conoscono le parti, esaminiamo un esempio del modello di distribuzione.

## <a name="how-to-get-from-a-current-hyper-v-fabric-to-a-guarded-fabric"></a>Come ottenere da un'infrastruttura di Hyper-V corrente a un'infrastruttura sorvegliata

Immaginiamo questo scenario: è presente un'infrastruttura di Hyper-V esistente, ad esempio Contoso.com, nell'immagine seguente e si vuole creare un'infrastruttura sorvegliata di Windows Server 2016.

![Infrastruttura di Hyper-V esistente](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-existing-hyper-v.png)

## <a name="step-1-deploy-the-hyper-v-hosts-running-windows-server-2016"></a>Passaggio 1: Distribuire gli host Hyper-V che eseguono Windows Server 2016 

Gli host Hyper-V devono eseguire Windows Server 2016 Datacenter Edition o versione successiva. Se si esegue l'aggiornamento degli host, è possibile [eseguire l'aggiornamento](https://technet.microsoft.com/windowsserver/dn527667.aspx) da Standard Edition a Datacenter Edition.

![Aggiornare gli host Hyper-V](../../security/media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-one-upgrade-hyper-v.png)

## <a name="step-2-deploy-the-host-guardian-service-hgs"></a>Passaggio 2: Distribuire il servizio sorveglianza host (HGS)

Installare quindi il ruolo del server HGS e distribuirlo come cluster a tre nodi, come nell'esempio di relecloud.com nell'immagine seguente. Sono necessari tre cmdlet di PowerShell:

- Per aggiungere il ruolo HGS, usare`Install-WindowsFeature` 
- Per installare HGS, usare`Install-HgsServer` 
- Per inizializzare HGS con la modalità di attestazione scelta, usare`Initialize-HgsServer` 

Se i server Hyper-V esistenti non soddisfano i prerequisiti per la modalità TPM (ad esempio, non hanno TPM 2,0), è possibile inizializzare HGS usando l'attestazione basata su amministratore (modalità AD), che richiede un trust Active Directory con il dominio dell'infrastruttura. 

In questo esempio contoso inizialmente viene distribuito in modalità AD per soddisfare immediatamente i requisiti di conformità e prevede di eseguire la conversione a un'attestazione più sicura basata su TPM dopo che è possibile acquistare l'hardware server appropriato. 

![Installare HGS](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-two-deploy-hgs.png)

## <a name="step-3-extract-identities-hardware-baselines-and-code-integrity-policies"></a>Passaggio 3: Estrarre le identità, le linee di base hardware e i criteri di integrità del codice

Il processo di estrazione delle identità dagli host Hyper-V dipende dalla modalità di attestazione utilizzata.

Per la modalità AD, l'ID dell'host è il proprio account computer aggiunto al dominio, che deve essere un membro di un gruppo di sicurezza designato nel dominio infrastruttura.
L'appartenenza al gruppo designato è l'unica decisione che indica se l'host è integro o meno. 

In questa modalità, l'amministratore dell'infrastruttura è responsabile solo per garantire l'integrità degli host Hyper-V. Poiché HGS non è in grado di decidere quale sia o non è consentita l'esecuzione, malware e debugger funzioneranno nel modo previsto. 

Tuttavia, i debugger che tentano di connettersi direttamente a un processo (ad esempio, WinDbg. exe) sono bloccati per le macchine virtuali schermate, perché il processo di lavoro della macchina virtuale (VMWP. exe) è una libreria PPL (Protected Process Light).
Le tecniche di debug alternative, ad esempio quelle usate da LiveKd. exe, non sono bloccate. Diversamente dalle VM schermate, il processo di lavoro per le macchine virtuali supportate dalla crittografia non viene eseguito come PPL, quindi i debugger tradizionali come WinDbg. exe continueranno a funzionare normalmente.

Detto un altro modo, i rigorosi passaggi di convalida usati per la modalità TPM non vengono usati per la modalità AD in alcun modo.

Per la modalità TPM sono necessari tre elementi: 

1.  Una _chiave_ di verifica dell'autenticità pubblica (o _EKPUB_) dal TPM 2,0 in ogni host Hyper-V. Per acquisire EKpub, usare `Get-PlatformIdentifier`. 
2.  Una _baseline hardware_. Se ognuno degli host Hyper-V è identico, è sufficiente una singola linea di base. In caso contrario, sarà necessario uno per ogni classe di hardware. La linea di base è sotto forma di file di log del gruppo di calcolo attendibile, o TCGlog. Il TCGlog contiene tutto ciò che l'host ha fatto, dal firmware UEFI al kernel, fino a quando l'host è stato completamente avviato. Per acquisire la baseline hardware, installare il ruolo Hyper-V e la funzionalità di supporto di sorveglianza host per Hyper- `Get-HgsAttestationBaselinePolicy`v e usare. 
3.  _Criteri di integrità del codice_. Se ognuno degli host Hyper-V è identico, è sufficiente un singolo criterio CI. In caso contrario, sarà necessario uno per ogni classe di hardware. Windows Server 2016 e Windows 10 sono entrambi dotati di un nuovo tipo di applicazione per i criteri CI, denominati _integrità del codice applicato dall'hypervisor (hvci obbligatoria)_ . HVCI obbligatoria fornisce un'imposizione avanzata e garantisce che a un host sia consentita l'esecuzione solo dei file binari consentiti da un amministratore attendibile. Queste istruzioni vengono incapsulate in un criterio CI aggiunto a HGS. HGS misura i criteri CI di ogni host prima che siano autorizzati a eseguire macchine virtuali schermate. Per acquisire un criterio CI, usare `New-CIPolicy`. Il criterio deve quindi essere convertito nel formato binario usando `ConvertFrom-CIPolicy`.

![Estrai identità, linea di base e criteri CI](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-three-extract-identity-baseline-ci-policy.png)

Questo è tutto: l'infrastruttura sorvegliata è compilata in termini di infrastruttura per l'esecuzione.  
A questo punto è possibile creare un disco modello di macchina virtuale schermato e un file di dati di schermatura in modo che sia possibile eseguire il provisioning di macchine virtuali schermate in modo semplice e sicuro. 

## <a name="step-4-create-a-template-for-shielded-vms"></a>Passaggio 4: Creare un modello per le macchine virtuali schermate

Un modello di macchina virtuale schermata protegge i dischi modello creando una firma del disco in un momento attendibile noto. 
Se il disco del modello viene successivamente infettato da malware, la firma sarà diversa dal modello originale che verrà rilevata dal processo di provisioning della VM schermato protetto. 
I dischi modello schermati vengono creati eseguendo la **creazione guidata disco modello schermato** o `Protect-TemplateDisk` su un normale disco modello. 

Ogni è incluso nella funzionalità **strumenti della VM schermata** nel [strumenti di amministrazione remota del server per Windows 10](https://www.microsoft.com/download/details.aspx?id=45520).
Dopo aver scaricato gli strumenti di amministrazione remota del server, eseguire questo comando per installare la funzionalità **strumenti di VM schermate** :

```powershell
Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
```

Un amministratore attendibile, ad esempio l'amministratore dell'infrastruttura o il proprietario della macchina virtuale, dovrà disporre di un certificato (spesso fornito da un provider di servizi di hosting) per firmare il disco del modello VHDX. 

![Creazione guidata disco modello schermato](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-shielded-template-wizard.png)

La firma del disco viene calcolata sulla partizione del sistema operativo del disco virtuale.
Se viene apportata alcuna modifica alla partizione del sistema operativo, viene modificata anche la firma.
Ciò consente agli utenti di identificare in modo sicuro i dischi attendibili specificando la firma appropriata.

Prima di iniziare, esaminare i [requisiti del disco del modello](guarded-fabric-create-a-shielded-vm-template.md) . 

## <a name="step-5-create-a-shielding-data-file"></a>Passaggio 5: Creare un file di dati di schermatura 

Un file di dati di schermatura, noto anche come file con estensione PDK, acquisisce informazioni riservate sulla macchina virtuale, ad esempio la password dell'amministratore. 

![Schermatura dei dati](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-five-create-shielding-data.png)

Il file di dati di schermatura include anche l'impostazione dei criteri di sicurezza per la macchina virtuale schermata. Quando si crea un file di dati di schermatura, è necessario scegliere uno dei due criteri di sicurezza seguenti:

- Schermate
   
    L'opzione più sicura, che elimina molti vettori di attacco amministrativi.

- Crittografia supportata

    Un livello di protezione inferiore che fornisce comunque i vantaggi di conformità della possibilità di crittografare una macchina virtuale, ma consente agli amministratori di Hyper-V di eseguire operazioni come l'uso della connessione della console VM e di PowerShell Direct. 

    ![Nuova VM supportata dalla crittografia](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-shielded-vm.png)

È possibile aggiungere componenti di gestione facoltativi come VMM o Windows Azure Pack. Se si vuole creare una macchina virtuale senza installare tali componenti, vedere [procedura dettagliata: creazione di VM schermate senza VMM](https://blogs.technet.microsoft.com/datacentersecurity/2016/06/06/step-by-step-creating-shielded-vms-without-vmm/).

## <a name="step-6-create-a-shielded-vm"></a>Passaggio 6: Creare una macchina virtuale schermata

La creazione di macchine virtuali schermate è leggermente diversa rispetto alle normali macchine virtuali. In Windows Azure Pack, l'esperienza è ancora più semplice rispetto alla creazione di una normale VM perché è sufficiente specificare un nome, un file di dati di schermatura (che contiene le altre informazioni sulla specializzazione) e la rete VM. 

![Nuova VM schermata in Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-vm-config.png)

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Prerequisiti di HGS](guarded-fabric-prepare-for-hgs.md)
