---
title: Ripristino della foresta di Active Directory-esecuzione del ripristino iniziale
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: ca942691a88465061ebbde2b78314844a694fbf2
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868200"
---
# <a name="perform-initial-recovery"></a>Eseguire il ripristino iniziale  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Questa sezione include i passaggi seguenti:  

- [Ripristinare il primo controller di dominio scrivibile in ogni dominio](#restore-the-first-writeable-domain-controller-in-each-domain)  
- [Riconnettere ogni controller di dominio scrivibile ripristinato alla rete](#reconnect-each-restored-writeable-domain-controller-to-a-common-network)  
- [Aggiungere il catalogo globale a un controller di dominio nel dominio radice della foresta](#add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain)  

## <a name="restore-the-first-writeable-domain-controller-in-each-domain"></a>Ripristinare il primo controller di dominio scrivibile in ogni dominio  

A partire da un controller di dominio scrivibile nel dominio radice della foresta, completare i passaggi descritti in questa sezione per ripristinare il primo controller di dominio. Il dominio radice della foresta è importante perché archivia i gruppi Schema Admins ed Enterprise Admins. Consente inoltre di mantenere la gerarchia di trust nell'insieme di strutture. Il dominio radice della foresta, inoltre, contiene in genere il server radice DNS per lo spazio dei nomi DNS della foresta. Di conseguenza, la zona DNS integrata Active Directory per quel dominio contiene i record di risorse alias (CNAME) per tutti gli altri controller di dominio nella foresta (richiesti per la replica) e i record di risorse DNS del catalogo globale. 
  
Al termine del ripristino del dominio radice della foresta, ripetere gli stessi passaggi per ripristinare i domini rimanenti nella foresta. È possibile ripristinare più di un dominio simultaneamente; Tuttavia, ripristinare sempre un dominio padre prima di recuperare un elemento figlio per evitare interruzioni nella gerarchia di trust o nella risoluzione dei nomi DNS. 
  
Per ogni dominio ripristinato, ripristinare un solo controller di dominio scrivibile da un backup. Questa è la parte più importante del ripristino perché il controller di dominio deve avere un database che non è stato influenzato da ciò che ha causato la mancata riuscita della foresta. È importante disporre di un backup attendibile testato accuratamente prima di essere introdotto nell'ambiente di produzione. 
  
Quindi, eseguire i passaggi seguenti. Le procedure per l'esecuzione di alcuni passaggi sono [le procedure per il ripristino della foresta Active Directory](AD-Forest-Recovery-Procedures.md). 
  
1. Se si prevede di ripristinare un server fisico, verificare che il cavo di rete del controller di dominio di destinazione non sia collegato e che quindi non sia connesso alla rete di produzione. Per una macchina virtuale, è possibile rimuovere la scheda di rete o usare una scheda di rete collegata a un'altra rete in cui è possibile testare il processo di ripristino mentre è isolato dalla rete di produzione. 
  
2. Poiché si tratta del primo controller di dominio scrivibile nel dominio, è necessario eseguire un ripristino non autorevole di servizi di dominio Active Directory e un ripristino autorevole di SYSVOL. Per completare l'operazione di ripristino, è necessario usare un'applicazione di backup e ripristino con Active Directory, ad esempio Windows Server Backup, ovvero non è necessario ripristinare il controller di dominio usando metodi non supportati come il ripristino di uno snapshot della macchina virtuale. 
   - È necessario eseguire un ripristino autorevole di SYSVOL perché la replica della cartella replicata SYSVOL deve essere avviata dopo il ripristino da un'emergenza. Tutti i controller di dominio successivi aggiunti nel dominio devono risincronizzare la cartella SYSVOL con una copia della cartella selezionata per essere autorevole prima di poter annunciare la cartella. 

   > [!CAUTION]
   > Eseguire un'operazione di ripristino autorevole (o primaria) di SYSVOL solo per il primo controller di dominio da ripristinare nel dominio radice della foresta. L'esecuzione non corretta delle operazioni di ripristino primarie di SYSVOL su altri controller di dominio comporta conflitti di replica dei dati SYSVOL. 

   - Sono disponibili due opzioni per eseguire un ripristino non autorevole di servizi di dominio Active Directory e un ripristino autorevole di SYSVOL:  
   - Eseguire un ripristino completo del server e quindi forzare una sincronizzazione autorevole di SYSVOL. Per le procedure dettagliate, vedere [esecuzione di un ripristino completo del server](AD-Forest-Recovery-Perform-a-Full-Recovery.md) ed [eseguire una sincronizzazione autorevole di SYSVOL con replica DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md). 
   - Eseguire un ripristino completo del server seguito da un ripristino dello stato del sistema. Per questa opzione è necessario creare in anticipo entrambi i tipi di backup: un backup completo del server e un backup dello stato del sistema. Per le procedure dettagliate, vedere [esecuzione di un ripristino completo del server](AD-Forest-Recovery-Perform-a-Full-Recovery.md) ed [esecuzione di un ripristino non autorevole di Active Directory Domain Services](AD-Forest-Recovery-Nonauthoritative-Restore.md). 
  
3. Dopo aver ripristinato e riavviato il controller di dominio scrivibile, verificare che l'errore non abbia effetto sui dati del controller di dominio. Se i dati del controller di dominio sono danneggiati, ripetere il passaggio 2 con un backup diverso. 
   - Se il controller di dominio ripristinato ospita un ruolo di master operazioni, potrebbe essere necessario aggiungere la seguente voce del registro di sistema per evitare che servizi di dominio Active Directory sia disponibile fino a quando non viene completata la replica di una partizione di directory scrivibile:  

      **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Repl eseguire sincronizzazioni iniziali**  
  
      Creare la voce con il tipo di dati **REG_DWORD** e un valore pari a **0**. Dopo che la foresta è stata ripristinata completamente, è possibile reimpostare il valore di questa voce su **1**, per cui è necessario un controller di dominio che riavvii e contenga i ruoli di master operazioni per la replica in ingresso e in uscita di servizi di dominio Active Directory con la replica nota. partner prima di annunciarsi come controller di dominio e iniziare a fornire servizi ai client. Per ulteriori informazioni sui requisiti di sincronizzazione iniziali, vedere l'articolo della Knowledge [305476](https://support.microsoft.com/kb/305476). 
  
      Continuare con i passaggi successivi solo dopo il ripristino e la verifica dei dati e prima di aggiungere il computer alla rete di produzione. 
  
4. Se si ritiene che l'errore a livello di foresta sia correlato ad intrusioni di rete o attacchi dannosi, reimpostare le password dell'account per tutti gli account amministrativi, inclusi i membri di Enterprise Admins, Domain Admins, Schema Admins, Server Operators, account Gruppi di operatori e così via. È necessario completare il ripristino delle password degli account amministrativi prima di installare altri controller di dominio durante la fase successiva del ripristino della foresta. 
5. Nel primo controller di dominio ripristinato nel dominio radice della foresta, requisire tutti i ruoli di master operazioni a livello di dominio e di foresta. Le credenziali Enterprise Admins e Schema Admins sono necessarie per requisire i ruoli di master operazioni a livello di foresta. 
  
     In ogni dominio figlio, requisire i ruoli di master operazioni a livello di dominio. Sebbene sia possibile mantenere temporaneamente i ruoli di master operazioni sul controller di dominio ripristinato, l'imposizione di questi ruoli garantisce che il controller di dominio li ospiti in questa fase del processo di ripristino della foresta. Come parte del processo di post-ripristino, è possibile ridistribuire i ruoli di master operazioni in base alle esigenze. Per altre informazioni su come requisire i ruoli di master operazioni, vedere [requisizione di un ruolo di master operazioni](AD-forest-recovery-seizing-operations-master-role.md). Per consigli su come collocare i ruoli di master operazioni, vedere informazioni sui [master operazioni](https://technet.microsoft.com/library/cc779716.aspx). 
  
6. Pulire i metadati di tutti gli altri controller di dominio scrivibile nel dominio radice della foresta che non si sta ripristinando dal backup (tutti i controller di dominio scrivibili nel dominio eccetto il primo controller di dominio). Se si utilizza la versione di Active Directory utenti e computer o Active Directory siti e servizi inclusi in Windows Server 2008 o versioni successive o strumenti di amministrazione remota del server per Windows Vista o versioni successive, la pulizia dei metadati viene eseguita automaticamente quando si elimina un oggetto controller di dominio. Inoltre, l'oggetto server e l'oggetto computer per il controller di dominio eliminato vengono eliminati automaticamente. Per ulteriori informazioni, vedere la pagina relativa alla [pulizia dei metadati di controller di dominio scrivibili rimossi](AD-Forest-Recovery-Cleaning-Metadata.md). 
  
     La pulizia dei metadati impedisce la possibile duplicazione di oggetti Impostazioni NTDS se servizi di dominio Active Directory è installato in un controller di dominio in un altro sito. Potenzialmente, questo potrebbe anche salvare il controllo di coerenza informazioni (KCC) il processo di creazione dei collegamenti di replica quando i controller di dominio potrebbero non essere presenti. Inoltre, come parte della pulizia dei metadati, i record di risorse DNS del localizzatore DC per tutti gli altri controller di dominio nel dominio verranno eliminati da DNS. 
  
     Fino a quando non vengono rimossi i metadati di tutti gli altri controller di dominio nel dominio, questo controller di dominio, se si tratta di un master RID prima del ripristino, non presuppone il ruolo di master RID e pertanto non sarà in grado di emettere nuovi RID. È possibile che venga visualizzato l'ID evento 16650 nel registro di sistema Visualizzatore eventi indicante questo errore, ma dovrebbe essere visualizzato l'evento con ID 16648 che indica l'esito positivo di un po' di tempo dopo la pulizia dei metadati. 
  
7. Se si dispone di zone DNS archiviate in servizi di dominio Active Directory, verificare che il servizio server DNS locale sia installato e in esecuzione nel controller di dominio ripristinato. Se il controller di dominio non è un server DNS prima dell'errore della foresta, è necessario installare e configurare il server DNS. 
  
    > [!NOTE]
    > Se il controller di dominio ripristinato esegue Windows Server 2008, è necessario installare l'hotfix nell'articolo della Knowledge base [975654](https://support.microsoft.com/kb/975654) o connettere temporaneamente il server a una rete isolata per poter installare il server DNS. L'hotfix non è necessario per le altre versioni di Windows Server. 
  
     Nel dominio radice della foresta configurare il controller di dominio ripristinato con il proprio indirizzo IP (o un indirizzo di loopback, ad esempio 127.0.0.1) come server DNS preferito. Questa impostazione può essere configurata nelle proprietà TCP/IP della scheda di rete locale (LAN). Si tratta del primo server DNS nella foresta. Per ulteriori informazioni, vedere la pagina relativa [alla configurazione di TCP/IP per l'utilizzo di DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx). 
  
     In ogni dominio figlio configurare il controller di dominio ripristinato con l'indirizzo IP del primo server DNS nel dominio radice della foresta come server DNS preferito. Questa impostazione può essere configurata nelle proprietà TCP/IP della scheda LAN. Per ulteriori informazioni, vedere la pagina relativa [alla configurazione di TCP/IP per l'utilizzo di DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx). 
  
     Nelle zone DNS _msdcs e Domain, eliminare i record NS dei controller di dominio che non esistono più dopo la pulizia dei metadati. Verificare che i record SRV dei controller di dominio puliti siano stati rimossi. Per velocizzare la rimozione del record DNS SRV, eseguire:  
  
    ```  
    nltest.exe /dsderegdns:server.domain.tld  
    ```  
  
8. Aumentare di 100.000 il valore del pool di RID disponibile. Per ulteriori informazioni, vedere [generazione del valore dei pool di RID disponibili](AD-Forest-Recovery-Raise-RID-Pool.md). Se si ritiene che la generazione del pool di RID di 100.000 sia insufficiente per la situazione specifica, è necessario determinare l'incremento più basso che è ancora sicuro usare. RID sono risorse finite che non devono essere utilizzate inutilmente. 
  
     Se nel dominio sono state create nuove entità di sicurezza dopo l'ora del backup utilizzato per il ripristino, è possibile che tali entità di sicurezza dispongano dei diritti di accesso per determinati oggetti. Queste entità di sicurezza non esistono più dopo il ripristino perché il ripristino è stato ripristinato al backup; Tuttavia, i diritti di accesso potrebbero ancora esistere. Se il pool di RID disponibile non viene generato dopo un ripristino, i nuovi oggetti utente creati dopo il ripristino della foresta potrebbero ottenere ID di sicurezza identici (SID) e potrebbero avere accesso a tali oggetti, che non era originariamente previsto. 
  
     Per illustrare, si consideri l'esempio del nuovo dipendente denominato Amy indicato nell'introduzione. L'oggetto utente per Amy non esiste più dopo l'operazione di ripristino perché è stato creato dopo il backup usato per ripristinare il dominio. Tuttavia, eventuali diritti di accesso assegnati a tale oggetto utente potrebbero essere mantenuti dopo l'operazione di ripristino. Se il SID per tale oggetto utente viene riassegnato a un nuovo oggetto dopo l'operazione di ripristino, il nuovo oggetto otterrebbe tali diritti di accesso. 
  
9. Invalida il pool di RID corrente. Il pool di RID corrente viene invalidato dopo un ripristino dello stato del sistema. Tuttavia, se non è stato eseguito un ripristino dello stato del sistema, è necessario invalidare il pool di RID corrente per impedire al controller di dominio ripristinato di riemettere RID dal pool di RID assegnato al momento della creazione del backup. Per ulteriori informazioni, vedere [invalidare il pool RID corrente](AD-Forest-Recovery-Invaildate-RID-Pool.md). 
  
    > [!NOTE]
    > La prima volta che si tenta di creare un oggetto con un SID dopo l'invalidamento del pool di RID verrà visualizzato un errore. Il tentativo di creare un oggetto attiva una richiesta per un nuovo pool di RID. Il tentativo di esecuzione dell'operazione riesce perché verrà allocato il nuovo pool di RID. 
  
10. Reimpostare la password dell'account computer del controller di dominio due volte. Per ulteriori informazioni, vedere [reimpostazione della password dell'account computer del controller di dominio](AD-Forest-Recovery-Reset-Computer-Account-DC.md). 
  
11. Reimpostare la password KRBTGT due volte. Per ulteriori informazioni, vedere [reimpostazione della password di KRBTGT](AD-Forest-Recovery-Resetting-the-krbtgt-password.md). 
  
     Poiché la cronologia delle password di KRBTGT è due password, reimpostare le password due volte per rimuovere la password originale (preerrore) dalla cronologia delle password. 
  
    > [!NOTE]
    > Se il ripristino della foresta è in risposta a una violazione della sicurezza, è anche possibile reimpostare le password di attendibilità. Per ulteriori informazioni, vedere [reimpostazione di una password di attendibilità su un lato della relazione di trust](AD-Forest-Recovery-Reset-Trust.md). 
  
12. Se la foresta dispone di più domini e il controller di dominio ripristinato era un server di catalogo globale prima dell'errore, deselezionare la casella di controllo **catalogo globale** nelle proprietà delle impostazioni NTDS per rimuovere il catalogo globale dal controller di dominio. L'eccezione a questa regola è il caso comune di una foresta con un solo dominio. In questo caso, non è necessario rimuovere il catalogo globale. Per ulteriori informazioni, vedere [la pagina relativa alla rimozione del catalogo globale](AD-Forest-Recovery-Remove-GC.md). 
  
     Il ripristino di un catalogo globale da un backup più recente di altri backup utilizzati per ripristinare i controller di dominio in altri domini può comportare l'introduzione di oggetti residui. Si consideri l'esempio seguente. Nel dominio A, DC1 viene ripristinato da un backup che è stato utilizzato al momento T1. Nel dominio B, DC2 viene ripristinato da un backup del catalogo globale che è stato utilizzato al momento T2. Si supponga che T2 sia più recente di T1 e che alcuni oggetti siano stati creati tra T1 e T2. Dopo il ripristino di questi controller di dominio, DC2, un catalogo globale, include i dati più recenti per la replica parziale del dominio A rispetto al dominio A. DC2, in questo caso, include oggetti residui perché questi oggetti non sono presenti in DC1. 
  
     La presenza di oggetti residui può causare problemi. È ad esempio possibile che i messaggi di posta elettronica non vengano recapitati a un utente il cui oggetto utente è stato spostato tra domini. Dopo aver portato online il controller di dominio o il server di catalogo globale obsoleto, entrambe le istanze dell'oggetto utente verranno visualizzate nel catalogo globale. Entrambi gli oggetti hanno lo stesso indirizzo di posta elettronica; Pertanto, non è possibile recapitare i messaggi di posta elettronica. 
  
     Un secondo problema è che un account utente che non esiste più potrebbe ancora essere visualizzato nell'elenco indirizzi globale. Un terzo problema è che un gruppo universale che non esiste più potrebbe ancora essere visualizzato nel token di accesso di un utente. 
  
     Se è stato eseguito il ripristino di un controller di dominio che era un catalogo globale, inavvertitamente o perché era il backup solitario considerato attendibile, si consiglia di impedire l'occorrenza di oggetti residui disabilitando il catalogo globale subito dopo l'operazione di ripristino. completare. La disabilitazione del flag di catalogo globale comporterà la perdita di tutte le repliche parziali (partizioni) e la recessione a un normale stato del controller di dominio. 
  
13. Configurare il servizio ora di Windows. Nel dominio radice della foresta configurare l'emulatore PDC per sincronizzare l'ora da un'origine ora esterna. Per altre informazioni, vedere [configurare il servizio ora di Windows nell'emulatore PDC nel dominio radice della foresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29). 
  
## <a name="reconnect-each-restored-writeable-domain-controller-to-a-common-network"></a>Riconnettere ogni controller di dominio scrivibile ripristinato a una rete comune

In questa fase è necessario ripristinare un controller di dominio (e i passaggi di ripristino) nel dominio radice della foresta e in ognuno dei domini rimanenti. Aggiungere i controller di dominio a una rete comune isolata dal resto dell'ambiente e completare i passaggi seguenti per convalidare l'integrità della foresta e la replica.

> [!NOTE]
> Quando si uniscono i controller di dominio fisici a una rete isolata, potrebbe essere necessario modificarne gli indirizzi IP. Di conseguenza, gli indirizzi IP dei record DNS non saranno corretti. Poiché un server di catalogo globale non è disponibile, gli aggiornamenti dinamici sicuri per DNS avranno esito negativo. In questo caso i controller di dominio virtuali sono più vantaggiosi perché possono essere aggiunti a una nuova rete virtuale senza modificare gli indirizzi IP. Questo è uno dei motivi per cui i controller di dominio virtuali sono consigliati come primo controller di dominio da ripristinare durante il ripristino della foresta. 
  
Dopo la convalida, aggiungere i controller di dominio alla rete di produzione e completare i passaggi per verificare l'integrità della replica della foresta.

- Per correggere la risoluzione dei nomi, creare record di delega DNS e configurare l'invio DNS e i parametri radice in base alle esigenze. Eseguire **Repadmin/Replsum.** per controllare la replica tra controller di dominio. 
- Se i controller di dominio ripristinati non sono partner di replica diretta, il ripristino della replica sarà molto più rapido creando oggetti di connessione temporanei tra di essi. 
- Per convalidare la pulizia dei metadati, eseguire **repadmin/viewlist \\** * per un elenco di tutti i controller di dominio nella foresta. Eseguire **nltest/DCList:** *< dominio\>*  per un elenco di tutti i controller di dominio nel dominio. 
- Per controllare l'integrità del controller di dominio e del DNS, eseguire DCDiag/v per segnalare gli errori in tutti i controller di dominio nella foresta. 

## <a name="add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain"></a>Aggiungere il catalogo globale a un controller di dominio nel dominio radice della foresta

Per questi e altri motivi è necessario un catalogo globale:  
  
- Per abilitare gli accessi per gli utenti. 
- Per abilitare il servizio Accesso rete in esecuzione nei controller di dominio in ciascun dominio figlio per la registrazione e la rimozione dei record nel server DNS nel dominio radice. 
  
Sebbene sia preferibile che il controller di dominio radice della foresta diventi un catalogo globale, è possibile scegliere uno dei controller di dominio ripristinati per diventare un catalogo globale. 
  
> [!NOTE]
> Un controller di dominio non verrà annunciato come server di catalogo globale fino a quando non viene completata una sincronizzazione completa di tutte le partizioni di directory nella foresta. Pertanto, è necessario forzare la replica del controller di dominio con ognuno dei controller di dominio ripristinati nella foresta. 
>
> Monitorare il registro eventi del servizio directory in Visualizzatore eventi per l'ID evento 1119, che indica che il controller di dominio è un server di catalogo globale, oppure verificare che la chiave del registro di sistema seguente abbia un valore pari a 1:  
>
> **Promozione Catalogo HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Global completata**  
  
Per ulteriori informazioni, vedere [aggiunta del catalogo globale](AD-Forest-Recovery-Add-GC.md). 
  
In questa fase è necessario disporre di una foresta stabile, con un controller di dominio per ogni dominio e un catalogo globale nella foresta. È necessario eseguire un nuovo backup di ogni controller di dominio appena ripristinato. È ora possibile iniziare a ridistribuire altri controller di dominio nella foresta installando servizi di dominio Active Directory. 

## <a name="next-steps"></a>Passaggi successivi

- [Ripristino della foresta di Active Directory - Prerequisiti](AD-Forest-Recovery-Prerequisties.md)  
- [Ripristino della foresta di Active Directory-definizione di un piano di ripristino della foresta personalizzato](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Ripristino della foresta di Active Directory-identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Ripristino della foresta di Active Directory-determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Ripristino della foresta di Active Directory-esecuzione del ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)  
- [Ripristino della foresta di Active Directory-Domande frequenti](AD-Forest-Recovery-FAQ.md)  
- [Ripristino della foresta di Active Directory-ripristino di un singolo dominio in una foresta con più domini](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Ripristino della foresta di Active Directory-ripristino della foresta con i controller di dominio Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
