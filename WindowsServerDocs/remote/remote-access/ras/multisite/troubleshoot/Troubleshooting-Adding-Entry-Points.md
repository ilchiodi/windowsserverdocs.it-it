---
title: Risoluzione dei problemi relativi all'aggiunta di punti di ingresso
description: Questo argomento fa parte della Guida alla distribuzione di più server di accesso remoto in una distribuzione multisito di Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dcc1037f-1a65-4497-99e6-0df9aef748a8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 51f49364aa4e7a6da6c51b1d8b7da7e37f842190
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282561"
---
# <a name="troubleshooting-adding-entry-points"></a>Risoluzione dei problemi relativi all'aggiunta di punti di ingresso

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento vengono fornite informazioni sulla risoluzione dei problemi relativi al comando `Add-DAEntryPoint`. Per verificare che l'errore ricevuto sia correlato all'aggiunta di un punto di ingresso, controllare che nel registro eventi di Windows sia presente l'ID evento 10067.  
  
## <a name="missing-remoteaccessserver-parameter"></a>Parametro RemoteAccessServer mancante  
**Errore ricevuto**. È necessario specificare un valore per il parametro RemoteAccessServer.  
  
**Causa**  
  
Quando si aggiunge un nuovo punto di ingresso a una distribuzione multisito è necessario specificare il parametro *RemoteAccessServer*, che è il nome del server che si desidera aggiungere come nuovo punto di ingresso.  
  
**Soluzione**  
  
Eseguire il comando specificando come parametro *RemoteAccessServer* il nome del server da aggiungere come punto di ingresso.  
  
## <a name="remote-access-is-not-configured"></a>Accesso remoto non configurato  
**Errore ricevuto**. Accesso remoto non è configurato in < nome_server >. Specificare il nome di un server che fa parte di una distribuzione multisito.  
  
**Causa**  
  
Accesso remoto non è configurato nel computer specificato dal parametro *ComputerName* o nel computer in cui viene eseguito il comando.  
  
Quando si aggiunge un nuovo punto di ingresso a una distribuzione multisito, è necessario specificare due parametri: *ComputerName* e *RemoteAccessServer*. *ComputerName* è il nome di un server che fa già parte della distribuzione multisito, mentre *RemoteAccessServer* è il nome del server che si desidera aggiungere come nuovo punto di ingresso. Se il comando viene eseguito da un computer che fa già parte della distribuzione multisito, il parametro ComputerName non è obbligatorio.  
  
**Soluzione**  
  
Eseguire il comando specificando come parametro *ComputerName* il nome del server già configurato come parte della distribuzione multisito, oppure eseguirlo da un computer che fa già parte della distribuzione multisito.  
  
## <a name="multisite-not-enabled"></a>Distribuzione multisito non abilitata  
**Errore ricevuto**. È necessario abilitare una distribuzione multisito prima di eseguire questa operazione. A tale scopo, utilizzare il cmdlet `Enable-DAMultiSite`.  
  
**Causa**  
  
Nel server specificato dal parametro *ComputerName* non è abilitata la distribuzione multisito. Per aggiungere un nuovo punto di ingresso alla distribuzione di Accesso remoto è necessario per prima cosa abilitarla.  
  
**Soluzione**  
  
Abilitare la distribuzione multisito mediante il cmdlet `Enable-DaMultiSite`. Per ulteriori informazioni, vedere l'articolo relativo alla [distribuzione di Accesso remoto multisito](https://technet.microsoft.com/library/hh831664.aspx).  
  
## <a name="ipv6-prefix-issues"></a>Problemi relativi al prefisso IPv6  
  
-   **Problema 1**  
  
    **Errore ricevuto**. IPv6 è distribuito nella rete interna, ma non è stato specificato un prefisso IPv6 client.  
  
    **Causa**  
  
    IPv6 è distribuito nella rete aziendale ed è richiesto un prefisso IP-HTTPS, ma nel parametro *ClientIPv6Prefix* per il nuovo punto di ingresso non è stato specificato alcun prefisso.  
  
    **Soluzione**  
  
    1.  Assegnare un prefisso IP-HTTPS univoco al nuovo punto di ingresso e assicurarsi che i pacchetti destinati a un indirizzo IP con questo prefisso siano indirizzati al server che si sta aggiungendo.  
  
    2.  Eseguire il cmdlet `Add-DAEntryPoint` e specificare il prefisso IP-HTTPS nel parametro *ClientIPv6Prefix*.  
  
-   **Problema 2**  
  
    **Errore ricevuto**. Il prefisso IPv6 client è già in uso da un altro punto di ingresso. Specificare un valore alternativo.  
  
    **Causa**  
  
    Il prefisso IP-HTTPS specificato nel parametro *ClientIPv6Prefix* è già utilizzato da un altro punto di ingresso.  
  
    **Soluzione**  
  
    1.  Assegnare un prefisso IP-HTTPS univoco al nuovo punto di ingresso e assicurarsi che i pacchetti destinati a un indirizzo IP con questo prefisso siano indirizzati al server che si sta aggiungendo.  
  
    2.  Eseguire il cmdlet `Add-DAEntryPoint` e specificare il prefisso IP-HTTPS nel parametro *ClientIPv6Prefix*.  
  
## <a name="connectto-address"></a>Indirizzo ConnectTo  
**Errore ricevuto**. L'indirizzo (< indirizzo_connect_to >) a cui si connettono i client DirectAccess nel server di accesso remoto è lo stesso indirizzo di rete percorso server. Specificare un valore alternativo.  
  
**Causa**  
  
L'indirizzo ConnectTo e quello del server dei percorsi di rete sono identici.  
  
**Soluzione**  
  
L'indirizzo ConnectTo deve essere risolvibile su Internet per consentire ai computer client di connettersi su IP-HTTPS. L'indirizzo del server dei percorsi di rete deve essere risolvibile nella rete aziendale ma non su Internet. Assicurarsi che l'indirizzo del server dei percorsi di rete non sia uguale all'indirizzo ConnectTo. Selezionare indirizzi differenti e riprovare.  
  
## <a name="directaccess-or-vpn-already-installed"></a>DirectAccess o VPN già installati  
**Errore ricevuto**. È stata rilevata un'installazione VPN nel server di < nome_server >. Specificare un server alternativo in cui non è installato Accesso remoto oppure rimuovere la configurazione VPN dal server.  
  
Oppure  
  
Accesso remoto è già installato sul server < nome_server >. Specificare un server alternativo in cui non viene eseguito DirectAccess oppure rimuovere la configurazione DirectAccess esistente dal server.  
  
**Causa**  
  
Nel nuovo punto di ingresso è già configurato DirectAccess o VPN. Non è possibile aggiungere un punto di ingresso configurato a una distribuzione multisito.  
  
**Soluzione**  
  
Per aggiungere un server a una distribuzione multisito è necessario installare il ruolo Accesso remoto nel server, ma DirectAccess e VPN non devono essere configurati.  
  
Eseguire il comando assicurandosi che nel server specificato nel parametro *RemoteAccessServer* non sia configurato DirectAccess o VPN.  
  
## <a name="ipsec-root-certificate"></a>Certificato radice IPsec  
**Errore ricevuto**. Non può trovarsi il certificato radice IPsec configurato nel server < nome_server >.  
  
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
  
    **Avviso ricevuto**. Il server di accesso remoto da aggiungere è configurato con gli indirizzi IPv4 e IPv6. Si tratta di una distribuzione solo IPv4, pertanto Accesso remoto ignorerà gli indirizzi IPv6.  
  
    **Causa**  
  
    Alla prima installazione di questa distribuzione la rete interna è stata rilevata come solo IPv4. In una distribuzione multisito si presuppone che diversi punti di ingresso si trovino in subnet diverse con caratteristiche differenti. Sebbene sia configurata come solo IPv4, la distribuzione può pertanto contenere un punto di ingresso situato in una subnet IPv6+IPv4. Tuttavia, anche se il punto di ingresso verrà aggiunto alla distribuzione, DirectAccess ignorerà gli indirizzi IPv6 configurati per l'interfaccia interna del nuovo punto di ingresso.  
  
    **Soluzione**  
  
    Se l'intera rete interna è configurata con indirizzi IPv6 e IPv4, può essere opportuno optare per una distribuzione IPv6+IPv4, per poter usufruire dei vantaggi delle tecnologie IPv6. Vedere la sezione "Dedicata alla transizione da un solo IPv4 a una rete aziendale IPv6 + IPv4" in [passaggio 3: Pianificazione della distribuzione multisito](assetId:///19d49dbf-1786-47bb-ab97-f0458c53d91d).  
  
-   **Problema 2**  
  
    **Errore ricevuto**. Le schede di rete interne dei server Accesso remoto in questa distribuzione multisito vengono configurate con indirizzi IPv4. Anche il punto di ingresso da aggiungere deve essere configurato con un indirizzo IPv4 sulla scheda di rete interna.  
  
    **Causa**  
  
    Alla prima installazione di questa distribuzione la rete interna è stata rilevata come solo IPv4. Accesso remoto ha rilevato che il punto di ingresso che si sta tentando di aggiungere è configurato solo con indirizzi IPv6 nella rete interna. In una distribuzione solo IPv4 questo non è consentito.  
  
    **Soluzione**  
  
    Se l'intera rete è già configurata con indirizzi IPv6, è necessario passare a una distribuzione IPv6+IPv4 o solo IPv6. Vedere l'argomento "Pianificazione della transizione a IPv6 nelle distribuzioni di accesso remoto multisito".  
  
-   **Problema 3**  
  
    **Errore ricevuto**. Il punto di ingresso si trova in una rete IPv4, ma i punti di ingresso precedenti si trovano in una rete IPv6. Connettere il punto di ingresso alla rete IPv6 prima di aggiungerlo alla stessa distribuzione multisito.  
  
    **Causa**  
  
    Alla prima installazione di questa distribuzione la rete interna è stata rilevata come IPv6+IPv4 o solo IPv6. È stato rilevato che il nuovo punto di ingresso che si sta tentando di aggiungere è configurato solo con indirizzi IPv4. Nelle distribuzioni IPv6+IPv4 o solo IPv6 questo non è consentito.  
  
    **Soluzione**  
  
    Configurare il nuovo punto di ingresso con indirizzi IPv6 e quindi aggiungerlo alla distribuzione multisito.  
  
-   **Problema 4**  
  
    **Avviso ricevuto**. La scheda di rete interna nel server di accesso remoto non è configurata con un indirizzo IPv4. DNS64 e NAT64 non verranno configurati sul server. I client DirectAccess possono accedere solo ai server IPv6 interni.  
  
    **Causa**  
  
    Alla prima installazione di questa distribuzione la rete interna è stata rilevata come IPv6+IPv4. In questa modalità di distribuzione sono abilitati DNS64 e NAT64 per consentire ai computer client di accedere ai computer della rete interna configurati solo con indirizzi IPv4.  
  
    Durante l'aggiunta del novo punto di ingresso, Accesso remoto da rilevato che l'interfaccia interna del nuovo computer presenta solo indirizzi IPv6. La configurazione di DNS64 e NAT64 richiede un indirizzo IPv4 per il routing dei pacchetti dal server di accesso remoto al computer solo IPv4. Poiché nel nuovo computer non è presente un indirizzo IPv4, NAT64 e DNS64 non saranno configurati nel server di accesso remoto. I computer client che accedono alla rete aziendale tramite DirectAccess utilizzando questo punto di ingresso non saranno i9n grado di accedere ai server solo IPv4 della rete interna. Per informazioni su come eseguire la transizione a una rete IPv6 + IPv4 o a una rete solo IPv6, vedere "Pianificazione della transizione a IPv6 quando si distribuisce accesso remoto multisito".  
  
    **Soluzione**  
  
    Aggiungere un indirizzo IPv4 al nuovo server di accesso remoto per assicurare il corretto funzionamento di DNS64 e NAT64.  
  
## <a name="domain-issues-with-the-servergponame"></a>Problemi di dominio con ServerGpoName  
  
-   **Problema 1**  
  
    **Errore ricevuto**. Il dominio specificato nel parametro ServerGpoName < oggetto_criteri_di_gruppo_server > non esiste. Specificare invece il dominio < nome_dominio >.  
  
    **Causa**  
  
    Nel nome dell'oggetto Criteri di gruppo server inviato dall'amministratore la parte relativa al nome del dominio non è stata trovata.  
  
    **Soluzione**  
  
    Verificare di avere digitato correttamente il nome del dominio. Se l'ortografia del nome del dominio è corretta, riprovare utilizzando il nome di dominio completo (FQDN).  
  
-   **Problema 2**  
  
    **Errore ricevuto**. Il server oggetto Criteri di gruppo deve trovarsi nel dominio del server Accesso remoto. Specificare il dominio < nome_dominio > nel parametro ServerGpoName.  
  
    **Causa**  
  
    Il dominio dell'oggetto Criteri di gruppo server non è lo stesso a cui appartiene il server di accesso remoto.  
  
    **Soluzione**  
  
    L'oggetto Criteri di gruppo server deve trovarsi nello stesso dominio del server di accesso remoto. Utilizzare il nome di dominio del server per l'oggetto Criteri di gruppo server e riprovare.  
  
## <a name="split-brain-dns"></a>DNS "split brain"  
**Avviso ricevuto**. La voce NRPT per il suffisso DNS < suffisso_dns > contiene il nome pubblico utilizzato dai computer client per connettersi al server di accesso remoto. Aggiungere il nome < indirizzo_connect_to > come esenzione nella tabella NRPT.  
  
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
**Errore ricevuto**. Si è verificato un errore durante il salvataggio delle impostazioni di accesso remoto nell'oggetto Criteri di gruppo < nome_oggetto >.  
  
Per risolvere questo errore, vedere il salvataggio delle impostazioni di server oggetto Criteri di gruppo in [risoluzione dei problemi di abilitazione multisito](https://technet.microsoft.com/library/jj591658.aspx).  
  
## <a name="gpo-updates-cannot-be-applied"></a>Impossibile applicare gli aggiornamenti dell'oggetto Criteri di gruppo  
**Avviso ricevuto**. Impossibile applicare gli aggiornamenti di oggetto Criteri di gruppo su < nome_server >. Le modifiche verranno applicate al prossimo aggiornamento dei criteri.  
  
**Causa**  
  
Si è verificato un errore durante il tentativo di aggiornamento dei criteri nel computer specificato. Le modifiche non verranno pertanto applicate fino al prossimo aggiornamento dei criteri.  
  
**Soluzione**  
  
Per forzare un aggiornamento dei criteri, eseguire `gpupdate /force` nel computer specificato.  
  


