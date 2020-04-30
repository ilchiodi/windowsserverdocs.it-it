---
ms.assetid: a91339ef-6ad4-445f-8ecc-a95fbcc04296
title: Pianificazione e progettazione di Servizi di dominio Active Directory
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5f88458c8e3f50853229f6f8f9c74fa4b8feba40
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624419"
---
# <a name="ad-ds-design-and-planning"></a>Pianificazione e progettazione di Servizi di dominio Active Directory

> Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Distribuendo Windows Server Active Directory Domain Services (AD DS) nell'ambiente in uso, è possibile sfruttare la funzionalità centralizzata, delegata e Single Sign-On (SSO) fornita da servizi di dominio Active Directory. Dopo aver identificato le attività di distribuzione e l'ambiente corrente per l'organizzazione, è possibile creare la strategia di distribuzione di servizi di dominio Active Directory che soddisfi le esigenze dell'organizzazione.

## <a name="about-this-guide"></a>Informazioni sulla guida

Questa guida fornisce consigli per sviluppare una strategia di distribuzione di servizi di dominio Active Directory in base ai requisiti dell'organizzazione e alla progettazione specifica che si desidera creare. Questa guida è rivolta agli specialisti di infrastruttura o agli architetti di sistema. Prima di leggere questa guida, è necessario avere una conoscenza ottimale del funzionamento di servizi di dominio Active Directory a livello funzionale. È inoltre necessario avere una conoscenza approfondita dei requisiti dell'organizzazione che verranno riflessi nella strategia di distribuzione di servizi di dominio Active Directory.

Questa guida descrive i set di attività per diversi punti di partenza possibili di una distribuzione di servizi di dominio Active Directory di Windows Server 2008. La Guida consente di determinare la strategia di distribuzione più appropriata per l'ambiente.

Sebbene le strategie presentate in questa guida siano appropriate per quasi tutte le distribuzioni del sistema operativo server, sono state testate e convalidate in modo specifico per gli ambienti che contengono meno di 100.000 utenti e meno di 1.000 siti, con connessioni di rete di almeno 28,8 kilobit al secondo (Kbps). Se l'ambiente non soddisfa questi criteri, è consigliabile utilizzare una società di consulenza che abbia esperienza nella distribuzione di servizi di dominio Active Directory in ambienti più complessi.

Per ulteriori informazioni sul test del processo di distribuzione di servizi di dominio Active Directory, vedere l'articolo [test e verifica del processo di distribuzione](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc772722(v=ws.10)).

## <a name="in-this-guide"></a>Contenuto della guida

[Informazioni sulla progettazione di Active Directory Domain Services](Understanding-AD-DS-Design.md)

[Identificazione dei requisiti di progettazione e distribuzione di Active Directory Domain Services](Identifying-Your-AD-DS-Design-and-Deployment-Requirements.md)

[Mapping dei requisiti per una strategia di distribuzione di Active Directory Domain Services](Mapping-Your-Requirements-to-an-AD-DS-Deployment-Strategy.md)

[Progettazione della struttura logica per servizi di dominio Active Directory di Windows Server 2008](Designing-the-Logical-Structure.md)

[Progettazione della topologia del sito per servizi di dominio Active Directory di Windows Server 2008](Designing-the-Site-Topology.md)

[Abilitazione delle funzionalità avanzate di Active Directory Domain Services](Enabling-Advanced-Features-for-AD-DS.md)

[Esempi di valutazione della strategia di distribuzione di Active Directory Domain Services](Evaluating-AD-DS-Deployment-Strategy-Examples.md)

[Appendice A: Termini importanti di Active Directory Domain Services](Appendix-A--Reviewing-Key-AD-DS-Terms.md)
