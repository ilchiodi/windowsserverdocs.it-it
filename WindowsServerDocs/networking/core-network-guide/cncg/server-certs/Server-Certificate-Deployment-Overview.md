---
title: Panoramica della distribuzione del certificato server
description: Questo argomento fa parte della Guida alla distribuzione di un Server dei certificati per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: ca5c3e04-ae25-4590-97f3-0376a9c2a9a2
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0cafa4bdeb80b22d6bac4ad09bcae9436cda97c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877822"
---
# <a name="server-certificate-deployment-overview"></a>Panoramica della distribuzione del certificato server

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento sono incluse le sezioni seguenti.  
  
-   [Componenti della distribuzione certificato server](#bkmk_components)
  
-   [Panoramica del processo di distribuzione certificato server](#bkmk_process)
  
## <a name="bkmk_components"></a>Componenti della distribuzione certificato server
Per installare Servizi certificati Active Directory (AD CS) come autorità di certificazione radice (CA) dell'organizzazione e per registrare i certificati server per i server che eseguono servizio Strumentazione gestione Windows (NPS, Network Policy Server), Routing e accesso remoto (RRAS), o dei criteri di RETE e routing e accesso REMOTO, è possibile utilizzare questa Guida.


Se si distribuisce SDN con l'autenticazione basata su certificato, sono necessari server per utilizzare un certificato server per dimostrare la propria identità in altri server in modo che raggiungano le comunicazioni protette.
  
Nella figura seguente vengono illustrati i componenti necessari per distribuire i certificati server a server dell'infrastruttura SDN.
  
![Infrastruttura richiesta di distribuzione del certificato server](../../../media/Nps-Certs/Nps-Certs.jpg)  
  
> [!NOTE]  
> Nella figura precedente, sono rappresentati più server: DC1, CA1, WEB1 e numero di server di rete SDN. In questa guida vengono fornite istruzioni per la distribuzione e configurazione CA1 e WEB1, nonché per la configurazione di DC1, in questa guida si presuppone che è già stato installato nella rete. Se il dominio di Active Directory non è già installato, è possibile farlo mediante la [Guida alla rete Core](https://technet.microsoft.com/library/mt604042.aspx) per Windows Server 2016.  
  
Per ulteriori informazioni su ogni elemento illustrato nella figura sopra riportata, vedere quanto segue:  
  
-   [CA1](#bkmk_ca1)  
  
-   [WEB1](#bkmk_web1)  
  
-   [DC1](#bkmk_dc1)  
  
-   [NPS1](#bkmk_nps1)  
  
### <a name="bkmk_ca1"></a>CA1 esegue il ruolo server Servizi certificati Active Directory  
In questo scenario, l'autorità di certificazione (CA) principale dell'organizzazione è anche una CA emittente. L'autorità di certificazione emette certificati per i computer server che dispone delle autorizzazioni di sicurezza corrette per registrare un certificato. Servizi certificati Active Directory (AD CS) viene installato su CA1.  
  
Per reti di grandi dimensioni o in cui giustificare i problemi di sicurezza, è possibile separare i ruoli della CA radice e CA emittente e distribuire le CA subordinate sono CA emittenti.  
  
Nelle distribuzioni più sicure, l'autorità di certificazione principale dell'organizzazione viene portato offline e fisicamente protetta.   
  
#### <a name="capolicyinf"></a>File CAPolicy. inf  
Prima di installare Servizi certificati Active Directory, si configura il file CAPolicy. inf con impostazioni specifiche per la distribuzione.  
  
#### <a name="copy-of-the-ras-and-ias-servers-certificate-template"></a>Copia di **server RAS e IAS** modello di certificato  
Quando si distribuiscono i certificati del server, eseguire una copia di **server RAS e IAS** del modello di certificato e quindi configurare il modello in base alle proprie esigenze e le istruzioni riportate in questa Guida.   
  
Utilizzare una copia del modello anziché il modello originale in modo che la configurazione del modello originale viene mantenuta per possibili utilizzi futuri. Si configura la copia di **server RAS e IAS** modello in modo che l'autorità di certificazione possono creare i certificati del server che ha emesso i gruppi in Active Directory Users and Computers desiderati.  
  
#### <a name="additional-ca1-configuration"></a>Configurazione aggiuntiva CA1  
L'autorità di certificazione pubblica un elenco di revoche di certificati (CRL) che devono controllare i computer per garantire che i certificati che sono presentati nel come prova di identità sono certificati validi e non sono stati revocati. È necessario configurare l'autorità di certificazione con il percorso corretto dell'elenco CRL in modo che i computer sapere dove cercare il CRL durante il processo di autenticazione.  
  
### <a name="bkmk_web1"></a>Web1 esegue il ruolo server servizi Web (IIS)  
Nel computer che esegue il ruolo server Server Web (IIS), WEB1, è necessario creare una cartella in Esplora risorse per l'utilizzo come posizione per il CRL e AIA.  
  
#### <a name="virtual-directory-for-the-crl-and-aia"></a>Directory virtuale per il CRL e AIA  
Dopo aver creato una cartella in Esplora risorse, è necessario configurare la cartella come una directory virtuale in Gestione Internet Information Services (IIS), nonché configurare l'elenco di controllo di accesso per la directory virtuale per consentire ai computer di accedere ai AIA e CRL dopo la pubblicazione non esiste.  
  
### <a name="bkmk_dc1"></a>DC1 esegue i ruoli di server DNS e Active Directory Domain Services  
DC1 è il controller di dominio e il server DNS sulla rete.  
  
#### <a name="group-policy-default-domain-policy"></a>Criterio dominio predefinito dei criteri di gruppo  
Dopo aver configurato il modello di certificato della CA, è possibile configurare il criterio dominio predefinito in Criteri di gruppo in modo che i certificati sono registrazione automatica per i server dei criteri di RETE e RAS. Criteri di gruppo sono configurato in Active Directory nel server DC1.  
  
#### <a name="dns-alias-cname-resource-record"></a>DNS record di risorse alias (CNAME)  
È necessario creare un record di risorse alias (CNAME) per il server Web in modo da altri computer possono individuare il server, nonché di AIA e CRL vengono archiviati nel server. Inoltre, un record di risorse CNAME alias offre flessibilità in modo che è possibile utilizzare il server Web per altri scopi, ad esempio l'hosting di siti Web e FTP.  
  
### <a name="bkmk_nps1"></a>NPS1 in esecuzione il servizio ruolo Server dei criteri di rete del ruolo del server Servizi di accesso e criteri di rete  
I criteri di rete viene installato quando si eseguono le attività nella Guida alla rete Windows Server 2016 Core, pertanto prima di eseguire le attività in questa Guida, è necessario disporre già uno o più NPSs installati in rete.  
  
#### <a name="group-policy-applied-and-certificate-enrolled-to-servers"></a>Criteri di gruppo applicati e certificati registrati nei server  
Dopo aver configurato il modello di certificato e la registrazione automatica, è possibile aggiornare i criteri di gruppo su tutti i server di destinazione. A questo punto, il server di registrare il certificato server da CA1.  
  
### <a name="bkmk_process"></a>Panoramica del processo di distribuzione certificato server  
  
> [!NOTE]  
> Vengono forniti i dettagli di come eseguire questi passaggi nella sezione [distribuzione del certificato Server](../../../core-network-guide/cncg/server-certs/Server-Certificate-Deployment.md).  
  
Il processo di configurazione di registrazione del certificato server si verifica in tali fasi:  
  
1.  In WEB1, installare il ruolo Server Web (IIS).  
  
2.  Su DC1, creare un record alias (CNAME) per il server Web, WEB1.  
  
3.  Configurare il server Web per ospitare il CRL dalla CA, quindi pubblicare il CRL e copiare il certificato CA radice aziendale nella nuova directory virtuale.  
  
4.  Nel computer in cui si prevede di installare Servizi certificati Active Directory, assegnare un indirizzo IP statico nel computer, rinominare il computer, aggiungere il computer al dominio e quindi accedere al computer con un account utente che è un membro dei gruppi Enterprise Admins e Domain Admins.  
  
5.  Nel computer in cui si prevede di installare Servizi certificati Active Directory, configurare il file CAPolicy. inf con le impostazioni specifiche per la distribuzione.  
  
6.  Installare il ruolo server Servizi certificati Active Directory ed eseguire un'ulteriore configurazione dell'autorità di certificazione.  
  
7.  Copiare il certificato CA e CRL da CA1 alla condivisione sul server Web WEB1.  
  
8.  Nella CA configurare una copia del modello di certificato server RAS e IAS. I certificati basati su un modello di certificato, pertanto è necessario configurare il modello per il certificato del server prima che l'autorità di certificazione può emettere un certificato rilasciato dalla CA.  
  
9.  Configurare la registrazione automatica del certificato del server in Criteri di gruppo. Quando si configura la registrazione automatica, tutti i server che è stato specificato con l'appartenenza al gruppo Active Directory automaticamente visualizzato un certificato del server all'aggiornamento di criteri di gruppo in ogni server. Se si aggiungono altri server in un secondo momento, riceverà automaticamente un certificato server troppo.  
  
10. Aggiornare criteri di gruppo nel server. Quando viene aggiornato criteri di gruppo, i server ricevano il certificato del server che si basa sul modello che è stato configurato nel passaggio precedente. Questo certificato viene utilizzato dal server per dimostrare la propria identità ai computer client e altri server durante il processo di autenticazione.  
  
    > [!NOTE]  
    > Tutti i computer membro di dominio riceveranno automaticamente il certificato della CA principale dell'organizzazione senza la configurazione della registrazione automatica. Questo certificato è diverso da quello il certificato del server di configurare e distribuire mediante la registrazione automatica. Il certificato della CA viene installato automaticamente nell'archivio certificati Autorità di certificazione radice attendibili per tutti i computer membro di dominio in modo che i certificati emessi da questa autorità di certificazione verrà attendibilità.   
  
10. Verificare che tutti i server hanno registrato un certificato server valido.  
  


