---
title: Novità di Windows Server 2016 Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: affff774-5fa6-4944-887a-9bfde05f6a3f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: bc83686f76c49773203d63a88894841f65ffd1d9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433764"
---
# <a name="whats-new-in-windows-server-2016-essentials"></a>Novità di Windows Server 2016 Essentials

> Si applica a: Windows Server 2016 Essentials

Seguito sono nuove e migliorate funzionalità in Windows Server 2016 Essentials.

## <a name="integration-with-azure-site-recovery-servicesazure-site-recovery-services-integrationmd"></a>[Integrazione con servizi di Azure Site Recovery](azure-site-recovery-services-integration.md)

**Cosa** , ovvero quando una macchina virtuale che è protetta ha esito negativo o il server host che viene eseguita la macchina virtuale protetta in ha esito negativo, eseguire il failover con servizi di Azure Site Recovery gestisce la continuità aziendale finché la macchina virtuale locale su o server host è corretto e disponibile. 

**Come funziona** -servizi di Azure Site Recovery, disponibile in Microsoft Azure, consente la replica in tempo reale delle macchine virtuali (VM) a un insieme di credenziali di backup in Azure. Nel caso in cui il server o il sito diventa inattivo a causa di un errore hardware o altri, è possibile effettuare il failover con servizi di Azure Site Recovery in modo che l'immagine di macchina virtuale archiviata nell'insieme di credenziali di backup verrà eseguito il provisioning come una macchina virtuale in esecuzione in Azure. In combinazione con una rete virtuale di Azure, client PC che precedentemente connesso al server locale in modo trasparente si connetterà al server in esecuzione in Azure.     
                                                                                                                                                                                                                                                                                                               

## <a name="integration-with-azure-virtual-networkazure-virtual-network-integrationmd"></a>[Integrazione con la rete virtuale di Azure](azure-virtual-network-integration.md)

**Cosa**: alle organizzazioni arrivino al cloud computing, raramente passano tutte le proprie risorse in una sola volta. Piuttosto, spostare alcune risorse nel cloud e mantenere alcune in locale. In questo modo, è facile spostare un'organizzazione nel cloud in più fasi nel corso del tempo. Integrazione rete virtuale di Azure fornisce l'infrastruttura di rete che rende il processo facile e gestibile.

**Come funziona** -rete virtuale di Azure è un servizio disponibile in Microsoft Azure che consente alle organizzazioni di creare un punto a punto (P2P) site-to-site (S2S) rete virtuale o privata che rende le risorse che sono in esecuzione in Azure (ad esempio le macchine virtuali e archiviazione) hanno un aspetto come se fossero nella rete locale per l'accesso alle risorse e applicazioni senza problemi.



## <a name="support-for-larger-deploymentssupport-for-larger-deploymentsmd"></a>[Supporto per distribuzioni di grandi dimensioni](support-for-larger-deployments.md) 

Alcune aziende di piccole dimensioni più grandi necessarie altre funzionalità e la capacità di implementare in modo efficace di Windows Server Essentials. Windows Server 2016 Essentials offre maggiore facilità di gestione di domini, utenti e dispositivi mediante l'aggiunta di supporto per distribuzioni più grandi con:                                                                                                                                                                                                 

 - più domini
 - più controller di dominio                                                                                                                                                                                                                                        
 - possibilità di specificare un controller di dominio designata                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       

<a name="see-also"></a>Vedere anche
--------

[Introduzione a Windows Server Essentials](get-started.md)
