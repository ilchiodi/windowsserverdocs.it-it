---
ms.assetid: ef4ef4a9-8969-4ad0-bd17-b2bb24f36ef6
title: Selezionare il dominio radice della foresta
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d87273df1e42e1fa88df09e0b86d4c86ea75ee3f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="selecting-the-forest-root-domain"></a>Selezionare il dominio radice della foresta

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il primo dominio distribuiti in una foresta Active Directory è denominato dominio radice della foresta. Questo dominio rimane dominio radice della foresta per il ciclo di vita della distribuzione di dominio Active Directory.  
  
Dominio radice della foresta contiene i gruppi Enterprise Admins e Schema Admins. Questi gruppi di amministratori del servizio vengono utilizzati per gestire operazioni a livello di foresta, ad esempio l'aggiunta e rimozione di domini e l'implementazione delle modifiche allo schema.  
  
Selezionare il dominio radice della foresta, è necessario definire se uno dei domini di Active Directory nella progettazione di dominio possa operare come dominio radice della foresta o se è necessario distribuire un dominio radice della foresta dedicata.  
  
Per informazioni sulla distribuzione di un dominio radice della foresta, vedere [la distribuzione di un dominio radice della foresta Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
## <a name="choosing-a-regional-or-dedicated-forest-root-domain"></a>Scelta di un dominio radice della foresta dedicata o internazionali  
Se si applica un modello di dominio singolo, il dominio singolo funziona come dominio radice della foresta. Se si applica un modello a più domini, è possibile scegliere di distribuire un dominio radice della foresta dedicata o selezionare un dominio regionale di funzionare come dominio radice della foresta.  
  
### <a name="dedicated-forest-root-domain"></a>Dominio radice della foresta dedicata  
Un dominio radice della foresta dedicata è un dominio che viene creato in modo specifico per l'utilizzo come radice della foresta. Non contiene gli account utente diversi da account di amministratore del servizio per il dominio radice della foresta. Inoltre, non rappresenta qualsiasi area geografica nella struttura del dominio. Tutti gli altri domini nella foresta sono elementi figlio del dominio radice della foresta dedicata.  
  
L'uso di una radice della foresta dedicata offre i vantaggi seguenti:  
  
-   Separazione operativa foresta degli amministratori del servizio da parte degli amministratori del servizio di dominio. In un ambiente di dominio singolo, con i membri del gruppo incorporato Administrators e Domain Admins è possono utilizzare gli strumenti standard e procedure per diventare membri dei gruppi Enterprise Admins e Schema Admins. In una foresta che utilizza un dominio radice della foresta dedicata, i membri del gruppo incorporato Administrators nei domini regionali e Domain Admins non possono diventare membri dei gruppi di amministratore del servizio di livello di foresta utilizzando le procedure e strumenti standard.  
  
-   Protezione da modifiche operative in altri domini. Un dominio radice della foresta dedicata non rappresenta una determinata area geografica nella struttura del dominio. Per questo motivo, non viene influenzata da riorganizzazioni o ad altre modifiche che comportano la ridenominazione o ristrutturazione dei domini.  
  
-   Viene utilizzato come una radice neutra in modo che viene visualizzato alcun paese o area geografica sia subordinato a un'altra area geografica. Alcune organizzazioni preferibile evitare l'aspetto di un paese o area geografica è subordinato a un altro paese o area geografica nello spazio dei nomi. Quando si utilizza un dominio radice della foresta dedicata, tutti i domini regionali possono essere peer nella gerarchia di dominio.  
  
In un ambiente più internazionali-domini in cui viene usata una radice della foresta dedicata, la replica del dominio radice della foresta ha un impatto minimo sull'infrastruttura di rete. Questo avviene perché la radice della foresta ospita solo gli account di amministratore del servizio. La maggior parte degli account utente nella foresta e altri dati specifici del dominio vengono archiviati in domini regionali.  
  
Uno svantaggio all'uso di un dominio radice della foresta dedicata è che crea il sovraccarico di gestione per il supporto di dominio aggiuntivo.  
  
### <a name="regional-domain-as-a-forest-root-domain"></a>Dominio regionale come un dominio radice della foresta  
Se si sceglie di non distribuire un dominio radice della foresta dedicata, è necessario selezionare un dominio regionale di funzionare come dominio radice della foresta. Questo dominio è il dominio padre di tutti gli altri domini regionali e sarà il primo dominio da distribuire. Dominio radice della foresta contiene gli account utente e gestito nello stesso modo che siano gestiti negli altri domini regionali. La differenza principale è che include anche i gruppi Enterprise Admins e Schema Admins.  
  
Crea il vantaggio della selezione di un dominio regionale alla funzione come dominio radice della foresta è che non crea il sovraccarico di gestione che la gestione di un dominio aggiuntivo. Selezionare un dominio regionale appropriato per la radice della foresta, ad esempio il dominio che rappresenta la sede centrale o dell'area geografica che ha le connessioni di rete più veloce. Se è difficile per l'organizzazione selezionare un dominio regionale sia il dominio radice della foresta, è possibile utilizzare un modello di foresta dedicata radice.  
  
## <a name="assigning-the-forest-root-domain-name"></a>Assegnare il nome dominio radice della foresta  
Il nome dominio radice della foresta è anche il nome dell'insieme di strutture. Il nome radice della foresta è un nome di sistema DNS (Domain Name) che è costituito da un prefisso e un suffisso sotto forma di prefix.suffix. Ad esempio, un'organizzazione potrebbe essere il nome radice della foresta corp.contoso.com. In questo esempio, corp è il prefisso e contoso.com è il suffisso.  
  
Selezionare il suffisso da un elenco di nomi esistenti nella rete. Il prefisso, selezionare un nuovo nome che non è stato utilizzato in precedenza nella rete. Collegando un nuovo prefisso di un suffisso esistente, creare uno spazio dei nomi univoco. Creazione di un nuovo spazio dei nomi per servizi di dominio Active Directory (AD DS) garantisce che qualsiasi infrastruttura DNS esistente non è necessario essere modificati per adattarli di dominio Active Directory.  
  
### <a name="selecting-a-suffix"></a>Selezione di un suffisso  
Per selezionare un suffisso di dominio radice della foresta:  
  
1.  Contattare il proprietario DNS per l'organizzazione per un elenco di suffissi DNS registrati in uso nella rete che ospiterà i servizi di dominio Active Directory. Si noti che i suffissi utilizzati nella rete interna potrebbero essere diversi da quello i suffissi usati esternamente. Ad esempio, un'organizzazione potrebbe usare contosopharma.com su Internet e contoso.com nella rete azienda interna.  
  
2.  Consultare il proprietario DNS per selezionare un suffisso per l'utilizzo con Active Directory. Se è presente alcun suffisso adatto, registrare un nuovo nome con una denominazione autorità a Internet.  
  
Si consiglia di utilizzare nomi DNS che sono registrati con un'autorità Internet nello spazio dei nomi di Active Directory. Solo i nomi registrati sono necessariamente essere globalmente univoco. Se un'altra organizzazione in un secondo momento registra lo stesso nome di dominio DNS (o se l'organizzazione unisce con, acquisisce o viene acquisita da un'altra società che usa lo stesso nome DNS), le due infrastrutture non possono interagire tra loro.  
  
> [!CAUTION]  
> Non utilizzare nomi DNS con etichetta singola. Per ulteriori informazioni, vedere informazioni sulla configurazione di Windows per i domini con nomi DNS con etichetta singola ([https://go.microsoft.com/fwlink/?LinkId=106631](https://go.microsoft.com/fwlink/?LinkId=106631)). Inoltre, è consigliabile utilizzare suffissi annullata la registrazione, ad esempio. Local.  
  
### <a name="selecting-a-prefix"></a>Selezione di un prefisso  
Se si sceglie un suffisso registrato che è già in uso nella rete, selezionare un prefisso per il nome dominio radice della foresta utilizzando le regole di prefisso nella tabella seguente. Aggiungere un prefisso che non è attualmente in uso per creare un nuovo nome subordinato. Ad esempio, se il nome radice DNS è contoso.com, è possibile creare il concorp.contoso.com nome di dominio di Active Directory foresta radice di concorp.contoso.com dello spazio dei nomi non è già in uso nella rete. Questo nuovo ramo dello spazio dei nomi dovrà essere dedicato a servizi di dominio Active Directory e può essere integrato facilmente con l'implementazione di DNS esistente.  
  
Se si è selezionato un dominio regionale di funzionare come un dominio radice della foresta, si potrebbe essere necessario selezionare un nuovo prefisso per il dominio. Poiché il nome dominio radice della foresta influisce su tutti gli altri nomi di dominio nella foresta, un nome di base nella regione potrebbe non essere appropriato. Se si utilizza un nuovo suffisso che non è attualmente in uso nella rete, è possibile utilizzare come il nome dominio radice della foresta senza scegliere un prefisso aggiuntivo.  
  
Nella tabella seguente elenca le regole per la selezione di un prefisso per un nome DNS registrato.  
  
|Regola|Spiegazione|  
|--------|---------------|  
|Selezionare un prefisso che non è probabilmente diventare obsoleti.|Evitare nomi, ad esempio una linea di prodotti o un sistema operativo che potrebbero cambiare in futuro. Ti consigliamo di usare nomi generici, ad esempio corp o directory.|  
|Selezionare un prefisso che include solo caratteri standard Internet.|A-Z, a-z, 0-9 e (-), ma non completamente numerico.|  
|Includere i 15 caratteri o meno del prefisso.|Se si sceglie una lunghezza del prefisso di 15 caratteri o meno, il nome NetBIOS è quello utilizzato per il prefisso.|  
  
È importante per il proprietario del DNS di Active Directory per l'utilizzo con il proprietario DNS per l'organizzazione per ottenere la proprietà del nome che verrà utilizzato per lo spazio dei nomi di Active Directory. Per ulteriori informazioni sulla progettazione di un'infrastruttura DNS per il supporto di dominio Active Directory, vedere [creazione di una progettazione dell'infrastruttura DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).  
  
## <a name="documenting-the-forest-root-domain-name"></a>Documentare il nome dominio radice della foresta  
Documentare il prefisso DNS e il suffisso selezionato per il dominio radice della foresta. A questo punto, identificare il dominio sarà la radice della foresta. È possibile aggiungere le informazioni sul nome di dominio radice foresta al foglio di lavoro "Pianificazione del dominio" creato per documentare il piano per i domini nuovi e aggiornati e i nomi di dominio. Per aprirlo, scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip dal processo Aid per Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) e Apri "pianificazione del dominio (DSSLOGI_5.doc).  
  


