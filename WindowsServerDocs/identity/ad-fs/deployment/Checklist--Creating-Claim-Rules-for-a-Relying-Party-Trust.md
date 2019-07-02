---
ms.assetid: 44271f44-b50a-4bce-9375-4fcab9618048
title: 'Elenco di controllo: creazione di regole attestazione per una Relying Party Trust'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 47c4913a09fb38c20752bdbc2a9e17ad0af93e01
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192380"
---
# <a name="checklist-creating-claim-rules-for-a-relying-party-trust"></a>Elenco di controllo: Creazione di regole attestazione per un Trust della Relying Party


Questo elenco di controllo include le attività che sono necessari per la pianificazione, progettazione, e distribuisce le regole associate a un trust della relying party in Active Directory Federation Services \(ADFS\).  
  
> [!NOTE]  
> Completare le attività dell'elenco di controllo nell'ordine indicato. Quando un collegamento a un riferimento porta a una procedura, tornare a questo argomento una volta completati i passaggi della procedura, in modo da poter procedere con le operazioni rimanenti nell'elenco di controllo.  
  
![creazione di regole attestazione](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**elenco di controllo: Creazione di un set di regole attestazione per un trust della relying party**  
  
||Attività|Riferimenti|  
|-|--------|-------------|  
|![creazione di regole attestazione](media/icon_checkboxo.gif)|Rivedere i concetti sulle attestazioni, le regole di attestazione, set di regole di attestazione e richiedere modelli di regole e come sono associati a relazioni di trust federative.|![creazione di regole attestazione](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[il ruolo di attestazioni](../../ad-fs/technical-reference/The-Role-of-Claims.md)<br /><br />![creazione di regole attestazione](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[ruolo delle attestazioni regole](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)|  
|![creazione di regole attestazione](media/icon_checkboxo.gif)|Rivedere i concetti sul modo in cui un'attestazione attraversa tutte le fasi della pipeline di rilascio delle attestazioni e come le regole vengono elaborate dal motore di rilascio delle attestazioni.|![creazione di regole attestazione](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[ruolo delle Pipeline delle attestazioni](../../ad-fs/technical-reference/The-Role-of-the-Claims-Pipeline.md)<br /><br />![creazione di regole attestazione](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[il ruolo del motore di attestazioni](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md)|  
|![creazione di regole attestazione](media/icon_checkboxo.gif)|Per pianificare e implementare le attestazioni di output che verranno eseguite su questo trust della relying party, determinare se sono necessari uno o più regole attestazione e che le regole che è necessario utilizzare con questo trust della relying party di attestazione.|![creazione di regole attestazione](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[determinare il tipo di modello di regola attestazione da usare](../../ad-fs/technical-reference/Determine-the-Type-of-Claim-Rule-Template-to-Use.md)|  
|![creazione di regole attestazione](media/icon_checkboxo.gif)|Rivedere i concetti su quando creare un'attestazione regola rispetto a un altro e come è possibile utilizzare il linguaggio di regola attestazione per fornire una logica più complessa rispetto alle regole standard per fornire un risultato desiderato nell'output ideale set di attestazioni.|![creazione di regole attestazione](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[quando utilizzare un Pass-Through o filtro di regola attestazione](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)<br /><br />![creazione di regole attestazione](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[quando usare una regola di attestazione di trasformazione](../../ad-fs/technical-reference/When-to-Use-a-Transform-Claim-Rule.md)<br /><br />![creazione di regole attestazione](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[quando utilizzare un inviare attributi LDAP come attestazioni regola](../../ad-fs/technical-reference/When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md)<br /><br />![creazione di regole attestazione](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[quando usare un gruppo di invio dell'appartenenza a una regola di attestazione](../../ad-fs/technical-reference/When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md)<br /><br />![creazione di regole attestazione](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[quando usare una regola di attestazione di autorizzazione](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)<br /><br />![creazione di regole attestazione](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[quando usare una regola attestazione personalizzata](../../ad-fs/technical-reference/When-to-Use-a-Custom-Claim-Rule.md)<br /><br />![creazione di regole attestazione](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[ruolo del linguaggio di regola attestazione](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md)|  
|![creazione di regole attestazione](media/icon_checkboxo.gif)|Una descrizione di attestazione deve essere creata se non ne esiste già che verrà usata per soddisfare le esigenze dell'organizzazione. ADFS è dotato di un set predefinito di descrizioni di attestazioni che vengono esposte nello snap di gestione di ADFS\-in.|![creazione di regole attestazione](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[aggiungere una descrizione di attestazione](../../ad-fs/operations/Add-a-Claim-Description.md)|  
|![creazione di regole attestazione](media/icon_checkboxo.gif)|A seconda delle esigenze dell'organizzazione, creare uno o più regole attestazione per il set di regole che sono associati a questo trust della relying party in modo che le attestazioni e verrà generate in modo appropriato.|![creazione di regole attestazione](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[creare una regola di Pass-Through o filtrare un'attestazione in ingresso](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)<br /><br />![creazione di regole attestazione](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[creare una regola per inviare attributi LDAP come attestazioni](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)<br /><br />![creazione di regole attestazione](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[creare una regola per inviare l'appartenenza al gruppo come attestazione](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)<br /><br />![creazione di regole attestazione](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[creare una regola per trasformare un'attestazione in ingresso](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)<br /><br />![creazione di regole attestazione](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[creare una regola per l'invio di un metodo di autenticazione attestazioni](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)<br /><br />![creazione di regole attestazione](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[creare una regola per inviare un'istanza di ADFS 1. x attestazione compatibile](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)<br /><br />![creazione di regole attestazione](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[creare una regola per inviare attestazioni mediante una regola personalizzata](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)|  
|![creazione di regole attestazione](media/icon_checkboxo.gif)|A seconda delle esigenze dell'organizzazione, creare uno o più regole attestazione per il set di regole di autorizzazione di rilascio o il set di regole di autorizzazione di delega che è associato a questo trust della relying party in modo che gli utenti saranno consentiti l'accesso alla relying party.|![creazione di regole attestazione](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[creare una regola per consentire tutti gli utenti](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)<br /><br />![creazione di regole attestazione](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[creare una regola per Permit or Deny Users Based in un'attestazione in ingresso](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)|  