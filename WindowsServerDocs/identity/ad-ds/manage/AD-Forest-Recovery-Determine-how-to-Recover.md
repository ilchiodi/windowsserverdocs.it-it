---
title: Ripristino della foresta Active Directory - determinare come ripristinare l'insieme di strutture
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: cc2525068225644b0964a2726bd9c0dcf2e0cba0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="determine-how-to-recover-the-forest"></a>Determinare come ripristinare l'insieme di strutture  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Ripristino di un'intera foresta di Active Directory comporta la reinstallazione di servizi di dominio Active Directory (AD DS) o eseguire il ripristino da backup in ogni controller di dominio (DC) nella foresta. Ripristino della foresta viene ripristinato ogni dominio nella foresta stato al momento dell'ultimo backup attendibile. Di conseguenza, l'operazione di ripristino comporta la perdita di almeno i dati di Active Directory seguenti:  
  
-   Tutti gli oggetti che sono stati aggiunti dopo l'ultimo backup attendibile (ad esempio, gli utenti e computer)  
  
-   Tutti gli aggiornamenti che sono stati apportati agli oggetti esistenti attendibile l'ultimo backup  
  
-   Tutte le modifiche apportate dall'ultimo backup attendibile per la partizione di configurazione o della partizione dello schema di dominio Active Directory (ad esempio, le modifiche dello schema)  
  
 Per ogni dominio nella foresta, è necessario conoscere la password di un account amministratore di dominio. Preferibilmente, questa è la password dell'account amministratore predefinito, non deve essere disabilitata. È inoltre necessario conoscere la password modalità ripristino servizi directory per eseguire un ripristino dello stato di sistema di un controller di dominio. In generale, è consigliabile archiviare l'account amministratore e la cronologia delle password modalità ripristino servizi directory in un luogo sicuro per purché i backup sono validi, ovvero entro il periodo di durata oggetto contrassegnato per rimozione o all'interno dell'oggetto eliminato periodo durata se Cestino per Active Directory è abilitato. È inoltre possibile sincronizzare la password modalità ripristino servizi directory con un account utente di dominio per renderlo più facile da ricordare. Per ulteriori informazioni, vedere l'articolo [961320](https://support.microsoft.com/kb/961320). Sincronizzare l'account della modalità ripristino servizi directory deve essere eseguita prima il ripristino della foresta, come parte di preparazione.  
  
> [!NOTE]
>  L'account amministratore è un membro del gruppo Administrators per impostazione predefinita, come i gruppi Enterprise Admins e Domain Admins. Questo gruppo dispone del controllo completo di tutti i controller di dominio nel dominio.  
  
## <a name="determining-which-backups-to-use"></a>Determinare quali backup da utilizzare  
 Almeno due controller di dominio scrivibile per ogni dominio eseguire regolarmente il backup in modo da diversi processi di backup da scegliere. Si noti che è possibile utilizzare il backup di un controller di dominio di sola lettura (RODC) per ripristinare un controller di dominio scrivibile. È consigliabile ripristinare i controller di dominio utilizzando i backup eseguiti alcuni giorni prima occorrenza dell'errore. In generale, è necessario determinare un compromesso tra le recentness e il safeness dei dati da ripristinare. Scelta di un backup più recente consente di recuperare dati più utili, ma potrebbe aumentare il rischio di causare dati pericolosi nell'insieme di strutture ripristinato.  
  
 Il ripristino di backup dello stato del sistema varia a seconda del sistema operativo e i server di backup originale. Ad esempio, non si deve ripristinare un backup dello stato del sistema a un altro server. In questo caso, viene visualizzato l'avviso seguente:  
  
 "Copia di backup specificata è di un server diverso da quello corrente. Si consiglia di non eseguire un ripristino dello stato del sistema con il backup in un server alternativo, perché il server potrebbe diventare inutilizzabile. Sei sicuro che si desidera utilizzare questo backup per il ripristino del server corrente?"  
  
 Se è necessario ripristinare Active Directory per diversi tipi di hardware, creare il backup completo del server e si prevede di eseguire un ripristino completo del server.  
  
> [!IMPORTANT]
>  A partire da Windows Server 2008, non è supportato per ripristinare i backup dello stato del sistema a una nuova installazione di Windows Server nel nuovo hardware o lo stesso hardware. Se Windows Server viene reinstallato lo stesso hardware, come consigliato più avanti in questa Guida, è possibile ripristinare il controller di dominio in questo ordine:  
>   
>  1.  Eseguire un ripristino completo del server per ripristinare il sistema operativo e tutti i file e applicazioni.  
> 2.  Eseguire un ripristino dello stato del sistema utilizzando wbadmin.exe per contrassegnare SYSVOL come autorevole.  
>   
>  Per ulteriori informazioni, vedere Microsoft l'articolo [249694](https://support.microsoft.com/kb/249694).  
  
 Se l'ora dell'occorrenza dell'errore è sconosciuto, analizzare ulteriormente per identificare i backup che contengono l'ultimo stato di sicurezza della foresta. Questo approccio è meno opportuno. Pertanto, è consigliabile mantenere i registri dettagliati sullo stato di integrità di dominio Active Directory su base giornaliera, in modo che, se si verifica un errore a livello di foresta, è possibile individuare il tempo approssimativo di errore. È anche necessario mantenere una copia locale dei backup per attivare il ripristino più veloce.  
  
 Se il Cestino per Active Directory è abilitato, la durata di backup è uguale al **deletedObjectLifetime** valore o **tombstoneLifetime** valore, a seconda del valore è minore. Per ulteriori informazioni, vedere [Cestino per Active Directory Step-by-Step Guida](https://go.microsoft.com/fwlink/?LinkId=178657) (https://go.microsoft.com/fwlink/?LinkId=178657).  
  
 In alternativa, è possibile utilizzare anche la Active Directory database montaggio strumento (Dsamain.exe) e uno strumento di Lightweight Directory Access Protocol (LDAP), ad esempio Ldp.exe o Active Directory Users and Computers per identificare quali backup è l'ultimo stato di sicurezza della foresta. Lo strumento di montaggio del database di Active Directory, è incluso in Windows Server 2008 e versioni successive i sistemi operativi Windows Server, espone i dati di Active Directory che viene archiviati nel backup o gli snapshot come un server LDAP. Quindi, è possibile utilizzare uno strumento LDAP per visualizzare i dati. Questo approccio ha il vantaggio di non richiedere il riavvio di qualsiasi controller di dominio nel ripristino servizi Directory modalità () per esaminare il contenuto del backup di dominio Active Directory.  
  
 Per ulteriori informazioni sull'utilizzo di database di Active Directory strumento di montaggio, vedere il [strumento di montaggio di Active Directory Database Step-by-Step Guida](https://technet.microsoft.com/library/cc753609\(WS.10\).aspx).  
  
 È inoltre possibile utilizzare il **ntdsutil snapshot** comando per creare snapshot dei database di Active Directory. Per pianificare un'attività per creare periodicamente gli snapshot, è possibile ottenere ulteriori copie del database di Active Directory nel tempo. È possibile utilizzare queste copie per identificare con maggiore precisione quando si è verificato l'errore a livello di foresta e quindi scegli il backup ottimale da ripristinare. Per creare gli snapshot, utilizzare la versione di **ntdsutil** che viene fornito con Windows Server 2008 o amministrazione strumenti (remota del Server) per Windows Vista o versione successiva. Il controller di dominio di destinazione possa eseguire qualsiasi versione di Windows Server. Per ulteriori informazioni sull'utilizzo di **ntdsutil snapshot** comando, vedere [Snapshot](https://technet.microsoft.com/library/cc731620\(WS.10\).aspx).  
  
  
## <a name="determining-which-domain-controllers-to-restore"></a>Determinare quali controller di dominio da ripristinare  
 Accessibilità del processo di ripristino è un fattore importante quando si decide di ripristinare il controller di dominio. È consigliabile disporre di un controller di dominio dedicato per ogni dominio in cui il controller di dominio preferito per un ripristino. Un ripristino dedicati controller di dominio rende più semplice in modo affidabile pianificare ed eseguire il ripristino della foresta perché usi la stessa configurazione di origine utilizzato per eseguire Ripristino configurazione di test. È possibile creare uno script di ripristino e non competere con configurazioni diverse, ad esempio se il controller di dominio include operazioni di ruoli di master o non oppure che si tratti di un server di catalogo globale o DNS.  
  
> [!NOTE]
>  Mentre non è consigliabile ripristinare un titolare del ruolo di master operazioni per dare maggiore semplicità, alcune organizzazioni possono scegliere di ripristinare uno per altri vantaggi. Ad esempio il ripristino del master RID potrà aiutare a evitare problemi con la gestione dei RID durante il ripristino.  
  
 Scegliere un controller di dominio che meglio soddisfa i criteri seguenti:  
  
-   Un controller di dominio scrivibile. Questo campo è obbligatorio.  
  
-   Un controller di dominio che esegue Windows Server 2012 come una macchina virtuale in un hypervisor che supporta VM-GenerationID. Questo controller di dominio è utilizzabile come origine per la clonazione.  
  
-   Un controller di dominio che è accessibile, fisicamente o in una rete virtuale e preferibilmente si trova in un Data Center. In questo modo, si può facilmente isolarla dalla rete durante il ripristino dell'insieme di strutture.  
  
-   Un controller di dominio che dispone di un backup completo del server valido. Un backup valido è un backup che può essere ripristinato correttamente, è stato scattato alcuni giorni prima dell'errore e contiene come dati molto utili possibile.  
  
-   Un controller di dominio che è stato un server di sistema DNS (Domain Name) prima dell'errore. In questo modo il tempo necessario per reinstallare il DNS.  
  
-   Se inoltre si utilizza servizi di distribuzione Windows, scegliere un controller di dominio che non è configurato per utilizzare sblocco di rete via BitLocker. In questo caso, lo sblocco di rete di BitLocker non è supportato da utilizzare per il primo controller di dominio che si ripristina da backup durante un ripristino della foresta.  
  
     Sblocco rete con BitLocker come il *solo* protezione con chiave *Impossibile* utilizzabile nei controller di dominio in cui è stato distribuito servizi di distribuzione Windows (WDS) perché questa operazione genera uno scenario in cui è necessario il primo controller di dominio Active Directory e servizi di distribuzione Windows funzionare per sbloccare. Ma prima di ripristinare il primo controller di dominio, Active Directory non è ancora disponibile per servizi di distribuzione Windows, in modo che non può sbloccare.  
  
     Per determinare se un controller di dominio è configurato per utilizzare sblocco di rete via BitLocker, verificare che un certificato di sblocco di rete viene identificato nella chiave del Registro di sistema seguente:  
  
     HKEY_LOCAL_MACHINESoftwarePoliciesMicrosoftSystemCertificatesFVE_NKP  
  
 Gestire le procedure di protezione durante la manipolazione o il ripristino dei file di backup che includono Active Directory. L'urgenza che accompagna ripristino della foresta può causare involontariamente che si affaccia su procedure ottimali di protezione. Per ulteriori informazioni, vedere la sezione "Definizione di dominio Controller di Backup e ripristino configurazione di strategie di" [Guida alle procedure consigliate per la protezione delle installazioni Active Directory e operazioni Day: parte II](https://technet.microsoft.com/library/bb727066.aspx).  
  
## <a name="identify-the-current-forest-structure-and-dc-functions"></a>Identificare le strutture corrente e le funzioni di controller di dominio  
 Determinare la struttura della foresta corrente identificando tutti i domini nella foresta. Crea un elenco di tutti i controller di dominio in ogni dominio, in particolare il controller di dominio che dispone di backup e controller di dominio virtualizzati che può essere un'origine per la clonazione. Un elenco di controller di dominio per il dominio radice della foresta sarà il più importante perché si potrà ripristinare innanzitutto questo dominio. Dopo aver ripristinato il dominio radice della foresta, è possibile ottenere un elenco di altri domini, i controller di dominio e i siti della foresta utilizzando lo snap-in Active Directory.  
  
 Preparare una tabella che mostra le funzioni di ogni controller di dominio nel dominio, come illustrato nell'esempio seguente. Ciò consentirà di ripristinare la configurazione di pre-errore di dell'insieme di strutture dopo il ripristino.  
  
|Nome controller di dominio|Sistema operativo|FSMO|CATALOGO GLOBALE|CONTROLLER DI DOMINIO|Backup|DNS|Server Core|MACCHINA VIRTUALE|VM-GenID|  
|-------------|----------------------|----------|--------|----------|------------|---------|-----------------|--------|---------------|  
|DC_1|Windows Server 2012|Master schema master denominazione domini|Sì|No|Sì|No|No|Sì|Sì|  
|DC_2|Windows Server 2012|Nessuno|Sì|No|Sì|Sì|No|Sì|Sì|  
|DC_3|Windows Server 2012|Master infrastrutture|No|No|No|Sì|Sì|Sì|Sì|  
|DC_4|Windows Server 2012|Emulatore PDC, Master RID|Sì|No|No|No|No|Sì|No|  
|DC_5|Windows Server 2012|Nessuno|No|No|Sì|Sì|No|Sì|Sì|  
|RODC_1|Windows Server 2008 R2|Nessuno|Sì|Sì|Sì|Sì|Sì|Sì|No|  
|RODC_2|Windows Server 2008|Nessuno|Sì|Sì|No|Sì|Sì|Sì|No|  
  
 Per ogni dominio nella foresta, identificare un controller di dominio scrivibile singolo che dispone di un backup del database di Active Directory per il dominio trusted. Prestare attenzione quando si sceglie un backup per ripristinare un controller di dominio. Se il giorno e causa dell'errore circa sono noti, in generale è consigliabile consiste nell'usare una copia di backup che è stata effettuata alcuni giorni prima di tale data.  
  
 In questo esempio, sono disponibili quattro backup candidati: DC_1, DC_2, DC_4 e DC_5. Di questi backup candidati, si ripristina uno solo. Il controller di dominio consigliato è DC_5 per i motivi seguenti:  
  
-   Soddisfa i requisiti per l'utilizzo come origine per la clonazione controller di dominio virtualizzato, che è, viene eseguito Windows Server 2012 come un controller di dominio virtuale in un hypervisor che supporta VM-GenerationID, esegue software che è consentito per la clonazione (o che possono essere rimossi in questo caso non è in grado di essere clonato). Dopo il ripristino, il ruolo emulatore PDC verrà assegnato a che i server e possano essere aggiunti al gruppo di controller di dominio clonabili per il dominio.  
  
-   Esegue un'installazione completa di Windows Server 2012. Un controller di dominio che esegue un'installazione Server Core può risultare complesso come destinazione per il ripristino.  
  
-   È un server DNS. Di conseguenza, DNS non deve essere reinstallato.  
  
> [!NOTE]
>  Poiché DC_5 non è un server di catalogo globale, include anche un vantaggio, in quanto il catalogo globale non è necessario rimuovere dopo il ripristino. Ma se il controller di dominio è anche un server di catalogo globale non è un fattore determinante perché a partire da Windows Server 2012, tutti i controller di dominio sono server di catalogo globale per impostazione predefinita e la rimozione e aggiunta del catalogo globale dopo il ripristino è consigliabile come parte del processo di ripristino della foresta in ogni caso.  
  
## <a name="recover-the-forest-in-isolation"></a>Ripristinare la foresta in isolamento  
 Lo scenario preferito è per arrestare tutti i controller di dominio scrivibile prima il primo controller di dominio ripristinato è riportato nell'ambiente di produzione. Ciò garantisce che tutti i dati pericolosi non viene replicato indietro nell'insieme di strutture ripristinato. È particolarmente importante arrestare tutti i titolari del ruolo di master operazioni.  
  
> [!NOTE]
>  Potrebbero essere casi in cui spostare il primo controller di dominio che si intende ripristinare per ogni dominio per una rete isolata, consentendo di altri controller di dominio di rimanere in linea per ridurre al minimo i tempi di inattività del sistema. Ad esempio, se si ripristina da un aggiornamento dello schema non riuscita, è possibile mantenere i controller di dominio nella rete di produzione durante l'esecuzione di passaggi di ripristino in isolamento.  
  
 Se si eseguono i controller di dominio virtualizzati, è possibile spostarli a una rete virtuale che è isolata dalla rete di produzione in cui si eseguirà il ripristino. Lo spostamento di controller di dominio virtualizzati in una rete separata offre due vantaggi:  
  
-   I controller di dominio ripristinato impedito ripresentarsi del problema che ha causato il ripristino della foresta, perché sono isolati.  
  
-   La clonazione del controller di dominio virtualizzati possono essere eseguita in rete separata, in modo che un numero critico di controller di dominio può eseguire e testare prima che vengono ripristinati alla rete di produzione.  
  
 Se si eseguono i controller di dominio in un hardware fisico, disconnettere il cavo di rete del primo controller di dominio che si intende ripristinare nel dominio radice della foresta. Se possibile, anche disconnettere i cavi di rete di tutti gli altri controller di dominio. Ciò impedisce controller di dominio di replica, se sono state avviate accidentalmente durante il processo di ripristino della foresta.  
  
 In una foresta di grandi dimensione distribuita su più percorsi, può essere difficile garantire che tutti i controller di dominio scrivibile vengono arrestati. Per questo motivo, le operazioni di ripristino, ad esempio reimpostare l'account computer e account krbtgt, inoltre la pulizia dei metadati, sono progettate per garantire che i controller di dominio scrivibile ripristinato non replicano con i controller di dominio scrivibile pericoloso (nel caso in cui alcuni sono ancora online nella foresta).  
  
 Tuttavia, solo da portare offline i controller di dominio scrivibile possibile si garantisce che la replica non viene eseguito? Pertanto, quando possibile, è necessario distribuire tecnologia di gestione remota che consentono di arrestare e isolare fisicamente i controller di dominio scrivibile durante il ripristino dell'insieme di strutture.  
  
 Lettura può continuare a funzionare anche se i controller di dominio scrivibili sono offline. Nessun altro controller di dominio verrà direttamente replicare le modifiche da qualsiasi controller di dominio, in particolare, nessuna modifica contenitore dello Schema o alla configurazione, in modo non determinino il rischio stesso come controller di dominio scrivibile durante il ripristino. Dopo che tutti i controller di dominio scrivibili vengono ripristinati e online, è necessario ricostruire tutti i controller.  
  
 Lettura continuerà a consentire l'accesso alle risorse locali che vengono memorizzate nella cache nei rispettivi siti mentre le operazioni di ripristino in corso in parallelo. Le risorse locali che non vengono memorizzate nella cache nel RODC avranno le richieste di autenticazione inoltrate a un controller di dominio scrivibile. Queste richieste avrà esito negativo perché i controller di dominio scrivibili sono offline. Alcune operazioni, ad esempio le modifiche delle password non funzionerà anche fino a quando non si ripristina i controller di dominio scrivibile.  
  
 Se si utilizza un'architettura di rete hub e spoke, è possibile concentrarsi prima di tutto per il ripristino del controller di dominio scrivibile in siti hub. In un secondo momento, è possibile ricreare il controller di siti remoti.  
  
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
