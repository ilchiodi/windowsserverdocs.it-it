---
title: 'PASSAGGIO 6: installare e configurare 2-DC1'
description: 'Questo argomento fa parte della Guida al Lab di test: illustra una distribuzione multisito di DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d66901a-c40b-474c-9948-f989f399cfea
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4c7a8243922f58f9705a85cd30b2a68cf4d876c6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404777"
---
# <a name="step-6-install-and-configure-2-dc1"></a>PASSAGGIO 6: installare e configurare 2-DC1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

2-DC1 fornisce i servizi seguenti:  
  
-   Un controller di dominio per il dominio di corp2.corp.contoso.com Active Directory Domain Services (AD DS).  
  
-   Un server DNS per il dominio DNS corp2.corp.contoso.com.  
  
la configurazione 2-DC1 è costituita dagli elementi seguenti:  
  
- Installare il sistema operativo in 2-DC1
  
- Configurare le proprietà TCP/IP

- Configurare 2-DC1 come controller di dominio e server DNS

- Fornire autorizzazioni Criteri di gruppo per Corp\user1.
  
- Consenti ai computer CORP2 di ottenere i certificati del computer
  
- Forzare la replica tra DC1 e 2-DC1
  
## <a name="install-the-operating-system-on-2-dc1"></a>Installare il sistema operativo in 2-DC1  
Prima di tutto, installare Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
### <a name="to-install-the-operating-system-on-2-dc1"></a>Per installare il sistema operativo in 2-DC1  
  
1.  Avviare l'installazione di Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
2.  Seguire le istruzioni per completare l'installazione, specificando Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (installazione completa) e una password complessa per l'account amministratore locale. Accedere utilizzando l'account Administrator locale.  
  
3.  Connettere 2-DC1 a una rete dotata di accesso a Internet ed eseguire Windows Update per installare gli aggiornamenti più recenti per Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012, quindi disconnettersi da Internet.  
  
4.  Connettere 2-DC1 alla subnet Corpnet.  
  
## <a name="configure-tcpip-properties"></a>Configurare le proprietà TCP/IP  
Configurare il protocollo TCP/IP con indirizzi IP statici.  
  
### <a name="to-configure-tcpip-on-2-dc1"></a>Per configurare TCP/IP in 2-DC1  
  
1.  Nella console di Server Manager fare clic su **server locale**e quindi nell'area **Proprietà** , accanto a **connessione Ethernet cablata**, fare clic sul collegamento.  
  
2.  In **Connessioni di rete** fare clic con il pulsante destro del mouse su **Connessione Ethernet cablata** e quindi scegliere **Proprietà**.  
  
3.  Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
4.  Fare clic su **Utilizza il seguente indirizzo IP**. In **indirizzo IP**Digitare **10.2.0.1**. In **Subnet mask**digitare **255.255.255.0**. In **gateway predefinito**Digitare **10.2.0.254**. Fare clic su **utilizza i seguenti indirizzi server DNS**, in **server DNS preferito**, digitare **10.2.0.1**e in **server DNS alternativo**digitare **10.0.0.1**.  
  
5.  Fare clic su **Avanzate**e quindi sulla scheda **DNS** .  
  
6.  In **suffisso DNS per la connessione**Digitare **Corp2.Corp.contoso.com**e quindi fare clic su **OK** due volte.  
  
7.  Fare clic su **Protocollo Internet versione 6 (TCP/IPv6)** , quindi fare clic su **Proprietà**.  
  
8.  Fare clic su **Usa il seguente indirizzo IPv6**. In **indirizzo IPv6**Digitare **2001: DB8:2:: 1**. In **lunghezza prefisso subnet**Digitare **64**. In **gateway predefinito**Digitare **2001: DB8:2:: Fe**. Fare clic su **utilizza i seguenti indirizzi server DNS**, in **server DNS preferito**, digitare **2001: DB8:2:: 1**e in **server DNS alternativo**digitare **2001: DB8:1:: 1**.  
  
9. Fare clic su **Avanzate**e quindi sulla scheda **DNS** .  
  
10. In **suffisso DNS per la connessione**Digitare **Corp2.Corp.contoso.com**e quindi fare clic su **OK** due volte.  
  
11. Nella finestra di dialogo **Proprietà connessione Ethernet cablata** fare clic su **Chiudi**.  
  
12. Chiudere la finestra **Connessioni di rete**.  
  
13. Nella console di Server Manager, in **server locale**, nell'area **Proprietà** , accanto a **nome computer**, fare clic sul collegamento.  
  
14. Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Cambia**.  
  
15. Nella finestra di dialogo **Cambiamenti dominio/nome computer** , in **nome computer**digitare **2-DC1**, quindi fare clic su **OK**.  
  
16. Quando viene richiesto il riavvio del computer, fare clic su **OK**.  
  
17. Nella finestra di dialogo **Proprietà del sistema** fare clic su **Chiudi**.  
  
18. Quando viene richiesto di riavviare il computer, fare clic su **Riavvia ora**.  
  
19. Dopo il riavvio, accedere utilizzando l'account amministratore locale.  
  
## <a name="configure-2-dc1-as-a-domain-controller-and-dns-server"></a>Configurare 2-DC1 come controller di dominio e server DNS  
Configurare 2-DC1 come controller di dominio per il dominio corp2.corp.contoso.com e come server DNS per il dominio DNS corp2.corp.contoso.com.  
  
### <a name="to-configure-2-dc1-as-a-domain-controller-and-dns-server"></a>Per configurare 2-DC1 come controller di dominio e server DNS  
  
1.  Nella console di Server Manager, nel **Dashboard**, fare clic su **Aggiungi ruoli e funzionalità**.  
  
2.  Fare clic su **Avanti** tre volte per visualizzare la schermata di selezione ruoli server  
  
3.  Nella pagina **Selezione ruoli server** selezionare **Active Directory Domain Services**. Fare clic su **Aggiungi funzionalità** quando richiesto e quindi fare clic su **Avanti** tre volte.  
  
4.  Nella pagina **Conferma selezioni per l'installazione** fare clic su **Installa**.  
  
5.  Al termine dell'installazione, fare clic su **promuovere il server a un controller di dominio**.  
  
6.  Nella configurazione guidata Active Directory Domain Services, nella pagina **Configurazione distribuzione** fare clic su **Aggiungi un nuovo dominio a una foresta esistente**.  
  
7.  In **nome dominio padre**Digitare **Corp.contoso.com**, in **nuovo nome dominio**, digitare **Corp2**.  
  
8.  In **fornire le credenziali per eseguire questa operazione**, fare clic su **Modifica**. Nella finestra di dialogo **sicurezza di Windows** , in **nome utente**digitare **Corp. contoso. com\Administrator**e in **password**immettere la password corp\Administrator, fare clic su **OK**e quindi fare clic su **Avanti**.  
  
9. Nella pagina **Opzioni controller di dominio** verificare che il nome del **sito** sia **Second-site**. In **digitare la password per la modalità ripristino servizi directory**, in **password** e **Conferma password**, digitare due volte una password complessa e quindi fare clic su **Avanti** cinque volte.  
  
10. Nel **controllo dei prerequisiti** pagina, dopo aver convalidati i prerequisiti, fare clic su **installare**.  
  
11. Attendere il completamento della configurazione dei servizi Active Directory e DNS da parte della procedura guidata, quindi fare clic su **Chiudi**.  
  
12. Dopo il riavvio del computer, accedere al dominio CORP2 utilizzando l'account Administrator.  
  
## <a name="provide-group-policy-permissions-to-corpuser1"></a>Fornire autorizzazioni Criteri di gruppo per Corp\user1.  
Usare questa procedura per fornire all'utente Corp\user1. le autorizzazioni complete per creare e modificare Corp2 Criteri di gruppo oggetti.  
  
### <a name="to-provide-group-policy-permissions"></a>Per fornire autorizzazioni di Criteri di gruppo  
  
1.  Nella schermata **Start** Digitare**gpmc. msc**, quindi premere INVIO.  
  
2.  Nella console di gestione Criteri di gruppo aprire **foresta: Corp.contoso.com/domains/Corp2.Corp.contoso.com**.  
  
3.  Nel riquadro dei dettagli fare clic sulla scheda **delega** . Nell'elenco a discesa **autorizzazione** fare clic su **Collega oggetti Criteri**di gruppo.  
  
4.  Fare clic su **Aggiungi**e nella finestra di dialogo nuovo **Seleziona utente, computer o gruppo** fare clic su **percorsi**.  
  
5.  Nella finestra di dialogo **percorsi** fare clic su **Corp.contoso.com**nell'albero del **percorso** , quindi fare clic su **OK**.  
  
6.  In **immettere il nome dell'oggetto da selezionare** digitare **User1**, fare clic su **OK**e nella finestra di dialogo **Aggiungi gruppo o utente** fare clic su **OK**.  
  
7.  Nell'albero di Criteri di gruppo Management Console fare clic su **criteri di gruppo oggetti**e nel riquadro dei dettagli fare clic sulla scheda **delega** .  
  
8.  Fare clic su **Aggiungi**e nella finestra di dialogo nuovo **Seleziona utente, computer o gruppo** fare clic su **percorsi**.  
  
9. Nella finestra di dialogo **percorsi** fare clic su **Corp.contoso.com**nell'albero del **percorso** , quindi fare clic su **OK**.  
  
10. In **immettere il nome dell'oggetto da selezionare** digitare **User1**, fare clic su **OK**.  
  
11. Nell'albero di Criteri di gruppo Management Console fare clic su **filtri WMI**e nel riquadro dei dettagli fare clic sulla scheda **delega** .  
  
12. Fare clic su **Aggiungi**e nella finestra di dialogo nuovo **Seleziona utente, computer o gruppo** fare clic su **percorsi**.  
  
13. Nella finestra di dialogo **percorsi** fare clic su **Corp.contoso.com**nell'albero del **percorso** , quindi fare clic su **OK**.  
  
14. In **immettere il nome dell'oggetto da selezionare** digitare **User1**, fare clic su **OK**. Nella finestra di dialogo **Aggiungi gruppo o utente** verificare che le **autorizzazioni** siano impostate su **controllo completo**, quindi fare clic su **OK**.  
  
15. Chiudere la console Gestione Criteri di gruppo.  
  
## <a name="allow-corp2-computers-to-obtain-computer-certificates"></a>Consenti ai computer CORP2 di ottenere i certificati del computer 

I computer nel dominio CORP2 devono ottenere i certificati del computer dall'autorità di certificazione in APP1. Eseguire questa procedura in APP1.  
  
### <a name="to-allow-corp2-computers-to-automatically-obtain-computer-certificates"></a>Per consentire ai computer CORP2 di ottenere automaticamente i certificati del computer  
  
1.  In APP1 fare clic sul pulsante **Start**, digitare **certtmpl. msc**, quindi premere INVIO.  
  
2.  Nel riquadro centrale della **console modello certificati**fare doppio clic su **autenticazione client-server**.  
  
3.  Nella finestra di dialogo **proprietà di autenticazione client-server** fare clic sulla scheda **sicurezza** .  
  
4.  Fare clic su **Aggiungi**e nella finestra di dialogo **Seleziona utenti, computer, account servizio o gruppi** fare clic su **percorsi**.  
  
5.  Nella finestra di dialogo **percorsi** , in **percorso**, espandere **corp.contoso.com**, fare clic su **Corp2.Corp.contoso.com**, quindi fare clic su **OK**.  
  
6.  In **immettere i nomi degli oggetti da selezionare**digitare **Domain Admins; Computer del dominio** e quindi fare clic su **OK**.  
  
7.  Nella finestra di dialogo **proprietà di autenticazione client-server** , **in utenti e gruppi**, fare clic su **Domain Admins (amministratori CORP2\Domain)** e in **autorizzazioni per Domain Admins**, nella colonna **Consenti** selezionare **Scrivi** e **registrarsi**.  
  
8.  In **utenti e gruppi**fare clic su **computer del dominio (computer CORP2\Domain)** e in **autorizzazioni per i computer del dominio**, nella colonna **Consenti** selezionare **registrazione** e **registrazione automatica**, quindi fare clic su **OK**.  
  
9. Chiudere la Console dei modelli di certificato.  
  
## <a name="replication"></a>Forzare la replica tra DC1 e 2-DC1  
Prima di poter registrare i certificati in 2 EDGE1, è necessario forzare la replica delle impostazioni da DC1 a 2-DC1. Questa operazione deve essere eseguita su DC1.  
  
### <a name="to-force-replication"></a>Per forzare la replica  
  
1.  In DC1 fare clic sul pulsante **Start**, quindi fare clic su **Active Directory siti e servizi**.  
  
2.  Nella console di Active Directory siti e servizi, nell'albero espandere **trasporti tra siti**, quindi fare clic su **IP**.  
  
3.  Nel riquadro dei dettagli fare doppio clic su **DEFAULTIPSITELINK**.  
  
4.  Nella finestra di dialogo **Proprietà DEFAULTIPSITELINK** , in **costo**, digitare **1**, in **replica ogni**, digitare **15**, quindi fare clic su **OK**. Attendere 15 minuti per il completamento della replica.  
  
5.  Per forzare ora la replica nell'albero della console, espandere **Impostazioni Sites\Default-First-Site-name\Servers\DC1\NTDS**, nel riquadro dei dettagli, fare clic con il pulsante destro del mouse su **<automatically generated>** , fare clic su **Replica ora**, quindi nella finestra di dialogo **Replica ora** , fare clic su **OK**.  
  
6.  Per assicurarsi che la replica sia stata completata correttamente, eseguire le operazioni seguenti:  
  
    1.  Nella schermata **Start** Digitare**cmd. exe**, quindi premere INVIO.  
  
    2.  Digitare il comando seguente e quindi premere INVIO.  
  
        ```  
        repadmin /syncall /e /A /P /d /q  
        ```  
  
    3.  Verificare che tutte le partizioni siano sincronizzate senza errori. In caso contrario, eseguire nuovamente il comando fino a quando non viene segnalato alcun errore prima di procedere.  
  
7.  Chiudere la finestra del prompt dei comandi.  
  


