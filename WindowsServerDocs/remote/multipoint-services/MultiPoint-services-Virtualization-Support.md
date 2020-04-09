---
title: Supporto della virtualizzazione di Servizi MultiPoint
description: Viene descritto come usare i servizi MultiPoint con Hyper-V
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 3f0864b8-a087-4890-94ef-05efbd3c4241
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: d23034b6a70d36df259fc26829b36ebebeade5ab
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857624"
---
# <a name="multipoint-services-virtualization-support"></a>Supporto della virtualizzazione di Servizi MultiPoint
MultiPoint Services supporta il ruolo Hyper-V in due modi:  
  
-   MultiPoint Services può essere distribuito come sistema operativo guest in un server che esegue Hyper-V.  
  
-   MultiPoint Services può essere usato come server di virtualizzazione.   
  
L'esecuzione di MultiPoint Services in una macchina virtuale consente di usare gli strumenti di Hyper-V per gestire i sistemi operativi. Questi strumenti includono funzionalità di checkpoint e di rollback che consentono di esportare e importare macchine virtuali. Per installazioni di dimensioni maggiori, è possibile consolidare i server eseguendo più computer virtuali MultiPoint Services in un singolo server fisico. Di seguito sono illustrati alcuni degli scenari possibili:  
  
-   Una singola classe o un Lab ha più di 20 postazioni. Invece di distribuire più computer fisici che eseguono MultiPoint Services, è possibile distribuire più macchine virtuali in un singolo computer fisico.  
  
    > [!NOTE]  
    > È possibile gestire più server MultiPoint, sia fisici che virtuali, tramite una singola console di gestione MultiPoint.  
  
-   Il server MultiPoint è in esecuzione in una macchina virtuale con un'altra infrastruttura server nello stesso computer fisico. In tal caso, questa infrastruttura Server centralizza il dominio, la sicurezza e i dati per la rete. Il server MultiPoint fornisce Servizi Desktop remoto e centralizza i desktop.  
  
> [!NOTE]  
> Quando si eseguono Servizi MultiPoint in una macchina virtuale, sono supportate le stazioni USB-over-Ethernet e RDP client. Le stazioni di connessione video diretta e USB zero client non sono supportate.  
  
Per ulteriori informazioni sul ruolo Hyper-V, vedere [Hyper-v](../../virtualization/hyper-v/hyper-v-on-windows-server.md).  