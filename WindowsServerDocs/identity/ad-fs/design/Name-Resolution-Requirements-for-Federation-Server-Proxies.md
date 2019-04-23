---
ms.assetid: c28c60ff-693d-49ee-a75b-58f24866217b
title: Requisiti per la risoluzione dei nomi per i proxy server federativi
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a94e4de181cd8794d479bbd6695a94658aba0f86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855022"
---
# <a name="name-resolution-requirements-for-federation-server-proxies"></a>Requisiti per la risoluzione dei nomi per i proxy server federativi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando i computer client su Internet tentano di accedere a un'applicazione protetta da Active Directory Federation Services \(ADFS\), devono prima autenticarsi al server federativo. Nella maggior parte dei casi, il server federativo non è in genere accessibile direttamente da Internet. I computer client Internet, pertanto, devono essere reindirizzati al proxy server federativo invece. È possibile eseguire correttamente il reindirizzamento tramite l'aggiunta di Domain Name System appropriato \(DNS\) record per la zona DNS o zone con connessione Internet.  
  
Il metodo che usa per reindirizzare i client Internet per il proxy server federativo è dipende da come si configura la zona DNS nella rete perimetrale o come si configura una zona DNS che controllano in Internet. I proxy server federativi devono essere usati in una rete perimetrale. Reindirizza le richieste client Internet ai server federativi correttamente solo quando DNS è stato configurato correttamente in tutti i Internet\-rivolto verso aree di cui si controllano. Pertanto, la configurazione di rete Internet\-rivolta zone, ovvero se si dispone di una zona DNS che serve solo la rete perimetrale o una zona DNS che serve sia la rete perimetrale e dai client Internet, è importante.  
  
In questo argomento vengono descritti i passaggi che è possibile eseguire per configurare la risoluzione dei nomi quando si inserisce un proxy server federativo nella rete perimetrale. Per stabilire la procedura da seguire, determinare innanzitutto quale tra gli scenari DNS seguenti rispecchia meglio l'infrastruttura nella rete perimetrale dell'organizzazione. Quindi, seguire la procedura per lo scenario individuato.  
  
## <a name="dns-zone-serving-only-the-perimeter-network"></a>Zona DNS che serve solo la rete perimetrale  
In questo scenario l'organizzazione ha una o due zone DNS nella rete perimetrale e l'organizzazione non controlla le zone DNS in Internet. Risoluzione dei nomi corretta per un proxy server federativo nella zona DNS che serve solo lo scenario di rete perimetrale dipende dalle condizioni seguenti:  
  
-   Il proxy server federativo deve avere un'impostazione nel file hosts per risolvere il nome di dominio completo \(FQDN\) dell'URL dell'endpoint del server federativo a un indirizzo IP di un server federativo o un cluster del server federativo.  
  
-   DNS nella rete perimetrale del partner account deve essere configurato in modo che il nome di dominio completo dell'URL dell'endpoint del server federativo viene risolto nell'indirizzo IP del proxy server federativo.  
  
L'illustrazione seguente e i passaggi corrispondenti mostrano in che modo vengono soddisfatte queste condizioni in uno specifico esempio. In questa illustrazione, Bilanciamento carico di rete Microsoft \(NLB\) tecnologia fornisce un singolo, FQDN del cluster e un singolo indirizzo IP del cluster per una server farm federativa esistente.  
  
![requisiti dei nomi](media/adfs2_deploy_single_fs.gif)  
  
Per altre informazioni sulla configurazione di un indirizzo IP del cluster o un cluster FQDN utilizzando bilanciamento carico di rete, vedere [specificando i parametri del Cluster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
### <a name="1-configure-the-hosts-file-on-the-federation-server-proxy"></a>1. Configurare il file Hosts nel proxy server federativo  
DNS nella rete perimetrale è configurato per risolvere tutte le richieste per fs.fabrikam.com per il proxy server federativo di account, il proxy server federativo partner account ha una voce nel file hosts locale per risolvere fs.fabrikam.com nell'indirizzo IP del server federativo di account effettivi \(o nome DNS del cluster per la server farm federativa\) che è connesso alla rete aziendale. Questo rende possibile per il proxy server federativo di account risolvere il nome host fs.fabrikam.com con il server federativo di account non al suo interno, come avverrebbe se tentasse di cercare fs.fabrikam.com con DNS perimetrale, in modo che la federazione proxy server può comunicare con il server federativo.  
  
### <a name="2-configure-perimeter-dns"></a>2. Configurare il DNS perimetrale  
Essendo presente solo un singolo ADFS nome host che i computer client vengono indirizzati a, ovvero se sono in una rete intranet o su Internet, ovvero i computer client su Internet che utilizzano il server DNS perimetrale devono risolvere il FQDN per il server federativo di account \(fs.fabrikam.com\) all'indirizzo IP del proxy server federativo account nella rete perimetrale. In modo che è possibile inoltrare i client del proxy server federativo di account quando tentano di risolvere fs.fabrikam.com, il DNS perimetrale contiene una zona DNS corp.fabrikam.com limitata con un singolo host \(oggetto\) record di risorse per fs \(fs.fabrikam.com\) e l'indirizzo IP del proxy server federativo account nella rete perimetrale.  
  
Per altre informazioni su come modificare il file hosts di proxy server federativo e configurare il DNS nella rete perimetrale, vedere [configurare la risoluzione dei nomi per un Proxy Server federativo in una zona DNS che serve solo la rete perimetrale](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md).  
  
## <a name="dns-zone-serving-both-the-perimeter-network-and-internet-clients"></a>Zona DNS che serve sia la rete perimetrale che i client Internet  
In questo scenario l'organizzazione controlla la zona DNS nella rete perimetrale e almeno una zona DNS in Internet. Risoluzione dei nomi corretta per un proxy server federativo in questo scenario dipende dalle condizioni seguenti:  
  
-   DNS nella zona Internet del partner account deve essere configurato in modo che il nome FQDN del nome host del server di federazione viene risolto nell'indirizzo IP del proxy server federativo nella rete perimetrale.  
  
-   DNS nella rete perimetrale del partner account deve essere configurato in modo che il nome FQDN del nome host del server di federazione viene risolto nell'indirizzo IP del server federativo nella rete aziendale.  
  
L'illustrazione seguente e i passaggi corrispondenti mostrano in che modo vengono soddisfatte queste condizioni in uno specifico esempio.  
  
![requisiti dei nomi](media/adfs2_deploy_fsp_3DNS.gif)  
  
### <a name="1-configure-perimeter-dns"></a>1. Configurare il DNS perimetrale  
Per questo scenario, perché si presuppone che si configurerà la zona DNS Internet controllata per risolvere le richieste che vengono eseguite per uno specifico URL endpoint \(ossia, fs.fabrikam.com\) per il proxy server federativo di rete perimetrale, è necessario configurare la zona in DNS perimetrale per inoltrare queste richieste al server federativo nella rete aziendale.  
  
In modo che i client possono essere inoltrati al server federativo di account quando tentano di risolvere fs.fabrikam.com, DNS perimetrale è configurato con un singolo host \(un'\) record di risorse per fs \(fs.fabrikam.com\) e l'indirizzo IP del server federativo di account nella rete aziendale. Questo rende possibile per il proxy server federativo di account risolvere il nome host fs.fabrikam.com con il server federativo di account non al suo interno, come avverrebbe se tentasse di cercare fs.fabrikam.com con DNS Internet, in modo che il server federativo proxy può comunicare con il server federativo.  
  
### <a name="2-configure-internet-dns"></a>2. Configurare DNS Internet  
Per una corretta risoluzione dei nomi in questo scenario, tutte le richieste dei computer client in Internet a fs.fabrikam.com devono essere risolte dalla zona DNS Internet controllata dall'utente. Di conseguenza, è necessario configurare la zona DNS Internet per inoltrare le richieste client per fs.fabrikam.com all'indirizzo IP del proxy server federativo account nella rete perimetrale.  
  
Per altre informazioni su come modificare la rete perimetrale e le zone DNS Internet, vedere [configurare la risoluzione dei nomi per un Proxy Server federativo in un DNS zona che viene utilizzata sia la rete perimetrale e dai client Internet](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
