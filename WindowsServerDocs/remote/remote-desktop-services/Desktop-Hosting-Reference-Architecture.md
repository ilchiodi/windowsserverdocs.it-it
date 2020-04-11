---
title: Architettura di riferimento dell'hosting del desktop
description: Indicazioni sull'architettura per la creazione di una soluzione di hosting desktop con Servizi Desktop remoto e Azure.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/02/2016
ms.topic: article
ms.assetid: 1bac5dd3-8430-46ee-8bef-10cc4b7cc437
author: lizap
manager: dongill
ms.openlocfilehash: c2bc0c2ba3d12ea1caf8737369ba882f69b111e4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818454"
---
# <a name="desktop-hosting-reference-architecture"></a>Architettura di riferimento dell'hosting del desktop

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Questo articolo definisce un set di componenti dell'architettura per l'uso di macchine virtuali di Servizi Desktop remoto e Microsoft Azure per creare servizi di desktop e applicazioni Windows multi-tenant ospitati a cui viene fatto riferimento come "hosting desktop". Puoi usare questa architettura di riferimento per creare soluzioni di hosting desktop altamente sicure, scalabili e affidabili per piccole e medie organizzazioni con un numero di utenti compreso tra 5 e 5000.    
  
I destinatari principali di questa architettura di riferimento sono i provider di hosting che intendono usare i servizi di infrastruttura Azure per offrire servizi di hosting desktop e licenze SAL (Subscriber Access License) a più tenant tramite il programma [SPLA (Service Provider License Agreement) di Microsoft](https://www.microsoft.com/hosting/en/us/licensing/splabenefits.aspx). Un secondo gruppo di destinatari di questa architettura di riferimento è costituito dai clienti finali che vogliono creare e gestire soluzioni di hosting desktop nei servizi di infrastruttura Microsoft Azure per i propri dipendenti che usano [diritti estesi di licenze CAL utente di Servizi Desktop remoto tramite Software Assurance](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf).   
  
Per offrire soluzioni di hosting desktop, i partner di hosting e i clienti Software Assurance si servono di Windows Server per garantire agli utenti di Windows un'esperienza di applicazione già nota a utenti aziendali e privati. Basato sui concetti fondamentali di Windows 10, Windows Server 2016 fornisce supporto per applicazioni note ed esperienza utente.    
  
L'ambito di questo documento è limitato a:   
  
* Indicazioni di progettazione dell'architettura per un servizio di hosting desktop. Informazioni dettagliate quali le procedure di distribuzione, le prestazioni e la pianificazione della capacità vengono illustrate in documenti separati. Per informazioni generali sui servizi di infrastruttura Azure, vedi [Macchine virtuali di Microsoft Azure](https://azure.microsoft.com/documentation/services/virtual-machines/).   
  
* Desktop basati su sessione, applicazioni RemoteApp e desktop personali basati su server che usano Host sessione Desktop remoto in Windows Server 2016. Le infrastrutture di desktop virtuali basati su Windows non sono coperte perché non esiste alcun programma Service Provider License Agreement (SPLA) per i sistemi operativi client Windows. Le infrastrutture di desktop virtuali basati su Windows Server sono consentite secondo il programma SPLA e le infrastrutture di desktop virtuali basati su client Windows sono consentite su hardware dedicato con licenze di clienti finali in determinati scenari. Tuttavia, le infrastrutture di desktop virtuali basati su client non rientrano nell'ambito di questo documento.   
  
* Prodotti e funzionalità Microsoft, principalmente servizi di infrastruttura Microsoft Azure e Windows Server 2016.   
  
* Servizi di hosting desktop per tenant che includono da 5 a 5.000 utenti.   Per i tenant di dimensioni maggiori, potrebbe essere necessario modificare questa architettura per garantire prestazioni adeguate. L'interfaccia utente grafica di Servizi Desktop remoto di Server Manager non è consigliata per le distribuzioni per oltre 500 utenti. PowerShell è consigliato per la gestione delle distribuzioni di Servizi Desktop remoto a un numero di utenti compreso tra 500 e 5000.   
  
* Set minimo di componenti e servizi necessari per un servizio di hosting desktop. Esistono molti componenti e servizi facoltativi che possono essere aggiunti per migliorare un servizio di hosting desktop, ma non rientrano nell'ambito di questo documento.    
  
Dopo aver letto questo documento, il lettore dovrebbe aver acquisito familiarità con i concetti seguenti:   
- I componenti di base necessari per fornire una soluzione di hosting desktop basata sui servizi di Microsoft Azure sicura, affidabile, multi-tenant.  
- Lo scopo di ogni componente e la modalità di interazione con gli altri.  
  
Esistono diversi modi per creare un soluzione di hosting desktop basata su questa architettura. Questa architettura illustra l'integrazione e i miglioramenti in Azure con Windows Server 2016. Altre opzioni di distribuzione sono disponibili con la [Guida sull'architettura di riferimento di hosting desktop](https://go.microsoft.com/fwlink/p/?LinkId=517389) per Windows Server 2012 R2.    
  
Vengono trattati i seguenti argomenti:  
- [Architettura logica dell'hosting desktop](Desktop-hosting-logical-architecture.md)  
- [Informazioni sui ruoli di Servizi Desktop remoto](Understanding-RDS-roles.md)
- [Informazioni sull'ambiente di hosting desktop](Understanding-the-desktop-hosting-environment.md)  
- [Servizi di Azure e considerazioni per l'hosting del desktop](Azure-services-and-considerations-for-desktop-hosting.md)
  
 


