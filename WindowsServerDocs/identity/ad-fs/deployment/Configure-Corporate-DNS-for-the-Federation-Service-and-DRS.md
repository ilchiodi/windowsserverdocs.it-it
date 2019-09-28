---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: Configurare DNS aziendale per il servizio federativo e il servizio DRS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9f0b04f9dc050117fdefc630759c86d2b1bb1ecc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408441"
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>Configurare DNS aziendale per il servizio federativo e il servizio DRS
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>Passaggio 6: Aggiungere un host \(A @ no__t-1 e alias \(CNAME @ no__t-3 record di risorse al DNS aziendale per il Servizio federativo e DRS  
È necessario aggiungere i record di risorse seguenti a Corporate Domain Name System \(DNS @ no__t-1 per il servizio federativo e il servizio di registrazione dei dispositivi configurati nei passaggi precedenti.  
  
|Voce|Type|Address|  
|---------|--------|-----------|  
|Federation @ no__t-0service @ no__t-1Name|Host \(A @ no__t-1|Indirizzo IP del server AD FS o indirizzo IP del servizio di bilanciamento del carico configurato davanti al AD FS server farm|  
|enterpriseregistration|Alias \(CNAME @ no__t-1|Federazione @ no__t-0server\_name.contoso.com|  
  
È possibile utilizzare la procedura seguente per aggiungere un host \(A @ no__t-1 e alias \(CNAME @ no__t-3 record di risorse al DNS aziendale per il server federativo e il servizio registrazione dispositivo.  
  
L'appartenenza al gruppo **Administrators**o a un gruppo equivalente è il requisito minimo per completare questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Per aggiungere un host \(A @ no__t-1 e alias \(CNAME @ no__t-3 record di risorse a DNS per il server federativo  
  
1.  Nel controller di dominio, in Server Manager, nel menu **strumenti** fare clic su **DNS** per aprire lo snap-in DNS @ no__t-2in.  
  
2.  Nell'albero della console espandere il nodo **Domain @ no__t-1Controller @ no__t-2name** , espandere **zone di ricerca diretta**, right @ no__t-4click **Domain @ no__t-6name**, quindi fare clic su **new host \(a o AAAA @ no__t-9**.  
  
3.  Nella casella **nome** Digitare il nome da utilizzare per la farm ad FS.  
  
4.  Nella casella **Indirizzo IP**, digita l'indirizzo IP del tuo server federativo. Fare clic su **Aggiungi host**.  
  
5.  Right @ no__t-0click il nodo **Domain @ no__t-2name** e quindi fare clic su **new alias \(CNAME @ no__t-5**.  
  
6.  Nella finestra di dialogo **Nuovo record di risorse** digitare **enterpriseregistration** nella casella **Nome alias**.  
  
7.  Nel nome di dominio completo \(FQDN @ no__t-1 della casella host di destinazione digitare **Federation @ no__t-3service @ no__t-4farm @ no__t-5name. Domain @ no__t-6name. com**, quindi fare clic su **OK**.  
  
    > [!IMPORTANT]  
    > In una distribuzione reale, se l'azienda ha più nomi dell'entità utente \(UPN @ no__t-1 suffissi, è necessario creare più record CNAME per ognuno di questi suffissi UPN in DNS.  
  
## <a name="see-also"></a>Vedere anche 

[Distribuzione di AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una server farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

