---
title: Funzionalità rimosse o pianificata la sostituzione a partire da Windows Server, versione 1903
description: Di seguito è riportato un elenco delle funzionalità e funzionalità di Windows Server, versione 1903 che sono state rimosse dal prodotto in cui rilasciare o cominciano a essere considerati per la sostituzione potenziali nelle versioni successive. Questo elenco è destinato ai professionisti IT responsabili dell'aggiornamento dei sistemi operativi in un ambiente commerciale.
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.date: 05/21/2019
author: jasongerend
ms.author: jgerend
manager: daveba
ms.openlocfilehash: e2f51af55ba7005cb20d8a1c22f6ba9edc20c704
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/21/2019
ms.locfileid: "65983421"
---
# <a name="features-removed-or-planned-for-replacement-starting-with-windows-server-version-1903"></a>Funzionalità rimosse o pianificata la sostituzione a partire da Windows Server, versione 1903

>Si applica a: Windows Server, versione 1903

Di seguito è riportato un elenco delle funzionalità e funzionalità di Windows Server, versione 1903 che sono state rimosse dal prodotto in cui rilasciare o cominciano a essere considerati per la sostituzione potenziali nelle versioni successive. Questo elenco è destinato ai professionisti IT responsabili dell'aggiornamento dei sistemi operativi in un ambiente commerciale. **Questo elenco è soggetto a modifiche nelle versioni successive e potrebbe non includere tutte le funzionalità interessate o funzionalità.**

Vedere anche [funzionalità rimosse o pianificata la sostituzione di avvio di Windows Server 2019](removed-features-19.md).

## <a name="features-were-no-longer-developing"></a>Funzionalità di cui è cessato lo sviluppo

Si sviluppano queste funzionalità non è più attivamente e possibile rimuoverli da un aggiornamento futuro. Alcune sono state sostituite con altre caratteristiche o funzionalità, mentre altre sono ora disponibili da origini differenti. 

Se hai commenti e suggerimenti da condividere sulla sostituzione proposta di una di queste funzionalità, puoi utilizzare l'[app Hub di Feedback](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). 

| Funzionalità | È invece possibile usare |
|-----------|---------------------|
|WEP Wi-Fi e a TKIP (**nuovo**)| Le reti Wi-Fi con crittografie precedenti meno avanzate WEP e a TKIP non sono protette a quelli con AES, ad esempio WPA3 e WPA2. In Windows 10, versione 1903, ci si connette a una rete WEP o TKIP mostrerà un messaggio di avviso che la rete non è sicura, ma nessun messaggio viene visualizzato in Windows Server, versione 1903. In una versione futura qualsiasi connessione a una rete Wi-Fi usando queste crittografie precedente non sarà consentita. Per altre informazioni sui rischi di sicurezza di WEP e TKIP, vedere questo [post di blog](https://go.microsoft.com/fwlink/p/?linkid=2008426).|
|Driver basati su XDDM visualizzazione remota (**nuovo**)|Inizia con questa versione Servizi Desktop remoto viene utilizzato un Windows Display Driver Model (WDDM) in base indiretta Display Driver (IDD) per il desktop remoto di una singola sessione. Il supporto per Windows 2000 Display Driver Model () basato su visualizzazione remota driver XDDM verrà rimosso in una versione futura. I fornitori di Software indipendenti che usano il driver basati su XDDM visualizzazione remota deve pianificare una migrazione per il modello di driver WDDM. Per altre informazioni sull'implementazione di visualizzazione remota driver video indiretta di fornitori di software indipendenti possono contattare [ rdsdev@microsoft.com ](mailto:rdsdev@microsoft.com).|
|Lo strumento di raccolta log UCS (**nuovo**)|Lo strumento di raccolta log UCS, anche se non esplicitamente destinato all'uso con Windows Server, devono comunque essere verrà sostituito dall'hub di commenti e suggerimenti su Windows 10.|
|Unità di archiviazione della chiave in Hyper-V|Non è più lavoriamo sulla funzionalità di unità di archiviazione della chiave in Hyper-V. Se si usano macchine virtuali di generazione 1, consultare [sicurezza di virtualizzazione della macchina virtuale di generazione 1](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v) per informazioni sulle opzioni in futuro. Se si creano nuove macchine virtuali usano macchine virtuali di generazione 2 con i dispositivi TPM per una soluzione più sicura. |
|Console di gestione Trusted Platform Module (TPM)|Le informazioni disponibili in precedenza nella console di gestione TPM sono ora disponibile nel [ **sicurezza del dispositivo** ](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security) nella pagina il [Windows Defender Security Center](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center).|
|Modalità di attestazione di Active Directory del servizio sorveglianza host|Stiamo sviluppando non è più modalità di attestazione di Active Directory del servizio sorveglianza Host, invece è stata aggiunta una nuova modalità di attestazione [ospitare l'attestazione chiave](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md), che è molto più semplice e ugualmente come compatibile come basata su Active Directory attestazione.  Questa nuova modalità fornisce una funzionalità equivalente con un'esperienza di installazione, gestione più semplice e meno dipendenze dell'infrastruttura di attestazione dell'Active Directory. Attestazione chiave host non esistono requisiti hardware aggiuntivi oltre l'attestazione quali Active Directory necessari, in modo che tutti i sistemi esistenti rimarranno compatibili con la nuova modalità. Visualizzare [distribuire host sorvegliati](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md) per altre informazioni sulle opzioni di attestazione.|
|Servizio OneSync|Il servizio OneSync Sincronizza i dati per le app di posta elettronica, calendario e le persone. È stato aggiunto un motore di sincronizzazione per l'app Outlook che fornisce la sincronizzazione stesso.|
|Supporto dell'API di compressione differenziale remoto|Supporto dell'API di compressione differenziale remoto è abilitata la sincronizzazione dei dati con un'origine remota usando tecnologie di compressione, che è ridotto al minimo la quantità di dati inviati attraverso la rete. Questo supporto non è attualmente usato da qualsiasi prodotto Microsoft.|
|Estensione del commutatore piattaforma filtro Windows filtro leggeri|L'estensione del commutatore piattaforma filtro Windows filtro leggero consente agli sviluppatori di compilare [pacchetto di rete semplice le estensioni del commutatore virtuale Hyper-V di filtro](https://docs.microsoft.com/en-us/windows-hardware/drivers/network/using-virtual-switch-filtering). È possibile ottenere le stesse funzionalità tramite la creazione di un'estensione di filtro completa. Di conseguenza, verrà rimosso questa estensione in futuro.|
