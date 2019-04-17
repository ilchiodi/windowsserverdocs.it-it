---
title: Distribuire i certificati Server per 802.1 X cablate e Wireless distribuzioni
description: Questo argomento fa parte della Guida di certificati del Server di distribuzione per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 0a39ecae-39cc-4f26-bd6f-b71ed02fc4ad
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fda9b2b53d088cc389e98cf96fecd748419bc6e2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>Distribuire i certificati Server per 802.1 X cablate e Wireless distribuzioni

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questa guida per distribuire i certificati server per i server dell'infrastruttura di accesso remoto e Server dei criteri di rete (NPS).   

Questa guida contiene le sezioni seguenti.  

-   [Prerequisiti per l'utilizzo di questa Guida](#bkmk_pre)  

-   [Cosa non fornisce questa Guida](#bkmk_not)  

-   [Panoramiche delle tecnologie](#bkmk_tech)  

-   [Cenni preliminari sulla distribuzione di certificati server](Server-Certificate-Deployment-Overview.md)  

-   [Pianificazione della distribuzione dei certificati del server](Server-Certificate-Deployment-Planning.md)  

-   [Distribuzione del certificato server](Server-Certificate-Deployment.md)  

### **<a name="digital-server-certificates"></a>Certificati digitali server**  
Questa guida fornisce istruzioni per l'utilizzo di servizi certificati Active Directory (AD CS) per registrare automaticamente i certificati per l'accesso remoto e server dell'infrastruttura Criteri di rete. Servizi certificati Active Directory consente di creare un'infrastruttura a chiave pubblica (PKI) e fornire crittografia a chiave pubblica, certificati digitali e funzionalità di firma digitale per l'organizzazione.  

Quando si utilizzano certificati digitali server per l'autenticazione tra computer della rete, forniscano i certificati:   

1. Riservatezza tramite crittografia.  
2. Integrità tramite firme digitali.  
3. Autenticazione di associazione di chiavi dei certificati con un account computer, utente o dispositivo in una rete di computer.  

### **<a name="server-types"></a>Tipi di server**  
Utilizzando questa Guida, è possibile distribuire certificati server per i seguenti tipi di server.  
- Server che eseguono il servizio di accesso remoto, che sono DirectAccess o server standard a una rete privata virtuale (VPN) e che sono membri del **server RAS e IAS** gruppo.  
- Server che eseguono il servizio Server dei criteri di rete (NPS) che sono membri del **server RAS e IAS** gruppo.  

### **<a name="advantages-of-certificate-autoenrollment"></a>Vantaggi di registrazione automatica dei certificati**  
Registrazione automatica dei certificati del server, denominati anche la registrazione automatica, offre i vantaggi seguenti.  

- L'autorità di certificazione (CA) di servizi certificati Active Directory registra automaticamente un certificato server per tutti i server dei criteri di rete e accesso remoto.  
- Tutti i computer nel dominio ricevono automaticamente il certificato CA, che viene installato in Autorità di certificazione radice attendibili archiviare in ogni computer membro del dominio. Per questo motivo, tutti i computer nel dominio ritenere attendibili i certificati rilasciati dalla CA. Questo trust consente i server di autenticazione dimostrare la propria identità tra loro e partecipa a comunicazioni protette.  
- Diverso da aggiornare criteri di gruppo, la riconfigurazione manuale di tutti i server non è obbligatorio.  
- Ogni certificato del server include lo scopo di autenticazione Server e lo scopo di autenticazione Client nelle estensioni di utilizzo chiavi avanzato (EKU).  
- Scalabilità. Dopo aver distribuito la CA radice aziendale con questa Guida, è possibile espandere l'infrastruttura a chiave pubblica (PKI) mediante l'aggiunta di CA subordinata dell'organizzazione.  
- Facilità di gestione. È possibile gestire servizi certificati Active Directory utilizzando la console di servizi certificati Active Directory o tramite script e comandi di Windows PowerShell.  
- Semplicità. Specificare i server che registrano i certificati del server utilizzando gli account di gruppo di Active Directory e l'appartenenza al gruppo.   
- Quando si distribuiscono i certificati del server, i certificati sono basati su un modello configurato con le istruzioni riportate in questa Guida. Ciò significa che è possibile personalizzare modelli di certificato diversi per tipi di server specifici, oppure è possibile utilizzare lo stesso modello per tutti i certificati server che si desidera rilasciare.  

## <a name="bkmk_pre"></a>Prerequisiti per l'utilizzo di questa Guida  

Questa guida fornisce istruzioni su come distribuire i certificati server con Servizi certificati Active Directory e il ruolo server Server Web (IIS) in Windows Server 2016. Di seguito sono i prerequisiti per l'esecuzione delle procedure illustrate in questa Guida.  

- È necessario distribuire una rete core utilizzando la Guida alla rete di Windows Server 2016 Core oppure è necessario avere già tecnologie fornite nella Guida alla rete Core installato e funzioni correttamente nella rete. Queste tecnologie includono TCP/IP v4, DHCP, Active Directory Domain Services (AD DS), DNS e dei criteri di rete.  
>[!NOTE]
>Guida alla rete Windows Server 2016 Core è disponibile nella libreria tecnica di Windows Server 2016. Per ulteriori informazioni, vedere [Guida alla rete Core](../../../core-network-guide/Core-Network-Guide.md).

- È necessario leggere la sezione pianificazione di questa guida per assicurarsi che siano preparati per la distribuzione prima di eseguire la distribuzione.  
- È necessario eseguire i passaggi in questa Guida nell'ordine in cui sono presentate. Non passare e distribuire l'autorità di certificazione senza eseguire i passaggi che portano alla distribuzione del server o la distribuzione avrà esito negativo.  
- È necessario essere pronti a distribuire due nuovi server nella rete, un server su cui verrà installato Servizi certificati Active Directory come CA principale dell'organizzazione e un server su cui si installerà il Server Web (IIS) in modo che l'autorità di certificazione può pubblicare l'elenco di revoche di certificati (CRL) per il server Web.   

>[!NOTE]  
>Si è pronti assegnare un indirizzo IP statico per il server Web e servizi certificati Active Directory che si distribuisce in questa Guida, nonché nome di computer in base alle convenzioni di denominazione dell'organizzazione. Inoltre, è necessario aggiungere i computer al dominio.  

## <a name="bkmk_not"></a>Cosa non fornisce questa Guida  
Questa Guida non fornisce istruzioni dettagliate per la progettazione e distribuzione di un'infrastruttura a chiave pubblica (PKI) utilizzando Servizi certificati Active Directory. È consigliabile esaminare la documentazione di servizi certificati Active Directory e documentazione di progettazione dell'infrastruttura PKI prima di distribuire le tecnologie disponibili in questa Guida.   

## <a name="bkmk_tech"></a>Panoramiche delle tecnologie  
Di seguito sono panoramiche sulla tecnologia per Servizi certificati Active Directory e Server Web (IIS).  

### <a name="active-directory-certificate-services"></a>Servizi certificati Active Directory  
Servizi certificati Active Directory in Windows Server 2016 offre servizi personalizzabili per creare e gestire i certificati x. 509 che vengono utilizzati in sistemi di sicurezza software che si avvalgono di tecnologie a chiave pubblica. Le organizzazioni possono utilizzare Servizi certificati Active Directory per migliorare la sicurezza associando l'identità di una persona, un dispositivo o un servizio a una chiave pubblica corrispondente. Servizi certificati Active Directory include inoltre funzionalità che consentono di gestire la registrazione di certificati e le revoche in un'ampia gamma di ambienti scalabili.  

Per ulteriori informazioni, vedere [Panoramica di servizi di Active Directory certificato](https://technet.microsoft.com/library/hh831740.aspx) e [indicazioni di progettazione dell'infrastruttura chiave pubblica ](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx).  

### <a name="web-server-iis"></a>Server Web (IIS)  

Il ruolo Server Web (IIS) in Windows Server 2016 offre una piattaforma sicura, facile da gestire, modulare ed estensibile per l'hosting affidabile di siti Web, servizi e applicazioni. Con IIS, è possibile condividere informazioni con utenti su Internet, intranet o extranet. IIS è una piattaforma web unificata che integra IIS, ASP.NET, servizi FTP, PHP e Windows Communication Foundation (WCF).  

Per ulteriori informazioni, vedere [Panoramica di Server Web (IIS)](https://technet.microsoft.com/library/hh831725.aspx).  
