---
title: Passaggio 2 configurare il Server DirectAccess-VPN
description: Questo argomento fa parte della Guida di aggiungere DirectAccess a una distribuzione di accesso remoto esistente (VPN) per Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe221fc9-c7d9-4508-b8a1-000d2515283c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f8a6448661861fdc9f97c66fb130bfc03d0ce72c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446968"
---
#  <a name="step-2-configure-the-directaccess-vpn-server"></a>Passaggio 2 configurare il Server DirectAccess-VPN

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento viene descritto come configurare le impostazioni del client e del server richieste per la distribuzione di base di Accesso remoto con l'Abilitazione guidata DirectAccess.

Nella tabella seguente fornisce una panoramica dei passaggi che è possibile eseguire utilizzando questo argomento.

|Attività       |Descrizione|
|-----------|-----------|
|Configurare i client DirectAccess|Consente di configurare il server di Accesso remoto con i gruppi di sicurezza contenenti i client DirectAccess.|
|Configurare la topologia di rete|Consente di configurare le impostazioni del server di Accesso remoto.|
|Configurare l'elenco di ricerca dei suffissi DNS|Consente di modificare l'elenco di ricerca dei suffissi se opportuno.|
|Configurazione oggetto Criteri di gruppo|Consente di modificare l'oggetto Criteri di gruppo se opportuno.|

## <a name="to-start-the-enable-directacces-wizard"></a>Per avviare l'Abilitazione guidata DirectAccess

1. In Server Manager fare clic su **degli strumenti**, quindi fare clic su **accesso remoto**. L'abilitazione guidata DirectAccess viene avviato automaticamente a meno che non è stato selezionato **non visualizzare più questa schermata**. 

2. Se la procedura guidata non viene avviato automaticamente, fare clic sul nodo del server nell'albero del Routing e accesso remoto e quindi fare clic su **Abilita DirectAccess**.

3. Fare clic su **Avanti**.

## <a name="configure-directaccess-clients"></a>Configurare i client DirectAccess

Per effettuarne il provisioning allo scopo di usare DirectAccess, un computer client deve appartenere al gruppo di sicurezza selezionato. Dopo aver configurato DirectAccess, viene effettuato il provisioning dei computer client nel gruppo di sicurezza in modo da ricevere i criteri di gruppo DirectAccess.

1. Nel **Seleziona gruppi** fare clic su **Aggiungi**.

2. Nella finestra di dialogo **Seleziona gruppi** selezionare i gruppi di sicurezza contenenti i computer client DirectAccess.

3. Selezionare la casella di controllo **Abilita DirectAccess solo per computer mobili** per consentire l'accesso alla rete interna solo ai computer portatili.

4. Selezionare la casella di controllo **Usa Imponi tunneling** per indirizzare tutto il traffico del client (verso la rete interna e Internet) attraverso il server di Accesso remoto.

5. Fare clic su **Avanti**.

## <a name="configure-the-network-topology"></a>Configurare la topologia di rete

Per distribuire Accesso remoto è necessario configurare il server di Accesso remoto con le schede di rete corrette, un URL pubblico per il server di Accesso remoto al quale i computer client possano connettersi (l'indirizzo ConnectTo) e un certificato IP-HTTPS il cui soggetto corrisponda all'indirizzo ConnectTo.

1. Nella pagina **Topologia di rete** fare clic sulla topologia di distribuzione che verrà usata nell'organizzazione. In **Digitare il nome pubblico o l'indirizzo IPv4 usati dai client per connettersi al server di Accesso remoto** immettere il nome pubblico per la distribuzione (che corrisponde al nome soggetto del certificato IP-HTTPS, ad esempio edge1.contoso.com) e quindi fare clic su **Avanti**.

## <a name="configure-the-dns-suffix-search-list"></a>Configurare l'elenco di ricerca dei suffissi DNS

Per i client DNS è possibile configurare un elenco di ricerca dei suffissi del dominio DNS che estenda o riveda le loro capacità di ricerca DNS. Aggiungendo altri suffissi all'elenco è possibile cercare nomi computer brevi non qualificati in più domini DNS specificati. Quindi, se una query DNS non riesce, il servizio Client DNS possa utilizzare questo elenco per aggiungere altri suffissi di nome originale e ripetere le query DNS al server DNS per questi nomi di dominio completi alternativi.

1. Selezionare **Configura client DirectAccess con elenco di ricerca suffissi client DNS** per specificare i suffissi aggiuntivi per le ricerche dei nomi client.

2. Digitare un nuovo suffisso di nome in **nuovo suffisso** e quindi fare clic su **Add**. Inoltre, è possibile modificare l'ordine di ricerca e rimuovere i suffissi da **suffissi di dominio da usare**.

>[NOTA] In uno scenario di spazio di nomi non contiguo \(in cui uno o più computer di dominio ha un suffisso DNS che corrisponde al dominio di Active Directory a cui appartengono i computer\), è necessario assicurarsi che l'elenco di ricerca sia personalizzato e includa tutti i suffissi richiesti. La Configurazione guidata Accesso remoto configura per impostazione predefinita il nome DNS di Active Directory come suffisso DNS primario nel client. L'amministratore dovrà assicurarsi di aggiungere il suffisso DNS usato dai client per la risoluzione dei nomi.

Per i computer e server, il seguente comportamento di ricerca DNS predefinito è predeterminato e usato durante il completamento e la risoluzione dei nomi brevi non qualificati. Quando l'elenco di ricerca suffisso è vuoto o non specificato, il suffisso DNS primario del computer viene aggiunto a breve i nomi non qualificato e una query DNS viene utilizzata per risolvere il FQDN risultante. 

Se questa query ha esito negativo, il computer può provare query aggiuntive per FQDN alternativi mediante l'aggiunta di qualsiasi suffisso DNS specifico della connessione configurata per le connessioni di rete. Se non sono configurati suffissi specifico della connessione o le query per questi nomi di dominio completi specifico della connessione risultante ha esito negativo, il client può iniziare a inviare le query basate su sistematico riduzione del suffisso primario (detto anche per la devoluzione).

Ad esempio, se il suffisso primario è "example.microsoft.com", il processo per la devoluzione possibile riprovare a eseguire query per il nome breve una ricerca nei domini "microsoft.com" e "com".

Quando il suffisso di ricerca elenco non è vuoto e dispone di almeno un suffisso DNS specificato, prova a soddisfa i requisiti e risolvere i nomi brevi di DNS è possibile eseguire la ricerca dei soli FQDN fornito attraverso l'elenco dei suffissi specificato. 

Se non è possibile risolvere le query per tutti gli FQDN formati come risultato dell'aggiunta e della prova di ogni suffisso nell'elenco, il processo di query ha esito negativo, producendo un risultato di "nome non trovato". 

> [!WARNING]
> Se si usa l'elenco di suffissi di dominio, i client continueranno a inviare altre query alternative in base ai diversi nomi di dominio DNS quando una query non riceve risposta né viene risolta. Una volta risolto un nome con una voce nell'elenco dei suffissi, non verranno provate le voci d'elenco non usate. Per questo motivo, è più efficiente ordinare l'elenco collocando per primi i suffissi di dominio più usati.
> 
> Le ricerche del suffisso del nome di dominio vengono effettuate solo quando una voce del nome DNS non è completa. Per completare un nome DNS viene immesso un punto (.) alla fine del nome.

## <a name="gpo-configuration"></a>Configurazione oggetto Criteri di gruppo

Quando si configura accesso remoto, le impostazioni DirectAccess vengono raccolte in oggetti Criteri di gruppo (GPO). 

Nelle **impostazioni di GPO**, sono elencati il nome del server oggetto Criteri di gruppo DirectAccess e il nome oggetto Criteri di gruppo Client. In aggiunta, è possibile modificare le impostazioni di selezione dell'oggetto Criteri di gruppo.

Due oggetti Criteri di gruppo vengono popolati automaticamente con impostazioni DirectAccess e distribuiti in questo modo:

1. **Oggetto Criteri di gruppo del client DirectAccess**. Questo oggetto Criteri di gruppo contiene le impostazioni del client, incluse le impostazioni per la tecnologia di transizione IPv6, le voci NRPT e le regole di sicurezza della connessione di Windows Firewall con sicurezza avanzata. L'oggetto Criteri di gruppo viene applicato ai gruppi di sicurezza specificati per i computer client.

2. **Oggetto Criteri di gruppo del server DirectAccess**. Questo oggetto Criteri di gruppo contiene le impostazioni di configurazione DirectAccess applicate a qualsiasi server configurato come server di accesso remoto nella distribuzione. Contiene anche le regole di sicurezza della connessione di Windows Firewall con sicurezza avanzata.

## <a name="summary"></a>Riepilogo

Dopo aver completato la configurazione di accesso remoto le **riepilogo** viene visualizzato. È possibile modificare le impostazioni configurate o fare clic su **fine** per applicare la configurazione.
