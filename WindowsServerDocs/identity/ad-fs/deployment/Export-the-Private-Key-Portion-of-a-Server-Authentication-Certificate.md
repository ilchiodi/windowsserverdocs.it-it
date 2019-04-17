---
ms.assetid: cd4d4902-dcdf-49dd-8059-82a56bf4b585
title: Esportare la parte di chiave privata di un certificato di autenticazione Server
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c968f0702d56b56d0a80459e5cf0c9e658c56741
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="export-the-private-key-portion-of-a-server-authentication-certificate"></a>Esportare la parte di chiave privata di un certificato di autenticazione Server

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ogni server federativo in una farm \(AD FS\) Active Directory Federation Services deve avere accesso alla chiave privata del certificato di autenticazione server. Se si implementa una server farm di server federativi o server Web, è necessario disporre di un certificato di autenticazione singolo. Questo certificato deve essere emesso da un'autorità di certificazione \(CA\) e deve avere una chiave privata esportabile. La chiave privata del certificato di autenticazione server deve essere esportabile in modo che possa essere resa disponibile a tutti i server della farm.  
  
Lo stesso concetto vale della farm di proxy server federativo nel senso che tutti i proxy server federativi in una farm devono condividere la parte di chiave privata del certificato di autenticazione server stesso.  
  
> [!NOTE]  
> Snap-in di gestione di ADFS fa riferimento ai certificati di autenticazione server per i server federativi come certificati per le comunicazioni del servizio.  
  
A seconda del ruolo di cui verrà riprodotto questo computer, utilizzare questa procedura nel computer server federativo o computer proxy server federativo in cui è installato il certificato di autenticazione server con la chiave privata. Una volta completata la procedura, è quindi possibile importare questo certificato al sito Web predefinito di ogni server della farm. Per ulteriori informazioni, vedere [importare un certificato di autenticazione Server per il sito Web predefinito](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-export-the-private-key-portion-of-a-server-authentication-certificate"></a>Per esportare la chiave privata di un certificato di autenticazione server  
  
1.  Nel **Start** digitare**\(IIS\) Internet Information Services Manager**, quindi premere INVIO.  
  
2.  Nell'albero della console, fare clic su **ComputerName**.  
  
3.  Nel riquadro centrale fare doppio clic **i certificati Server**.  
  
4.  Nel riquadro centrale, fare clic con il certificato che si desidera esportare, quindi fare clic su **esportare**.  
  
5.  Nel **Esporta certificato** la finestra di dialogo, fare clic su di **...** pulsante.  
  
6.  In **nome File**, tipo **C:\\***Nomecertificato*, quindi fare clic su **aprire**.  
  
7.  Digitare una password per il certificato, confermarla e quindi fare clic su **OK**.  
  
8.  Convalidare l'esito positivo dell'esportazione verificando che il file specificato viene creato nel percorso specificato.  
  
    > [!IMPORTANT]  
    > In modo che questo certificato può essere importato nell'archivio certificati locale nel nuovo server, è necessario trasferire il file di supporto fisico e proteggerne la sicurezza durante il trasporto al nuovo server. È estremamente importante proteggere la sicurezza della chiave privata. Se questa chiave viene compromessa la sicurezza dell'intera distribuzione AD FS \ (incluse le risorse all'interno dell'organizzazione e in organizations\ partner risorse) viene compromesso.  
  
9. Importare il certificato di autenticazione server esportato nell'archivio certificati nel nuovo server prima di installare il servizio federativo. Per informazioni su come importare il certificato, vedere importare un certificato del Server \ ([http:///\/go.microsoft.com\/fwlink\/? LinkId\ = 108283](https://go.microsoft.com/fwlink/?LinkId=108283)\).  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[Elenco di controllo: Configurazione di un Server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Elenco di controllo: Configurazione di un Proxy Server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisiti dei certificati per i server federativi](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Requisiti dei certificati per i proxy Server federativi](https://technet.microsoft.com/library/dd807054.aspx)  
  

