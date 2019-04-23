---
title: Installare il server dei criteri di rete
description: È possibile utilizzare questo argomento per installare Server dei criteri di rete (NPS) con sia Windows PowerShell o l'aggiunta guidata ruoli e funzionalità in Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4842a4ab-70bb-4744-bea7-70f2ac892ad1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9d11f77ff8b9db8bc10970b325afae60ba937cfa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831192"
---
# <a name="install-network-policy-server"></a>Installare il server dei criteri di rete

È possibile utilizzare questo argomento per installare Server dei criteri di rete (NPS) con sia Windows PowerShell o l'aggiunta guidata ruoli e funzionalità. Server dei criteri di rete è un servizio ruolo del ruolo server Servizi di accesso e criteri di rete.

> [!NOTE]
> Per impostazione predefinita, Server dei criteri di rete rimane in ascolto del traffico RADIUS sulle porte 1812, 1813, 1645 e 1646 di tutte le schede di rete installate. Se Windows Firewall con sicurezza avanzata è abilitata quando si installa NPS, eccezioni del firewall per queste porte vengono create automaticamente durante il processo di installazione di entrambi protocollo Internet versione 6 \(IPv6\) e il traffico IPv4. Se i server di accesso di rete sono configurati per l'invio di traffico RADIUS su porte diverse da quelle predefinite, rimuovere le eccezioni create durante l'installazione dei criteri di RETE in Windows Firewall con sicurezza avanzata e creare eccezioni per le porte utilizzate per il traffico RADIUS.

**Credenziali amministrative**

Per completare questa procedura, è necessario essere un membro del **Domain Admins** gruppo.

## <a name="to-install-nps-by-using-windows-powershell"></a>Per installare il server NPS con Windows PowerShell

Per eseguire questa procedura mediante Windows PowerShell, eseguire Windows PowerShell come amministratore, digitare il comando seguente e quindi premere INVIO.

`Install-WindowsFeature NPAS -IncludeManagementTools`

## <a name="to-install-nps-by-using-server-manager"></a>Per installare il server NPS tramite Server Manager

1.  Su NPS1, in Server Manager fare clic su **Gestisci** e quindi scegliere **Aggiungi ruoli e funzionalità**. Viene avviata l'Aggiunta guidata ruoli e funzionalità.

2.  In **Prima di iniziare** fare clic su **Avanti**.

    > [!NOTE]
    > La pagina **Prima di iniziare** dell'Aggiunta guidata ruoli e funzionalità non viene visualizzata se durante un'esecuzione precedente della procedura guidata è stata selezionata la casella di controllo **Ignora questa pagina per impostazione predefinita**.

3.  In **Select Installation Type**, assicurarsi che **installazione basata su ruoli o basata su funzionalità** sia selezionata e quindi fare clic su **Avanti**.

4.  In **Selezione server di destinazione** assicurarsi che sia selezionata l'opzione **Selezionare un server dal pool di server**. Assicurarsi che in **Pool di server** sia selezionato il computer locale. Fare clic su **Avanti**.

5.  Nelle **Selezione ruoli Server**, nel **ruoli**, selezionare **servizi di accesso e criteri di rete**. Viene aperta una finestra di dialogo che chiede se è necessario aggiungere le funzionalità necessarie per servizi di accesso e criteri di rete. Fare clic su **Aggiungi funzionalità**, quindi fare clic su **successivo**

6.  In **Selezione funzionalità** fare clic su **Avanti**, quindi in **Servizi di accesso e criteri di rete** esaminare le informazioni fornite e quindi fare clic su **Avanti**.

7.  In **Selezione servizi ruolo** fare clic su **Server dei criteri di rete**.  In **aggiungere le funzionalità necessarie per Server dei criteri di rete**, fare clic su **Aggiungi funzionalità**. Fare clic su **Avanti**.

8.  In **Conferma selezioni per l'installazione**, fare clic su **Riavvia automaticamente il server di destinazione se necessario**. Quando viene chiesto di confermare la selezione, fare clic su **Sì**, quindi fare clic su **installare**. Nella pagina di stato dell'installazione viene visualizzato lo stato di avanzamento del processo di installazione. Al termine del processo, il messaggio "Installazione completata in *nomecomputer*" viene visualizzato, in cui *nomecomputer* è il nome del computer su cui è installato Server dei criteri di rete. Fare clic su **Chiudi**.

Per altre informazioni, vedere [NPSs gestire](nps-manage-servers.md).