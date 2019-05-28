---
title: Ripristino della foresta Active Directory - eseguire il ripristino iniziale
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 8e05043d029636ddeb3a24349897ac61a713b2a7
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034118"
---
# <a name="perform-initial-recovery"></a>Eseguire il ripristino iniziale  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

In questa sezione include i passaggi seguenti:  

- [Ripristinare il primo controller di dominio scrivibile in ogni dominio](#restore-the-first-writeable-domain-controller-in-each-domain)  
- [Riconnettersi ogni controller di dominio scrivibile ripristinato alla rete](#reconnect-each-restored-writeable-domain-controller-to-a-common-network)  
- [Aggiungere il catalogo globale a un controller di dominio nel dominio radice della foresta](#add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain)  

## <a name="restore-the-first-writeable-domain-controller-in-each-domain"></a>Ripristinare il primo controller di dominio scrivibile in ogni dominio  

A partire da un controller di dominio scrivibile nel dominio radice della foresta, completare i passaggi in questa sezione per ripristinare il primo controller di dominio. Il dominio radice della foresta è importante perché archivia i gruppi Enterprise Admins e Schema Admins. Consente anche di mantenere la gerarchia di trust della foresta. Inoltre, il dominio radice della foresta contiene in genere il server radice DNS per lo spazio dei nomi DNS della foresta. Di conseguenza, la zona DNS integrate in Active Directory per tale dominio contiene i record di risorse alias (CNAME) per tutti gli altri controller di dominio della foresta (che sono necessarie per la replica) e i record di risorse DNS del catalogo globale. 
  
Dopo aver ripristinato il dominio radice della foresta, ripetere gli stessi passaggi per eseguire il ripristino di altri domini nella foresta. È possibile ripristinare più di un dominio contemporaneamente; Tuttavia, sempre il ripristino un dominio padre prima del ripristino di un elemento figlio per evitare qualsiasi interruzione nella gerarchia di trust o risoluzione dei nomi DNS. 
  
Per ogni dominio che è ripristinare, ripristinare un solo controller di dominio scrivibile dal backup. Questa è la parte più importante delle operazioni di ripristino perché il controller di dominio deve disporre di un database che non è stato influenzato da la causa dell'insieme di strutture esito negativo. È importante eseguire il backup attendibile che viene testato accuratamente prima che viene introdotto nell'ambiente di produzione. 
  
Quindi eseguire la procedura seguente. Procedure per eseguire alcuni passaggi sono nel [ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md). 
  
1. Se si prevede di ripristinare un server fisico, verificare che il cavo di rete della destinazione del controller di dominio non è collegato e pertanto non è connesso alla rete di produzione. Per una macchina virtuale, è possibile rimuovere la scheda di rete o utilizzare una scheda di rete collegata a un'altra rete in cui è possibile testare il processo di ripristino mentre isolata dalla rete di produzione. 
  
2. Poiché questo è il primo controller di dominio scrivibile nel dominio, è necessario eseguire un ripristino non autorevole di dominio Active Directory e un ripristino autorevole di SYSVOL. L'operazione di ripristino deve essere completata usando un backup compatibile con Active Directory e il ripristino dell'applicazione, ad esempio Windows Server Backup (vale a dire, evitare di ripristinare il controller di dominio usando i metodi non supportati, ad esempio il ripristino di uno snapshot macchina virtuale). 
   - Un ripristino autorevole di SYSVOL è necessario perché la replica di SYSVOL replicati cartella deve essere avviata dopo il ripristino di emergenza. Tutti i successivi controller di dominio che vengono aggiunti nel dominio deve sincronizzare di nuovo la cartella SYSVOL con una copia della cartella in cui è stata selezionata per essere autorevoli prima che la cartella può essere annunciata. 

   > [!CAUTION]
   > Eseguire un'operazione di ripristino autorevole (o primaria) della cartella SYSVOL solo per il primo controller di dominio da ripristinare nel dominio radice della foresta. Esecuzione in modo non corretto delle operazioni di ripristino primario della cartella SYSVOL in altri controller di dominio comporta i conflitti tra repliche di dati SYSVOL. 

   - Sono disponibili due opzioni di eseguono un ripristino non autorevole di dominio Active Directory e un ripristino autorevole di SYSVOL:  
   - Eseguire un ripristino completo del server, quindi forzare una sincronizzazione autorevole di SYSVOL. Per le procedure dettagliate, vedere [esecuzione di un ripristino completo del server](AD-Forest-Recovery-Perform-a-Full-Recovery.md) e [eseguire una sincronizzazione autorevole di DFSR-replicated SYSVOL](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md). 
   - Eseguire un ripristino completo del server seguito da un ripristino dello stato del sistema. Questa opzione è necessario creare in anticipo entrambi i tipi di backup: un backup completo del server e un backup dello stato del sistema. Per le procedure dettagliate, vedere [esecuzione di un ripristino completo del server](AD-Forest-Recovery-Perform-a-Full-Recovery.md) e [esegue un ripristino non autorevole di servizi di dominio Active Directory](AD-Forest-Recovery-Nonauthoritative-Restore.md). 
  
3. Dopo il ripristino e riavviare il controller di dominio scrivibile, verificare che l'errore non ha modificato i dati sul controller di dominio. Se i dati di controller di dominio sono danneggiati, quindi ripetere il passaggio 2 con un backup diverso. 
   - Se il controller di dominio ripristinato ospita un ruolo di master operazioni, occorre aggiungere la seguente voce del Registro di sistema per evitare di Active Directory Domain Services non è disponibile fino a quando non è stata completata la replica di una partizione di directory scrivibile:  

      **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Repl Perform Initial Synchronizations**  
  
      Creare la voce con il tipo di dati **REG_DWORD** e il valore **0**. Dopo avere recuperato l'insieme di strutture completamente, è possibile reimpostare il valore di questa voce al **1**, che richiede un controller di dominio che viene riavviato e connessioni in entrata contiene ruoli di master operazioni abbiano esito positivo Active Directory Domain Services e la replica in uscita con relativo noti partner di replica prima che promuove se stesso come controller di dominio e inizia a fornire servizi ai client. Per altre informazioni sui requisiti di sincronizzazione iniziale, vedere l'articolo della KB [305476](https://support.microsoft.com/kb/305476). 
  
      Continuare con i passaggi successivi solo dopo il ripristino e verificare i dati e aggiungere questo computer nella rete di produzione. 
  
4. Se si ritiene che l'errore a livello di foresta è correlata alla rete delle intrusioni o attacchi dannosi, reimpostare le password degli account per tutti gli account amministrativi, inclusi i membri del gruppo Enterprise Admins, Domain Admins, Schema Admins, Server Operators, Account I gruppi di operatori e così via. La reimpostazione della password dell'account amministrativo deve essere completata prima di dominio aggiuntivi, i controller vengono installati durante la fase successiva del ripristino della foresta. 
5. Il primo controller di dominio ripristinato nel dominio radice della foresta, assegnare tutti i ruoli di master operazioni a livello di dominio e a livello di foresta. Per riassegnare i ruoli di master operazioni a livello di foresta sono necessarie le credenziali di Enterprise Admins e Schema Admins. 
  
     In ogni dominio figlio, assegnare ruoli di master operazioni a livello di dominio. Anche se potrebbe conservare i ruoli di master operazioni nel controller di dominio ripristinato solo temporaneamente, la riassegnazione di questi ruoli garantisce relative ai controller di dominio ospita li a questo punto nel processo di ripristino dell'insieme di strutture. Come parte del processo di post-ripristino, è possibile ridistribuire i ruoli di master operazioni in base alle esigenze. Per altre informazioni sulla requisizione dei ruoli di master operazioni, vedere [requisizione un ruolo di master operazioni](AD-forest-recovery-seizing-operations-master-role.md). Per consigli sulla dove posizionare i ruoli di master operazioni, vedere [quali sono i master operazioni?](https://technet.microsoft.com/library/cc779716.aspx). 
  
6. Pulizia dei metadati di tutti gli altri controller di dominio scrivibile nel dominio radice della foresta che non si desidera ripristinare dal backup (tutti i DC scrivibili nel dominio, ad eccezione di questo primo controller di dominio). Se si usa la versione di Active Directory Users e computer o siti di Active Directory e servizi che è incluso in Windows Server 2008 o versione successiva o amministrazione remota del server per Windows Vista o versioni successive, la pulizia dei metadati viene eseguita automaticamente quando si elimina un oggetto controller di dominio. Inoltre, l'oggetto server e un oggetto computer per il controller di dominio eliminato vengono inoltre eliminate automaticamente. Per altre informazioni, vedere [pulizia dei metadati di rimossa i controller di dominio scrivibile](AD-Forest-Recovery-Cleaning-Metadata.md). 
  
     Pulizia dei metadati impedisce di oggetti Impostazioni NTDS possibili duplicati se Active Directory Domain Services è installato in un controller di dominio in un sito diverso. Potenzialmente, questo è stato possibile inoltre salvare il controllo di coerenza informazioni (KCC) il processo di creazione di collegamenti di replica quando i controller di dominio stesse potrebbe non essere presente. Inoltre, come parte della pulizia dei metadati, verranno eliminati i record risorsa DNS del localizzatore di controller di dominio per tutti gli altri controller di dominio nel dominio in DNS. 
  
     Fino a quando non viene rimosso i metadati di tutti gli altri controller di dominio nel dominio, questo controller di dominio, se si trattasse di un master RID prima di ripristino, non verrà considerato il ruolo di master RID e pertanto non sarà in grado di emettere nuovi RID. ID evento 16650 nel Registro di sistema nel Visualizzatore eventi che indicano questo errore potrebbe essere visualizzato, ma dovrebbe essere l'ID evento 16648 che indica la riuscita offline dopo avere eliminato i metadati. 
  
7. Se si dispone di zone DNS che vengono archiviate in Active Directory Domain Services, assicurarsi che il servizio Server DNS locale sia installato e in esecuzione sul controller di dominio che è stato ripristinato. Se il controller di dominio non è un server DNS prima dell'errore di foresta, è necessario installare e configurare il server DNS. 
  
    > [!NOTE]
    > Se il controller di dominio ripristinato è in esecuzione Windows Server 2008, è necessario installare l'hotfix descritto nell'articolo [975654](https://support.microsoft.com/kb/975654) o connettere il server a una rete isolata temporaneamente per poter installare il server DNS. L'hotfix non è necessario per qualsiasi altra versione di Windows Server. 
  
     Nel dominio radice della foresta, configurare il controller di dominio ripristinato con il proprio indirizzo IP (o un indirizzo di loopback, ad esempio 127.0.0.1) come server DNS preferito. È possibile configurare questa impostazione nelle proprietà TCP/IP della scheda di rete locale (LAN). Questo è il primo server DNS della foresta. Per altre informazioni, vedere [configurare TCP/IP da usare DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx). 
  
     In ogni dominio figlio, configurare il controller di dominio ripristinato con l'indirizzo IP del primo server DNS nel dominio radice della foresta come server DNS preferito. È possibile configurare questa impostazione nelle proprietà TCP/IP della scheda di rete LAN. Per altre informazioni, vedere [configurare TCP/IP da usare DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx). 
  
     Nelle zone DNS zona msdcs e il dominio, eliminare i record NS dei controller di dominio che non esistono più dopo la pulizia dei metadati. Controllare se sono stati rimossi i record SRV dei controller di dominio pulito. Per ridurre la durata di rimozione dei record SRV in DNS, eseguire:  
  
    ```  
    nltest.exe /dsderegdns:server.domain.tld  
    ```  
  
8. Aumentare il valore del pool di RID disponibile di 100.000 volte. Per altre informazioni, vedere [che genera il valore disponibile di pool di RID](AD-Forest-Recovery-Raise-RID-Pool.md). Se hai motivo di ritenere che la generazione di Pool di RID di 100.000 volte è insufficiente per la situazione specifica, è necessario determinare la quantità più bassa che è comunque al sicura da usare. I RID sono una risorsa limitata che non deve essere utilizzata inutilmente backup. 
  
     Se dopo l'ora del backup che usano per il ripristino, nuove entità di sicurezza sono state create nel dominio, queste entità di sicurezza che disponga dei diritti di accesso su determinati oggetti. Queste entità di sicurezza non più disponibile dopo il ripristino perché il ripristino ha ripristinato il backup. Tuttavia, i diritti di accesso potrebbe essere ancora disponibili. Se il pool di RID disponibile non viene generato dopo un ripristino, nuovo utente gli oggetti che vengono create dopo il ripristino della foresta può comunque ottenere gli ID identici sicurezza (SID) e può avere accesso a tali oggetti, che non è stata originariamente progettata. 
  
     Per illustrare questo concetto, si consideri l'esempio del nuovo dipendente denominato Amy che è stata menzionata nell'introduzione. L'oggetto utente per Alice non esiste più dopo l'operazione di ripristino perché è stato creato dopo il backup è stato usato per ripristinare il dominio. Diritti di accesso che sono stati assegnati a tale oggetto utente, tuttavia, potrebbe essere mantenute dopo l'operazione di ripristino. Se il SID per l'oggetto utente viene riassegnato a un nuovo oggetto dopo l'operazione di ripristino, il nuovo oggetto ottenuto i diritti di accesso. 
  
9. Invalida il pool RID corrente. Viene invalidato il pool RID corrente dopo uno stato del sistema ripristinato. Ma se non è stato eseguito un ripristino dello stato del sistema, il pool RID corrente deve essere invalidati per impedire che il controller di dominio ripristinato inviano di nuovo i RID dal pool di RID che è stato assegnato al momento che della creazione del backup. Per altre informazioni, vedere [invalidando il pool RID corrente](AD-Forest-Recovery-Invaildate-RID-Pool.md). 
  
    > [!NOTE]
    > La prima volta che si prova a creare un oggetto con un SID dopo si invalida il pool di RID si riceverà un errore. Il tentativo di creare un oggetto attiva una richiesta di un nuovo pool di RID. Ripetizione dei tentativi dell'operazione ha esito positivo perché il nuovo pool di RID verrà allocato. 
  
10. Reimpostare la password dell'account computer del controller di dominio due volte. Per altre informazioni, vedere [reimpostare la password dell'account computer del controller di dominio](AD-Forest-Recovery-Reset-Computer-Account-DC.md). 
  
11. Reimpostare la password krbtgt due volte. Per altre informazioni, vedere [la reimpostazione della password krbtgt](AD-Forest-Recovery-Resetting-the-krbtgt-password.md). 
  
     Poiché la cronologia delle password krbtgt due password, reimpostare le password due volte per rimuovere la password (prefailure) originale dalla cronologia delle password. 
  
    > [!NOTE]
    > Se il ripristino della foresta è in risposta a una violazione della sicurezza, è inoltre possibile reimpostare le password di trust. Per altre informazioni, vedere [reimpostare una password di attendibilità sul uno lato della relazione di trust](AD-Forest-Recovery-Reset-Trust.md). 
  
12. Se la foresta ha più domini e controller di dominio ripristinato è un server di catalogo globale prima dell'errore, deselezionare il **catalogo globale** casella di controllo nelle proprietà dell'oggetto impostazioni NTDS per rimuovere il catalogo globale del controller di dominio. L'eccezione a questa regola è il caso comune di una foresta con un solo dominio. In questo caso, non è necessario rimuovere il catalogo globale. Per altre informazioni, vedere [rimozione del catalogo globale](AD-Forest-Recovery-Remove-GC.md). 
  
     Tramite il ripristino di un catalogo globale da un backup più recente rispetto a altro backup che vengono usati per ripristinare i controller di dominio in altri domini, si potrebbero introdurre oggetti residui. Si consideri l'esempio seguente. Nel dominio A, DC1 viene ripristinato da un backup effettuato all'ora T1. Nel dominio B, DC2 viene ripristinato da un backup del catalogo globale effettuato all'ora T2. Si supponga che T2 è più recente di T1 e alcuni oggetti sono stati creati tra le tabelle T1 e T2. Dopo questi controller di dominio vengono ripristinati, DC2, ovvero un catalogo globale, contiene dati più recenti per la replica parziale del dominio di dominio A contiene se stesso. DC2, contiene gli oggetti residui in questo caso, poiché questi oggetti non sono presenti in DC1. 
  
     La presenza di oggetti residui può causare problemi. Ad esempio, messaggi di posta elettronica potrebbero non essere recapitati a un utente il cui oggetto utente è stato spostato tra domini. Dopo aver portato il controller di dominio non aggiornati o il server di catalogo globale online, vengono visualizzate entrambe le istanze dell'oggetto utente nel catalogo globale. Entrambi gli oggetti hanno lo stesso indirizzo di posta elettronica. Pertanto, non possono recapitare messaggi di posta elettronica. 
  
     Un secondo problema è che un account utente che non esiste più potrebbe continuare ad apparire nell'elenco indirizzi globale. Un terzo problema è che un gruppo universale che non esiste più potrebbe comunque visualizzato nel token di accesso dell'utente. 
  
     Se invece è stato ripristinato un controller di dominio che è stato un catalogo globale, ovvero sia inavvertitamente o perché questo è il backup solitari attendibile, è consigliabile impedire l'occorrenza di oggetti residui disabilitando il catalogo globale subito dopo l'operazione di ripristino completare. La disabilitazione del flag di catalogo globale comporterà il computer perdere tutte le relative repliche parziale (partizioni) e se stessa relegating allo stato normale di controller di dominio. 
  
13. Configurare il servizio ora di Windows. Nel dominio radice della foresta, configurare l'emulatore PDC per sincronizzare l'ora da un'origine ora esterna. Per altre informazioni, vedere [configurare il servizio ora di Windows sull'emulatore PDC nel dominio radice della foresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29). 
  
## <a name="reconnect-each-restored-writeable-domain-controller-to-a-common-network"></a>Riconnettersi ogni controller di dominio scrivibile ripristinato a una rete comune

In questa fase si deve avere un controller di dominio ripristinato (e i passaggi di ripristino eseguiti) nel dominio radice della foresta e in ognuno dei domini rimanenti. Aggiungere questi controller di dominio a una rete più comune che è isolata dal resto dell'ambiente e completare la procedura seguente per convalidare l'integrità della foresta e la replica.

> [!NOTE]
> Quando si aggiunge il controller di dominio fisico a una rete isolata, potrebbe essere necessario modificare gli indirizzi IP. Di conseguenza, gli indirizzi IP dei record DNS sarà errati. Poiché non è disponibile un server di catalogo globale, gli aggiornamenti dinamici protetti DNS avrà esito negativo. Controller di dominio virtuali sono più vantaggiose in questo caso, poiché può essere aggiunto a una nuova rete virtuale senza modificare gli indirizzi IP. Questo è il motivo per cui i controller di dominio virtuale sono consigliate come il primo controller di dominio da ripristinare durante il ripristino della foresta. 
  
Dopo la convalida, aggiungere i controller di dominio nella rete di produzione e completare i passaggi per verificare l'integrità della replica dell'insieme di strutture.

- Per risolvere la risoluzione dei nomi, creare record di delega DNS e configurare i DNS radice e l'inoltro dei parametri in base alle esigenze. Eseguire **repadmin /replsum.** per verificare la replica tra controller di dominio. 
- Se il controller di dominio ripristinato non sono partner di replica diretto, ripristino replica sarà molto più veloce tramite la creazione di oggetti di connessione temporanea tra di essi. 
- Per convalidare la pulizia dei metadati, eseguire **Repadmin /viewlist \***  per un elenco di tutti i controller di dominio nella foresta. Eseguire **Nltest /DCList:** *< dominio\>*  per un elenco di tutti i controller di dominio nel dominio. 
- Per controllare l'integrità del controller di dominio e DNS, eseguire DCDiag /v per segnalare gli errori in tutti i controller di dominio nella foresta. 

## <a name="add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain"></a>Aggiungere il catalogo globale a un controller di dominio nel dominio radice della foresta

Un catalogo globale è obbligatorio per questi e altri motivi:  
  
- Per abilitare gli accessi per gli utenti. 
- Per abilitare il servizio Accesso rete in esecuzione nei controller di dominio in ogni dominio figlio per registrare e rimuovere i record nel server DNS nel dominio radice. 
  
Sebbene sia preferibile che il controller di dominio radice della foresta diventa un catalogo globale, è possibile scegliere uno dei controller di dominio ripristinato per diventare un catalogo globale. 
  
> [!NOTE]
> Un controller di dominio non verrà annunciato come server di catalogo globale finché non ha completato una sincronizzazione completa di tutte le partizioni di directory nella foresta. Pertanto, il controller di dominio deve essere forzato a eseguire la replica con ognuno dei controller di dominio ripristinato nella foresta. 
>
> Monitorare il servizio Directory registro eventi nel Visualizzatore eventi per l'evento ID 1119, che indica che il controller di dominio è un server di catalogo globale, o verificare che la chiave del Registro di sistema seguente abbia un valore pari a 1:  
>
> **Innalzamento di livello catalogo HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Global completo**  
  
Per altre informazioni, vedere [aggiungere il catalogo globale](AD-Forest-Recovery-Add-GC.md). 
  
In questa fase si deve avere una foresta stabile, con un controller di dominio per ogni dominio e un catalogo globale nella foresta. È opportuno creare un nuovo backup della ognuno dei controller di dominio che è appena stato ripristinato. È ora possibile iniziare a ridistribuire altri controller di dominio nella foresta tramite l'installazione di Active Directory Domain Services. 

## <a name="next-steps"></a>Passaggi successivi

- [Ripristino della foresta di Active Directory - Prerequisiti](AD-Forest-Recovery-Prerequisties.md)  
- [Ripristino della foresta Active Directory - concepire un piano di ripristino personalizzata-foresta](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Ripristino della foresta Active Directory - identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Ripristino della foresta Active Directory - determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Ripristino della foresta Active Directory - eseguire il ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)  
- [Ripristino della foresta Active Directory - domande frequenti](AD-Forest-Recovery-FAQ.md)  
- [Ripristino della foresta Active Directory - il ripristino di un singolo dominio all'interno di un Multidomain foresta](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Ripristino della foresta Active Directory - ripristino della foresta con controller di dominio di Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
