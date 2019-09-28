---
ms.assetid: ''
title: Configurazione dei sistemi per la precisione elevata
description: La sincronizzazione dell'ora in Windows 10 e Windows Server 2016 è stata notevolmente migliorata.  In condizioni operative ragionevoli, i sistemi possono essere configurati in modo da mantenere l'accuratezza 1 ms (millisecondi) o superiore (rispetto all'ora UTC).
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: b7cd256fdbbdbe7432e5b5d5b16254314132560f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405198"
---
# <a name="configuring-systems-for-high-accuracy"></a>Configurazione dei sistemi per la precisione elevata
>Si applica a: Windows Server 2016 e Windows 10 versione 1607 o successiva

La sincronizzazione dell'ora in Windows 10 e Windows Server 2016 è stata notevolmente migliorata.  In condizioni operative ragionevoli, i sistemi possono essere configurati in modo da mantenere l'accuratezza 1 ms (millisecondi) o superiore (rispetto all'ora UTC).

Le linee guida seguenti consentono di configurare i sistemi per ottenere una maggiore precisione.  Questo articolo illustra i requisiti seguenti:

- Sistemi operativi supportati
- Configurazione del sistema 

> [!WARNING]
> **Obiettivi di accuratezza dei sistemi operativi precedenti**<br>
>Windows Server 2012 R2 e versioni precedenti non possono soddisfare gli stessi obiettivi di precisione elevata. Questi sistemi operativi non sono supportati per un'accuratezza elevata.
>
>In queste versioni, il servizio ora di Windows soddisfa i requisiti seguenti:
>
> - È stata fornita la precisione temporale necessaria per soddisfare i requisiti di autenticazione Kerberos versione 5.
> - Fornito tempo molto accurato per i client e i server Windows aggiunti a una foresta Active Directory comune.
>
>Maggiori tolleranze in 2012 R2 e versioni precedenti non rientrano nella specifica di progettazione del servizio ora di Windows.

## <a name="windows-10-and-windows-server-2016-default-configuration"></a>Configurazione predefinita di Windows 10 e Windows Server 2016

Sebbene sia supportata l'accuratezza fino a 1 ms in Windows 10 o Windows Server 2016, la maggior parte dei clienti non richiede un tempo molto accurato.

Di conseguenza, la **configurazione predefinita** è progettata per soddisfare gli stessi requisiti dei sistemi operativi precedenti, ovvero:

- Fornire l'accuratezza del tempo necessaria per soddisfare i requisiti di autenticazione Kerberos versione 5.
- Fornire un tempo di accuratezza per i client e i server Windows aggiunti a una foresta di Active Directory comune.

## <a name="how-to-configure-systems-for-high-accuracy"></a>Come configurare i sistemi per un'accuratezza elevata

>[!IMPORTANT]
>**Nota sul supporto dei sistemi altamente accurati**<br>
> L'accuratezza dell'ora comporta la distribuzione end-to-end dell'ora esatta dall'origine dell'ora autorevole al dispositivo finale.  Qualsiasi elemento che aggiunge asimmetria nelle misurazioni lungo questo percorso influirà negativamente sull'accuratezza che può influire sull'accuratezza ottenibile nei dispositivi.
>
>Per questo motivo, è stato documentato il [limite di supporto per configurare il servizio ora di Windows per gli ambienti ad alta precisione](support-boundary.md) che descrivono i requisiti ambientali che devono essere soddisfatti anche per raggiungere obiettivi di accuratezza elevata.

### <a name="operating-system-requirements"></a>Requisiti del sistema operativo

Le configurazioni con accuratezza elevata richiedono Windows 10 o Windows Server 2016.  Tutti i dispositivi Windows nella topologia temporale devono soddisfare questo requisito, inclusi i server ora di Windows di strato superiore e negli scenari virtualizzati, gli host Hyper-V che eseguono le macchine virtuali con distinzione di tempo. Tutti questi dispositivi devono essere almeno Windows 10 o Windows Server 2016.

Nell'illustrazione mostrata di seguito, le macchine virtuali che richiedono una precisione elevata eseguono Windows 10 o Windows Server 2016.  Analogamente, l'host Hyper-V in cui risiedono le macchine virtuali e il server ora di Windows upstream deve eseguire anche Windows Server 2016.

![Topologia temporale-1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/Topology2016.png)


>[!TIP] 
>**Determinazione della versione di Windows**<br>
> È possibile eseguire il comando `winver` al prompt dei comandi per verificare che la versione del sistema operativo sia 1607 (o superiore) e che la build del sistema operativo sia 14393 (o successiva), come illustrato di seguito:
>
> ![Winver-2016 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/winver2016.png)

### <a name="system-configuration"></a>Configurazione di sistema

Per raggiungere obiettivi di accuratezza elevata è necessaria la configurazione di sistema.  Esistono diversi modi per eseguire questa configurazione, incluso direttamente nel registro di sistema o tramite criteri di gruppo.  Per ulteriori informazioni su ognuna di queste impostazioni, vedere riferimento tecnico per il servizio ora di Windows- [strumenti del servizio ora di Windows](Windows-Time-Service-Tools-and-Settings.md#windows-time-service-tools).

#### <a name="windows-time-service-startup-type"></a>Tipo di avvio del servizio ora di Windows

Il servizio ora di Windows (W32Time) deve essere eseguito in modo continuo.  A tale scopo, configurare il tipo di avvio del servizio ora di Windows sull'avvio automatico.

![Configurazione automatica](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/AutomaticService.PNG)

#### <a name="cumulative-one-way-network-latency"></a>Latenza di rete unidirezionale cumulativa

L'incertezza di misurazione e il "rumore" si insinuano in quanto aumenta la latenza di rete.  Di conseguenza, è fondamentale che una latenza di rete si trovi all'interno di un limite ragionevole.  I requisiti specifici dipendono dall'accuratezza della destinazione e sono descritti nel limite del [supporto per configurare il servizio ora di Windows per gli ambienti ad alta precisione](support-boundary.md) .

Per calcolare la latenza di rete unidirezionale cumulativa, aggiungere i singoli ritardi unidirezionali tra coppie di nodi client-server NTP nella topologia temporale, iniziando con la destinazione e terminando con l'origine dell'ora di uno strato di accuratezza elevata 1.

Esempio: Si consideri una gerarchia di sincronizzazione dell'ora con un'origine altamente accurata, due server NTP intermedi A e B e il computer di destinazione in tale ordine. Per ottenere la latenza di rete cumulativa tra la destinazione e l'origine, misurare la media dei singoli tempi di round trip NTP (RTTs) tra:

- Il server di destinazione e l'ora B
- Time Server B e Time Server A
- Server temporale A e origine

Questa misurazione può essere ottenuta utilizzando lo strumento W32tm. exe della posta in arrivo.  A tale scopo, effettuare le seguenti operazioni:

1. Eseguire il calcolo dal server di destinazione e dell'ora B.
    
    `w32tm /stripchart /computer:TimeServerB /rdtsc /samples:450 > c:\temp\Target_TsB.csv`

2. Eseguire il calcolo da time server b rispetto a (a cui viene fatto riferimento) time server a.
    
    `w32tm /stripchart /computer:TimeServerA /rdtsc /samples:450 > c:\temp\Target_TsA.csv`

3. Eseguire il calcolo da Time Server a rispetto all'origine.
 
4. Successivamente, aggiungere la media RoundTripDelay misurata nel passaggio precedente e dividere per 2 per ottenere il ritardo di rete cumulativo tra destinazione e origine.

#### <a name="registry-settings"></a>Impostazioni del Registro di sistema

# <a name="minpollintervaltabminpollinterval"></a>[MinPollInterval](#tab/MinPollInterval)
Configura l'intervallo più piccolo in log2 secondi consentiti per il polling del sistema.

|  |  | 
|---------|---------|
|Posizione chiave     | HKLM: \ SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Impostazione    | 6        |
|Risultato | L'intervallo di polling minimo è ora di 64 secondi. |

Il comando seguente segnala il tempo di Windows per selezionare le impostazioni aggiornate:

`w32tm /config /update`


# <a name="maxpollintervaltabmaxpollinterval"></a>[MaxPollInterval](#tab/MaxPollInterval)
Configura l'intervallo massimo in log2 secondi consentiti per il polling del sistema.

|  |  |  
|---------|---------|
|Posizione chiave     | HKLM: \ SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Impostazione    | 6        |
|Risultato | L'intervallo di polling massimo è ora di 64 secondi.  |

Il comando seguente segnala il tempo di Windows per selezionare le impostazioni aggiornate:

`w32tm /config /update`

# <a name="updateintervaltabupdateinterval"></a>[UpdateInterval](#tab/UpdateInterval)
Il numero di tick del clock tra le regolazioni della correzione della fase.

|  |  |  
|---------|---------|
|Posizione chiave     | HKLM: \ SYSTEM\CurrentControlSet\Services\W32Time\Config       |
|Impostazione    | 100        |
|Risultato | Il numero di tick del clock tra le regolazioni della correzione della fase è ora 100 tick. |

Il comando seguente segnala il tempo di Windows per selezionare le impostazioni aggiornate:

`w32tm /config /update`

# <a name="specialpollintervaltabspecialpollinterval"></a>[SpecialPollInterval](#tab/SpecialPollInterval)
Configura l'intervallo di polling in secondi quando il flag SpecialInterval 0x1 è abilitato.

|  |  |  
|---------|---------|
|Posizione chiave     | HKLM: \ SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient        |
|Impostazione    | 64        |
|Risultato | L'intervallo di polling è ora di 64 secondi. |

Il comando seguente riavvia l'ora di Windows per selezionare le impostazioni aggiornate:

`net stop w32time && net start w32time`

# <a name="frequencycorrectratetabfrequencycorrectrate"></a>[FrequencyCorrectRate](#tab/FrequencyCorrectRate)

|  |  |  
|---------|---------|
|Posizione chiave     | HKLM: \ SYSTEM\CurrentControlSet\Services\W32Time\Config      |
|Impostazione    | 2        |


---
