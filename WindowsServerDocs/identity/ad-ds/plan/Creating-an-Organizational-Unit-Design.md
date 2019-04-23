---
ms.assetid: b8df1828-5ead-4c90-b0fe-95c675116b7c
title: Creazione di un progetto di unità organizzativa
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9ecd0228b50a4e597c3fa1dbd3fdaf1f84ca1d3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830402"
---
# <a name="creating-an-organizational-unit-design"></a>Creazione di un progetto di unità organizzativa

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Foresta proprietari sono responsabili per la creazione di progettazioni di unità organizzativa (OU) per i loro domini. Creazione di una struttura OU richiede la progettazione di struttura dell'unità Organizzativa, assegnare il ruolo di proprietario dell'unità Organizzativa e creare account e la risorsa unità organizzative.  
  
Inizialmente, progettare la struttura dell'unità Organizzativa per abilitare la delega dell'amministrazione. Una volta completata la progettazione dell'unità Organizzativa, è possibile creare altre strutture di unità Organizzativa per l'applicazione dei criteri di gruppo per gli utenti e computer e limitare la visibilità degli oggetti. Per altre informazioni, vedere Progettazione di un'infrastruttura di criteri di gruppo ([https://go.microsoft.com/fwlink/?LinkId=106655](https://go.microsoft.com/fwlink/?LinkId=106655)).  
  
## <a name="ou-owner-role"></a>Ruolo di proprietario dell'unità Organizzativa  
Il proprietario della foresta designa un proprietario per ogni unità Organizzativa progettate per il dominio. Unità Organizzativa proprietari sono responsabili dei dati che controllano un sottoalbero di oggetti in Active Directory Domain Services (AD DS). Proprietari di unità Organizzative è possono controllare la modalità amministrazione è delegata e la modalità di applicazione dei criteri per gli oggetti all'interno della propria unità amministrativa. Possono anche creare nuovi sottoalberi e delegare l'amministrazione delle unità organizzative all'interno di tali sottoalberi.  
  
Poiché i proprietari di unità Organizzativa non possiede o controlla l'operazione del servizio directory, è possibile separare la proprietà e l'amministrazione del servizio directory dalla proprietà e la gestione di oggetti, riducendo il numero di amministratori del servizio che hanno livelli elevati di accesso.  
  
Le unità organizzative forniscono i mezzi per controllare la visibilità degli oggetti nella directory e offra autonomia amministrativa. Le unità organizzative forniscono l'isolamento da altri amministratori dei dati, ma non forniscono isolamento da amministratori del servizio. Anche se i proprietari di unità Organizzativa possono controllare un sottoalbero di oggetti, il proprietario della foresta mantiene il controllo completo su tutti i sottoalberi. In questo modo il proprietario della foresta per correggere gli errori, ad esempio un errore in un elenco di controllo di accesso (ACL) e per recuperare i sottoalberi delegati quando gli amministratori dei dati vengono terminati.  
  
## <a name="account-ous-and-resource-ous"></a>OU di account e OU di risorse  
OU di account contengono oggetti utente, gruppo e computer. I proprietari dell'insieme di strutture è necessario creare una struttura OU per gestire questi oggetti e quindi delegare il controllo della struttura al proprietario dell'unità Organizzativa. Se si distribuisce un nuovo dominio di Active Directory Domain Services, creare un OU di account per il dominio in modo che è possibile delegare il controllo degli account nel dominio.  
  
Le unità organizzative risorse contengono risorse e gli account che sono responsabili della gestione di tali risorse. È anche responsabile della creazione di una struttura OU per gestire queste risorse e della delega del controllo di tale struttura al proprietario dell'unità Organizzativa proprietario della foresta. Crea risorsa le unità organizzative in base alle necessità in base ai requisiti di ogni gruppo di nell'organizzazione per autonomia per la gestione dei dati e apparecchiature.  
  
## <a name="documenting-the-ou-design-for-each-domain"></a>Come documentare la progettazione dell'unità Organizzativa per ogni dominio  
Assemblare un team per progettare la struttura OU che consente di delegare il controllo sulle risorse all'interno della foresta. Proprietario della foresta potrebbe essere coinvolto nel processo di progettazione e necessario approvare la progettazione dell'unità Organizzativa. Si potrebbe richiedere almeno un amministratore del servizio per garantire che il progetto è valido. Gli altri partecipanti del team di progettazione potrebbero includere gli amministratori dei dati che funzioneranno su unità organizzative e l'unità Organizzativa proprietari che saranno responsabili per la loro gestione.  
  
È importante documentare la progettazione dell'unità Organizzativa. Elencare i nomi delle unità organizzative che si intende creare. Inoltre, per ogni unità Organizzativa, documentare il tipo di unità Organizzativa, il proprietario dell'unità Organizzativa, l'elemento padre dell'unità Organizzativa (se applicabile) e l'origine dell'unità Organizzativa.  
  
Per un foglio di lavoro agevolare la documentazione della progettazione dell'unità Organizzativa, scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip dal processo di supporto per Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) e aprire " Identificazione delle unità organizzative per ogni dominio"(DSSLOGI_9.doc).  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Esaminare i concetti di progettazione OU](../../ad-ds/plan/Reviewing-OU-Design-Concepts.md)  
  
-   [Delega dell'amministrazione tramite oggetti di OU](../../ad-ds/plan/Delegating-Administration-by-Using-OU-Objects.md)  
  


