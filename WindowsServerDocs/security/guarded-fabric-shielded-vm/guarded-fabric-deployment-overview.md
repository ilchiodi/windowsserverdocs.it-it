---
title: Guida introduttiva per la distribuzione dell'infrastruttura sorvegliata
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: e060e052-39a0-4154-90bb-b97cc6dde68e
manager: dongill
author: justinha
ms.author: justinha
ms.technology: security-guarded-fabric
ms.date: 01/30/2019
ms.openlocfilehash: d63726e7046896c9ef7aa0c3b3d85237bc3f788d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879062"
---
# <a name="quick-start-for-guarded-fabric-deployment"></a>Guida introduttiva per la distribuzione dell'infrastruttura sorvegliata

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento spiega che cos'è un'infrastruttura sorvegliata, i relativi requisiti e un riepilogo del processo di distribuzione. Per procedure dettagliate di distribuzione, vedere [il servizio sorveglianza Host per la distribuzione di host sorvegliati e macchine virtuali schermate](https://technet.microsoft.com/windows-server-docs/security/guarded-fabric-shielded-vm/guarded-fabric-deploying-hgs-overview).

Preferisci il video? Vedere il corso Microsoft Virtual Academy [distribuisce le VM schermate e un'infrastruttura protetta con Windows Server 2016](https://mva.microsoft.com/en-US/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474).

## <a name="what-is-a-guarded-fabric"></a>Che cos'è un'infrastruttura sorvegliata

Oggetto _sorvegliato fabric_ è un'infrastruttura di Windows Server 2016 Hyper-V in grado di proteggere carichi di lavoro tenant da ispezione, furti e manomissioni da malware in esecuzione nell'host, oltre che agli amministratori di sistema. Questi virtualizzare i carichi di lavoro tenant, protetto sia inattivi e in-flight, ovvero vengono chiamati _macchine virtuali schermate_. 

## <a name="what-are-the-requirements-for-a-guarded-fabric"></a>Quali sono i requisiti per un'infrastruttura sorvegliata

I requisiti per un'infrastruttura sorvegliata includono:

- **Un'unica posizione per l'esecuzione di macchine virtuali che è gratuita da software dannoso schermate.**

    Questi sono denominati _host sorvegliati_. 
    Gli host sorvegliati sono gli host Hyper-V di Windows Server 2016 Datacenter edition che possono eseguire VM schermate solo se e possono dimostrare che sono in esecuzione in uno stato noto e attendibile a un'autorità esterna chiamata il servizio sorveglianza Host (HGS). 
    Il servizio HGS è un nuovo ruolo del server in Windows Server 2016 e viene in genere distribuito come un cluster a tre nodi. 

- **Un modo per verificare un host è in uno stato integro.**

    Esegue il servizio HGS _attestazione_, in cui misura l'integrità degli host sorvegliati.

- **Un processo per rilasciare in modo sicuro le chiavi a host integri.**

    Esegue il servizio HGS _chiave di protezione e rilasciare il tasto_, in cui rilascia nuovamente le chiavi host integri.

- **Gli strumenti di gestione per automatizzare il provisioning sicuro e l'hosting di macchine virtuali schermate.**

    Facoltativamente, è possibile aggiungere questi strumenti di gestione per un'infrastruttura sorvegliata:

    - System Center 2016 Virtual Machine Manager (VMM). VMM è consigliato perché offre gestione aggiuntiva degli strumenti di là offerti dall'uso di solo i cmdlet di PowerShell forniti con Hyper-V e i carichi di lavoro dell'infrastruttura sorvegliata).
    - System Center 2016 Service Provider Foundation (SPF). Si tratta di un livello di API tra VMM e Windows Azure Pack e un prerequisito per l'uso di Windows Azure Pack.
    - Windows Azure Pack offre un'interfaccia buona web con interfaccia grafica per gestire un'infrastruttura sorvegliata e macchine virtuali schermate. 

In pratica, è necessario effettuare una delle decisioni in anticipo: il [modalità di attestazione](guarded-fabric-and-shielded-vms.md#attestation-modes-in-the-guarded-fabric-solution) usato per l'infrastruttura sorvegliata. Sono disponibili due metodi, ovvero due modalità si escludono a vicenda, ovvero per il quale è possibile misurare HGS che un host Hyper-V è integro. Quando si inizializza HGS, è necessario scegliere la modalità:  

- Attestazione chiave host o modalità chiave, è meno sicura, ma semplificare l'adozione  
- Attestazione basata su TPM, o in modalità TPM, è più sicura ma richiede più una configurazione e hardware specifico

Se necessario, è possibile distribuire in modalità principali con gli host Hyper-V esistenti che sono stati aggiornati a Windows Server 2019 Datacenter edition e quindi convertire alla modalità di TPM più sicura quando è disponibile il supporto hardware del server (tra cui TPM 2.0). 

Ora che sai di cosa sono le parti, di seguito viene illustrato un esempio di modello di distribuzione.

## <a name="how-to-get-from-a-current-hyper-v-fabric-to-a-guarded-fabric"></a>Come ottenere da un'infrastruttura Hyper-V corrente a un'infrastruttura sorvegliata

Si immagini questo scenario, si dispone di un'infrastruttura Hyper-V esistente, come Contoso.com nell'immagine seguente, e si vuole compilare un'infrastruttura protetta a Windows Server 2016.

![Infrastruttura Hyper-V esistente](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-existing-hyper-v.png)

## <a name="step-1-deploy-the-hyper-v-hosts-running-windows-server-2016"></a>Passaggio 1: Distribuire gli host Hyper-V che eseguono Windows Server 2016 

L'host Hyper-V devono eseguire Windows Server 2016 Datacenter edition o versione successiva. Se si esegue l'aggiornamento host, è possibile [aggiornare](https://technet.microsoft.com/windowsserver/dn527667.aspx) dall'edizione Standard di Datacenter edition.

![Eseguire l'aggiornamento host Hyper-V](../../security/media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-one-upgrade-hyper-v.png)

## <a name="step-2-deploy-the-host-guardian-service-hgs"></a>Passaggio 2: Distribuire il servizio sorveglianza Host (HGS)

Quindi installare il ruolo del server HGS e distribuirlo come un cluster a tre nodi, come illustrato nell'esempio relecloud.com nell'immagine seguente. Questa operazione richiede tre cmdlet di PowerShell:

- Per aggiungere il ruolo del servizio HGS, usare `Install-WindowsFeature` 
- Per installare il servizio HGS, usare `Install-HgsServer` 
- Per inizializzare il servizio HGS con la modalità di attestazione scelta, usare `Initialize-HgsServer` 

Se i server Hyper-V esistenti non soddisfano i prerequisiti per la modalità TPM (ad esempio, non hanno TPM 2.0), è possibile inizializzare HGS usando l'attestazione basata su Amministrazione (modalità Active Directory), che richiede un trust di Active Directory con il dominio dell'infrastruttura. 

In questo esempio, si supponga che Contoso inizialmente installato in modalità di Active Directory per poter per immediatamente soddisfare i requisiti di conformità e piani in cui convertire l'attestazione basata su TPM più sicuro dopo che è possibile acquistare hardware del server appropriato. 

![Installare HGS](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-two-deploy-hgs.png)

## <a name="step-3-extract-identities-hardware-baselines-and-code-integrity-policies"></a>Passaggio 3: Estrarre le identità, le linee di base di hardware e i criteri di integrità di codice

Il processo per estrarre le identità da host Hyper-V dipende dalla modalità attestazione in uso.

Per la modalità Active Directory, l'ID dell'host è l'account di computer aggiunti al dominio, che deve essere un membro di un gruppo di sicurezza designato nel dominio dell'infrastruttura.
L'appartenenza al gruppo designato è la determinazione sola del fatto che l'host è integra o meno. 

In questa modalità, l'amministratore dell'infrastruttura è responsabile esclusivamente per garantire l'integrità degli host Hyper-V. Poiché HGS non svolge alcuna parte decidere cosa è o non è consentita per l'esecuzione, malware e i debugger funzioneranno come previsto. 

Tuttavia, i debugger che tentano di connettersi direttamente a un processo (ad esempio WinDbg.exe) vengono bloccati per le macchine virtuali schermate perché il processo di lavoro della macchina virtuale (VMWP.exe) è una luce processo protetto (PPL).
Le tecniche di debug alternative, ad esempio quelle usate da LiveKd.exe, non vengono bloccate. A differenza delle macchine virtuali schermate, il processo di lavoro per le macchine virtuali di supporto della crittografia non viene eseguito come una libreria PPL in modo che i debugger tradizionali, ad esempio WinDbg.exe continueranno a funzionare normalmente.

In altre parole, i passaggi di convalida rigorosa utilizzati per la modalità TPM non vengono usati per la modalità di AD in alcun modo.

Per la modalità TPM, sono necessari tre elementi: 

1.  Oggetto _chiave pubblica di verifica dell'autenticità_ (o _EKpub_) da TPM 2.0 in ogni host Hyper-V. Per acquisire il EKpub, usare `Get-PlatformIdentifier`. 
2.  Oggetto _linea di base di hardware_. Se ognuno degli host Hyper-V è identico, una singola linea di base è tutto quello che serve. Se non lo sono, quindi è necessario uno per ogni classe dell'hardware. La linea di base è sotto forma di un file di log di Trustworthy Computing Group o TCGlog. Il TCGlog contiene tutto ciò che l'host è stato fatto, dal firmware UEFI, attraverso il kernel, fino a destra in cui l'host è completamente avviato. Per acquisire la linea di base di hardware, installare il ruolo Hyper-V e la funzionalità di supporto Hyper-V per sorveglianza Host e utilizzare `Get-HgsAttestationBaselinePolicy`. 
3.  Oggetto _criteri di integrità del codice_. Se ognuno degli host Hyper-V è identico, un singolo criterio di integrazione continua è tutto quello che serve. Se non lo sono, quindi è necessario uno per ogni classe dell'hardware. Windows Server 2016 e Windows 10 sia disponibile un nuovo modulo di imposizione per i criteri degli elementi di configurazione, chiamato _l'integrità di codice applicata da Hypervisor (HVCI)_. HVCI garantisce l'applicazione avanzata e assicura che un host è consentito solo per l'esecuzione dei file binari che un amministratore attendibile ha consentito l'esecuzione. Queste istruzioni vengono eseguito il wrapping in un criterio di integrazione continua che viene aggiunto al servizio HGS. HGS misura criteri degli elementi di configurazione di ciascun host prima che si autorizzati a eseguire VM schermate. Per acquisire un criterio di integrazione continua, usare `New-CIPolicy`. Il criterio deve quindi essere convertito per il formato binario usando `ConvertFrom-CIPolicy`.

![Estrarre le identità, linea di base e degli elementi di configurazione dei criteri](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-three-extract-identity-baseline-ci-policy.png)

Questo è tutto: l'infrastruttura sorvegliata viene compilato, in termini di infrastruttura per eseguirlo.  
Ora è possibile creare un disco modello VM schermato e un file di dati di schermatura schermate in modo semplice e sicuro è possono eseguire il provisioning le macchine virtuali. 

## <a name="step-4-create-a-template-for-shielded-vms"></a>Passaggio 4: Creare un modello per le macchine virtuali schermate

Un modello VM schermato protegge dischi modello creando una firma del disco in un punto noto affidabile nel tempo. 
Se il disco modello in un secondo momento viene infettato da malware, la firma sarà diversa modello originale che verrà rilevato per la macchina virtuale schermata sicura processo di provisioning. 
Dischi di modelli di schermate vengono creati eseguendo il **Creazione guidata la creazione di disco modello schermato** o `Protect-TemplateDisk` su un disco modello normale. 

Ognuno è incluso con il **strumenti macchina virtuale schermata** funzionalità nel [Remote Server Administration Tools per Windows 10](https://www.microsoft.com/download/details.aspx?id=45520).
Dopo aver scaricato l'amministrazione remota del server, eseguire questo comando per installare il **strumenti macchina virtuale schermata** funzionalità:

```powershell
Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
```

Un amministratore attendibile, ad esempio l'amministratore dell'infrastruttura o il proprietario della macchina virtuale, sarà necessario un certificato (spesso fornito da un Provider di servizi di Hosting) per firmare il disco di modello VHDX. 

![Creazione guidata disco modello schermato](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-shielded-template-wizard.png)

La firma del disco viene calcolata tramite la partizione del sistema operativo del disco virtuale.
Se eventuali modifiche nella partizione del sistema operativo, verrà modificato anche la firma.
Ciò consente agli utenti di fortemente identificare i dischi di cui si fidano, specificando la firma appropriata.

Rivedere le [requisiti del disco modello](guarded-fabric-create-a-shielded-vm-template.md) prima di iniziare. 

## <a name="step-5-create-a-shielding-data-file"></a>Passaggio 5: Creare un file di dati di schermatura 

Un file di dati schermatura, anche noto come file con estensione pdk, acquisisce informazioni sensibili relative alla macchina virtuale, ad esempio la password dell'amministratore. 

![Dati di schermatura](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-five-create-shielding-data.png)

Il file di dati di schermatura include anche l'impostazione dei criteri di sicurezza per la macchina virtuale schermata. È necessario scegliere uno dei due criteri di sicurezza quando si crea un file di dati di schermatura:

- Schermate
   
    L'opzione più sicura, che elimina molti vettori di attacco amministrativi.

- Crittografia supportata

    Un minore livello di protezione che comunque offre i vantaggi di conformità di essere in grado di crittografare una macchina virtuale, ma consente agli amministratori di Hyper-V eseguire le operazioni utilizzare connessione alla console della macchina virtuale e PowerShell Direct. 

    ![Nuova crittografia supportate della macchina virtuale](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-shielded-vm.png)

È possibile aggiungere parti di gestione facoltative, ad esempio VMM o Windows Azure Pack. Se si desidera creare una macchina virtuale senza installare tali componenti, vedere [procedura dettagliata: creazione di macchine virtuali schermati senza VMM](https://blogs.technet.microsoft.com/datacentersecurity/2016/06/06/step-by-step-creating-shielded-vms-without-vmm/).

## <a name="step-6-create-a-shielded-vm"></a>Passaggio 6: Creare una macchina virtuale schermata

Creazione di macchine virtuali schermate differisce molto poco da macchine virtuali normali. In Windows Azure Pack, l'esperienza è ancora più semplice rispetto alla creazione di una normale VM perché è necessario solo fornire un nome, la schermatura, che include il resto delle informazioni di specializzazione, file di dati e la rete VM. 

![Nuova macchina virtuale schermata in Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-vm-config.png)

## <a name="next-step"></a>Passaggio successivo

>[!div class="nextstepaction"]
[Prerequisiti HGS](guarded-fabric-prepare-for-hgs.md)
