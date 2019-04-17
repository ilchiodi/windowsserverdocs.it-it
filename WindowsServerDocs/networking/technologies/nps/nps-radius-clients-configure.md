---
title: Configurare i client RADIUS
description: Questo argomento fornisce informazioni sulla configurazione di client RADIUS per Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cde37849-ce79-4c26-aa14-cd0ef31cae18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9af3189199c8282394ca34f181a90b4290c55044
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="configure-radius-clients"></a>Configurare i client RADIUS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per configurare il server di accesso di rete come client RADIUS in Criteri di rete.

Quando si aggiunge un nuovo server di accesso di rete \ (server VPN, il punto di accesso wireless, l'autenticazione di commutatore o server di accesso remoto) alla rete, è necessario aggiungere il server come client RADIUS in Criteri di rete e quindi configurare client RADIUS per comunicare con il server dei criteri di rete.

>[!IMPORTANT]
>I computer client e dispositivi, ad esempio computer portatili, Tablet, telefoni e altri computer che eseguono sistemi operativi client, non sono client RADIUS. I client RADIUS sono server di accesso di rete - ad esempio punti di accesso wireless, commutatori che supportano X 802.1, la rete privata virtuale (VPN) Server e i server di accesso remoto - perché usano il protocollo RADIUS per comunicare con server RADIUS, ad esempio i Server dei criteri di rete \(NPS\) Server.

Questo passaggio è necessario anche quando il server dei criteri di rete è un membro di un gruppo di server RADIUS remoto che viene configurato su un proxy dei criteri di rete. In questo caso, oltre a eseguire i passaggi in questa attività sul proxy di criteri di rete, è necessario eseguire le operazioni seguenti:

- Sul proxy di criteri di rete, configurare un gruppo di server RADIUS remoto che contiene il server dei criteri di rete.
- Il server dei criteri di rete remoto, configurare il proxy di criteri di rete come client RADIUS.

Per eseguire le procedure descritte in questo argomento, è necessario disporre di almeno un server di accesso di rete \ (server VPN, il punto di accesso wireless, l'autenticazione di commutatore o server di accesso remoto \) o un proxy dei criteri di rete fisicamente installato nella rete.

## <a name="configure-the-network-access-server"></a>Configurare il Server di accesso di rete

Utilizzare questa procedura per configurare i server di accesso di rete per l'utilizzo con criteri di rete. Quando si distribuisce il server di accesso di rete (NAS) come client RADIUS, è necessario configurare i client per comunicare con il server dei criteri di rete in cui il server NAS sono configurati come client.

Questa procedura fornisce linee guida generali sulle impostazioni che è necessario utilizzare per configurare il server NAS; Per istruzioni specifiche su come configurare il dispositivo che si esegue la distribuzione della rete, vedere la documentazione del prodotto server NAS.

### <a name="to-configure-the-network-access-server"></a>Per configurare il server di accesso di rete

1. Sul server NAS, in **impostazioni RADIUS**selezionare **autenticazione RADIUS** sulla porta di protocollo UDP (User Datagram) **1812** e **accounting RADIUS** sulla porta UDP **1813**.
2. In **server Authentication** o **server RADIUS**, specificare il server dei criteri di rete per indirizzo IP o nome di dominio completo (FQDN), a seconda dei requisiti del server NAS. 
3. In **segreto** o **segreto condiviso**, digitare una password complessa. Quando si configura il server NAS come client RADIUS in Criteri di rete, utilizzare la stessa password, in modo non dimenticare.
4. Se si utilizza il metodo di autenticazione PEAP o EAP, configurare il server NAS per utilizzare l'autenticazione EAP.
5. Se si sta configurando un punto di accesso wireless, in **SSID**, specificare un \(SSID\) Service Set Identifier, ovvero una stringa di caratteri alfanumerica che rappresenta il nome di rete. Questo nome trasmessi dal punti di accesso ai client senza fili ed è visibile agli utenti durante l'hotspot \(Wi-Fi\) fedeltà wireless.
6. Se si sta configurando un punto di accesso wireless, in **802.1 X e WPA**, abilitare **autenticazione IEEE 802.1 X** se si desidera distribuire PEAP-MS-CHAP v2, PEAP-TLS o EAP-TLS.

## <a name="add-the-network-access-server-as-a-radius-client-in-nps"></a>Aggiungere il Server di accesso di rete come un Client RADIUS in Criteri di rete

Utilizzare questa procedura per aggiungere un server di accesso di rete come un client RADIUS in Criteri di rete. È possibile utilizzare questa procedura per configurare un server NAS come client RADIUS utilizzando la console dei criteri di rete.

Per completare questa procedura, è necessario essere un membro del **amministratori** gruppo.

### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Per aggiungere un server di accesso di rete come un client RADIUS in Criteri di rete

1. Nel server dei criteri di rete, in Server Manager, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete**. Apre la console dei criteri di rete.
2. Nella console di criteri di rete, fare doppio clic su **client e server RADIUS**. Fare doppio clic su **client RADIUS**, quindi fare clic su **nuovo Client RADIUS**. 
3. In **nuovo Client RADIUS**, verificare che il **attiva questo client RADIUS** casella di controllo è selezionata.
4. In **nuovo Client RADIUS**, in **nome descrittivo**, digitare un nome per il server NAS. In **indirizzo (IP o DNS)**, digitare il nome di dominio completo (FQDN) o l'indirizzo IP NAS. Se si immette il nome di dominio completo, fare clic su **verifica** se si desidera verificare che il nome sia corretto e che esegue il mapping a un indirizzo IP valido. 
5. In **nuovo Client RADIUS**, in **fornitore**, specificare il nome del produttore NAS. Se non si è certi del nome del produttore NAS, selezionare **standard RADIUS**.
6. In **nuovo Client RADIUS**, in **segreto condiviso**, effettuare una delle operazioni seguenti:
    - Assicurarsi che **manuale** è selezionata, quindi nel **segreto condiviso**, digitare la password complessa che viene inserita anche sul server NAS. Digitare nuovamente il segreto condiviso in **Conferma segreto condiviso**.
    - Selezionare **genera**, quindi fare clic su **genera** per generare automaticamente un segreto condiviso. Salvare il segreto condiviso generato per la configurazione sul server NAS in modo che possa comunicare con il server dei criteri di rete.
7. In **nuovo Client RADIUS**, in **opzioni aggiuntive**, se utilizza qualsiasi metodo di autenticazione EAP e PEAP e, se il server NAS supporta l'utilizzo dell'attributo autenticatore messaggio, selezionare **messaggi di richiesta di accesso devono contenere l'attributo autenticatore messaggio**.
8. Fare clic su **OK**. Il server NAS viene visualizzato nell'elenco dei client RADIUS configurati nel server dei criteri di rete.

## <a name="configure-radius-clients-by-ip-address-range-in-windows-server-2016-datacenter"></a>Configurare i client RADIUS per l'intervallo di indirizzi IP in Windows Server 2016 Datacenter

Se si esegue Windows Server 2016 Datacenter, è possibile configurare client RADIUS in Criteri di rete dall'intervallo di indirizzi IP. In questo modo è possibile aggiungere un numero elevato di client RADIUS (ad esempio punti di accesso wireless) la console dei criteri di rete contemporaneamente, anziché aggiungere singolarmente ogni client RADIUS.

Se si esegue criteri di rete in Windows Server 2016 Standard, è possibile configurare client RADIUS per l'intervallo di indirizzi IP.

Utilizzare questa procedura per aggiungere un gruppo di server di accesso di rete (NAS) come client RADIUS che sono configurati con indirizzi IP dallo stesso intervallo di indirizzi IP.

Tutti i client RADIUS nell'intervallo devono utilizzare la stessa configurazione e il segreto condiviso.

Per completare questa procedura, è necessario essere un membro del **amministratori** gruppo.

### <a name="to-set-up-radius-clients-by-ip-address-range"></a>Per configurare client RADIUS intervallo di indirizzi IP

1. Nel server dei criteri di rete, in Server Manager, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete**. Apre la console dei criteri di rete.
2. Nella console di criteri di rete, fare doppio clic su **client e server RADIUS**. Fare doppio clic su **client RADIUS**, quindi fare clic su **nuovo Client RADIUS**.
3. In **nuovo Client RADIUS**, in **nome descrittivo**, digitare un nome visualizzato per la raccolta di server NAS.
4. In **indirizzo \(IP or DNS\)**, digitare l'intervallo di indirizzi IP per i client RADIUS utilizzando la notazione Classless Inter-Domain Routing \(CIDR\). Ad esempio, se l'intervallo di indirizzi IP per il server NAS 10.10.0.0, digitare **10.10.0.0/16**.
5. In **nuovo Client RADIUS**, in **fornitore**, specificare il nome del produttore NAS. Se non si è certi del nome del produttore NAS, selezionare **standard RADIUS**.
6. In **nuovo Client RADIUS**, in **segreto condiviso**, effettuare una delle operazioni seguenti:
    - Assicurarsi che **manuale** è selezionata, quindi nel **segreto condiviso**, digitare la password complessa che viene inserita anche sul server NAS. Digitare nuovamente il segreto condiviso in **Conferma segreto condiviso**.
    - Selezionare **genera**, quindi fare clic su **genera** per generare automaticamente un segreto condiviso. Salvare il segreto condiviso generato per la configurazione sul server NAS in modo che possa comunicare con il server dei criteri di rete.
7. In **nuovo Client RADIUS**, in **opzioni aggiuntive**, se utilizza qualsiasi metodo di autenticazione EAP e PEAP e, se tutti il server NAS supporta l'utilizzo dell'attributo autenticatore messaggio, selezionare **messaggi di richiesta di accesso devono contenere l'attributo autenticatore messaggio**.
8. Fare clic su **OK**. Il server NAS vengono visualizzati nell'elenco dei client RADIUS configurati nel server dei criteri di rete.

Per ulteriori informazioni, vedere [client RADIUS](nps-radius-clients.md).

Per ulteriori informazioni sui criteri di rete, vedere [Server dei criteri di rete (NPS)](nps-top.md).