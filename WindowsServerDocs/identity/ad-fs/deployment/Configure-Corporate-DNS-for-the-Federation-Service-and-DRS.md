---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: Configurare il DNS aziendale per il servizio federativo e DRS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9b66bed99cbc2ac2cdf116579adaea282c45fabe
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>Configurare il DNS aziendale per il servizio federativo e DRS

>Si applica a: Windows Server 2016, Windows Server 2012 R2
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>Passaggio 6: Aggiungere un Host \(A\) e Alias \(CNAME\) Record di risorse DNS aziendale per il servizio federativo e DRS  
È necessario aggiungere i seguenti record di risorse aziendali \(DNS\) Domain Name System per il servizio federativo e il servizio DRS configurato nella procedura precedente.  
  
|Voce|Tipo|Indirizzo|  
|---------|--------|-----------|  
|federation\_service\_name|Host \(A\)|Indirizzo IP del server AD FS o l'indirizzo IP del bilanciamento del carico configurato davanti la server farm AD FS|  
|enterpriseregistration|Alias \(CNAME\)|federation\_server\_name.contoso.com|  
  
È possibile utilizzare la procedura seguente per aggiungere un host \(A\) e alias \(CNAME\) record di risorse al DNS aziendale per il server federativo e il servizio Registrazione dispositivi.  
  
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Per aggiungere un host \(A\) e alias \(CNAME\) record di risorse DNS per il server federativo  
  
1.  In è controller di dominio, in Server Manager, nel **strumenti** menu, fare clic su **DNS** per aprire DNS snap-in.  
  
2.  Nell'albero della console, espandere il **domain\_controller\_name** nodo, espandere **zone di ricerca diretta**, fare clic con **domain\_name**, quindi fare clic su **nuovo Host \(A or AAAA\)**.  
  
3.  Nel **nome**, digitare il nome da utilizzare per la farm ADFS.  
  
4.  Nel **indirizzo IP**, digitare l'indirizzo IP del server federativo. Fare clic su **aggiungere Host**.  
  
5.  Fare clic con il **domain\_name** nodo, quindi fare clic su **nuovo Alias \(CNAME\)**.  
  
6.  Nel **nuovo Record di risorse** la finestra di dialogo, digitare **enterpriseregistration** nel **nome Alias** casella.  
  
7.  Nel nome di dominio completo nome \(FQDN\) della casella di host di destinazione, tipo **federation\_service\_farm\_name.domain\_name.com**, quindi fare clic su **OK**.  
  
    > [!IMPORTANT]  
    > In una distribuzione reale, se la società ha più suffissi di nome dell'entità utente \(UPN\), è necessario creare più record CNAME per ognuno dei suffissi UPN nel DNS.  
  
## <a name="see-also"></a>Vedere anche 

[Distribuzione di ADFS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD ADFS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una Server Farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

