---
ms.assetid: 026747c7-4c34-41c7-b7ea-27f9a7f64a35
title: Aggiungere un record di risorse host (A) al DNS aziendale per un server federativo
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 47d619803133a29bd0217b738577c93522f1ab59
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815024"
---
# <a name="add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Aggiungere un record di risorse host (A) al DNS aziendale per un server federativo



Per consentire ai client della rete aziendale di accedere correttamente a un server federativo utilizzando l'autenticazione integrata di Windows, è necessario creare un host \(un record di risorse di\) nella Domain Name System aziendale \(DNS\) che risolve il nome host del server federativo di account \(ad esempio, fs.fabrikam.com\) all'indirizzo IP del server federativo o del cluster di server federativo. È possibile utilizzare la procedura seguente per aggiungere un host \(un\) record di risorse al DNS aziendale per un server federativo.  
  
Per poter completare questa procedura, è richiesta almeno l'appartenenza al gruppo **Administrators** o a un gruppo equivalente.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Per aggiungere un host \(un\) record di risorse al DNS aziendale per un server federativo  
  
1.  In un server DNS per la rete aziendale, aprire lo snap\-DNS in.  
  
2.  Nell'albero della console, a destra\-fare clic sulla zona di ricerca diretta applicabile e quindi fare clic su **Nuovo Host \(A o AAAA\)** .  
  
3.  In **nome**Digitare solo il nome computer del server federativo o del cluster di server federativo. ad esempio, per il nome di dominio completo \(FQDN\) fs.fabrikam.com, digitare **FS**.  
  
4.  In **indirizzo IP**, digitare l'indirizzo IP del server federativo o del cluster di server federativo, ad esempio 192.168.1.4.  
  
5.  Fare clic su **Aggiungi host**.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisiti per la risoluzione dei nomi per i server federativi](https://technet.microsoft.com/library/dd807055.aspx)  
  

