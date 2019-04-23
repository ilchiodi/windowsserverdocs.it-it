---
title: Gestione di Server dei criteri di rete con gli strumenti di amministrazione
description: È possibile utilizzare questo argomento per informazioni sugli strumenti che è possibile usare per gestire il Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 5de80dc0-53be-42b7-8e5b-24d213bf2b25
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fa1767e49b16f4a55f36e052d4354aaead540f26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856882"
---
# <a name="network-policy-server-management-with-administration-tools"></a>Gestione di Server dei criteri di rete con gli strumenti di amministrazione

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sugli strumenti che è possibile usare per gestire il NPSs.

Dopo aver installato NPS, è possibile amministrare NPSs:

- In locale, usando la Console di gestione dei criteri di rete Microsoft \(MMC\) snap-in, la console Criteri di rete statica in strumenti di amministrazione, i comandi di Windows PowerShell o la Shell di rete \(Netsh\) comandi per criteri di rete.
- Da un remoto dei criteri di rete, tramite lo snap-in NPS MMC, i comandi Netsh per criteri di rete, i comandi di Windows PowerShell per criteri di rete, o connessione Desktop remoto.
- Da una workstation remota, utilizzando connessione Desktop remoto in combinazione con altri strumenti, ad esempio il NPS MMC o Windows PowerShell.

>[!NOTE]
>In Windows Server 2016, è possibile gestire i criteri di rete locale tramite la console Criteri di rete. Per gestire NPSs remoti e locali, è necessario utilizzare lo snap-NPS MMC\-in.

Le sezioni seguenti forniscono istruzioni su come gestire le NPSs locali e remoti.

## <a name="configure-the-local-nps-by-using-the-nps-console"></a>Configurare i criteri di rete locale tramite la Console Criteri di rete

Dopo aver installato NPS, è possibile utilizzare questa procedura per gestire i criteri di rete locale usando NPS MMC.

**Credenziali amministrative** 

Per completare questa procedura, è necessario essere un membro del gruppo Administrators.

### <a name="to-configure-the-local-nps-by-using-the-nps-console"></a>Per configurare i criteri di rete locale tramite la console Criteri di rete

1. In Server Manager, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete**. Apre la console Criteri di rete.

2. Nella console Criteri di rete, fare clic su NPS \(locale\). Nel riquadro dei dettagli, scegliere **configurazione Standard** oppure **Advanced Configuration**, e quindi eseguire una delle operazioni seguenti in base alla selezione:
    - Se si sceglie **configurazione Standard**, selezionare uno scenario nell'elenco e quindi seguire le istruzioni per avviare la configurazione guidata.
    - Se si sceglie **Advanced Configuration**, fare clic sulla freccia per espandere **opzioni di configurazione avanzate**e quindi esaminare e configurare le opzioni disponibili basate sulle funzionalità dei criteri di rete che si desidera: Server RADIUS, proxy RADIUS o entrambi.

## <a name="manage-multiple-npss-by-using-the-nps-mmc-snap-in"></a>Gestire più NPSs utilizzando lo snap-NPS MMC\-in

È possibile utilizzare questa procedura per gestire i criteri di rete locale e più NPSs remoto utilizzando lo snap-NPS MMC\-in.

Prima di eseguire la procedura riportata di seguito, è necessario installare i criteri di rete nel computer locale e nei computer remoti.

A seconda delle condizioni della rete e il numero di NPSs gestiti tramite lo snap-NPS MMC\-, nella risposta di snap MMC\-in potrebbe risultare lento. Inoltre, il traffico di configurazione dei criteri di rete viene inviato in rete durante una sessione di amministrazione remota utilizzando lo snap NPS\-in. Assicurarsi che la rete è fisicamente protetta e che gli utenti malintenzionati non hanno accesso a questo traffico di rete.

**Credenziali amministrative** 

Per completare questa procedura, è necessario essere un membro del gruppo Administrators.

### <a name="to-manage-multiple-npss-by-using-the-nps-snap-in"></a>Per gestire più NPSs utilizzando lo snap NPS\-in

1. Per aprire MMC, eseguire Windows PowerShell come amministratore. In Windows PowerShell, digitare **mmc**, quindi premere INVIO. Verrà visualizzata la finestra di Microsoft Management Console.
2. In MMC nel **File** menu, fare clic su **Aggiungi/Rimuovi snap-\-in**. Il **Aggiungi o Rimuovi Snap\-aggiuntivi** verrà visualizzata la finestra di dialogo.
3. Nella **Aggiungi o Rimuovi Snap\-componenti aggiuntivi**, in **disponibili snap\-componenti aggiuntivi**, scorrere verso il basso nell'elenco, fare clic su **Server dei criteri di rete**e quindi fare clic su  **Aggiungere**. Viene visualizzata la finestra di dialogo **Seleziona computer**.
4. Nelle **seleziona Computer**, verificare che **computer locale \(il computer in cui è in esecuzione questa console\)**  sia selezionata e quindi fare clic su **OK**. Lo snap\-in per i criteri di rete locale viene aggiunto all'elenco in **selezionati snap\-aggiuntivi**.
5. Nella **Aggiungi o Rimuovi Snap\-componenti aggiuntivi**, in **disponibili snap\-componenti aggiuntivi**, assicurarsi che **Server dei criteri di rete** sia ancora selezionata e quindi fare clic su  **Aggiungere**. Il **seleziona Computer** verrà nuovamente visualizzata la finestra di dialogo.
6. Nelle **seleziona Computer**, fare clic su **un altro computer**, quindi digitare l'indirizzo IP o nome di dominio completo \(FQDN\) di NPS remoto che si desidera gestire con i criteri di rete Blocca\-in. Facoltativamente, è possibile fare clic su **esplorare** consultare la directory per computer in cui si desidera aggiungere. Fare clic su **OK**.
7. Ripetere i passaggi 5 e 6 per aggiungere ulteriori NPSs per lo snap di NPS\-in. Dopo aver aggiunto tutti i NPSs si desidera gestire, fare clic su **OK**.
8. Per salvare lo snap-in Criteri di rete per un uso successivo, fare clic su **File**, quindi fare clic su **salvare**. Nel **Salva con nome** finestra di dialogo, passare al percorso del disco rigido in cui si desidera salvare il file, digitare un nome per la Console di gestione di Microsoft \(msc\) del file e quindi fare clic su **Salva**. 

## <a name="manage-an-nps-by-using-remote-desktop-connection"></a>Gestire un NPS tramite connessione Desktop remoto

È possibile utilizzare questa procedura per gestire un remoto dei criteri di rete tramite connessione Desktop remoto.

Utilizzando connessione Desktop remoto, è possibile gestire in remoto il NPSs che esegue Windows Server 2016. È possibile gestire NPSs in modalità remota da un computer che eseguono Windows 10 o versioni precedenti dei sistemi operativi client Windows.

È possibile utilizzare connessione Desktop remoto per gestire più NPSs usando uno dei due metodi.

1. Creare una connessione Desktop remoto a ogni il NPSs singolarmente.
2. Usare Desktop remoto per connettersi a uno dei criteri di rete e quindi usare NPS MMC in tale server per gestire altri server remoti. Per altre informazioni, vedere la sezione precedente **NPSs più gestire utilizzando la blocca NPS MMC\-in**.

**Credenziali amministrative** 

Per completare questa procedura, è necessario essere un membro del gruppo Administrators su NPS.

### <a name="to-manage-an-nps-by-using-remote-desktop-connection"></a>Per gestire un NPS tramite connessione Desktop remoto

1. In ogni dei criteri di rete che si desidera gestire in remoto, in Server Manager, selezionare **Server locale**. Nel riquadro dei dettagli di Server Manager, visualizzare il **Desktop remoto** impostazione e quindi eseguire una delle operazioni seguenti. 
    1. Se il valore della **Desktop remoto** impostazione **abilitato**, non è necessario eseguire alcuni passaggi di questa procedura. Passare direttamente al passaggio 4 per avviare la configurazione delle autorizzazioni utente Desktop remoto.
    2. Se il **Desktop remoto** impostazione **disabilitato**, fare clic sulla parola **disabilitato**. Il **le proprietà di sistema** verrà visualizzata la finestra di dialogo nel **remoto** scheda.
2. Nelle **Desktop remoto**, fare clic su **Consenti connessioni remote a computer**. Il **connessione Desktop remoto** verrà visualizzata la finestra di dialogo. Effettuare una delle operazioni seguenti.
    1. Per personalizzare le connessioni di rete che sono consentite, fare clic su **Windows Firewall con sicurezza avanzata**e quindi configurare le impostazioni che si vuole consentire. 
    2. Per abilitare la connessione Desktop remoto per tutte le connessioni al computer di rete, fare clic su **OK**.
3. Nelle **le proprietà di sistema**, nel **Desktop remoto**, decidere se abilitare **Consenti connessioni solo da computer che eseguono Desktop remoto con autenticazione a livello di rete**ed effettuare la selezione.
4. Fare clic su **selezionare utenti**. Il **Remote Desktop Users** verrà visualizzata la finestra di dialogo.
5. Nelle **Remote Desktop Users**, per concedere autorizzazioni a un utente di connettersi in remoto ai criteri di rete, fare clic su **Add**e quindi digitare il nome utente per l'account dell'utente. Fare clic su **OK**.
6. Ripetere il passaggio 5 per ogni utente per il quale si desidera concedere l'autorizzazione di accesso remoto ai criteri di rete. Una volta terminato l'aggiunta di utenti, fare clic su **OK** per chiudere la **utenti Desktop remoto** la finestra di dialogo e **OK** per chiudere il **le proprietà di sistema**finestra di dialogo.
7. Per connettersi a un remoto dei criteri di rete che è stato configurato con i passaggi precedenti, fare clic su **avviare**, scorrere verso il basso l'elenco in ordine alfabetico e quindi fare clic su **accessori di Windows**, fare clic su **remoto Connessione desktop**. Il **connessione Desktop remoto** verrà visualizzata la finestra di dialogo.
8. Nel **connessione Desktop remoto** nella finestra di dialogo **Computer**, digitare il nome dei criteri di rete o l'indirizzo IP. Se si preferisce, fare clic su **le opzioni**, configurare le opzioni di connessione aggiuntive e quindi fare clic su **salvare** per salvare la connessione per l'uso ripetuto.
9. Fare clic su **Connect**e quando richiesto specificare le credenziali dell'account utente di un account che disponga delle autorizzazioni per l'accesso e quindi configurare i criteri di rete.

## <a name="use-netsh-nps-commands-to-manage-an-nps"></a>Usare i comandi Netsh NPS per gestire un criteri di rete

Per visualizzare e impostare la configurazione dell'autenticazione, autorizzazione, contabilità e controllo del database usato sia da criteri di rete e il servizio di accesso remoto, è possibile usare i comandi nel contesto Netsh NPS. Usare i comandi nel contesto Netsh NPS per:

- Configurare o riconfigurare un criteri di rete, inclusi tutti gli aspetti di criteri di rete che sono anche disponibili per la configurazione tramite la console Criteri di rete nell'interfaccia di Windows.
- Esportare la configurazione di un archivio dei criteri di rete (server di origine), incluse le chiavi del Registro di sistema e la configurazione dei criteri di rete, come uno script di Netsh.
- Importare la configurazione in un altro dei criteri di rete usando uno script di Netsh e il file di configurazione esportati dall'origine dei criteri di rete.

È possibile eseguire questi comandi dal Prompt di comandi di Windows Server 2016 o da Windows PowerShell. È anche possibile eseguire i comandi netsh nps in script e file batch.

**Credenziali amministrative** 

Per eseguire questa procedura, è necessario essere membri del gruppo Administrators nel computer locale.

### <a name="to-enter-the-netsh-nps-context-on-an-nps"></a>Per accedere al contesto Netsh NPS in dei criteri di rete

1. Aprire il prompt dei comandi o Windows PowerShell.
2. Tipo di **netsh**, quindi premere INVIO.
3. Tipo di **nps**, quindi premere INVIO.
4. Per visualizzare un elenco di comandi disponibili, digitare un punto interrogativo \(?\) e premere INVIO.


Per altre informazioni sui comandi Netsh NPS, vedere [comandi Netsh per Server dei criteri di rete in Windows Server 2008](https://technet.microsoft.com/library/cc754428(v=ws.10).aspx), o scaricare l'intero [riferimento tecnico su Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc?redir=0) dalla TechNet Gallery. Questo download è la rete Shell tecnico riferimento per Windows Server 2008 e Windows Server 2008 R2 full. Il formato è la Guida di Windows \(chm\) in un file zip. Questi comandi sono ancora presenti in Windows Server 2016 e Windows 10, quindi è possibile usare netsh in questi ambienti, anche se è consigliabile usare Windows PowerShell.

## <a name="use-windows-powershell-to-manage-npss"></a>Usare Windows PowerShell per gestire NPSs

È possibile usare i comandi di Windows PowerShell per gestire NPSs. Per altre informazioni, vedere gli argomenti di riferimento di comando Windows PowerShell seguenti.

- [Cmdlet di Server dei criteri in Windows PowerShell di rete](https://technet.microsoft.com/library/jj872739(v=wps.630).aspx). È possibile usare questi comandi netsh in Windows Server 2012 R2 o versioni successive.
- [Modulo dei criteri di rete](https://technet.microsoft.com/itpro/powershell/windows/nps/index). È possibile usare questi comandi netsh di Windows Server 2016.

Per altre informazioni sull'amministrazione dei criteri di rete, vedere [gestire Server criteri di rete (NPS)](nps-manage-top.md).
