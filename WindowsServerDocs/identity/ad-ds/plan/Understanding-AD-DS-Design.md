---
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: Informazioni sulla progettazione di Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 09afe3d19add87327d05bfafba6e0492a278b968
ms.sourcegitcommit: 1c3e6375b2e8eb01cfd299d0e9478fee46905c99
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/27/2018
---
# <a name="understanding-ad-ds-design"></a>Informazioni sulla progettazione di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le organizzazioni possono utilizzare servizi di dominio Active Directory (AD DS) in Windows Server per semplificare la gestione di utenti e risorse durante la creazione di infrastrutture scalabile, sicura e gestibile. È possibile utilizzare servizi di dominio Active Directory per gestire l'infrastruttura di rete, inclusi succursale, Microsoft Exchange Server e più ambienti di foresta.  
  
Un progetto di distribuzione di dominio Active Directory è suddivisa in tre fasi: una fase di progettazione, una fase di distribuzione e una fase di operazioni. Durante la fase di progettazione, il team di progettazione crea una progettazione per la struttura logica di dominio Active Directory che meglio soddisfi le esigenze di ogni reparto dell'organizzazione che verrà utilizzato il servizio directory. Dopo l'approvazione di progettazione, il team di distribuzione verifica la progettazione in un ambiente lab e quindi implementa la progettazione nell'ambiente di produzione. Poiché il test viene eseguito dal team di distribuzione e potrebbe potenzialmente influire sulla fase di progettazione, è un'attività provvisoria che si sovrappone sia progettazione e distribuzione. Una volta completata la distribuzione, il team operativo è responsabile della manutenzione del servizio directory.  
  
Sebbene le strategie di progettazione e distribuzione di Windows Server Active Directory che sono presentate in questa guida si basano su ampia lab e test pilota programma e corretta implementazione negli ambienti dei clienti, potrebbe essere necessario personalizzare il progetto di dominio Active Directory e distribuzione per adattarlo ambienti complessi specifico.  
  
-   Per ulteriori informazioni sulla distribuzione di dominio Active Directory in un ambiente di succursali, vedere il Read-Only Controller di dominio (RODC) Branch Office Planning Guide ([https://go.microsoft.com/fwlink/?LinkId=100207](https://go.microsoft.com/fwlink/?LinkId=100207)).  
  
-   Per ulteriori informazioni sulla distribuzione di Active Directory in un ambiente Exchange, vedere Exchange 2007 - pianificazione di Active Directory ([https://go.microsoft.com/fwlink/?LinkId=88904](https://go.microsoft.com/fwlink/?LinkId=88904)).  
  
-   Per ulteriori informazioni sulla distribuzione di dominio Active Directory in un ambiente con più foreste, vedere Considerazioni relative alla foresta più in Windows 2000 e Windows Server 2003 ([https://go.microsoft.com/fwlink/?LinkId=88905](https://go.microsoft.com/fwlink/?LinkId=88905)).  
  


