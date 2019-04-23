---
title: Infrastruttura sorvegliata e macchine Virtuali schermate Guida alla pianificazione di hosting
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 392af37f-a02d-4d40-a25d-384211cbbfdd
manager: dongill
author: nirb-ms
ms.technology: security-guarded-fabric
ms.openlocfilehash: 0a43cedf8ce0138e89624ff3df5bc7088f583252
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875302"
---
# <a name="guarded-fabric-and-shielded-vm-planning-guide-for-tenants"></a>Infrastruttura sorvegliata e macchine Virtuali schermate Guida alla pianificazione per i tenant

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

In questo argomento è incentrato sui proprietari della macchina virtuale che vogliono proteggere le macchine virtuali (VM) per scopi di sicurezza e conformità. Indipendentemente dal fatto che le macchine virtuali eseguono su infrastruttura sorvegliata dei provider di hosting o di un'infrastruttura sorvegliata privata, i proprietari della macchina virtuale necessario controllare il livello di sicurezza delle proprie macchine virtuali schermate, che include la possibilità di decrittografare i file, se necessario.

Esistono tre aree da considerare quando usando macchine virtuali schermate:

- Il livello di sicurezza per le macchine virtuali
- Le chiavi di crittografia utilizzate per la protezione
- I dati di schermatura, le informazioni riservate usate per creare macchine virtuali schermate 

## <a name="security-level-for-the-vms"></a>Livello di protezione per le macchine virtuali

Durante la distribuzione di macchine virtuali schermate, è necessario selezionare uno dei due livelli di sicurezza:

- Schermate 
- Crittografia supportata

Entrambi schermate e macchine virtuali supportate da crittografia dispongono di un TPM virtuale collegato a essi e quelli che l'esecuzione di Windows sono protette da BitLocker. La differenza principale è che schermato macchine virtuali di bloccare l'accesso per gli amministratori dell'infrastruttura mentre macchine virtuali supportate da crittografia consentono gli amministratori dell'infrastruttura lo stesso livello di accesso come dovranno essere una normale VM. Per altre informazioni su queste differenze, vedere [sorvegliati fabric e panoramica di macchine virtuali schermate](guarded-fabric-and-shielded-vms.md). 

Scegli **macchine virtuali schermati** se si sta cercando di proteggere la macchina virtuale da un'infrastruttura compromessa (inclusi amministratori compromessi). Devono essere utilizzati in ambienti in cui gli amministratori dell'infrastruttura e dell'infrastruttura a se stesso non sono attendibili. Scegli **macchine virtuali supportate da crittografia** se si intende per soddisfare una barra di conformità che potrebbe richiedere la crittografia dei dati inattivi sia la crittografia della macchina virtuale in transito (ad esempio, durante la migrazione in tempo reale).

Supporto della crittografia le macchine virtuali sono ideali in ambienti in cui gli amministratori dell'infrastruttura sono completamente attendibili ma crittografia resta un requisito.

È possibile eseguire una combinazione di macchine virtuali supportate da crittografia normali macchine virtuali e macchine virtuali schermate in un'infrastruttura sorvegliata e persino nello stesso host Hyper-V. 

Se una macchina virtuale è schermata o supporto della crittografia è determinato in base ai dati di schermatura che sia selezionati quando si crea la macchina virtuale. I proprietari della macchina virtuale configurare il livello di sicurezza quando si crea i dati di schermatura (vedere la [dati di schermatura](#shielding-data) sezione).
Si noti che dopo che è stata effettuata questa scelta, non può essere modificato mentre la macchina virtuale rimane nell'infrastruttura di virtualizzazione.

## <a name="cryptographic-keys-used-for-shielded-vms"></a>Chiavi crittografiche usate per le macchine virtuali schermate

Le macchine virtuali schermate sono protette da virtualizzazione fabric vettori di attacco con dischi crittografati e vari altri elementi crittografati che possono essere decrittografati solo da:

- Una chiave del proprietario questa è una chiave di crittografia gestita dal proprietario della macchina virtuale che è in genere usato per il ripristino di emergenza o la risoluzione dei problemi. I proprietari della macchina virtuale sono responsabili della gestione delle chiavi proprietario in un luogo sicuro.
- Uno o più Guardians (chiavi di sorveglianza Host): ogni sorveglianza rappresenta un'infrastruttura di virtualizzazione in cui un proprietario autorizza le macchine virtuali schermate per l'esecuzione. Le aziende spesso dispongono di una replica primaria e un'infrastruttura di virtualizzazione disaster recovery (ripristino di emergenza) e vengono in genere autorizzare le proprie macchine virtuali schermate per l'esecuzione in entrambi. In alcuni casi, l'infrastruttura (DR) secondario potrebbe essere ospitato da un provider di cloud pubblico. Le chiavi private per qualsiasi infrastruttura sorvegliata vengono mantenute solo nell'infrastruttura di virtualizzazione, mentre le chiavi pubbliche possono essere scaricati e sono contenuti all'interno di relativo sorveglianza. 

**Come si crea una chiave del proprietario?** Una chiave del proprietario è rappresentata da due certificati. Un certificato per la crittografia e un certificato per la firma. È possibile creare questi due certificati mediante la propria infrastruttura PKI oppure ottenere i certificati SSL da un'autorità di certificazione pubblica (CA). Per scopi di test, è anche possibile creare un certificato autofirmato su inizio qualsiasi computer con Windows 10 o Windows Server 2016.

**Numero di chiavi del proprietario devono usare?** È possibile usare una chiave proprietario singolo o più chiavi di proprietario. Una chiave proprietario singola procedura consigliata, per un gruppo di macchine virtuali che condividono la stessa sicurezza, a livello di attendibilità o dei rischi e per il controllo amministrativo. È possibile condividere una chiave del singolo proprietario per tutte le dominio le macchine virtuali schermate e depositare la chiave di proprietario per essere gestiti dagli amministratori di dominio.

**È possibile usare le proprie chiavi per sorveglianza Host?** Sì, è possibile "Bring Your Own" chiave all'hosting provider e all'utilizzo di chiavi per le macchine virtuali schermate. In questo modo è possibile usare le chiavi specifiche (confronto usando la chiave del provider di hosting) e può essere usato quando si dispone di sicurezza specifico o alle normative che devi rispettare. Per motivi di igiene chiave, le chiavi di sorveglianza Host devono essere diverse da quello della chiave del proprietario.

## <a name="shielding-data"></a>Dati di schermatura

I dati di schermatura contengano i segreti necessari per distribuire le macchine virtuali schermate o supporto della crittografia. Viene inoltre utilizzato durante la conversione dei normali macchine virtuali a macchine virtuali schermate.

I dati di schermatura, viene creato utilizzando la procedura guidata File di dati di schermatura e viene archiviato in file con estensione PDK che carica i proprietari della macchina virtuale per l'infrastruttura sorvegliata.

Le macchine virtuali schermate proteggersi da attacchi da un'infrastruttura di virtualizzazione compromesso, pertanto è necessario un meccanismo sicuro per passare dati di inizializzazione sensibili, ad esempio password, credenziali di aggiunta al dominio o i certificati RDP, l'amministratore senza rivelare al l'infrastruttura di virtualizzazione stesso o per gli amministratori. Inoltre, i dati di schermatura contiene quanto segue:

1. Sicurezza di livello – schermate o supporto della crittografia
2. Proprietario e l'elenco dei Sorveglianti di Host attendibili in cui è possibile eseguire la macchina virtuale
3. Dati di inizializzazione di macchina virtuale (Unattend. XML, certificato RDP)
4. Elenco di dischi modello firmati trusted per la creazione della macchina virtuale nell'ambiente di virtualizzazione 

Quando la creazione di una VM schermata o supporto della crittografia o la conversione di una macchina virtuale esistente, verrà richiesto di selezionare i dati di schermatura invece che venga richiesto per le informazioni riservate.

**Il numero dei file di dati di schermatura è necessario?** Un singolo file di dati di schermatura è utilizzabile per creare ogni macchina virtuale schermata. Se, tuttavia, una determinata macchina virtuale schermata richiede che uno qualsiasi dei quattro elementi siano diversi, quindi è necessario un file di dati di schermatura aggiuntive. Ad esempio, potrebbe essere un file di dati di schermatura per il reparto IT e un diverso file di dati di schermatura per il reparto risorse Umane in quanto la password amministratore iniziale e i certificati RDP è diverso.

Anche usando i file di dati di schermatura separato per ogni macchina virtuale schermata è possibile, non è necessariamente la scelta ottimale e deve essere effettuata per i giusti motivi. Ad esempio, se tutte le macchine Virtuali schermate devono avere una password di amministratore diverse, è consigliabile invece utilizzando una gestione delle password di servizio o strumento, ad esempio [locale amministratore Password soluzione (LAP di Microsoft)](https://www.microsoft.com/en-us/download/details.aspx?id=46899).

## <a name="creating-a-shielded-vm-on-a-virtualization-fabric"></a>Creazione di una macchina virtuale schermata in un'infrastruttura di virtualizzazione

Esistono diverse opzioni per la creazione di una macchina virtuale schermata su un'infrastruttura di virtualizzazione (di seguito sono rilevante per le macchine virtuali schermate e supporto della crittografia):

1. Creare una macchina virtuale schermata nell'ambiente in uso e caricarlo per l'infrastruttura di virtualizzazione
2. Creare una nuova VM schermata da un modello firmati nell'infrastruttura di virtualizzazione
3. Shield, una macchina virtuale esistente (la macchina virtuale esistente generazione 2 è necessario e deve essere in esecuzione Windows Server 2012 o versioni successive)

Creazione di nuove macchine virtuali da un modello è normale pratica. Tuttavia, poiché si trova il disco modello utilizzato per creare nuove VM schermate nell'infrastruttura di virtualizzazione, misure aggiuntive sono necessarie per garantire che che non sia stato manomesso da un amministratore di infrastrutture dannose o da malware in esecuzione nell'infrastruttura. Questo problema viene risolto utilizzando dischi modello firmati, ovvero dischi modello firmati e le relative firme disco vengono create dagli amministratori attendibili o il proprietario della macchina virtuale. Quando viene creata una macchina virtuale schermata, firma del disco modello viene confrontata con le firme contenute all'interno del file di dati di schermatura specificato. Se una delle firme del file di dati di schermatura corrisponde la firma del disco modello, il processo di distribuzione continua. Se nessuna corrispondenza è reperibile, il processo di distribuzione viene interrotta, assicurando che i segreti della macchina virtuale non venga compromessa a causa di un disco modello non attendibili.

Durante l'uso di dischi modello firmati per creare macchine virtuali schermate, sono disponibili due opzioni:

1. Usare un disco modello firmato esistente che viene fornito dal provider di virtualizzazione. In questo caso, il provider di virtualizzazione conserva dischi modello firmati.
2. Caricare un disco modello firmato per l'infrastruttura di virtualizzazione. Il proprietario della macchina virtuale è responsabile della gestione di dischi modello firmati. 


