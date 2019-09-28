---
title: AD FS risoluzione dei problemi-regole attestazioni
description: In questo documento viene descritto come risolvere i problemi della sintassi delle regole attestazioni con AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d0146ba7cfc736f4d37ca66d58d624cc2f7f9a23
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366285"
---
# <a name="ad-fs-troubleshooting---claims-rules-syntax"></a>AD FS risoluzione dei problemi-sintassi delle regole attestazioni
Un'attestazione è un'istruzione che un soggetto crea su se stesso o su un altro soggetto.  Le attestazioni vengono rilasciate da un relying party, a cui vengono assegnati uno o più valori e quindi inseriti in un pacchetto nei token di sicurezza emessi dal server di AD FS.  Questo articolo riguarda la sintassi e la creazione delle attestazioni.  Per informazioni sul rilascio di attestazioni [, vedere ad FS risoluzione dei problemi-rilascio di attestazioni](ad-fs-tshoot-claims-issuance.md).

>[!NOTE]  
>È possibile utilizzare [ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) nel sito della [Guida di ADFS](https://adfshelp.microsoft.com) per facilitare la risoluzione dei problemi relativi alle attestazioni.   

## <a name="how-claim-rules-are-processed"></a>Modalità di elaborazione delle regole attestazioni
Le regole attestazioni vengono elaborate tramite la [pipeline](../../ad-fs/technical-reference/The-Role-of-the-Claims-Pipeline.md) delle attestazioni usando il [motore delle attestazioni](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md). Il motore delle attestazioni è un componente logico del servizio federativo che esamina il set di attestazioni in ingresso fornite da un utente e quindi, in base alla logica di ogni regola, produce un set di attestazioni di output.

## <a name="how-to-create-a-claim-rule"></a>Come creare una regola attestazioni
Le regole attestazioni vengono create separatamente per ogni relazione di trust federativa nel servizio federativo e non vengono condivise tra più trust. È possibile creare una regola da un [modello di regola attestazioni](../../ad-fs/technical-reference/determine-the-type-of-claim-rule-template-to-use.md), iniziare da zero creando la regola usando il [linguaggio delle regole attestazioni](../../ad-fs/technical-reference/when-to-use-a-custom-claim-rule.md) oppure usare Windows PowerShell per personalizzare una regola.

## <a name="understanding-the-components-of-the-claim-rule-language"></a>Informazioni sui componenti del linguaggio delle regole attestazioni
Il linguaggio delle regole attestazioni è costituito dai componenti seguenti, separati dall'operatore "= >":

- Condizione: utilizzata per controllare le attestazioni di input e determinare se deve essere eseguita l'istruzione di rilascio della regola.  Rappresenta un'espressione logica che deve essere valutata true per eseguire la parte corpo della regola.

- Istruzione di rilascio

Esempio:

```c:[type == "Name", value == "domain user"] => issue(type = "Role", value = "employee");``` 

Nell'attestazione seguente sono presenti gli elementi seguenti:
- Condition-`c:[type == "Name", value == "domain user"] `-valuta l'attestazione di input che indica se il nome dell'account di Windows è un utente di dominio
- rilascio-`issue(type = "Role", value = "employee")`-se la condizione è true, aggiunge una nuova attestazione all'attestazione di input con il ruolo Employee.

Per ulteriori informazioni sulle attestazioni e sulla sintassi, vedere [il ruolo del linguaggio delle regole attestazioni](../../ad-fs/technical-reference/the-role-of-the-claim-rule-language.md).

## <a name="claims-rule-editor"></a>Editor delle regole attestazioni
Il controllo della sintassi viene eseguito dall'editor delle regole attestazioni dopo aver completato l'attestazione, quindi fare clic su **OK**.  Quindi, se la sintassi non è corretta, l'editor lo noterà.

![attestazioni](media/ad-fs-tshoot-claims/claims1.png)

## <a name="event-logs"></a>Registri eventi
Quando si cerca di risolvere i problemi relativi a un'attestazione usando i log, l'approccio migliore consiste nel cercare l'output delle attestazioni.  È possibile cercare gli eventi 1000 e 1001 nel registro eventi.

![attestazioni](media/ad-fs-tshoot-claims/claims2.png)

## <a name="creating-a-sample-application"></a>Creazione di un'applicazione di esempio
È anche possibile creare un'applicazione di esempio per l'eco delle attestazioni.  Ad esempio, è possibile usare un'applicazione di esempio e creare un relying party con la stessa attestazione che si sta tentando di risolvere e verificare se l'applicazione presenta problemi con tale attestazione.

![attestazioni](media/ad-fs-tshoot-claims/claim4.png)

Un'app Web di esempio è disponibile qui.  Questa app è una semplice app Web che restituisce le attestazioni ricevute dal relying party.  Per usarlo, è necessario modificare l'app Web. config:
- modifica di https://app1.contoso.com/sampapp nell'URL che verrà utilizzato per ospitare il sampapp
- modifica di tutte le istanze di sts.contoso.com in modo da puntare a AD FS server federativo
- Sostituzione dell'identificazione personale con l'identificazione personale

![attestazioni](media/ad-fs-tshoot-claims/claims3.png)

Nell' [articolo del Blog](https://blogs.technet.microsoft.com/tangent_thoughts/2015/02/20/install-and-configure-a-simple-net-4-5-sample-federated-application-samapp/) seguente sono disponibili istruzioni ottimali per la configurazione.

## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi relativi ad AD FS](ad-fs-tshoot-overview.md)