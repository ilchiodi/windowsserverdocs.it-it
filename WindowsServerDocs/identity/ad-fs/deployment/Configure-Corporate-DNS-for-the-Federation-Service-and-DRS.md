---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: Configurare DNS aziendale per il servizio federativo e il servizio DRS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3e3f2b36b7949e4bbde78942006e985f41abf9df
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814265"
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>Configurare DNS aziendale per il servizio federativo e il servizio DRS
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>Passaggio 6: aggiungere un host \(un\) e un alias \(CNAME\) record di risorse al DNS aziendale per le Servizio federativo e DRS  
È necessario aggiungere i record di risorse seguenti ai Domain Name System aziendali \(\) DNS per il servizio federativo e il servizio di registrazione dei dispositivi configurati nei passaggi precedenti.  
  
|Voce|Type|Address|  
|---------|--------|-----------|  
|nome\_del servizio\_di Federazione|Host \(un\)|Indirizzo IP del server AD FS o indirizzo IP del servizio di bilanciamento del carico configurato davanti al AD FS server farm|  
|enterpriseregistration|Alias \(CNAME\)|Federazione\_server\_name.contoso.com|  
  
È possibile utilizzare la procedura seguente per aggiungere un host \(un\) e alias \(CNAME\) record di risorse al DNS aziendale per il server federativo e il servizio registrazione dispositivo.  
  
L'appartenenza al gruppo **Administrators**o a un gruppo equivalente è il requisito minimo per completare questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Per aggiungere un host \(un\) e alias \(CNAME\) record di risorse a DNS per il server federativo  
  
1.  Nel controller di dominio, in Server Manager, nel menu **strumenti** fare clic su **DNS** per aprire lo snap-in DNS\-in.  
  
2.  Nell'albero della console espandere il nodo **dominio\_controller\_nome** , espandere **zone di ricerca diretta**, fare clic con il pulsante\-destro del mouse su **nome\_dominio**, quindi fare clic su **nuovo host \(A o AAAA\)** .  
  
3.  Nella casella **nome** Digitare il nome da utilizzare per la farm ad FS.  
  
4.  Nella casella **Indirizzo IP**, digita l'indirizzo IP del tuo server federativo. Fare clic su **Aggiungi host**.  
  
5.  Fare clic con il pulsante\-destro del mouse sul nodo **\_nome dominio** , quindi fare clic su **Nuovo Alias \(CNAME\)** .  
  
6.  Nella finestra di dialogo **Nuovo record di risorse** digitare **enterpriseregistration** nella casella **Nome alias**.  
  
7.  Nel nome di dominio completo \(FQDN\) della casella host di destinazione, digitare **federation\_service\_farm\_nome. domain\_Name.com**, quindi fare clic su **OK**.  
  
    > [!IMPORTANT]  
    > In una distribuzione reale, se l'azienda dispone di più nomi dell'entità utente \(suffissi UPN\), è necessario creare più record CNAME per ognuno di questi suffissi UPN in DNS.  
  
## <a name="see-also"></a>Vedi anche 

[Distribuzione di AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una server farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

