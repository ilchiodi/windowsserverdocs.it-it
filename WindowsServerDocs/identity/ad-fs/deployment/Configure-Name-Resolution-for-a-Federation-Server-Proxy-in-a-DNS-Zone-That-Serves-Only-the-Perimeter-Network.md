---
ms.assetid: b7109e46-b66e-4c5c-8b87-a6611d68415a
title: Configurare la risoluzione dei nomi per un Proxy Server federativo in una zona DNS che serve solo la rete perimetrale
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 32b8e3cc133ce95872881115608bb8cfb17b2427
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-only-the-perimeter-network"></a>Configurare la risoluzione dei nomi per un Proxy Server federativo in una zona DNS che serve solo la rete perimetrale

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In modo che la risoluzione dei nomi può funzionare correttamente per un server federativo in uno scenario \(AD FS\) Active Directory Federation Services in cui uno o più zone di Domain Name System \(DNS\) servono solo la rete perimetrale, è necessario completare le attività seguenti:  
  
-   Il file hosts nel proxy server federativo deve essere aggiornato per aggiungere l'indirizzo IP di un server federativo.  
  
-   Per risolvere che nome per il proxy server federativo host tutte le richieste client per AD FS, è necessario configurare DNS nella rete perimetrale. A tale scopo, aggiungere un record di risorse host \(A\) al DNS perimetrale per il proxy server federativo.  
  
> [!NOTE]  
> Queste procedure presuppongono che un record di risorse host \(A\) per il server federativo è già stato creato nel DNS della rete aziendale. Se il record non esiste ancora, creare il record e quindi eseguire queste procedure. Per ulteriori informazioni su come creare il record di risorse host \(A\) per il server federativo, vedere [aggiungere un Host & #40; A & #41; Record di risorse al DNS aziendale per un Server federativo](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Aggiungere l'indirizzo IP di un server federativo al file degli host  
In modo che un proxy server federativo possa funzionare come previsto nella rete perimetrale del partner account, è necessario aggiungere una voce al file degli host su tale proxy server federativo che punta al nome host DNS del server federativo \ (ad esempio fs.fabrikam.com\) e l'indirizzo IP \ (ad esempio 192.168.1.4\) nella rete aziendale del partner account. Aggiunta di questa voce al file degli host impedisce che il proxy server federativo contattare per risolvere una chiamata avviate da connessione tra client a un server federativo nel partner account.  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Per aggiungere l'indirizzo IP di un server federativo al file degli host  
  
1.  Passare alla cartella directory %systemroot%\\Winnt\\System32\\Drivers e individuare il **host** file.  
  
2.  Avviare Blocco note e quindi aprire il **host** file.  
  
3.  Aggiungere l'indirizzo IP e il nome host di un server federativo nel partner account per il **host** file, come illustrato nell'esempio seguente:  
  
    **192.168.1.4fs.fabrikam.com**  
  
4.  Salvare e chiudere il file.  
  
## <a name="add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Aggiungere un record di risorse host \(A\) al DNS perimetrale per un proxy server federativo  
In modo che i client su Internet possono accedere un server federativo tramite un proxy server federativo appena distribuito, è innanzitutto necessario creare un record di risorse host \(A\) nel DNS perimetrale. Questo record di risorse consente di risolvere il nome host del server federativo di account \ (ad esempio fs.fabrikam.com\) all'indirizzo IP del proxy server federativo account \ (ad esempio 131.107.27.68\) nella rete perimetrale.  
  
> [!NOTE]  
> Si presuppone che si utilizza un server DNS, in esecuzione Windows 2000 Server, Windows Server 2003 o Windows Server 2008 con il servizio Server DNS, per controllare la zona DNS perimetrale.  
  
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Per aggiungere un record di risorse host \(A\) DNS perimetrale per un proxy server federativo  
  
1.  In un server DNS per la rete perimetrale, aprire il DNS snap-in. Fare clic su **Start**, scegliere **strumenti di amministrazione**, quindi fare clic su **DNS**.  
  
2.  Nella console di struttura ad albero, fare clic con la zona di ricerca diretta applicabile, quindi fare clic su **nuovo Host \(A or AAAA\)**.  
  
3.  In **nome**, digitare solo il nome del computer del server federativo. Ad esempio, per il nome completo dominio nome \(FQDN\) fs.fabrikam.com, tipo **ADFS**.  
  
4.  In **indirizzo IP**, digitare l'indirizzo IP per il nuovo proxy server federativo, ad esempio, **131.107.27.68**.  
  
5.  Fare clic su **aggiungere Host**.  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[Elenco di controllo: Configurazione di un Proxy Server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisiti di risoluzione dei nomi per i proxy Server federativi](https://technet.microsoft.com/library/dd807055.aspx)  
  

