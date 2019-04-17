---
ms.assetid: d92731f1-e4d8-4223-9b07-ca1f40bb0e1f
title: Namespace indipendente
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac231dbfbaaeafa39199e29a1744d84d5cec23e8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="disjoint-namespace"></a>Namespace indipendente

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Uno spazio dei nomi disgiunto si verifica quando uno o più computer membri del dominio dispone di un suffisso di dominio DNS (Name Service) primario che non corrisponde al nome DNS del dominio di Active Directory di cui i computer sono membri. Ad esempio, un computer membro che usa un suffisso DNS primario del corp.fabrikam.com in un dominio di Active Directory denominato na.corp.fabrikam.com utilizza uno spazio dei nomi indipendente.  
  
Uno spazio dei nomi indipendente è più complesso da amministrare, gestire e risolvere i problemi relativi a uno spazio dei nomi contiguo. In uno spazio dei nomi contiguo, il suffisso DNS primario corrisponde al nome di dominio Active Directory. Applicazioni di rete che vengono scritti in si presuppone che lo spazio dei nomi di Active Directory è identico al suffisso DNS primario per tutti i computer membro di dominio non funzionare correttamente in uno spazio dei nomi indipendente.  
  
## <a name="support-for-disjoint-namespaces"></a>Supporto per gli spazi dei nomi indipendente  
Computer membri del dominio, tra cui i controller di dominio, possono essere utilizzati in uno spazio dei nomi indipendente. Computer membri del dominio possono registrare dell'host (A) IP versione 6 (IPv6) e record di risorse host record di risorse (AAAA) in uno spazio dei nomi DNS indipendente. Quando i computer membro di dominio registrano i relativi record di risorse in questo modo, i controller di dominio continuano a registrare globali e specifiche del sito di servizio (SRV) nella zona DNS che è identica al nome di dominio Active Directory.  
  
Ad esempio, si supponga che un controller di dominio per il dominio di Active Directory denominato na.corp.fabrikam.com che usa un suffisso DNS primario del corp.fabrikam.com registra host (A) e record di risorse host (AAAA) IPv6 nella zona DNS corp.fabrikam.com. Il controller di dominio continua a registrare globali e specifiche del sito di servizio (SRV) nella zona msdcs. na.corp.fabrikam.com e na.corp.fabrikam.com degli zone DNS, che rende possibile il percorso del servizio.  
  
> [!IMPORTANT]  
> Anche se i sistemi operativi Windows può supportare uno spazio dei nomi indipendente, le applicazioni che vengono scritti dal presupposto che il suffisso DNS primario è quello utilizzato per il suffisso del dominio Active Directory potrebbero non funzionare in tale ambiente. Per questo motivo, è necessario verificare tutte le applicazioni e i rispettivi sistemi operativi con attenzione prima di distribuire uno spazio dei nomi indipendente.  
  
Uno spazio dei nomi disgiunto dovrebbe funzionare (ed è supportata) nelle situazioni seguenti:  
  
-   Quando una foresta con più domini di Active Directory utilizza uno spazio dei nomi DNS singolo, che è noto anche come una zona DNS  
  
    Un esempio di questa è una società che usa i domini regionali con nomi come na.corp.fabrikam.com, sa.corp.fabrikam.com e asia.corp.fabrikam.com e Usa un singolo spazio dei nomi DNS, ad esempio corp.fabrikam.com.  
  
-   Quando un singolo dominio di Active Directory è suddivisa in spazi dei nomi DNS separati  
  
    Un esempio di questa è un'azienda con un dominio di Active Directory di corp.contoso.com che utilizza le zone DNS, ad esempio hr.corp.contoso.com production.corp.contoso.com e it.corp.contoso.com.  
  
Uno spazio dei nomi disgiunto non funziona correttamente (e non è supportata) nelle situazioni seguenti:  
  
-   Un suffisso indipendente utilizzato dai membri di dominio corrisponde a un nome di dominio Active Directory in questo o un'altra foresta. In questo modo si interrompe il routing del suffisso di nome Kerberos.  
  
-   Viene utilizzato lo stesso suffisso indipendente in un'altra foresta. Questo impedisce di routing in modo univoco i seguenti suffissi tra foreste.  
  
-   Quando un'autorità di certificazione membro di dominio (CA) le modifiche al server che è completamente dominio nome completo (FQDN) in modo che non utilizzi lo stesso suffisso DNS primario che viene utilizzato dai controller di dominio del dominio a cui il server Autorità di certificazione è un membro. In questo caso, potrebbe essere problemi di convalida dei certificati della CA rilasciata, a seconda di quali nomi DNS vengono utilizzati per i punti di distribuzione CRL. Ma se un server Autorità di certificazione si trova in uno spazio dei nomi disgiunto stabile, essa funziona correttamente ed è supportata.  
  
## <a name="considerations-for-disjoint-namespaces"></a>Considerazioni per gli spazi dei nomi indipendente  
Le considerazioni seguenti possono aiutarti a decidere se è necessario utilizzare uno spazio dei nomi indipendente.  
  
### <a name="application-compatibility"></a>Compatibilità delle applicazioni  
Come accennato in precedenza, uno spazio dei nomi disgiunto può causare problemi per le applicazioni e servizi che vengono scritti in un suffisso DNS primario di computer è identico al nome del nome di dominio di cui è membro. Prima di distribuire uno spazio dei nomi indipendente, è necessario controllare per problemi di compatibilità delle applicazioni. Inoltre, assicurarsi di verificare la compatibilità di tutte le applicazioni che usi quando si esegue l'analisi. Questo include applicazioni di Microsoft e di altri sviluppatori di software.  
  
### <a name="advantages-of-disjoint-namespaces"></a>Vantaggi di spazi dei nomi indipendente  
Utilizza uno spazio dei nomi indipendente, è possibile impostare i seguenti vantaggi:  
  
-   Poiché il suffisso DNS primario di un computer può indicare informazioni diverse, è possibile gestire lo spazio dei nomi DNS separatamente dal nome di dominio Active Directory.  
  
-   È possibile separare lo spazio dei nomi DNS basato su posizione geografica o struttura di business. Ad esempio, è possibile separare lo spazio dei nomi in base a posizione fisica, ad esempio continente, paese/area geografica o edificio o i nomi delle unità aziendali.  
  
### <a name="disadvantages-of-disjoint-namespaces"></a>Svantaggi di spazi dei nomi indipendente  
Utilizza uno spazio dei nomi indipendente, è possibile impostare gli svantaggi seguenti:  
  
-   È necessario creare e gestire le zone DNS separate per ogni dominio di Active Directory nella foresta che dispone di computer membro che utilizzano uno spazio dei nomi indipendente. (Ovvero, richiede una configurazione aggiuntiva e più complessa.)  
  
-   È necessario eseguire passaggi manuali per modificare e gestire l'attributo di Active Directory che consente ai membri di dominio di utilizzare i suffissi DNS primari, specificati.  
  
-   Per ottimizzare la risoluzione dei nomi, è necessario eseguire passaggi manuali per modificare e gestire i criteri di gruppo per configurare i computer membri con alternativi suffissi DNS primari.  
  
    > [!NOTE]  
    > Windows Internet Name Service (WINS) può essere utilizzato per l'offset lo svantaggio di risoluzione dei nomi con etichetta singola. Per ulteriori informazioni su WINS, vedere la documentazione tecnica su WINS ([https://go.microsoft.com/fwlink/?LinkId=102303](https://go.microsoft.com/fwlink/?LinkId=102303)).  
  
-   Quando l'ambiente richiede più suffissi DNS primari, è necessario configurare l'ordine di ricerca suffissi DNS per tutti i domini di Active Directory nella foresta in modo appropriato.  
  
    Per impostare l'ordine di ricerca suffissi DNS, è possibile utilizzare oggetti Criteri di gruppo o parametri di servizio del Server Dynamic Host Configuration Protocol (DHCP). È inoltre possibile modificare il Registro di sistema.  
  
-   È necessario testare attentamente tutte le applicazioni per problemi di compatibilità.  
  
Per ulteriori informazioni sui passaggi da eseguire per risolvere questi svantaggi, vedere come creare un Namespace indipendente ([https://go.microsoft.com/fwlink/?LinkId=106638](https://go.microsoft.com/fwlink/?LinkId=106638)).  
  
### <a name="planning-a-namespace-transition"></a>Pianificazione di una transizione dello spazio dei nomi  
Prima di modificare uno spazio dei nomi, rivedere le considerazioni seguenti, che riguardano le transizioni di spazi dei nomi contiguo di indipendenti spazi dei nomi (o viceversa):  
  
-   Configurare manualmente i nomi dell'entità servizio (SPN) potrebbero non corrispondere i nomi DNS dopo una modifica dello spazio dei nomi. Ciò può causare errori di autenticazione.  
  
    Per ulteriori informazioni, vedere servizio accessi esito negativo a causa di SPN a impostare in modo errato ([https://go.microsoft.com/fwlink/?LinkId=102304](https://go.microsoft.com/fwlink/?LinkId=102304)).  
  
    -   Se si utilizzano computer basati su Windows Server 2003 con la delega vincolata, questi computer potrebbero richiedere una configurazione aggiuntiva per modificare i nomi SPN. Per ulteriori informazioni, vedere l'articolo 936628 della Microsoft Knowledge Base ([https://go.microsoft.com/fwlink/?LinkId=102306](https://go.microsoft.com/fwlink/?LinkId=102306)).  
  
    -   Se si desidera delegare le autorizzazioni per modificare i nomi SPN per amministratori subordinati, vedere la delega dell'autorità per modificare gli SPN ([https://go.microsoft.com/fwlink/?LinkId=106639](https://go.microsoft.com/fwlink/?LinkId=106639)).  
  
-   Se si usa Lightweight Directory Access Protocol (LDAP) tramite SSL Secure Sockets Layer (), noto come LDAPS, con un'autorità di certificazione in una distribuzione che dispone di controller di dominio che sono configurati in uno spazio dei nomi indipendente, è necessario utilizzare il nome di dominio Active Directory appropriato e il suffisso DNS primario quando si configurano i certificati LDAPS.  
  
    Per ulteriori informazioni sui requisiti dei certificati di controller di dominio, vedere l'articolo 321051 della Microsoft Knowledge Base ([https://go.microsoft.com/fwlink/?LinkId=102307](https://go.microsoft.com/fwlink/?LinkId=102307)).  
  
    > [!NOTE]  
    > I controller di dominio che utilizzano certificati per LDAPS potrebbero essere necessario ridistribuire i certificati. In questo caso, i controller di dominio non possono selezionare un certificato appropriato fino a quando non vengono riavviate. Per ulteriori informazioni sull'autenticazione LDAP e un aggiornamento correlato per Windows Server 2003, vedere l'articolo 932834 della Microsoft Knowledge Base ([https://go.microsoft.com/fwlink/?LinkId=102308](https://go.microsoft.com/fwlink/?LinkId=102308)).  
  
### <a name="planning-for-disjoint-namespace-deployments"></a>Pianificazione delle distribuzioni di spazio dei nomi indipendente  
Se si distribuiscono i computer in un ambiente che dispone di uno spazio dei nomi indipendente, adottare le precauzioni seguenti:  
  
1.  Notifica tutti i fornitori di software con cui si lavora che devono testare e supportare uno spazio dei nomi indipendente. Chiedere di verificare che supporti le applicazioni in ambienti che utilizzano spazi dei nomi indipendente.  
  
2.  Testare tutte le versioni di sistemi operativi e applicazioni in ambienti di laboratorio spazio dei nomi indipendente. Quando esegue questa operazione, seguire queste indicazioni:  
  
    1.  Risolvere tutti i problemi software prima di distribuire il software nel proprio ambiente.  
  
    2.  Se possibile, partecipare beta test dei sistemi operativi e applicazioni che si prevedono di distribuire gli spazi dei nomi indipendente.  
  
3.  Assicurarsi che gli amministratori e il personale dell'help desk siano compatibili con lo spazio dei nomi indipendente e l'impatto.  
  
4.  Creare un piano che rende possibile per consentire di eseguire la transizione da uno spazio dei nomi indipendente a uno spazio dei nomi contiguo, se necessario.  
  
5.  Divulgazione l'importanza del supporto di spazio dei nomi disgiunto con sistema operativo e i provider dell'applicazione.  
  


