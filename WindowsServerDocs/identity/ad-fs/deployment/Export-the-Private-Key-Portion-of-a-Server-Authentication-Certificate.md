---
ms.assetid: cd4d4902-dcdf-49dd-8059-82a56bf4b585
title: Esportare la parte di chiave privata di un certificato di autenticazione server
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1a7e59dd83ebc9a9eabd5bda1dc598d320f5028d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442505"
---
# <a name="export-the-private-key-portion-of-a-server-authentication-certificate"></a>Esportare la parte di chiave privata di un certificato di autenticazione server

Ogni server federativo in una Active Directory Federation Services \(ADFS\) farm deve avere accesso alla chiave privata del certificato di autenticazione server. Se si implementa una server farm di server federativi o server Web, è necessario disporre di un singolo certificato di autenticazione. Questo certificato deve essere emesso da un'autorità di certificazione \(CA\), e deve avere una chiave privata esportabile. La chiave privata del certificato di autenticazione server deve essere esportabile in modo che possa essere resa disponibile a tutti i server della farm.  
  
Lo stesso concetto vale farm proxy server federativo nel senso che tutti i server federativi in una farm devono condividere la parte di chiave privata del certificato di autenticazione server stesso.  
  
> [!NOTE]  
> Lo snap di gestione di AD FS\-in si riferisce ai certificati di autenticazione server per i server federativi come certificati per le comunicazioni del servizio.  
  
A seconda di quale ruolo svolgerà questo computer, utilizzare questa procedura nel computer server federativo o computer proxy server federativo in cui è installato il certificato di autenticazione server con la chiave privata. Al termine della procedura, è quindi possibile importare il certificato nel sito Web predefinito di ogni server della farm. Per altre informazioni, vedere [importare un certificato di autenticazione Server nel sito Web predefinito](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-export-the-private-key-portion-of-a-server-authentication-certificate"></a>Per esportare la parte di chiave privata di un certificato di autenticazione server  
  
1. Nel **avviare** digitare**Internet Information Services \(IIS\) Manager**, quindi premere INVIO.  
  
2. Nell'albero della console fare clic su **Nome computer**.  
  
3. Nel riquadro centrale fare doppio\-fare clic su **certificati Server**.  
  
4. Nel riquadro centrale destra\-fare clic sul certificato che si desidera esportare e quindi fare clic su **esportare**.  
  
5. Nel **Esporta certificato** finestra di dialogo, fare clic su di **...** .  
  
6. Nelle **nome File**, digitare **c:\\** <em>Nomecertificato</em>e quindi fare clic su **Open**.  
  
7. Immettere una password per il certificato, confermarla e quindi fare clic su **OK**.  
  
8. Convalidare l'esito positivo dell'esportazione verificando che il file specificato è stato creato nel percorso indicato.  
  
   > [!IMPORTANT]  
   > Per poter importare questo certificato nell'archivio certificati locali del nuovo server, è necessario trasferire il file nel supporto fisico e proteggerne la sicurezza durante il trasporto. È estremamente importante proteggere la sicurezza della chiave privata. Se questa chiave viene compromessa, la sicurezza dell'intera distribuzione ADFS \(tra cui le risorse all'interno dell'organizzazione e delle organizzazioni partner risorse\) è compromesso.  
  
9. Importare il certificato di autenticazione server esportato nell'archivio certificati del nuovo server prima di installare il Servizio federativo. Per informazioni su come importare il certificato, vedere importare un certificato del Server \( [http:\/\/go.microsoft.com\/fwlink\/? LinkId\=108283](https://go.microsoft.com/fwlink/?LinkId=108283)\).  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Elenco di controllo: Configurazione di un proxy server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Requisiti dei certificati per i proxy server federativi](https://technet.microsoft.com/library/dd807054.aspx)  
  

