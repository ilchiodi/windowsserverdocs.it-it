---
title: Creare un Record Alias (CNAME) in DNS per WEB1
description: Questo argomento fa parte della Guida di certificati del Server di distribuzione per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: bfae23f0-ae12-486b-94fe-50a137e141a5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 65f10efe1cfd2866fb99406bf031197f6268b05b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="create-an-alias-cname-record-in-dns-for-web1"></a>Creare un Record di Alias \(CNAME\) in DNS per WEB1

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questa procedura per aggiungere un record di risorse \(CNAME\) nome canonico di Alias per il server Web a una zona DNS nel controller di dominio. Con i record CNAME, è possibile utilizzare più di un nome per puntare a un singolo host, rendendo più semplice svolgere attività quali l'host sia un server File Transfer Protocol \(FTP\) e un server Web nello stesso computer.   
  
Per questo motivo, si possono utilizzare il server Web per ospitare la revoca del certificato elencati \(CRL\) per l'autorità di certificazione \(CA\) anche per eseguire servizi aggiuntivi, ad esempio server FTP o Web.  
  
Quando si esegue questa procedura, sostituire **nome Alias** e altre variabili con valori appropriati per la distribuzione.  
  
Per eseguire questa procedura, è necessario essere un membro del **Domain Admins**.  
  
## <a name="to-add-an-alias-cname-resource-record-to-a-zone"></a>Per aggiungere un record di risorse alias \(CNAME\) a una zona  
  
>[!NOTE]  
>Per eseguire questa procedura mediante Windows PowerShell, vedere [Aggiungi DnsServerResourceRecordCName](https://technet.microsoft.com/library/jj649894(v=wps.630).aspx).  
  
1.  Su DC1, in Server Manager fare clic su **strumenti** e quindi fare clic su **DNS**. Verrà visualizzata la finestra di DNS Manager Microsoft Management Console (MMC).  
  
2.  Nell'albero della console, fare doppio clic su **zone di ricerca diretta**, fare clic sulla zona di ricerca diretta in cui si desidera aggiungere il record di risorse Alias, quindi fare clic su **nuovo Alias \(CNAME\)**. Il **nuovo Record di risorse** apre la finestra di dialogo.  
  
3.  In **nome Alias**, digitare il nome di alias **pki**.  
  
4.  Quando si digita un valore per **nome Alias**, **nome di dominio completo \(FQDN\)** auto-riempimenti nella finestra di dialogo. Ad esempio, se il nome di alias è "pki" e il dominio è corp.contoso.com, il valore **pki.corp.contoso.com** viene compilata automaticamente per te.  
  
5.  In **\(FQDN\) nome di dominio completo per host destinazione**, digitare il nome FQDN del server Web. Ad esempio, se il server Web è denominato WEB1 e il dominio è corp.contoso.com, digitare **WEB1.corp.contoso.com**.  
  
6.  Fare clic su **OK** per aggiungere il nuovo record per la zona.  
  

