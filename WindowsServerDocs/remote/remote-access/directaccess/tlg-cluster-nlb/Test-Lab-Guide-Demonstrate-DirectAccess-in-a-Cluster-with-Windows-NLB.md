---
title: Guida al Lab di test-dimostrazione di DirectAccess in un cluster con bilanciamento carico di servizio di Windows
description: 'Questo argomento fa parte della Guida al Lab di test: dimostrazione di DirectAccess in un cluster con bilanciamento carico di servizio di Windows per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: db15dcf5-4d64-48d7-818a-06c2839e1289
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e0c82f9f56ea680c11cd612e17326fe7cf96aeca
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388435"
---
# <a name="test-lab-guide-demonstrate-directaccess-in-a-cluster-with-windows-nlb"></a>Guida al lab di test: Dimostrazione di DirectAccess in un cluster con Bilanciamento carico di rete di Windows

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Accesso remoto è un ruolo del server nei sistemi operativi Windows Server 2016, Windows Server 2012 R2 E Windows Server 2012 che consente agli utenti remoti di accedere in modo sicuro alle risorse di rete interne tramite DirectAccess o VPN RRAS. Questa guida contiene istruzioni dettagliate per estendere la [Guida al lab di test: Dimostrazione della configurazione del singolo server DirectAccess con ambiente misto IPv4 e IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004) e dimostrare la configurazione del Bilanciamento carico di rete di DirectAccess e del cluster.  
  
## <a name="about-this-guide"></a>Informazioni sulla guida  
Questa guida contiene istruzioni per configurare e dimostrare Accesso remoto usando sei server e due computer client. Il lab di test di Accesso remoto completato con NLB simula una intranet, Internet e una rete domestica, dimostrando la funzionalità di Accesso remoto in diversi scenari di connessione Internet.  
  
> [!IMPORTANT]  
> Questo lab è un modello di prova che usa il numero minimo di computer. La configurazione descritta in questa guida è unicamente a scopi di lab di test e non deve essere usata in un ambiente di produzione.  
  
## <a name="KnownIssues"></a>Problemi noti  
Di seguito sono riportati alcuni problemi noti durante la configurazione di uno scenario cluster:  
  
-   Dopo aver configurato DirectAccess in una distribuzione solo IPv4 con un'unica scheda di rete e dopo che l'impostazione predefinita DNS64 (l'indirizzo IPv6 che contiene ":3333::") viene automaticamente configurata nella scheda di rete, il tentativo di abilitare il bilanciamento del carico usando la Console di gestione Accesso remoto comporta la visualizzazione di una richiesta all'utente di fornire un DIP IPv6. Se si specifica un DIP IPv6, la configurazione non riesce dopo aver fatto clic su **Commit** e viene visualizzato l'errore Parametro non corretto.  
  
    Per risolvere il problema:  
  
    1.  Scaricare gli script di backup e ripristino da [Backup e ripristino della configurazione di Accesso remoto](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6).  
  
    2.  Eseguire il backup degli oggetti Criteri di gruppo di Accesso remoto usando lo script Backup RemoteAccess.ps1 scaricato  
  
    3.  Provare ad abilitare il bilanciamento del carico fino al passaggio in cui si verifica un errore. Nella finestra di dialogo Abilita bilanciamento del carico, espandere l'area dei dettagli, farvi clic con il pulsante destro del mouse e quindi scegliere **Copia script**.  
  
    4.  Aprire Blocco note e incollare il contenuto degli Appunti. Ad esempio:  
  
        ```  
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19/255.255.255.0','fdc4:29bd:abde:3333::2/128') -InternetVirtualIPAddress @('fdc4:29bd:abde:3333::1/128', '10.244.4.21/255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose  
        ```  
  
    5.  Chiudere eventuali finestre di dialogo di Accesso remoto aperte e chiudere la Console di gestione Accesso remoto.  
  
    6.  Modificare il testo incollato e rimuovere gli indirizzi IPv6. Ad esempio:  
  
        ```  
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19/255.255.255.0') -InternetVirtualIPAddress @('10.244.4.21/255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose  
        ```  
  
    7.  In una finestra di PowerShell con privilegi elevati, eseguire il comando del passaggio precedente.  
  
    8.  Se il cmdlet non riesce in fase di esecuzione (non a causa di valori di input non corretti), eseguire il comando Restore RemoteAccess.ps1 e seguire le istruzioni per assicurarsi che l'integrità della configurazione originale venga mantenuta.  
  
    9. È ora possibile riaprire la Console di gestione Accesso remoto.  
  


