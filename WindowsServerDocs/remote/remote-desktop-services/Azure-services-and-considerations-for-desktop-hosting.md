---
title: Servizi di Azure e considerazioni per l'hosting del desktop
description: Informazioni sulle considerazioni specifiche per Azure con una soluzione di hosting di Desktop remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.topic: article
ms.assetid: 0f402ae3-5391-4c7d-afea-2c5c9044de46
author: heidilohr
manager: lizross
ms.openlocfilehash: f73f28500c136ec8bdd32084cc5949f5e9804699
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80818524"
---
# <a name="azure-services-and-considerations-for-desktop-hosting"></a>Servizi di Azure e considerazioni per l'hosting del desktop

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Le sezioni seguenti descrivono i servizi di infrastruttura Azure.
  
## <a name="azure-portal"></a>Portale di Azure

Dopo che il provider ha creato una sottoscrizione di Azure, è possibile usare il portale di Azure per creare manualmente l'ambiente di ogni tenant. Questo processo può anche essere automatizzato tramite script di PowerShell.  

Per altre informazioni, visita il sito Web [Microsoft Azure](https://www.azure.microsoft.com).
  
## <a name="azure-load-balancer"></a>Servizio di bilanciamento del carico di Azure

I componenti del tenant vengono eseguiti in macchine virtuali che comunicano tra loro su una rete isolata. Durante il processo di distribuzione, puoi accedere esternamente a queste macchine virtuali tramite Azure Load Balancer con gli endpoint di Remote Desktop Protocol o un endpoint di PowerShell remoto. Una volta completata una distribuzione, in genere questi endpoint vengono eliminati in modo da ridurre la superficie di attacco. Gli unici endpoint saranno gli endpoint HTTPS e UDP creati per la macchina virtuale che esegue i componenti di Gateway Desktop remoto e Web Desktop remoto. In questo modo i client su Internet possono connettersi alle sessioni in esecuzione nel servizio di hosting desktop del tenant. Se un utente apre un'applicazione che si connette a Internet, ad esempio un Web browser, le connessioni verranno passate tramite Azure Load Balancer.  
  
Per altre informazioni, vedi [Informazioni su Azure Load Balancer](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-load-balance/).
  
## <a name="security-considerations"></a>Considerazioni sulla sicurezza

La Guida all'architettura di riferimento dell'hosting desktop di Azure è progettata per fornire un ambiente altamente sicuro e isolato per ogni tenant. La sicurezza del sistema dipende anche dalle misure di sicurezza implementate dal provider durante la distribuzione e l'uso del servizio ospitato. Di seguito sono riportate alcune considerazioni di cui il provider deve tenere conto per mantenere la sicurezza della soluzione di hosting desktop basata su questa architettura di riferimento.

- Tutte le password amministrative devono essere complesse, preferibilmente generate in modo casuale, modificate di frequente e salvate in una posizione centralizzata, sicura e accessibile solo a pochi amministratori del provider selezionati.  
- In caso di replica dell'ambiente di tenant per nuovi tenant, evita di usare le stesse password amministrative oppure password vulnerabili.
- L'URL del sito di Accesso Web Desktop remoto, nonché il nome e i certificati devono essere univoci e riconoscibili per ogni tenant per impedire attacchi di spoofing.  
- Durante il normale funzionamento del servizio di hosting desktop, è necessario eliminare tutti gli indirizzi IP pubblici per tutte le macchine virtuali, ad eccezione della macchina virtuale Web Desktop remoto e Gateway Desktop remoto, che consente agli utenti di connettersi in modo sicuro al servizio cloud di hosting desktop del tenant. È possibile aggiungere temporaneamente indirizzi IP pubblici quando necessario per attività di gestione, ma successivamente questi indirizzi devono sempre essere eliminati.  
  
Per altre informazioni, vedere gli articoli seguenti:

- [Sicurezza e protezione](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831778(v=ws.11))  
- [Procedure di sicurezza consigliate per IIS 8](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj635855(v=ws.11))  
- [Proteggere Windows Server 2012 R2](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831360(v=ws.11))  
  
## <a name="design-considerations"></a>Considerazioni sulla progettazione

È importante prendere in considerazione i vincoli dei servizi di infrastruttura Microsoft Azure quando si progetta un servizio di hosting desktop multi-tenant. Di seguito sono riportate alcune considerazioni di cui il provider deve tenere conto per realizzare una soluzione di hosting desktop funzionale e conveniente basata su questa architettura di riferimento.  
  
- Una sottoscrizione di Azure prevede un numero massimo di reti virtuali, core di macchina virtuale e servizi cloud che è possibile usare. Se un provider necessita di più risorse, potrebbe essere necessario creare più sottoscrizioni.
- Un servizio cloud Azure prevede un numero massimo di macchine virtuali che è possibile usare. È possibile che il provider debba creare più servizi cloud per tenant di dimensioni maggiori che superano il limite massimo.  
- I costi di distribuzione di Azure dipendono parzialmente dal numero e dalle dimensioni delle macchine virtuali. Il provider deve ottimizzare il numero e le dimensioni delle macchine virtuali per ogni tenant per fornire un ambiente di hosting desktop funzionale e altamente sicuro a un costo inferiore.  
- Le risorse del computer fisico nel data center di Azure sono virtualizzate tramite Hyper-V. Gli host Hyper-V non sono configurati in cluster host, pertanto la disponibilità delle macchine virtuali dipende dalla disponibilità dei singoli server usati nell'infrastruttura Azure. Per garantire una disponibilità maggiore, è possibile creare più istanze di ogni macchina virtuale del servizio ruolo in un set di disponibilità e quindi implementare il clustering guest nelle macchine virtuali.  
- In una configurazione di archiviazione tipica un provider di servizi avrà un singolo account di archiviazione con più contenitori (ad esempio, uno per ogni tenant) e più dischi in ogni contenitore. È previsto tuttavia un limite per l'archiviazione totale e le prestazioni che possono essere ottenute per ogni account di archiviazione. Per i provider di servizi che supportano un numero elevato di tenant oppure tenant con requisiti di prestazioni o capacità di archiviazione elevate, è possibile che il provider di servizi debba creare più account di archiviazione.  
  
Per altre informazioni, vedere gli articoli seguenti:

- [Dimensioni dei servizi cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs)  
- [Dettagli sui prezzi delle macchine virtuali di Microsoft Azure](https://azure.microsoft.com/pricing/details/virtual-machines/)  
- [Panoramica di Hyper-V](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831531(v=ws.11))  
- [Obiettivi di scalabilità e prestazioni di Archiviazione di Azure](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)  

## <a name="azure-active-directory-application-proxy"></a>Azure Active Directory Application Proxy

Azure Active Directory Application Proxy è un servizio fornito in SKU a pagamento di Azure AD che consentono agli utenti di connettersi alle applicazioni interne tramite il servizio di proxy inverso di Azure. In questo modo gli endpoint Web Desktop remoto e Gateway Desktop remoto possono essere nascosti all'interno della rete virtuale, eliminando la necessità dell'esposizione su Internet tramite un indirizzo IP pubblico. I provider di servizi di hosting possono usare Azure Active Directory Application Proxy per ridurre il numero di macchine virtuali nell'ambiente del tenant mantenendo al tempo stesso una distribuzione completa. Azure Active Directory Application Proxy consente inoltre di sfruttare molti dei vantaggi forniti da Azure AD, ad esempio l'accesso condizionale e l'autenticazione a più fattori.

Per altre informazioni, vedi [Introduzione al proxy di applicazione e all'installazione del connettore](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-enable).