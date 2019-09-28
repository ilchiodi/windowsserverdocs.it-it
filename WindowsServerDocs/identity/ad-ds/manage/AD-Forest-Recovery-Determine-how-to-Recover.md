---
title: Ripristino della foresta di Active Directory-determinare come ripristinare la foresta
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: d604efded5b6a2ff3911a92f52817498f43c9933
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369170"
---
# <a name="determine-how-to-recover-the-forest"></a>Determinare come ripristinare la foresta

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Il ripristino di un'intera foresta Active Directory comporta il ripristino dal backup o la reinstallazione di Active Directory Domain Services (AD DS) in ogni controller di dominio nella foresta. Il ripristino della foresta consente di ripristinare lo stato di ogni dominio della foresta al momento dell'ultimo backup attendibile. Di conseguenza, l'operazione di ripristino comporterà la perdita di almeno i dati Active Directory seguenti:

- Tutti gli oggetti (ad esempio utenti e computer) aggiunti dopo l'ultimo backup attendibile
- Tutti gli aggiornamenti apportati agli oggetti esistenti dall'ultimo backup attendibile
- Tutte le modifiche apportate alla partizione di configurazione o alla partizione dello schema in servizi di dominio Active Directory, ad esempio modifiche dello schema, dall'ultimo backup attendibile

Per ogni dominio nella foresta, è necessario conoscere la password di un account amministratore di dominio. Preferibilmente, si tratta della password dell'account Administrator predefinito. Per eseguire un ripristino dello stato del sistema di un controller di dominio, è inoltre necessario conoscerne la password. In generale, è consigliabile archiviare l'account amministratore e la cronologia delle password della modalità ripristino servizi directory in un luogo sicuro per il periodo di tempo in cui i backup sono validi, ovvero entro il periodo di durata della rimozione definitiva o entro il periodo di durata degli oggetti eliminati se Active Directory riciclo Il contenitore è abilitato. È anche possibile sincronizzare la password della modalità ripristino servizi directory con un account utente di dominio per renderla più semplice da ricordare. Per ulteriori informazioni, vedere l'articolo KB [961320](https://support.microsoft.com/kb/961320). La sincronizzazione dell'account della modalità ripristino servizi directory deve essere eseguita prima del ripristino della foresta, come parte della preparazione.

> [!NOTE]
> Per impostazione predefinita, l'account Administrator è un membro del gruppo Administrators predefinito, così come i gruppi Domain Admins ed Enterprise Admins. Questo gruppo dispone del controllo completo di tutti i controller di dominio nel dominio.

## <a name="determining-which-backups-to-use"></a>Determinazione dei backup da usare

Eseguire regolarmente il backup di almeno due controller di dominio scrivibili per ogni dominio, in modo da poter scegliere tra più backup. Si noti che non è possibile utilizzare il backup di un controller di dominio di sola lettura (RODC) per ripristinare un controller di dominio scrivibile. Si consiglia di ripristinare i controller di dominio utilizzando i backup che sono stati eseguiti alcuni giorni prima del verificarsi dell'errore. In generale, è necessario determinare un compromesso tra la recentità e la sicurezza dei dati ripristinati. La scelta di un backup più recente consente di recuperare dati più utili, ma potrebbe aumentare il rischio di reintrodurre dati pericolosi nella foresta ripristinata.

Il ripristino dei backup dello stato del sistema dipende dal sistema operativo e dal server originale del backup. Ad esempio, non è consigliabile ripristinare un backup dello stato del sistema in un server diverso. In questo caso, è possibile che venga visualizzato il seguente avviso:

"Il backup specificato è di un server diverso rispetto a quello corrente. Non è consigliabile eseguire un ripristino dello stato del sistema con il backup in un server alternativo perché il server potrebbe diventare inutilizzabile. Utilizzare questo backup per il ripristino del server corrente? "

Se è necessario ripristinare Active Directory su hardware diverso, creare backup completi del server e pianificare l'esecuzione di un ripristino completo del server.

> [!IMPORTANT]
> A partire da Windows Server 2008, non è supportato per ripristinare il backup dello stato del sistema in una nuova installazione di Windows Server su un nuovo hardware o sullo stesso hardware. Se Windows Server viene reinstallato nello stesso hardware, come consigliato più avanti in questa guida, è possibile ripristinare il controller di dominio nell'ordine seguente:
>
> 1. Eseguire un ripristino completo del server per ripristinare il sistema operativo e tutti i file e le applicazioni.
> 2. Eseguire un ripristino dello stato del sistema utilizzando WBADMIN. exe per contrassegnare SYSVOL come autorevole.
>
> Per ulteriori informazioni, vedere l'articolo [249694](https://support.microsoft.com/kb/249694)della Microsoft Knowledge Knowledge.

Se l'ora dell'occorrenza dell'errore è sconosciuta, controllare ulteriormente per identificare i backup che contengono l'ultimo stato di sicurezza della foresta. Questo approccio è meno appropriato. Pertanto, si consiglia di tenere i log dettagliati sullo stato di integrità di servizi di dominio Active Directory su base giornaliera, in modo che, se si verifica un errore a livello di foresta, sia possibile identificare l'ora approssimativa dell'errore. È inoltre consigliabile tenere una copia locale dei backup per consentire un ripristino più veloce.

Se Active Directory Cestino è abilitato, la durata del backup è uguale al valore di **deletedObjectLifetime** o al valore **tombstoneLifetime** , a seconda del valore minore. Per ulteriori informazioni, vedere la [Guida dettagliata al cestino Active Directory](https://go.microsoft.com/fwlink/?LinkId=178657) (https://go.microsoft.com/fwlink/?LinkId=178657).

In alternativa, è anche possibile usare lo strumento di montaggio del database Active Directory (dsamain. exe) e uno strumento LDAP (Lightweight Directory Access Protocol), ad esempio LDP. exe o Active Directory utenti e computer, per identificare il backup con l'ultimo stato sicuro del foresta. Lo strumento di montaggio Active Directory database, incluso nei sistemi operativi Windows Server 2008 e versioni successive, espone Active Directory dati archiviati in backup o snapshot come server LDAP. Quindi, è possibile usare uno strumento LDAP per esplorare i dati. Questo approccio ha il vantaggio di non richiedere il riavvio del controller di dominio in modalità ripristino servizi directory per esaminare il contenuto del backup di servizi di dominio Active Directory.

Per ulteriori informazioni sull'utilizzo dello strumento di montaggio Active Directory database, vedere la [Guida dettagliata allo strumento di montaggio dei database Active Directory](https://technet.microsoft.com/library/cc753609\(WS.10\).aspx).

È inoltre possibile utilizzare il comando **Ntdsutil snapshot** per creare snapshot del database Active Directory. Pianificando un'attività per creare periodicamente gli snapshot, è possibile ottenere copie aggiuntive del database Active Directory nel tempo. È possibile utilizzare queste copie per identificare meglio il momento in cui si è verificato l'errore a livello di foresta, quindi scegliere il backup migliore da ripristinare. Per creare gli snapshot, utilizzare la versione di **Ntdsutil** fornita con windows Server 2008 o il strumenti di amministrazione remota del server (strumenti di amministrazione remota) per Windows Vista o versione successiva. Il controller di dominio di destinazione può eseguire qualsiasi versione di Windows Server. Per ulteriori informazioni sull'utilizzo del comando **Ntdsutil snapshot** , vedere [snapshot](https://technet.microsoft.com/library/cc731620\(WS.10\).aspx).

## <a name="determining-which-domain-controllers-to-restore"></a>Determinazione dei controller di dominio da ripristinare

La facilità del processo di ripristino è un fattore importante nella scelta del controller di dominio da ripristinare. È consigliabile disporre di un controller di dominio dedicato per ogni dominio che rappresenta il controller di dominio preferito per un ripristino. Un controller di dominio di ripristino dedicato semplifica la pianificazione e l'esecuzione del ripristino della foresta in modo affidabile perché si usa la stessa configurazione di origine usata per eseguire i test di ripristino. È possibile generare uno script per il ripristino e non contendersi con configurazioni diverse, ad esempio se il controller di dominio include o meno i ruoli master operazioni o se si tratta di un server GC o DNS.

> [!NOTE]
> Sebbene non sia consigliabile ripristinare un titolare del ruolo di master operazioni per semplicità, è possibile che alcune organizzazioni scelgano di ripristinarne una per altri vantaggi. Ad esempio, il ripristino del master RID può contribuire a evitare problemi di gestione di RID durante il ripristino.  

Scegliere un controller di dominio che soddisfi al meglio i criteri seguenti:

- Un controller di dominio scrivibile. Questa operazione è obbligatoria.

- Un controller di dominio che esegue Windows Server 2012 come macchina virtuale in un hypervisor che supporta VM-generazione. Questo controller di dominio può essere usato come origine per la clonazione.
- Un controller di dominio accessibile, fisicamente o in una rete virtuale e preferibilmente situato in un Data Center. In questo modo, è possibile isolarlo facilmente dalla rete durante il ripristino della foresta.
- Un controller di dominio che dispone di un backup completo del server valido. Un backup valido è un backup che è possibile ripristinare correttamente, è stato eseguito alcuni giorni prima dell'errore e contiene il maggior numero di dati utili possibile.
- Un controller di dominio che era un server DNS (Domain Name System) prima dell'errore. Questo consente di risparmiare il tempo necessario per reinstallare DNS.
- Se si usa anche servizi di distribuzione Windows, scegliere un controller di dominio non configurato per l'uso di sblocco di rete di BitLocker. In questo caso, lo sblocco di rete di BitLocker non è supportato per il primo controller di dominio ripristinato dal backup durante il ripristino di una foresta.

   Lo sblocco di rete di BitLocker come *unica* protezione con chiave *non può* essere utilizzato nei controller di dominio in cui è stato distribuito Servizi di distribuzione Windows (WDS) perché in questo modo si ottiene uno scenario in cui il primo controller di dominio richiede il funzionamento di Active Directory e WDS per sbloccare. Tuttavia, prima di ripristinare il primo controller di dominio, Active Directory non è ancora disponibile per WDS, quindi non può essere sbloccato.

   Per determinare se un controller di dominio è configurato per l'utilizzo di sblocco di rete BitLocker, verificare che nella seguente chiave del registro di sistema venga identificato un certificato di sblocco di rete:

   HKEY_LOCAL_MACHINESoftwarePoliciesMicrosoftSystemCertificatesFVE_NKP

Gestire le procedure di sicurezza per la gestione o il ripristino dei file di backup che includono Active Directory. L'urgenza che accompagna il ripristino della foresta può compromettere involontariamente le procedure consigliate per la sicurezza. Per ulteriori informazioni, vedere la sezione intitolata "definizione delle strategie di backup e ripristino del controller di dominio" nella Guida alle procedure di @no__t 0Best per la protezione di installazioni Active Directory e operazioni quotidiane: Parte II @ no__t-0.

## <a name="identify-the-current-forest-structure-and-dc-functions"></a>Identificare la struttura e le funzioni del controller di dominio correnti

Determinare la struttura della foresta corrente identificando tutti i domini della foresta. Creare un elenco di tutti i controller di dominio in ogni dominio, in particolare i controller di dominio con backup e DCs virtualizzati che possono essere un'origine per la clonazione. Un elenco di controller di dominio per il dominio radice della foresta sarà il più importante perché il dominio verrà recuperato per primo. Dopo il ripristino del dominio radice della foresta, è possibile ottenere un elenco degli altri domini, dei controller di dominio e dei siti nella foresta utilizzando Active Directory snap-in.

Preparare una tabella che mostra le funzioni di ogni controller di dominio nel dominio, come illustrato nell'esempio seguente. Ciò consentirà di ripristinare la configurazione di pre-errore della foresta dopo il ripristino.

|Nome controller di dominio|Sistema operativo|FSMO|GC|Controller di dominio di sola lettura|Backup|DNS|Server Core|VM|Con GenId VM|  
|-------------|----------------------|----------|--------|----------|------------|---------|-----------------|--------|---------------|  
|DC_1|Windows Server 2012|Master schema, Master per la denominazione dei domini|Yes|No|Yes|No|No|Yes|Yes|  
|DC_2|Windows Server 2012|Nessuno|Yes|No|Yes|Yes|No|Yes|Yes|  
|DC_3|Windows Server 2012|Master infrastrutture|No|No|No|Yes|Yes|Yes|Yes|  
|DC_4|Windows Server 2012|Emulatore PDC, master RID|Yes|No|No|No|No|Yes|No|  
|DC_5|Windows Server 2012|Nessuno|No|No|Yes|Yes|No|Yes|Yes|  
|RODC_1|Windows Server 2008 R2|Nessuno|Yes|Yes|Yes|Yes|Yes|Yes|No|  
|RODC_2|Windows Server 2008|Nessuno|Yes|Yes|No|Yes|Yes|Yes|No|  

Per ogni dominio nella foresta, identificare un singolo controller di dominio scrivibile che disponga di un backup attendibile del database Active Directory per quel dominio. Prestare attenzione quando si sceglie un backup per ripristinare un controller di dominio. Se il giorno e la motivo dell'errore sono circa noti, l'indicazione generale prevede l'uso di un backup eseguito pochi giorni prima di tale data.
  
In questo esempio sono disponibili quattro candidati di backup: DC_1, DC_2, DC_4 e DC_5. Di questi candidati di backup, è possibile ripristinarne solo uno. Il controller di dominio consigliato è DC_5 per i motivi seguenti:  

- Soddisfa i requisiti per l'uso come origine per la clonazione del controller di dominio virtualizzato, ovvero esegue Windows Server 2012 come controller di dominio virtuale in un hypervisor che supporta VM-generazione, esegue software che può essere clonato (o che può essere rimosso se non è possibile clonare). d). Dopo il ripristino, il ruolo emulatore PDC verrà sequestrato a tale server e potrà essere aggiunto al gruppo controller di dominio clonabili per il dominio.  
- Esegue un'installazione completa di Windows Server 2012. Un controller di dominio che esegue un'installazione dei componenti di base del server può essere meno comodo come destinazione per il ripristino.  
- Si tratta di un server DNS. Di conseguenza, non è necessario reinstallare il DNS.  

> [!NOTE]
> Poiché DC_5 non è un server di catalogo globale, presenta anche un vantaggio nel fatto che non è necessario rimuovere il catalogo globale dopo il ripristino. Tuttavia, indipendentemente dal fatto che il controller di dominio sia anche un server di catalogo globale non è un fattore decisivo perché a partire da Windows Server 2012, tutti i controller di dominio sono server di catalogo globale per impostazione predefinita e la rimozione e l'aggiunta del catalogo globale dopo il ripristino sono consigliate come parte della foresta processo di ripristino in qualsiasi caso.  

## <a name="recover-the-forest-in-isolation"></a>Ripristinare la foresta in isolamento

Lo scenario preferito consiste nell'arrestare tutti i controller di dominio scrivibili prima che il primo controller di dominio ripristinato venga riportato in produzione. In questo modo si garantisce che i dati pericolosi non vengano replicati nuovamente nella foresta ripristinata. È particolarmente importante arrestare tutti i titolari del ruolo master operazioni.  

> [!NOTE]
> In alcuni casi è possibile spostare il primo controller di dominio che si prevede di ripristinare per ogni dominio in una rete isolata, consentendo al tempo stesso agli altri controller di dominio di rimanere online per ridurre al minimo i tempi di inattività del sistema. Se, ad esempio, si esegue il ripristino da un aggiornamento dello schema non riuscito, è possibile scegliere di lasciare i controller di dominio in esecuzione nella rete di produzione mentre si eseguono i passaggi di ripristino in isolamento.

Se si eseguono controller di dominio virtualizzati, è possibile spostarli in una rete virtuale isolata dalla rete di produzione in cui si eseguirà il ripristino. Lo trasferimento di controller di dominio virtualizzati in una rete separata offre due vantaggi:  

- I controller di dominio ripristinati non sono in grado di ricorrenza del problema che ha causato il ripristino della foresta perché sono isolati.  
- La clonazione del controller di dominio virtualizzato può essere eseguita sulla rete separata, in modo che un numero critico di controller di dominio possa essere eseguito e testato prima di essere ripristinato nella rete di produzione.

Se i controller di dominio sono in esecuzione su hardware fisico, disconnettere il cavo di rete del primo controller di dominio che si intende ripristinare nel dominio radice della foresta. Se possibile, disconnettere anche i cavi di rete di tutti gli altri controller di dominio. In questo modo si impedisce la replica dei controller di dominio, se vengono accidentalmente avviati durante il processo di ripristino della foresta.  

In una foresta di grandi dimensioni distribuita in più posizioni, può essere difficile garantire che tutti i controller di dominio scrivibili siano arrestati. Per questo motivo, i passaggi di ripristino, ad esempio la reimpostazione dell'account del computer e dell'account krbtgt, oltre alla pulizia dei metadati, sono progettati per garantire che i controller di dominio scrivibili scrivibile non vengano replicati con controller di dominio scrivibile pericolosi (nel caso in cui alcuni siano ancora online nel foresta).  
  
Tuttavia, solo con i controller di dominio scrivibili in modalità offline, è possibile garantire che la replica non venga eseguita. Pertanto, laddove possibile, è consigliabile distribuire la tecnologia di gestione remota che consente di arrestare e isolare fisicamente i controller di dominio scrivibili durante il ripristino della foresta.  
  
RODC può continuare a funzionare mentre i controller di dominio scrivibili sono offline. Nessun altro controller di dominio replica direttamente le modifiche da un controller di dominio di sola lettura, in particolare senza modifiche al contenitore dello schema o della configurazione, in modo che non creino lo stesso rischio dei controller di dominio scrivibile durante il ripristino. Dopo che tutti i controller di dominio scrivibili sono stati ripristinati e online, è necessario ricompilare tutti i RODC.  
  
RODC continuerà a consentire l'accesso alle risorse locali memorizzate nella cache nei rispettivi siti mentre le operazioni di ripristino vengono eseguite in parallelo. Per le risorse locali non memorizzate nella cache del controller di dominio di sola lettura, le richieste di autenticazione verranno inviate a un controller di dominio scrivibile. Queste richieste avranno esito negativo perché i controller di dominio scrivibili sono offline. Alcune operazioni, ad esempio le modifiche delle password, non funzioneranno fino a quando non si ripristinano i controller di dominio scrivibili.  
  
Se si usa un'architettura di rete Hub e spoke, è possibile concentrarsi innanzitutto sul ripristino dei controller di dominio scrivibile nei siti hub. Successivamente, è possibile ricompilare il RODC in siti remoti.  
  
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
