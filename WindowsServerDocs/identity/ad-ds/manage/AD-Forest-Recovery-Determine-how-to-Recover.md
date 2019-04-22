---
title: Ripristino della foresta Active Directory - determinare come eseguire il ripristino della foresta
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 30d23d977b4d7cd320d78ff340120df7013dee4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817732"
---
# <a name="determine-how-to-recover-the-forest"></a>Determinare come eseguire il ripristino della foresta

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Ripristinare un'intera foresta di Active Directory prevede ripristinarlo da un backup o reinstallare Servizi di dominio Active Directory (AD DS) in ogni controller di dominio (DC) nella foresta. Il ripristino della foresta Ripristina ogni dominio nella foresta stato al momento dell'ultimo backup attendibile. Di conseguenza, l'operazione di ripristino comporterà la perdita di almeno i dati di Active Directory seguenti:

- Tutti gli oggetti che sono stati aggiunti dopo l'ultimo backup attendibile (ad esempio utenti e computer)
- Tutti gli aggiornamenti che sono stato apportati agli oggetti esistenti poiché attendibile l'ultimo backup
- Tutte le modifiche apportate dall'ultimo backup attendibile per la partizione di configurazione o la partizione dello schema in Active Directory Domain Services (ad esempio, le modifiche dello schema)

Per ogni dominio nella foresta, la password di un account amministratore di dominio deve essere noto. Preferibilmente, si tratta della password dell'account amministratore predefinito. È anche necessario conoscere la password modalità ripristino servizi directory per eseguire un ripristino dello stato del sistema di un controller di dominio. In generale, è consigliabile archiviare l'account amministratore e la cronologia delle password modalità ripristino servizi directory in un luogo sicuro per, purché i backup sono validi, vale a dire, entro il periodo di durata oggetto contrassegnato per rimozione o entro la durata dell'oggetto eliminato se Active Directory riciclo Bin è abilitata. È anche possibile sincronizzare la password DSRM con un account utente di dominio per renderlo più facile da ricordare. Per altre informazioni, vedere l'articolo della KB [961320](https://support.microsoft.com/kb/961320). La sincronizzazione dell'account della modalità ripristino servizi directory deve essere eseguita prima che il ripristino della foresta, nell'ambito della preparazione.

> [!NOTE]
> L'account amministratore è un membro del gruppo Administrators predefinito per impostazione predefinita, perché sono i gruppi Enterprise Admins e Domain Admins. Questo gruppo dispone del controllo completo di tutti i controller di dominio nel dominio.

## <a name="determining-which-backups-to-use"></a>Determinazione di backup da usare

Almeno due controller di dominio scrivibile per ogni dominio eseguire regolarmente il backup in modo che si dispone di diversi backup tra cui scegliere. Si noti che è possibile usare il backup di un controller di dominio di sola lettura (RODC) per ripristinare un controller di dominio scrivibile. È consigliabile ripristinare il controller di dominio tramite i backup che sono stati eseguiti alcuni giorni prima dell'errore. In generale, è necessario stabilire un buon compromesso tra il recentness safeness i dati ripristinati. Scelta di un backup più recente consente di recuperare dati più utili, ma potrebbe aumentare il rischio di reintroduzione dati pericolosi nell'insieme di strutture ripristinato.

Il ripristino di backup dello stato del sistema varia a seconda del sistema operativo e i server di backup originale. Ad esempio, non è necessario ripristinare un backup dello stato del sistema in un server diverso. In questo caso, viene visualizzato l'avviso seguente:

"Il backup specificato è di un server diverso da quello corrente. Non è consigliabile eseguire un ripristino dello stato di sistema con il backup in un server alternativo, perché il server potrebbe diventare inutilizzabile. Continuare che si desidera utilizzare questo backup per il ripristino del server corrente?"

Se si vuole ripristinare Active Directory su un hardware diverso, creare i backup completo del server e si prevede di eseguire un ripristino completo del server.

> [!IMPORTANT]
> A partire da Windows Server 2008, non è supportato per ripristinare i backup dello stato del sistema in una nuova installazione di Windows Server nel nuovo hardware o lo stesso hardware. Se Windows Server viene reinstallato sullo stesso hardware, come consigliato più avanti in questa Guida, è possibile ripristinare il controller di dominio nell'ordine indicato:
>
> 1. Eseguire un ripristino completo del server per ripristinare il sistema operativo e tutti i file e applicazioni.
> 2. Eseguire un ripristino dello stato del sistema tramite wbadmin.exe per contrassegna SYSVOL come autorevole.
>
> Per altre informazioni, vedere Microsoft Knowledge Base l'articolo [249694](https://support.microsoft.com/kb/249694).

Se l'ora dell'occorrenza dell'errore è sconosciuto, indagare ulteriormente per identificare i backup che contengono l'ultimo stato sicura della foresta. Questo approccio è meno consigliato. Pertanto, è consigliabile mantenere i log dettagliati sullo stato di integrità di dominio Active Directory su base giornaliera, in modo che, se si verifica un errore a livello di foresta, l'ora approssimativa dell'errore può essere identificato. È anche necessario conservare una copia locale dei backup per consentire il ripristino più veloce.

Se al Cestino per Active Directory è abilitato, la durata di backup è uguale al **deletedObjectLifetime** valore o il **tombstoneLifetime** valore, a seconda del valore è minore. Per altre informazioni, vedere [Active Directory Guida dettagliata al Cestino](https://go.microsoft.com/fwlink/?LinkId=178657) (https://go.microsoft.com/fwlink/?LinkId=178657).

In alternativa, è anche possibile usare lo strumento di montaggio di database di Active Directory (Dsamain.exe) e uno strumento di Lightweight Directory Access Protocol (LDAP), ad esempio Ldp.exe o Active Directory Users and Computers per identificare quali backup è l'ultimo stato sicuro del insieme di strutture. Lo strumento di montaggio di database di Active Directory, incluso in Windows Server 2008 e versioni successive i sistemi operativi Windows Server, espone i dati di Active Directory che viene archiviati nel backup o snapshot come un server LDAP. Quindi, è possibile usare uno strumento LDAP per esplorare i dati. Questo approccio presenta il vantaggio di non richiedere il riavvio di qualsiasi controller di dominio nel ripristino servizi Directory modalità () per esaminare il contenuto del backup di Active Directory Domain Services.

Per altre informazioni sull'uso di database di Active Directory strumento di montaggio, vedere la [Guida dettagliata Active Directory Database montaggio strumento](https://technet.microsoft.com/library/cc753609\(WS.10\).aspx).

È anche possibile usare la **ntdsutil snapshot** comando per creare gli snapshot del database di Active Directory. Pianificazione di un'attività per la creazione periodica di snapshot, è possibile ottenere copie aggiuntive del database di Active Directory nel tempo. È possibile utilizzare queste copie per identificare più facilmente quando si è verificato l'errore a livello di foresta e quindi scegliere il migliore backup da ripristinare. Per creare gli snapshot, usare la versione di **ntdsutil** fornito con Windows Server 2008 o amministrazione strumenti (remota del Server) per Windows Vista o versioni successive. Il controller di dominio di destinazione può eseguire qualsiasi versione di Windows Server. Per altre informazioni sull'uso di **snapshot ntdsutil** comando, vedere [Snapshot](https://technet.microsoft.com/library/cc731620\(WS.10\).aspx).

## <a name="determining-which-domain-controllers-to-restore"></a>Determinare quali controller di dominio da ripristinare

Semplicità del processo di ripristino è un fattore importante quando si decide quale controller di dominio da ripristinare. È consigliabile disporre di un controller di dominio dedicato per ogni dominio che è il controller di dominio preferito per un ripristino. Un ripristino dedicato DC semplifica in modo affidabile pianificare ed eseguire il ripristino della foresta, perché si usa la stessa configurazione di origine che è stata usata per eseguire Ripristino configurazione di test. È possibile creare uno script di ripristino ed entrino in conflitto con configurazioni diverse, ad esempio se il controller di dominio contiene operazioni di ruoli di master o non o se è un server di catalogo globale o DNS o meno.

> [!NOTE]
> Anche se non è consigliabile ripristinare un titolare del ruolo di master operazioni per ragioni di semplicità, in alcune organizzazioni potrebbero scegliere di ripristinare uno per gli altri vantaggi. Ad esempio il ripristino del master RID può aiutare a evitare problemi relativi alla gestione dei RID durante il ripristino.  

Scegliere un controller di dominio che meglio soddisfa i criteri seguenti:

- Un controller di dominio scrivibile. Questo campo è obbligatorio.

- Un controller di dominio che esegue Windows Server 2012 come una macchina virtuale in un hypervisor che supporta VM-GenerationID. Questo controller di dominio è utilizzabile come origine per la clonazione.
- Un controller di dominio che sia accessibile, fisicamente o in una rete virtuale e preferibilmente che si trova in un Data Center. In questo modo, è possibile isolare facilmente, dalla rete durante il ripristino della foresta.
- Un controller di dominio che dispone di un backup completo del server valido. Un backup valido è un backup che può essere ripristinato correttamente, è stato impiegato alcuni giorni prima dell'errore e contiene dati molto utile possibile.
- Un controller di dominio che è stato un server di sistema DNS (Domain Name) prima dell'errore. Ciò consente di risparmiare il tempo necessario per reinstallare il DNS.
- Se inoltre si usa servizi di distribuzione Windows, scegliere un controller di dominio che non è configurato per utilizzare sblocco di rete via BitLocker. In questo caso, sblocco di rete via BitLocker non è supportato da utilizzare per il primo controller di dominio che si ripristina da backup durante un ripristino della foresta.

   Sblocco rete con BitLocker come le *solo* protezione con chiave *non è possibile* utilizzabile nei controller di dominio in cui è stato distribuito servizi di distribuzione Windows (WDS) perché tale operazione, viene generato in uno scenario in cui è necessario il primo controller di dominio Active Directory e servizi di distribuzione Windows a lavorare per lo sblocco. Ma prima di ripristinare il primo controller di dominio, Active Directory non è ancora disponibile per servizi di distribuzione Windows, in modo che non può sbloccare.

   Per determinare se un controller di dominio è configurato per utilizzare sblocco di rete via BitLocker, verificare che un certificato di sblocco di rete è identificato nella chiave del Registro di sistema seguente:

   HKEY_LOCAL_MACHINESoftwarePoliciesMicrosoftSystemCertificatesFVE_NKP

Gestire le procedure di sicurezza durante la gestione o il ripristino di file di backup che includono Active Directory. L'urgenza che accompagna ripristino della foresta può causare inavvertitamente trascurare le procedure consigliate. Per altre informazioni, vedere la sezione intitolata "Definizione di dominio Controller e il ripristino strategie di Backup" in [migliore Guida alle procedure consigliate per la sicurezza delle installazioni Active Directory e Day operazioni: Parte II](https://technet.microsoft.com/library/bb727066.aspx).

## <a name="identify-the-current-forest-structure-and-dc-functions"></a>Identificare le strutture corrente e le funzioni di controller di dominio

Determinare la struttura della foresta corrente identificando tutti i domini nella foresta. Creare un elenco di tutti i controller di dominio in ogni dominio, in particolare i controller di dominio che dispongono di backup e controller di dominio virtualizzati, che può essere un'origine per la clonazione. Un elenco di controller di dominio per il dominio radice della foresta sarà il più importante in quanto si ripristinerà innanzitutto questo dominio. Dopo aver ripristinato il dominio radice della foresta, è possibile ottenere un elenco di altri domini, i controller di dominio e i siti nella foresta usando gli snap-in Active Directory.

Preparare una tabella che mostra le funzioni di ogni controller di dominio nel dominio, come illustrato nell'esempio seguente. Ciò consentirà di ripristinare la configurazione di errore non definitiva della foresta dopo il ripristino.

|Nome del controller di dominio|Sistema operativo|FSMO|GC|Controller di dominio di sola lettura|Backup|DNS|Server Core|VM|VM-GenID|  
|-------------|----------------------|----------|--------|----------|------------|---------|-----------------|--------|---------------|  
|DC_1|Windows Server 2012|Master schema, master denominazione domini|Yes|No|Yes|No|No|Yes|Yes|  
|DC_2|Windows Server 2012|Nessuno|Yes|No|Yes|Yes|No|Yes|Yes|  
|DC_3|Windows Server 2012|Master infrastrutture|No|No|No|Yes|Yes|Yes|Yes|  
|DC_4|Windows Server 2012|Emulatore PDC, Master RID|Yes|No|No|No|No|Yes|No|  
|DC_5|Windows Server 2012|Nessuno|No|No|Yes|Yes|No|Yes|Yes|  
|RODC_1|Windows Server 2008 R2|Nessuno|Yes|Yes|Yes|Yes|Yes|Yes|No|  
|RODC_2|Windows Server 2008|Nessuno|Yes|Yes|No|Yes|Yes|Yes|No|  

Per ogni dominio nella foresta, identificare un controller di dominio scrivibile single con un backup del database di Active Directory per il dominio trusted. Prestare attenzione quando si sceglie un backup per ripristinare un controller di dominio. Se il giorno e la causa dell'errore circa è noto, la raccomandazione generale consiste nell'utilizzare un backup eseguito alcuni giorni prima di tale data.
  
In questo esempio, sono disponibili quattro backup candidati: DC_1 DC_2, DC_4 e DC_5. Di questi candidati di backup, si ripristina uno solo. Il controller di dominio consigliato è DC_5 per i motivi seguenti:  

- Soddisfa i requisiti per l'uso come origine per la clonazione controller di dominio virtualizzati, che è, viene eseguito Windows Server 2012, mentre è in esecuzione un controller di dominio virtuale in un hypervisor che supporta VM-GenerationID e software che può essere clonato (o che può essere rimosso se è non in grado di essere clone d). Dopo il ripristino, il ruolo emulatore PDC verrà riassegnato al che server e possono essere aggiunti al gruppo di controller di dominio clonabili per il dominio.  
- Esegue un'installazione completa di Windows Server 2012. Un controller di dominio che esegue un'installazione Server Core può essere meno efficaci come destinazione per il ripristino.  
- È un server DNS. Pertanto, DNS non deve essere reinstallato.  

> [!NOTE]
> Poiché DC_5 non è un server di catalogo globale, include anche un vantaggio, in quanto il catalogo globale non dovranno essere rimossi dopo il ripristino. Ma se il controller di dominio è anche un server di catalogo globale non è un fattore determinante perché a partire da Windows Server 2012, tutti i controller di dominio sono i server di catalogo globale per impostazione predefinita e la rimozione e aggiunta di catalogo globale dopo il ripristino è consigliabile come parte della foresta ripristino di elaborare in ogni caso.  

## <a name="recover-the-forest-in-isolation"></a>Ripristino della foresta in modalità isolata

Lo scenario preferito è arrestare tutti i controller di dominio scrivibile prima il primo controller di dominio ripristinato viene rimesso in produzione. Ciò garantisce che tutti i dati pericolosi non viene replicato nuovamente nella foresta recuperata. È particolarmente importante arrestare tutti i titolari ruolo di master operazioni.  

> [!NOTE]
> Potrebbero esserci casi in cui spostare il primo controller di dominio che si intende ripristinare per ogni dominio a una rete isolata, consentendo di altri controller di dominio di rimanere in linea per ridurre al minimo i tempi di inattività del sistema. Ad esempio, se si ripristina da un aggiornamento dello schema non riuscita, è possibile scegliere di mantenere i controller di dominio della rete di produzione durante l'esecuzione della procedura di ripristino in isolamento.

Se si eseguono i controller di dominio virtualizzato, è possibile spostarli a una rete virtuale isolata dalla rete di produzione in cui si eseguirà il ripristino. Lo spostamento di controller di dominio virtualizzati in una rete separata offre due vantaggi:  

- I controller di dominio ripristinato è stata impedita ripresentarsi del problema che ha causato il ripristino della foresta, in quanto sono isolati.  
- La clonazione di controller di dominio virtualizzati possono essere eseguita sulla rete separata in modo che può essere in esecuzione un numero critico dei controller di dominio e testata prima che vengano aperte nuovamente alla rete di produzione.

Se si eseguono i controller di dominio su hardware fisico, disconnettere il cavo di rete del primo controller di dominio che si intende ripristinare nel dominio radice della foresta. Se possibile, inoltre disconnettere i cavi di rete di tutti gli altri controller di dominio. Ciò impedisce i controller di dominio esegue la replica, se sono state avviate accidentalmente durante il processo di ripristino della foresta.  

In una foresta di grandi dimensioni che viene distribuita tra più sedi, può essere difficile garantire che tutti i controller di dominio scrivibili vengono arrestate. Per questo motivo, la procedura di ripristino, ovvero come reimpostare l'account del computer e l'account krbtgt, anche per eseguire la pulizia dei metadati, sono progettate per garantire che i controller di dominio scrivibile ripristinati non verranno replicate con controller di dominio scrivibile pericoloso (nel caso in cui alcuni sono ancora online nel foresta).  
  
Tuttavia, solo da portare offline i controller di dominio scrivibile può si garantisce che la replica non viene eseguito. Pertanto, quando possibile, è consigliabile distribuire tecnologia di gestione remota che consentono di arrestare e isolare fisicamente i controller di dominio scrivibile durante il ripristino della foresta.  
  
Questi controller possono continuare a funzionare anche se i controller di dominio scrivibili sono offline. Nessun altro controller di dominio direttamente replicare le modifiche da qualsiasi controller di dominio, in particolare, nessuna modifica contenitore dello Schema o la configurazione, in modo da non determinino il rischio stesso come controller di dominio scrivibile durante il ripristino. Dopo che tutti i controller di dominio scrivibili vengono ripristinati e online, è necessario ricompilare tutti i controller.  
  
Questi controller continueranno a consentire l'accesso alle risorse locali che vengono memorizzati nella cache nei rispetti siti durante le operazioni di ripristino sono attività in parallelo. Le risorse locali che non vengono memorizzate nel RODC avrà le richieste di autenticazione inoltrate a un controller di dominio scrivibile. Queste richieste avrà esito negativo perché i controller di dominio scrivibili sono offline. Alcune operazioni, ad esempio modifiche delle password non funzioneranno fino a quando non si ripristinano i controller di dominio scrivibile.  
  
Se si usa un'architettura di rete hub-spoke, è possibile concentrarsi prima di controller di dominio scrivibili nei siti di hub per il recupero. In un secondo momento, è possibile ricompilare il RODC in siti remoti.  
  
## <a name="next-steps"></a>Passaggi successivi

- [Ripristino della foresta Active Directory - prerequisiti](AD-Forest-Recovery-Prerequisties.md)  
- [Ripristino della foresta Active Directory - concepire un piano di ripristino personalizzata-foresta](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Ripristino della foresta Active Directory - identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Ripristino della foresta Active Directory - determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Ripristino della foresta Active Directory - eseguire il ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)  
- [Ripristino della foresta Active Directory - domande frequenti](AD-Forest-Recovery-FAQ.md)  
- [Ripristino della foresta Active Directory - il ripristino di un singolo dominio all'interno di un Multidomain foresta](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Ripristino della foresta Active Directory - ripristino della foresta con controller di dominio di Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
