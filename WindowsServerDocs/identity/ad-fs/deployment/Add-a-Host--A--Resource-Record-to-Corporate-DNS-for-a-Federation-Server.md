---
ms.assetid: 026747c7-4c34-41c7-b7ea-27f9a7f64a35
title: Aggiungere un record di risorse host (A) al DNS aziendale per un server federativo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 132e71cec134d17dd73be998683c09f752fdc414
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360327"
---
# <a name="add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Aggiungere un record di risorse host (A) al DNS aziendale per un server federativo



Per consentire ai client della rete aziendale di accedere correttamente a un server federativo utilizzando l'autenticazione integrata di Windows, è necessario creare un host \(un record di risorse di\) nella Domain Name System aziendale \(DNS\) che risolve il nome host del server federativo di account \(ad esempio, fs.fabrikam.com\) all'indirizzo IP del server federativo o del cluster di server federativo. È possibile utilizzare la procedura seguente per aggiungere un host \(un\) record di risorse al DNS aziendale per un server federativo.  
  
L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Per aggiungere un host \(un\) record di risorse al DNS aziendale per un server federativo  
  
1.  In un server DNS per la rete aziendale, aprire lo snap\-DNS in.  
  
2.  Nell'albero della console, a destra\-fare clic sulla zona di ricerca diretta applicabile e quindi fare clic su **Nuovo Host \(A o AAAA\)** .  
  
3.  In **nome**Digitare solo il nome computer del server federativo o del cluster di server federativo. ad esempio, per il nome di dominio completo \(FQDN\) fs.fabrikam.com, digitare **FS**.  
  
4.  In **indirizzo IP**, digitare l'indirizzo IP del server federativo o del cluster di server federativo, ad esempio 192.168.1.4.  
  
5.  Fai clic su **Aggiungi host**.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisiti per la risoluzione dei nomi per i server federativi](https://technet.microsoft.com/library/dd807055.aspx)  
  

