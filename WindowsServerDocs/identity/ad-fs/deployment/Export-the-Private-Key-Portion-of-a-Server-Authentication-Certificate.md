---
ms.assetid: cd4d4902-dcdf-49dd-8059-82a56bf4b585
title: Esportare la parte di chiave privata di un certificato di autenticazione server
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6baa734e3fc346d94f4387e2ed54d3e707e5af75
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855424"
---
# <a name="export-the-private-key-portion-of-a-server-authentication-certificate"></a>Esportare la parte di chiave privata di un certificato di autenticazione server

Ogni server federativo in un Active Directory Federation Services \(AD FS Farm\) deve avere accesso alla chiave privata del certificato di autenticazione server. Se si sta implementando un server farm di server federativi o server Web, è necessario disporre di un singolo certificato di autenticazione. Questo certificato deve essere emesso da un'autorità di certificazione dell'organizzazione (Enterprise) \(CA\)e deve avere una chiave privata esportabile. La chiave privata del certificato di autenticazione server deve essere esportabile in modo che possa essere resa disponibile a tutti i server della farm.  
  
Questo stesso concetto è vero per le farm proxy server federative nel senso che tutti i proxy server federativi in una farm devono condividere la parte della chiave privata dello stesso certificato di autenticazione server.  
  
> [!NOTE]  
> Lo snap\-di gestione AD FS in fa riferimento ai certificati di autenticazione server per i server federativi come certificati di comunicazione del servizio.  
  
A seconda del ruolo che verrà riprodotto da questo computer, usare questa procedura nel computer server federativo o nel computer proxy server federativo in cui è stato installato il certificato di autenticazione server con la chiave privata. Al termine della procedura, è quindi possibile importare il certificato nel sito Web predefinito di ogni server della farm. Per ulteriori informazioni, vedere [importare un certificato di autenticazione server nel sito Web predefinito](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
L'appartenenza al gruppo **Administrators**, o a un gruppo equivalente, nel computer locale è il requisito minimo per eseguire questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-export-the-private-key-portion-of-a-server-authentication-certificate"></a>Per esportare la parte di chiave privata di un certificato di autenticazione server  
  
1. Nella schermata **Start** digitare**Internet Information Services \(IIS\) Manager**, quindi premere INVIO.  
  
2. Nell'albero della console fare clic su **NomeComputer**.  
  
3. Nel riquadro centrale, fare doppio\-fare clic su **certificati server**.  
  
4. Nel riquadro centrale fare clic con il pulsante destro del mouse\-fare clic sul certificato che si desidera esportare, quindi fare clic su **Esporta**.  
  
5. Nella finestra di dialogo **Esporta certificato** fare clic su **...** .  
  
6. In **nome file**Digitare **C:\\** <em>nomecertificato</em>, quindi fare clic su **Apri**.  
  
7. Immettere una password per il certificato, confermarla e quindi fare clic su **OK**.  
  
8. Convalidare l'esito positivo dell'esportazione verificando che il file specificato è stato creato nel percorso indicato.  
  
   > [!IMPORTANT]  
   > Per poter importare questo certificato nell'archivio certificati locali del nuovo server, è necessario trasferire il file nel supporto fisico e proteggerne la sicurezza durante il trasporto. È estremamente importante proteggere la sicurezza della chiave privata. Se questa chiave viene compromessa, la protezione dell'intera distribuzione di AD FS \(incluse le risorse all'interno dell'organizzazione e nelle organizzazioni partner risorse\) è compromessa.  
  
9. Importare il certificato di autenticazione server esportato nell'archivio certificati del nuovo server prima di installare il Servizio federativo. Per informazioni su come importare il certificato, vedere Importare un certificato del server \([http:\/\/go.microsoft.com\/fwlink\/? LinkId\=108283](https://go.microsoft.com/fwlink/?LinkId=108283)\).  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Elenco di controllo: configurazione di un proxy server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Requisiti dei certificati per i proxy server federativi](https://technet.microsoft.com/library/dd807054.aspx)  
  

