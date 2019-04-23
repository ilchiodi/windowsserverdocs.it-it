---
title: Servizi di Azure e considerazioni per l'hosting del desktop
description: Informazioni sulle considerazioni specifiche per Azure con una soluzione di hosting del Desktop remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f402ae3-5391-4c7d-afea-2c5c9044de46
author: heidilohr
manager: dougkim
ms.openlocfilehash: 37210a5d75399309c53364f5b8ee9e06d26d6f32
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849802"
---
# <a name="azure-services-and-considerations-for-desktop-hosting"></a>Servizi di Azure e considerazioni per l'hosting del desktop

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Le sezioni seguenti descrivono i servizi di infrastruttura di Azure.
  
## <a name="azure-portal"></a>Portale di Azure

Dopo che il provider ha creato una sottoscrizione di Azure, il portale di Azure è utilizzabile per creare manualmente l'ambiente di ogni tenant. Questo processo può anche essere automatizzato tramite script di PowerShell.  

Per altre informazioni, visitare il [Microsoft Azure](https://www.azure.microsoft.com) sito Web.
  
## <a name="azure-load-balancer"></a>Servizio di bilanciamento del carico di Azure

Componenti del tenant eseguiti nelle macchine virtuali che comunicano tra loro in una rete isolata. Durante il processo di distribuzione, è possibile accedere esternamente queste macchine virtuali tramite il bilanciamento del carico di Azure tramite gli endpoint di Remote Desktop Protocol o un endpoint di PowerShell remoto. Una volta completata una distribuzione, in genere questi endpoint verranno eliminati in modo da ridurre la superficie di attacco. Gli endpoint soli saranno gli endpoint HTTPS e UDP creati per la macchina virtuale in esecuzione i componenti di Gateway Desktop remoto e il Web desktop remoto. In questo modo i client su internet per connettersi alle sessioni in esecuzione nel servizio di hosting del desktop del tenant. Se un utente apre un'applicazione che si connette a internet, ad esempio un web browser, le connessioni verranno passate tramite il bilanciamento del carico di Azure.  
  
Per altre informazioni, vedere [che cos'è Azure Load Balancer?](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-load-balance/)
  
## <a name="security-considerations"></a>Considerazioni sulla sicurezza

Questo Desktop Hosting riferimento Guida all'architettura Azure è progettato per fornire un ambiente altamente sicuro e isolato per ogni tenant. Sicurezza del sistema dipende anche dalle misure di sicurezza eseguite dal provider durante la distribuzione e l'operazione del servizio ospitato. Di seguito sono riportate alcune considerazioni che il provider deve intraprendere per mantenere la soluzione di hosting del desktop basata su questa architettura di riferimento sicura.

- Tutte le password amministrative devono essere complesse, in teoria in modo casuale generato modificati di frequente e salvato in una posizione centralizzata sicura accessibile solo a una selezione numero esiguo di amministratori del provider.  
- Quando si replica l'ambiente di tenant per i nuovi tenant, evitare l'utilizzo delle password amministrative vulnerabili o stesso.
- L'URL del sito accesso Web desktop remoto, nome e i certificati devono essere univoco e riconoscibile per ogni tenant per impedire attacchi di spoofing.  
- Durante il normale funzionamento del servizio di hosting del desktop, è necessario eliminare tutti gli indirizzi IP pubblici per tutte le macchine virtuali, ad eccezione di macchina virtuale di Web desktop remoto e Gateway Desktop remoto che consente agli utenti di connettersi in modo sicuro al servizio cloud hosting del desktop del tenant. Gli indirizzi IP pubblici possono essere aggiunte temporaneamente quando è necessario per attività di gestione, ma devono sempre essere eliminati in un secondo momento.  
  
Per informazioni, vedi gli articoli seguenti:

- [Sicurezza e protezione](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831778(v=ws.11))  
- [Le procedure consigliate per IIS 8](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj635855(v=ws.11))  
- [Secure Windows Server 2012 R2](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831360(v=ws.11))  
  
## <a name="design-considerations"></a>Considerazioni per la progettazione

È importante prendere in considerazione i vincoli dei servizi di infrastruttura di Microsoft Azure quando si progetta un servizio di hosting del desktop multi-tenant. Nell'elenco seguente vengono descritte considerazioni che il provider deve eseguire per ottenere una soluzione di hosting desktop, funzionale e conveniente, basata su questa architettura di riferimento.  
  
- Una sottoscrizione di Azure è un numero massimo di reti virtuali, Core della macchina virtuale e i servizi Cloud che può essere utilizzato. Se un provider richiede più risorse rispetto a questo, si potrebbe essere necessario creare più sottoscrizioni.
- Un servizio Cloud di Azure ha un numero massimo di macchine virtuali che possono essere usate. Il provider potrebbe essere necessario creare più servizi Cloud per il tenant di dimensioni maggiori che superano il massimo.  
- I costi di distribuzione di Azure dipendono parzialmente il numero e dimensioni delle macchine virtuali. Il provider deve ottimizzare il numero e dimensioni delle macchine virtuali per ogni tenant per fornire un funzionale e altamente sicuro ambiente di Hosting del Desktop a un costo inferiore.  
- Le risorse del computer fisico nel data center di Azure sono virtualizzate tramite Hyper-V. Host Hyper-V non sono configurati in cluster host, in modo che la disponibilità delle macchine virtuali è dipende dalla disponibilità dei singoli server utilizzato nell'infrastruttura di Azure. Per garantire una disponibilità più elevata, più istanze di ciascun ruolo macchina virtuale del servizio possono essere create in un set di disponibilità, quindi il clustering guest può essere implementato nelle macchine virtuali.  
- In una configurazione tipica di archiviazione, un provider del servizio avrà un singolo account di archiviazione con più contenitori (ad esempio, uno per ogni tenant) e più dischi in ogni contenitore. Tuttavia, è previsto un limite per l'archiviazione totale e le prestazioni che possono essere ottenuta per ogni account di archiviazione. Per i provider di servizi che supportano un numero elevato di tenant o tenant con requisiti di capacità o prestazioni elevate di archiviazione, il provider del servizio potrebbe essere necessario creare più account di archiviazione.  
  
Per informazioni, vedi gli articoli seguenti:

- [Dimensioni dei servizi Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs)  
- [Macchina virtuale di Microsoft Azure i dettagli sui prezzi](https://azure.microsoft.com/pricing/details/virtual-machines/)  
- [Panoramica di Hyper-V](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831531(v=ws.11))  
- [Obiettivi di scalabilità e prestazioni archiviazione di Azure](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)  

## <a name="azure-active-directory-application-proxy"></a>Azure Active Directory Application Proxy

Proxy applicazione Azure Active Directory (AD) è un servizio fornito in a pagamento SKU di Azure AD che consentono agli utenti di connettersi alle applicazioni interne tramite il servizio di proxy inverso di Azure. In questo modo gli endpoint Web desktop remoto e Gateway Desktop remoto devono essere nascoste all'interno della rete virtuale, eliminando la necessità di essere esposti a internet tramite un indirizzo IP pubblico. Hosting può usare Azure AD Application Proxy per ridurre il numero di macchine virtuali nell'ambiente del tenant mantenendo al tempo stesso una distribuzione completa. Proxy applicazione Azure AD consente inoltre di sfruttare molti dei vantaggi forniti da Azure AD, ad esempio l'accesso condizionale e multi-factor authentication.

Per altre informazioni, vedere [iniziare a usare il Proxy di applicazione e installare il connettore](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-enable).