---
title: Risoluzione dei problemi di AD FS - le regole delle attestazioni
description: Questo documento descrive come risolvere i problemi di sintassi delle regole attestazioni con AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 027b2afc9e580253ec820e7e5be14419387ddd44
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837922"
---
# <a name="ad-fs-troubleshooting---claims-rules-syntax"></a>Risoluzione dei problemi di AD FS - di sintassi delle regole attestazioni
Un'attestazione è un'istruzione che un soggetto compie relativa a se stesso o un altro soggetto.  Le attestazioni vengono generate da una relying party e vengono assegnati uno o più valori e quindi inserite nei token di sicurezza emessi dal server AD FS.  Questo articolo riguarda la sintassi di attestazioni e la creazione.  Per informazioni sulle attestazioni vedere emissione [AD FS risoluzione dei problemi - rilascio delle attestazioni](ad-fs-tshoot-claims-issuance.md).

>[!NOTE]  
>È possibile usare [ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) nel [Guida di ad FS](https://adfshelp.microsoft.com) sito per facilitare la risoluzione dei problemi di attestazioni.   

## <a name="how-claim-rules-are-processed"></a>Modalità di elaborazione delle regole attestazioni
Le regole attestazioni vengono elaborate tramite il [pipeline delle attestazioni](../../ad-fs/technical-reference/The-Role-of-the-Claims-Pipeline.md) usando la [motore delle attestazioni](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md). Il motore delle attestazioni è un componente logico del servizio federativo che esamina il set di attestazioni in ingresso fornite da un utente e quindi, in base alla logica di ogni regola, produce un set di attestazioni di output.

## <a name="how-to-create-a-claim-rule"></a>Come creare una regola attestazioni
Le regole attestazioni vengono create separatamente per ogni relazione di trust federativa nel servizio federativo e non vengono condivise tra più trust. È possibile creare una regola da un [modello di regola attestazione](../../ad-fs/technical-reference/determine-the-type-of-claim-rule-template-to-use.md), iniziare da zero, è sufficiente creare la regola usando la [linguaggio delle regole attestazioni](../../ad-fs/technical-reference/when-to-use-a-custom-claim-rule.md) o usare Windows PowerShell per personalizzare una regola.

## <a name="understanding-the-components-of-the-claim-rule-language"></a>Informazioni sui componenti del linguaggio delle regole attestazioni
Il linguaggio di regola attestazione include i componenti seguenti, separati dal "= >" operatore:

- Una condizione - usata per controllare le attestazioni di input e determinare se eseguire l'istruzione di rilascio della regola.  Rappresenta un'espressione logica che deve restituire true per eseguire la parte di corpo della regola.

- Istruzione di rilascio

Esempio:

```c:[type == "Name", value == "domain user"] => issue(type = "Role", value = "employee");``` 

L'attestazione seguente sia presente quanto segue:
- condizione - `c:[type == "Name", value == "domain user"] ` -valuta l'attestazione di input se il nome dell'account windows è un utente di dominio
- rilascio - `issue(type = "Role", value = "employee")` : se la condizione è true, aggiunge una nuova attestazione per l'attestazione di input con il ruolo di dipendente.

Per altre informazioni su attestazioni e la sintassi, vedere [ruolo del linguaggio delle regole attestazioni](../../ad-fs/technical-reference/the-role-of-the-claim-rule-language.md).

## <a name="claims-rule-editor"></a>Editor delle regole attestazioni
Controllo della sintassi avviene mediante l'editor delle regole attestazioni dopo aver completato l'attestazione e fare clic su **OK**.  Pertanto, se si dispone di sintassi non corretta quindi l'editor sarà consentono di sapere.

![attestazioni](media/ad-fs-tshoot-claims/claims1.png)

## <a name="event-logs"></a>Registri eventi
Durante la ricerca cercando di risolvere i problemi di un'attestazione utilizzando i registri l'approccio migliore consiste nel cercare output attestazioni.  È possibile cercare gli eventi 1000 e 1001 nel registro eventi.

![attestazioni](media/ad-fs-tshoot-claims/claims2.png)

## <a name="creating-a-sample-application"></a>Creazione di un'applicazione di esempio
È anche possibile creare un'applicazione di esempio l'eco di attestazioni.  Ad esempio è possibile usare un'applicazione di esempio e creare un relying party che ha la stessa attestazione che si sta tentando di risolvere i problemi e se l'app dispone di eventuali problemi con tale attestazione.

![attestazioni](media/ad-fs-tshoot-claims/claim4.png)

Un'app web di esempio valido è disponibile qui.  Questa app è un'app web semplice che restituisce le attestazioni che riceve dalla relying party.  Per poter utilizzare questa opzione è necessario modificare l'app Web. config da:
- Modifica https://app1.contoso.com/sampapp all'URL la si utilizzerà per l'hosting di sampapp
- Modifica tutte le istanze di sts.contoso.com indicare server federativo AD FS
- Sostituire l'identificazione personale con l'identificazione personale

![attestazioni](media/ad-fs-tshoot-claims/claims3.png)

Quanto segue [post di blog](https://blogs.technet.microsoft.com/tangent_thoughts/2015/02/20/install-and-configure-a-simple-net-4-5-sample-federated-application-samapp/) contiene istruzioni eccellente, approfondite, per questa configurazione.

## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi di AD FS](ad-fs-tshoot-overview.md)