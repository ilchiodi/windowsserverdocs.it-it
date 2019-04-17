---
ms.assetid: b8df1828-5ead-4c90-b0fe-95c675116b7c
title: "Creazione di una progettazione di un'unità organizzativa"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3a74770a4558c79d1f9250f37181562e1d235d34
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="creating-an-organizational-unit-design"></a>Creazione di una progettazione di un'unità organizzativa

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Proprietari di foresta sono responsabili per la creazione di progettazioni di un'unità organizzativa (OU) per i relativi domini. Creazione di una struttura OU comporta la progettazione della struttura di unità Organizzativa, assegnare il ruolo di proprietario della OU, creare account e la risorsa unità organizzative.  
  
Inizialmente, progetta la struttura di unità Organizzativa per abilitare la delega dell'amministrazione. Una volta completata la progettazione di unità Organizzativa, è possibile creare strutture OU aggiuntive per l'applicazione di criteri di gruppo a utenti e computer e limitare la visibilità di oggetti. Per ulteriori informazioni, vedere Progettazione di un'infrastruttura di criteri di gruppo ([https://go.microsoft.com/fwlink/?LinkId=106655](https://go.microsoft.com/fwlink/?LinkId=106655)).  
  
## <a name="ou-owner-role"></a>Ruolo di proprietario della OU  
Il proprietario della foresta designa un proprietario per ogni unità Organizzativa che si progetta per il dominio. Proprietari di unità Organizzative sono responsabili dei dati che controllano un sottoalbero di oggetti in servizi di dominio Active Directory (AD DS). Proprietari di unità Organizzative è possono controllare la modalità amministrazione è delegata e come viene applicato il criterio a oggetti di OU. Possono inoltre creare nuove sottostrutture e delegare l'amministrazione di unità organizzative all'interno di tali sottostrutture.  
  
Poiché i proprietari di unità Organizzativa non proprietari o controllare il funzionamento del servizio directory, è possibile separare proprietà e la gestione del servizio directory di proprietà e la gestione di oggetti, riducendo il numero di amministratori del servizio che dispongono di livelli di accesso elevati.  
  
Le unità organizzative forniscono autonomia amministrativa e consente di controllare la visibilità di oggetti nella directory. Le unità organizzative offrono l'isolamento da altri amministratori di dati, ma non forniscono isolamento da parte degli amministratori del servizio. Anche se i proprietari di unità Organizzativa hanno un sottoalbero di oggetti di controllo, il proprietario della foresta mantiene il controllo completo su tutti i sottoalberi. In questo modo il proprietario della foresta per correggere gli errori, ad esempio un errore in un elenco di controllo di accesso (ACL) e per recuperare i sottoalberi delegati quando gli amministratori di dati vengono terminati.  
  
## <a name="account-ous-and-resource-ous"></a>OU di account e OU di risorse  
OU di account contengono oggetti utente e gruppo di computer. Proprietari di foreste necessario creare una struttura di unità Organizzativa per gestire questi oggetti e quindi delegare il controllo della struttura per il proprietario dell'unità Organizzativa. Se si distribuisce un nuovo dominio di Active Directory, creare un'unità Organizzativa di account per il dominio in modo che è possibile delegare il controllo degli account nel dominio.  
  
Unità organizzative di risorse contengono risorse e gli account che sono responsabili per la gestione di tali risorse. È anche responsabile per la creazione di una struttura di unità Organizzativa per gestire queste risorse e per la delega del controllo della struttura per il proprietario dell'unità Organizzativa proprietario della foresta. Creare unità organizzative di risorse in base alle esigenze in base ai requisiti di ciascun gruppo all'interno dell'organizzazione per autonomia la gestione di dispositivi e dati.  
  
## <a name="documenting-the-ou-design-for-each-domain"></a>Documentare la struttura di unità Organizzativa per ogni dominio  
Costituire un team per progettare la struttura di unità Organizzativa che usi per delegare il controllo sulle risorse all'interno della foresta. Il proprietario della foresta potrebbe essere coinvolto nel processo di progettazione e necessario approvare la progettazione di unità Organizzativa. Si può inoltre comportare almeno un amministratore del servizio per garantire che la progettazione sia valida. Gli altri partecipanti del team di progettazione potrebbero includere gli amministratori di dati che funzioneranno in unità organizzative e l'unità Organizzativa proprietari che saranno responsabili per la loro gestione.  
  
È importante documentare la progettazione di unità Organizzativa. Elencare i nomi delle unità organizzative che si intende creare. E, per ogni unità Organizzativa, documentare il tipo di unità Organizzativa, il proprietario dell'unità Organizzativa, l'elemento padre unità Organizzativa (se applicabile) e l'origine di tale unità Organizzativa.  
  
Per un foglio di lavoro agevolare la documentazione della progettazione di unità Organizzativa, scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip).  
  
## <a name="in-this-section"></a>In questa sezione  
  
-   [Revisione dei concetti di progettazione OU](../../ad-ds/plan/Reviewing-OU-Design-Concepts.md)  
  
-   [La delega dell'amministrazione tramite oggetti unità Organizzative](../../ad-ds/plan/Delegating-Administration-by-Using-OU-Objects.md)  
  


