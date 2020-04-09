---
ms.assetid: b7109e46-b66e-4c5c-8b87-a6611d68415a
title: Configurare la risoluzione dei nomi per un proxy server federativo in una zona DNS che serve solo la rete perimetrale
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 451ed2bb2b2da9481d33c6e9e339bb582824a4e1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854924"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-only-the-perimeter-network"></a>Configurare la risoluzione dei nomi per un proxy server federativo in una zona DNS che serve solo la rete perimetrale


In modo che la risoluzione dei nomi possa funzionare correttamente per un server federativo in un Active Directory Federation Services \(AD FS\) scenario in cui una o più Domain Name System \(le zone DNS\) servono solo la rete perimetrale, è necessario completare le attività seguenti:  
  
-   File degli host proxy server federativo deve essere aggiornato per aggiungere l'indirizzo IP di un server federativo.  
  
-   Per risolvere che nome per il proxy server federativo host tutte le richieste client per AD FS, è necessario configurare DNS nella rete perimetrale. A tale scopo, si aggiunge un host \(A\) record di risorse al DNS perimetrale per il proxy server federativo.  
  
> [!NOTE]  
> Queste procedure presuppongono che un host \(A\) record di risorse è già stato creato il server federativo della sede centrale di rete DNS. Se il record non esiste ancora, creare il record e quindi eseguire queste procedure. Per ulteriori informazioni su come creare l'host \(A\) record di risorse per il server federativo, vedere [aggiungere un Host & #40; A & #41; Record di risorse al DNS aziendale per un Server federativo](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Aggiungere l'indirizzo IP di un server federativo al file degli host  
In modo che un proxy server federativo possa funzionare come previsto nella rete perimetrale di un partner account, è necessario aggiungere una voce al file degli host su tale proxy server federativo che punta al nome host DNS del server federativo \(ad esempio, fs.fabrikam.com\) e l'indirizzo IP \(ad esempio, 192.168.1.4\) nella rete aziendale del partner account. Aggiunta di questa voce al file degli host, si impedisce che il proxy server federativo contattare per risolvere un client\-chiamata a un server federativo nel partner account.  
  
L'appartenenza al gruppo **Administrators**, o a un gruppo equivalente, nel computer locale è il requisito minimo per eseguire questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Per aggiungere l'indirizzo IP di un server federativo al file degli host  
  
1.  Passare a % systemroot %\\Winnt\\System32\\cartella directory di driver e individuare il **host** file.  
  
2.  Avviare il Blocco note, quindi aprire il file degli **host**.  
  
3.  Aggiungere l'indirizzo IP e il nome host di un server federativo nel partner account per il **host** file, come illustrato nell'esempio seguente:  
  
    **192.168.1.4fs.fabrikam.com**  
  
4.  Salvare e chiudere il file.  
  
## <a name="add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Aggiungere un host \(A\) record di risorse al DNS perimetrale per un proxy server federativo  
In modo che i client su Internet possono accedere un server federativo tramite un proxy server federativo appena distribuito, è innanzitutto necessario creare un host \(A\) record di risorse DNS perimetrale. Questo record di risorse consente di risolvere il nome host del server federativo di account \(ad esempio, fs.fabrikam.com\) all'indirizzo IP del proxy server federativo account \(ad esempio, 131.107.27.68\) nella rete perimetrale.  
  
> [!NOTE]  
> Si presuppone che si stia usando un server DNS, che esegue Windows 2000 Server, Windows Server 2003 o Windows Server 2008 con il servizio server DNS, per controllare la zona DNS perimetrale.  
  
Per poter completare questa procedura, è richiesta almeno l'appartenenza al gruppo **Administrators** o a un gruppo equivalente.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Per aggiungere un host \(A\) record di risorse al DNS perimetrale per un proxy server federativo  
  
1.  In un server DNS per la rete perimetrale, aprire lo snap-DNS\-in. Fare clic su **avviare**, scegliere **Strumenti di amministrazione**, quindi fare clic su **DNS**.  
  
2.  Nell'albero della console, a destra\-fare clic sulla zona di ricerca diretta applicabile e quindi fare clic su **Nuovo Host \(A o AAAA\)** .  
  
3.  In **nome**, digitare solo il nome del computer del server federativo. Ad esempio, per il nome di dominio completo \(FQDN\) fs.fabrikam.com, digitare **ADFS**.  
  
4.  In **indirizzo IP**, digitare l'indirizzo IP per il nuovo server federativo, ad esempio, **131.107.27.68**.  
  
5.  Fare clic su **Aggiungi host**.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: configurazione di un proxy server federativo](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisiti per la risoluzione dei nomi per i proxy server federativi](https://technet.microsoft.com/library/dd807055.aspx)  
  

