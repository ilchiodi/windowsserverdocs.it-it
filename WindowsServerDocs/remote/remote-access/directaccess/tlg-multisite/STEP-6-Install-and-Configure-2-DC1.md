---
title: PASSAGGIO 6 di installare e configurare 2-DC1
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare una distribuzione multisito DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d66901a-c40b-474c-9948-f989f399cfea
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4f31c0e1d36ff458fb4807ab6856a56498f449ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838182"
---
# <a name="step-6-install-and-configure-2-dc1"></a>PASSAGGIO 6 di installare e configurare 2-DC1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

2-DC1 offre i servizi seguenti:  
  
-   Un controller di dominio per il dominio di Active Directory Domain Services (AD DS) corp2.corp.contoso.com.  
  
-   Un server DNS per il dominio DNS corp2.corp.contoso.com.  
  
Configurazione 2-DC1 è costituita dai seguenti elementi:  
  
- Installare il sistema operativo in 2-DC1
  
- Configurare le proprietà TCP/IP

- Configurare 2-DC1 come controller di dominio e server DNS

- Concedere le autorizzazioni di criteri di gruppo al CORP\User1
  
- Consenti ai computer CORP2 ottenere i certificati del computer
  
- Forzare la replica tra DC1 e 2-DC1
  
## <a name="install-the-operating-system-on-2-dc1"></a>Installare il sistema operativo in 2-DC1  
In primo luogo, installare Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
### <a name="to-install-the-operating-system-on-2-dc1"></a>Per installare il sistema operativo in 2-DC1  
  
1.  Avviare l'installazione di Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
2.  Seguire le istruzioni per completare l'installazione, specificando Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (installazione completa) e una password complessa per l'account amministratore locale. Accedere utilizzando l'account Administrator locale.  
  
3.  Connettersi 2-DC1 a una rete che abbia accesso a Internet ed eseguire Windows Update per installare gli aggiornamenti più recenti per Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 e quindi disconnettersi da Internet.  
  
4.  Connettere 2-DC1 nella subnet Corpnet di 2.  
  
## <a name="configure-tcpip-properties"></a>Configurare le proprietà TCP/IP  
Configurare il protocollo TCP/IP con indirizzi IP statici.  
  
### <a name="to-configure-tcpip-on-2-dc1"></a>Per configurare TCP/IP su DC1-2  
  
1.  Nella console di Server Manager, fare clic su **Server locale**e quindi il **delle proprietà** area, accanto a **connessione Ethernet cablata**, fare clic sul collegamento.  
  
2.  In **Connessioni di rete** fare clic con il pulsante destro del mouse su **Connessione Ethernet cablata** e quindi scegliere **Proprietà**.  
  
3.  Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
4.  Fare clic su **Utilizza il seguente indirizzo IP**. Nelle **indirizzo IP**, digitare **10.2.0.1**. In **Subnet mask**digitare **255.255.255.0**. Nelle **gateway predefinito**, digitare **10.2.0.254**. Fare clic su **utilizza i seguenti indirizzi server DNS**, nel **server DNS preferito**, digitare **10.2.0.1**e nella **server DNS alternativo**, tipo **10.0.0.1**.  
  
5.  Fare clic su **avanzate**, quindi fare clic sui **DNS** scheda.  
  
6.  Nella **suffisso DNS per la connessione**, digitare **corp2.corp.contoso.com**, quindi fare clic su **OK** due volte.  
  
7.  Fare clic su **Protocollo Internet versione 6 (TCP/IPv6)**, quindi fare clic su **Proprietà**.  
  
8.  Fare clic su **Usa l'indirizzo IPv6 seguente**. Nelle **indirizzo IPv6**, digitare **2001:db8:2::1**. Nelle **lunghezza prefisso Subnet**, digitare **64**. Nelle **gateway predefinito**, digitare **2001:db8:2::fe**. Fare clic su **utilizza i seguenti indirizzi server DNS**, nel **server DNS preferito**, digitare **2001:db8:2::1**e nella **server DNS alternativo**, tipo di **2001:db8:1::1**.  
  
9. Fare clic su **avanzate**, quindi fare clic sui **DNS** scheda.  
  
10. Nella **suffisso DNS per la connessione**, digitare **corp2.corp.contoso.com**, quindi fare clic su **OK** due volte.  
  
11. Nel **proprietà di connessione Ethernet cablata** finestra di dialogo, fare clic su **Chiudi**.  
  
12. Chiudere la finestra **Connessioni di rete**.  
  
13. Nella console di Server Manager, in **Server locale**, nella **delle proprietà** area, accanto a **nome Computer**, fare clic sul collegamento.  
  
14. Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Cambia**.  
  
15. Nel **cambiamenti dominio/nome Computer** nella finestra di dialogo **nome Computer**, tipo **2-DC1**, quindi fare clic su **OK**.  
  
16. Quando viene richiesto il riavvio del computer, fare clic su **OK**.  
  
17. Nella finestra di dialogo **Proprietà del sistema** fare clic su **Chiudi**.  
  
18. Quando viene richiesto di riavviare il computer, fare clic su **Riavvia ora**.  
  
19. Dopo il riavvio, accedere usando l'account amministratore locale.  
  
## <a name="configure-2-dc1-as-a-domain-controller-and-dns-server"></a>Configurare 2-DC1 come controller di dominio e server DNS  
Configurare 2-DC1 come controller di dominio per il dominio corp2.corp.contoso.com e come server DNS per il dominio DNS corp2.corp.contoso.com.  
  
### <a name="to-configure-2-dc1-as-a-domain-controller-and-dns-server"></a>Per configurare 2-DC1 come controller di dominio e server DNS  
  
1.  Nella console di Server Manager, sul **Dashboard**, fare clic su **Aggiungi ruoli e funzionalità**.  
  
2.  Fare clic su **Avanti** tre volte per visualizzare la schermata di selezione ruoli server  
  
3.  Nel **Selezione ruoli server** pagina, selezionare **Active Directory Domain Services**. Fare clic su **Aggiungi funzionalità** quando richiesto e quindi fare clic su **Avanti** tre volte.  
  
4.  Nella pagina **Conferma selezioni per l'installazione** fare clic su **Installa**.  
  
5.  Al termine dell'installazione, fare clic su **promuovere il server a un controller di dominio**.  
  
6.  In Active Directory Domain Services configurazione guidata, nella **configurazione di distribuzione** pagina, fare clic su **aggiungere un nuovo dominio a una foresta esistente**.  
  
7.  In **nome del dominio padre**, digitare **corp.contoso.com**, in **nuovo nome di dominio**, digitare **corp2**.  
  
8.  In **fornire le credenziali per eseguire questa operazione**, fare clic su **Modifica**. Nel **sicurezza di Windows** nella finestra di dialogo **nome utente**, tipo **corp.contoso.com\Administrator**e nella **Password**, immettere il corp \ Password dell'amministratore, fare clic su **OK**, quindi fare clic su **successivo**.  
  
9. Nel **opzioni Controller di dominio** pagina, assicurarsi che le **nome sito** viene **secondo sito**. Sotto **digitare la password modalità ripristino servizi Directory (DSRM)**, nel **Password** e **Conferma password**, digitare due volte una password complessa e quindi fare clic su  **Avanti** cinque volte.  
  
10. Nel **controllo dei prerequisiti** pagina, dopo aver convalidati i prerequisiti, fare clic su **installare**.  
  
11. Attendere fino al termine della procedura guidata della configurazione di servizi di Active Directory e DNS e quindi fare clic su **Chiudi**.  
  
12. Dopo il riavvio del computer, accedere al dominio CORP2 usando l'account amministratore.  
  
## <a name="provide-group-policy-permissions-to-corpuser1"></a>Concedere le autorizzazioni di criteri di gruppo al CORP\User1  
Utilizzare questa procedura per fornire autorizzazioni complete per creare e modificare gli oggetti Criteri di gruppo corp2 all'utente di CORP\User1.  
  
### <a name="to-provide-group-policy-permissions"></a>Per fornire autorizzazioni basate sui criteri di gruppo  
  
1.  Nel **avviare** digitare**GPMC. msc**, quindi premere INVIO.  
  
2.  Nella console Gestione criteri di gruppo, aprire **foresta: corp.contoso.com/Domains/corp2.corp.contoso.com**.  
  
3.  Nel riquadro dei dettagli, fare clic sui **delega** scheda. Nel **l'autorizzazione** elenco a discesa, fare clic su **oggetti Criteri di gruppo collegamento**.  
  
4.  Fare clic su **Add**e nella nuova **Seleziona utenti, Computer o gruppo** della finestra di dialogo fare clic su **percorsi**.  
  
5.  Nel **posizioni** nella finestra di dialogo il **posizione** struttura ad albero, fare clic su **corp.contoso.com**e fare clic su **OK**.  
  
6.  In **immettere il nome dell'oggetto da selezionare** tipo **User1**, fare clic su **OK**e scegliere il **Aggiungi gruppo o utente** della finestra di dialogo fare clic su **OK** .  
  
7.  Nella console Gestione criteri di gruppo, nell'albero, fare clic su **oggetti Criteri di gruppo**, e i dettagli fare clic sul riquadro le **delega** scheda.  
  
8.  Fare clic su **Add**e nella nuova **Seleziona utenti, Computer o gruppo** della finestra di dialogo fare clic su **percorsi**.  
  
9. Nel **posizioni** nella finestra di dialogo il **posizione** struttura ad albero, fare clic su **corp.contoso.com**e fare clic su **OK**.  
  
10. Nelle **immettere il nome dell'oggetto da selezionare** tipo **User1**, fare clic su **OK**.  
  
11. Nella console Gestione criteri di gruppo, nell'albero, fare clic su **i filtri WMI**, e i dettagli fare clic sul riquadro le **delega** scheda.  
  
12. Fare clic su **Add**e nella nuova **Seleziona utenti, Computer o gruppo** della finestra di dialogo fare clic su **percorsi**.  
  
13. Nel **posizioni** nella finestra di dialogo il **posizione** struttura ad albero, fare clic su **corp.contoso.com**e fare clic su **OK**.  
  
14. Nelle **immettere il nome dell'oggetto da selezionare** tipo **User1**, fare clic su **OK**. Nel **Aggiungi gruppo o utente** finestra di dialogo, assicurarsi che **autorizzazioni** sono impostate su **controllo completo**e quindi fare clic su **OK**.  
  
15. Chiudere la console Gestione Criteri di gruppo.  
  
## <a name="allow-corp2-computers-to-obtain-computer-certificates"></a>Consenti ai computer CORP2 ottenere i certificati del computer 

I computer nel dominio CORP2 devono ottenere i certificati del computer da autorità di certificazione in APP1. In APP1, seguire questa procedura.  
  
### <a name="to-allow-corp2-computers-to-automatically-obtain-computer-certificates"></a>Per consentire ai computer CORP2 ottenere automaticamente i certificati del computer  
  
1.  In APP1, fare clic su **avviare**, digitare **certtmpl. msc**, quindi premere INVIO.  
  
2.  Nel **certificati modello Console**, nel riquadro centrale, fare doppio clic su **autenticazione Client-Server**.  
  
3.  Nel **le proprietà di autenticazione Client-Server** della finestra di dialogo fare clic sui **sicurezza** scheda.  
  
4.  Fare clic su **Add**e scegliere il **Seleziona utenti, computer, account del servizio o gruppi** nella finestra di dialogo fare clic su **percorsi**.  
  
5.  Nel **posizioni** nella finestra di dialogo **posizione**, espandere **corp.contoso.com**, fare clic su **corp2.corp.contoso.com**e quindi fare clic su  **OK**.  
  
6.  Nelle **immettere i nomi degli oggetti da selezionare**, tipo **Domain Admins; I computer del dominio** e quindi fare clic su **OK**.  
  
7.  Nel **le proprietà di autenticazione Client-Server** nella finestra di dialogo **nomi utente o gruppo**, fare clic su **Domain Admins (amministratori CORP2\Domain)** e in  **Le autorizzazioni per Domain Admins**, nella **Allow** colonna, selezionare **scrivere** e **registrazione**.  
  
8.  Nella **nomi utente o gruppo**, fare clic su **computer del dominio (computer CORP2\Domain)** e in **le autorizzazioni per i computer del dominio**, nel **Consenti**colonna, selezionare **Enroll** e **registrazione automatica**, quindi fare clic su **OK**.  
  
9. Chiudere la Console dei modelli di certificato.  
  
## <a name="replication"></a>Forzare la replica tra DC1 e 2-DC1  
Prima di poter registrare certificati sulla EDGE1 2, è necessario forzare la replica delle impostazioni da DC1 a 2-DC1. Questa operazione deve essere eseguita in DC1.  
  
### <a name="to-force-replication"></a>Per forzare la replica  
  
1.  In DC1 fare clic su **avviare**, quindi fare clic su **Active Directory Sites and Services**.  
  
2.  Nella console di Active Directory Sites and Services, nell'albero, espandere **trasporti tra siti**, quindi fare clic su **IP**.  
  
3.  Nel riquadro dei dettagli fare doppio clic **DEFAULTIPSITELINK**.  
  
4.  Nel **DEFAULTIPSITELINK Properties** nella finestra di dialogo **costo**, tipo **1**nella **replicare ogni**, tipo **15**, quindi fare clic su **OK**. Attendere 15 minuti per il completamento della replica.  
  
5.  Per forzare la replica ora nell'albero della console, espandere **impostazioni Sites\Default-primo-sito-name\Servers\DC1\NTDS**, nel riquadro dei dettagli, fare doppio clic su **<automatically generated>**, fare clic su  **Replicate Now**, quindi scegliere il **Replicate Now** nella finestra di dialogo fare clic su **OK**.  
  
6.  Per garantire la replica è stata completata eseguire le operazioni seguenti:  
  
    1.  Nel **avviare** digitare**cmd.exe**, quindi premere INVIO.  
  
    2.  Digitare il comando seguente e quindi premere INVIO.  
  
        ```  
        repadmin /syncall /e /A /P /d /q  
        ```  
  
    3.  Assicurarsi che tutte le partizioni siano sincronizzate senza errori. In caso contrario, quindi eseguire di nuovo il comando fino a quando non vengono segnalati errori prima di procedere.  
  
7.  Chiudere la finestra del prompt dei comandi.  
  


