---
title: Configurare i client RADIUS
description: In questo argomento vengono fornite informazioni sulla configurazione di client RADIUS per il Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cde37849-ce79-4c26-aa14-cd0ef31cae18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 851bdb0677a17567f81a2c331baad595fe40436a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839492"
---
# <a name="configure-radius-clients"></a>Configurare i client RADIUS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per configurare il server di accesso di rete come computer client RADIUS in Criteri di rete.

Quando si aggiunge un nuovo server di accesso di rete \(server VPN, il punto di accesso wireless, l'autenticazione di switch, o un server remoto\) alla rete, è necessario aggiungere il server come client RADIUS in Criteri di rete e quindi configurare il client RADIUS per comunicare con i criteri di rete.

>[!IMPORTANT]
>I computer client e dispositivi, ad esempio computer portatili, Tablet, telefoni e altri computer che eseguono sistemi operativi client, non sono client RADIUS. I client RADIUS sono server di accesso di rete - ad esempio punti di accesso wireless, commutatori che supportano 802.1X, la rete privata virtuale (VPN) Server e server di accesso remoto - perché usano il protocollo RADIUS per comunicare con server RADIUS, ad esempio criteri di rete Server \(NPS\) server.

Questo passaggio è necessario anche quando i criteri di rete è un membro di un gruppo di server RADIUS remoto che è configurato su un proxy di criteri di rete. In questa circostanza, oltre a eseguire i passaggi descritti in questa attività nel proxy di criteri di rete, è necessario eseguire quanto segue:

- Sul proxy dei criteri di rete e configurare un gruppo di server RADIUS remoto che contiene i criteri di rete.
- Nei criteri di rete remota, configurare il proxy di criteri di rete come client RADIUS.

Per eseguire le procedure descritte in questo argomento, è necessario disporre di almeno un server di accesso di rete \(server VPN, il punto di accesso wireless, l'autenticazione di switch, o un server remoto\) o il proxy di criteri di rete fisicamente installati in rete.

## <a name="configure-the-network-access-server"></a>Configurare il Server di accesso di rete

Utilizzare questa procedura per configurare i server di accesso di rete per l'uso con criteri di rete. Quando si distribuisce il server di accesso di rete (NAS) come client RADIUS, è necessario configurare i client per comunicare con il NPSs in cui il server NAS sono configurati come client.

Questa procedura vengono fornite linee guida generali sulle impostazioni che deve usare per configurare il server NAS; Per istruzioni specifiche su come configurare il dispositivo che si esegue la distribuzione nella rete, vedere la documentazione del prodotto server NAS.

### <a name="to-configure-the-network-access-server"></a>Per configurare il server di accesso di rete

1. Sul server NAS, nella **le impostazioni RADIUS**, selezionare **autenticazione RADIUS** sulla porta di protocollo UDP (User Datagram) **1812** e **accounting RADIUS**sulla porta UDP **1813**.
2. Nelle **server Authentication** oppure **server RADIUS**, specificare i criteri di rete tramite indirizzo IP o nome di dominio completo (FQDN), a seconda dei requisiti del server NAS. 
3. Nelle **segreto** oppure **segreto condiviso**, digitare una password complessa. Quando si configura il server NAS come client RADIUS in Criteri di rete, si userà la stessa password, in modo da non dimenticare.
4. Se si utilizza PEAP o EAP come un metodo di autenticazione, configurare il server NAS per usare l'autenticazione EAP.
5. Se si sta configurando un punto di accesso wireless, in **SSID**, specificare un Service Set Identifier \(SSID\), ovvero una stringa alfanumerica che funge da nome di rete. Questo nome viene trasmesso da punti di accesso ai client senza fili ed è visibile agli utenti la fedeltà senza fili \(Wi-Fi\) HotSpot.
6. Se si sta configurando un punto di accesso wireless, in **802.1X e WPA**, abilitare **autenticazione IEEE 802.1x** se si desidera distribuire PEAP-MS-CHAP v2, PEAP-TLS o EAP-TLS.

## <a name="add-the-network-access-server-as-a-radius-client-in-nps"></a>Aggiungere il Server di accesso di rete come Client RADIUS in Criteri di rete

Utilizzare questa procedura per aggiungere un server di accesso di rete come client RADIUS in Criteri di rete. È possibile utilizzare questa procedura per configurare un server NAS come client RADIUS utilizzando la console Criteri di rete.

Per completare questa procedura, è necessario essere un membro del **amministratori** gruppo.

### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Per aggiungere un server di accesso di rete come client RADIUS in Criteri di rete

1. Su NPS, in Server Manager fare clic su **degli strumenti**, quindi fare clic su **Server dei criteri di rete**. Apre la console Criteri di rete.
2. Nella console Criteri di rete, fare doppio clic su **client RADIUS e server**. Fare doppio clic su **client RADIUS**, quindi fare clic su **nuovo Client RADIUS**. 
3. In **Nuovo Client RADIUS**, verificare che il **attiva questo client RADIUS** casella di controllo è selezionata.
4. Nelle **nuovo Client RADIUS**, nel **soprannome**, digitare un nome visualizzato per il server NAS. Nelle **indirizzo (IP o DNS)**, digitare il nome di dominio completo (FQDN) o l'indirizzo IP NAS. Se si immette il nome di dominio completo, fare clic su **Verify** se si desidera verificare che il nome sia corretto e che esegue il mapping a un indirizzo IP valido. 
5. Nelle **nuovo Client RADIUS**, nel **fornitore**, specificare il nome del produttore NAS. Se non si è certi del nome del produttore NAS, selezionare **standard RADIUS**.
6. Nelle **nuovo Client RADIUS**, nel **segreto condiviso**, effettuare una delle operazioni seguenti:
    - Assicurarsi che **manuali** è selezionata, quindi nel **segreto condiviso**, digitare la password complessa che viene inserita anche sul server NAS. Digitare nuovamente il segreto condiviso in **Conferma segreto condiviso**.
    - Selezionare **genera**, quindi fare clic su **genera** per generare automaticamente un segreto condiviso. Salvare il segreto condiviso generato per la configurazione sul server NAS, in modo che possa comunicare con i criteri di rete.
7. Nelle **nuovo Client RADIUS**, nel **opzioni aggiuntive**, se si usa qualsiasi metodo di autenticazione EAP e PEAP e, se il server NAS supporta l'uso dell'attributo autenticatore messaggio, selezionare **Messaggi di richiesta di accesso devono contenere l'attributo autenticatore messaggio**.
8. Fare clic su **OK**. Il server NAS viene visualizzato nell'elenco dei computer client RADIUS configurato su NPS.

## <a name="configure-radius-clients-by-ip-address-range-in-windows-server-2016-datacenter"></a>Configurare i client RADIUS dall'intervallo di indirizzi IP in Windows Server 2016 Datacenter

Se si esegue Windows Server 2016 Datacenter, è possibile configurare client RADIUS in Criteri di rete dall'intervallo di indirizzi IP. In questo modo è possibile aggiungere un numero elevato di client RADIUS (ad esempio punti di accesso wireless) nella console Criteri di rete in una sola volta, invece di aggiungere singolarmente ogni client RADIUS.

Se si eseguono criteri di rete in Windows Server 2016 Standard, è possibile configurare client RADIUS dall'intervallo di indirizzi IP.

Utilizzare questa procedura per aggiungere un gruppo di server di accesso di rete (NAS) come client RADIUS che sono configurati con indirizzi IP dallo stesso intervallo di indirizzi IP.

Tutti i client RADIUS compreso nell'intervallo devono usare la stessa configurazione e il segreto condiviso.

Per completare questa procedura, è necessario essere un membro del **amministratori** gruppo.

### <a name="to-set-up-radius-clients-by-ip-address-range"></a>Configurare i client RADIUS, intervallo di indirizzi IP

1. Su NPS, in Server Manager fare clic su **degli strumenti**, quindi fare clic su **Server dei criteri di rete**. Apre la console Criteri di rete.
2. Nella console Criteri di rete, fare doppio clic su **client RADIUS e server**. Fare doppio clic su **client RADIUS**, quindi fare clic su **nuovo Client RADIUS**.
3. Nelle **nuovo Client RADIUS**, nel **soprannome**, digitare un nome visualizzato per la raccolta di server NAS.
4. Nelle **indirizzi \(IP o DNS\)**, digitare l'intervallo di indirizzi IP per i client RADIUS utilizzando Classless Interdomain Routing \(CIDR\) notation. Ad esempio, se l'intervallo di indirizzi IP per il server NAS è indirizzo 10.10.0.0, digitare **10.10.0.0/16**.
5. Nelle **nuovo Client RADIUS**, nel **fornitore**, specificare il nome del produttore NAS. Se non si è certi del nome del produttore NAS, selezionare **standard RADIUS**.
6. Nelle **nuovo Client RADIUS**, nel **segreto condiviso**, effettuare una delle operazioni seguenti:
    - Assicurarsi che **manuali** è selezionata, quindi nel **segreto condiviso**, digitare la password complessa che viene inserita anche sul server NAS. Digitare nuovamente il segreto condiviso in **Conferma segreto condiviso**.
    - Selezionare **genera**, quindi fare clic su **genera** per generare automaticamente un segreto condiviso. Salvare il segreto condiviso generato per la configurazione sul server NAS, in modo che possa comunicare con i criteri di rete.
7. Nelle **nuovo Client RADIUS**, nel **opzioni aggiuntive**, se si usa qualsiasi metodo di autenticazione EAP e PEAP e, se tutti i server NAS supporta l'uso dell'attributo autenticatore messaggio, selezionare  **I messaggi di richiesta di accesso devono contenere l'attributo autenticatore messaggio**.
8. Fare clic su **OK**. Il server NAS vengono visualizzati nell'elenco dei computer client RADIUS configurato su NPS.

Per altre informazioni, vedere [client RADIUS](nps-radius-clients.md).

Per altre informazioni sui criteri di rete, vedere [Strumentazione gestione Windows (NPS, Network Policy Server)](nps-top.md).