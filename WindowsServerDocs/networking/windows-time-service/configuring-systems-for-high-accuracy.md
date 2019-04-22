---
ms.assetid: ''
title: Configurazione dei sistemi per verificarne l'accuratezza elevata
description: Sincronizzazione dell'ora di Windows 10 e Windows Server 2016 è stata notevolmente migliorata.  In condizioni operative ragionevole, sistemi possono essere configurati per gestire (in millisecondi) pari a 1 ms accuratezza o versione successiva (rispetto all'ora UTC).
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: d872252180d49bd41a91e9a8eaf516b834ed242a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814672"
---
# <a name="configuring-systems-for-high-accuracy"></a>Configurazione dei sistemi per verificarne l'accuratezza elevata
>Si applica a: Windows Server 2016 e Windows 10 versione 1607 o successiva

Sincronizzazione dell'ora di Windows 10 e Windows Server 2016 è stata notevolmente migliorata.  In condizioni operative ragionevole, sistemi possono essere configurati per gestire (in millisecondi) pari a 1 ms accuratezza o versione successiva (rispetto all'ora UTC).

Le linee guida seguenti aiuteranno a configurare i sistemi per ottenere precisione elevata.  Questo articolo illustra i requisiti seguenti:

- Sistemi operativi supportati
- Configurazione del sistema 

> [!WARNING]
> **Obiettivi di precisione nei sistemi operativi precedenti**<br>
>Windows Server 2012 R2 e di seguito possono non soddisfare gli stessi obiettivi di elevata precisione. Questi sistemi operativi non supportati per la precisione elevata.
>
>In queste versioni, il servizio ora di Windows soddisfatti i requisiti seguenti:
>
> - Fornito l'accuratezza di tempo necessaria per soddisfare i requisiti di autenticazione Kerberos versione 5.
> - Fornito ora esatta regime di controllo libero per i client Windows e i server aggiunti a una foresta di Active Directory comune.
>
>Le tolleranze maggiore in 2012 R2 e versioni precedenti si trovano all'esterno di specifiche di progettazione del servizio ora di Windows.

## <a name="windows-10-and-windows-server-2016-default-configuration"></a>Configurazione predefinita di Windows Server 2016 e Windows 10

Anche se si supporta fino a 1 ms accuratezza in Windows 10 o Windows Server 2016, la maggior parte dei clienti non richiedono tempo estremamente preciso.

Di conseguenza, il **configurazione predefinita** al fine di soddisfare gli stessi requisiti di sistemi operativi precedenti che sono a:

- Fornire l'accuratezza di tempo necessaria per soddisfare i requisiti di autenticazione Kerberos versione 5.
- Fornire l'ora esatta regime di controllo libero per i client Windows e i server aggiunti a una foresta di Active Directory comune.

## <a name="how-to-configure-systems-for-high-accuracy"></a>Come configurare i sistemi per verificarne l'accuratezza elevata

>[!IMPORTANT]
>**Una nota relativa al supporto dei sistemi altamente accurati**<br>
> La distribuzione end-to-end di ora esatta dall'origine ora autorevole per il dispositivo finale comporta l'accuratezza di tempo.  Tutto ciò che aggiunge assymetry nelle misurazioni lungo il tracciato influiranno negativamente sulle accuratezza influirà l'accuratezza ottenibile nei dispositivi.
>
>Per questo motivo, abbiamo documentato il [limiti di supporto per configurare il servizio ora di Windows per gli ambienti ad alta precisione](support-boundary.md) delineare i requisiti dell'ambiente che devono essere soddisfatto anche per raggiungere obiettivi di elevata precisione.

### <a name="operating-system-requirements"></a>Requisiti del sistema operativo

Le configurazioni di elevata precisione richiedono Windows 10 o Windows Server 2016.  Tutti i dispositivi Windows nella topologia di tempo devono soddisfare questo requisito inclusi superiore strato Windows ora i server e in virtualizzati scenari, l'host Hyper-V che eseguono le macchine virtuali di scadenza. Tutti questi dispositivi devono essere almeno Windows 10 o Windows Server 2016.

Nella figura seguente, vengono eseguite le macchine virtuali che richiedono elevata precisione per Windows 10 o Windows Server 2016.  Analogamente, l'Host Hyper-V in cui si trovano le macchine virtuali e quella del server upstream di Windows deve eseguire Windows Server 2016.

![Topologia ora - 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/Topology2016.png)


>[!TIP] 
>**Determinazione della versione di Windows**<br>
> È possibile eseguire il comando `winver` un prompt dei comandi per verificare il sistema operativo è versione 1607 (o versione successiva) e di Build del sistema operativo è 14393 (o versione successiva) come illustrato di seguito:
>
> ![WINVER - 2016 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/winver2016.png)

### <a name="system-configuration"></a>Configurazione di sistema

Raggiungimento di obiettivi di elevata precisione richiede la configurazione di sistema.  Esistono diversi modi per eseguire questa configurazione, inclusi direttamente nel Registro di sistema o tramite criteri di gruppo.  Altre informazioni per ognuna di queste impostazioni sono disponibili in Windows ora servizio Technical Reference – [strumenti di Windows ora servizio](Windows-Time-Service-Tools-and-Settings.md#windows-time-service-tools).

#### <a name="windows-time-service-startup-type"></a>Servizio ora di Windows di tipo di avvio

Il servizio ora di Windows (W32Time) è necessario eseguire in modo continuo.  A tale scopo, configurare il tipo di avvio del servizio ora di Windows per l'avvio 'Automatico'.

![Configurazione automatica](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/AutomaticService.PNG)

#### <a name="cumulative-one-way-network-latency"></a>Latenza di rete unidirezionale cumulativo

Incertezza e "rumore" potrebbe subentrare man mano che aumenta la latenza di rete.  Di conseguenza, è fondamentale che una latenza di rete sia all'interno di un ragionevole.  I requisiti specifici dipendono l'accuratezza di destinazione e vengono illustrati nel [limiti di supporto per configurare il servizio ora di Windows per gli ambienti ad alta precisione](support-boundary.md) articolo.

Per calcolare la latenza di rete unidirezionale cumulativo, aggiungere i singoli ritardi unidirezionali tra coppie di nodi client e server NTP nella topologia di tempo, inizia con la destinazione e termina con lo strato di elevata precisione 1 origine dell'ora.

Ad esempio: Prendere in considerazione una gerarchia di sincronizzazione di tempo con un'origine estremamente precisa, due server NTP intermediari, A e B e il computer di destinazione in quell'ordine. Per ottenere la latenza di rete cumulativo tra origine e destinazione, misurare la media di andata e ritorno NTP singoli times (RTTs) tra:

- Il server di destinazione e l'ora B
- Ora server B e server ora A
- Ora server a e l'origine

Questa misura può essere ottenuta utilizzando lo strumento w32tm.exe della posta in arrivo.  A tale scopo, effettuare le seguenti operazioni:
<!-- Use PowerShell to import the CSV then average the RTT Column -->

1. Eseguire il calcolo dal server di destinazione e l'ora B.
    
    `w32tm /stripchart /computer:TimeServerB /rdtsc /samples:450 > c:\temp\Target_TsB.csv`

2. Eseguire il calcolo dal server b tempo rispetto a (a) di riferimento ora una.
    
    `w32tm /stripchart /computer:TimeServerA /rdtsc /samples:450 > c:\temp\Target_TsA.csv`

3. Eseguire il calcolo dal server di riferimento ora una rispetto all'origine.
 
4. Successivamente, aggiungere che il medio RoundTripDelay misurato nel passaggio precedente e dividere per 2 per ottenere il ritardo di rete cumulativo tra origine e destinazione.

#### <a name="registry-settings"></a>Impostazioni del Registro di sistema

# <a name="minpollintervaltabminpollinterval"></a>[MinPollInterval](#tab/MinPollInterval)
Configura l'intervallo più piccolo in log2 secondi consentito per il polling di sistema.

|  |  | 
|---------|---------|
|Percorso della chiave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Impostazione    | 6        |
|Risultato | L'intervallo di polling minima è ora 64 secondi. |

Il comando seguente segnala l'ora di Windows per prelevare le impostazioni aggiornate:

`w32tm /config /update`


# <a name="maxpollintervaltabmaxpollinterval"></a>[MaxPollInterval](#tab/MaxPollInterval)
Configura l'intervallo massimo in secondi log2 consentito per il polling del sistema.

|  |  |  
|---------|---------|
|Percorso della chiave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Impostazione    | 6        |
|Risultato | A questo punto, l'intervallo di polling massimo è 64 secondi.  |

Il comando seguente segnala l'ora di Windows per prelevare le impostazioni aggiornate:

`w32tm /config /update`

# <a name="updateintervaltabupdateinterval"></a>[UpdateInterval](#tab/UpdateInterval)
Il numero di tick del clock tra modifiche di correzione di fase.

|  |  |  
|---------|---------|
|Percorso della chiave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config       |
|Impostazione    | 100        |
|Risultato | Il numero di tick del clock tra modifiche di correzione fase è ora 100 segni di graduazione. |

Il comando seguente segnala l'ora di Windows per prelevare le impostazioni aggiornate:

`w32tm /config /update`

# <a name="specialpollintervaltabspecialpollinterval"></a>[SpecialPollInterval](#tab/SpecialPollInterval)
Configura l'intervallo di polling in secondi quando è abilitato lo SpecialInterval 0x1.

|  |  |  
|---------|---------|
|Percorso della chiave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient        |
|Impostazione    | 64        |
|Risultato | L'intervallo di polling è ora 64 secondi. |

Il comando seguente riavvia Windows ora per prelevare le impostazioni aggiornate:

`net stop w32time && net start w32time`

# <a name="frequencycorrectratetabfrequencycorrectrate"></a>[FrequencyCorrectRate](#tab/FrequencyCorrectRate)

|  |  |  
|---------|---------|
|Percorso della chiave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config      |
|Impostazione    | 2        |


---
