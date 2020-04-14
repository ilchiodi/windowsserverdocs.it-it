---
ms.assetid: 1a6740e6-5b6d-41f8-9ec4-32cdbee3e1bb
title: Risoluzione dei nomi per i client perimetrali e Internet
author: billmath
manager: femila
ms.date: 04/13/2020
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 060733c558433bfc94e37a4937bad43ea9cc9b37
ms.sourcegitcommit: 20d07170c7f3094c2fb4455f54b13ec4b102f2d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/13/2020
ms.locfileid: "81269422"
---
# <a name="name-resolution-for-perimeter-and-internet-clients"></a>Risoluzione dei nomi per i client perimetrali e Internet


In modo che la risoluzione dei nomi possa funzionare correttamente per un proxy server federativo in un Active Directory Federation Services \(AD FS\) scenario in cui una o più Domain Name System \(le zone\) DNS servono sia la rete perimetrale che i client Internet, è necessario completare le attività seguenti:  
  
-   Per risolvere che nome per il proxy server federativo host tutte le richieste client Internet per ADFS è necessario configurare DNS nella zona Internet che è possibile controllare. A tale scopo, si aggiunge un host \(A\) record di risorse per la zona DNS Internet per il proxy server federativo.  
  
-   Per risolvere che nome per il server federativo host tutte le richieste client in ingresso per ADFS, è necessario configurare DNS nella rete perimetrale. A tale scopo, si aggiunge un host \(A\) record di risorse per la zona DNS perimetrale per il proxy server federativo.  
  
> [!NOTE]  
> Si presuppone che un host \(A\) record di risorse è già stato creato il server federativo della sede centrale di rete DNS. Se il record non esiste ancora, creare il record e quindi eseguire queste procedure. Per ulteriori informazioni su come creare un host \(A\) record di risorse per il server federativo, vedere [aggiungere un Host & #40; A & #41; Record di risorse al DNS aziendale per un Server federativo](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Aggiungere un host \(A\) record di risorse per la zona DNS Internet per un proxy server federativo  
In modo che i computer client su Internet possono accedere un server federativo tramite un proxy server federativo appena distribuito, è innanzitutto necessario creare un host \(A\) record di risorse nella zona DNS Internet che è possibile controllare. Questo record di risorse consente di risolvere il nome host del server federativo di account \(ad esempio, fs.fabrikam.com\) all'indirizzo IP del proxy server federativo account \(ad esempio, 131.107.27.68\) nella rete perimetrale.  
  
> [!NOTE]  
> Si presuppone che si stia usando un server DNS che esegue Windows 2000 Server, Windows Server 2003 o Windows Server 2008 con il servizio server DNS per controllare la zona DNS Internet.  
  
Per poter completare questa procedura, è richiesta almeno l'appartenenza al gruppo **Administrators** o a un gruppo equivalente.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Per aggiungere un host \(A\) record di risorse per la zona DNS Internet per un proxy server federativo  
  
1.  In un server DNS per la zona DNS Internet, aprire lo snap-DNS\-in.  
  
2.  Nell'albero della console, a destra\-fare clic sulla zona di ricerca diretta applicabile e quindi fare clic su **Nuovo Host \(A o AAAA\)** .  
  
3.  In **nome**, digitare solo il nome del computer del server federativo. Ad esempio, per il nome di dominio completo \(FQDN\) fs.fabrikam.com, digitare **ADFS**.  
  
4.  In **indirizzo IP**, digitare l'indirizzo IP per il nuovo proxy server federativo, ad esempio, 131.107.27.68.  
  
5.  Fare clic su **Aggiungi host**.  
  
## <a name="add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Aggiungere un host \(A\) record di risorse per la zona DNS perimetrale per un proxy server federativo  
In modo che le richieste client Internet possono essere elaborate correttamente dai proxy server federativo e raggiungano il server federativo dopo che vengono risolte nella zona DNS Internet, è necessario creare un host \(A\) record di risorse nella zona DNS perimetrale. Questo record di risorse consente di risolvere il nome host del server federativo di account \(ad esempio, fs. Fabrikam.com\) all'indirizzo IP del server federativo di account \(ad esempio, 192.168.1.4\) nella rete aziendale.  
  
> [!NOTE]  
> Si presuppone che si stia usando un server DNS che esegue Windows 2000 Server, Windows Server 2003, Windows Server 2008 o Windows Server&reg; 2012 con il servizio server DNS per controllare la zona DNS perimetrale.  
  
Per poter completare questa procedura, è richiesta almeno l'appartenenza al gruppo **Administrators** o a un gruppo equivalente.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Per aggiungere un host \(A\) record di risorse per la zona DNS perimetrale per un proxy server federativo  
  
1.  In un server DNS per la rete perimetrale, aprire il **DNS blocca\-in**.  
  
2.  Nell'albero della console, a destra\-fare clic sulla zona di ricerca diretta applicabile e quindi fare clic su **Nuovo Host \(A o AAAA\)** .  
  
3.  In **nome**, digitare solo il nome del computer del server federativo. Ad esempio, per l'FQDN fs.fabrikam.com, digitare **fs**.  
  
4.  Nel **indirizzo IP** nella casella di testo digitare l'indirizzo IP di un indirizzo per il server federativo nella rete aziendale, ad esempio, 192.168.1.4.  
  
5.  Fare clic su **Aggiungi host**.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: configurazione di un proxy server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisiti per la risoluzione dei nomi per i proxy server federativi](https://technet.microsoft.com/library/dd807055.aspx)  
  

