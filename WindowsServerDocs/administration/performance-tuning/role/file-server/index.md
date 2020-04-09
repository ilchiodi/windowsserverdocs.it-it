---
title: Ottimizzazione delle prestazioni per file server
description: Ottimizzazione delle prestazioni per file server che eseguono Windows Server
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: nedpyle; danlo; dkruse; v-tea
ms.date: 12/12/2019
manager: dcscontentpm
audience: Admin
ms.openlocfilehash: 1236b961f77fe46f19b70a2c48d32f05585bd29c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851844"
---
# <a name="performance-tuning-for-file-servers"></a>Ottimizzazione delle prestazioni per file server

È consigliabile selezionare l'hardware appropriato per soddisfare il carico previsto dei file server, considerando il carico medio, il picco di carico, la capacità, i piani di crescita e i tempi di risposta. I colli di bottiglia dell'hardware limitano l'efficacia dell'ottimizzazione a livello software.

## <a name="general-tuning-parameters-for-clients"></a>Parametri di ottimizzazione generali per i client

Le impostazioni REG\_DWORD seguenti del Registro di sistema possono incidere sulle prestazioni dei computer client che interagiscono con i file server SMB:

-   **ConnectionCountPerNetworkInterface**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\ConnectionCountPerNetworkInterface
    ```

    Si applica a Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012

    È consigliabile usare il valore predefinito, ovvero 1. L'intervallo valido è compreso tra 1 e 16. Il numero massimo di connessioni, per interfaccia, da stabilire con un server per le interfacce non RSS.


-   **ConnectionCountPerRssNetworkInterface**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\ConnectionCountPerRssNetworkInterface
    ```

    Si applica a Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012

    È consigliabile usare il valore predefinito, ovvero 4. L'intervallo valido è compreso tra 1 e 16. Il numero massimo di connessioni, per interfaccia, da stabilire con un server per le interfacce RSS.

-   **ConnectionCountPerRdmaNetworkInterface**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\ConnectionCountPerRdmaNetworkInterface
    ```

    Si applica a Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012

    È consigliabile usare il valore predefinito, ovvero 2. L'intervallo valido è compreso tra 1 e 16. Il numero massimo di connessioni, per interfaccia, da stabilire con un server per le interfacce RDMA.

-   **MaximumConnectionCountPerServer**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\MaximumConnectionCountPerServer
    ```

    Si applica a Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012

    Il valore predefinito è 32 e l'intervallo valido è compreso tra 1 e 64. Il numero massimo di connessioni da stabilire con un server singolo che esegue Windows Server 2012 per tutte le interfacce.

-   **DormantDirectoryTimeout**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DormantDirectoryTimeout
    ```

    Si applica a Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012

    Il valore predefinito è 600 secondi. L'intervallo di tempo massimo durante il quale gli handle di directory del server vengono mantenuti aperti con lease di directory.

-   **FileInfoCacheLifetime**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileInfoCacheLifetime
    ```

    Si applica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    Il valore predefinito è 10 secondi. Il periodo che deve trascorrere prima del timeout della cache con le informazioni dei file.

-   **DirectoryCacheLifetime**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheLifetime
    ```

    Si applica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    Il valore predefinito è 10 secondi. Si tratta del timeout della cache delle directory.

    > [!NOTE]  
    > Questo parametro controlla la memorizzazione nella cache dei metadati di directory in assenza di lease di directory.
     
     > [!NOTE]  
     > Un problema noto in Windows 10, versione 1803, influisce sulla capacità di Windows 10 di memorizzare nella cache directory di grandi dimensioni. Dopo l'aggiornamento di un computer a Windows 10, versione 1803, si accede a una condivisione di rete che contiene migliaia di file e cartelle e si apre un documento presente in tale condivisione. Durante queste due operazioni si riscontrano ritardi significativi.
     >  
     > Per risolvere il problema, installare Windows 10, versione 1809 o successiva.
     >  
     > Come soluzione alternativa, impostare **DirectoryCacheLifetime** su **0**.
     >  
     > Il problema interessa le edizioni seguenti di Windows 10:  
     > - Windows 10 Enterprise, versione 1803
     > - Windows 10 Pro for Workstations, versione 1803
     > - Windows 10 Pro Education, versione 1803
     > - Windows 10 Professional, versione 1803
     > - Windows 10 Education, versione 1803
     > - Windows 10 Home, versione 1803
   
-   **DirectoryCacheEntrySizeMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheEntrySizeMax
    ```

    Si applica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    Il valore predefinito è 64 KB. Si tratta della dimensione massima delle voci della cache delle directory.

-   **FileNotFoundCacheLifetime**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileNotFoundCacheLifetime
    ```

    Si applica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    Il valore predefinito è 5 secondi. Il periodo che deve trascorrere prima del timeout della cache dei file non trovati.

-   **CacheFileTimeout**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\CacheFileTimeout
    ```

    Si applica a Windows 8.1, Windows 8, Windows Server 2012, Windows Server 2012 R2 e Windows 7

    Il valore predefinito è 10 secondi. Questa impostazione controlla l'intervallo di tempo (in secondi) durante il quale il redirector mantiene i dati memorizzati nella cache per un file dopo che l'ultimo handle al file viene chiuso da un'applicazione.

-   **DisableBandwidthThrottling**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DisableBandwidthThrottling
    ```

    Si applica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    Il valore predefinito è 0. Per impostazione predefinita, il redirector SMB limita la velocità effettiva nelle connessioni di rete ad alta latenza, in alcuni casi per evitare timeout relativi alla rete. Se si imposta su 1 questo valore del Registro di sistema, si disabilita tale limitazione, rendendo possibile una velocità effettiva di trasferimento file superiore su connessioni di rete ad alta latenza.

-   **DisableLargeMtu**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DisableLargeMtu
    ```

    Si applica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    Il valore predefinito è 0 solo per Windows 8. In Windows 8 il redirector SMB trasferisce i payload fino a 1 MB per ogni richiesta, migliorando la velocità di trasferimento dei file. Se si imposta su 1 questo valore del Registro di sistema, si limitano le dimensioni della richiesta a 64 KB. È consigliabile valutare l'impatto di questa impostazione prima di applicarla.

-   **RequireSecuritySignature**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\RequireSecuritySignature
    ```

    Si applica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    Il valore predefinito è 0 ed equivale a disabilitare la firma SMB. Se si modifica questo valore impostandolo su 1, si abilita la firma SMB per tutte le comunicazioni SMB, impedendole con i computer in cui la firma SMB è disabilitata. La firma SMB può far aumentare i costi relativi alla CPU e i round trip di rete, ma facilita il blocco degli attacchi man-in-the-middle. Se la firma SMB non è necessaria, assicurarsi che questo valore del Registro di sistema sia impostato su 0 in tutti i client e server. 
    
    Per altre informazioni, vedere [The Basics of SMB Signing](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/) (Introduzione alla firma SMB).

-   **FileInfoCacheEntriesMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileInfoCacheEntriesMax
    ```

    Si applica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    Il valore predefinito è 64 e l'intervallo valido è compreso tra 1 e 65536. Questo valore viene usato per determinare la quantità di metadati dei file che può essere memorizzata nella cache dal client. Aumentando il valore, è possibile ridurre il traffico di rete e migliorare le prestazioni quando si accede a un numero elevato di file.

-   **DirectoryCacheEntriesMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheEntriesMax
    ```

    Si applica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    Il valore predefinito è 16 e l'intervallo valido è compreso tra 1 e 4096. Questo valore viene usato per determinare la quantità di informazioni delle directory che può essere memorizzata nella cache dal client. Aumentando il valore, è possibile ridurre il traffico di rete e migliorare le prestazioni quando si accede a directory di grandi dimensioni.

-   **FileNotFoundCacheEntriesMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileNotFoundCacheEntriesMax
    ```

    Si applica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    Il valore predefinito è 128 e l'intervallo valido è compreso tra 1 e 65536. Questo valore viene usato per determinare la quantità di informazioni relative ai nomi dei file che può essere memorizzata nella cache dal client. Aumentando il valore, è possibile ridurre il traffico di rete e migliorare le prestazioni quando si accede a un numero elevato di nomi di file.

-   **MaxCmds**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\MaxCmds
    ```

    Si applica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    Il valore predefinito è 15. Questo parametro limita il numero di richieste in sospeso in una sessione. Aumentando il valore, si usa più memoria, ma le prestazioni possono migliorare grazie all'abilitazione di una pipeline di richieste più profonda. Aumentando il valore insieme a MaxMpxCt, è anche possibile eliminare gli errori che si verificano a causa di un numero elevato di richieste di file a lungo termine in sospeso, ad esempio chiamate FindFirstChangeNotification. Questo parametro non incide sulle connessioni a server SMB 2.0.

-   **DormantFileLimit**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DormantFileLimit
    ```

    Si applica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

    Il valore predefinito è 1023. Questo parametro specifica il numero massimo di file che è consigliabile lasciare aperti in una risorsa condivisa dopo che l'applicazione ha chiuso il file.

### <a name="client-tuning-example"></a>Esempio di ottimizzazione di client

I parametri di ottimizzazione generali per i computer client consentono di ottimizzare un computer per l'accesso a condivisioni file remote, in particolare su alcune reti ad alta latenza quali succursali, comunicazioni tra data center, sedi centrali e reti a banda larga mobili. Le impostazioni non sono ottimali o appropriate per tutti i computer. È consigliabile valutare l'impatto delle singole impostazioni prima di applicarle.

| Parametro                   | Value | Valore predefinito |
|-----------------------------|-------|---------|
| DisableBandwidthThrottling  | 1     | 0       |
| FileInfoCacheEntriesMax     | 32768 | 64      |
| DirectoryCacheEntriesMax    | 4096  | 16      |
| FileNotFoundCacheEntriesMax | 32768 | 128     |
| MaxCmds                     | 32768 | 15      |

 

A partire da Windows 8, è possibile configurare molte di queste impostazioni SMB usando i cmdlet **Set-SmbClientConfiguration** e **Set-SmbServerConfiguration** di Windows PowerShell. Le impostazioni solo del Registro di sistema possono essere configurate anche usando Windows PowerShell.

```
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" RequireSecuritySignature -Value 0 -Force
```
