---
title: 'Guida al Lab di test: dimostrazione di DirectAccess in un Cluster con bilanciamento carico di rete di Windows'
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare DirectAccess in un Cluster con bilanciamento carico di rete di Windows per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: db15dcf5-4d64-48d7-818a-06c2839e1289
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2318fa58a343b24ec401390b3cbbd6f22fe86870
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281597"
---
# <a name="test-lab-guide-demonstrate-directaccess-in-a-cluster-with-windows-nlb"></a>Guida dell'ambiente di prova: Dimostrazione di DirectAccess in un cluster con Bilanciamento carico di rete di Windows

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Accesso remoto è un ruolo del server in Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 i sistemi operativi che consente agli utenti remoti di accedere in modo sicuro le risorse di rete interna usando DirectAccess o VPN RRAS. Questa guida contiene istruzioni dettagliate per estendere il [Guida al Lab di Test: Dimostrazione del singolo Server DirectAccess con ambiente misto IPv4 e IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004) per dimostrare il bilanciamento del carico di rete di DirectAccess e configurazione del cluster.  
  
## <a name="about-this-guide"></a>Informazioni sulla guida  
Questa guida contiene istruzioni per configurare e dimostrare Accesso remoto usando sei server e due computer client. Il lab di test di Accesso remoto completato con NLB simula una intranet, Internet e una rete domestica, dimostrando la funzionalità di Accesso remoto in diversi scenari di connessione Internet.  
  
> [!IMPORTANT]  
> Questo lab è un modello di prova che usa il numero minimo di computer. La configurazione descritta in questa guida è unicamente a scopi di lab di test e non deve essere usata in un ambiente di produzione.  
  
## <a name="KnownIssues"></a>Problemi noti  
Di seguito sono riportati alcuni problemi noti durante la configurazione di uno scenario cluster:  
  
-   Dopo aver configurato DirectAccess in una distribuzione solo IPv4 con un'unica scheda di rete e dopo che l'impostazione predefinita DNS64 (l'indirizzo IPv6 che contiene ":3333::") viene automaticamente configurata nella scheda di rete, il tentativo di abilitare il bilanciamento del carico usando la Console di gestione Accesso remoto comporta la visualizzazione di una richiesta all'utente di fornire un DIP IPv6. Se viene fornito un DIP IPv6, la configurazione non riesce dopo aver fatto clic su **Commit** con l'errore: Parametro non corretto.  
  
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
  


