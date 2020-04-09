---
title: Novità di Windows Server 2016 Essentials
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: affff774-5fa6-4944-887a-9bfde05f6a3f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 420d3b043959b8b1201aad7a5b3210fd9bd6a0da
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817754"
---
# <a name="whats-new-in-windows-server-2016-essentials"></a>Novità di Windows Server 2016 Essentials

> Si applica a: Windows Server 2016 Essentials

Di seguito sono riportate le funzionalità nuove e migliorate di Windows Server 2016 Essentials.

## <a name="integration-with-azure-site-recovery-services"></a>[Integrazione con Azure Site Recovery Services](azure-site-recovery-services-integration.md)

** --&reg;** quando una macchina virtuale protetta ha esito negativo o se il server host in cui viene eseguita la macchina virtuale protetta non riesce, il failover con Azure Site Recovery Services mantiene la continuità aziendale fino a quando la macchina virtuale o il server host locale non viene ripristinato e disponibile. 

**Come funziona** : Azure Site Recovery Services, offerto in Microsoft Azure, consente la replica in tempo reale delle macchine virtuali (VM) in un insieme di credenziali per il backup in Azure. Se il server o il sito si arresta a causa di un errore hardware o di altro genere, è possibile eseguire il failover con Azure Site Recovery servizi in modo che l'immagine di macchina virtuale archiviata nell'insieme di credenziali di backup venga sottoposta a provisioning come macchina virtuale in esecuzione in Azure. In combinazione con una rete virtuale di Azure, i computer client che in precedenza erano connessi al server locale si connetteranno in modo trasparente al server in esecuzione in Azure.     
                                                                                                                                                                                                                                                                                                               

## <a name="integration-with-azure-virtual-network"></a>[Integrazione con rete virtuale di Azure](azure-virtual-network-integration.md)

**Che cosa fa**, in quanto le organizzazioni si ricloud computingno, ma raramente spostano tutte le risorse in una sola volta. Ma spostano alcune risorse nel cloud e le conservano in locale. In questo modo, è facile spostare un'organizzazione nel cloud in fasi nel tempo. L'integrazione della rete virtuale di Azure offre l'infrastruttura di rete che rende il processo trasparente e gestibile.

**Come funziona** : la rete virtuale di Azure è un servizio offerto in Microsoft Azure che consente alle organizzazioni di creare una rete privata virtuale da punto a punto (P2P) o da sito a sito (S2S) che rende le risorse in esecuzione in Azure (ad esempio macchine virtuali e archiviazione) come se fossero nella rete locale per l'accesso trasparente alle applicazioni e alle risorse.



## <a name="support-for-larger-deployments"></a>[Supporto per distribuzioni di grandi dimensioni](support-for-larger-deployments.md) 

Alcune piccole imprese più grandi necessitano di più funzionalità e capacità per implementare in modo efficace Windows Server Essentials. Windows Server 2016 Essentials offre una maggiore gestibilità di domini, utenti e dispositivi grazie all'aggiunta del supporto per distribuzioni di grandi dimensioni con:                                                                                                                                                                                                 

 - più domini
 - più controller di dominio                                                                                                                                                                                                                                        
 - possibilità di specificare un controller di dominio designato                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       

<a name="see-also"></a>Vedere anche
--------

[Introduzione a Windows Server Essentials](get-started.md) &copy;&reg;