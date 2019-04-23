---
title: Architettura di riferimento dell'hosting del desktop
description: Indicazioni sull'architettura per la creazione di un soluzione con Azure e servizi desktop remoto di hosting del desktop.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/02/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1bac5dd3-8430-46ee-8bef-10cc4b7cc437
author: lizap
manager: dongill
ms.openlocfilehash: 6f235fd89c34c00601c802f4ea71e440af630169
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890242"
---
# <a name="desktop-hosting-reference-architecture"></a>Architettura di riferimento dell'hosting del desktop

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo articolo definisce un set di blocchi dell'architettura per l'uso di Servizi Desktop remoto (RDS) e le macchine virtuali di Microsoft Azure per creare multi-tenant, ospitato applicazione e desktop Windows services, che chiameremo "hosting del desktop". È possibile usare questo riferimento architettura per creare soluzioni per piccole e medie organizzazioni con utenti da 5 a 5000 dell'hosting del desktop altamente sicuro, scalabile e affidabile.    
  
I principali destinatari di questa architettura di riferimento ospitano provider che vogliono sfruttare servizi di infrastruttura di Microsoft Azure per offrire servizi di hosting del desktop e licenze SAL (Subscriber) Access a più tenant tramite il [ Microsoft Service Provider Licensing Agreement](https://www.microsoft.com/hosting/en/us/licensing/splabenefits.aspx) programma (SPLA). Un secondo gruppo di destinatari di questa architettura di riferimento sono i clienti finali che vogliono creare e gestire soluzioni di hosting del desktop nei servizi infrastruttura di Microsoft Azure per i propri dipendenti che li usano [CAL utente i diritti tramite il Software estesi Assurance](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf) (SA).   
  
Per fornire un desktop, soluzioni di hosting di hosting partner e clienti SA usano Windows Server per offrire un'esperienza di applicazione che è familiare agli utenti aziendali e gli utenti di Windows. Basato su concetti fondamentali di Windows 10, Windows Server 2016 fornisce supporto per applicazioni familiari e utente esperienza.    
  
L'ambito di questo documento è limitato a:   
  
* Linee guida di progettazione dell'architettura per un servizio di hosting del desktop. Informazioni dettagliate, ad esempio le procedure di distribuzione, prestazioni e la pianificazione della capacità viene spiegate in documenti distinti. Per informazioni più generali sui servizi infrastruttura di Azure, vedere [macchine virtuali di Microsoft Azure](https://azure.microsoft.com/documentation/services/virtual-machines/).   
  
* Desktop basati su sessione, le applicazioni RemoteApp e desktop personali basato su server che utilizzano Windows Server 2016 Host sessione Desktop remoto (Host sessione Desktop remoto). Poiché non esiste alcun Service Provider License Agreement (SPLA) per i sistemi operativi client Windows, Windows basata su client infrastrutture desktop virtuali non vengono analizzate. Sono consentite le finestre infrastrutture desktop virtuali in base al Server sotto il SPLA e Windows basati su client infrastrutture desktop virtuali sono consentite su hardware dedicato con licenze di clienti finali in determinati scenari. Tuttavia, infrastrutture desktop virtuali basati su client sono out-of-scope per questo documento.   
  
* I prodotti Microsoft e le funzionalità, principalmente i servizi di infrastruttura di Microsoft Azure e Windows Server 2016.   
  
* Hosting del desktop di servizi per i tenant che vanno in dimensioni comprese tra 5 e 5.000 utenti.   Per i tenant più grandi, si potrebbe essere necessario modificare questa architettura per garantire prestazioni adeguate. L'interfaccia utente grafica di Server Manager RDS (GUI) non è consigliabile per le distribuzioni oltre 500 utenti. PowerShell è consigliato per la gestione delle distribuzioni di servizi desktop remoto tra gli utenti di 500 a 5000.   
  
* Il set minimo di componenti e servizi necessari per un servizio di hosting del desktop. Esistono molti servizi che possono essere aggiunti per migliorare un servizio di hosting del desktop e i componenti facoltativi, ma si tratta di out-of-scope per questo documento.    
  
Dopo aver letto questo documento, è necessario comprendere il lettore:   
- I blocchi predefiniti necessari fornire un desktop sicuro, affidabile, multi-tenant soluzione di hosting basata in servizi di Microsoft Azure.  
- Lo scopo di ogni blocco predefinito e come interagiscono.  
  
Esistono diversi modi per compilare un soluzione basata su questa architettura di hosting del desktop. Questa architettura illustra l'integrazione e i miglioramenti in Azure con Windows Server 2016. Altre opzioni di distribuzione sono disponibili con il [Guida sull'architettura di riferimento di Hosting Desktop](https://go.microsoft.com/fwlink/p/?LinkId=517389) per Windows Server 2012 R2.    
  
Vengono trattati i seguenti argomenti:  
- [Architettura logica dell'hosting del desktop](Desktop-hosting-logical-architecture.md)  
- [Comprendere i ruoli Servizi Desktop remoto](Understanding-RDS-roles.md)
- [Comprendere l'ambiente di hosting del desktop](Understanding-the-desktop-hosting-environment.md)  
- [Servizi di Azure e considerazioni per l'hosting del desktop](Azure-services-and-considerations-for-desktop-hosting.md)
  
 


