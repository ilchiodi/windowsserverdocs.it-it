---
title: Come rilevare, abilitare e disabilitare SMBv1, SMBv2 e SMBv3 in Windows
description: Viene descritto come abilitare e disabilitare il protocollo Server Message Block (SMBv1, SMBv2 e SMBv3) negli ambienti client e server Windows.
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: d6c47843dedaf45842f70d1bb408b59d63c03eb4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815504"
---
# <a name="how-to-detect-enable-and-disable-smbv1-smbv2-and-smbv3-in-windows"></a>Come rilevare, abilitare e disabilitare SMBv1, SMBv2 e SMBv3 in Windows

## <a name="summary"></a>Riepilogo

Questo articolo descrive come abilitare e disabilitare Server Message Block (SMB) versione 1 (SMBv1), SMB versione 2 (SMBv2) e SMB versione 3 (SMBv3) sui componenti client e server SMB. 

> [!IMPORTANT]
> Si consiglia di **non** Disabilitare SMBv2 o SMBv3. Disabilitare SMBv2 o SMBv3 solo come misura temporanea per la risoluzione dei problemi. Non lasciare SMBv2 o SMBv3 disabilitato.  

In Windows 7 e Windows Server 2008 R2, la disabilitazione di SMBv2 disattiva le funzionalità seguenti: 
 
- Composizione della richiesta: consente di inviare più richieste SMB 2 come singola richiesta di rete    
- Letture e scritture più grandi: uso migliore di reti più veloci    
- Caching delle proprietà di file e cartelle: i client conservano le copie locali di cartelle e file    
- Handle durevoli-Consenti alla connessione di riconnettersi in modo trasparente al server in caso di disconnessione temporanea    
- Firma del messaggio migliorata-HMAC SHA-256 sostituisce MD5 come algoritmo di hashing    
- Scalabilità migliorata per la condivisione di file: il numero di utenti, condivisioni e file aperti per server è notevolmente aumentato    
- Supporto per i collegamenti simbolici    
- Modello di leasing blocco opportunistico client: limita i dati trasferiti tra il client e il server, migliorando le prestazioni delle reti a latenza elevata e aumentando la scalabilità del server SMB    
- Supporto MTU elevato: per l'uso completo di 10 gigabye (GB) Ethernet    
- Efficienza energetica migliorata: i client con file aperti a un server possono essere sospesi    

In Windows 8, Windows 8.1, Windows 10, Windows Server 2012 e Windows Server 2016, la disabilitazione di SMBv3 disattiva le funzionalità seguenti (e anche la funzionalità SMBv2 descritta nell'elenco precedente): 
 
- Failover trasparente-i client si riconnettono senza interruzioni nei nodi del cluster durante la manutenzione o il failover    
- Scale Out: accesso simultaneo ai dati condivisi in tutti i nodi del cluster di file     
- Multicanale: aggregazione della larghezza di banda di rete e tolleranza di errore se sono disponibili più percorsi tra client e server  
- SMB diretto: aggiunge il supporto per la rete RDMA per prestazioni molto elevate, con bassa latenza e utilizzo CPU ridotto    
- Crittografia: fornisce la crittografia end-to-end e protegge l'intercettazione su reti non attendibili    
- Leasing di directory: migliora i tempi di risposta delle applicazioni nelle succursali attraverso la memorizzazione nella cache    
- Ottimizzazioni delle prestazioni: ottimizzazioni per I/O di lettura/scrittura casuali di piccole dimensioni

##  <a name="more-information"></a>Altre informazioni

Il protocollo SMBv2 è stato introdotto in Windows Vista e Windows Server 2008.

Il protocollo SMBv3 è stato introdotto in Windows 8 e Windows Server 2012.

Per ulteriori informazioni sulle funzionalità delle funzionalità di SMBv2 e SMBv3, vedere gli articoli seguenti:

[Panoramica di SMB (Server Message Block)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831795(v=ws.11))

[Novità di SMB](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff625695(v=ws.10))  

## <a name="how-to-gracefully-remove-smb-v1-in-windows-81-windows-10-windows-2012-r2-and-windows-server-2016"></a>Come rimuovere correttamente SMB v1 in Windows 8.1, Windows 10, Windows 2012 R2 e Windows Server 2016

#### <a name="windows-server-2012-r2--2016-powershell-methods"></a>Windows Server 2012 R2 & 2016: metodi di PowerShell

##### <a name="smb-v1"></a>SMB v1

- Rilevare 

  ```PowerShell
  Get-WindowsFeature FS-SMB1
  ```

- Disabilitare 

  ```PowerShell
  Disable-WindowsOptionalFeature -Online -FeatureName smb1protocol
  ```

- Abilita 

  ```PowerShell
  Enable-WindowsOptionalFeature -Online -FeatureName smb1protocol
  ```

##### <a name="smb-v2v3"></a>SMB V2/V3

- Rilevare
  
  ```PowerShell
  Get-SmbServerConfiguration | Select EnableSMB2Protocol
  ```

- Disabilitare

  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB2Protocol $false
  ```

- Abilita

  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB2Protocol $true 
  ```

#### <a name="windows-server-2012-r2-and-windows-server-2016-server-manager-method-for-disabling-smb"></a>Windows Server 2012 R2 e Windows Server 2016: Server Manager metodo per disabilitare SMB

##### <a name="smb-v1"></a>SMB v1

![Metodo Server Manager-dashboard](media/detect-enable-and-disable-smbv1-v2-v3-1.png)

#### <a name="windows-81-and-windows-10-powershell-method"></a>Windows 8.1 e Windows 10: Metodo PowerShell

##### <a name="smb-v1-protocol"></a>Protocollo SMB v1

- Rilevare 
  
  ```PowerShell
  Get-WindowsOptionalFeature –Online –FeatureName SMB1Protocol
  ```

- Disabilitare 

  ```PowerShell
  Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
  ```

- Abilita 

  ```PowerShell
  Enable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
  ```

##### <a name="smb-v2v3protocol"></a>Protocollo SMB V2/V3

- Rilevare 
  
  ```PowerShell
  Get-SmbServerConfiguration | Select EnableSMB2Protocol
  ```

- Disabilitare 
  
  ```PowerShell
  Set-SmbServerConfiguration –EnableSMB2Protocol $false
  ```

- Abilita

  ```PowerShell
  Set-SmbServerConfiguration –EnableSMB2Protocol $true
  ```

#### <a name="windows-81-and-windows-10-add-or-remove-programs-method"></a>Windows 8.1 e Windows 10: metodo Aggiungi o Rimuovi programmi

![Metodo Client Add-Remove Programs](media/detect-enable-and-disable-smbv1-v2-v3-2.png)

## <a name="how-to-detect-status-enable-and-disable-smb-protocols-on-the-smb-server"></a>Come rilevare lo stato, abilitare e disabilitare i protocolli SMB nel server SMB

### <a name="for-windows-8-and-windows-server-2012"></a>Per Windows 8 e Windows Server 2012

Windows 8 e Windows Server 2012 introducono il nuovo cmdlet **set-SMBServerConfiguration** di Windows PowerShell. Il cmdlet consente di abilitare o disabilitare i protocolli SMBv1, SMBv2 e SMBv3 sul componente server.  

> [!NOTE]   
> Quando si Abilita o Disabilita SMBv2 in Windows 8 o Windows Server 2012, anche SMBv3 è abilitato o disabilitato. Questo comportamento si verifica perché questi protocolli condividono lo stesso stack.     

Non è necessario riavviare il computer dopo l'esecuzione del cmdlet **set-SMBServerConfiguration** . 

##### <a name="smb-v1-on-smb-server"></a>SMB v1 nel server SMB

- Rilevare 
  
  ```PowerShell
  Get-SmbServerConfiguration | Select EnableSMB1Protocol
  ```

- Disabilitare 

  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB1Protocol $false
  ```

- Abilita 
  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB1Protocol $true
  ```

Per ulteriori informazioni, vedere [archiviazione server in Microsoft](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Stop-using-SMB1/ba-p/425858). 
##### <a name="smb-v2v3-on-smb-server"></a>SMB V2/V3 nel server SMB

- Rilevare

  ```PowerShell
  Get-SmbServerConfiguration | Select EnableSMB2Protocol
  ```

- Disabilitare 
  
  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB2Protocol $false
  ```

- Abilita
  
  ```PowerShell
  Set-SmbServerConfiguration -EnableSMB2Protocol $true
  ```

### <a name="for-windows-7-windows-server-2008-r2-windows-vista-and-windows-server-2008"></a>Per Windows 7, Windows Server 2008 R2, Windows Vista e Windows Server 2008

Per abilitare o disabilitare i protocolli SMB in un server SMB che esegue Windows 7, Windows Server 2008 R2, Windows Vista o Windows Server 2008, usare Windows PowerShell o l'editor del registro di sistema. 

#### <a name="powershell-methods"></a>Metodi di PowerShell

> [!NOTE]
> Questo metodo richiede PowerShell 2,0 o versione successiva di PowerShell.

##### <a name="smb-v1-on-smb-server"></a>SMB v1 nel server SMB

Rilevare

```PowerShell
Get-Item HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters | ForEach-Object {Get-ItemProperty $_.pspath}
```

Default Configuration = Enabled (nessuna chiave del registro di sistema creata), quindi non verrà restituito alcun valore SMB1

Disabilitare

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB1 -Type DWORD -Value 0 –Force
```

Abilita  

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB1 -Type DWORD -Value 1 –Force
```  

**Nota** Dopo aver apportato queste modifiche, è necessario riavviare il computer. Per ulteriori informazioni, vedere [archiviazione server in Microsoft](https://blogs.technet.microsoft.com/filecab/2016/09/16/stop-using-smb1/). 
##### <a name="smb-v2v3-on-smb-server"></a>SMB V2/V3 nel server SMB

Rilevare  

```PowerShell
Get-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters | ForEach-Object {Get-ItemProperty $_.pspath} 
```

Disabilitare

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB2 -Type DWORD -Value 0 –Force  
```

Abilita

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB2 -Type DWORD -Value 1 –Force 
```

> [!NOTE]
> Dopo aver apportato queste modifiche, è necessario riavviare il computer. 

#### <a name="registry-editor"></a>Editor del Registro di sistema

> [!IMPORTANT]
> Segui con attenzione la procedura descritta in questa sezione. Possono verificarsi dei problemi seri se si modifica il Registro di sistema in modo incorretto. Prima di modificarlo, [esegui il backup del Registro di sistema per il ripristino](https://support.microsoft.com/help/322756) nel caso in cui si verifichino problemi.
 
Per abilitare o disabilitare SMBv1 nel server SMB, configurare la chiave del registro di sistema seguente:

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters** e fare clic per selezionarla.

```
Registry entry: SMB1
REG_DWORD: 0 = Disabled
REG_DWORD: 1 = Enabled
Default: 1 = Enabled (No registry key is created)
```

Per abilitare o disabilitare SMBv2 nel server SMB, configurare la chiave del registro di sistema seguente: 

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters** e fare clic per selezionarla.

```
Registry entry: SMB2
REG_DWORD: 0 = Disabled
REG_DWORD: 1 = Enabled
Default: 1 = Enabled (No registry key is created) 
```

> [!NOTE]
> è necessario riavviare il computer dopo avere apportato queste modifiche. 

## <a name="how-to-detect-status-enable-and-disable-smb-protocols-on-the-smb-client"></a>Come rilevare lo stato, abilitare e disabilitare i protocolli SMB nel client SMB

### <a name="for-windows-vista-windows-server-2008-windows-7-windows-server-2008-r2-windows-8-and-windows-server-2012"></a>Per Windows Vista, Windows Server 2008, Windows 7, Windows Server 2008 R2, Windows 8 e Windows Server 2012

> [!NOTE]
> Quando si Abilita o Disabilita SMBv2 in Windows 8 o in Windows Server 2012, anche SMBv3 è abilitato o disabilitato. Questo comportamento si verifica perché questi protocolli condividono lo stesso stack. 

##### <a name="smb-v1-on-smb-client"></a>SMB v1 nel client SMB

- Rileva
  
  ```cmd
  sc.exe qc lanmanworkstation
  ```

- Disabilitare
  
  ```cmd
  sc.exe config lanmanworkstation depend= bowser/mrxsmb20/nsi
  sc.exe config mrxsmb10 start= disabled
  ```

- Abilita

  ```cmd
  sc.exe config lanmanworkstation depend= bowser/mrxsmb10/mrxsmb20/nsi
  sc.exe config mrxsmb10 start= auto
  ```

Per ulteriori informazioni, vedere [archiviazione server in Microsoft](https://blogs.technet.microsoft.com/filecab/2016/09/16/stop-using-smb1/) 
##### <a name="smb-v2v3-on-smb-client"></a>SMB V2/V3 nel client SMB

- Rilevare

  ```cmd
  sc.exe qc lanmanworkstation
  ```

- Disabilitare

  ```cmd
  sc.exe config lanmanworkstation depend= bowser/mrxsmb10/nsi
  sc.exe config mrxsmb20 start= disabled 
  ```

- Abilita

  ```cmd
  sc.exe config lanmanworkstation depend= bowser/mrxsmb10/mrxsmb20/nsi
  sc.exe config mrxsmb20 start= auto
  ```

> [!NOTE]
> - È necessario eseguire questi comandi a un prompt dei comandi con privilegi elevati.    
> - Dopo aver apportato queste modifiche, è necessario riavviare il computer.    
 

## <a name="disable-smbv1-server-with-group-policy"></a>Disabilitare il server SMBv1 con Criteri di gruppo

Questa procedura consente di configurare il nuovo elemento seguente nel registro di sistema:

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters** e fare clic per selezionarla. 

- Voce del registro di sistema: **SMB1** 
- REG_DWORD: **0** = disabilitato   

Per configurare questa impostazione tramite Criteri di gruppo, attenersi alla procedura seguente:
 
1. Aprire il **Console Gestione criteri di gruppo**. Fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo che dovrebbe contenere il nuovo elemento preferenza, quindi fare clic su **Modifica**.

2. Nell'albero della console in **Configurazione computer**espandere la cartella **Preferenze** , quindi espandere la cartella **impostazioni di Windows** .

3. Fare clic con il pulsante destro del mouse sul nodo **Registro di sistema** scegliere **Nuovo** e fare clic su **Elemento Registro di sistema**.

   ![Registro di sistema-nuovo-elemento del registro di sistema](media/detect-enable-and-disable-smbv1-v2-v3-3.png)    
 
Nella finestra di dialogo **Proprietà nuovo registro di sistema**selezionare le opzioni seguenti: 
 
- **Azione**: crea    
- **Hive**: HKEY_LOCAL_MACHINE    
- **Percorso della chiave**: System\CurrentControlSet\Services\LanManServer\Parameters    
- **Nome valore**: SMB1    
- **Tipo di valore**: REG_DWORD    
- **Dati valore**: 0    
 
![Proprietà nuovo registro di sistema-generale](media/detect-enable-and-disable-smbv1-v2-v3-4.png)

In questo modo vengono disabilitati i componenti del server SMBv1. Questo Criteri di gruppo deve essere applicato a tutte le workstation, i server e i controller di dominio necessari nel dominio.

> [!NOTE]
>I  [filtri WMI](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc947846(v=ws.10)) possono inoltre essere impostati in modo da escludere sistemi operativi non supportati o esclusioni selezionate, ad esempio Windows XP.

> [!IMPORTANT]
> Prestare attenzione quando si apportano queste modifiche nei controller di dominio in cui Windows XP o sistemi Linux e di terze parti precedenti (che non supportano SMBv2 o SMBv3) richiedono l'accesso a SYSVOL o ad altre condivisioni file in cui è in corso la disabilitazione di SMB v1.     

## <a name="disable-smbv1-client-with-group-policy"></a>Disabilitare il client SMBv1 con Criteri di gruppo

Per disabilitare il client SMBv1, è necessario aggiornare la chiave del registro di sistema dei servizi per disabilitare l'avvio di **MRxSMB10** e quindi la dipendenza da **MRxSMB10** deve essere rimossa dalla voce per **LanmanWorkstation** , in modo che possa essere avviata normalmente senza che sia necessario avviare **MRxSMB10** per la prima volta.

I valori predefiniti dei due elementi seguenti nel registro di sistema vengono aggiornati e sostituiti: 

**HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\services\mrxsmb10** 

Voce del registro di sistema: **Start** REG_DWORD: **4**= disabled

**HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\LanmanWorkstation** 

Voce del registro di sistema: **DependOnService** REG_MULTI_SZ: **"Bowser", "MRxSmb20", "NSI"**   

> [!NOTE]
> il MRxSMB10 incluso predefinito che ora è stato rimosso come dipendenza.

Per configurare questa impostazione tramite Criteri di gruppo, attenersi alla procedura seguente:
 
1. Aprire il **Console Gestione criteri di gruppo**. Fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo che dovrebbe contenere il nuovo elemento preferenza, quindi fare clic su **Modifica**.

2. Nell'albero della console in **Configurazione computer**espandere la cartella **Preferenze** , quindi espandere la cartella **impostazioni di Windows** .

3. Fare clic con il pulsante destro del mouse sul nodo **Registro di sistema** scegliere **Nuovo** e fare clic su **Elemento Registro di sistema**.    

4. Nella finestra di dialogo **Proprietà nuovo registro di sistema** selezionare le opzioni seguenti: 
 
   - **Azione**: aggiornamento
   - **Hive**: HKEY_LOCAL_MACHINE
   - **Percorso della chiave**: SYSTEM\CurrentControlSet\services\mrxsmb10
   - **Nome valore**: Start
   - **Tipo di valore**: REG_DWORD
   - **Dati valore**: 4
 
   ![Proprietà di avvio-generale](media/detect-enable-and-disable-smbv1-v2-v3-5.png)

5. Rimuovere quindi la dipendenza da **MRxSMB10** che è stata appena disabilitata.

   Nella finestra di dialogo **Proprietà nuovo registro di sistema** selezionare le opzioni seguenti: 
 
   - **Azione**: Replace
   - **Hive**: HKEY_LOCAL_MACHINE
   - **Percorso della chiave**: SYSTEM\CurrentControlSet\Services\LanmanWorkstation
   - **Nome valore**: DependOnService
   - **Tipo di valore**: REG_MULTI_SZ 
   - **Dati valore**:
      - Bowser
      - MRxSmb20
      - NSI
 
   > [!NOTE]
   > Queste tre stringhe non avranno punti elenco (vedere la schermata seguente).

   ![Proprietà di DependOnService](media/detect-enable-and-disable-smbv1-v2-v3-6.png) 

   Il valore predefinito include **MRxSMB10** in molte versioni di Windows. Pertanto, sostituendo tali stringhe con questa stringa multivalore, è in effetti necessario rimuovere **MRxSMB10** come dipendenza per **LanmanServer** e passare da quattro valori predefiniti a questi tre valori precedenti.

   > [!NOTE]
   > Quando si usa Console Gestione Criteri di gruppo, non è necessario usare le virgolette o le virgole. È sufficiente digitare ogni voce su singole righe.

6. Riavviare i sistemi di destinazione per completare la disabilitazione di SMB v1.

### <a name="summary"></a>Riepilogo

Se tutte le impostazioni si trovano nello stesso oggetto Criteri di gruppo (GPO), Criteri di gruppo Management Visualizza le impostazioni seguenti.

![Registro di sistema Editor Gestione Criteri di gruppo](media/detect-enable-and-disable-smbv1-v2-v3-7.png) 

### <a name="testing-and-validation"></a>Test e convalida

Una volta configurate queste impostazioni, consentire la replica e l'aggiornamento dei criteri. Se necessario per i test, eseguire **gpupdate/force** al prompt dei comandi e quindi esaminare i computer di destinazione per assicurarsi che le impostazioni del registro di sistema siano applicate correttamente. Verificare che SMB V2 e SMB V3 funzionino per tutti gli altri sistemi dell'ambiente.

> [!NOTE]
> Non dimenticare di riavviare i sistemi di destinazione.
