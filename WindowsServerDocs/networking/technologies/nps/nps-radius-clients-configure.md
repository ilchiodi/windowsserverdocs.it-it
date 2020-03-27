---
title: Configurare i client RADIUS
description: In questo argomento vengono fornite informazioni sulla configurazione di client RADIUS per server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: cde37849-ce79-4c26-aa14-cd0ef31cae18
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b7bc75ea81133c91ad7e9883f03c3e32f085b5eb
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315705"
---
# <a name="configure-radius-clients"></a>Configurare i client RADIUS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per configurare i server di accesso alla rete come client RADIUS in server dei criteri di rete.

Quando si aggiunge un nuovo server di accesso alla rete \(server VPN, un punto di accesso wireless, un commutatore di autenticazione o un server di connessione remota\) alla rete, è necessario aggiungere il server come client RADIUS in NPS, quindi configurare il client RADIUS in modo che comunichi con il server dei criteri di rete.

>[!IMPORTANT]
>I computer client e i dispositivi, ad esempio computer portatili, tablet, telefoni e altri computer che eseguono sistemi operativi client, non sono client RADIUS. I client RADIUS sono server di accesso alla rete, ad esempio punti di accesso wireless, commutatori compatibili con 802.1 X, server VPN (Virtual Private Network) e server di connessione remota, perché utilizzano il protocollo RADIUS per comunicare con i server RADIUS, ad esempio server dei criteri di rete \(NPS\) server.

Questo passaggio è necessario anche quando il server dei criteri di gruppo è membro di un gruppo di server RADIUS remoto configurato in un proxy NPS. In questa circostanza, oltre a eseguire i passaggi di questa attività sul proxy server dei criteri di gruppo, è necessario eseguire le operazioni seguenti:

- Nel proxy NPS configurare un gruppo di server RADIUS remoti che contenga il server dei criteri di gruppo.
- Nel server dei criteri di configurazione remoto configurare il proxy NPS come client RADIUS.

Per eseguire le procedure descritte in questo argomento, è necessario disporre di almeno un server di accesso alla rete \(server VPN, un punto di accesso wireless, un commutatore di autenticazione o un server di connessione remota\) o un proxy server dei criteri di rete installato fisicamente nella rete.

## <a name="configure-the-network-access-server"></a>Configurare il server di accesso alla rete

Utilizzare questa procedura per configurare i server di accesso alla rete per l'utilizzo con server dei criteri di rete. Quando si distribuiscono server di accesso alla rete come client RADIUS, è necessario configurare i client in modo che comunicano con il NPSs in cui i server NAS sono configurati come client.

Questa procedura fornisce linee guida generali sulle impostazioni da usare per la configurazione dei NAS. per istruzioni specifiche su come configurare il dispositivo che si sta distribuendo nella rete, vedere la documentazione del prodotto NAS.

### <a name="to-configure-the-network-access-server"></a>Per configurare il server di accesso alla rete

1. In **impostazioni RADIUS**del NAS selezionare **autenticazione RADIUS** sulla porta UDP (User Datagram Protocol) **1812** e **Accounting RADIUS** sulla porta UDP **1813**.
2. In **server di autenticazione** o **server RADIUS**specificare i criteri di dominio in base all'indirizzo IP o al nome di dominio completo (FQDN), a seconda dei requisiti del server NAS. 
3. In **segreto** o **segreto condiviso**digitare una password complessa. Quando si configura il NAS come client RADIUS in server dei criteri di accesso, si userà la stessa password, pertanto non è necessario dimenticarla.
4. Se si usa PEAP o EAP come metodo di autenticazione, configurare il server NAS per l'uso dell'autenticazione EAP.
5. Se si sta configurando un punto di accesso wireless, in **SSID**specificare un identificatore del set di servizi \(SSID\), ovvero una stringa alfanumerica che funge da nome di rete. Questo nome viene trasmesso dai punti di accesso ai client wireless ed è visibile agli utenti presso gli hotspot wireless fidelity \(\) Wi-Fi.
6. Se si sta configurando un punto di accesso wireless, in **802.1 x e WPA**, abilitare **l'autenticazione IEEE 802.1 x** se si desidera distribuire PEAP-MS-CHAP v2, PEAP-TLS o EAP-TLS.

## <a name="add-the-network-access-server-as-a-radius-client-in-nps"></a>Aggiungere il server di accesso alla rete come client RADIUS in server dei criteri di rete

Usare questa procedura per aggiungere un server di accesso alla rete come client RADIUS in server dei criteri di rete. È possibile usare questa procedura per configurare un NAS come client RADIUS usando la console NPS.

Per completare questa procedura è necessaria l'appartenenza al gruppo **Administrators**.

### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Per aggiungere un server di accesso alla rete come client RADIUS in server dei criteri di rete

1. Nel server dei criteri di rete, in Server Manager, fare clic su **strumenti**e quindi su **Server dei criteri di rete**. Si apre la console NPS.
2. Nella console NPS fare doppio clic su **client e server RADIUS**. Fare clic con il pulsante destro del mouse su **client RADIUS**, quindi scegliere **nuovo client RADIUS**. 
3. In **Nuovo Client RADIUS**, verificare che il **attiva questo client RADIUS** casella di controllo è selezionata.
4. In **nuovo client RADIUS**, in **nome descrittivo**, digitare un nome visualizzato per il server NAS. In **indirizzo (IP o DNS)** Digitare l'indirizzo IP del NAS o il nome di dominio completo (FQDN). Se si immette il nome di dominio completo, fare clic su **Verifica** se si desidera verificare che il nome sia corretto e che sia mappato a un indirizzo IP valido. 
5. In **nuovo client RADIUS**, in **Fornitore**, specificare il nome del produttore NAS. Se non si è certi del nome del produttore NAS, selezionare **standard RADIUS**.
6. In **nuovo client RADIUS**, in **segreto condiviso**, eseguire una delle operazioni seguenti:
    - Verificare che sia selezionata l'opzione **manuale** , quindi in **segreto condiviso**digitare la password complessa immessa anche sul NAS. Digitare nuovamente il segreto condiviso in **Conferma segreto condiviso**.
    - Selezionare **genera**, quindi fare clic su **genera** per generare automaticamente un segreto condiviso. Salvare il segreto condiviso generato per la configurazione sul NAS in modo che possa comunicare con il server dei criteri di server.
7. In **nuovo client RADIUS**, in **Opzioni aggiuntive**, se si utilizzano metodi di autenticazione diversi da EAP e PEAP e se il NAS supporta l'utilizzo dell'attributo Message Authenticator, selezionare i messaggi di **richiesta di accesso devono contenere l'attributo Message Authenticator**.
8. Fare clic su **OK**. Il NAS viene visualizzato nell'elenco dei client RADIUS configurati nel server dei criteri di server.

## <a name="configure-radius-clients-by-ip-address-range-in-windows-server-2016-datacenter"></a>Configurare i client RADIUS in base all'intervallo di indirizzi IP in Windows Server 2016 Datacenter

Se si esegue Windows Server 2016 datacenter, è possibile configurare i client RADIUS in NPS in base all'intervallo di indirizzi IP. In questo modo è possibile aggiungere contemporaneamente un numero elevato di client RADIUS, ad esempio punti di accesso wireless, alla console server dei criteri di rete, anziché aggiungere ogni singolo client RADIUS.

Non è possibile configurare i client RADIUS in base all'intervallo di indirizzi IP se si esegue NPS in Windows Server 2016 standard.

Usare questa procedura per aggiungere un gruppo di server di accesso alla rete come client RADIUS tutti configurati con indirizzi IP dallo stesso intervallo di indirizzi IP.

Tutti i client RADIUS nell'intervallo devono usare la stessa configurazione e la stessa chiave privata condivisa.

Per completare questa procedura è necessaria l'appartenenza al gruppo **Administrators**.

### <a name="to-set-up-radius-clients-by-ip-address-range"></a>Per configurare i client RADIUS in base all'intervallo di indirizzi IP

1. Nel server dei criteri di rete, in Server Manager, fare clic su **strumenti**e quindi su **Server dei criteri di rete**. Si apre la console NPS.
2. Nella console NPS fare doppio clic su **client e server RADIUS**. Fare clic con il pulsante destro del mouse su **client RADIUS**, quindi scegliere **nuovo client RADIUS**.
3. In **nuovo client RADIUS**, in **nome descrittivo**, digitare un nome visualizzato per la raccolta di NAS.
4. In **indirizzo \(IP o DNS\)** , digitare l'intervallo di indirizzi IP per i client RADIUS usando il routing tra domini \(notazione CIDR\). Ad esempio, se l'intervallo di indirizzi IP per il NAS è 10.10.0.0, digitare **10.10.0.0/16**.
5. In **nuovo client RADIUS**, in **Fornitore**, specificare il nome del produttore NAS. Se non si è certi del nome del produttore NAS, selezionare **standard RADIUS**.
6. In **nuovo client RADIUS**, in **segreto condiviso**, eseguire una delle operazioni seguenti:
    - Verificare che sia selezionata l'opzione **manuale** , quindi in **segreto condiviso**digitare la password complessa immessa anche sul NAS. Digitare nuovamente il segreto condiviso in **Conferma segreto condiviso**.
    - Selezionare **genera**, quindi fare clic su **genera** per generare automaticamente un segreto condiviso. Salvare il segreto condiviso generato per la configurazione sul NAS in modo che possa comunicare con il server dei criteri di server.
7. In **nuovo client RADIUS**, in **Opzioni aggiuntive**, se si utilizzano metodi di autenticazione diversi da EAP e PEAP e se tutti i NAS supportano l'utilizzo dell'attributo di autenticazione del messaggio, selezionare i **messaggi di richiesta di accesso devono contenere l'attributo Message Authenticator**.
8. Fare clic su **OK**. I NAS vengono visualizzati nell'elenco dei client RADIUS configurati nel server dei criteri di server.

Per ulteriori informazioni, vedere [client RADIUS](nps-radius-clients.md).

Per ulteriori informazioni su NPS, vedere [Server dei criteri di rete (NPS)](nps-top.md).