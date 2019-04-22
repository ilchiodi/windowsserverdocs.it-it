---
title: Creare un record alias (CNAME) in DNS per WEB1
description: Questo argomento fa parte della Guida alla distribuzione di un Server dei certificati per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: bfae23f0-ae12-486b-94fe-50a137e141a5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 442ee8b70311eaad3f0b3f263003786b6beab8bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813162"
---
# <a name="create-an-alias-cname-record-in-dns-for-web1"></a>Creare un Alias \(CNAME\) Record in DNS per WEB1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per aggiungere un nome canonico di Alias \(CNAME\) record di risorse per il server Web a una zona DNS nel controller di dominio. Con i record CNAME, è possibile utilizzare più di un nome in modo che punti a un singolo host, rendendo più semplice svolgere attività quali l'host sia un File Transfer Protocol \(FTP\) server e un server Web nello stesso computer.   
  
Per questo motivo, si possono utilizzare il server Web per ospitare l'elenco certificati revocati \(CRL\) per autorità di certificazione \(CA\) nonché per eseguire servizi aggiuntivi, ad esempio server FTP o Web.  
  
Quando si esegue questa procedura, sostituire **nome Alias** e altre variabili con valori appropriati per la distribuzione.  
  
Per eseguire questa procedura, è necessario essere un membro di **Domain Admins**.  
  
## <a name="to-add-an-alias-cname-resource-record-to-a-zone"></a>Per aggiungere un alias \(CNAME\) record di risorse a una zona  
  
>[!NOTE]  
>Per eseguire questa procedura mediante Windows PowerShell, vedere [Aggiungi DnsServerResourceRecordCName](https://technet.microsoft.com/library/jj649894(v=wps.630).aspx).  
  
1.  Su DC1, in Server Manager fare clic su **strumenti** e quindi fare clic su **DNS**. Verrà visualizzata la finestra di DNS Manager Microsoft Management Console (MMC).  
  
2.  Nell'albero della console, fare doppio clic su **zone di ricerca**, pulsante destro del mouse sulla zona di ricerca diretta in cui si desidera aggiungere il record di risorse Alias, quindi fare clic su **Nuovo Alias \(CNAME\)**. Il **Nuovo Record di risorse** verrà visualizzata la finestra di dialogo.  
  
3.  In **nome Alias**, digitare il nome di alias **pki**.  
  
4.  Quando si digita un valore per **nome Alias**,  **nome di dominio completo \(FQDN\)** auto-riempimenti nella finestra di dialogo. Ad esempio, se il nome di alias è "pki" e il dominio è corp.contoso.com, il valore **pki.corp.contoso.com** viene automaticamente compilato automaticamente.  
  
5.  In **nome di dominio completo \(FQDN\) per host destinazione**, digitare il nome FQDN del server Web. Ad esempio, se il server Web è denominato WEB1 e il dominio è corp.contoso.com, digitare **WEB1.corp.contoso.com**.  
  
6.  Fare clic su **OK** per aggiungere il nuovo record per la zona.  
  

