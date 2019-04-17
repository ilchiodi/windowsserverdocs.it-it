---
title: Pianificazione della distribuzione dei certificati del server
description: Questo argomento fa parte della Guida di certificati del Server di distribuzione per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 7eb746e0-1046-4123-b532-77d5683ded44
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: eacfa404da91d14a7a7328646c2320be8220000c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="server-certificate-deployment-planning"></a>Pianificazione della distribuzione dei certificati del server

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Prima di distribuire i certificati del server, è necessario pianificare gli elementi seguenti:  
  
-   [Pianificare la configurazione di base del server](#bkmk_basic)  
  
-   [Pianificare l'accesso al dominio](#bkmk_domain)  
  
-   [Pianificare il percorso e nome della directory virtuale sul server Web](#bkmk_virtual)  
  
-   [Pianificare un record alias (CNAME) DNS per il server Web](#bkmk_cname)  
  
-   [Pianificare la configurazione del file CAPolicy. inf](#bkmk_capolicy)  
  
-   [Pianificare la configurazione delle estensioni CDP e AIA su CA1](#bkmk_cdp)  
  
-   [Pianificare l'operazione di copia tra l'autorità di certificazione e il server Web](#bkmk_copy)  
  
-   [Pianificare la configurazione del modello di certificato server nell'autorità di certificazione](#bkmk_template)  
  
## <a name="bkmk_basic"></a>Pianificare la configurazione di base del server  
Dopo aver installato Windows Server 2016 nei computer che si prevede di utilizzare come autorità di certificazione e server Web, è necessario rinominare il computer e assegnare e configurare un indirizzo IP statico per il computer locale.  
  
Per ulteriori informazioni, vedere Windows Server 2016 [Guida alla rete Core](../../../core-network-guide/Core-Network-Guide.md).  
  
## <a name="bkmk_domain"></a>Pianificare l'accesso al dominio  
Per accedere al dominio, il computer deve essere un computer membro del dominio e l'account utente deve essere creato in servizi di dominio Active Directory prima di tentare l'accesso. Inoltre, la maggior parte delle procedure descritte in questa guida richiedono che l'account utente sia un membro dei gruppi Enterprise Admins o Domain Admins in Active Directory Users and Computers, pertanto è necessario accedere all'autorità di certificazione con un account con l'appartenenza al gruppo appropriato.  
  
Per ulteriori informazioni, vedere Windows Server 2016 [Guida alla rete Core](../../../core-network-guide/Core-Network-Guide.md).  
  
## <a name="bkmk_virtual"></a>Pianificare il percorso e nome della directory virtuale sul server Web  
Per fornire l'accesso a CRL e il certificato CA da altri computer, è necessario archiviare questi elementi in una directory virtuale sul server Web. In questa Guida, la directory virtuale si trova sul server Web WEB1. In questa cartella sull'unità "C" ed è denominata "pki". È possibile individuare la directory virtuale sul server Web in qualsiasi percorso della cartella che è appropriato per la distribuzione.  
  
## <a name="bkmk_cname"></a>Pianificare un record alias (CNAME) DNS per il server Web  
Record di risorse alias (CNAME) vengono talvolta definite anche record di risorse nome canonico. Con questi record, è possibile utilizzare più di un nome per puntare a un singolo host, rendendo più semplice svolgere attività quali l'host sia un server FTP File Transfer Protocol () e un server Web nello stesso computer. Ad esempio, i nomi di server conosciuti (ftp, www) vengono registrati utilizzando record di risorse alias (CNAME) che eseguono il mapping al sistema DNS (Domain Name) nome host, ad esempio WEB1 per il computer server che ospita i servizi.  
  
Questa guida fornisce istruzioni per la configurazione del server Web per ospitare l'elenco di revoche di certificati (CRL) per l'autorità di certificazione (CA). Poiché si potrebbe inoltre desidera utilizzare il server Web per altri scopi, ad esempio per ospitare un sito Web o FTP, è consigliabile creare un record di risorse alias in DNS per il server Web. In questa Guida, il record CNAME è denominato "pki", ma è possibile scegliere un nome appropriato per la distribuzione.  
  
## <a name="bkmk_capolicy"></a>Pianificare la configurazione del file CAPolicy. inf  
Prima di installare Servizi certificati Active Directory, è necessario configurare file CAPolicy. inf nell'autorità di certificazione con informazioni corrette per la distribuzione. Un file CAPolicy. inf contiene le informazioni seguenti:  
  
```  
[Version]  
Signature="$Windows NT$"  
[PolicyStatementExtension]  
Policies=InternalPolicy  
[InternalPolicy]  
OID=1.2.3.4.1455.67.89.5  
Notice="Legal Policy Statement"  
URL=http://pki.corp.contoso.com/pki/cps.txt  
[Certsrv_Server]  
RenewalKeyLength=2048  
RenewalValidityPeriod=Years  
RenewalValidityPeriodUnits=5  
CRLPeriod=weeks  
CRLPeriodUnits=1  
LoadDefaultTemplates=0  
AlternateSignatureAlgorithm=1  
```  
È necessario pianificare i seguenti elementi per questo file:  
  
-   **URL**. Il file CAPolicy.inf di esempio ha un valore URL **http://pki.corp.contoso.com/pki/cps.txt**. Questo avviene perché il server Web in questa guida è denominato WEB1 e dispone di un record di risorse DNS CNAME dell'infrastruttura pki. Il server Web viene inoltre aggiunto al dominio corp.contoso.com. Inoltre, non esiste una directory virtuale sul server Web denominato "pki" in cui è archiviato l'elenco certificati revocati. Verificare che il valore fornito dall'utente per l'URL nei punti di file CAPolicy. inf in una directory virtuale sul server Web nel dominio.  
  
-   **RenewalKeyLength**. La lunghezza della chiave rinnovo predefinito per Servizi certificati Active Directory in Windows Server 2012 è 2048. La lunghezza della chiave selezionato deve avere una lunghezza possibile pur fornendo la compatibilità con le applicazioni che si desidera utilizzare.  
  
-   **RenewalValidityPeriodUnits**. Il file CAPolicy. inf di esempio ha un valore di RenewalValidityPeriodUnits di 5 anni. Questo accade perché la durata prevista dell'autorità di certificazione è circa dieci anni. Il valore di RenewalValidityPeriodUnits deve riflettere il periodo di validità generale della CA o il maggior numero di anni per cui si desidera fornire la registrazione.  
  
-   **CRLPeriodUnits**. Il file CAPolicy. inf di esempio ha un valore CRLPeriodUnits 1. Questo accade perché l'intervallo di aggiornamento di esempio per l'elenco certificati revocati in questa guida è 1 settimana. Il valore di intervallo che specifica questa impostazione, è necessario pubblicare il CRL della CA per la directory virtuale del server Web in cui archiviare il CRL e fornire l'accesso a esso per i computer inclusi nel processo di autenticazione.  
  
-   **AlternateSignatureAlgorithm**. Questo file CAPolicy. inf implementa un meccanismo di sicurezza migliorata mediante l'implementazione di formati di firma alternativo. È consigliabile non implementare questa impostazione se si dispone ancora di client Windows XP che richiedono certificati da questa autorità di certificazione.  
  
Se non si intenda sull'aggiunta di tutte le CA subordinate per l'infrastruttura a chiave pubblica in un secondo momento, e se si desidera impedire l'aggiunta di tutte le CA subordinate, è possibile aggiungere la chiave PathLength per il file CAPolicy. inf con un valore pari a 0. Per aggiungere questa chiave, copiare e incollare il codice seguente nel file:  
  
```  
[BasicConstraintsExtension]  
PathLength=0  
Critical=Yes  
```  
  
> [!IMPORTANT]  
> Non è consigliabile che modificare le altre impostazioni nel file CAPolicy. inf a meno che non hai un motivo specifico per questa operazione.  
  
## <a name="bkmk_cdp"></a>Pianificare la configurazione delle estensioni CDP e AIA su CA1  
Quando si configura il punto di distribuzione elenco di revoche di certificati (CRL) (CDP) e le impostazioni di accesso alle informazioni di autorità (AIA) su CA1, è necessario il nome del server Web e il nome di dominio. È inoltre necessario il nome della directory virtuale creata nel server Web in cui sono archiviati l'elenco di revoche di certificati (CRL) e il certificato dell'autorità di certificazione.  
  
Il percorso CDP che è necessario immettere durante questo passaggio di distribuzione ha il formato:  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl.`  
      
Ad esempio, se il server Web è denominato WEB1 e il DNS alias record CNAME per il server Web è "pki", il dominio è corp.contoso.com e la directory virtuale viene denominata pki, il percorso CDP:  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`  
      
Il percorso di AIA che è necessario immettere ha il formato:  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<ServerDNSName>\_<CaName><CertificateName>.crt.`  
      
Ad esempio, se il server Web è denominato WEB1 e il DNS alias record CNAME per il server Web è "pki", il dominio è corp.contoso.com e la directory virtuale viene denominata pki, è il percorso di AIA:  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`  
      
## <a name="bkmk_copy"></a>Pianificare l'operazione di copia tra l'autorità di certificazione e il server Web  
Per pubblicare il certificato CA e CRL dalla CA per la directory virtuale del server Web, è possibile eseguire il comando di certutil - crl dopo aver configurato i percorsi CDP e AIA nella CA. Assicurarsi di configurare i percorsi corretti sulle proprietà CA **estensioni** scheda prima di eseguire il comando utilizzando le istruzioni riportate in questa Guida. Inoltre, per copiare il certificato CA dell'organizzazione per il server Web, è necessario avere già creato la directory virtuale sul server Web e configurata la cartella come una cartella condivisa.  
  
## <a name="bkmk_template"></a>Pianificare la configurazione del modello di certificato server nell'autorità di certificazione  
Per distribuire per i certificati server, è necessario copiare il modello di certificato denominato **Server RAS e IAS**. Per impostazione predefinita, questa copia è denominata **copia di Server RAS e IAS**. Se si desidera rinominare questa copia del modello, pianificare il nome che si desidera utilizzare durante il passaggio di distribuzione.  
  
> [!NOTE]  
> Le sezioni distribuzione ultime tre in questa Guida, che consentono di configurare la registrazione automatica del certificato server, aggiornare i criteri di gruppo nei server e verificare che i server hanno ricevuto un certificato server valido dalla CA - non richiedono ulteriori passaggi di pianificazione.  
  


