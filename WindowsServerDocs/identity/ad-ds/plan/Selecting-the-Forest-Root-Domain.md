---
ms.assetid: ef4ef4a9-8969-4ad0-bd17-b2bb24f36ef6
title: Selezione del dominio radice della foresta
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c669a5c8103fba52140147957823db978f8b6955
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871352"
---
# <a name="selecting-the-forest-root-domain"></a>Selezione del dominio radice della foresta

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il primo dominio distribuibili in una foresta di Active Directory è denominato dominio radice della foresta. Questo dominio rimane il dominio radice della foresta per il ciclo di vita della distribuzione di Active Directory Domain Services.  
  
Il dominio radice della foresta contiene i gruppi Enterprise Admins e Schema Admins. Questi gruppi di amministratore del servizio vengono usati per gestire le operazioni a livello di foresta, ad esempio l'aggiunta e rimozione di domini e l'implementazione delle modifiche allo schema.  
  
Selezionare il dominio radice foresta, è necessario definire se uno dei domini di Active Directory nella progettazione di dominio possa operare come dominio radice della foresta o se è necessario distribuire un dominio radice foresta dedicata.  
  
Per informazioni sulla distribuzione di un dominio radice della foresta, vedere [distribuzione di un dominio radice foresta di Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
## <a name="choosing-a-regional-or-dedicated-forest-root-domain"></a>Scelta di un dominio radice della foresta a livello di area o dedicato

Se si sta applicando un modello di dominio singolo, il singolo dominio funziona come il dominio radice della foresta. Se si sta applicando un modello a più domini, è possibile distribuire un dominio radice foresta dedicata o selezionare un dominio a livello di area per funzionare come dominio radice della foresta.  
  
### <a name="dedicated-forest-root-domain"></a>Dominio radice della foresta dedicata

Un dominio radice foresta dedicata è un dominio creato appositamente per funzionare come radice della foresta. Non contiene tutti gli account utente diverso dagli account di amministratore del servizio per il dominio radice della foresta. Inoltre, non rappresenta qualsiasi area geografica in una struttura di dominio. Tutti gli altri domini nella foresta sono elementi figlio del dominio radice foresta dedicata.  

Usando una radice della foresta dedicata offre i vantaggi seguenti:  

- OPERATIONAL separazione foresta degli amministratori del servizio da parte degli amministratori del servizio di dominio. In un ambiente di dominio singolo, i membri del gruppo incorporato Administrators e Domain Admins possono usare gli strumenti standard e procedure per diventare membri dei gruppi Enterprise Admins e Schema Admins. In una foresta che usa un dominio radice foresta dedicata, i membri del gruppo incorporato Administrators nei domini regionali e Domain Admins non possono diventare membri dei gruppi di amministratore del servizio a livello di foresta usando strumenti standard e procedure.  
- Protezione da modifiche operative in altri domini. Un dominio radice foresta dedicata non rappresenta una determinata area geografica in una struttura di dominio. Per questo motivo, non è influenzata da riorganizzazioni o altre modifiche che comportano la ridenominazione o ristrutturazione dei domini.  
- Funge da una radice neutra, in modo che venga visualizzato alcun paese o area geografica sia subordinato a un'altra area. Alcune organizzazioni potrebbero preferire evitare l'aspetto di un paese o regione è subordinato a un altro paese o area geografica nello spazio dei nomi. Quando si usa un dominio radice foresta dedicata, tutti i domini a livello di area possono essere peer nella gerarchia di dominio.  

In un ambiente con più aree-domini in cui viene utilizzata una radice della foresta dedicata, la replica del dominio radice della foresta ha un impatto minimo sull'infrastruttura di rete. Questo avviene perché la radice della foresta ospita solo gli account di amministratore del servizio. La maggior parte degli account utente nella foresta e altri dati specifici del dominio vengono archiviati nei domini regionali.  
  
Uno degli svantaggi dell'utilizzo di un dominio radice foresta dedicata è che viene creato un sovraccarico di gestione aggiuntiva per supportare il dominio aggiuntivo.  
  
### <a name="regional-domain-as-a-forest-root-domain"></a>Domini regionali come un dominio radice della foresta

Se si sceglie di non distribuire un dominio radice foresta dedicata, è necessario selezionare un dominio regionale di funzionare come dominio radice della foresta. Questo dominio è il dominio padre di tutti gli altri domini regionali e sarà il primo dominio da distribuire. Il dominio radice della foresta contiene gli account utente e viene gestito nello stesso modo che siano gestiti gli altri domini regionali. La differenza principale è che include anche i gruppi Enterprise Admins e Schema Admins.  
  
Crea il vantaggio della selezione di un dominio regionale alla funzione come dominio radice della foresta è che non crea il sovraccarico di gestione aggiuntivi di gestione di un dominio aggiuntivo. Seleziona un dominio a livello di area appropriato per essere la radice della foresta, ad esempio il dominio che rappresenta la sede centrale o nell'area con le connessioni di rete più veloci. Se è difficile per all'organizzazione di selezionare un dominio a livello di area per il dominio radice foresta, è possibile scegliere di usare invece un modello di foresta dedicata alla radice.  
  
## <a name="assigning-the-forest-root-domain-name"></a>Assegnare il nome di dominio radice della foresta

Il nome di dominio radice della foresta è anche il nome della foresta. Il nome radice della foresta è un nome di sistema DNS (Domain Name) che include un prefisso e suffisso sotto forma di prefix.suffix. Ad esempio, un'organizzazione può avere il nome di foresta radice corp.contoso.com. In questo esempio, corp è il prefisso e contoso.com è il suffisso.  
  
Selezionare il suffisso da un elenco dei nomi esistenti nella rete. Per il prefisso, selezionare un nuovo nome che non è stato usato in precedenza nella rete. Mediante l'aggiunta di un nuovo prefisso per un suffisso esistente, si crea uno spazio dei nomi univoco. La creazione di un nuovo spazio dei nomi per Active Directory Domain Services (AD DS) garantisce che qualsiasi infrastruttura DNS esistente non dovrà essere modificati per adattarli a Active Directory Domain Services.  
  
### <a name="selecting-a-suffix"></a>Selezione di un suffisso

Per selezionare un suffisso per il dominio radice della foresta:  
  
1. Contattare il proprietario DNS per l'organizzazione per un elenco dei suffissi DNS registrati in uso nella rete che ospita Active Directory Domain Services. Si noti che i suffissi usati nella rete interna potrebbero essere diversi da quello i suffissi usati esternamente. Ad esempio, un'organizzazione potrebbe usare contosopharma.com su Internet e contoso.com nella rete azienda interna.  
  
2. Contattare il proprietario DNS per selezionare un suffisso per l'utilizzo con Active Directory Domain Services. Se è presente alcun suffisso appropriato, registrare un nuovo nome con un Internet naming authority.  
  
È consigliabile usare nomi DNS che sono registrati con un'autorità di Internet nello spazio dei nomi Active Directory. Solo i nomi registrati sono necessariamente essere globalmente univoco. Se un'altra organizzazione in un secondo momento registra lo stesso nome di dominio DNS (o se l'organizzazione unito, acquisisce oppure viene acquisita da un'altra società che utilizza lo stesso nome DNS), le due infrastrutture non possono interagire tra loro.  
  
> [!CAUTION]  
> Non usare nomi DNS con etichetta singola. Per altre informazioni, vedere le informazioni sulla configurazione di Windows per domini con nomi DNS con etichetta singola ([https://go.microsoft.com/fwlink/?LinkId=106631](https://go.microsoft.com/fwlink/?LinkId=106631)). Inoltre, non è consigliabile con suffissi dell'annullamento della registrazione, ad esempio Local.  
  
### <a name="selecting-a-prefix"></a>Selezione di un prefisso

Se si sceglie un suffisso registrato che è già in uso nella rete, selezionare un prefisso per il nome di dominio radice della foresta utilizzando le regole di prefisso nella tabella seguente. Aggiungere un prefisso che non è attualmente in uso per creare un nuovo nome subordinato. Ad esempio, se il nome radice DNS è contoso.com, è possibile creare il concorp.contoso.com nome di dominio di Active Directory foresta radice se il concorp.contoso.com dello spazio dei nomi non è già in uso nella rete. Il nuovo ramo dello spazio dei nomi dedicato ai servizi di dominio Active Directory e può essere integrato facilmente con l'implementazione di DNS esistente.  
  
Se si seleziona un dominio regionale di funzionare come un dominio radice della foresta, è necessario selezionare un nuovo prefisso per il dominio. Poiché il nome di dominio radice della foresta influisce su tutti gli altri nomi di dominio nella foresta, un nome in base a livello di area potrebbe non essere appropriato. Se si usa un nuovo suffisso che non è attualmente in uso nella rete, è possibile usarlo come il nome di dominio radice della foresta senza scegliere un altro prefisso.  
  
Nella tabella seguente elenca le regole per la selezione di un prefisso per un nome DNS registrato.  
  
|Regola|Spiegazione|  
|--------|---------------|  
|Selezionare un prefisso che non è destinato a diventare obsoleto.|Evitare nomi, ad esempio una linea di prodotti o sistema operativo che potrebbe cambiare in futuro. È consigliabile usare nomi generici come la società o di dominio Active Directory.|  
|Selezionare un prefisso che include solo caratteri standard Internet.|Alla Z, a-z, 0-9 e (-), ma non è totalmente numerica.|  
|Includere i 15 caratteri o meno nel prefisso.|Se si sceglie una lunghezza del prefisso di 15 caratteri o meno, il nome NetBIOS è lo stesso come prefisso.|  
  
È importante per il proprietario di DNS di Active Directory lavorare con il proprietario DNS per l'organizzazione per ottenere la proprietà del nome che verrà usato per lo spazio dei nomi Active Directory. Per altre informazioni sulla progettazione di un'infrastruttura DNS per il supporto di Active Directory Domain Services, vedere [creazione di un progetto di infrastruttura DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).  
  
## <a name="documenting-the-forest-root-domain-name"></a>Documentare il nome di dominio radice della foresta

Documentare il prefisso DNS e il suffisso scelto per il dominio radice della foresta. A questo punto, identificare a quale dominio sarà la radice della foresta. È possibile aggiungere le informazioni sul nome di dominio radice foresta nel foglio di lavoro "Pianificazione di dominio" creato per documentare il piano per i domini nuovi e aggiornati e i nomi di dominio. Per aprirlo, scaricare da Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip [processo di supporto per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558) e aprire "Pianificazione Domain" (DSSLOGI_5.doc).
