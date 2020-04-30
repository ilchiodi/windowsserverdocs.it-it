---
ms.assetid: ef4ef4a9-8969-4ad0-bd17-b2bb24f36ef6
title: Selezione del dominio radice della foresta
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 1abf845ce69b395bf46a0f155db2c683c359207c
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81623879"
---
# <a name="selecting-the-forest-root-domain"></a>Selezione del dominio radice della foresta

> Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il primo dominio distribuito in una foresta Active Directory è denominato dominio radice della foresta. Questo dominio rimane il dominio radice della foresta per il ciclo di vita della distribuzione di servizi di dominio Active Directory.

Il dominio radice della foresta contiene i gruppi Enterprise Admins e Schema Admins. Questi gruppi di amministratori del servizio vengono utilizzati per gestire operazioni a livello di foresta quali l'aggiunta e la rimozione di domini e l'implementazione delle modifiche allo schema.

Per selezionare il dominio radice della foresta, è necessario determinare se uno dei domini Active Directory nella progettazione del dominio può fungere da dominio radice della foresta o se è necessario distribuire un dominio radice della foresta dedicata.

Per informazioni sulla distribuzione di un dominio radice della foresta, vedere [distribuzione di un dominio radice della foresta di Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731174(v=ws.10)).

## <a name="choosing-a-regional-or-dedicated-forest-root-domain"></a>Scelta di un dominio radice della foresta locale o dedicata

Se si applica un modello a dominio singolo, il singolo dominio funziona come dominio radice della foresta. Se si applica un modello di dominio multiplo, è possibile scegliere di distribuire un dominio radice della foresta dedicato oppure selezionare un dominio di area per funzionare come dominio radice della foresta.

### <a name="dedicated-forest-root-domain"></a>Dominio radice della foresta dedicata

Un dominio radice della foresta dedicato è un dominio creato in modo specifico per funzionare come radice della foresta. Non contiene account utente diversi dagli account di amministratore del servizio per il dominio radice della foresta. Non rappresenta inoltre un'area geografica nella struttura del dominio. Tutti gli altri domini nella foresta sono elementi figlio del dominio radice della foresta dedicata.

L'uso di una radice della foresta dedicata offre i vantaggi seguenti:

- Separazione operativa degli amministratori del servizio foresta dagli amministratori del servizio del dominio. In un ambiente con un solo dominio, i membri dei gruppi Domain Admins e Administrators predefinito possono usare gli strumenti e le procedure standard per rendere i membri dei gruppi Enterprise Admins e Schema Admins. In una foresta in cui viene utilizzato un dominio radice della foresta dedicato, i membri dei gruppi amministratori di dominio e amministratori predefiniti nei domini regionali non possono diventare membri dei gruppi di amministratori del servizio a livello di foresta utilizzando gli strumenti e le procedure standard.
- Protezione da modifiche operative in altri domini. Un dominio radice della foresta dedicato non rappresenta una determinata area geografica nella struttura del dominio. Per questo motivo, non è influenzato dalle riorganizzazioni o da altre modifiche che comportano la ridenominazione o la ristrutturazione dei domini.
- Funge da radice neutra, in modo che nessun paese o regione appaia subordinato a un'altra area. Per alcune organizzazioni potrebbe essere preferibile evitare l'aspetto che un paese o un'area è subordinata a un altro paese o regione nello spazio dei nomi. Quando si usa un dominio radice della foresta dedicato, tutti i domini regionali possono essere peer nella gerarchia di domini.

In un ambiente di dominio a più aree in cui viene utilizzata una radice della foresta dedicata, la replica del dominio radice della foresta ha un impatto minimo sull'infrastruttura di rete. Questo perché la radice della foresta ospita solo gli account di amministratore del servizio. La maggior parte degli account utente nella foresta e altri dati specifici del dominio vengono archiviati nei domini regionali.

Uno svantaggio dell'uso di un dominio radice della foresta dedicato è la creazione di un sovraccarico di gestione aggiuntivo per il supporto del dominio aggiuntivo.

### <a name="regional-domain-as-a-forest-root-domain"></a>Dominio locale come dominio radice della foresta

Se si sceglie di non distribuire un dominio radice della foresta dedicata, è necessario selezionare un dominio a livello di area per funzionare come dominio radice della foresta. Questo dominio è il dominio padre di tutti gli altri domini regionali e sarà il primo dominio distribuito. Il dominio radice della foresta contiene gli account utente e viene gestito nello stesso modo in cui vengono gestiti gli altri domini regionali. La differenza principale consiste nel fatto che include anche i gruppi Enterprise Admins e Schema Admins.

Il vantaggio di selezionare un dominio a livello di area per funzionare come dominio radice della foresta è che non crea il sovraccarico di gestione aggiuntivo che gestisce un dominio aggiuntivo. Selezionare un dominio locale appropriato come radice della foresta, ad esempio il dominio che rappresenta la sede centrale o l'area con le connessioni di rete più veloci. Se è difficile per l'organizzazione selezionare un dominio locale come dominio radice della foresta, è possibile scegliere di usare invece un modello radice della foresta dedicato.

## <a name="assigning-the-forest-root-domain-name"></a>Assegnazione del nome di dominio radice della foresta

Il nome di dominio radice della foresta è anche il nome della foresta. Il nome della radice della foresta è un nome Domain Name System (DNS) costituito da un prefisso e un suffisso sotto forma di prefisso. suffisso. Ad esempio, un'organizzazione potrebbe avere il nome radice della foresta corp.contoso.com. In questo esempio, Corp è il prefisso e contoso.com è il suffisso.

Selezionare il suffisso da un elenco di nomi esistenti nella rete. Per il prefisso selezionare un nuovo nome che non sia stato usato in precedenza nella rete. Se si connette un nuovo prefisso a un suffisso esistente, si crea uno spazio dei nomi univoco. La creazione di un nuovo spazio dei nomi per Active Directory Domain Services (AD DS) assicura che qualsiasi infrastruttura DNS esistente non debba essere modificata per supportare servizi di dominio Active Directory.

### <a name="selecting-a-suffix"></a>Selezione di un suffisso

Per selezionare un suffisso per il dominio radice della foresta:

1. Contattare il proprietario DNS dell'organizzazione per un elenco di suffissi DNS registrati in uso nella rete che ospiterà servizi di dominio Active Directory. Si noti che i suffissi usati nella rete interna potrebbero essere diversi dai suffissi usati esternamente. Ad esempio, un'organizzazione può usare contosopharma.com in Internet e contoso.com nella rete aziendale interna.

2. Per selezionare un suffisso da utilizzare con servizi di dominio Active Directory, consultare il proprietario DNS. Se non sono presenti suffissi appropriati, registrare un nuovo nome con un'autorità di denominazione Internet.

Si consiglia di utilizzare nomi DNS registrati con un'autorità Internet nello spazio dei nomi Active Directory. Solo i nomi registrati sono sicuramente univoci a livello globale. Se successivamente un'altra organizzazione registra lo stesso nome di dominio DNS (o se l'organizzazione si unisce, acquisisce o viene acquisita da un'altra società che usa lo stesso nome DNS), le due infrastrutture non possono interagire tra loro.

> [!CAUTION]
> Non usare nomi DNS con etichetta singola. Per ulteriori informazioni, vedere [distribuzione e funzionamento dei domini Active Directory configurati utilizzando nomi DNS con etichetta singola](https://support.microsoft.com/help/300684/). Inoltre, non è consigliabile utilizzare suffissi non registrati, ad esempio. local.

### <a name="selecting-a-prefix"></a>Selezione di un prefisso

Se si sceglie un suffisso registrato già in uso nella rete, selezionare un prefisso per il nome di dominio radice della foresta usando le regole prefisso nella tabella seguente. Aggiungere un prefisso non attualmente in uso per creare un nuovo nome subordinato. Se, ad esempio, il nome radice DNS è contoso.com, è possibile creare il nome di dominio radice della foresta Active Directory concorp.contoso.com se lo spazio dei nomi concorp.contoso.com non è già in uso nella rete. Questo nuovo ramo dello spazio dei nomi sarà dedicato ai servizi di dominio Active Directory e potrà essere integrato facilmente con l'implementazione DNS esistente.

Se è stato selezionato un dominio a livello di area per funzionare come dominio radice della foresta, potrebbe essere necessario selezionare un nuovo prefisso per il dominio. Poiché il nome di dominio radice della foresta influiscono su tutti gli altri nomi di dominio nella foresta, un nome in base all'area potrebbe non essere appropriato. Se si usa un nuovo suffisso che non è attualmente in uso nella rete, è possibile usarlo come nome di dominio radice della foresta senza scegliere un prefisso aggiuntivo.

Nella tabella seguente sono elencate le regole per la selezione di un prefisso per un nome DNS registrato.

| Regola     | Spiegazione |
| -------- | --------------- |
| Selezionare un prefisso che probabilmente non diventi obsoleto. | Evitare nomi come una linea di prodotti o un sistema operativo che potrebbero cambiare in futuro. Si consiglia di usare nomi generici, ad esempio Corp o DS.|
| Selezionare un prefisso che includa solo i caratteri Internet standard. | A-Z, a-z, 0-9 e (-), ma non interamente numerici. |
| Includere almeno 15 caratteri nel prefisso. | Se si sceglie una lunghezza del prefisso di 15 caratteri o meno, il nome NetBIOS sarà uguale al prefisso. |

È importante che il proprietario del DNS Active Directory collabori con il proprietario DNS dell'organizzazione per ottenere la proprietà del nome che verrà usato per lo spazio dei nomi Active Directory. Per ulteriori informazioni sulla progettazione di un'infrastruttura DNS per supportare servizi di dominio Active Directory, vedere [creazione di un progetto di infrastruttura DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).

## <a name="documenting-the-forest-root-domain-name"></a>Documentazione del nome di dominio radice della foresta

Documentare il prefisso e il suffisso DNS selezionati per il dominio radice della foresta. A questo punto, identificare quale dominio sarà la radice della foresta. È possibile aggiungere le informazioni sul nome di dominio radice della foresta al foglio di lavoro di pianificazione del dominio creato per documentare il piano per i domini nuovi e aggiornati e i nomi di dominio. Per aprirlo, scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip da [Job Aids per Windows Server 2003 Deployment Kit](https://microsoft.com/download/details.aspx?id=9608) e aprire "Domain Planning" (DSSLOGI_5. doc).
