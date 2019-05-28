---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: Configurare DNS aziendale per il servizio federativo e il servizio DRS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cd8febf9eff300b1a83d22828874b4a577b8af36
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192324"
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>Configurare DNS aziendale per il servizio federativo e il servizio DRS
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>Passaggio 6: Aggiunge un Host \(un'\) e un Alias \(CNAME\) Record di risorse al DNS aziendale per il servizio federativo e il servizio DRS  
È necessario aggiungere i seguenti record di risorse al DNS aziendale \(DNS\) per il servizio federativo e Device Registration Service configurati nei passaggi precedenti.  
  
|Voce|Type|Address|  
|---------|--------|-----------|  
|federation\_service\_name|Host \(A\)|Indirizzo IP del server AD FS o l'indirizzo IP del servizio di bilanciamento del carico configurato davanti la server farm AD FS|  
|enterpriseregistration|Alias \(CNAME\)|federation\_server\_name.contoso.com|  
  
È possibile usare la procedura seguente per aggiungere un host \(un'\) e un alias \(CNAME\) i record di risorse al DNS aziendale per il server federativo e il servizio registrazione dispositivo.  
  
L'appartenenza al **gli amministratori**, o equivalente è il requisito minimo per completare questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Per aggiungere un host \(un'\) e un alias \(CNAME\) record di risorse DNS per il server federativo  
  
1.  In è controller di dominio, in Server Manager nel **degli strumenti** dal menu fare clic su **DNS** per aprire lo snap-DNS\-in.  
  
2.  Nell'albero della console, espandere la **domain\_controller\_nome** nodo, espandere **zone di ricerca diretta**, a destra\-fare clic su **dominio\_name**, quindi fare clic su **nuovo Host \(A o AAAA\)** .  
  
3.  Nel **nome** , digitare il nome da utilizzare per la farm AD FS.  
  
4.  Nella casella **Indirizzo IP**, digita l'indirizzo IP del tuo server federativo. Fare clic su **Aggiungi host**.  
  
5.  A destra\-fare clic sul **domain\_name** nodo e quindi fare clic su **nuovo Alias \(CNAME\)** .  
  
6.  Nella finestra di dialogo **Nuovo record di risorse** digitare **enterpriseregistration** nella casella **Nome alias**.  
  
7.  Il nome di dominio completo \(FQDN\) della finestra host di destinazione, digitare **federation\_servizio\_farm\_nome\_. nome dominio.com**e quindi Fare clic su **OK**.  
  
    > [!IMPORTANT]  
    > In una distribuzione reale, se la società ha più User Principal Name \(UPN\) suffissi, è necessario creare più record CNAME per ognuno dei suffissi UPN nel DNS.  
  
## <a name="see-also"></a>Vedere anche 

[Distribuzione di AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una server farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

