---
title: Infrastruttura sorvegliata e guida alla pianificazione delle VM schermate per i provider di hosting
ms.prod: windows-server
ms.topic: article
ms.assetid: 392af37f-a02d-4d40-a25d-384211cbbfdd
manager: dongill
author: nirb-ms
ms.author: nirb
ms.technology: security-guarded-fabric
ms.openlocfilehash: 829d6a3efef082e35c6a4f98e0ba9e4b70c27a93
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856474"
---
# <a name="guarded-fabric-and-shielded-vm-planning-guide-for-tenants"></a>Guida alla pianificazione dell'infrastruttura sorvegliata e della VM schermata per i tenant

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Questo argomento è incentrato sui proprietari di VM che vogliono proteggere le macchine virtuali (VM) per finalità di conformità e sicurezza. Indipendentemente dal fatto che le macchine virtuali vengano eseguite nell'infrastruttura sorvegliata di un provider di hosting o in un'infrastruttura sorvegliata privata, i proprietari delle macchine virtuali devono controllare il livello di sicurezza delle macchine virtuali schermate, che include la possibilità di decrittografarle, se necessario.

Quando si usano macchine virtuali schermate, è necessario prendere in considerazione tre aree:

- Livello di sicurezza per le macchine virtuali
- Chiavi crittografiche usate per proteggerle
- Schermatura dei dati: informazioni riservate usate per creare VM schermate 

## <a name="security-level-for-the-vms"></a>Livello di sicurezza per le macchine virtuali

Quando si distribuiscono VM schermate, è necessario selezionare uno dei due livelli di sicurezza seguenti:

- Schermate 
- Crittografia supportata

Sia le VM schermate che quelle supportate dalla crittografia hanno un TPM virtuale collegato e quelle che eseguono Windows sono protette da BitLocker. La differenza principale consiste nel fatto che le macchine virtuali schermate bloccano l'accesso da parte degli amministratori dell'infrastruttura mentre le macchine virtuali supportate dalla crittografia consentono agli amministratori dell'infrastruttura lo stesso livello di accesso di una normale macchina virtuale. Per altre informazioni su queste differenze, vedere [Panoramica dell'infrastruttura sorvegliata e delle macchine virtuali schermate](guarded-fabric-and-shielded-vms.md). 

Scegliere **macchine virtuali schermate** se si vuole proteggere la macchina virtuale da un'infrastruttura compromessa, inclusi gli amministratori compromessi. Devono essere usati negli ambienti in cui gli amministratori dell'infrastruttura e l'infrastruttura stessa non sono attendibili. Scegliere **macchine virtuali supportate** per la crittografia se si vuole rispettare una barra di conformità che potrebbe richiedere sia la crittografia inattiva sia la crittografia della macchina virtuale in transito, ad esempio durante la migrazione in tempo reale.

Le macchine virtuali supportate dalla crittografia sono ideali negli ambienti in cui gli amministratori dell'infrastruttura sono completamente attendibili, ma la crittografia rimane un requisito.

È possibile eseguire una combinazione di VM normali, VM schermate e macchine virtuali supportate dalla crittografia in un'infrastruttura sorvegliata e anche nello stesso host Hyper-V. 

Se una macchina virtuale è schermata o se la crittografia supportata è determinata dai dati di schermatura selezionati durante la creazione della macchina virtuale. I proprietari delle macchine virtuali configurano il livello di sicurezza quando si creano i dati di schermatura. vedere la sezione relativa ai [dati di schermatura](#shielding-data) .
Si noti che una volta effettuata questa scelta, non è possibile modificarla mentre la macchina virtuale rimane nell'infrastruttura di virtualizzazione.

## <a name="cryptographic-keys-used-for-shielded-vms"></a>Chiavi crittografiche usate per le macchine virtuali schermate

Le macchine virtuali schermate sono protette da vettori di attacco dell'infrastruttura di virtualizzazione usando dischi crittografati e vari altri elementi crittografati che possono essere decrittografati solo da:

- Chiave del proprietario: chiave crittografica gestita dal proprietario della macchina virtuale che viene in genere usata per il ripristino o la risoluzione dei problemi dell'ultima risorsa. I proprietari delle macchine virtuali sono responsabili della gestione delle chiavi del proprietario in una posizione sicura.
- Uno o più tutori (chiavi sorveglianza host): ogni tutore rappresenta un'infrastruttura di virtualizzazione in cui un proprietario autorizza le macchine virtuali schermate per l'esecuzione. Le aziende spesso dispongono di un'infrastruttura di virtualizzazione primaria e di ripristino di emergenza e in genere autorizzano le macchine virtuali schermate per l'esecuzione in entrambi. In alcuni casi, l'infrastruttura secondaria (DR) potrebbe essere ospitata da un provider di cloud pubblico. Le chiavi private per qualsiasi infrastruttura sorvegliata vengono gestite solo nell'infrastruttura di virtualizzazione, mentre le chiavi pubbliche possono essere scaricate e sono contenute all'interno del suo tutore. 

**Ricerca per categorie creare una chiave del proprietario?** Una chiave del proprietario è rappresentata da due certificati. Un certificato per la crittografia e un certificato per la firma. È possibile creare questi due certificati usando l'infrastruttura PKI o ottenere i certificati SSL da un'autorità di certificazione pubblica (CA). A scopo di test, è anche possibile creare un certificato autofirmato in qualsiasi computer a partire da Windows 10 o Windows Server 2016.

**Quante chiavi del proprietario è necessario avere?** È possibile usare una singola chiave del proprietario o più chiavi del proprietario. Procedure consigliate consiglia una singola chiave del proprietario per un gruppo di macchine virtuali che condividono lo stesso livello di sicurezza, attendibilità o rischio e per il controllo amministrativo. È possibile condividere una singola chiave del proprietario per tutte le macchine virtuali schermate appartenenti a un dominio e depositare la chiave del proprietario per la gestione da parte degli amministratori di dominio.

**Posso usare chiavi personali per il sorvegliante host?** Sì, è possibile usare la chiave "Bring Your Own" per il provider di hosting e usarla per le macchine virtuali schermate. In questo modo è possibile usare le chiavi specifiche (rispetto all'uso della chiave del provider di hosting) e possono essere usate quando si dispone di una sicurezza o di normative specifiche che è necessario rispettare. Per motivi di igiene chiave, le chiavi di sorveglianza host devono essere diverse dalla chiave del proprietario.

## <a name="shielding-data"></a>Schermatura dei dati

I dati di schermatura contengono i segreti necessari per distribuire macchine virtuali schermate o supportate dalla crittografia. Viene usato anche per la conversione di VM normali in VM schermate.

I dati di schermatura vengono creati usando la creazione guidata file di dati di schermatura e vengono archiviati nei file PDK che i proprietari delle macchine virtuali caricano nell'infrastruttura sorvegliata.

Le macchine virtuali schermate proteggono da attacchi provenienti da un'infrastruttura di virtualizzazione compromessa, quindi è necessario un meccanismo sicuro per passare dati di inizializzazione sensibili, ad esempio la password dell'amministratore, le credenziali di aggiunta al dominio o i certificati RDP, senza rivelarli all'infrastruttura di virtualizzazione o agli amministratori. Inoltre, i dati di schermatura contengono gli elementi seguenti:

1. Livello di sicurezza: supportato con schermate o con crittografia
2. Proprietario ed elenco di sorveglianza host attendibili in cui è possibile eseguire la macchina virtuale
3. Dati di inizializzazione della macchina virtuale (Unattend. XML, certificato RDP)
4. Elenco di dischi modello firmati attendibili per la creazione della macchina virtuale nell'ambiente di virtualizzazione 

Quando si crea una macchina virtuale schermata o supportata dalla crittografia o si converte una macchina virtuale esistente, verrà richiesto di selezionare i dati di schermatura anziché richiedere le informazioni riservate.

**Quanti file di dati di schermatura sono necessari?** È possibile usare un singolo file di dati di schermatura per creare ogni macchina virtuale schermata. Se, tuttavia, una determinata VM schermata richiede che uno dei quattro elementi sia diverso, è necessario un file di dati di schermatura aggiuntivo. Ad esempio, si potrebbe avere un file di dati di schermatura per il reparto IT e un file di dati di schermatura diverso per il reparto risorse umane perché la password di amministratore iniziale e i certificati RDP sono diversi.

L'uso di file di dati di schermatura separati per ogni macchina virtuale schermata è possibile, non è necessariamente la scelta ottimale e deve essere eseguita per i motivi appropriati. Ad esempio, se ogni macchina virtuale schermata deve avere una password di amministratore diversa, prendere in considerazione l'uso di un servizio di gestione delle password o di uno strumento come la [soluzione di password di amministratore locale di Microsoft (giri)](https://www.microsoft.com/download/details.aspx?id=46899).

## <a name="creating-a-shielded-vm-on-a-virtualization-fabric"></a>Creazione di una VM schermata in un'infrastruttura di virtualizzazione

Sono disponibili diverse opzioni per la creazione di una macchina virtuale schermata in un'infrastruttura di virtualizzazione. di seguito sono riportate le seguenti operazioni per le VM schermate e supportate da crittografia:

1. Creare una VM schermata nell'ambiente e caricarla nell'infrastruttura di virtualizzazione
2. Creare una nuova macchina virtuale schermata da un modello firmato nell'infrastruttura di virtualizzazione
3. Schermare una macchina virtuale esistente (la macchina virtuale esistente deve essere di seconda generazione e deve essere in esecuzione Windows Server 2012 o versione successiva)

La creazione di nuove macchine virtuali da un modello è una procedura normale. Tuttavia, poiché il disco modello usato per creare una nuova macchina virtuale schermata risiede nell'infrastruttura di virtualizzazione, sono necessarie misure aggiuntive per assicurarsi che non sia stata manomessa da un amministratore di infrastruttura malintenzionato o da malware in esecuzione nell'infrastruttura. Questo problema viene risolto usando i dischi modello firmati: i dischi modello firmati e le relative firme del disco vengono creati da amministratori attendibili o dal proprietario della macchina virtuale. Quando viene creata una macchina virtuale schermata, la firma del disco del modello viene confrontata con le firme contenute nel file di dati di schermatura specificato. Se una delle firme del file di dati di schermatura corrisponde alla firma del disco del modello, il processo di distribuzione continua. Se non viene trovata alcuna corrispondenza, il processo di distribuzione viene interrotto, assicurando che i segreti delle VM non saranno compromessi a causa di un disco modello non attendibile.

Quando si usano dischi modello firmati per creare VM schermate, sono disponibili due opzioni:

1. Usare un disco modello firmato esistente fornito dal provider di virtualizzazione. In questo caso, il provider di virtualizzazione mantiene i dischi modello firmati.
2. Caricare un disco modello firmato nell'infrastruttura di virtualizzazione. Il proprietario della macchina virtuale è responsabile della manutenzione dei dischi modello firmati. 


