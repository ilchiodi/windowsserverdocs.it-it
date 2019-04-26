---
title: Supporto ISV recapito aggiornamenti Express
description: Argomento Windows Server Update Service (WSUS) - come indipendente dal Software fornitori (ISV) possa configurare il recapito di aggiornamento Express tramite WSUS
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: b891f61ff2c930591c33805d0e3bc595ebf196f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850492"
---
#<a name="express-update-delivery-isv-support"></a>Supporto ISV recapito aggiornamenti Express

>Si applica a: Windows 10, Windows Server 2016

Download aggiornamento Windows 10 può essere estesa perché tutti i pacchetti contiene tutte le correzioni rilasciate in precedenza per garantire la coerenza e la semplicità.  

Fin dalla versione 7, Windows è stato in grado di ridurre le dimensioni del download degli aggiornamenti di Windows con una funzione denominata [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2), e anche se i dispositivi consumer supportano per impostazione predefinita, i dispositivi Windows 10 enterprise è richiesto Windows Server Update Services (WSUS) per sfruttare i vantaggi di Express.

## <a name="how-microsoft-supports-express"></a>Supporto di Microsoft per Express

- **Express in Windows Server Update Services autonomi**

    Distribuzione aggiornamento rapido è già [disponibile in tutte le versioni supportate di WSUS](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx).

- **Express su dispositivi direttamente connessi a Windows Update** 

    I dispositivi consumer supportano download Express: utilizzare il client di Windows Update (WU) per analizzare, scaricare e installare gli aggiornamenti. Durante la fase di download, il client di Windows Update richiede i pacchetti Express e scarica gli intervalli di byte appropriato.

-  **Anche i dispositivi aziendali gestiti tramite [Windows Update for Business](https://technet.microsoft.com/itpro/windows/manage/waas-manage-updates-wufb)** usufruiscono del supporto del recapito degli aggiornamenti Express senza la necessità apportare modifiche di configurazione.

##<a name="how-isvs-can-take-advantage-of-express"></a>Come parte degli ISV può sfruttare i vantaggi di Express

Fornitori di software indipendenti possono utilizzare WSUS e il client di Windows Update per supportare il recapito di aggiornamento rapido. Microsoft consiglia di tre passaggi, ciascuno descritto più dettagliatamente nelle sezioni seguenti:

1.  [**Configurare WSUS**](#BKMK_1)

    Server WSUS è necessario per analizzare & le sincronizzazioni degli aggiornamenti (ulteriori informazioni sono reperibili [qui](https://technet.microsoft.com/library/dn800972(v=ws.11).aspx))

2.  [**Specificare e popolare una cache di file ISV**](#BKMK_2)

    Una cache di file ISV è consigliata per ospitare il contenuto dell'aggiornamento, che include i file CAB di aggiornamento (file con estensione cab) e (PsF file) di pacchetti di Express.

3.  [**Configurare un agente client di ISV per indirizzare le operazioni client di Windows Update**](#BKMK_3)

>[!NOTE]
>Richiede la versione Cumulative Update per Windows 10 versione 1607 in (o dopo) gennaio 2017 ([KB3213986 (14393.693 di Build del sistema operativo)](https://support.microsoft.com/en-us/help/4009938/january-10-2017-kb3213986-os-build-14393-693) da installare.
    
   - L'agente client ISV determina gli aggiornamenti da approvare e quando Scarica e installa aggiornamenti
   - Il client WU determina gli intervalli di byte per il download e avvia la richiesta di download

### <a name="BKMK_1"></a>Passaggio 1: Configurare WSUS

WSUS funge da interfaccia di Windows Update e gestisce tutti i metadati che descrivono i pacchetti di Express che devono essere scaricati. Se si desidera distribuire, vedere [ **Panoramica di Windows Server Update Services 3.0 SP2**](https://technet.microsoft.com/library/dd939931(v=ws.10).aspx). Dopo la distribuzione di WSUS, la considerazione principale è o meno archiviare il contenuto degli aggiornamenti in locale nel server WSUS. Quando si configura WSUS, è consigliabile non archiviare gli aggiornamenti in locale. Ciò presuppone che si disponga già di software indirizza la distribuzione di questi pacchetti nell'ambiente in uso. Per altre informazioni su come configurare l'archiviazione locale di Windows Server Update Services, vedere [ **Determine Where to Store aggiornamenti**](https://technet.microsoft.com/library/cc720494(v=ws.10).aspx).

### <a name="BKMK_2"></a>Passaggio 2: Specificare e popolare la Cache del File di ISV 

####<a name="specify-the-isv-file-cache"></a>Specificare la Cache del File di ISV

Nuovo lato client Criteri di gruppo e gestione dei dispositivi mobili (MDM) le impostazioni descritte nel [ **riferimento al provider di servizio di configurazione** ](https://msdn.microsoft.com/en-us/windows/hardware/commercialize/customize/mdm/configuration-service-provider-reference) definire il percorso della cache del file ISV.

| **Name**                                              | **Descrizione**                                                                                                                                                      |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Configurare un percorso di download alternativo per gli aggiornamenti. | Specifica un server intranet alternativo che ospiterà gli aggiornamenti da Microsoft Update. È quindi possibile usare questo servizio di aggiornamento per aggiornare automaticamente i computer nella rete |

Quando si configura il percorso di download alternativo per la cache del file ISV sono disponibili due opzioni:

1. **Specificare un nome host del server HTTP ISV**, ovvero la cache del file ISV
    
    Questo approccio consente di configurare il client di Windows Update per rendere le richieste di download per il server HTTP specificato nei criteri

2. **Specificare localhost**
 
    Questo approccio consente di configurare il client di Windows Update per rendere le richieste di download a localhost. In questo modo l'agente client di ISV gestire queste richieste e route come appropriato per soddisfare la richiesta di download.

>[!IMPORTANT]
>La cache del file ISV è necessario quanto segue:                                                          
                                                                                                                                   >- Il server deve essere HTTP 1.1 conforme base alle regole RFC: <http://www.w3.org/Protocols/rfc2616/rfc2616.html>                                                                                                                                                                
                                                                                                                                   >In particolare, deve supportare il server web                                                                                                                                                                                                                                       [ **HEAD** ](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) e [ **OTTIENI** ](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.htm) richieste<br>                                                                                                                                                                                                                                                                                                  -Le richieste di intervalli parziale<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   Keep-alive<br>                                                                                                                                                                                                                                                                                                                                                                                                                            -Non usare "Transfer-Encoding: chunked?                                                                                                 

#### <a name="populate-the-isv-file-cache"></a>Popolare la Cache del File di ISV

La cache del file ISV deve essere compilata con file associati con gli aggiornamenti da installare sui client gestiti. 

**Per popolare la cache del file ISV:**

1. Uso [le API WSUS](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.updateservices.administration.updatefile(v=vs.85).aspx) per accedere al percorso del file dell'aggiornamento e il nome di file per il servizio Microsoft Update.

    I metadati per ogni aggiornamento nel server WSUS contengono come indicato di seguito l'aggiornamento percorso del file e nome del file in Microsoft Update (nome host di Microsoft Update in grassetto, seguito dal percorso di file e nome file): **http://download.windowsupdate.com** /c/msdownload/aggiornamento / software/updt/2016/09/windows10.0-kb3195781-x64_0c06079bccc35cba35a48bd2b1ec46f818bd2e74.msu

2. Scaricare i file da Microsoft Update e memorizzarli nella cache del file ISV usando uno di questi due metodi: 

 - File di Store tramite il **stesso percorso della cartella come nel servizio Microsoft Update**

 - File di Store tramite un **percorso della cartella definito dall'ISV**

    Reindirizzamento HTTP sul server (o localhost) hanno **HTTP GET** le richieste che fanno riferimento al MU cartella percorso e nome, al percorso del file ISV.

### <a name="BKMK_3"></a>Passaggio 3: Configurare un agente client di ISV per indirizzare le operazioni client di Windows Update

L'agente client ISV Orchestra il download e installazione degli aggiornamenti approvati utilizzando il flusso di lavoro consigliato seguente:

1.  L'agente client di ISV chiama il client di Windows Update di analizzare con il server WSUS

2.  L'analisi restituisce il set di aggiornamenti applicabili al client di Windows Update

3.  Il client di ISV determina gli aggiornamenti da approvare, scaricare e installare

4.  Il client ISV client agent chiama Windows Update per scaricare gli aggiornamenti approvati

5.  Dopo aver scaricati gli aggiornamenti, l'agente client di ISV chiama il client di Windows Update per installare gli aggiornamenti approvati

Visualizzare [ricerca, download e installazione di aggiornamenti](https://msdn.microsoft.com/en-us/library/windows/desktop/aa387102(v=vs.85).aspx) per altre informazioni sull'uso del client WU da analizzare, scaricare e installare gli aggiornamenti.

### <a name="download-workflow-options"></a>Opzioni del flusso di lavoro di download

Di seguito sono due figure delle opzioni di download del flusso di lavoro da una cache di file ISV:

![Flusso di lavoro 1](../../media/express-update-delivery-isv-support/image1.png)

![Flusso di lavoro 2](../../media/express-update-delivery-isv-support/image2.png)
### <a name="how-express-download-works"></a>Funzionamento del download Express

- Per gli aggiornamenti del sistema operativo che supportano Express, esistono due versioni di payload del file memorizzate nel servizio:

 - **La versione completa** -essenzialmente sostituendo le versioni locali dei file binari di aggiornamento

 - **Versione Express** -contenente i valori delta necessarie per applicare patch i file binari esistenti nel dispositivo. 

   La versione di file completo e la versione Express viene fatto riferimento nei metadati dell'aggiornamento, che sono stato scaricato il client come parte della fase di analisi. 

   **Download Express funziona nel modo seguente:**

   Il client di Windows Update tenterà di scaricare Express prima di tutto, quindi in autunno determinate situazioni nuovamente completo del file se necessario (ad esempio, se passare attraverso un proxy che non supporta le richieste di intervalli di byte).

  1. Quando il client di Windows Update avvia un download, Express **il client di Windows Update scarica per prima cosa uno stub**, che fa parte del pacchetto di Express.

  2. **Il client di Windows Update passa questo stub per il programma di installazione di Windows**, che usa lo stub per eseguire un inventario locale, confronto tra gli elementi differenziali del file nel dispositivo con ciò che serve per ottenere la versione più recente del file che viene offerto.

  3. Il **programma di installazione di Windows richiede quindi il client di Windows Update per scaricare gli intervalli** che è state determinate richiesta.

  4. **Il client di Windows Update scarica questi intervalli e li passa al programma di installazione Windows**, che applica gli intervalli e determina quindi se sono necessari intervalli aggiuntivi. Questo processo si ripete fino a quando il programma di installazione di Windows indica il client di Windows Update che sono stati scaricati tutti gli intervalli necessari.

  A questo punto, il download è completato e l'aggiornamento è pronto per l'installazione.

### <a name="how-delivery-optimization-reduces-bandwidth-consumption"></a>Come ottimizzazione recapito riduce il consumo di larghezza di banda

Ottimizzazione recapito (non) è una soluzione cache distribuita organizzandosi per le aziende che vogliono ridurre il consumo di larghezza di banda per gli aggiornamenti del sistema operativo, aggiornamenti del sistema operativo e applicazioni. Consente ai client di scaricare gli elementi da origini alternative (ad esempio, gli altri peer nella rete) in combinazione con il percorso di download specificata (cache del file di ISV in questo scenario).

Per impostazione predefinita in Windows 10 Enterprise ed Education, consente la condivisione peer-to-peer nella rete dell'organizzazione solo, ma è possibile configurarlo in modo diverso mediante criteri di gruppo e impostazioni dei dispositivi mobili (MDM) di gestione.

Fare riferimento a [Aggiorna configurazione Delivery Optimization per Windows 10](https://technet.microsoft.com/itpro/windows/manage/waas-delivery-optimization) per ulteriori informazioni su.
