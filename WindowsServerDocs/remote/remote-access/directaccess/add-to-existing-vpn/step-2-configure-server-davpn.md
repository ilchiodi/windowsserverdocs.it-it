---
title: 'Passaggio 2: configurare il server DirectAccess-VPN'
description: Questo argomento fa parte della guida aggiungere DirectAccess a una distribuzione di accesso remoto esistente (VPN) per Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: fe221fc9-c7d9-4508-b8a1-000d2515283c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f055a51c93276474bb1b5d4162a914dfa7bbb69a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819634"
---
#  <a name="step-2-configure-the-directaccess-vpn-server"></a>Passaggio 2: configurare il server DirectAccess-VPN

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento viene descritto come configurare le impostazioni del client e del server richieste per la distribuzione di base di Accesso remoto con l'Abilitazione guidata DirectAccess.

Nella tabella seguente viene fornita una panoramica dei passaggi che è possibile completare utilizzando questo argomento.

|Attività       |Descrizione|
|-----------|-----------|
|Configurare i client DirectAccess|Consente di configurare il server di Accesso remoto con i gruppi di sicurezza contenenti i client DirectAccess.|
|Configurare la topologia di rete|Consente di configurare le impostazioni del server di Accesso remoto.|
|Configurare l'elenco di ricerca dei suffissi DNS|Consente di modificare l'elenco di ricerca dei suffissi se opportuno.|
|Configurazione oggetto Criteri di gruppo|Consente di modificare l'oggetto Criteri di gruppo se opportuno.|

## <a name="to-start-the-enable-directacces-wizard"></a>Per avviare l'Abilitazione guidata DirectAccess

1. In Server Manager fare clic su **strumenti**e quindi su **accesso remoto**. L'abilitazione guidata DirectAccess viene avviata automaticamente a meno che non sia stata selezionata l'opzione non **visualizzare più questa schermata**. 

2. Se la procedura guidata non viene avviata automaticamente, fare clic con il pulsante destro del mouse sul nodo server nell'albero routing e accesso remoto e quindi scegliere **Abilita DirectAccess**.

3. Fare clic su **Avanti**.

## <a name="configure-directaccess-clients"></a>Configurare i client DirectAccess

Per effettuarne il provisioning allo scopo di usare DirectAccess, un computer client deve appartenere al gruppo di sicurezza selezionato. Dopo aver configurato DirectAccess, viene effettuato il provisioning dei computer client nel gruppo di sicurezza in modo da ricevere i criteri di gruppo DirectAccess.

1. Nella pagina **Seleziona gruppi** fare clic su **Aggiungi**.

2. Nella finestra di dialogo **Seleziona gruppi** selezionare i gruppi di sicurezza contenenti i computer client DirectAccess.

3. Selezionare la casella di controllo **Abilita DirectAccess solo per computer mobili** per consentire l'accesso alla rete interna solo ai computer portatili.

4. Selezionare la casella di controllo **Usa Imponi tunneling** per indirizzare tutto il traffico del client (verso la rete interna e Internet) attraverso il server di Accesso remoto.

5. Fare clic su **Avanti**.

## <a name="configure-the-network-topology"></a>Configurare la topologia di rete

Per distribuire Accesso remoto è necessario configurare il server di Accesso remoto con le schede di rete corrette, un URL pubblico per il server di Accesso remoto al quale i computer client possano connettersi (l'indirizzo ConnectTo) e un certificato IP-HTTPS il cui soggetto corrisponda all'indirizzo ConnectTo.

1. Nella pagina **Topologia di rete** fare clic sulla topologia di distribuzione che verrà usata nell'organizzazione. In **Digitare il nome pubblico o l'indirizzo IPv4 usati dai client per connettersi al server di Accesso remoto** immettere il nome pubblico per la distribuzione (che corrisponde al nome soggetto del certificato IP-HTTPS, ad esempio edge1.contoso.com) e quindi fare clic su **Avanti**.

## <a name="configure-the-dns-suffix-search-list"></a>Configurare l'elenco di ricerca dei suffissi DNS

Per i client DNS è possibile configurare un elenco di ricerca dei suffissi del dominio DNS che estenda o riveda le loro capacità di ricerca DNS. Aggiungendo altri suffissi all'elenco è possibile cercare nomi computer brevi non qualificati in più domini DNS specificati. Quindi, in caso di errore di una query DNS, il servizio client DNS può utilizzare questo elenco per aggiungere altre terminazioni del suffisso del nome al nome originale e ripetere le query DNS al server DNS per questi FQDN alternativi.

1. Selezionare **Configura client DirectAccess con elenco di ricerca suffissi client DNS** per specificare i suffissi aggiuntivi per le ricerche dei nomi client.

2. Digitare un nuovo nome di suffisso in **nuovo suffisso** e quindi fare clic su **Aggiungi**. Inoltre, è possibile modificare l'ordine di ricerca e rimuovere i suffissi dai **suffissi di dominio da utilizzare**.

>Si noti In uno scenario di spazio dei nomi non contiguo \(in cui uno o più computer del dominio hanno un suffisso DNS che non corrisponde al dominio Active Directory a cui appartengono i computer\), è necessario assicurarsi che l'elenco di ricerca sia personalizzato in modo da includere tutti i suffissi richiesti. La Configurazione guidata Accesso remoto configura per impostazione predefinita il nome DNS di Active Directory come suffisso DNS primario nel client. L'amministratore dovrà assicurarsi di aggiungere il suffisso DNS usato dai client per la risoluzione dei nomi.

Per i computer e i server, il seguente comportamento di ricerca DNS predefinito è predeterminato e utilizzato per il completamento e la risoluzione di nomi brevi e non qualificati. Quando l'elenco di ricerca dei suffissi è vuoto o non specificato, il suffisso DNS primario del computer viene aggiunto a nomi non qualificati brevi e viene usata una query DNS per risolvere il nome di dominio completo risultante. 

Se la query ha esito negativo, il computer può provare query aggiuntive per i nomi di dominio completi alternativi aggiungendo qualsiasi suffisso DNS specifico della connessione configurato per le connessioni di rete. Se non è configurato alcun suffisso specifico della connessione o se le query per questi nomi di dominio completi specifici della connessione risultanti hanno esito negativo, il client può quindi iniziare a ripetere le query in base alla riduzione sistematica del suffisso primario (noto anche come devoluzione).

Se, ad esempio, il suffisso primario è "example.microsoft.com", il processo di devoluzione può ritentare le query per il nome breve cercandolo nei domini "microsoft.com" e "com".

Quando l'elenco di ricerca dei suffissi non è vuoto e è stato specificato almeno un suffisso DNS, i tentativi di qualificazione e risoluzione dei nomi DNS brevi sono limitati alla ricerca di soli FQDN resi possibili dall'elenco di suffissi specificato. 

Se non è possibile risolvere le query per tutti gli FQDN formati come risultato dell'aggiunta e della prova di ogni suffisso nell'elenco, il processo di query ha esito negativo, producendo un risultato di "nome non trovato". 

> [!WARNING]
> Se si usa l'elenco di suffissi di dominio, i client continueranno a inviare altre query alternative in base ai diversi nomi di dominio DNS quando una query non riceve risposta né viene risolta. Una volta risolto un nome con una voce nell'elenco dei suffissi, non verranno provate le voci d'elenco non usate. Per questo motivo, è più efficiente ordinare l'elenco collocando per primi i suffissi di dominio più usati.
> 
> Le ricerche del suffisso del nome di dominio vengono effettuate solo quando una voce del nome DNS non è completa. Per completare un nome DNS viene immesso un punto (.) alla fine del nome.

## <a name="gpo-configuration"></a>Configurazione oggetto Criteri di gruppo

Quando si configura accesso remoto, le impostazioni DirectAccess vengono raccolte in oggetti Criteri di gruppo (GPO). 

In **impostazioni GPO**vengono elencati il nome dell'oggetto Criteri di gruppo del server DirectAccess e il nome dell'oggetto Criteri di gruppo client. In aggiunta, è possibile modificare le impostazioni di selezione dell'oggetto Criteri di gruppo.

Due oggetti Criteri di gruppo vengono popolati automaticamente con le impostazioni di DirectAccess e distribuiti in questo modo:

1. **Oggetto Criteri di gruppo del client DirectAccess**. Questo oggetto Criteri di gruppo contiene le impostazioni del client, incluse le impostazioni per la tecnologia di transizione IPv6, le voci NRPT e le regole di sicurezza della connessione di Windows Firewall con sicurezza avanzata. L'oggetto Criteri di gruppo viene applicato ai gruppi di sicurezza specificati per i computer client.

2. **Oggetto Criteri di gruppo del server DirectAccess**. Questo oggetto Criteri di gruppo contiene le impostazioni di configurazione DirectAccess applicate a qualsiasi server configurato come server di accesso remoto nella distribuzione. Contiene anche le regole di sicurezza della connessione di Windows Firewall con sicurezza avanzata.

## <a name="summary"></a>Riepilogo

Al termine della configurazione di accesso remoto, viene visualizzato il **Riepilogo** . È possibile modificare le impostazioni configurate oppure fare clic su **fine** per applicare la configurazione.
