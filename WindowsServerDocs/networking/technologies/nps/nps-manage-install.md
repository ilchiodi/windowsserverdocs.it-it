---
title: Installare Server dei criteri di rete
description: È possibile utilizzare questo argomento per installare Server dei criteri di rete (NPS) tramite di Windows PowerShell o l'aggiunta guidata ruoli e funzionalità in Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4842a4ab-70bb-4744-bea7-70f2ac892ad1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0b76654fa01b68b5a8c9ea1efc80dfbc47e6a62f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="install-network-policy-server"></a>Installare Server dei criteri di rete

È possibile utilizzare questo argomento per installare Server dei criteri di rete (NPS) tramite di Windows PowerShell o l'aggiunta guidata ruoli e funzionalità. Criteri di rete è un servizio ruolo del ruolo server Servizi di accesso e criteri di rete.

> [!NOTE]
> Per impostazione predefinita, dei criteri di rete è in ascolto del traffico RADIUS sulle porte 1812, 1813, 1645 e 1646 su tutte le schede di rete installate. Se Windows Firewall con sicurezza avanzata è abilitata quando si installa NPS, eccezioni del firewall per queste porte vengono create automaticamente durante il processo di installazione per il protocollo Internet versione 6 \(IPv6\) sia il traffico IPv4. Se i server di accesso di rete sono configurati per inviare il traffico RADIUS su porte diverse da quelle predefinite, rimuovere le eccezioni create durante l'installazione dei criteri di rete in Windows Firewall con sicurezza avanzata e creare eccezioni per le porte utilizzate per il traffico RADIUS.

**Credenziali amministrative**

Per completare questa procedura, è necessario essere un membro del **Domain Admins** gruppo.

## <a name="to-install-nps-by-using-windows-powershell"></a>Per installare dei criteri di rete tramite Windows PowerShell

Per eseguire questa procedura mediante Windows PowerShell, eseguire Windows PowerShell come amministratore, digitare il comando seguente e quindi premere INVIO.

`Install-WindowsFeature NPAS -IncludeManagementTools`

## <a name="to-install-nps-by-using-server-manager"></a>Per installare il server NPS tramite Server Manager

1.  Su NPS1, in Server Manager, fare clic su **Gestisci**, quindi fare clic su **Aggiungi ruoli e funzionalità**. Verrà avviata l'aggiunta guidata ruoli e funzionalità.

2.  In **prima di iniziare**, fare clic su **Avanti**.

    > [!NOTE]
    > Il **prima di iniziare** pagina l'aggiunta guidata ruoli e funzionalità non viene visualizzato se è stato selezionato in precedenza **Ignora questa pagina per impostazione predefinita** quando è stato eseguito l'aggiunta guidata ruoli e funzionalità.

3.  In **Selezione tipo di installazione**, assicurarsi che **installazione basata su ruoli o basata su funzionalità** sia selezionata e quindi fare clic su **Avanti**.

4.  In **server di destinazione**, assicurarsi che **selezionare un server dal pool di server** sia selezionata. In **Pool di Server**, verificare che sia selezionato il computer locale. Fare clic su **Avanti**.

5.  In **Selezione ruoli Server**, in **ruoli**selezionare **servizi di accesso e criteri di rete**. Una finestra di dialogo viene visualizzata se è necessario aggiungere le funzionalità necessarie per servizi di accesso e criteri di rete. Fare clic su **Aggiungi funzionalità**, quindi fare clic su **Avanti**

6.  In **selezionare le funzionalità**, fare clic su **Avanti**e in **servizi di accesso e criteri di rete**, esaminare le informazioni fornite e quindi fare clic su **Avanti**.

7.  In **Selezione servizi ruolo**, fare clic su **Server dei criteri di rete**.  In **Aggiungi funzionalità necessarie per Server dei criteri di rete**, fare clic su **Aggiungi funzionalità**. Fare clic su **Avanti**.

8.  In **Conferma selezioni per l'installazione**, fare clic su **riavvia automaticamente il server di destinazione se necessario**. Quando viene chiesto di confermare la selezione, fare clic su **Sì**, quindi fare clic su **installare**. La pagina di avanzamento installazione Visualizza stato durante il processo di installazione. Al termine del processo, il messaggio "Installazione completata in *ComputerName*" viene visualizzato, in cui *ComputerName* è il nome del computer su cui è installato Server dei criteri di rete. Fare clic su **Chiudi**.

Per ulteriori informazioni, vedere [gestione dei server dei criteri di rete](nps-manage-servers.md).