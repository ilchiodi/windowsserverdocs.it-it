---
title: Definizione del sito e posizionamento del controller di dominio in consente di ottimizzare le prestazioni
description: Considerazioni sul posizionamento del controller di dominio e della definizione del sito in Active Directory ottimizzazione delle prestazioni.
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: ba3c9e8792b425fd24d01ab997a5f7c2ac573814
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370253"
---
# <a name="proper-placement-of-domain-controllers-and-site-considerations"></a>Posizionamento appropriato dei controller di dominio e delle considerazioni sul sito

Una definizione di sito corretta è essenziale per le prestazioni. I client che rientrano fuori sito possono compromettere le prestazioni per le query e le autenticazioni. Inoltre, con l'introduzione di IPv6 sui client, la richiesta può provenire dall'indirizzo IPv4 o IPv6 e Active Directory necessario che i siti siano definiti correttamente per IPv6. Quando sono configurati entrambi, il sistema operativo preferisce IPv6 a IPv4.

A partire da Windows Server 2008, il controller di dominio tenta di usare la risoluzione dei nomi per eseguire una ricerca inversa per determinare il sito in cui deve trovarsi il client. Questo può causare l'esaurimento del pool di thread ATQ e causare la mancata risposta del controller di dominio. La soluzione appropriata è la definizione corretta della topologia del sito per IPv6. Come soluzione alternativa, è possibile ottimizzare l'infrastruttura di risoluzione dei nomi per rispondere rapidamente alle richieste del controller di dominio. Per altre informazioni, vedere [risposta ritardata del controller di dominio di Windows server 2008 o Windows server 2008 R2 a richieste LDAP o Kerberos](https://support.microsoft.com/kb/2668820).

Un'area di riflessione aggiuntiva riguarda l'individuazione dei controller di dominio di lettura/scrittura per gli scenari in cui RODC sono in uso.  Per alcune operazioni è necessario l'accesso a un controller di dominio scrivibile o a un controller di dominio scrivibile quando è sufficiente un controller di dominio di sola lettura.  L'ottimizzazione di questi scenari richiederebbe due percorsi:
-   Quando è sufficiente un controller di dominio di sola lettura, contattare i controller di dominio scrivibili.  Questa operazione richiede una modifica del codice dell'applicazione.
-   Potrebbe essere necessario un controller di dominio scrivibile.  Posizionare i controller di dominio di lettura/scrittura in posizioni centrali per ridurre al minimo la latenza.

Per ulteriori informazioni, fare riferimento a:
-   [Compatibilità delle applicazioni con RODC](https://technet.microsoft.com/library/cc772597.aspx)
-   [Active Directory Service Interface (ADSI) e il controller di dominio di sola lettura (RODC), evitando problemi di prestazioni](https://blogs.technet.microsoft.com/fieldcoding/2012/06/24/active-directory-service-interface-adsi-and-the-read-only-domain-controller-rodc-avoiding-performance-issues/)

## <a name="optimize-for-referrals"></a>Ottimizza per i riferimenti

I riferimenti sono il modo in cui le query LDAP vengono reindirizzate quando il controller di dominio non ospita una copia della partizione sottoposta a query. Quando viene restituito un riferimento, esso contiene il nome distinto della partizione, un nome DNS e un numero di porta. Il client utilizza queste informazioni per continuare la query su un server che ospita la partizione. Si tratta di uno scenario DCLocator e vengono mantenute tutte le definizioni del sito e il posizionamento dei controller di dominio, ma le applicazioni che dipendono dai riferimenti vengono spesso trascurate. È consigliabile assicurarsi che la topologia di Active Directory, incluse le definizioni dei siti e il posizionamento del controller di dominio, rispecchi correttamente le esigenze del client. Inoltre, può includere controller di dominio da più domini in un singolo sito, ottimizzazione delle impostazioni DNS o spostamento del sito di un'applicazione.

## <a name="optimization-considerations-for-trusts"></a>Considerazioni sull'ottimizzazione per i trust

In uno scenario con una foresta, i trust vengono elaborati in base alla gerarchia di domini seguente: Dominio-figlio-&gt; dominio figlio-&gt; foresta radice dominio-&gt; dominio figlio-&gt; dominio di grandi età. Ciò significa che i canali sicuri nella radice della foresta e in ogni elemento padre possono essere sottoposto a overload a causa dell'aggregazione delle richieste di autenticazione in transito nei controller di dominio nella gerarchia di trust. Questo può comportare ritardi nelle directory attive di grande dispersione geografica quando l'autenticazione deve anche transitare a collegamenti estremamente latenti per influire sul flusso precedente. Gli overload possono verificarsi in scenari di trust tra foreste e di livello inferiore. Le indicazioni seguenti si applicano a tutti gli scenari:

-   Ottimizzare correttamente il MaxConcurrentAPI per supportare il carico sul canale sicuro. Per ulteriori informazioni, vedere [come eseguire l'ottimizzazione delle prestazioni per l'autenticazione NTLM tramite l'impostazione MaxConcurrentApi](https://support.microsoft.com/kb/2688798/EN-US).

-   Creare trust di collegamento appropriati in base al carico.

-   Verificare che ogni controller di dominio nel dominio sia in grado di eseguire la risoluzione dei nomi e comunicare con i controller di dominio nel dominio trusted.

-   Assicurarsi che vengano prese in considerazione le considerazioni relative alla località per i trust.

-   Abilitare Kerberos laddove possibile e ridurre al minimo l'uso del canale sicuro per ridurre il rischio di incorrere in colli di bottiglia MaxConcurrentAPI.

Gli scenari di trust tra domini sono un'area che è stata sempre un punto problematico per molti clienti. Problemi di risoluzione dei nomi e di connettività, spesso causati da firewall, causano l'esaurimento delle risorse sul controller di dominio trusting e influire su tutti i client. Inoltre, uno scenario spesso trascurato è l'ottimizzazione dell'accesso ai controller di dominio attendibili. Le aree principali per verificare il corretto funzionamento sono le seguenti:

-   Assicurarsi che la risoluzione dei nomi DNS e WINS utilizzata dai controller di dominio trusting possa risolvere un elenco accurato di controller di dominio per il dominio trusted.

    -   I record aggiunti in modo statico hanno tendenza a diventare obsoleti e a reintrodurre problemi di connettività nel tempo. Gli inoltri DNS, il DNS dinamico e l'Unione delle infrastrutture WINS/DNS sono più gestibili a lungo termine.

    -   Verificare la corretta configurazione dei server d'avanzamento, degli inoltri condizionali e delle copie secondarie per le zone di ricerca diretta e inversa per ogni risorsa nell'ambiente a cui un client potrebbe dover accedere. Anche in questo caso è richiesta la manutenzione manuale e la tendenza a diventare obsoleta. Il consolidamento delle infrastrutture è ideale.

-   I controller di dominio nel dominio trusting tenterà di individuare i controller di dominio nel dominio trusted che si trovano nello stesso sito e quindi di eseguire il failback ai localizzatori generici.

    -   Per ulteriori informazioni sul funzionamento di DCLocator, vedere [la pagina relativa alla ricerca di un controller di dominio nel sito più vicino](https://technet.microsoft.com/library/cc978016.aspx).

    -   Convergere i nomi dei siti tra i domini trusted e trusting per riflettere il controller di dominio nello stesso percorso. Verificare che i mapping degli indirizzi IP e delle subnet siano collegati correttamente ai siti in entrambe le foreste. Per altre informazioni, vedere [Domain Locator in un trust tra foreste](http://blogs.technet.com/b/askds/archive/2008/09/24/domain-locator-across-a-forest-trust.aspx).

    -   Verificare che le porte siano aperte, in base alle esigenze di DCLocator, per la posizione del controller di dominio. Se tra i domini esistono firewall, assicurarsi che i firewall siano configurati correttamente per tutti i trust. Se i firewall non sono aperti, il controller di dominio trusting tenterà comunque di accedere al dominio trusted. Se la comunicazione ha esito negativo per qualsiasi motivo, il controller di dominio trusting raggiungerà il timeout della richiesta al controller di dominio attendibile. Tuttavia, questi timeout possono richiedere alcuni secondi per richiesta e possono esaurire le porte di rete sul controller di dominio trusting se il volume delle richieste in ingresso è elevato. Il client può riscontrare il timeout di attesa per il controller di dominio come thread bloccati, che potrebbero tradursi in applicazioni bloccate (se l'applicazione esegue la richiesta nel thread in primo piano). Per ulteriori informazioni, vedere [come configurare un firewall per domini e trust](https://support.microsoft.com/kb/179442).

    -   Usare DnsAvoidRegisterRecords per eliminare i controller di dominio con prestazioni ridotte o con latenza elevata, ad esempio quelli nei siti satellite, dall'annuncio ai localizzatori generici. Per ulteriori informazioni, vedere [come ottimizzare il percorso di un controller di dominio o di un catalogo globale che risiede all'esterno del sito del client](https://support.microsoft.com/kb/306602).

        > [!NOTE]
        > Esiste un limite pratico di circa 50 al numero di controller di dominio che il client può utilizzare. Questi devono essere i controller di dominio con la massima capacità e il sito ottimale.

    
    -  Si consiglia di collocare i controller di dominio da domini trusted e trusting nella stessa posizione fisica.

Per tutti gli scenari di attendibilità, le credenziali vengono indirizzate in base al dominio specificato nelle richieste di autenticazione. Questo vale anche per le query a LookupAccountName e LsaLookupNames (così come altre, queste sono solo le API usate più di frequente). Quando ai parametri di dominio per queste API viene passato un valore NULL, il controller di dominio tenterà di trovare il nome dell'account specificato in ogni dominio trusted disponibile.

-   Disabilitare il controllo di tutti i trust disponibili quando si specifica un dominio NULL. [Come limitare la ricerca di nomi isolati in domini trusted esterni usando la voce del registro di sistema LsaLookupRestrictIsolatedNameLevel](https://support.microsoft.com/kb/818024)

-   Disabilitare il passaggio delle richieste di autenticazione con il dominio NULL specificato in tutti i trust disponibili. [Il processo Lsass. exe potrebbe smettere di rispondere se si dispone di molti trust esterni in un controller di dominio Active Directory](https://support.microsoft.com/kb/923241/EN-US)

## <a name="see-also"></a>Vedere anche
- [Ottimizzazione delle prestazioni di Active Directory Server](index.md)
- [Considerazioni relative ai requisiti hardware](hardware-considerations.md)
- [Considerazioni relative a LDAP](ldap-considerations.md)
- [Risoluzione dei problemi delle prestazioni di Active Directory Domain Services](troubleshoot.md) 
- [Pianificazione della capacità per Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566)