---
ms.assetid: cd4d4902-dcdf-49dd-8059-82a56bf4b585
title: Esportare la parte di chiave privata di un certificato di autenticazione server
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8e1bbeddc4bae1c420b6cc78b52d6b873320ae8f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359582"
---
# <a name="export-the-private-key-portion-of-a-server-authentication-certificate"></a>Esportare la parte di chiave privata di un certificato di autenticazione server

Ogni server federativo in una farm Active Directory Federation Services \(AD FS @ no__t-1 deve avere accesso alla chiave privata del certificato di autenticazione server. Se si sta implementando un server farm di server federativi o server Web, è necessario disporre di un singolo certificato di autenticazione. Questo certificato deve essere emesso da un'autorità di certificazione globale (Enterprise) \(CA @ no__t-1 e deve avere una chiave privata esportabile. La chiave privata del certificato di autenticazione server deve essere esportabile in modo che possa essere resa disponibile a tutti i server della farm.  
  
Questo stesso concetto è vero per le farm proxy server federative nel senso che tutti i proxy server federativi in una farm devono condividere la parte della chiave privata dello stesso certificato di autenticazione server.  
  
> [!NOTE]  
> Lo snap-in di gestione AD FS @ no__t-0cm fa riferimento ai certificati di autenticazione server per i server federativi come certificati di comunicazione del servizio.  
  
A seconda del ruolo che verrà riprodotto da questo computer, usare questa procedura nel computer server federativo o nel computer proxy server federativo in cui è stato installato il certificato di autenticazione server con la chiave privata. Al termine della procedura, è quindi possibile importare il certificato nel sito Web predefinito di ogni server della farm. Per ulteriori informazioni, vedere [importare un certificato di autenticazione server nel sito Web predefinito](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-export-the-private-key-portion-of-a-server-authentication-certificate"></a>Per esportare la parte di chiave privata di un certificato di autenticazione server  
  
1. Nella schermata **Start** digitare**Internet Information Services \(IIS @ No__t-3 Manager**, quindi premere INVIO.  
  
2. Nell'albero della console fare clic su **Nome computer**.  
  
3. Nel riquadro centrale, Double @ no__t-0click **Server Certificates**.  
  
4. Nel riquadro centrale, fare clic con il pulsante destro del mouse su no__t-0click sul certificato che si desidera esportare, quindi fare clic su **Esporta**.  
  
5. Nella finestra di dialogo **Esporta certificato** fare clic su **...** .  
  
6. In **nome file**Digitare **C: \\** <em>nomecertificato</em>, quindi fare clic su **Apri**.  
  
7. Immettere una password per il certificato, confermarla e quindi fare clic su **OK**.  
  
8. Convalidare l'esito positivo dell'esportazione verificando che il file specificato è stato creato nel percorso indicato.  
  
   > [!IMPORTANT]  
   > Per poter importare questo certificato nell'archivio certificati locali del nuovo server, è necessario trasferire il file nel supporto fisico e proteggerne la sicurezza durante il trasporto. È estremamente importante proteggere la sicurezza della chiave privata. Se questa chiave viene compromessa, la protezione dell'intera distribuzione AD FS @no__t le risorse 0including all'interno dell'organizzazione e nelle organizzazioni partner risorse @ no__t-1 è compromessa.  
  
9. Importare il certificato di autenticazione server esportato nell'archivio certificati del nuovo server prima di installare il Servizio federativo. Per informazioni su come importare il certificato, vedere Importare un certificato server \([http: @no__t -2\/go.microsoft.com @ no__t-4fwlink @ no__t-5? LinkId @ no__t-6108283](https://go.microsoft.com/fwlink/?LinkId=108283)\).  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Elenco di controllo: Configurazione di un proxy server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Requisiti dei certificati per i proxy server federativi](https://technet.microsoft.com/library/dd807054.aspx)  
  

