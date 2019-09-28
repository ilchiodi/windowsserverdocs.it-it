---
title: Supporto ISV recapito aggiornamenti Express
description: Argomento Windows Server Update Service (WSUS)-come i fornitori di software indipendenti (ISV) possono configurare la distribuzione di aggiornamenti rapidi tramite WSUS
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: a4880a1a66d9c722cfda9e194c4eff38c5058674
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361718"
---
# <a name="express-update-delivery-isv-support"></a>Supporto ISV recapito aggiornamenti Express

>Si applica a: Windows 10, Windows Server 2016

I download degli aggiornamenti di Windows 10 possono essere di grandi dimensioni perché ogni pacchetto contiene tutte le correzioni rilasciate in precedenza per garantire coerenza e semplicità.  

Dalla versione 7, Windows è stato in grado di ridurre le dimensioni dei download di Windows Update con una funzionalità denominata [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2)e, sebbene i dispositivi consumer lo supportino per impostazione predefinita, i dispositivi Windows 10 enterprise richiedono Windows Server Update Services (WSUS) vantaggio di Express.

## <a name="how-microsoft-supports-express"></a>Supporto di Microsoft per Express

- **Express in WSUS autonomo**

    Il recapito con aggiornamento rapido è già [disponibile in tutte le versioni supportate di WSUS](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx).

- **Express nei dispositivi connessi direttamente a Windows Update** 

    I dispositivi consumer supportano il download rapido: usano il client Windows Update (WU) per analizzare, scaricare e installare gli aggiornamenti. Durante la fase di download, il client WU richiede pacchetti Express e Scarica gli intervalli di byte appropriati.

-  **Anche i dispositivi aziendali gestiti tramite [Windows Update for Business](https://technet.microsoft.com/itpro/windows/manage/waas-manage-updates-wufb)** usufruiscono del supporto del recapito degli aggiornamenti Express senza la necessità apportare modifiche di configurazione.

## <a name="how-isvs-can-take-advantage-of-express"></a>Come gli ISV possono sfruttare i vantaggi di Express

Gli ISV possono usare WSUS e il client WU per supportare la distribuzione di aggiornamenti rapidi. Microsoft consiglia i tre passaggi seguenti, descritti in modo più dettagliato nelle sezioni riportate di seguito:

1.  [**Configurare WSUS**](#BKMK_1)

    Il server WSUS è necessario per l'analisi & le sincronizzazioni degli aggiornamenti. altre informazioni sono reperibili [qui](https://technet.microsoft.com/library/dn800972(v=ws.11).aspx).

2.  [**Specificare e popolare una cache di file ISV**](#BKMK_2)

    È consigliabile usare una cache di file ISV per ospitare il contenuto dell'aggiornamento, che include i file CAB di aggiornamento (file con estensione cab) e i pacchetti Express (file con estensione PSF).

3.  [**Configurare un agente client ISV per indirizzare le operazioni client WU**](#BKMK_3)

>[!NOTE]
>Richiede l'aggiornamento cumulativo per Windows 10 versione 1607 in (o dopo) gennaio 2017 ([KB3213986 (sistema operativo Build 14393,693)](https://support.microsoft.com/en-us/help/4009938/january-10-2017-kb3213986-os-build-14393-693) da installare.
    
   - L'agente client ISV determina gli aggiornamenti da approvare e quando scaricare e installare gli aggiornamenti
   - Il client WU determina gli intervalli di byte da scaricare e avvia la richiesta di download

### <a name="BKMK_1"></a>Passaggio 1: Configurare WSUS

WSUS funge da interfaccia per Windows Update e gestisce tutti i metadati che descrivono i pacchetti Express che devono essere scaricati. Se è necessario distribuire, vedere [**Panoramica di Windows Server Update Services 3,0 SP2**](https://technet.microsoft.com/library/dd939931(v=ws.10).aspx). Una volta distribuito WSUS, la considerazione principale è se archiviare o meno il contenuto di aggiornamento localmente nel server WSUS. Quando si configura WSUS, è consigliabile non archiviare gli aggiornamenti in locale. Si presuppone che l'utente abbia già installato il software per la distribuzione di questi pacchetti nell'ambiente in uso. Per ulteriori informazioni su come configurare l'archiviazione locale di WSUS, vedere [**determinare dove archiviare gli aggiornamenti**](https://technet.microsoft.com/library/cc720494(v=ws.10).aspx).

### <a name="BKMK_2"></a>Passaggio 2: Specificare e popolare la cache dei file ISV 

#### <a name="specify-the-isv-file-cache"></a>Specificare la cache dei file ISV

Nuove impostazioni del Criteri di gruppo lato client e della gestione dei dispositivi mobili (MDM) dettagliate nel [**riferimento del provider di servizi di configurazione**](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/configuration-service-provider-reference) definire il percorso della cache dei file ISV.

| **Name**                                              | **Descrizione**                                                                                                                                                      |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Configurare un percorso di download alternativo per gli aggiornamenti. | Specifica un server Intranet alternativo per ospitare gli aggiornamenti da Microsoft Update. È quindi possibile usare questo servizio di aggiornamento per aggiornare automaticamente i computer nella rete |

Quando si configura il percorso di download alternativo per la cache di file ISV, sono disponibili due opzioni:

1. **Specificare un nome host del server http ISV**, ovvero la cache dei file ISV
    
    Questo approccio configura il client WU per eseguire richieste di download al server HTTP specificato nei criteri

2. **Specificare localhost**
 
    Questo approccio configura il client WU per eseguire richieste di download a localhost. Ciò consente all'agente client ISV di gestire queste richieste e indirizzarle nel modo appropriato per soddisfare la richiesta di download.

> [!IMPORTANT]
> Per la cache di file ISV sono necessari gli elementi seguenti:                                                          
> - Il server deve essere conforme a HTTP 1,1 in base alla specifica RFC:<http://www.w3.org/Protocols/rfc2616/rfc2616.html>                                                                                                                                                                
> In particolare, il server Web deve supportare                                                                                                                                                                                                                                       Richieste [**Head**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) e [**Get**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.htm)<br>                                                                                                                                                                                                                                                                                                  -Richieste di intervallo parziale<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   -Keep-Alive<br>                                                                                                                                                                                                                                                                                                                                                                                                                            -Non usare "Transfer-Encoding: chunked"                                                                                                 

#### <a name="populate-the-isv-file-cache"></a>Popolamento della cache dei file ISV

La cache dei file ISV deve essere popolata con i file associati agli aggiornamenti da installare nei client gestiti. 

**Per popolare la cache dei file ISV:**

1. Utilizzare le [API WSUS](https://msdn.microsoft.com/library/windows/desktop/microsoft.updateservices.administration.updatefile(v=vs.85).aspx) per accedere al percorso e al nome file dell'aggiornamento per il servizio MU.

    I metadati per ogni aggiornamento nel server WSUS contengono il percorso e il nome file dell'aggiornamento in Microsoft Update come indicato di seguito (Microsoft Update nome host in grassetto, seguito da percorso e nome file): **<http://download.windowsupdate.com>** /c/msdownload/update/software/UPDT/2016/09/ Windows 10.0-kb3195781-x64_0c06079bccc35cba35a48bd2b1ec46f818bd2e74. msu

2. Scaricare i file da Microsoft Update e archiviarli nella cache dei file ISV usando uno di questi due metodi: 

   - Archiviare i file usando lo **stesso percorso di cartella del servizio MU**

   - Archiviare file usando un **percorso di cartella definito da ISV**

     Disporre di richieste **http Get** di reindirizzamento del server http (o localhost), che fanno riferimento al percorso e al nome del file della cartella MU, al percorso del file ISV.

### <a name="BKMK_3"></a>Passaggio 3: Configurare un agente client ISV per indirizzare le operazioni client WU

L'agente client ISV orchestra il download e l'installazione degli aggiornamenti approvati utilizzando il flusso di lavoro consigliato seguente:

1.  L'agente client ISV chiama il client WU per eseguire un'analisi rispetto al server WSUS

2.  L'analisi restituisce il set di aggiornamenti applicabili al client WU

3.  Il client ISV determina gli aggiornamenti da approvare, scaricare e installare

4.  L'agente client ISV chiama il client WU per scaricare gli aggiornamenti approvati

5.  Una volta scaricati gli aggiornamenti, l'agente client ISV chiama il client WU per installare gli aggiornamenti approvati

Per altre informazioni sull'uso del client WU per analizzare, scaricare e installare gli aggiornamenti, vedere [ricerca, download e installazione degli aggiornamenti](https://msdn.microsoft.com/library/windows/desktop/aa387102(v=vs.85).aspx) .

### <a name="download-workflow-options"></a>Scaricare le opzioni del flusso di lavoro

Di seguito sono illustrate due illustrazioni delle opzioni di download del flusso di lavoro da una cache di file ISV:

![Flusso di lavoro 1](../../media/express-update-delivery-isv-support/image1.png)

![Flusso di lavoro 2](../../media/express-update-delivery-isv-support/image2.png)
### <a name="how-express-download-works"></a>Funzionamento del download Express

- Per gli aggiornamenti del sistema operativo che supportano Express, esistono due versioni di payload del file memorizzate nel servizio:

  - **Versione file completo** --sostituisce essenzialmente le versioni locali dei file binari di aggiornamento

  - **Versione Express** : contiene i Delta necessari per applicare patch ai file binari esistenti nel dispositivo. 

    Nei metadati dell'aggiornamento viene fatto riferimento sia alla versione del file completo che alla versione Express, che è stata scaricata nel client come parte della fase di analisi. 

    **Il download rapido funziona nel modo seguente:**

    Il client WU tenterà prima di tutto di scaricare Express e, in determinate situazioni, eseguirà il fallback al file completo, se necessario (ad esempio, se si attraversa un proxy che non supporta le richieste di intervalli di byte).

  1. Quando il client WU avvia un download rapido, **il client Wu Scarica innanzitutto uno stub**, che fa parte del pacchetto Express.

  2. **Il client Wu passa questo stub a Windows Installer**, che usa lo stub per eseguire un inventario locale, confrontando i Delta del file sul dispositivo con quello necessario per ottenere la versione più recente del file offerto.

  3. **Windows Installer richiede quindi al client Wu di scaricare gli intervalli** che sono stati determinati come obbligatori.

  4. **Il client Wu Scarica questi intervalli e li passa a Windows Installer**, che applica gli intervalli e quindi determina se sono necessari intervalli aggiuntivi. Questa operazione si ripete fino a quando Windows Installer indica al client WU che tutti gli intervalli necessari sono stati scaricati.

  A questo punto, il download è completato e l'aggiornamento è pronto per l'installazione.

### <a name="how-delivery-optimization-reduces-bandwidth-consumption"></a>Riduzione dell'utilizzo della larghezza di banda da Ottimizzazione recapito

Ottimizzazione recapito è una soluzione di cache distribuita con organizzazione automatica per le aziende che desiderano ridurre l'utilizzo della larghezza di banda per gli aggiornamenti del sistema operativo, gli aggiornamenti del sistema operativo e le applicazioni. Consente ai client di scaricare gli elementi da origini alternative, ad esempio altri peer sulla rete, in combinazione con il percorso di download specificato (la cache dei file ISV in questo scenario).

Per impostazione predefinita, in Windows 10 Enterprise e Education, DO consente la condivisione peer-to-peer solo nella rete dell'organizzazione, ma è possibile configurarla in modo diverso usando Criteri di gruppo e le impostazioni di gestione di dispositivi mobili (MDM).

Per ulteriori informazioni su, vedere [configurare l'ottimizzazione recapito per gli aggiornamenti di Windows 10](https://technet.microsoft.com/itpro/windows/manage/waas-delivery-optimization) .
