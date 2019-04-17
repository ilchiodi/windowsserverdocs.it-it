---
ms.assetid: 026747c7-4c34-41c7-b7ea-27f9a7f64a35
title: Aggiungere un Record di risorse Host (A) al DNS aziendale per un Server federativo
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 425cfe794095f1515eb3fae2f1a5e5db90ba3d00
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Aggiungere un Record di risorse Host (A) al DNS aziendale per un Server federativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Per i client della sede centrale di rete di accedere correttamente un server federativo con l'autenticazione integrata di Windows, è necessario creare un record di risorse host \(A\) prima nel \(DNS\) Domain Name System aziendale che consente di risolvere il nome host del server federativo di account \ (ad esempio fs.fabrikam.com\) all'indirizzo IP del server federativo o del cluster di server federativo. È possibile utilizzare la procedura seguente per aggiungere un record di risorse host \(A\) al DNS aziendale per un server federativo.  
  
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Per aggiungere un record di risorse host \(A\) al DNS aziendale per un server federativo  
  
1.  In un server DNS per la rete aziendale, aprire il DNS snap-in.  
  
2.  Nella console di struttura ad albero, fare clic con la zona di ricerca diretta applicabile, quindi fare clic su **nuovo Host \(A or AAAA\)**.  
  
3.  In **nome**, digitare solo il nome del computer del server federativo o del cluster di server federativo. ad esempio, per il nome completo dominio nome \(FQDN\) fs.fabrikam.com, tipo **ADFS**.  
  
4.  In **indirizzo IP**, digitare l'indirizzo IP per il server federativo o cluster del server federativo, ad esempio, 192.168.1.4.  
  
5.  Fare clic su **aggiungere Host**.  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[Elenco di controllo: Configurazione di un Server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisiti di risoluzione dei nomi per i server federativi](https://technet.microsoft.com/library/dd807055.aspx)  
  

