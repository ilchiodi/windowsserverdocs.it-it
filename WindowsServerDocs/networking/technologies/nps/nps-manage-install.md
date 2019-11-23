---
title: Installare il server dei criteri di rete
description: È possibile utilizzare questo argomento per installare Server dei criteri di rete tramite Windows PowerShell o l'aggiunta guidata ruoli e funzionalità in Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 4842a4ab-70bb-4744-bea7-70f2ac892ad1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 25b8586d370865dd3ae4393c2536c348a5d0810f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396188"
---
# <a name="install-network-policy-server"></a>Installare il server dei criteri di rete

È possibile utilizzare questo argomento per installare Server dei criteri di rete utilizzando Windows PowerShell o l'aggiunta guidata ruoli e funzionalità. Server dei criteri di rete è un servizio ruolo del ruolo server Servizi di accesso e criteri di rete.

> [!NOTE]
> Per impostazione predefinita, Server dei criteri di rete rimane in ascolto del traffico RADIUS sulle porte 1812, 1813, 1645 e 1646 di tutte le schede di rete installate. Se Windows Firewall con sicurezza avanzata è abilitata quando si installa NPS, le eccezioni del firewall per queste porte vengono create automaticamente durante il processo di installazione sia per il protocollo Internet versione 6 \(IPv6\) che per il traffico IPv4. Se i server di accesso di rete sono configurati per l'invio di traffico RADIUS su porte diverse da quelle predefinite, rimuovere le eccezioni create durante l'installazione dei criteri di RETE in Windows Firewall con sicurezza avanzata e creare eccezioni per le porte utilizzate per il traffico RADIUS.

**Credenziali amministrative**

Per completare questa procedura, è necessario essere un membro del **Domain Admins** gruppo.

## <a name="to-install-nps-by-using-windows-powershell"></a>Per installare NPS utilizzando Windows PowerShell

Per eseguire questa procedura usando Windows PowerShell, eseguire Windows PowerShell come amministratore, digitare il comando seguente e quindi premere INVIO.

`Install-WindowsFeature NPAS -IncludeManagementTools`

## <a name="to-install-nps-by-using-server-manager"></a>Per installare NPS utilizzando Server Manager

1.  Su NPS1, in Server Manager fare clic su **Gestisci** e quindi scegliere **Aggiungi ruoli e funzionalità**. Viene avviata l'Aggiunta guidata ruoli e funzionalità.

2.  In **Prima di iniziare** fare clic su **Avanti**.

    > [!NOTE]
    > La pagina **Prima di iniziare** dell'Aggiunta guidata ruoli e funzionalità non viene visualizzata se durante un'esecuzione precedente della procedura guidata è stata selezionata la casella di controllo **Ignora questa pagina per impostazione predefinita**.

3.  In **Select Installation Type**, assicurarsi che **installazione basata su ruoli o basata su funzionalità** sia selezionata e quindi fare clic su **Avanti**.

4.  In **Selezione server di destinazione** assicurarsi che sia selezionata l'opzione **Selezionare un server dal pool di server**. Assicurarsi che in **Pool di server** sia selezionato il computer locale. Fai clic su **Next**.

5.  In **Selezione ruoli server**, in **ruoli**, selezionare **servizi di accesso e criteri di rete**. Verrà visualizzata una finestra di dialogo in cui viene chiesto se aggiungere funzionalità richieste per servizi di accesso e criteri di rete. Fare clic su **Aggiungi funzionalità**e quindi su **Avanti** .

6.  In **Selezione funzionalità** fare clic su **Avanti**, quindi in **Servizi di accesso e criteri di rete** esaminare le informazioni fornite e quindi fare clic su **Avanti**.

7.  In **Selezione servizi ruolo** fare clic su **Server dei criteri di rete**.  In **aggiungere le funzionalità necessarie per Server dei criteri di rete**, fare clic su **Aggiungi funzionalità**. Fai clic su **Next**.

8.  In **Conferma selezioni per l'installazione**, fare clic su **Riavvia automaticamente il server di destinazione se necessario**. Quando viene chiesto di confermare la selezione, fare clic su **Sì**, quindi fare clic su **installare**. Nella pagina di stato dell'installazione viene visualizzato lo stato di avanzamento del processo di installazione. Al termine del processo, il messaggio "Installazione completata in *nomecomputer*" viene visualizzato, in cui *nomecomputer* è il nome del computer su cui è installato Server dei criteri di rete. Fare clic su **Chiudi**.

Per altre informazioni, vedere [Manage NPSs](nps-manage-servers.md).