---
title: Risoluzione dei problemi relativi all'aggiunta di punti di ingresso
description: Questo argomento fa parte della Guida distribuire più server di accesso remoto in una distribuzione multisito di Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: dcc1037f-1a65-4497-99e6-0df9aef748a8
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 9e8f67709e6059b879eab92fdd06609df90cb9a2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858304"
---
# <a name="troubleshooting-adding-entry-points"></a>Risoluzione dei problemi relativi all'aggiunta di punti di ingresso

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento vengono fornite informazioni sulla risoluzione dei problemi relativi al comando `Add-DAEntryPoint`. Per verificare che l'errore ricevuto sia correlato all'aggiunta di un punto di ingresso, controllare che nel registro eventi di Windows sia presente l'ID evento 10067.  
  
## <a name="missing-remoteaccessserver-parameter"></a>Parametro RemoteAccessServer mancante  
**Errore ricevuto**. È necessario specificare un valore per il parametro RemoteAccessServer.  
  
**Causa**  
  
Quando si aggiunge un nuovo punto di ingresso a una distribuzione multisito è necessario specificare il parametro *RemoteAccessServer*, che è il nome del server che si desidera aggiungere come nuovo punto di ingresso.  
  
**Soluzione**  
  
Eseguire il comando specificando come parametro *RemoteAccessServer* il nome del server da aggiungere come punto di ingresso.  
  
## <a name="remote-access-is-not-configured"></a>Accesso remoto non configurato  
**Errore ricevuto**. Accesso remoto non configurato in < server_name >. Specificare il nome di un server che fa parte di una distribuzione multisito.  
  
**Causa**  
  
Accesso remoto non è configurato nel computer specificato dal parametro *ComputerName* o nel computer in cui viene eseguito il comando.  
  
Quando si aggiunge un nuovo punto di ingresso a una distribuzione multisito è necessario specificare due parametri: *ComputerName* e *RemoteAccessServer*. *ComputerName* è il nome di un server che fa già parte della distribuzione multisito, mentre *RemoteAccessServer* è il nome del server che si desidera aggiungere come nuovo punto di ingresso. Se il comando viene eseguito da un computer che fa già parte della distribuzione multisito, il parametro ComputerName non è obbligatorio.  
  
**Soluzione**  
  
Eseguire il comando specificando come parametro *ComputerName* il nome del server già configurato come parte della distribuzione multisito, oppure eseguirlo da un computer che fa già parte della distribuzione multisito.  
  
## <a name="multisite-not-enabled"></a>Distribuzione multisito non abilitata  
**Errore ricevuto**. Prima di eseguire questa operazione, è necessario abilitare una distribuzione multisito. A tale scopo, utilizzare il cmdlet `Enable-DAMultiSite`.  
  
**Causa**  
  
Nel server specificato dal parametro *ComputerName* non è abilitata la distribuzione multisito. Per aggiungere un nuovo punto di ingresso alla distribuzione di Accesso remoto è necessario per prima cosa abilitarla.  
  
**Soluzione**  
  
Abilitare la distribuzione multisito mediante il cmdlet `Enable-DaMultiSite`. Per ulteriori informazioni, vedere l'articolo relativo alla [distribuzione di Accesso remoto multisito](https://technet.microsoft.com/library/hh831664.aspx).  
  
## <a name="ipv6-prefix-issues"></a>Problemi relativi al prefisso IPv6  
  
-   **Problema 1**  
  
    **Errore ricevuto**. IPv6 viene distribuito nella rete interna, ma non è stato specificato un prefisso IPv6 client.  
  
    **Causa**  
  
    IPv6 è distribuito nella rete aziendale ed è richiesto un prefisso IP-HTTPS, ma nel parametro *ClientIPv6Prefix* per il nuovo punto di ingresso non è stato specificato alcun prefisso.  
  
    **Soluzione**  
  
    1.  Assegnare un prefisso IP-HTTPS univoco al nuovo punto di ingresso e assicurarsi che i pacchetti destinati a un indirizzo IP con questo prefisso siano indirizzati al server che si sta aggiungendo.  
  
    2.  Eseguire il cmdlet `Add-DAEntryPoint` e specificare il prefisso IP-HTTPS nel parametro *ClientIPv6Prefix*.  
  
-   **Problema 2**  
  
    **Errore ricevuto**. Il prefisso IPv6 del client è già in uso da parte di un altro punto di ingresso. Specificare un valore alternativo.  
  
    **Causa**  
  
    Il prefisso IP-HTTPS specificato nel parametro *ClientIPv6Prefix* è già utilizzato da un altro punto di ingresso.  
  
    **Soluzione**  
  
    1.  Assegnare un prefisso IP-HTTPS univoco al nuovo punto di ingresso e assicurarsi che i pacchetti destinati a un indirizzo IP con questo prefisso siano indirizzati al server che si sta aggiungendo.  
  
    2.  Eseguire il cmdlet `Add-DAEntryPoint` e specificare il prefisso IP-HTTPS nel parametro *ClientIPv6Prefix*.  
  
## <a name="connectto-address"></a>Indirizzo ConnectTo  
**Errore ricevuto**. L'indirizzo (< connect_to_address >) a cui i client DirectAccess si connettono sul server RemoteAccess è uguale all'indirizzo del server dei percorsi di rete. Specificare un valore alternativo.  
  
**Causa**  
  
L'indirizzo ConnectTo e quello del server dei percorsi di rete sono identici.  
  
**Soluzione**  
  
L'indirizzo ConnectTo deve essere risolvibile su Internet per consentire ai computer client di connettersi su IP-HTTPS. L'indirizzo del server dei percorsi di rete deve essere risolvibile nella rete aziendale ma non su Internet. Assicurarsi che l'indirizzo del server dei percorsi di rete non sia uguale all'indirizzo ConnectTo. Selezionare indirizzi differenti e riprovare.  
  
## <a name="directaccess-or-vpn-already-installed"></a>DirectAccess o VPN già installati  
**Errore ricevuto**. È stata rilevata un'installazione VPN nel server < server_name >. Specificare un server alternativo in cui non è installato Accesso remoto oppure rimuovere la configurazione VPN dal server.  
  
Oppure  
  
Accesso remoto è già installato nel server < server_name >. Specificare un server alternativo in cui non viene eseguito DirectAccess oppure rimuovere la configurazione DirectAccess esistente dal server.  
  
**Causa**  
  
Nel nuovo punto di ingresso è già configurato DirectAccess o VPN. Non è possibile aggiungere un punto di ingresso configurato a una distribuzione multisito.  
  
**Soluzione**  
  
Per aggiungere un server a una distribuzione multisito è necessario installare il ruolo Accesso remoto nel server, ma DirectAccess e VPN non devono essere configurati.  
  
Eseguire il comando assicurandosi che nel server specificato nel parametro *RemoteAccessServer* non sia configurato DirectAccess o VPN.  
  
## <a name="ipsec-root-certificate"></a>Certificato radice IPsec  
**Errore ricevuto**. Impossibile trovare il certificato radice IPsec configurato nel server < server_name >.  
  
**Causa**  
  
Nel server che si sta tentando di aggiungere alla distribuzione non è stato trovato il certificato dell'Autorità di certificazione (CA) radice o intermedia che emette i certificati computer.  
  
**Soluzione**  
  
Nella console di Gestione Accesso remoto, al **Passaggio 2 Server di accesso remoto** fare clic su **Modifica** e controllare che il certificato selezionato sia valido in **Usa certificati computer**, nella pagina **Autenticazione**. Se il certificato è valido, verificare che si trovi nella CA radice attendibile nel server che si desidera aggiungere, quindi riprovare.  
  
> [!NOTE]  
> Il certificato deve essere lo stesso e con la stessa identificazione personale.  
  
Se il certificato non è valido, selezionarne uno valido che sia configurato come CA radice attendibile in tutti i server di accesso remoto.  
  
## <a name="mixing-ipv6-and-ipv4-entry-points"></a>Combinazione di punti di ingresso IPv6 e IPv4  
Quando si installa DirectAccess per la prima volta, la scheda di rete interna viene ispezionata per determinare se la rete contiene solo indirizzi IPv4, (rete solo IPv4), indirizzi sia IPv6 che IPv4 o solo indirizzi IPv6 (rete solo IPv6). Le informazioni vengono utilizzate per determinare il tipo di distribuzione (solo IPv4, IPv6+IPv4 o solo IPv6).  
  
-   **Problema 1**  
  
    **Avviso ricevuto**. Il server di accesso remoto da aggiungere viene configurato con indirizzi sia IPv4 che IPv6. Si tratta di una distribuzione solo IPv4, pertanto Accesso remoto ignorerà gli indirizzi IPv6.  
  
    **Causa**  
  
    Alla prima installazione di questa distribuzione la rete interna è stata rilevata come solo IPv4. In una distribuzione multisito si presuppone che diversi punti di ingresso si trovino in subnet diverse con caratteristiche differenti. Sebbene sia configurata come solo IPv4, la distribuzione può pertanto contenere un punto di ingresso situato in una subnet IPv6+IPv4. Tuttavia, sebbene il punto di ingresso venga aggiunto alla distribuzione, DirectAccess ignorerà gli indirizzi IPv6 configurati nell'interfaccia interna del nuovo punto di ingresso.  
  
    **Soluzione**  
  
    Se l'intera rete interna è configurata con indirizzi IPv6 e IPv4, può essere opportuno optare per una distribuzione IPv6+IPv4, per poter usufruire dei vantaggi delle tecnologie IPv6. Vedere la sezione relativa alla transizione da una rete aziendale IPv4 a una rete aziendale IPv6 + IPv4 in [passaggio 3: pianificare la distribuzione multisito](assetId:///19d49dbf-1786-47bb-ab97-f0458c53d91d).  
  
-   **Problema 2**  
  
    **Errore ricevuto**. Le schede di rete interne dei server accedere remoti in questa distribuzione multisito sono configurate con indirizzi IPv4. Anche il punto di ingresso da aggiungere deve essere configurato con un indirizzo IPv4 sulla scheda di rete interna.  
  
    **Causa**  
  
    Alla prima installazione di questa distribuzione la rete interna è stata rilevata come solo IPv4. Accesso remoto ha rilevato che il punto di ingresso che si sta tentando di aggiungere è configurato solo con indirizzi IPv6 nella rete interna. In una distribuzione solo IPv4 questo non è consentito.  
  
    **Soluzione**  
  
    Se l'intera rete è già configurata con indirizzi IPv6, è necessario passare a una distribuzione IPv6+IPv4 o solo IPv6. Vedere la sezione relativa alla pianificazione della transizione a IPv6 durante la distribuzione di accesso remoto multisito.  
  
-   **Problema 3**  
  
    **Errore ricevuto**. Questo punto di ingresso si trova in una rete IPv4, ma i punti di ingresso precedenti si trovano in una rete IPv6. Connettere il punto di ingresso alla rete IPv6 prima di aggiungerlo alla stessa distribuzione multisito.  
  
    **Causa**  
  
    Alla prima installazione di questa distribuzione la rete interna è stata rilevata come IPv6+IPv4 o solo IPv6. È stato rilevato che il nuovo punto di ingresso che si sta tentando di aggiungere è configurato solo con indirizzi IPv4. Nelle distribuzioni IPv6+IPv4 o solo IPv6 questo non è consentito.  
  
    **Soluzione**  
  
    Configurare il nuovo punto di ingresso con indirizzi IPv6 e quindi aggiungerlo alla distribuzione multisito.  
  
-   **Problema 4**  
  
    **Avviso ricevuto**. La scheda di rete interna nel server di accesso remoto non è configurata con un indirizzo IPv4. DNS64 e NAT64 non verranno configurati sul server. I client DirectAccess possono accedere solo ai server IPv6 interni.  
  
    **Causa**  
  
    Alla prima installazione di questa distribuzione la rete interna è stata rilevata come IPv6+IPv4. In questa modalità di distribuzione sono abilitati DNS64 e NAT64 per consentire ai computer client di accedere ai computer della rete interna configurati solo con indirizzi IPv4.  
  
    Durante l'aggiunta del novo punto di ingresso, Accesso remoto da rilevato che l'interfaccia interna del nuovo computer presenta solo indirizzi IPv6. La configurazione di DNS64 e NAT64 richiede un indirizzo IPv4 per il routing dei pacchetti dal server di accesso remoto al computer solo IPv4. Poiché nel nuovo computer non è presente un indirizzo IPv4, NAT64 e DNS64 non saranno configurati nel server di accesso remoto. I computer client che accedono alla rete aziendale tramite DirectAccess utilizzando questo punto di ingresso non saranno i9n grado di accedere ai server solo IPv4 della rete interna. Per informazioni su come eseguire la transizione a una rete IPv6 + IPv4 o a una rete solo IPv6, vedere la sezione relativa alla pianificazione della transizione a IPv6 quando si distribuisce accesso remoto multisito.  
  
    **Soluzione**  
  
    Aggiungere un indirizzo IPv4 al nuovo server di accesso remoto per assicurare il corretto funzionamento di DNS64 e NAT64.  
  
## <a name="domain-issues-with-the-servergponame"></a>Problemi di dominio con ServerGpoName  
  
-   **Problema 1**  
  
    **Errore ricevuto**. Il dominio specificato nel parametro ServerGpoName < server_GPO > non esiste. Specificare invece il dominio < domain_name >.  
  
    **Causa**  
  
    Nel nome dell'oggetto Criteri di gruppo server inviato dall'amministratore la parte relativa al nome del dominio non è stata trovata.  
  
    **Soluzione**  
  
    Verificare di avere digitato correttamente il nome del dominio. Se l'ortografia del nome del dominio è corretta, riprovare utilizzando il nome di dominio completo (FQDN).  
  
-   **Problema 2**  
  
    **Errore ricevuto**. L'oggetto Criteri di gruppo del server deve trovarsi nel dominio del server di accesso remoto. Specificare il < di dominio domain_name > nel parametro ServerGpoName.  
  
    **Causa**  
  
    Il dominio dell'oggetto Criteri di gruppo server non è lo stesso a cui appartiene il server di accesso remoto.  
  
    **Soluzione**  
  
    L'oggetto Criteri di gruppo server deve trovarsi nello stesso dominio del server di accesso remoto. Utilizzare il nome di dominio del server per l'oggetto Criteri di gruppo server e riprovare.  
  
## <a name="split-brain-dns"></a>DNS "split brain"  
**Avviso ricevuto**. La voce della tabella dei criteri di risoluzione dei nomi del suffisso DNS < DNS_suffix > contiene il nome pubblico utilizzato dai computer client per la connessione al server di accesso remoto. Aggiungere il nome < connect_to_address > come esenzione nella tabella dei criteri di risoluzione dei nomi.  
  
**Causa**  
  
Si sta utilizzando un DNS "split brain". Per consentire ai client di connettersi mediante IP-HTTPS è necessario specificare l'indirizzo ConnectTo selezionato come esenzione nelle regole NRPT.  
  
**Soluzione**  
  
Per una distribuzione multisito, assicurarsi che tutti gli indirizzi ConnectTo dei diversi punti di ingresso siano esentati dalle regole NRPT.  
  
Per esentare un indirizzo nelle regole NRPT:  
  
1.  Nella console di Gestione Accesso remoto, al **Passaggio 3 Server di infrastruttura** fare clic su **Modifica**.  
  
2.  Nella pagina **DNS** della **Configurazione guidata server di infrastruttura** fare doppio clic sulla tabella per immettere un nuovo suffisso del nome.  
  
3.  Nella finestra di dialogo **Indirizzi server DNS** immettere l'indirizzo ConnectTo del punto di ingresso in Suffisso DNS, quindi fare clic su **Applica**.  
  
Se si aggiunge un suffisso del nome senza specificare un indirizzo del server, il suffisso viene trattato come un'esenzione NRPT.  
  
## <a name="saving-server-gpo-settings"></a>Salvataggio delle impostazioni dell'oggetto Criteri di gruppo server  
**Errore ricevuto**. Si è verificato un errore durante il salvataggio delle impostazioni di accesso remoto nell'oggetto Criteri di gruppo < GPO_name >.  
  
Per risolvere questo errore, vedere Salvataggio delle impostazioni dell'oggetto Criteri di gruppo del server in [risoluzione dei problemi abilitati multisito](https://technet.microsoft.com/library/jj591658.aspx)  
  
## <a name="gpo-updates-cannot-be-applied"></a>Impossibile applicare gli aggiornamenti dell'oggetto Criteri di gruppo  
**Avviso ricevuto**. Impossibile applicare gli aggiornamenti dell'oggetto Criteri di gruppo in < server_name >. Le modifiche verranno applicate al prossimo aggiornamento dei criteri.  
  
**Causa**  
  
Si è verificato un errore durante il tentativo di aggiornamento dei criteri nel computer specificato. Le modifiche non verranno pertanto applicate fino al prossimo aggiornamento dei criteri.  
  
**Soluzione**  
  
Per forzare un aggiornamento dei criteri, eseguire `gpupdate /force` nel computer specificato.  
  


