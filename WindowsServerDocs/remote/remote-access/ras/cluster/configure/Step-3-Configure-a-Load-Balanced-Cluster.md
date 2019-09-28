---
title: Passaggio 3 configurare un Cluster con bilanciamento del carico
description: Questo argomento fa parte della Guida deploy Remote Access in a cluster in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f000066e-7cf8-4085-82a3-4f4fe1cb3c5c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fb7dca9a0f7875936cbb30cbc9c5e9e0a7473237
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404639"
---
# <a name="step-3-configure-a-load-balanced-cluster"></a>Passaggio 3 configurare un Cluster con bilanciamento del carico

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Dopo la preparazione server per il cluster, configurare il bilanciamento del carico sul server singolo, configurare i certificati necessari e distribuire il cluster.  
  
|Attività|Descrizione|  
|----|--------|  
|[3,1 configurare il prefisso IPv6](#BKMK_Prefix)|Se l'ambiente azienda è IPv6 + IPv4, o solo IPv6, quindi sul singolo server di accesso remoto, assicurarsi che il prefisso IPv6 assegnato ai computer client DirectAccess sia sufficiente a coprire tutti i server del cluster.|  
|[3,2 abilitare il bilanciamento del carico](#BKMK_NLB)|Abilitare il bilanciamento del carico sul server di accesso remoto singolo.|  
|[3,3 installare il certificato IP-HTTPS](#BKMK_InstallIPHTTP)|Ogni server del cluster richiede un certificato server per autenticare la connessione IP-HTTPS.  Esportare il certificato IP-HTTPS dal singolo server di accesso remoto e distribuirlo in ogni server che verranno aggiunti al cluster. Questo è necessario solo se l'utilizzo di certificati non autofirmato.|  
|[3,4 installare il certificato del server dei percorsi di rete](#BKMK_NLS)|Se nel server singolo è il server dei percorsi rete distribuito in locale, è necessario distribuire il certificato server percorsi di rete in ogni server del cluster. Se il server dei percorsi di rete è ospitato su un server esterno, non è necessario un certificato in ogni server. Questo è necessario solo se l'utilizzo di certificati non autofirmato.|  
|[3,5 aggiungere server al cluster](#BKMK_Add)|Aggiungere tutti i server al cluster. Accesso remoto non deve essere configurata sul server da aggiungere.|  
|[3,6 rimuovere un server dal cluster](#BKMK_remove)|Istruzioni per rimuovere un server dal cluster.|  
|[3,7 disabilitare il bilanciamento del carico](#BKBK_disable)|Istruzioni per la disabilitazione di bilanciamento del carico.|  
  
> [!NOTE]  
> L'indirizzo IP selezionato per il DIP non deve essere in uso alle schede di rete del primo server di accesso remoto nel cluster. Iniziare la distribuzione di DirectAccess con indirizzi VIP e DIP aggiunto alla scheda di rete verrà generato un errore.  
  
> [!NOTE]  
> Assicurarsi di non utilizzare un DIP che è già presente in un altro computer nella rete.  
  
## <a name="BKMK_Prefix"></a>3,1 configurare il prefisso IPv6  
  
### <a name="configDA"></a>Per configurare il prefisso  
  
1.  Nel server di accesso remoto, fare clic su **avviare**, quindi fare clic su **Gestione accesso remoto**. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
2.  Nella console di gestione Accesso remoto fare clic su **Configurazione**.  
  
3.  Nel riquadro centrale della console, nel **passaggio 2 DirectAccess Server** area, fare clic su **modificare**.  
  
4.  Fare clic su **Configurazione prefissi**. Nel **Configurazione prefissi** pagina **prefisso IPv6 assegnato ai computer client DirectAccess**, immettere il prefisso IPv6 utilizzato per i computer client DirectAccess con una lunghezza di subnet di 59, ad esempio, **2001:db8:1:1000:: 59**. Se sono stati inoltre abilitata la VPN con IPv6, quindi verrà visualizzato un prefisso IPv6 e la lunghezza della subnet devono essere modificati e 59. Fare clic su **Avanti**.  
  
5.  Nel riquadro centrale della console, fare clic su **Fine**.  
  
6.  Nel **controllare l'accesso remoto** la finestra di dialogo, rivedere le impostazioni di configurazione e quindi fare clic su **Applica**. Nella finestra di dialogo **Applicazione delle impostazioni della Configurazione guidata Accesso remoto** fare clic su **Chiudi**.  
  
## <a name="BKMK_NLB"></a>3,2 abilitare il bilanciamento del carico  
  
#### <a name="to-enable-load-balancing"></a>Per abilitare il bilanciamento del carico  
  
1.  Nel server DirectAccess configurato, fare clic su **avviare**, quindi fare clic su **Gestione accesso remoto**. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
2.  Nella console di gestione accesso remoto, nel riquadro a sinistra, fare clic su **configurazione**, e quindi la **attività** riquadro, fare clic su **abilitare il bilanciamento del carico**.  
  
3.  Di abilitare carico bilanciamento guidata fare clic su **Avanti**.  
  
4.  Si è scelto in passaggi di pianificazione in base:  
  
    1.  Bilanciamento carico di Windows: Nella pagina **metodo di bilanciamento del carico** fare clic su **Usa bilanciamento carico di rete (NLB) di Windows**, quindi fare clic su **Avanti**.  
  
    2.  Servizio di bilanciamento del carico esterno: Nella pagina **metodo di bilanciamento del carico** fare clic su **Usa un servizio di bilanciamento del carico esterno**, quindi fare clic su **Avanti**.  
  
5.  In una distribuzione di adapter singola rete, nel **indirizzi IP dedicati** pagina, eseguire le operazioni seguenti e quindi fare clic su **Avanti**:  
  
    1.  Nel **indirizzo IPv4** casella, immettere il nuovo indirizzo IPv4 per il server di accesso remoto, la classe di indirizzo IPv4 corrente essere l'indirizzo IP virtuale (VIP) del cluster con bilanciamento del carico. Nel **Subnet mask** immettere la subnet mask.  
  
    2.  Se l'ambiente azienda è IPv6 nativo, quindi nel **indirizzo IPv6** casella, immettere il nuovo indirizzo IPv6 per il server di accesso remoto; corrente verrà indirizzo IPv6 costituita dall'indirizzo VIP del cluster con bilanciamento del carico. Nel **lunghezza prefisso Subnet** immettere la lunghezza del prefisso della subnet.  
  
6.  In un due distribuzione, scheda di rete nel **dedicato gli indirizzi IP esterni** pagina, eseguire le operazioni seguenti e quindi fare clic su **successivo**:  
  
    1.  Nel **indirizzo IPv4** casella, immettere il nuovo indirizzo IPv4 esterno per questo server di accesso remoto; il corrente IPv4 indirizzo verrà l'indirizzo IP virtuale (VIP) al bilanciamento del carico del cluster. Nel **Subnet mask** immettere la subnet mask.  
  
    2.  Se sono presenti indirizzi IPv6 nativi attualmente configurati nella scheda di rete con connessione internet del server di accesso remoto, nel **indirizzo IPv6** casella, immettere il nuovo indirizzo IPv6 esterno per questo server di accesso remoto; il corrente IPv6 indirizzo verrà l'indirizzo VIP al bilanciamento del carico del cluster. Nel **lunghezza prefisso Subnet** immettere la lunghezza del prefisso della subnet.  
  
7.  In un due distribuzione, scheda di rete sul **dedicato agli indirizzi IP interni** pagina, eseguire le operazioni seguenti e quindi fare clic su **Avanti**:  
  
    1.  Nel **indirizzo IPv4** casella, immettere il nuovo indirizzo IPv4 interno per il server di accesso remoto; il corrente IPv4 indirizzo verrà l'indirizzo VIP al bilanciamento del carico del cluster. Nel **Subnet mask** immettere la subnet mask.  
  
    2.  Se l'ambiente azienda è IPv6 nativo, quindi nel **indirizzo IPv6** casella, immettere il nuovo indirizzo IPv6 interno per il server di accesso remoto; il corrente IPv6 indirizzo verrà l'indirizzo VIP al bilanciamento del carico del cluster. Nel **lunghezza prefisso Subnet** immettere la lunghezza del prefisso della subnet.  
  
8.  Nel **riepilogo** pagina, fare clic su **Commit**.  
  
9. Nel **abilitare il bilanciamento del carico** la finestra di dialogo, fare clic su **Chiudi**.  
  
10. Di abilitare carico bilanciamento guidata fare clic su **Chiudi**.  
  
    > [!NOTE]  
    > Se viene utilizzato il bilanciamento del carico esterno, annotare gli indirizzi IP virtuali e inserirle in servizi di bilanciamento del carico esterno.  
  
](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>comandi equivalenti</em> di PowerShell per Windows PowerShell @no__t 0Windows***  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
Se si sceglie di utilizzare Bilanciamento carico di RETE di Windows i passaggi di pianificazione, quindi eseguire le operazioni seguenti:  
  
```  
Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress "2.1.1.20/255.255.255.0" -InternalDedicatedIPAddress @("10.1.1.30/255.255.255.0","3ffe::20/64") -InternetVirtualIPAddress @("2.1.1.1/255.255.255.0","2.1.1.2/255.255.255.0") -InternalVirtualIPAddress @("10.1.1.2/255.255.255.0","3ffe::2/64")  
```  
  
Se si sceglie di utilizzare un servizio di bilanciamento del carico esterno i passaggi di pianificazione: quindi eseguire le operazioni seguenti:  
  
```  
Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress "2.1.1.20/255.255.255.0" -InternalDedicatedIPAddress @("10.1.1.30/255.255.255.0","3ffe::20/64") -UseThirdPrtyLoadBalancer  
```  
  
> [!NOTE]  
> Si consiglia di non includere le modifiche alle impostazioni di bilanciamento del carico con le modifiche a tutte le altre impostazioni, se si utilizza oggetti Criteri di gruppo di gestione temporanea. Qualsiasi modifica alle impostazioni di bilanciamento del carico deve essere applicato per primo e quindi devono essere apportate altre modifiche di configurazione. Inoltre, dopo la configurazione di bilanciamento del carico in un nuovo server DirectAccess, attendi del tempo essere applicato e replicati tra i server DNS nell'organizzazione, prima di modificare altre impostazioni di DirectAccess relative al nuovo cluster le modifiche di IP.  
  
## <a name="BKMK_InstallIPHTTP"></a>3,3 installare il certificato IP-HTTPS  
Appartenenza al gruppo locale **amministratori** o gruppo equivalente, è il requisito minimo necessario per completare questa procedura.  
  
### <a name="IPHTTPSCert"></a>Per installare il certificato IP-HTTPS  
  
1.  Nel server di accesso remoto configurato, fare clic su **avviare**, tipo **mmc** e premere INVIO. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
2.  Nella console di MMC, nel **File** menu, fare clic su **Aggiungi/Rimuovi Snap-in**.  
  
3.  Nel **Aggiungi o Rimuovi Snap-in** la finestra di dialogo, fare clic su **certificati**, fare clic su **Aggiungi**, fare clic su **account Computer**, fare clic su **Avanti**, fare clic su **Fine**, e quindi fare clic su **OK**.  
  
4.  Nel riquadro sinistro della console, passare a **certificati (Computer locale) \Personal\Certificates**. Fare clic su certificato IP-HTTPS, scegliere **tutte le attività** e fare clic su **esportazione**.  
  
5.  Nel **iniziale per l'esportazione guidata certificati** pagina, fare clic su **Avanti**.  
  
6.  Nel **esportazione della chiave privata** pagina, fare clic su **Sì, Esporta la chiave privata**, e quindi fare clic su **Avanti**.  
  
7.  Nel **Formato File di esportazione** pagina, fare clic su **scambio informazioni personali - PKCS #12 (. PFX)** , quindi fare clic su **Avanti**.  
  
8.  Nel **sicurezza** pagina, selezionare il **Password** casella di controllo, immettere una password nel **Password** casella e confermare la password e quindi fare clic su **Avanti**.  
  
9. Nel **File da esportare** pagina, immettere un nome per il file del certificato e salvarlo sul desktop e quindi fare clic su **Avanti**.  
  
10. Nel **il completamento dell'esportazione guidata certificati** pagina, fare clic su **Fine**.  
  
11. Nel **Esportazione guidata certificati** la finestra di dialogo, fare clic su **OK**.  
  
12. Copiare il certificato in tutti i server che si desidera essere membri del cluster.  
  
13. Nel nuovo server DirectAccess, fare clic su **avviare**, tipo **mmc** e premere INVIO. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
14. Nella console di MMC, nel **File** menu, fare clic su **Aggiungi/Rimuovi Snap-in**.  
  
15. Nel **Aggiungi o Rimuovi Snap-in** la finestra di dialogo, fare clic su **certificati**, fare clic su **Aggiungi**, fare clic su **account Computer**, fare clic su **Avanti**, fare clic su **Fine**, e quindi fare clic su **OK**.  
  
16. Nel riquadro sinistro della console, passare a **certificati (Computer locale) \Personal\Certificates**. Fare clic con il **certificati** nodo, scegliere **tutte le attività**, quindi fare clic su **importazione**.  
  
17. Nella pagina **Importazione guidata certificati** fare clic su **Avanti**.  
  
18. Nel **File da importare** pagina, fare clic su **Sfoglia** per individuare il certificato. Selezionare il certificato e quindi fare clic su **Avanti**.  
  
19. Nel **protezione chiave privata** nella pagina di **Password** casella, digitare la password e quindi fare clic su **Avanti**.  
  
20. Nella pagina **Archivio certificati** fare clic su **Avanti**.  
  
21. Nella pagina **Completamento dell'Importazione guidata certificati** fare clic su **Fine**.  
  
22. Nel **Importazione guidata certificati** la finestra di dialogo, fare clic su **OK**.  
  
23. Ripetere i passaggi da 13 a 22 in tutti i server che si desidera essere membri del cluster.  
  
## <a name="BKMK_NLS"></a>3,4 installare il certificato del server dei percorsi di rete  
Appartenenza al gruppo locale **amministratori** o gruppo equivalente, è il requisito minimo necessario per completare questa procedura.  
  
#### <a name="to-install-a-certificate-for-network-location"></a>Per installare un certificato per il percorso di rete  
  
1.  Nel server di accesso remoto, fare clic su **avviare**, tipo **mmc**, quindi premere INVIO. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
2.  Scegliere **Aggiungi/Rimuovi snap-in** dal menu **File**.  
  
3.  Fare clic su **certificati**, fare clic su **Aggiungi**, fare clic su **account Computer**, fare clic su **Avanti**, fare clic su **computer locale**, fare clic su **Fine**, quindi fare clic su **OK**.  
  
4.  Nell'albero della console dello snap-in Certificati aprire **Certificati (computer locale)\Personale\Certificati**.  
  
5.  Fare clic con il pulsante destro del mouse su **Certificati**, scegliere **Tutte le attività** e quindi fare clic su **Richiedi nuovo certificato**.  
  
6.  Fare clic due volte su **Avanti** .  
  
7.  Nel **richiesta certificati** pagina, fare clic sul modello di certificato Server Web e quindi fare clic su **sono necessarie ulteriori informazioni per registrare questo certificato**.  
  
    Se non viene visualizzato il modello di certificato Server Web, assicurarsi che l'account computer del server Accesso remoto dispone di autorizzazioni di registrazione per il modello di certificato Server Web. Per ulteriori informazioni, vedere [configurare le autorizzazioni per il modello di certificato Server Web](https://msdn.microsoft.com/library/ee649249.aspx).  
  
8.  Nel **soggetto** scheda il **Proprietà certificato** della finestra di dialogo **nome soggetto**, per **tipo**, selezionare **nome comune**.  
  
9. In **valore**, digitare il nome di dominio completo (FQDN) per il nome intranet del sito Web server di percorso di rete (ad esempio, NLS) e quindi fare clic su **Aggiungi**.  
  
10. Fare clic su **OK**, fare clic su **registrazione**, quindi fare clic su **Fine**.  
  
11. Nel riquadro dei dettagli dello snap-in certificati, verificare che sia registrato un nuovo certificato con il nome FQDN **scopi designati** di **l'autenticazione Server**.  
  
12. Fare clic con il pulsante destro del mouse sul certificato e scegliere **Proprietà**.  
  
13. In **nome descrittivo**, tipo **certificato percorsi di rete**, quindi fare clic su **OK**.  
  
    > [!TIP]  
    > I passaggi 12 e 13 sono facoltativi, ma rendono più semplice per selezionare il certificato per il percorso di rete quando si configura accesso remoto.  
  
14. Ripetere questa procedura su tutti i server che si desidera essere membri del cluster.  
  
## <a name="BKMK_Add"></a>3,5 aggiungere server al cluster  
 
  
#### <a name="to-add-servers-to-the-cluster"></a>Per aggiungere server al cluster  
  
1.  Nel server DirectAccess configurato, fare clic su **avviare**, quindi fare clic su **Gestione accesso remoto**. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
2.  Nella console di gestione Accesso remoto fare clic su **Configurazione**. Nel **attività** riquadro, in **cluster con bilanciamento del carico**, fare clic su **aggiungere o rimuovere server**.  
  
3.  Nel **aggiungere o rimuovere server** la finestra di dialogo, fare clic su **Aggiungi Server**.  
  
4.  Nel **aggiungere un Server** della finestra di dialogo di **selezionare Server** pagina, immettere il nome del server di accesso remoto aggiuntivo e quindi fare clic su **Avanti**.  
  
5.  Nel **schede di rete** pagina, eseguire le operazioni seguenti:  
  
    -   Se si distribuisce una topologia con due schede di rete **adapter esterno**, selezionare la scheda connessa alla rete esterna. In **scheda interna**, selezionare la scheda connessa alla rete interna.  
  
    -   Se si distribuisce una topologia con una scheda di rete, in **scheda di rete**, selezionare la scheda connessa alla rete interna.  
  
6.  Nel **schede di rete** pagina **Selezionare il certificato utilizzato per autenticare le connessioni IP-HTTPS**, fare clic su **Sfoglia** per individuare e selezionare il certificato IP-HTTPS e quindi fare clic su **Avanti**.  
  
7.  Nel **Server dei percorsi di rete** pagina, fare clic su **Sfoglia** per selezionare il certificato per il sito Web server percorsi di rete in esecuzione sul server di accesso remoto e quindi fare clic su **Avanti**.  
  
    > [!NOTE]  
    > Il **Server dei percorsi di rete** verrà visualizzata la pagina solo quando il server dei percorsi rete è in esecuzione sul server di accesso remoto.  
  
    > [!NOTE]  
    > Se VPN sono stati inoltre configurata nel server di accesso remoto, verrà richiesto di aggiungere la VPN pool informazioni sugli indirizzi IP a questo punto.  
  
8.  Nel **riepilogo** pagina, fare clic su **Aggiungi**.  
  
9. Nel **completamento** pagina, fare clic su **Chiudi**.  
  
10. Ripetere questa procedura per tutti i server di accesso remoto da aggiungere al cluster.  
  
11. Nel **aggiungere o rimuovere server** la finestra di dialogo, fare clic su **Commit**.  
  
12. Nel **aggiunta e rimozione di server** la finestra di dialogo, fare clic su **Chiudi**.  
  
](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>comandi equivalenti</em> di PowerShell per Windows PowerShell @no__t 0Windows***  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
```  
Add-RemoteAccessLoadBalancerNode -RemoteAccessServer <server name>  
```  
  
> [!NOTE]  
> Se VPN non è stata abilitata in un cluster con bilanciamento di carico, è necessario fornire non eventuali intervalli di indirizzi VPN quando si aggiunge un nuovo server al cluster tramite i cmdlet di Windows PowerShell. Se si è fatto per errore, rimuovere il server dal cluster e aggiungerlo nuovamente al cluster senza specificare gli intervalli di indirizzi VPN.  
  
## <a name="BKMK_remove"></a>3,6 rimuovere un server dal cluster  
 
  
#### <a name="to-remove-a-server-from-the-cluster"></a>Per rimuovere un server dal cluster  
  
1.  Nel server di accesso remoto configurato, fare clic su **avviare**, quindi fare clic su **Gestione accesso remoto**. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
2.  Nella console di gestione Accesso remoto fare clic su **Configurazione**. Nel **attività** riquadro, in **cluster con bilanciamento del carico**, fare clic su **aggiungere o rimuovere server**.  
  
3.  Nel **aggiungere o rimuovere server** la finestra di dialogo, selezionare il server di accesso remoto che si desidera rimuovere e quindi fare clic su **Rimuovi Server**.  
  
4.  Nel **rimuovere avvisi Server** la finestra di dialogo, verificare che si è scelto di server appropriato e quindi fare clic su **OK**.  
  
5.  Ripetere questa procedura per tutti i server di accesso remoto da rimuovere dal cluster.  
  
6.  Nel **aggiungere o rimuovere server** la finestra di dialogo, fare clic su **Commit**.  
  
7.  Nel **aggiunta e rimozione di server** la finestra di dialogo, fare clic su **Chiudi**.  
  
](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>comandi equivalenti</em> di PowerShell per Windows PowerShell @no__t 0Windows***  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
```  
Remove-RemoteAccessLoadBalancerNode -RemoteAccessServer <server name>  
```  
  
## <a name="BKBK_disable"></a>3,7 disabilitare il bilanciamento del carico  
[Eseguire questo passaggio con Windows PowerShell](assetId:///7a817ca0-2b4a-4476-9d28-9a63ff2453f9)  
  
#### <a name="to-disable-load-balancing"></a>Per disabilitare il bilanciamento del carico  
  
1.  Nel server DirectAccess configurato, fare clic su **avviare**, quindi fare clic su **Gestione accesso remoto**. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
2.  Nella console di gestione Accesso remoto fare clic su **Configurazione**. Nel **attività** riquadro, in **cluster con bilanciamento del carico**, fare clic su **disabilitare il bilanciamento del carico**.  
  
3.  Nel **disabilitare il bilanciamento del carico** la finestra di dialogo, fare clic su **Ok**.  
  
4.  Nel **disabilitare il bilanciamento del carico** la finestra di dialogo, fare clic su **Chiudi**.  
  
](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>comandi equivalenti</em> di PowerShell per Windows PowerShell @no__t 0Windows***  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
```  
set-RemoteAccessLoadBalancer -disable  
```  
  
Disabilitare il bilanciamento del carico rimuoverà le impostazioni di accesso remoto e le impostazioni di bilanciamento carico di RETE (se configurata) da tutti i server, ad eccezione del server da cui viene eseguito. In questo server di accesso remoto, le impostazioni di bilanciamento carico di RETE verranno rimossa (se configurata) ma rimarranno impostazioni di accesso remoto.  
  
Fare clic su **rimuovere impostazioni di configurazione** rimuoverà bilanciamento carico di RETE e accesso remoto (se configurata) da tutti i server nella distribuzione.  
  
> [!NOTE]  
> -   Se accesso remoto viene disinstallato quando il bilanciamento del carico viene distribuito, tutti i server vengono lasciati con DIP. Vengono rimossi gli indirizzi VIP. In questo modo tutte le route della rete aziendale che sono destinati agli indirizzi VIP per avere esito negativo. Questo influisce anche sulle voci DNS che sono stati risolti per gli indirizzi VIP, ad esempio il nome soggetto del certificato rete percorso server. Per evitare questo problema, disabilitare il bilanciamento del carico, che lascia gli indirizzi VIP nell'ultimo server di accesso remoto e quindi disinstallare accesso remoto.  
> -   Dopo aver utilizzato la **RemoteAccessLoadBalancer Set** cmdlet per disabilitare il bilanciamento del carico, attendere due minuti prima di eseguire qualsiasi altro cmdlet. Questa deve essere eseguita anche in tutti gli script che eseguono un altro cmdlet dopo il **RemoteAccessLoadBalancer Set-disabilitare** cmdlet.  
> -   Disabilitazione delle modifiche di bilanciamento del carico l'indirizzo IP virtuale del cluster in un indirizzo IP dedicato. Di conseguenza, qualsiasi operazione che esegue una query per il nome del server avrà esito negativo finché non scade la voce nella cache DNS sul server. Assicurarsi di non eseguire i cmdlet PowerShell di accesso remoto dopo la disattivazione di bilanciamento del carico finché non scade la cache sul server. Questo problema è più comune se si tenta di disabilitare il bilanciamento del carico in un computer da un altro computer che si trova in un altro dominio. Ciò si verifica anche se si disabilita il bilanciamento del carico dalla console di gestione accesso remoto e può impedire la configurazione del caricamento. La configurazione verrà caricato dopo che la cache è scaduto o è stata scaricata.  
  
## <a name="BKMK_Links"></a>Vedere anche  
  
-   [Passaggio 4: Verifica del cluster @ no__t-0  
  


