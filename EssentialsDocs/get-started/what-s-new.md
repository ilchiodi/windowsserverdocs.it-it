---
title: "Novità in Windows Server 2016 Essentials"
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
ms.openlocfilehash: 7fed7e71f7ac163437fe5d32da7c867f93fbcf00
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
#<a name="whats-new-in-windows-server-2016-essentials"></a>Novità in Windows Server 2016 Essentials

> Si applica a: Windows Server 2016 Essentials

Di seguito sono nuove e avanzate funzionalità di Windows Server 2016 Essentials.

##[<a name="integration-with-azure-site-recovery-services"></a>Integrazione con servizi di ripristino del sito di Azure](azure-site-recovery-services-integration.md)

**Effetto** - quando una macchina virtuale che viene protetto avrà esito negativo, o il server host che esegue la macchina virtuale protetta in avrà esito negativo, eseguire il failover con servizi di ripristino del sito di Azure mantiene la continuità aziendale finché la macchina virtuale locale su o server host viene ripristinato e disponibile. 

**Come funziona** -servizi di ripristino del sito di Azure, disponibile in Microsoft Azure, consente la replica in tempo reale delle macchine virtuali (VM) in un archivio di backup in Azure. Nel caso in cui il server o il sito viene interrotto a causa di un errore hardware o altri, è possibile il failover con servizi di ripristino del sito di Azure in modo che l'immagine di macchina virtuale archiviata nell'insieme di credenziali di backup verrà eseguito come una macchina virtuale in esecuzione in Azure. In combinazione con una rete virtuale di Azure, client PC che in precedenza connesso al server locale in modo trasparente si connetterà al server in esecuzione in Azure.     
                                                                                                                                                                                                                                                                                                               

## [<a name="integration-with-azure-virtual-network"></a>Integrazione con la rete virtuale di Azure](azure-virtual-network-integration.md)

**Effetto**- come le organizzazioni effettuare verso il cloud computing, essi raramente spostare tutte le risorse in una sola volta. Piuttosto, spostare alcune risorse nel cloud e conservare alcune in locale. In questo modo, è facile spostare un'organizzazione nel cloud in fasi nel tempo. Integrazione di rete virtuale Azure fornisce l'infrastruttura di rete che rende il processo semplice e gestibile.

**Come funziona** - rete virtuale di Azure è un servizio offerto in Microsoft Azure che consente alle organizzazioni di creare un (P2P) Point-to-site-to-site (S2S) rete privata virtuale che rende le risorse che sono in esecuzione o in cerca di Azure (ad esempio, le macchine virtuali e archiviazione) come se fossero nella rete locale per l'accesso trasparente applicazioni e risorse.



##[<a name="support-for-larger-deployments"></a>Supporto per distribuzioni estese](support-for-larger-deployments.md) 

Alcune piccole aziende più grandi necessario più funzionalità e capacità di implementare in modo efficace di Windows Server Essentials. Windows Server 2016 Essentials offre maggiore facilità di gestione di domini, utenti e dispositivi aggiungendo il supporto per le distribuzioni più grandi con:                                                                                                                                                                                                 

 - più domini
 - più controller di dominio                                                                                                                                                                                                                                        
 - possibilità di specificare un controller di dominio designato                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       

<a name="see-also"></a>Vedere anche
--------

[Introduzione a Windows Server Essentials](get-started.md)
