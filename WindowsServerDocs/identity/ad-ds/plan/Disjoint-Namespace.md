---
ms.assetid: d92731f1-e4d8-4223-9b07-ca1f40bb0e1f
title: Spazio dei nomi indipendente
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0a2e911e889343b05a515c94e615d3648289f2df
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883572"
---
# <a name="disjoint-namespace"></a>Spazio dei nomi indipendente

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Uno spazio dei nomi non contiguo si verifica quando uno o più computer membri del dominio hanno un suffisso Domain Name Service (DNS) primario che non corrisponde al nome DNS del dominio di Active Directory di cui i computer sono membri. Ad esempio, un computer membro che usa un suffisso DNS primario di corp.fabrikam.com in un dominio di Active Directory denominato na.corp.fabrikam.com utilizza uno spazio dei nomi non contiguo.  
  
Uno spazio dei nomi non contiguo è più complesso da amministrare, gestire e risolvere i problemi di uno spazio dei nomi contiguo. In uno spazio dei nomi contiguo, il suffisso DNS primario corrisponde al nome di dominio Active Directory. Le applicazioni di rete scritti presupporre che lo spazio dei nomi Active Directory è identico al suffisso DNS primario per tutti i computer membri del dominio non funzionano correttamente in uno spazio dei nomi non contiguo.  
  
## <a name="support-for-disjoint-namespaces"></a>Supporto per gli spazi dei nomi non contiguo  
Computer membri del dominio, tra cui i controller di dominio, può essere utilizzata in uno spazio dei nomi non contiguo. Computer membri del dominio possono registrare proprio host (A) IP versione 6 (IPv6) e un record di risorse ospitare i record di risorse (AAAA) in uno spazio dei nomi DNS non contiguo. Quando i computer membri del dominio registrano i record di risorse in questo modo, i controller di dominio continuano a registrare record di risorse globali e specifiche del sito del servizio (SRV) nella zona DNS che è identica al nome di dominio Active Directory.  
  
Ad esempio, si supponga che un controller di dominio per il dominio di Active Directory denominato na.corp.fabrikam.com che usa un suffisso DNS primario di corp.fabrikam.com registra host (A) e record di risorse host (AAAA) IPv6 nella zona DNS corp.fabrikam.com. Il controller di dominio continua a registrare record di risorse globali e specifiche del sito del servizio (SRV) nelle zone DNS _msdcs.na.corp.fabrikam.com e na.corp.fabrikam.com, posizione del servizio che rende possibili.  
  
> [!IMPORTANT]  
> Anche se i sistemi operativi Windows può supportare uno spazio dei nomi non contiguo, le applicazioni scritte per presupporre che il suffisso DNS primario sia quello utilizzato per il suffisso di dominio Active Directory potrebbero non funzionare in tale ambiente. Per questo motivo, è necessario testare tutte le applicazioni e i rispettivi sistemi operativi con attenzione prima di distribuire uno spazio dei nomi non contiguo.  
  
Uno spazio dei nomi non contiguo dovrebbe funzionare, simile alla è supportato nelle situazioni seguenti:  
  
-   Quando una foresta con più domini di Active Directory usa uno spazio dei nomi DNS singola, che è noto anche come una zona DNS  
  
    Un esempio di questo oggetto è una società che utilizza domini regionali con nomi quali na.corp.fabrikam.com sa.corp.fabrikam.com e asia.corp.fabrikam.com e un singolo spazio dei nomi DNS, ad esempio corp.fabrikam.com.  
  
-   Quando un singolo dominio di Active Directory viene suddivisa in diversi spazi dei nomi DNS  
  
    Un esempio di questo oggetto è un'azienda con un dominio di Active Directory di corp.contoso.com che usa le zone DNS, ad esempio hr.corp.contoso.com production.corp.contoso.com e it.corp.contoso.com.  
  
Uno spazio dei nomi non contiguo non funziona correttamente (e non è supportata) nelle situazioni seguenti:  
  
-   Un suffisso non contiguo usato per i membri del dominio corrisponde a un nome di dominio Active Directory nella foresta o in un altro. Questa operazione interrompe il routing del suffisso di nome Kerberos.  
  
-   Viene usato lo stesso suffisso non contiguo in un'altra foresta. Ciò impedisce che identifica in modo univoco il routing questi suffissi tra foreste.  
  
-   Quando un server di autorità (CA) certificazione membro di dominio cambia il nome di dominio completo (FQDN), in modo che non sono più utilizzi lo stesso suffisso DNS primario che viene usato dai controller di dominio del dominio a cui il server di autorità di certificazione è un membro. In questo caso, è possibile i problemi di convalida dei certificati generate, a seconda di quali nomi DNS vengono utilizzati nei punti di distribuzione CRL dal server di autorità di certificazione. Tuttavia, se un server di autorità di certificazione si trova in uno spazio dei nomi non contiguo stabile, essa funziona correttamente ed è supportata.  
  
## <a name="considerations-for-disjoint-namespaces"></a>Considerazioni per gli spazi dei nomi non contiguo  
Le considerazioni seguenti consentono di decidere se usare uno spazio dei nomi non contiguo.  
  
### <a name="application-compatibility"></a>Compatibilità dell'applicazione  
Come accennato in precedenza, uno spazio dei nomi non contiguo può causare problemi per le applicazioni e servizi che vengono scritti in si presuppone che sia identico al nome del nome di dominio di cui è membro di un suffisso DNS primario di computer. Prima di distribuire uno spazio dei nomi non contiguo, è necessario controllare i problemi di compatibilità delle applicazioni. Inoltre, assicurarsi di verificare la compatibilità di tutte le applicazioni che usano quando si esegue l'analisi. Incluse le applicazioni da Microsoft e da altri sviluppatori di software.  
  
### <a name="advantages-of-disjoint-namespaces"></a>Vantaggi degli spazi dei nomi non contiguo  
Utilizza uno spazio dei nomi non contiguo, è possibile impostare i vantaggi seguenti:  
  
-   Poiché il suffisso DNS primario di un computer può indicare informazioni diverse, è possibile gestire lo spazio dei nomi DNS separatamente dal nome di dominio Active Directory.  
  
-   È possibile separare lo spazio dei nomi DNS basato su posizione geografica o la struttura aziendale. Ad esempio, è possibile separare lo spazio dei nomi basato su posizione fisica, ad esempio continente, paese/area geografica o predefiniti o i nomi delle unità aziendali.  
  
### <a name="disadvantages-of-disjoint-namespaces"></a>Svantaggi di spazi dei nomi non contiguo  
Utilizza uno spazio dei nomi non contiguo, è possibile impostare i seguenti svantaggi:  
  
-   È necessario creare e gestire le zone DNS separate per ogni dominio di Active Directory nella foresta che contiene i computer membri che utilizzano uno spazio dei nomi non contiguo. (Vale a dire, richiede una configurazione aggiuntiva e più complessa.)  
  
-   È necessario eseguire passaggi manuali per modificare e gestire l'attributo di Active Directory che consente ai membri del dominio da usare i suffissi DNS primari, specificati.  
  
-   Per ottimizzare la risoluzione dei nomi, è necessario eseguire passaggi manuali per modificare e gestire i criteri di gruppo per configurare i computer membri con suffissi DNS primari alternativi.  
  
    > [!NOTE]  
    > Il Windows Internet Name Service WINS () è stato possibile utilizzabile per lo svantaggio di offset per la risoluzione dei nomi con etichetta singola. Per ulteriori informazioni su WINS, vedere la documentazione tecnica WINS ([https://go.microsoft.com/fwlink/?LinkId=102303](https://go.microsoft.com/fwlink/?LinkId=102303)).  
  
-   Quando l'ambiente richiede più suffissi DNS primari, è necessario configurare in modo appropriato l'ordine di ricerca suffisso DNS per tutti i domini di Active Directory nella foresta.  
  
    Per impostare l'ordine di ricerca suffisso DNS, è possibile usare oggetti Criteri di gruppo o i parametri del servizio Server Dynamic Host Configuration Protocol (DHCP). È inoltre possibile modificare il Registro di sistema.  
  
-   È necessario testare attentamente tutte le applicazioni per i problemi di compatibilità.  
  
Per altre informazioni sui passaggi che è possibile eseguire per risolvere questi svantaggi, vedere creare un Namespace non contiguo ([https://go.microsoft.com/fwlink/?LinkId=106638](https://go.microsoft.com/fwlink/?LinkId=106638)).  
  
### <a name="planning-a-namespace-transition"></a>Pianificazione di una transizione dello spazio dei nomi  
Prima di modificare uno spazio dei nomi, rivedere le considerazioni seguenti, che si applicano alle transizioni da contigui spazi dei nomi non contiguo di spazi dei nomi (o viceversa):  
  
-   Configurare manualmente nomi dell'entità servizio (SPN) potrebbero non corrispondere i nomi DNS dopo una modifica dello spazio dei nomi. Ciò può causare errori di autenticazione.  
  
    Per altre informazioni, vedere servizio gli accessi non riuscire a causa di impostare nomi SPN in modo non corretto ([https://go.microsoft.com/fwlink/?LinkId=102304](https://go.microsoft.com/fwlink/?LinkId=102304)).  
  
    -   Se si utilizzano computer basati su Windows Server 2003 con la delega vincolata, tali computer potrebbero richiedere un'ulteriore configurazione per modificare i nomi SPN. Per altre informazioni, vedere l'articolo 936628 nella Microsoft Knowledge Base ([https://go.microsoft.com/fwlink/?LinkId=102306](https://go.microsoft.com/fwlink/?LinkId=102306)).  
  
    -   Se si desidera delegare le autorizzazioni per modificare i nomi SPN per amministratori subordinati, vedere la delega dell'autorità per modificare i nomi SPN ([https://go.microsoft.com/fwlink/?LinkId=106639](https://go.microsoft.com/fwlink/?LinkId=106639)).  
  
-   Se si usa Lightweight Directory Access Protocol (LDAP) su SSL Secure Sockets Layer () (noto come LDAPS) con un'autorità di certificazione in una distribuzione che è presenti controller di dominio configurati in uno spazio dei nomi non contiguo, è necessario usare il nome di dominio Active Directory appropriato e suffisso DNS primario quando si configurano i certificati LDAPS.  
  
    Per altre informazioni sui requisiti dei certificati di controller di dominio, vedere l'articolo 321051 nella Microsoft Knowledge Base ([https://go.microsoft.com/fwlink/?LinkId=102307](https://go.microsoft.com/fwlink/?LinkId=102307)).  
  
    > [!NOTE]  
    > I controller di dominio che usano certificati per LDAPS potrebbe essere necessario ridistribuire i relativi certificati. Quando si esegue questa operazione, i controller di dominio non possono selezionare un certificato appropriato fino a quando non vengono riavviate. Per altre informazioni sull'autenticazione LDAP e un aggiornamento correlato per Windows Server 2003, vedere l'articolo 932834 nella Microsoft Knowledge Base ([https://go.microsoft.com/fwlink/?LinkId=102308](https://go.microsoft.com/fwlink/?LinkId=102308)).  
  
### <a name="planning-for-disjoint-namespace-deployments"></a>Pianificazione delle distribuzioni di spazio dei nomi indipendente  
Se si distribuiscono i computer in un ambiente con uno spazio dei nomi non contiguo, adottare le precauzioni seguenti:  
  
1.  Inviare una notifica di tutti i fornitori di software con cui si lavori che è necessario verificare e supportare uno spazio dei nomi non contiguo. Chiedere di verificare che supportino le proprie applicazioni in ambienti che utilizzano spazi dei nomi non contiguo.  
  
2.  Testare tutte le versioni di sistemi operativi e applicazioni in ambienti di laboratorio dello spazio dei nomi non contiguo. Quando esegue questa operazione, seguire queste indicazioni:  
  
    1.  Risolvere tutti i problemi relativi al software prima di distribuire il software nel proprio ambiente.  
  
    2.  Se possibile, partecipano nel test beta di sistemi operativi e applicazioni che si prevedono di distribuire negli spazi dei nomi non contiguo.  
  
3.  Assicurarsi che gli amministratori e il personale help desk sono compatibili con lo spazio dei nomi non contiguo e l'impatto.  
  
4.  Creare un piano che rende possibile per l'utente per la transizione da uno spazio dei nomi non contiguo per uno spazio dei nomi contiguo, se necessario.  
  
5.  Diffondere l'importanza del supporto per lo spazio dei nomi non contiguo con sistema operativo e i provider dell'applicazione.  
  


