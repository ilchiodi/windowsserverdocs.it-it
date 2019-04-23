---
title: Supporto della virtualizzazione di Servizi MultiPoint
description: Viene descritto come utilizzare servizi MultiPoint con Hyper-V
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3f0864b8-a087-4890-94ef-05efbd3c4241
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 06d518dcea154ac2bab49a7d0e83a90f96be6e44
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872522"
---
# <a name="multipoint-services-virtualization-support"></a>Supporto della virtualizzazione di Servizi MultiPoint
Servizi multiPoint supporta il ruolo Hyper-V in due modi:  
  
-   Servizi multiPoint possono essere distribuiti come un sistema operativo guest in un server che esegue Hyper-V.  
  
-   MultiPoint Services può essere utilizzato come un server di virtualizzazione.   
  
L'esecuzione di servizi MultiPoint in una macchina virtuale consente l'uso degli strumenti di Hyper-V per gestire i sistemi operativi. Questi strumenti includono le funzionalità di checkpoint e rollback e consentono di esportare e importare macchine virtuali. Per le installazioni di dimensioni maggiori, è possibile consolidare server tramite l'esecuzione di più computer virtuale MultiPoint Services in un unico server fisico. Di seguito sono illustrati alcuni degli scenari possibili:  
  
-   Una sola aula o lab ha più di 20 postazioni. Invece di distribuire più computer fisici che eseguono MultiPoint Services, è possibile distribuire più macchine virtuali in un singolo computer fisico.  
  
    > [!NOTE]  
    > È possibile gestire più server MultiPoint, fisica o virtuale, tramite un'unica console di gestione MultiPoint.  
  
-   Il server MultiPoint è in esecuzione in una macchina virtuale con un'altra infrastruttura di server nello stesso computer fisico. In tal caso questa infrastruttura di server consente di centralizzare il dominio, sicurezza e i dati per la rete. Il server MultiPoint offre servizi Desktop remoto e centralizza i desktop.  
  
> [!NOTE]  
> Quando si esegue MultiPoint Services in una macchina virtuale, USB-over-Ethernet e stazioni client RDP sono supportate. Video diretta e USB zero stazioni client connesso non sono supportati.  
  
Per altre informazioni sul ruolo Hyper-V, vedere [Hyper-V](../../virtualization/hyper-v/hyper-v-on-windows-server.md).  