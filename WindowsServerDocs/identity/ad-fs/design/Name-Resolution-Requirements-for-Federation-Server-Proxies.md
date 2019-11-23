---
ms.assetid: c28c60ff-693d-49ee-a75b-58f24866217b
title: Requisiti per la risoluzione dei nomi per i proxy server federativi
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 51176101b471ec940e2b43a95e1a1a8d37b394f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408069"
---
# <a name="name-resolution-requirements-for-federation-server-proxies"></a>Requisiti per la risoluzione dei nomi per i proxy server federativi

Quando i computer client su Internet tentano di accedere a un'applicazione protetta da Active Directory Federation Services \(AD FS\), devono prima eseguire l'autenticazione al server federativo. Nella maggior parte dei casi, il server federativo non è in genere accessibile direttamente da Internet. Pertanto, i computer client Internet devono essere reindirizzati al proxy server federativo. È possibile eseguire il reindirizzamento con esito positivo aggiungendo il Domain Name System appropriato \(record di\) DNS alla zona o alle zone DNS che si trovano su Internet.  
  
Il metodo usato per reindirizzare i client Internet al proxy server federativo dipende da come si configura la zona DNS nella rete perimetrale o come si configura una zona DNS controllata in Internet. I proxy server federativi devono essere usati in una rete perimetrale. Reindirizzano le richieste client Internet ai server federativi solo quando DNS è stato configurato correttamente in tutte le zone con connessione Internet\-che si controllano. Pertanto, la configurazione di Internet\-le zone rivolte, sia che si disponga di una zona DNS che serve solo la rete perimetrale o di una zona DNS che serve sia la rete perimetrale che i client Internet, è importante.  
  
In questo argomento vengono descritti i passaggi da eseguire per configurare la risoluzione dei nomi quando si inserisce un proxy server federativo nella rete perimetrale. Per stabilire la procedura da seguire, determinare innanzitutto quale tra gli scenari DNS seguenti rispecchia meglio l'infrastruttura nella rete perimetrale dell'organizzazione. Quindi, seguire la procedura per lo scenario individuato.  
  
## <a name="dns-zone-serving-only-the-perimeter-network"></a>Zona DNS che serve solo la rete perimetrale  
In questo scenario l'organizzazione ha una o due zone DNS nella rete perimetrale e l'organizzazione non controlla le zone DNS in Internet. La corretta risoluzione dei nomi per un proxy server federativo nella zona DNS che serve solo lo scenario della rete perimetrale dipende dalle condizioni seguenti:  
  
-   Il proxy server federativo deve avere un'impostazione nel file hosts per risolvere il nome di dominio completo \(FQDN\) dell'URL dell'endpoint server federativo a un indirizzo IP di un server federativo o di un cluster di server federativo.  
  
-   Il DNS nella rete perimetrale del partner account deve essere configurato in modo che il nome di dominio completo dell'URL dell'endpoint server federativo venga risolto nell'indirizzo IP del proxy server federativo.  
  
L'illustrazione seguente e i passaggi corrispondenti mostrano in che modo vengono soddisfatte queste condizioni in uno specifico esempio. In questa illustrazione, il bilanciamento del carico di rete Microsoft \(NLB\) Technology fornisce un singolo FQDN del cluster e un singolo indirizzo IP del cluster per una Federazione server farm esistente.  
  
![requisiti del nome](media/adfs2_deploy_single_fs.gif)  
  
Per ulteriori informazioni sulla configurazione di un indirizzo IP del cluster o di un FQDN del cluster tramite NLB, vedere [specifica dei parametri del cluster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
### <a name="1-configure-the-hosts-file-on-the-federation-server-proxy"></a>1. Configurare il file Hosts nel proxy server federativo  
Poiché il DNS nella rete perimetrale è configurato per risolvere tutte le richieste per fs.fabrikam.com al proxy server federativo di account, il proxy server federativo del partner account ha una voce nel file hosts locale per risolvere fs.fabrikam.com nell'indirizzo IP del server federativo di account effettivo \(o il nome DNS del cluster per la Federazione server farm\) connesso alla rete aziendale. In questo modo, il proxy server federativo di account può risolvere il nome host fs.fabrikam.com nel server federativo di account anziché in se stesso, come si verificherebbe se tentasse di cercare fs.fabrikam.com con il DNS perimetrale, in modo che la Federazione il proxy server può comunicare con il server federativo.  
  
### <a name="2-configure-perimeter-dns"></a>2. Configurare il DNS perimetrale  
Poiché è presente un solo AD FS nome host a cui vengono indirizzati i computer client, sia che si trovino in una Intranet o in Internet, i computer client in Internet che usano il server DNS perimetrale devono risolvere il nome di dominio completo per il server federativo di account \(fs.fabrikam.com\) all'indirizzo IP del proxy server federativo di account nella rete perimetrale. In modo che possa inviare i client al proxy server federativo di account quando tentano di risolvere fs.fabrikam.com, il DNS perimetrale contiene una zona DNS corp.fabrikam.com limitata con un singolo host \(un record di risorse\) per FS \(fs.fabrikam.com\) e l'indirizzo IP del proxy server federativo di account nella rete perimetrale.  
  
Per ulteriori informazioni su come modificare il file hosts del proxy server federativo e configurare DNS nella rete perimetrale, vedere [configurare la risoluzione dei nomi per un proxy server federativo in una zona DNS che serve solo la rete perimetrale](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md).  
  
## <a name="dns-zone-serving-both-the-perimeter-network-and-internet-clients"></a>Zona DNS che serve sia la rete perimetrale che i client Internet  
In questo scenario l'organizzazione controlla la zona DNS nella rete perimetrale e almeno una zona DNS in Internet. La corretta risoluzione dei nomi per un proxy server federativo in questo scenario dipende dalle condizioni seguenti:  
  
-   Il DNS nella zona Internet del partner account deve essere configurato in modo che il nome di dominio completo del nome host del server federativo venga risolto nell'indirizzo IP del proxy server federativo nella rete perimetrale.  
  
-   Il DNS nella rete perimetrale del partner account deve essere configurato in modo che il nome di dominio completo del nome host del server federativo venga risolto nell'indirizzo IP del server federativo nella rete aziendale.  
  
L'illustrazione seguente e i passaggi corrispondenti mostrano in che modo vengono soddisfatte queste condizioni in uno specifico esempio.  
  
![requisiti del nome](media/adfs2_deploy_fsp_3DNS.gif)  
  
### <a name="1-configure-perimeter-dns"></a>1. Configurare il DNS perimetrale  
Per questo scenario, poiché si presuppone che si configuri la zona DNS Internet controllata per la risoluzione delle richieste effettuate per un URL di endpoint specifico \(, fs.fabrikam.com\) al proxy server federativo nella rete perimetrale, è necessario configurare anche la zona nel DNS perimetrale per l'inoltro delle richieste al server federativo nella rete aziendale.  
  
In modo che i client possano essere reindirizzati al server federativo di account quando tentano di risolvere fs.fabrikam.com, il DNS perimetrale è configurato con un singolo host \(un record di risorse\) per FS \(fs.fabrikam.com\) e l'indirizzo IP del server federativo di account nella rete aziendale. In questo modo, il proxy server federativo di account può risolvere il nome host fs.fabrikam.com nel server federativo di account anziché in se stesso, come si verificherebbe se tentasse di cercare fs.fabrikam.com con il DNS Internet, in modo che il server federativo il proxy può comunicare con il server federativo.  
  
### <a name="2-configure-internet-dns"></a>2. Configurare DNS Internet  
Per una corretta risoluzione dei nomi in questo scenario, tutte le richieste dei computer client in Internet a fs.fabrikam.com devono essere risolte dalla zona DNS Internet controllata dall'utente. Di conseguenza, è necessario configurare la zona DNS Internet in modo che inoltri le richieste client per fs.fabrikam.com all'indirizzo IP del proxy server federativo di account nella rete perimetrale.  
  
Per altre informazioni su come modificare la rete perimetrale e le zone DNS Internet, vedere [configurare la risoluzione dei nomi per un proxy server federativo in una zona DNS che serve sia la rete perimetrale che i client Internet](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md).  
  
## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
