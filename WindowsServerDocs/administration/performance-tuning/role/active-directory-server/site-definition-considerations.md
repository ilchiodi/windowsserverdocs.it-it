---
title: Definizione e dominio controller posizionamento del sito in ottimizzazione delle prestazioni di ADDS
description: Sito definizione e dominio controller considerazioni sulla selezione host nell'ottimizzazione delle prestazioni di Active Directory.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: e1652e45f51500ceeb0026b8892fbe9c54ff38f3
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266633"
---
# <a name="proper-placement-of-domain-controllers-and-site-considerations"></a>Esatto posizionamento dei controller di dominio e considerazioni sul sito

Definizione sito appropriato è fondamentale per le prestazioni. I client che non sito rientrano possono riscontrare una riduzione delle prestazioni per le autenticazioni e le query. Inoltre, con l'introduzione di IPv6 nei client, la richiesta può provenire da IPv4 o l'indirizzo IPv6 e Active Directory deve avere siti definiti correttamente per IPv6. Il sistema operativo preferito IPv6 a IPv4 quando sono configurati entrambi.

A partire da Windows Server 2008, i tentativi di controller di dominio usare la risoluzione dei nomi per eseguire una ricerca inversa per determinare il sito del client devono essere. Questo può causare l'esaurimento del Pool di Thread ATQ e causare il controller di dominio a smettere di rispondere. La soluzione appropriata per questo consiste nel definire correttamente la topologia del sito per IPv6. Come soluzione alternativa, una possibile ottimizzare l'infrastruttura di risoluzione nome per rispondere rapidamente alle richieste di controller di dominio. Per altre informazioni, vedere [Controller di dominio di Windows Server 2008 R2 o Windows Server°2008 ritardato risposta alle richieste LDAP o Kerberos](https://support.microsoft.com/kb/2668820).

Un'area aggiuntiva di considerazione ha individuato i controller di dominio di lettura/scrittura per gli scenari in cui i controller sono in uso.  Alcune operazioni richiedono l'accesso a un Controller di dominio scrivibile o un Controller di dominio scrivibili di destinazione quando invece basterebbe un Controller di dominio di sola lettura.  Ottimizzazione di questi scenari richiederebbe due percorsi:
-   Quando invece basterebbe un Controller di dominio di sola lettura, contattare il controller di dominio scrivibile.  Ciò richiede una modifica del codice dell'applicazione.
-   Un Controller di dominio scrivibile in cui potrebbe essere necessario.  Posizione in lettura / scrittura controller di dominio in posizioni centrali per ridurre al minimo la latenza.

Per altri riferimenti di informazioni:
-   [Compatibilità delle applicazioni con i RODC](https://technet.microsoft.com/library/cc772597.aspx)
-   [Active Directory Service Interface (ADSI) e la lettura solo Controller di dominio (RODC): come evitare problemi di prestazioni](https://blogs.technet.microsoft.com/fieldcoding/2012/06/24/active-directory-service-interface-adsi-and-the-read-only-domain-controller-rodc-avoiding-performance-issues/)

## <a name="optimize-for-referrals"></a>Ottimizzare per i riferimenti

I riferimenti sono come le query LDAP vengono reindirizzate quando il controller di dominio non ospita una copia della partizione sottoposti a query. Quando un riferimento viene restituito, contiene il nome distinto della partizione, un nome DNS e un numero di porta. Il client utilizza queste informazioni per continuare la query in un server che ospita la partizione. Questo è uno scenario di DCLocator e le indicazioni riportate le definizioni del sito e posizionamento del controller di dominio viene mantenuto, ma le applicazioni che dipendono da riferimenti sono spesso trascurate. È consigliabile verificare che la topologia di Active Directory inclusi definizioni di sito e posizionamento del controller di dominio rispecchia correttamente le esigenze del client. Inoltre, possono includere con i controller di dominio da più domini in un singolo sito, ottimizzazione delle impostazioni DNS, o della posizione del sito di un'applicazione.

## <a name="optimization-considerations-for-trusts"></a>Considerazioni sull'ottimizzazione per i trust

In uno scenario di insieme di strutture, trust vengono elaborati in base alla gerarchia di domini seguenti: Dominio nipote -&gt; sottodominio -&gt; dominio - radice della foresta&gt; dominio figlio -&gt; nipote dominio. Ciò significa che canali a livello radice della foresta e ogni elemento padre protetti può diventare sottoposti a overload a causa di aggregazione delle richieste di autenticazione controller di dominio nella gerarchia di trust in transito. Questo inoltre può causare ritardi nella directory di Active Directory di grandi dimensioni dispersione geografica quando l'autenticazione deve anche i collegamenti presenta una latenza elevata per influire sul flusso precedente di transito. Gli overload possono verificarsi negli scenari di relazione di trust tra foreste e di livello inferiore. Le indicazioni seguenti si applicano a tutti gli scenari:

-   Ottimizzare correttamente il MaxConcurrentAPI per supportare il carico tra il canale sicuro. Per altre informazioni, vedi [come ottimizzare le prestazioni per l'autenticazione NTLM tramite l'impostazione MaxConcurrentApi](https://support.microsoft.com/kb/2688798/EN-US).

-   Creare trust di collegamento come appropriato in base al caricamento.

-   Assicurarsi che ogni controller di dominio nel dominio sia in grado di eseguire la risoluzione dei nomi e comunicare con i controller di dominio nel dominio trusted.

-   Assicurarsi di considerazioni sulla località vengono presi in considerazione per i trust.

-   Attivare l'autenticazione Kerberos dove possibile e ridurre al minimo l'utilizzo del canale sicuro per ridurre il rischio di raggiungere i colli di bottiglia MaxConcurrentAPI.

Attendibilità tra domini gli scenari sono un'area in cui è stato in modo coerente il punto debole di molti clienti. Nome connettività e risoluzione dei problemi, spesso a causa di un firewall, causano l'esaurimento delle risorse nel controller di dominio trusting e influire sulla tutti i client. Inoltre, uno scenario spesso trascurato Ottimizza l'accesso ai controller di dominio trusted. Le aree principali per garantire il che corretto funzionamento questa sono i seguenti:

-   Verificare che la risoluzione del nome DNS e WINS che utilizzano i controller di dominio trusting in grado di risolvere un elenco accurato di controller di dominio per il dominio trusted.

    -   I record aggiunti in modo statico hanno la tendenza a diventare non aggiornate e reintrodurre problemi di connettività nel corso del tempo. DNS inoltra, DNS dinamico, e si fondono infrastrutture DNS/WINS sono più facilmente gestibile a lungo termine.

    -   Verificare la configurazione corretta del server d'inoltro condizionale inoltra e le copie secondarie per entrambi le zone di ricerca diretta e inverse per tutte le risorse nell'ambiente a cui un client potrebbe dover accedere. Anche in questo caso, ciò richiede la manutenzione manuale e tende a diventare non aggiornate. Il consolidamento delle infrastrutture è ideale.

-   I controller di dominio nel dominio trusting tenterà di individuare i controller di dominio nel dominio trusted che sono nello stesso sito prima di tutto e quindi eseguire il failback per i localizzatori generici.

    -   Per altre informazioni sul funzionamento di DCLocator, vedi [ricerca di un Controller di dominio nel sito più vicino](https://technet.microsoft.com/library/cc978016.aspx).

    -   Eseguire la convergenza di nomi di sito tra i domini trusted e trusting in modo da riflettere il controller di dominio nello stesso percorso. Verificare i mapping sono collegati in modo corretto ai siti in entrambe le foreste subnet e indirizzo IP. Per altre informazioni, vedi [dominio localizzatore attraverso un Trust tra foreste](http://blogs.technet.com/b/askds/archive/2008/09/24/domain-locator-across-a-forest-trust.aspx).

    -   Verificare che le porte siano aperte, in base alle esigenze di DCLocator, per l'individuazione dei controller di dominio. In presenza di firewall tra i domini, verificare che i firewall siano configurati correttamente per tutti i trust. Se i firewall non sono aperti, il controller di dominio trusting ancora tenterà di accedere al dominio trusted. Se la comunicazione non riesce per qualsiasi motivo, il controller di dominio trusting alla fine si verificherà il timeout della richiesta al controller di dominio trusted. Tuttavia, questi timeout può richiedere alcuni secondi per ogni richiesta e può esaurire le porte di rete nel controller di dominio trusting se il volume di richieste in ingresso è elevato. I client possono presentare le attese per timeout sul controller di dominio come i thread bloccati, che potrebbe implicare applicazioni bloccate (se l'applicazione viene eseguita la richiesta nel thread in primo piano). Per altre informazioni, vedi [come configurare un firewall per domini e trust](https://support.microsoft.com/kb/179442).

    -   Utilizzare DnsAvoidRegisterRecords per eliminare in modo inadeguato prestazioni o ad alta latenza controller di dominio, ad esempio quelli in siti satellite, grazie alla pubblicità per i localizzatori generici. Per altre informazioni, vedi [come ottimizzare la posizione di un controller di dominio o un catalogo globale che si trova di fuori di un sito del client](https://support.microsoft.com/kb/306602).

        > [!Note]   È previsto un limite pratico di circa 50 al numero di controller di dominio che può usare il client. Deve trattarsi di maggiore capacità sito ottimali e massimi i controller di dominio.

         

    -   È consigliabile inserire i controller di dominio da domini trusted e trusting nella stessa posizione fisica.

Per tutti gli scenari di attendibilità, le credenziali vengono indirizzate in base al dominio specificato nelle richieste di autenticazione. Questo vale anche per le query le LsaLookupNames LookupAccountName per (, oltre ad altri utenti, questi vengono semplicemente più comunemente utilizzati) le API. Quando i parametri di dominio per queste API vengono passati un valore NULL, verrà effettuato un tentativo di trovare il nome dell'account specificato in ogni dominio trusted disponibile il controller di dominio.

-   Disabilitare il controllo di tutti i trust disponibili quando si specifica un dominio NULL. [Come limitare la ricerca dei nomi di tipo isolati in domini trusted esterni tramite la voce del Registro di sistema LsaLookupRestrictIsolatedNameLevel](https://support.microsoft.com/kb/818024)

-   Disabilitare il passaggio delle richieste di autenticazione con il dominio NULL specificato in tutti i trust disponibili. [Il processo Lsass.exe potrebbe bloccarsi se si dispone di molti trust esterni in un controller di dominio Active Directory](https://support.microsoft.com/kb/923241/EN-US)

## <a name="see-also"></a>Vedere anche
- [Server Active Directory l'ottimizzazione delle prestazioni](index.md)
- [Considerazioni relative ai requisiti hardware](hardware-considerations.md)
- [Considerazioni relative a LDAP](ldap-considerations.md)
- [Risoluzione dei problemi delle prestazioni di Active Directory Domain Services](troubleshoot.md) 
- [Pianificazione della capacità per Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566)