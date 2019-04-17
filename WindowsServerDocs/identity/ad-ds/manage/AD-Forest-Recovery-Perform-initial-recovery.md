---
title: Ripristino della foresta Active Directory - eseguire il ripristino iniziale
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: fc2ec09c5b96b76229d532adc6a254c8108d8940
ms.sourcegitcommit: ac73f0f0dca04be731b928183bfdffc97d82c277
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/02/2018
---
# <a name="perform-initial-recovery"></a>Eseguire il ripristino iniziale  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 In questa sezione include i passaggi seguenti:  
  
-   [Ripristinare il primo controller di dominio scrivibile in ogni dominio](#Restore-the-first-writeable-domain-controller-in-each-domain)  

-   [Riconnessione ogni controller di dominio scrivibile ripristinato alla rete](#Reconnect-each-restored-writeable-domain-controller-to-a-common-network)  
  
-   [Aggiungere il catalogo globale in un controller di dominio nel dominio radice della foresta](#Add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain)  
  
  
## <a name="restore-the-first-writeable-domain-controller-in-each-domain"></a>Ripristinare il primo controller di dominio scrivibile in ogni dominio  
 A partire da un controller di dominio scrivibile nel dominio radice della foresta, completare i passaggi in questa sezione per ripristinare il primo controller di dominio. Dominio radice della foresta è importante poiché archivia i gruppi Schema Admins ed Enterprise Admins. Consente inoltre di mantenere la gerarchia di trust nella foresta. Inoltre, il dominio radice della foresta contiene in genere il server radice DNS per spazio dei nomi DNS della foresta. Di conseguenza, la zona DNS integrate in Active Directory per il dominio contiene i record di risorse alias (CNAME) per tutti gli altri controller di dominio della foresta (che sono necessari per la replica) e i record di risorse DNS del catalogo globale.  
  
 Dopo aver ripristinato il dominio radice della foresta, ripetere la stessa procedura per ripristinare i domini nella foresta rimanenti. È possibile ripristinare più di un dominio contemporaneamente; Tuttavia, ripristinare sempre un dominio padre prima del ripristino di un bambino per evitare qualsiasi interruzione nella gerarchia di trust o risoluzione dei nomi DNS.  
  
 Per ogni dominio che è ripristinare, ripristinare un solo controller di dominio scrivibile dal backup. Questa è la parte più importante delle operazioni di ripristino poiché il controller di dominio deve disporre di un database che non è stato influenzato dalla causa dell'insieme di strutture di esito negativo. È importante disporre di un backup attendibile che viene testato accuratamente prima che è stato introdotto nell'ambiente di produzione.  
  
 Eseguire quindi i passaggi seguenti. Procedure per l'esecuzione di alcune operazioni [ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md).  
  
1.  Se si intende ripristinare un server fisico, assicurarsi che il cavo di rete della destinazione controller di dominio non è collegato e pertanto non è connesso alla rete di produzione. Per una macchina virtuale, è possibile rimuovere la scheda di rete o utilizzare una scheda di rete che è collegata a un'altra rete in cui è possibile testare il processo di ripristino mentre isolato dalla rete di produzione.  
  
2.  Poiché questo è il primo controller di dominio scrivibile nel dominio, è necessario eseguire un ripristino non autorevole di dominio Active Directory e un ripristino autorevole di SYSVOL. L'operazione di ripristino deve essere completata con una copia di backup compatibile con Active Directory e ripristinare l'applicazione, ad esempio Windows Server Backup (vale a dire, è consigliabile non ripristinare il controller di dominio utilizzando metodi non supportati, ad esempio il ripristino di uno snapshot macchina virtuale).  
  
     Un ripristino autorevole di SYSVOL è necessario perché la replica di SYSVOL replicati cartella deve essere avviata dopo il ripristino di emergenza. Tutti i controller di dominio successivi che vengono aggiunti nel dominio necessario risincronizzare la cartella SYSVOL con una copia della cartella che è stata selezionata come autorevole prima che la cartella può essere annunciata.  
  
    > [!CAUTION]
    >  Eseguire un'operazione di ripristino autorevole (o primario) di SYSVOL solo per il primo controller di dominio nel dominio radice della foresta è ripristinata. In modo errato l'esecuzione di operazioni di ripristino primario di SYSVOL in altri controller di dominio clienti potenziali conflitti di replica dei dati SYSVOL.  
  
     Esistono due opzioni di eseguono un ripristino non autorevole di dominio Active Directory e un ripristino autorevole di SYSVOL:  
  
    -   Eseguire un ripristino completo del server e quindi forzare una sincronizzazione autorevole di SYSVOL. Per procedure dettagliate, vedere [eseguire un ripristino completo del server](AD-Forest-Recovery-Perform-a-Full-Recovery.md) e [eseguire una sincronizzazione autorevole di SYSVOL con replica DFS](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).  
  
    -   Eseguire un ripristino completo del server, seguito da un ripristino dello stato del sistema. Questa opzione è necessario creare in anticipo entrambi i tipi di backup: un backup completo del server e un backup dello stato del sistema. Per procedure dettagliate, vedere [eseguire un ripristino completo del server](AD-Forest-Recovery-Perform-a-Full-Recovery.md) e [eseguire un ripristino non autorevole di servizi di dominio Active Directory](AD-Forest-Recovery-Nonauthoritative-Restore.md).  
  
3.  Dopo aver ripristinato e riavviare il controller di dominio scrivibile, verificare che l'errore è stato non interessano i dati sul controller di dominio. Se i dati di controller di dominio sono danneggiati, quindi ripetere il passaggio 2 con un altro backup.  
  
     Se il controller di dominio ripristinato ospita un ruolo di master operazioni, devi aggiungere la seguente voce del Registro di sistema per evitare di dominio Active Directory non sono disponibili fino al completamento della replica di una partizione di directory scrivibile:  
  
     **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Repl eseguire sincronizzazioni iniziali**  
  
     Creare la voce con il tipo di dati **REG_DWORD** e il valore **0**. Dopo avere recuperato completamente l'insieme di strutture, è possibile reimpostare il valore di questa voce per **1**, che richiede un controller di dominio che viene riavviato e contiene corretta ruoli di master di operazioni di dominio Active Directory in entrata e in uscita replica con i relativi partner di replica noti prima di essere annuncia come controller di dominio e inizia a fornire servizi ai client. Per ulteriori informazioni sui requisiti della sincronizzazione iniziale, vedere l'articolo [305476](https://support.microsoft.com/kb/305476).  
  
     Continuare la procedura successiva solo dopo aver ripristinato e verificare i dati e aggiungere questo computer alla rete di produzione.  
  
4.  Se si sospetta che l'errore a livello di foresta è correlata a intrusioni di rete o attacchi di utenti malintenzionati, reimpostare le password degli account per tutti gli account amministrativi, inclusi i membri del gruppo Enterprise Admins, Domain Admins, Schema Admins, Server Operators, gruppi di Account Operators e così via. La reimpostazione delle password di account amministrativi deve essere completata prima installati durante la fase successiva del ripristino foresta i controller di dominio aggiuntivo.  
  
5.  Il primo controller di dominio ripristinato nel dominio radice della foresta, assegnare tutti i ruoli di master operazioni a livello di dominio e di foresta. Per assegnare ruoli di master operazioni a livello di foresta sono necessarie le credenziali di Enterprise Admins e Schema Admins.  
  
     In ogni dominio figlio, assegnare ruoli di master operazioni a livello di dominio. Anche se potrebbe conservare i ruoli di master operazioni sul controller di dominio ripristinato solo temporaneamente, la riassegnazione di questi ruoli garantisce relative ai controller di dominio ospita li a questo punto nel processo di ripristino della foresta. Come parte del processo di post-ripristino, è possibile ridistribuire i ruoli di master operazioni in base alle esigenze. Per ulteriori informazioni sull'assegnazione dei ruoli di master operazioni, vedere [requisizione un ruolo di master operazioni](AD-forest-recovery-seizing-operations-master-role.md). Per consigli sulla dove posizionare i ruoli di master operazioni, vedere [quali sono i master operazioni?](https://technet.microsoft.com/en-us/library/cc779716.aspx).  
  
6.  La pulizia dei metadati di tutti gli altri controller di dominio scrivibile nel dominio radice della foresta che non si ripristina da backup (tutti scrivibile controller di dominio nel dominio, ad eccezione di questo primo controller di dominio). Se si utilizza la versione di Active Directory Users e computer o siti di Active Directory e servizi che è incluso in Windows Server 2008 o versione successiva o amministrazione remota del server per Windows Vista o versioni successive, la pulizia dei metadati viene eseguita automaticamente quando si elimina un oggetto controller di dominio. Inoltre, l'oggetto server e un oggetto computer per il controller di dominio eliminato vengono eliminati anche automaticamente. Per ulteriori informazioni, vedere [pulizia dei metadati di rimossa i controller di dominio scrivibile](AD-Forest-Recovery-Cleaning-Metadata.md).  
  
     Pulizia dei metadati impedisce possibili duplicazione di oggetti Impostazioni NTDS, se è installato su un controller di dominio in un diverso sito Active Directory. Potenzialmente, questo potrebbe anche salvare il controllo di coerenza informazioni (KCC) il processo di creazione di collegamenti di replica quando i controller di dominio se stessi potrebbero non essere presenti. Inoltre, come parte di pulizia dei metadati, verranno eliminati da DNS record di risorse DNS del localizzatore di controller di dominio per tutti gli altri controller di dominio nel dominio.  
  
     Fino a quando non viene rimosso i metadati di tutti gli altri controller di dominio nel dominio, il controller di dominio, se si trattasse di un ruolo di master RID prima del ripristino, non presuppone il ruolo di master RID e pertanto non sarà in grado di rilasciare i RID nuovo. Potresti vedere ID evento 16650 nel Registro di sistema nel Visualizzatore eventi che indicano che questo errore, ma verrà visualizzato l'ID evento 16648 che indica la riuscita leggermente durante dopo avere eliminato i metadati.  
  
7.  Se si dispone di zone DNS che sono archiviate in Active Directory, verificare che il servizio Server DNS locale sia installato e in esecuzione sul controller di dominio che è stato ripristinato. Se il controller di dominio non è un server DNS prima dell'errore di foresta, è necessario installare e configurare il server DNS.  
  
    > [!NOTE]
    >  Se il controller di dominio ripristinato esegue Windows Server 2008, è necessario installare l'hotfix descritto nell'articolo [975654](https://support.microsoft.com/kb/975654) o connettere il server a una rete isolata temporaneamente per installare il server DNS. L'hotfix non è necessaria per le altre versioni di Windows Server.  
  
     Nel dominio radice della foresta, configurare il controller di dominio ripristinato con il proprio indirizzo IP (o un indirizzo di loopback, ad esempio 127.0.0.1) come il server DNS preferito. È possibile configurare questa impostazione nelle proprietà TCP/IP della scheda di rete locale (LAN). Questo è il primo server DNS della foresta. Per ulteriori informazioni, vedere [configurare TCP/IP da utilizzare DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx).  
  
     In ogni dominio figlio, configurare il controller di dominio ripristinato con l'indirizzo IP del primo server DNS nel dominio radice della foresta come del server DNS preferito. È possibile configurare questa impostazione nelle proprietà TCP/IP della scheda di rete LAN. Per ulteriori informazioni, vedere [configurare TCP/IP da utilizzare DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx).  
  
     Nelle zone DNS msdcs e il dominio, eliminare i record NS dei controller di dominio che non esistono più dopo la pulizia dei metadati. Controllare se sono stati rimossi i record SRV dei controller di dominio backup pulito. Per velocizzare la rimozione di record DNS SRV, eseguire:  
  
    ```  
    nltest.exe /dsderegdns:server.domain.tld  
    ```  
  
8.  Aumentare il valore del pool di RID disponibile di 100.000. Per ulteriori informazioni, vedere [generare il valore del pool di RID disponibile](AD-Forest-Recovery-Raise-RID-Pool.md). Se hai un motivo di ritenere che la generazione di Pool di RID di 100.000 è insufficiente per la situazione specifica, è necessario determinare l'incremento più basso che è comunque possibile. RID sono una risorsa limitata, che non deve essere utilizzata inutilmente backup.  
  
     Se nuove entità di sicurezza sono state create nel dominio dopo l'ora del backup che usi per il ripristino, queste entità di sicurezza che disponga dei diritti di accesso per alcuni oggetti. Queste entità di sicurezza non esistono più dopo il ripristino perché il ripristino è stato ripristinato per il backup. Tuttavia, i diritti di accesso potrebbe essere ancora presenti. Se il pool di RID disponibile non viene generato dopo il ripristino, nuovo utente gli oggetti che vengono creati dopo il ripristino della foresta potrebbe ottenere gli ID identici sicurezza (SID) e può avere accesso a tali oggetti, che non è stato originariamente progettato.  
  
     Per illustrare il concetto, si consideri l'esempio del dipendente nuovo denominato Luca citati nella sezione introduttiva. L'oggetto utente per Luca non esiste più dopo l'operazione di ripristino perché è stato creato dopo il backup che è stato utilizzato per ripristinare il dominio. Tuttavia, diritti di accesso che sono stati assegnati a tale oggetto utente potrebbero vengono mantenute dopo l'operazione di ripristino. Se il SID per l'oggetto utente viene riassegnato a un nuovo oggetto dopo l'operazione di ripristino, il nuovo oggetto potrebbe ottenere i diritti di accesso.  
  
9. Invalidare il pool RID corrente. Viene invalidato il pool RID corrente dopo il ripristino di uno stato del sistema. Ma se non è stato eseguito un ripristino dello stato di sistema, il pool RID corrente deve essere invalidato per impedire che il controller di dominio ripristinato nuovo rilascio di RID dal pool di RID che è stato assegnato al momento che della creazione del backup. Per ulteriori informazioni, vedere [invalidare il pool RID corrente](AD-Forest-Recovery-Invaildate-RID-Pool.md).  
  
    > [!NOTE]
    >  La prima volta che si tenta di creare un oggetto con un SID dopo si invalida il pool di RID viene visualizzato un errore. Il tentativo di creare un oggetto attiva una richiesta per un nuovo pool di RID. Nuovo tentativo dell'operazione ha esito positivo poiché verrà allocato il nuovo pool di RID.  
  
10. Reimpostare la password dell'account computer del controller di dominio due volte. Per ulteriori informazioni, vedere [reimpostare la password dell'account computer del controller di dominio](AD-Forest-Recovery-Reset-Computer-Account-DC.md).  
  
11. Reimpostare la password di krbtgt due volte. Per ulteriori informazioni, vedere [la reimpostazione della password krbtgt](AD-Forest-Recovery-Resetting-the-krbtgt-password.md).  
  
     Poiché la cronologia delle password krbtgt due password, reimpostare le password due volte per rimuovere la password (prefailure) originale dalla cronologia delle password.  
  
    > [!NOTE]
    >  Se il ripristino della foresta è in risposta a una violazione della protezione, è inoltre possibile reimpostare le password di trust. Per ulteriori informazioni, vedere [reimpostare una password di trust su un lato del trust](AD-Forest-Recovery-Reset-Trust.md).  
  
12. Se la foresta sono presenti più domini e controller di dominio ripristinato è un server di catalogo globale prima dell'errore, deselezionare il **catalogo globale** casella di controllo nelle proprietà Impostazioni NTDS per rimuovere il catalogo globale del controller di dominio. L'eccezione a questa regola è il caso più comune di una foresta con un solo dominio. In questo caso, non è necessario rimuovere il catalogo globale. Per ulteriori informazioni, vedere [la rimozione del catalogo globale](AD-Forest-Recovery-Remove-GC.md).  
  
     Per ripristinare un catalogo globale da un backup di più recenti rispetto a altro backup che vengono utilizzati per ripristinare i controller di dominio in altri domini, si potrebbero introdurre oggetti residui. Si consideri l'esempio seguente. In un dominio, DC1 viene ripristinato da un backup che è stato acquisito all'ora T1. Nel dominio B, DC2 viene ripristinato da un backup di catalogo globale che è stato acquisito all'ora T2. Si supponga che T2 è più recente T1 e alcuni oggetti sono stati creati tra T1 e T2. Dopo questi controller di dominio vengono ripristinati, DC2 è un catalogo globale, contiene dati più recenti per replica parziale del dominio rispetto a contiene il dominio stesso. DC2 contiene gli oggetti residui in questo caso, poiché questi oggetti non sono presenti in DC1.  
  
     La presenza di oggetti residui può causare problemi. Ad esempio, messaggi di posta elettronica potrebbero non essere recapitati all'utente il cui oggetto utente è stato spostato tra domini. Dopo aver portato il controller di dominio non aggiornati o server di catalogo globale in linea, entrambe le istanze dell'oggetto utente visualizzato nel catalogo globale. Entrambi gli oggetti hanno lo stesso indirizzo di posta elettronica; Pertanto, è Impossibile recapitare messaggi di posta elettronica.  
  
     Un secondo problema è che un account utente che non esiste ancora potrebbero essere visualizzati nell'elenco indirizzi globale. Un terzo problema è che un gruppo universale che non esiste più potrebbe comunque visualizzata in un token di accesso utente.  
  
     Se invece è stato ripristinato un controller di dominio che è stato un catalogo globale, ovvero sia inavvertitamente o perché era il backup unico metodo attendibile, è consigliabile impedire l'occorrenza di oggetti residui disabilitando il catalogo globale subito dopo l'operazione di ripristino è stata completata. La disabilitazione del flag di catalogo globale determinerà il computer perdere tutte le relative repliche parziale (partizioni) e relegating stesso allo stato normale di controller di dominio.  
  
13. Configurare il servizio ora di Windows. Nel dominio radice della foresta, configurare l'emulatore PDC per sincronizzare l'ora da un'origine ora esterna. Per ulteriori informazioni, vedere [configurare il servizio ora di Windows sull'emulatore PDC nel dominio radice della foresta](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
 

## <a name="reconnect-each-restored-writeable-domain-controller-to-a-common-network"></a>Riconnessione ogni controller di dominio scrivibile a una rete comune  
 In questa fase è un controller di dominio ripristinato (e i passaggi di ripristino eseguiti) nel dominio radice della foresta in ognuno dei domini rimanenti. Aggiungere questi controller di dominio a una rete comune che è isolata dal resto dell'ambiente e completare i passaggi seguenti per convalidare l'integrità della foresta e replica.  
  
> [!NOTE]
>  Quando si aggiunge il controller di dominio fisico a una rete isolata, potrebbe essere necessario modificare gli indirizzi IP. Di conseguenza, gli indirizzi IP dei record DNS saranno corretti. Poiché non è disponibile un server di catalogo globale, gli aggiornamenti dinamici protetti per DNS avrà esito negativo. Controller di dominio virtuali sono più vantaggioso in questo caso perché può essere aggiunto a una nuova rete virtuale senza modificare gli indirizzi IP. Questo è il motivo perché i controller di dominio virtuali sono consigliate come il primo controller di dominio da ripristinare durante il ripristino dell'insieme di strutture.  
  
 Dopo la convalida, aggiungere i controller di dominio nella rete di produzione e completare i passaggi per verificare lo stato di replica della foresta.  
  
-   Per correggere la risoluzione dei nomi, creare record di delega DNS e configurare DNS inoltro e radice suggerimenti in base alle esigenze. Eseguire **repadmin /replsum.** per controllare la replica tra controller di dominio.  
  
-   Se il controller di dominio ripristinato non sono partner di replica diretta, ripristino della replica sarà molto più veloce tramite la creazione di oggetti connessione temporanea tra di essi.  
  
-   Per convalidare la pulizia dei metadati, eseguire **Repadmin /viewlist \ *** per un elenco di tutti i controller di dominio nella foresta. Eseguire **Nltest /DCList:***< domain \ >* per un elenco di tutti i controller di dominio nel dominio.  
  
-   Per controllare l'integrità DNS e controller di dominio, eseguire DCDiag /v per segnalare gli errori in tutti i controller di dominio nella foresta.  
  
  

## <a name="add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain"></a>Aggiungere il catalogo globale in un controller di dominio nel dominio radice della foresta  
 Un catalogo globale è necessario per questi e altri motivi:  
  
-   Per abilitare gli accessi per gli utenti.  
  
-   Per abilitare il servizio Accesso rete in esecuzione sul controller di dominio in ogni dominio figlio per registrare e rimuovere i record nel server DNS nel dominio radice.  
  
 Sebbene sia preferito che il controller di dominio radice della foresta diventa un catalogo globale, è possibile scegliere uno dei controller di dominio ripristinato a diventare un catalogo globale.  
  
> [!NOTE]
>  Non verrà annunciato un controller di dominio come server di catalogo globale fino al completamento di una sincronizzazione completa di tutte le partizioni di directory nella foresta. Di conseguenza, il controller di dominio deve essere forzata di replicare con ognuno dei controller di dominio ripristinato nella foresta.  
>   
>  Controllare il servizio Directory registro eventi nel Visualizzatore eventi per l'evento ID 1119, che indica che il controller di dominio è un server di catalogo globale, o verificare che la seguente chiave del Registro di sistema ha un valore pari a 1:  
>   
>  **Innalzamento di livello catalogo HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Global completo**  
  
 Per ulteriori informazioni, vedere [aggiunta del catalogo globale](AD-Forest-Recovery-Add-GC.md).  
  
 In questa fase è un insieme di strutture stabile, con un controller di dominio per ogni dominio e un catalogo globale nella foresta. Verificare che un nuovo backup di tutti i controller di dominio appena ripristinato. È ora possibile iniziare a ridistribuire altri controller di dominio nella foresta installando Servizi di dominio Active Directory.  

## <a name="next-steps"></a>Passaggi successivi
-   [Ripristino della foresta Active Directory - prerequisiti](AD-Forest-Recovery-Prerequisties.md)  
-   [Ripristino della foresta Active Directory - sviluppo di un piano di ripristino personalizzato foresta](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Ripristino della foresta Active Directory - identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Ripristino della foresta Active Directory - determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Ripristino della foresta Active Directory - eseguire il ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)  
-   [Ripristino della foresta Active Directory - domande frequenti](AD-Forest-Recovery-FAQ.md)  
-   [Ripristino della foresta Active Directory - ripristino di un singolo dominio all'interno di un insieme di strutture Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [Ripristino della foresta Active Directory - ripristino della foresta con i controller di dominio di Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
  
