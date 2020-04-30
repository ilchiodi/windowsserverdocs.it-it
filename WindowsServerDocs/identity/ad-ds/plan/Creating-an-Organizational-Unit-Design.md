---
ms.assetid: b8df1828-5ead-4c90-b0fe-95c675116b7c
title: Creazione di un progetto di unità organizzativa
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 8e92ad4b280572fc3b44a0161af9a4ea25653c89
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624319"
---
# <a name="creating-an-organizational-unit-design"></a>Creazione di un progetto di unità organizzativa

> Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I proprietari della foresta sono responsabili della creazione di progetti di unità organizzativa (OU) per i loro domini. La progettazione di un'unità organizzativa comporta la progettazione della struttura dell'unità organizzativa, l'assegnazione del ruolo proprietario unità organizzativa e la creazione di account e unità organizzative di risorse.

Inizialmente, progettare la struttura dell'unità organizzativa per abilitare la delega dell'amministrazione. Al termine della progettazione dell'unità organizzativa, è possibile creare ulteriori strutture di unità organizzative per l'applicazione di Criteri di gruppo a utenti e computer e per limitare la visibilità degli oggetti. Per ulteriori informazioni, vedere [progettazione di un'infrastruttura Criteri di gruppo](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786524(v=ws.10)).

## <a name="ou-owner-role"></a>Ruolo proprietario unità organizzativa
Il proprietario della foresta designa un proprietario OU per ogni unità organizzativa progettata per il dominio. I proprietari delle unità organizzative sono responsabili dei dati che controllano un sottoalbero di oggetti in Active Directory Domain Services (AD DS). I proprietari delle unità organizzative possono controllare la delega dell'amministrazione e la modalità di applicazione dei criteri agli oggetti all'interno dell'unità organizzativa. Possono inoltre creare nuovi sottoalberi e delegare l'amministrazione di unità organizzative all'interno di tali sottoalberi.

Poiché i proprietari delle unità organizzative non possiedono o controllano il funzionamento del servizio directory, è possibile separare la proprietà e l'amministrazione del servizio directory dalla proprietà e dall'amministrazione degli oggetti, riducendo il numero di amministratori del servizio con livelli di accesso elevati.

Le unità organizzative forniscono autonomia amministrativa e i mezzi per controllare la visibilità degli oggetti nella directory. Le unità organizzative forniscono l'isolamento da altri amministratori dei dati, ma non forniscono isolamento dagli amministratori del servizio. Sebbene i proprietari delle unità organizzative dispongano del controllo su un sottoalbero di oggetti, il proprietario della foresta mantiene il controllo completo su tutti i sottoalberi. Ciò consente al proprietario della foresta di correggere gli errori, ad esempio un errore in un elenco di controllo di accesso (ACL) e di recuperare i sottoalberi delegati quando gli amministratori dei dati vengono terminati.

## <a name="account-ous-and-resource-ous"></a>Unità organizzative di account e di risorse
Le unità organizzative dell'account contengono oggetti utente, gruppo e computer. I proprietari della foresta devono creare una struttura dell'unità organizzativa per gestire questi oggetti e quindi delegare il controllo della struttura al proprietario dell'unità organizzativa. Se si distribuisce un nuovo dominio di servizi di dominio Active Directory, creare una OU dell'account per il dominio in modo che sia possibile delegare il controllo degli account nel dominio.

Le unità organizzative di risorse contengono risorse e gli account responsabili della gestione di tali risorse. Il proprietario della foresta è inoltre responsabile della creazione di una struttura dell'unità organizzativa per gestire queste risorse e della delega del controllo di tale struttura al proprietario dell'unità organizzativa. Creare unità organizzative di risorse in base alle esigenze di ogni gruppo all'interno dell'organizzazione per autonomia nella gestione dei dati e delle apparecchiature.

## <a name="documenting-the-ou-design-for-each-domain"></a>Documentazione della progettazione dell'unità organizzativa per ogni dominio
Assemblare un team per progettare la struttura dell'unità organizzativa utilizzata per delegare il controllo sulle risorse all'interno della foresta. Il proprietario della foresta potrebbe essere associato al processo di progettazione e deve approvare la progettazione dell'unità organizzativa. È anche possibile coinvolgere almeno un amministratore del servizio per assicurarsi che la progettazione sia valida. Altri partecipanti al team di progettazione possono includere gli amministratori dei dati che lavorano sulle unità organizzative e i proprietari dell'unità organizzativa che saranno responsabili della loro gestione.

È importante documentare la progettazione dell'unità organizzativa. Elenca i nomi delle unità organizzative che si prevede di creare. Per ogni unità organizzativa, quindi, documentare il tipo di unità organizzativa, il proprietario dell'unità organizzativa, l'unità organizzativa padre (se applicabile) e l'origine di tale unità organizzativa.

Per un foglio di lavoro che assiste l'utente nella documentazione della progettazione dell'unità organizzativa, scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip da [supporti per i processi per Windows Server 2003 Deployment Kit](https://microsoft.com/download/details.aspx?id=9608) e aprire "identificazione delle unità organizzative per ogni dominio" (DSSLOGI_9. doc).

## <a name="in-this-section"></a>Contenuto della sezione

- [Revisione dei concetti di progettazione di unità organizzative](../../ad-ds/plan/Reviewing-OU-Design-Concepts.md)

- [Delega dell'amministrazione tramite oggetti unità organizzative](../../ad-ds/plan/Delegating-Administration-by-Using-OU-Objects.md)
