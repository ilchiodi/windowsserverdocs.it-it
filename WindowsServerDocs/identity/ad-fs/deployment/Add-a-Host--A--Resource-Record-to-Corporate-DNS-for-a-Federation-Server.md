---
ms.assetid: 026747c7-4c34-41c7-b7ea-27f9a7f64a35
title: Aggiungere un record di risorse host (A) al DNS aziendale per un server federativo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 425cfe794095f1515eb3fae2f1a5e5db90ba3d00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856182"
---
# <a name="add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Aggiungere un record di risorse host (A) al DNS aziendale per un server federativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Per i client su della sede centrale di rete di accedere correttamente a un server federativo utilizzando l'autenticazione integrata di Windows, un host \(un'\) record di risorse deve essere creato prima di tutto in Domain Name System aziendali \(DNS\) che risolve il nome host del server federativo di account \(ad esempio, fs.fabrikam.com\) all'indirizzo IP del server federativo o del cluster di server federativo. È possibile usare la procedura seguente per aggiungere un host \(oggetto\) record di risorse al DNS aziendale per un server federativo.  
  
L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Per aggiungere un host \(oggetto\) record di risorse al DNS aziendale per un server federativo  
  
1.  In un server DNS per la rete aziendale, aprire lo snap-DNS\-in.  
  
2.  Nell'albero della console, a destra\-fare clic sulla zona di ricerca diretta applicabile e quindi fare clic su **Nuovo Host \(A o AAAA\)**.  
  
3.  Nelle **Name**, digitare il nome del computer del server federativo o del cluster di server federativo, ad esempio, per il nome di dominio completo \(FQDN\) fs.fabrikam.com, digitare **fs**.  
  
4.  Nelle **indirizzo IP**, digitare l'indirizzo IP per il server federativo o un cluster del server federativo, ad esempio, 192.168.1.4.  
  
5.  Fare clic su **Aggiungi host**.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un Server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisiti di risoluzione dei nomi per i server federativi](https://technet.microsoft.com/library/dd807055.aspx)  
  

