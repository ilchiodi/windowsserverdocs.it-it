---
title: Risoluzione dei problemi di abilitazione della distribuzione multisito
description: Questo argomento fa parte della Guida distribuire più server di accesso remoto in una distribuzione multisito di Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 570c81d6-c4f4-464c-bee9-0acbd4993584
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ae01a63a494504120ca248ec56dd9bc9648c1ae4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858294"
---
# <a name="troubleshooting-enabling-multisite"></a>Risoluzione dei problemi di abilitazione della distribuzione multisito

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento vengono fornite informazioni sulla risoluzione dei problemi relativi al comando `Enable-DAMultisite`. Per verificare che il messaggio di errore ricevuto sia correlato all'abilitazione della distribuzione multisito, controllare che nel registro eventi di Windows sia presente l'ID evento 10051.  
  
## <a name="user-connectivity-issues"></a>Problemi di connettività degli utenti  
Gli utenti possono riscontrare problemi di connettività quando si abilita la distribuzione multisito con una configurazione non corretta.  
  
**Causa**  
  
In una distribuzione multisito, i computer client Windows 10 e Windows 8 sono in grado di eseguire il roaming tra diversi punti di ingresso.  I computer client Windows 7 devono essere associati a uno specifico punto di ingresso nella distribuzione multisito. Se i computer non sono nel gruppo di sicurezza corretto, possono ricevere impostazioni di Criteri di gruppo errate.  
  
**Soluzione**  
  
DirectAccess richiede almeno un gruppo di sicurezza per tutti i computer client Windows 10 e Windows 8. si consiglia di usare un gruppo di sicurezza per tutti i computer Windows 10 e Windows 8 per dominio. DirectAccess richiede anche un gruppo di sicurezza per i computer client Windows 7 per ogni punto di ingresso. Ogni computer client deve essere incluso in un solo gruppo di sicurezza. Pertanto, è necessario assicurarsi che i gruppi di sicurezza per i client Windows 10 e Windows 8 includano solo i computer che eseguono Windows 10 o Windows 8 e che ogni computer client Windows 7 appartenga a un singolo gruppo di sicurezza dedicato per il punto di ingresso pertinente e che nessun client Windows 10 o Windows 8 appartenga ai gruppi di sicurezza di Windows 7.  
  
Configurare i gruppi di sicurezza di Windows 8 nella pagina **Seleziona gruppi** della **Configurazione guidata client DirectAccess** . Configurare i gruppi di sicurezza di Windows 7 nella pagina **supporto client** della procedura guidata **Abilita distribuzione multisito** oppure nella pagina **supporto client** della procedura guidata **Aggiungi punto di ingresso** .  
  
## <a name="kerberos-proxy-authentication"></a>Autenticazione proxy Kerberos  
**Errore ricevuto**. L'autenticazione proxy Kerberos non è supportata in una distribuzione multisito. È necessario abilitare l'uso dei certificati computer per l'autenticazione server IPsec.  
  
**Causa**  
  
Prima di abilitare la distribuzione multisito è necessario abilitare l'autenticazione del certificato del computer.  
  
**Soluzione**  
  
Per abilitare l'autenticazione del certificato del computer  
  
1.  Nel riquadro dei dettagli della console di gestione Accesso remoto fare clic su **Modifica** in **Passaggio 2 Server di Accesso remoto**.  
  
2.  Nella pagina **Autenticazione** della procedura guidata **Installazione del server di Accesso remoto** selezionare la casella di controllo **Usa certificati computer**, quindi l'autorità di certificazione radice o intermedia che rilascia i certificati nella distribuzione.  
  
Per abilitare l'autenticazione del certificato del computer con Windows PowerShell, usare il cmdlet `Set-DAServer` e specificare il parametro *IPsecRootCertificate* .  
  
## <a name="ip-https-certificates"></a>Certificati IP-HTTPS  
**Errore ricevuto**. Il server DirectAccess utilizza un certificato IP-HTTPS autofirmato. Configurare IP-HTTPS per l'utilizzo di un certificato firmato proveniente da un'autorità di certificazione nota.  
  
**Causa**  
  
Il certificato IP-HTTPS è autofirmato. In una distribuzione multisito non è possibile utilizzare certificati autofirmati.  
  
**Soluzione**  
  
Per selezionare un certificato IP-HTTPS:  
  
1.  Nel riquadro dei dettagli della console di gestione Accesso remoto fare clic su **Modifica** in **Passaggio 2 Server di Accesso remoto**.  
  
2.  Nella pagina **Schede di rete** della procedura guidata **Installazione del server di Accesso remoto**, sotto **Selezionare il certificato utilizzato per autenticare connessioni IP-HTTPS**, verificare che la casella di controllo **Usa certificato autofirmato creato automaticamente da DirectAccess** sia deselezionata e fare clic su **Sfoglia** per selezionare un certificato emesso da una CA attendibile.  
  
## <a name="network-location-server"></a>Server dei percorsi di rete  
  
-   **Problema 1**  
  
    **Errore ricevuto**. DirectAccess è configurato per l'uso di un certificato autofirmato per il server dei percorsi di rete. Configurare il server dei percorsi di rete per l'utilizzo di un certificato firmato rilasciato da una CA.  
  
    **Causa**  
  
    Il server dei percorsi di rete è distribuito nel server di Accesso remoto e utilizza un certificato autofirmato. In una distribuzione multisito non è possibile utilizzare certificati autofirmati.  
  
    **Soluzione**  
  
    Per selezionare un certificato del server dei percorsi di rete:  
  
    1.  Nel riquadro dei dettagli della console di gestione Accesso remoto fare clic su **Modifica** in **Passaggio 3 Server di infrastruttura**.  
  
    2.  Nella pagina **Server dei percorsi di rete** della procedura guidata **Configurazione server di infrastruttura**, sotto **Il server dei percorsi di rete viene distribuito nel server di Accesso remoto**, verificare che la casella di controllo **Usa certificato autofirmato** sia deselezionata e fare clic su **Sfoglia** per selezionare un certificato emesso da una CA globale (enterprise).  
  
-   **Problema 2**  
  
    **Errore ricevuto**. Per distribuire un cluster con bilanciamento del carico di rete o una distribuzione multisito, ottenere un certificato per il server dei percorsi di rete con un nome soggetto diverso dal nome interno del server di accesso remoto.  
  
    **Causa**  
  
    Il nome del soggetto del certificato usato per il sito Web del server dei percorsi di rete è identico al nome interno del server di Accesso remoto. Ciò provocherà problemi di risoluzione dei nomi.  
  
    **Soluzione**  
  
    Ottenere un certificato con un nome soggetto diverso dal nome interno del server di Accesso remoto.  
  
    Per configurare il server dei percorsi di rete  
  
    1.  Nel riquadro dei dettagli della console di gestione Accesso remoto fare clic su **Modifica** in **Passaggio 3 Server di infrastruttura**.  
  
    2.  Nella pagina **Server dei percorsi di rete** della procedura guidata **Configurazione server di infrastruttura**, sotto **Il server dei percorsi di rete viene distribuito nel server di Accesso remoto**, fare clic su **Sfoglia** per selezionare il certificato ottenuto in precedenza. Il certificato deve avere un nome soggetto diverso dal nome interno del server di Accesso remoto.  
  
## <a name="windows-7-client-computers"></a>Computer client Windows 7  
**Avviso ricevuto**. Quando si Abilita multisito, i gruppi di sicurezza configurati per i client DirectAccess non devono contenere computer con Windows 7. Per supportare computer client Windows 7 in una distribuzione multisito, selezionare un gruppo di sicurezza contenente i client per ogni punto di ingresso.  
  
**Causa**  
  
Nella distribuzione di DirectAccess esistente è stato abilitato il supporto client di Windows 7.  
  
**Soluzione**  
  
DirectAccess richiede almeno un gruppo di sicurezza per tutti i computer client Windows 8 e un gruppo di sicurezza per i computer client Windows 7 per ogni punto di ingresso. Ogni computer client deve essere incluso in un solo gruppo di sicurezza. Pertanto, è necessario assicurarsi che il gruppo di sicurezza per i client Windows 8 contenga solo computer che eseguono Windows 8 e che ogni computer client Windows 7 appartenga a un singolo gruppo di sicurezza dedicato per il punto di ingresso pertinente e che nessun client Windows 8 appartenere ai gruppi di sicurezza di Windows 7.  
  
## <a name="active-directory-site"></a>Il sito di Active Directory  
**Errore ricevuto**. Il server < server_name > non è associato a un sito di Active Directory.  
  
**Causa**  
  
DirectAccess non è in grado di determinare il sito Active Directory. Nella console Siti e servizi di Active Directory è possibile configurare diverse subnet di rete e associare ognuna di esse al sito Active Directory pertinente. Questo messaggio di errore può essere visualizzato quando l'indirizzo IP del server di Accesso remoto non appartiene ad alcuna subnet oppure quando la subnet a cui appartiene l'indirizzo IP non è definita con un sito Active Directory.  
  
**Soluzione**  
  
Verificare che sia questo il problema eseguendo il comando `nltest /dsgetsite` nel server di Accesso remoto. In caso affermativo, verrà restituito ERROR_NO_SITENAME. Per risolvere questo problema, verificare che nel controller del dominio sia presente una subnet contenente l'indirizzo IP del server interno e che tale subnet sia definita con un sito Active Directory.  
  
## <a name="saving-server-gpo-settings"></a><a name="SaveGPOSettings"></a>Salvataggio delle impostazioni dell'oggetto Criteri di gruppo Server  
**Errore ricevuto**. Si è verificato un errore durante il salvataggio delle impostazioni di accesso remoto nell'oggetto Criteri di gruppo < GPO_name >.  
  
**Causa**  
  
Non è stato possibile salvare le modifiche all'oggetto Criteri di gruppo del server a causa di problemi di connettività o se si è verificata una violazione di condivisione nel file Registry. pol. ad esempio, un altro utente ha bloccato il file.  
  
**Soluzione**  
  
Verificare che vi sia connettività tra il server di Accesso remoto e il controller di dominio. Se la connettività è presente, verificare nel controller di dominio che il file registry.pol non sia bloccato da un altro utente e, se necessario, terminare la sessione in questione per sbloccare il file.  
  
## <a name="internal-error-occurred"></a><a name="InternalServerError"></a>Si è verificato un errore interno  
**Errore ricevuto**. Si è verificato un errore interno.  
  
**Causa**  
  
Il problema potrebbe essere provocato da una configurazione imprevista della tabella dei punti di ingresso nell'oggetto Criteri di gruppo del client. Ciò può verificarsi quando l'amministratore usa i cmdlet del client DirectAccess per modificare la tabella dei punti di ingresso nell'oggetto Criteri di gruppo del client.  
  
**Soluzione**  
  
Rivedere la configurazione della tabella dei punti di ingresso in tutti gli oggetti Criteri di gruppo del client e correggere eventuali incoerenze nella configurazione multisito tra le differenti istanze degli oggetti Criteri di gruppo del client e la configurazione di DirectAccess. Utilizzare il cmdlet `Get-DaEntryPointTableItem` con il nome dell'oggetto Criteri di gruppo del client per acquisire la tabella dei punti di ingresso sul client. Utilizzare il cmdlet `Get-NetIPHttpsConfiguration` per ottenere tutti i profili IP-HTTPS per tutti i punti di ingresso.  
  
Per ulteriori informazioni, vedere la pagina relativa ai [cmdlet di client DirectAccess in Windows PowerShell](https://technet.microsoft.com/library/hh848426).  
  


