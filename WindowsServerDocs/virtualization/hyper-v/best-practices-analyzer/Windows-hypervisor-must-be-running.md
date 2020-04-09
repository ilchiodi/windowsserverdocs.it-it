---
title: Hypervisor di Windows deve essere in esecuzione
description: Vengono fornite istruzioni per risolvere il problema segnalato da questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 501a9beb-c464-46c0-88c5-e3e7e3e70101
author: kbdazure
ms.date: 10/03/2016
ms.openlocfilehash: b24700e0ed617177af888013e36f971870d0ac59
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860954"
---
# <a name="windows-hypervisor-must-be-running"></a>Hypervisor di Windows deve essere in esecuzione

>Si applica a: Windows Server 2016
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Prerequisiti|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*Windows Hypervisor non è in esecuzione.*  
  
## <a name="impact"></a>Impatto  
  
*Le macchine virtuali non possono essere avviate fino a quando Windows Hypervisor non è in esecuzione.*  
  
## <a name="resolution"></a>Risoluzione  
  
*Controllare il catalogo di Windows Server per verificare se il server è qualificato per l'esecuzione di Hyper-V. Verificare quindi che il BIOS sia abilitato per la virtualizzazione assistita mediante hardware e la prevenzione dell'esecuzione dei dati applicata dall'hardware. Quindi, controllare il registro eventi di hypervisor Hyper-V.*  
  
Per controllare il catalogo, vedere il [Catalogo di Windows Server](https://go.microsoft.com/fwlink/?LinkId=111228) (https://go.microsoft.com/fwlink/?LinkId=111228).  
  
> [!CAUTION]  
> Modificare alcuni parametri nel BIOS di sistema di un computer può causare il computer per arrestare il caricamento del sistema operativo o rendere i dispositivi hardware, ad esempio unità disco rigido, non disponibile. Sempre, consultare il manuale dell'utente per il computer per determinare la modalità appropriata per configurare il BIOS di sistema. Inoltre, è sempre una buona idea per tenere traccia dei parametri che si modifica e il valore originale in modo che sia possibile ripristinarle in un secondo momento se necessario. Se si verificano problemi dopo la modifica dei parametri nel BIOS di sistema, provare a caricare le impostazioni predefinite (un'opzione viene in genere è disponibile nell'utilità di configurazione del BIOS) o per assistenza, contattare il produttore del computer.  
  
#### <a name="to-verify-virtualization-support-in-the-bios-or-uefi"></a>Per verificare il supporto di virtualizzazione nel BIOS o UEFI  
  
1.  Riavviare il computer e accedere al BIOS o UEFI tramite lo strumento di configurazione. Accesso a questo strumento è disponibile in genere quando il computer passa attraverso un processo di avvio. Immediatamente dopo avere attivato la maggior parte dei computer, viene visualizzato un messaggio per alcuni secondi che elenca la chiave o una combinazione di tasti da premere per aprire lo strumento di configurazione.  
  
2.  Trovare le impostazioni per la virtualizzazione e applicata dall'hardware dati esecuzione programmi (DEP) e verificare che si trovano in. Di seguito sono posizioni di menu comuni per queste impostazioni, lo strumento di configurazione ed esempi di ciò che può essere denominati:  
  
    -   Supporto della virtualizzazione:  
  
        -   In genere disponibile in base alle impostazioni per il processore principale o prestazioni. Talvolta è sotto le impostazioni di sicurezza.  
  
        -   Cercare i nomi dei parametri che includono "virtualizzazione" o "la tecnologia di virtualizzazione".  
  
    -   Protezione esecuzione Programmi applicata dall'hardware:  
  
        -   In genere è disponibile con le impostazioni di sicurezza o di memoria.  
  
        -   Cercare i nomi dei parametri che includono "esecuzione", "Esegui" o "programmi".  
  
3.  Se necessario, attivare le impostazioni seguendo le istruzioni nello strumento di configurazione. Salvare le modifiche e uscire.  
  
4.  Se sono state apportate modifiche, spegnere l'alimentazione e quindi eseguire di nuovo per terminare.  
  
    > [!IMPORTANT]  
    > Poiché le modifiche non vengono applicate alcuni computer finché non si verifica, è consigliabile che spegnere e riaccendere (talvolta chiamato un ciclo di alimentazione).  
  
Successivamente, controllare il registro eventi di Hypervisor Hyper-V. Se vi sono problemi, si controllerà inoltre il Registro di sistema.  
  
#### <a name="to-check-the-event-logs"></a>Per controllare i registri eventi  
  
1.  Aprire Visualizzatore eventi. Fare clic su **avviare**, fare clic su **Strumenti di amministrazione**, quindi fare clic su **Visualizzatore eventi**.  
  
2.  Aprire il registro eventi di Hypervisor Hyper-V. Nel riquadro di spostamento, espandere **registri applicazioni e servizi** >> **Microsoft** >> **Windows** >> **Hypervisor Hyper-V**, quindi fare clic su **operativo**.  
  
3.  Se è in esecuzione Windows hypervisor, è richiesta alcuna azione ulteriore. Se non è in esecuzione Windows hypervisor, eseguire questa operazione:  
  
4.  Aprire il Registro di sistema. (Nel riquadro di spostamento, espandere **I registri di Windows** e quindi selezionare **sistema**.)  
  
5.  Utilizzare un filtro per trovare gli eventi di Hypervisor Hyper-V:   
    1. Nel **azioni** riquadro, fare clic su **Filtro registro corrente**. Per **origini evento**, specificare "Hypervisor Hyper-V".   
    2. Cercare eventi che indicano problemi. Ad esempio, l'evento con ID 41 indica un problema con la configurazione del BIOS: "avvio di Hyper-V non è riuscita. Entrambi VMX non è presente o non è abilitato nel BIOS."  
  
### <a name="see-also"></a>Vedi anche  
Per informazioni dettagliate sull'utilizzo di Hyper-V in Windows 10, tra cui come verificare che il computer può eseguire Hyper-V, vedere [requisiti di sistema di Windows 10 Hyper-V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_compatibility). 


