---
ms.assetid: 1a6740e6-5b6d-41f8-9ec4-32cdbee3e1bb
title: Configurare la risoluzione dei nomi per un Proxy Server federativo in un DNS zona utilizzati entrambi il perimetro di rete e i client Internet
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 3b154459163b2142ff1d3aba424a86305d093de4
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-both-the-perimeter-network-and-internet-clients"></a>Configurare la risoluzione dei nomi per un Proxy Server federativo in un DNS zona utilizzati entrambi il perimetro di rete e i client Internet

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In modo che la risoluzione dei nomi può funzionare correttamente per un proxy server federativo in uno scenario \(AD FS\) Active Directory Federation Services in cui uno o più zone di Domain Name System \(DNS\) servono sia la rete perimetrale e dai client Internet, è necessario completare le attività seguenti:  
  
-   Per risolvere che nome per il proxy server federativo host tutte le richieste client Internet per ADFS, è necessario configurare DNS nella zona Internet che è possibile controllare. A tale scopo, aggiungere un record di risorse host \(A\) per la zona DNS Internet per il proxy server federativo.  
  
-   Per risolvere che nome per il server federativo host tutte le richieste client in ingresso per ADFS, è necessario configurare DNS nella rete perimetrale. A tale scopo, aggiungere un record di risorse host \(A\) per la zona DNS perimetrale per il proxy server federativo.  
  
> [!NOTE]  
> Si presuppone che un record di risorse host \(A\) per il server federativo è già stato creato nel DNS della rete aziendale. Se il record non esiste ancora, creare il record e quindi eseguire queste procedure. Per ulteriori informazioni su come creare un record di risorse host \(A\) per il server federativo, vedere [aggiungere un Host & #40; A & #41; Record di risorse al DNS aziendale per un Server federativo](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Aggiungere un record di risorse host \(A\) per la zona DNS Internet per un proxy server federativo  
In modo che i computer client su Internet possono accedere un server federativo tramite un proxy server federativo appena distribuito, è innanzitutto necessario creare un record di risorse host \(A\) nella zona DNS Internet che è possibile controllare. Questo record di risorse consente di risolvere il nome host del server federativo di account \ (ad esempio fs.fabrikam.com\) all'indirizzo IP del proxy server federativo account \ (ad esempio 131.107.27.68\) nella rete perimetrale.  
  
> [!NOTE]  
> Si presuppone che si utilizza un server DNS che esegue Windows 2000 Server, Windows Server 2003 o Windows Server 2008 con il servizio Server DNS per controllare la zona DNS Internet.  
  
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e ](http://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Per aggiungere un record di risorse host \(A\) la zona DNS Internet per un proxy server federativo  
  
1.  In un server DNS per la zona DNS Internet, aprire il DNS snap-in.  
  
2.  Nella console di struttura ad albero, fare clic con la zona di ricerca diretta applicabile, quindi fare clic su **nuovo Host \(A or AAAA\)**.  
  
3.  In **nome**, digitare solo il nome del computer del server federativo. Ad esempio, per il nome completo dominio nome \(FQDN\) fs.fabrikam.com, tipo **ADFS**.  
  
4.  In **indirizzo IP**, digitare l'indirizzo IP per il nuovo proxy server federativo, ad esempio, 131.107.27.68.  
  
5.  Fare clic su **aggiungere Host**.  
  
## <a name="add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Aggiungere un record di risorse host \(A\) per la zona DNS perimetrale per un proxy server federativo  
In modo che le richieste client Internet possono essere elaborate correttamente dai proxy server federativo e raggiungano il server federativo dopo che vengono risolte nella zona DNS Internet, è necessario creare un record di risorse host \(A\) nella zona DNS perimetrale. Questo record di risorse consente di risolvere il nome host del server federativo di account \ (ad esempio, fs. fabrikam.com\) per l'indirizzo IP del server federativo di account \ (ad esempio 192.168.1.4\) nella rete aziendale.  
  
> [!NOTE]  
> Si presuppone che si utilizza un server DNS che esegue Windows 2000 Server, Windows Server 2003, Windows Server 2008 o Windows Server® 2012 con il servizio Server DNS per controllare la zona DNS perimetrale.  
  
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e ](http://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Per aggiungere un record di risorse host \(A\) la zona DNS perimetrale per un proxy server federativo  
  
1.  In un server DNS per la rete perimetrale, aprire il **snap-in DNS**.  
  
2.  Nella console di struttura ad albero, fare clic con la zona di ricerca diretta applicabile, quindi fare clic su **nuovo Host \(A or AAAA\)**.  
  
3.  In **nome**, digitare solo il nome del computer del server federativo. Ad esempio, per il nome di dominio completo fs.fabrikam.com, digitare **ADFS**.  
  
4.  Nel **indirizzo IP** nella casella di testo digitare l'IP per il server federativo nella rete aziendale, ad esempio indirizzo, 192.168.1.4.  
  
5.  Fare clic su **aggiungere Host**.  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[Elenco di controllo: Configurazione di un Proxy Server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisiti di risoluzione dei nomi per i proxy Server federativi](https://technet.microsoft.com/library/dd807055.aspx)  
  

