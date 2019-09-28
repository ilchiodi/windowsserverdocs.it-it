---
ms.assetid: d92731f1-e4d8-4223-9b07-ca1f40bb0e1f
title: Spazio dei nomi indipendente
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5abe67c89ce4c2f4b5056f6197242b5db8db340e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408857"
---
# <a name="disjoint-namespace"></a>Spazio dei nomi indipendente

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Uno spazio dei nomi non contiguo si verifica quando uno o più computer membri del dominio dispongono di un suffisso DNS (Primary Domain Name Service) che non corrisponde al nome DNS del dominio Active Directory di cui i computer sono membri. Ad esempio, un computer membro che usa un suffisso DNS primario di corp.fabrikam.com in un dominio Active Directory denominato na.corp.fabrikam.com usa uno spazio dei nomi non contiguo.  
  
Uno spazio dei nomi non contiguo è più complesso da amministrare, gestire e risolvere i problemi rispetto a uno spazio dei nomi contiguo. In uno spazio dei nomi contiguo il suffisso DNS primario corrisponde al nome di dominio Active Directory. Le applicazioni di rete scritte per presumere che lo spazio dei nomi Active Directory sia identico al suffisso DNS primario per tutti i computer membri del dominio non funzionano correttamente in uno spazio dei nomi non contiguo.  
  
## <a name="support-for-disjoint-namespaces"></a>Supporto per spazi dei nomi non contigui  
I computer membri del dominio, inclusi i controller di dominio, possono funzionare in uno spazio dei nomi non contiguo. I computer membri del dominio possono registrare il record di risorse host (A) e l'host IP versione 6 (IPv6) host (AAAA) in uno spazio dei nomi DNS non contiguo. Quando i computer membri del dominio registrano i propri record di risorse in questo modo, i controller di dominio continuano a registrare i record di risorse di servizio (SRV) globali e specifici del sito nella zona DNS identica al nome di dominio Active Directory.  
  
Si supponga, ad esempio, che un controller di dominio per il dominio Active Directory denominato na.corp.fabrikam.com che usa un suffisso DNS primario di corp.fabrikam.com registri i record di risorse host (A) e host IPv6 (AAAA) nella zona DNS corp.fabrikam.com. Il controller di dominio continua a registrare i record di risorse di servizio (SRV) globali e specifici del sito nelle zone DNS _msdcs. na. Corp. fabrikam. com e na.corp.fabrikam.com, che rendono possibile la posizione del servizio.  
  
> [!IMPORTANT]  
> Sebbene i sistemi operativi Windows possano supportare uno spazio dei nomi non contiguo, le applicazioni scritte per presupporre che il suffisso DNS primario corrisponda al suffisso del dominio Active Directory potrebbe non funzionare in un ambiente di questo tipo. Per questo motivo, è consigliabile testare attentamente tutte le applicazioni e i rispettivi sistemi operativi prima di distribuire uno spazio dei nomi non contiguo.  
  
Uno spazio dei nomi non contiguo dovrebbe funzionare (ed è supportato) nelle situazioni seguenti:  
  
-   Quando una foresta con più domini Active Directory usa un singolo spazio dei nomi DNS, noto anche come zona DNS  
  
    Un esempio è costituito da una società che usa domini regionali con nomi come na.corp.fabrikam.com, sa.corp.fabrikam.com e asia.corp.fabrikam.com e usa un singolo spazio dei nomi DNS, ad esempio corp.fabrikam.com.  
  
-   Quando un singolo dominio di Active Directory viene suddiviso in spazi dei nomi DNS distinti  
  
    Un esempio è costituito da una società con un dominio Active Directory di corp.contoso.com che usa zone DNS come hr.corp.contoso.com, production.corp.contoso.com e it.corp.contoso.com.  
  
Uno spazio dei nomi non contiguo non funziona correttamente (e non è supportato) nelle situazioni seguenti:  
  
-   Un suffisso non contiguo utilizzato dai membri del dominio corrisponde a un Active Directory nome di dominio in questa o in un'altra foresta. Questa operazione interrompe il routing del suffisso del nome Kerberos.  
  
-   Lo stesso suffisso non contiguo viene usato in un'altra foresta. In questo modo si impedisce il routing di questi suffissi in modo univoco tra le foreste.  
  
-   Quando un server dell'autorità di certificazione (CA) membro del dominio modifica il nome di dominio completo (FQDN) in modo che non usi più lo stesso suffisso DNS primario usato dai controller di dominio del dominio a cui il server CA è membro. In questo caso, è possibile che si verifichino problemi di convalida dei certificati emessi dal server CA, a seconda dei nomi DNS utilizzati nei punti di distribuzione CRL. Tuttavia, se si inserisce un server CA in uno spazio dei nomi non contiguo stabile, funziona correttamente ed è supportato.  
  
## <a name="considerations-for-disjoint-namespaces"></a>Considerazioni per gli spazi dei nomi non contigui  
Le considerazioni seguenti possono essere utili per decidere se usare uno spazio dei nomi non contiguo.  
  
### <a name="application-compatibility"></a>Compatibilità dell'applicazione  
Come indicato in precedenza, uno spazio dei nomi non contiguo può causare problemi per tutte le applicazioni e i servizi scritti per presupporre che un suffisso DNS primario del computer sia identico al nome del nome di dominio di cui è membro. Prima di distribuire uno spazio dei nomi non contiguo, è necessario controllare le applicazioni per i problemi di compatibilità. Assicurarsi inoltre di controllare la compatibilità di tutte le applicazioni utilizzate quando si esegue l'analisi. Sono incluse le applicazioni di Microsoft e di altri sviluppatori di software.  
  
### <a name="advantages-of-disjoint-namespaces"></a>Vantaggi degli spazi dei nomi non contigui  
L'uso di uno spazio dei nomi non contiguo può presentare i vantaggi seguenti:  
  
-   Poiché il suffisso DNS primario di un computer può indicare informazioni diverse, è possibile gestire lo spazio dei nomi DNS separatamente dal nome del dominio Active Directory.  
  
-   È possibile separare lo spazio dei nomi DNS in base alla struttura aziendale o alla posizione geografica. Ad esempio, è possibile separare lo spazio dei nomi in base ai nomi delle unità aziendali o alla posizione fisica, ad esempio continente, paese/area geografica o compilazione.  
  
### <a name="disadvantages-of-disjoint-namespaces"></a>Svantaggi degli spazi dei nomi non contigui  
L'uso di uno spazio dei nomi non contiguo può presentare gli svantaggi seguenti:  
  
-   È necessario creare e gestire zone DNS separate per ogni dominio Active Directory nella foresta con computer membri che utilizzano uno spazio dei nomi non contiguo. Ovvero è necessaria una configurazione aggiuntiva e più complessa.  
  
-   È necessario eseguire i passaggi manuali per modificare e gestire l'attributo Active Directory che consente ai membri del dominio di utilizzare suffissi DNS primari specificati.  
  
-   Per ottimizzare la risoluzione dei nomi, è necessario eseguire i passaggi manuali per modificare e gestire Criteri di gruppo per configurare i computer membri con suffissi DNS primari alternativi.  
  
    > [!NOTE]  
    > È possibile utilizzare Windows Internet Name Service (WINS) per compensare questo svantaggio risolvendo i nomi con etichetta singola. Per ulteriori informazioni su WINS, vedere il riferimento tecnico WINS ([https://go.microsoft.com/fwlink/?LinkId=102303](https://go.microsoft.com/fwlink/?LinkId=102303)).  
  
-   Quando l'ambiente richiede più suffissi DNS primari, è necessario configurare in modo appropriato l'ordine di ricerca dei suffissi DNS per tutti i domini Active Directory nella foresta.  
  
    Per impostare l'ordine di ricerca dei suffissi DNS, è possibile utilizzare Criteri di gruppo oggetti o i parametri del servizio server Dynamic Host Configuration Protocol (DHCP). È anche possibile modificare il registro di sistema.  
  
-   È necessario testare con attenzione tutte le applicazioni per problemi di compatibilità.  
  
Per ulteriori informazioni sui passaggi da eseguire per risolvere questi svantaggi, vedere creare uno spazio dei nomi non contiguo ([https://go.microsoft.com/fwlink/?LinkId=106638](https://go.microsoft.com/fwlink/?LinkId=106638)).  
  
### <a name="planning-a-namespace-transition"></a>Pianificazione di una transizione dello spazio dei nomi  
Prima di modificare uno spazio dei nomi, esaminare le considerazioni seguenti, che si applicano alle transizioni da spazi dei nomi contigui a spazi dei nomi non contigui (o viceversa):  
  
-   I nomi dell'entità servizio (SPN) configurati manualmente potrebbero non corrispondere più ai nomi DNS dopo una modifica dello spazio dei nomi. Ciò può causare errori di autenticazione.  
  
    Per ulteriori informazioni, vedere Errore degli accessi al servizio a causa dell'impostazione non corretta dei nomi SPN ([https://go.microsoft.com/fwlink/?LinkId=102304](https://go.microsoft.com/fwlink/?LinkId=102304)).  
  
    -   Se si utilizzano computer basati su Windows Server 2003 con delega vincolata, tali computer potrebbero richiedere una configurazione aggiuntiva per modificare i nomi SPN. Per ulteriori informazioni, vedere l'articolo 936628 della Microsoft Knowledge base ([https://go.microsoft.com/fwlink/?LinkId=102306](https://go.microsoft.com/fwlink/?LinkId=102306)).  
  
    -   Se si desidera delegare le autorizzazioni per modificare i nomi SPN agli amministratori subordinati, vedere Delega dell'autorità per modificare i nomi SPN ([https://go.microsoft.com/fwlink/?LinkId=106639](https://go.microsoft.com/fwlink/?LinkId=106639)).  
  
-   Se si usa LDAP (Lightweight Directory Access Protocol) su Secure Sockets Layer (SSL) (noto come LDAPs) con una CA in una distribuzione che dispone di controller di dominio configurati in uno spazio dei nomi non contiguo, è necessario usare il nome di dominio Active Directory appropriato e suffisso DNS primario quando si configurano i certificati LDAPs.  
  
    Per ulteriori informazioni sui requisiti dei certificati del controller di dominio, vedere l'articolo 321051 della Microsoft Knowledge base ([https://go.microsoft.com/fwlink/?LinkId=102307](https://go.microsoft.com/fwlink/?LinkId=102307)).  
  
    > [!NOTE]  
    > I controller di dominio che usano certificati per LDAPs possono richiedere la ridistribuzione dei certificati. Quando si esegue questa operazione, è possibile che i controller di dominio non selezionino un certificato appropriato finché non vengono riavviati. Per ulteriori informazioni sull'autenticazione LDAPs e su un aggiornamento correlato per Windows Server 2003, vedere l'articolo 932834 della Microsoft Knowledge base ([https://go.microsoft.com/fwlink/?LinkId=102308](https://go.microsoft.com/fwlink/?LinkId=102308)).  
  
### <a name="planning-for-disjoint-namespace-deployments"></a>Pianificazione delle distribuzioni dello spazio dei nomi non contigue  
Se si distribuiscono computer in un ambiente con uno spazio dei nomi non contiguo, adottare le precauzioni seguenti:  
  
1.  Inviare una notifica a tutti i fornitori di software con cui si esegue l'attività di test e supporto di uno spazio dei nomi non contiguo. Chiedere loro di verificare che supportino le applicazioni in ambienti che usano spazi dei nomi non contigui.  
  
2.  Testare tutte le versioni di sistemi operativi e applicazioni negli ambienti Lab dello spazio dei nomi non contigui. Quando si esegue questa operazione, attenersi alle indicazioni seguenti:  
  
    1.  Risolvere tutti i problemi software prima di distribuire il software nell'ambiente in uso.  
  
    2.  Quando possibile, partecipare ai test beta dei sistemi operativi e delle applicazioni che si prevede di distribuire in spazi dei nomi non contigui.  
  
3.  Assicurarsi che gli amministratori e il personale helpdesk siano consapevoli dello spazio dei nomi non contiguo e del relativo effetto.  
  
4.  Creare un piano che consenta di eseguire la transizione da uno spazio dei nomi non contiguo a uno spazio dei nomi contiguo, se necessario.  
  
5.  Evangelizzare l'importanza del supporto dello spazio dei nomi non contiguo con i provider del sistema operativo e dell'applicazione.  
  


