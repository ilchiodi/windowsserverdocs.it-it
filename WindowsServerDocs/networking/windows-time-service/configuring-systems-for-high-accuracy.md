---
ms.assetid: ''
title: Configurazione dei sistemi per l'accuratezza elevata
description: La sincronizzazione dell'ora in Windows 10 e Windows Server 2016 è stata migliorata in modo significativo.  In condizioni operative ragionevoli i sistemi possono essere configurati in modo da mantenere un'accuratezza di 1 ms (millisecondo) o superiore (rispetto all'ora UTC).
author: eross-msft
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 8cdded0eb0dc663d352011fb1a6765a2ed358764
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315030"
---
# <a name="configuring-systems-for-high-accuracy"></a>Configurazione dei sistemi per l'accuratezza elevata
>Si applica a: Windows Server 2016 e Windows 10, versione 1607 o successive

La sincronizzazione dell'ora in Windows 10 e Windows Server 2016 è stata migliorata in modo significativo.  In condizioni operative ragionevoli i sistemi possono essere configurati in modo da mantenere un'accuratezza di 1 ms (millisecondo) o superiore (rispetto all'ora UTC).

Le indicazioni seguenti consentono di configurare i sistemi per ottenere un'accuratezza elevata.  Questo articolo illustra i requisiti seguenti:

- Sistemi operativi supportati
- Configurazione del sistema 

> [!WARNING]
> **Obiettivi di accuratezza dei sistemi operativi precedenti**<br>
>Windows Server 2012 R2 e versioni precedenti non possono soddisfare gli stessi obiettivi di accuratezza elevata. Questi sistemi operativi non sono supportati per l'accuratezza elevata.
>
>In queste versioni il servizio Ora di Windows soddisfa i requisiti seguenti:
>
> - Accuratezza dell'ora necessaria per soddisfare i requisiti di autenticazione Kerberos versione 5.
> - Limitata accuratezza dell'ora per i client e i server Windows aggiunti a una foresta Active Directory comune.
>
>Una maggiore tolleranza in 2012 R2 e versioni precedenti non rientra nella specifica di progettazione del servizio Ora di Windows.

## <a name="windows-10-and-windows-server-2016-default-configuration"></a>Configurazione predefinita di Windows 10 e Windows Server 2016

Sebbene sia supportata l'accuratezza fino a 1 ms in Windows 10 o Windows Server 2016, la maggior parte dei clienti non richiede un'accuratezza elevata dell'ora.

Di conseguenza, la **configurazione predefinita** ha lo scopo di soddisfare gli stessi requisiti dei sistemi operativi precedenti, ovvero:

- Accuratezza dell'ora necessaria per soddisfare i requisiti di autenticazione Kerberos versione 5.
- Limitata accuratezza dell'ora per i client e i server Windows aggiunti a una foresta Active Directory comune.

## <a name="how-to-configure-systems-for-high-accuracy"></a>Come configurare i sistemi per l'accuratezza elevata

>[!IMPORTANT]
>**Nota sul supporto dei sistemi con accuratezza elevata**<br>
> L'accuratezza dell'ora comporta la distribuzione end-to-end dell'ora esatta dall'origine ora autorevole al dispositivo finale.  Qualsiasi elemento che aggiunga asimmetria nelle misurazioni lungo questo percorso influirà negativamente sull'accuratezza che può essere ottenuta nei dispositivi.
>
>Per questo motivo nell'articolo [Limiti di supporto per configurare il servizio Ora di Windows per gli ambienti con accuratezza elevata](support-boundary.md) sono stati illustrati i requisiti ambientali aggiuntivi che devono essere soddisfatti per raggiungere obiettivi di accuratezza elevata.

### <a name="operating-system-requirements"></a>Requisiti per il sistema operativo

Le configurazioni con accuratezza elevata richiedono Windows 10 o Windows Server 2016.  Tutti i dispositivi Windows nella topologia temporale devono soddisfare questo requisito, inclusi i server di riferimento ora di Windows di strato superiore e, negli scenari virtualizzati, gli host Hyper-V che eseguono macchine virtuali dipendenti dall'ora. Tutti questi dispositivi devono eseguire almeno Windows 10 o Windows Server 2016.

Nella figura mostrata di seguito le macchine virtuali che richiedono un'accuratezza elevata eseguono Windows 10 o Windows Server 2016.  Analogamente, anche l'host Hyper-V in cui risiedono le macchine virtuali e il server di riferimento ora di Windows upstream devono eseguire Windows Server 2016.

![Topologia temporale - 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/Topology2016.png)


>[!TIP] 
>**Determinazione della versione di Windows**<br>
> Puoi eseguire il comando `winver` al prompt dei comandi per verificare che la versione del sistema operativo sia 1607 (o successiva) e che la build del sistema operativo sia 14393 (o successiva), come illustrato di seguito:
>
> ![Winver - 2016 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/winver2016.png)

### <a name="system-configuration"></a>Configurazione di sistema

Per raggiungere obiettivi di accuratezza elevata è necessario eseguire la configurazione del sistema.  Esistono diversi modi per eseguire questa configurazione, ad esempio direttamente nel Registro di sistema o tramite criteri di gruppo.  Per altre informazioni su ognuna di queste impostazioni, vedi le informazioni tecniche di riferimento relative al servizio Ora di Windows in [Strumenti del servizio Ora di Windows](Windows-Time-Service-Tools-and-Settings.md#windows-time-service-tools).

#### <a name="windows-time-service-startup-type"></a>Tipo di avvio del servizio Ora di Windows

Il servizio Ora di Windows (W32Time) deve essere continuamente in esecuzione.  A tale scopo, configura l'avvio automatico come tipo di avvio del servizio Ora di Windows.

![Configurazione automatica](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/AutomaticService.PNG)

#### <a name="cumulative-one-way-network-latency"></a>Latenza di rete unidirezionale cumulativa

Quando aumenta la latenza di rete, si verificano problemi di incertezza delle misurazioni e "rumore".  Di conseguenza, è fondamentale definire un limite ragionevole di latenza di rete.  I requisiti specifici dipendono dall'accuratezza della destinazione e sono descritti nell'articolo [Limiti di supporto per configurare il servizio Ora di Windows per gli ambienti con accuratezza elevata](support-boundary.md).

Per calcolare la latenza di rete unidirezionale cumulativa, aggiungi i singoli ritardi unidirezionali tra coppie di nodi client-server NTP nella topologia temporale, iniziando con la destinazione e terminando con l'origine ora di uno strato 1 di accuratezza elevata.

Ad esempio: considera una gerarchia di sincronizzazione dell'ora con un'origine con accuratezza elevata, due server NTP intermedi A e B e il computer di destinazione in tale ordine. Per ottenere la latenza di rete cumulativa tra la destinazione e l'origine, misura la media dei singoli tempi di round trip NTP (RTT) tra:

- La destinazione e il server di riferimento ora B
- Il server di riferimento ora B e il server di riferimento ora A
- Il server di riferimento ora A e l'origine

Questa misurazione può essere ottenuta usando lo strumento w32tm.exe incorporato.  A tale scopo:

1. Esegui il calcolo dalla destinazione e dal server di riferimento ora B.
    
    `w32tm /stripchart /computer:TimeServerB /rdtsc /samples:450 > c:\temp\Target_TsB.csv`

2. Esegui il calcolo dal server di riferimento ora B a fronte del server di riferimento ora A (a cui viene fatto riferimento).
    
    `w32tm /stripchart /computer:TimeServerA /rdtsc /samples:450 > c:\temp\Target_TsA.csv`

3. Esegui il calcolo dal server di riferimento ora A a fronte dell'origine.
 
4. Successivamente, aggiungi la media RoundTripDelay misurata nel passaggio precedente e dividi per due per ottenere il ritardo di rete cumulativo tra destinazione e origine.

#### <a name="registry-settings"></a>Impostazioni del Registro di sistema

# <a name="minpollinterval"></a>[MinPollInterval](#tab/MinPollInterval)
Configura l'intervallo minimo in secondi log2 consentito per il polling del sistema.

|  |  | 
|---------|---------|
|Percorso della chiave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Impostazione    | 6        |
|Risultato | L'intervallo di polling minimo è ora di 64 secondi. |

Il comando seguente segnala al servizio Ora di Windows di selezionare le impostazioni aggiornate:

`w32tm /config /update`


# <a name="maxpollinterval"></a>[MaxPollInterval](#tab/MaxPollInterval)
Configura l'intervallo massimo di secondi log2 consentito per il polling del sistema.

|  |  |  
|---------|---------|
|Percorso della chiave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Impostazione    | 6        |
|Risultato | L'intervallo di polling massimo è ora di 64 secondi.  |

Il comando seguente segnala al servizio Ora di Windows di selezionare le impostazioni aggiornate:

`w32tm /config /update`

# <a name="updateinterval"></a>[UpdateInterval](#tab/UpdateInterval)
Il numero di tick del clock tra le regolazioni di correzione di fase.

|  |  |  
|---------|---------|
|Percorso della chiave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config       |
|Impostazione    | 100        |
|Risultato | Il numero di tick del clock tra le regolazioni di correzione di fase corrisponde ora a 100 tick. |

Il comando seguente segnala al servizio Ora di Windows di selezionare le impostazioni aggiornate:

`w32tm /config /update`

# <a name="specialpollinterval"></a>[SpecialPollInterval](#tab/SpecialPollInterval)
Configura l'intervallo di polling in secondi quando è abilitato il flag SpecialInterval 0x1.

|  |  |  
|---------|---------|
|Percorso della chiave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient        |
|Impostazione    | 64        |
|Risultato | L'intervallo di polling è ora di 64 secondi. |

Il comando seguente riavvia il servizio Ora di Windows per selezionare le impostazioni aggiornate:

`net stop w32time && net start w32time`

# <a name="frequencycorrectrate"></a>[FrequencyCorrectRate](#tab/FrequencyCorrectRate)

|  |  |  
|---------|---------|
|Percorso della chiave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config      |
|Impostazione    | 2        |


---
