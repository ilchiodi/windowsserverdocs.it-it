---
title: Supporto ISV per il recapito degli aggiornamenti Express
description: 'Argomento di Windows Server Update Service (WSUS): Configurazione del recapito degli aggiornamenti Express da parte dei fornitori di software indipendenti (ISV) tramite WSUS'
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
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361718"
---
# <a name="express-update-delivery-isv-support"></a>Supporto ISV per il recapito degli aggiornamenti Express

>Si applica a: Windows 10, Windows Server 2016

I download degli aggiornamenti di Windows 10 possono avere dimensioni elevate perché ogni pacchetto contiene tutte le correzioni rilasciate in precedenza per garantire coerenza e semplicità.  

Dalla versione 7, Windows è stato in grado di ridurre le dimensioni dei download di Windows Update con una funzionalità denominata [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2) e, sebbene i dispositivi consumer la supportino per impostazione predefinita, i dispositivi Windows 10 di livello aziendale richiedono Windows Server Update Services (WSUS) per sfruttare i vantaggi offerti da Express.

## <a name="how-microsoft-supports-express"></a>Supporto di Microsoft per Express

- **Express in WSUS autonomo**

    Il recapito degli aggiornamenti Express è già [disponibile in tutte le versioni supportate di WSUS](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx).

- **Express in dispositivi connessi direttamente a Windows Update** 

    I dispositivi consumer supportano il download Express. Usano il client Windows Update (WU) per analizzare, scaricare e installare gli aggiornamenti. Durante la fase di download, il client WU richiede pacchetti Express e scarica gli intervalli di byte appropriati.

-  Anche i **dispositivi aziendali gestiti tramite [Windows Update per le aziende](https://technet.microsoft.com/itpro/windows/manage/waas-manage-updates-wufb)** usufruiscono del supporto del recapito degli aggiornamenti Express senza la necessità di modifiche alla configurazione.

## <a name="how-isvs-can-take-advantage-of-express"></a>Vantaggi di Express per i fornitori di software indipendenti (ISV)

Gli ISV possono usare WSUS e il client WU per supportare il recapito di aggiornamenti Express. Microsoft consiglia di eseguire i tre passaggi seguenti, descritti in modo più dettagliato nelle sezioni riportate di seguito:

1.  [**Configurare WSUS**](#BKMK_1)

    Il server WSUS è necessario per l'analisi e le sincronizzazioni degli aggiornamenti. Per altre informazioni, vedi [qui](https://technet.microsoft.com/library/dn800972(v=ws.11).aspx).

2.  [**Specificare e popolare una cache dei file ISV**](#BKMK_2)

    È consigliabile usare una cache dei file ISV per ospitare il contenuto degli aggiornamenti, che include i file CAB degli aggiornamenti (file con estensione cab) e i pacchetti Express (file con estensione psf).

3.  [**Configurare un agente client ISV per indirizzare le operazioni del client WU**](#BKMK_3)

>[!NOTE]
>È necessario installare l'aggiornamento cumulativo per Windows 10 versione 1607 rilasciata a gennaio 2017 o successivamente ([KB3213986 (build OS 14393.693)](https://support.microsoft.com/en-us/help/4009938/january-10-2017-kb3213986-os-build-14393-693)).
    
   - L'agente client ISV determina quali aggiornamenti approvare e quando eseguire il download e installare gli aggiornamenti
   - Il client WU determina gli intervalli di byte da scaricare e avvia la richiesta di download

### <a name="BKMK_1"></a>Passaggio 1: Configurare WSUS

WSUS funge da interfaccia per Windows Update e gestisce tutti i metadati che descrivono i pacchetti Express da scaricare. Se devi eseguire la distribuzione, vedi [**Panoramica di Windows Server Update Services 3.0 SP2**](https://technet.microsoft.com/library/dd939931(v=ws.10).aspx). Dopo aver distribuito WSUS, è necessario in primo luogo considerare se archiviare o meno il contenuto degli aggiornamenti localmente nel server WSUS. Quando si configura WSUS, è consigliabile non archiviare gli aggiornamenti in locale. Questa operazione presuppone che tu abbia già installato il software per la distribuzione di questi pacchetti nell'ambiente in uso. Per altre informazioni su come configurare l'archiviazione locale di WSUS, vedi [**Determinare dove archiviare gli aggiornamenti**](https://technet.microsoft.com/library/cc720494(v=ws.10).aspx).

### <a name="BKMK_2"></a>Passaggio 2: Specificare e popolare la cache dei file ISV 

#### <a name="specify-the-isv-file-cache"></a>Specificare la cache dei file ISV

Le nuove impostazioni di Criteri di gruppo e della gestione di dispositivi mobili (MDM) sul lato client, illustrate in dettaglio nella [**documentazione di riferimento del provider di servizi di configurazione**](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/configuration-service-provider-reference), definiscono il percorso della cache dei file ISV.

| **Nome**                                              | **Descrizione**                                                                                                                                                      |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Configurare un percorso di download alternativo per gli aggiornamenti. | Specificare un server intranet alternativo per ospitare gli aggiornamenti di Microsoft Update. Puoi quindi usare questo servizio per aggiornare automaticamente i computer della tua rete. |

Quando si configura il percorso di download alternativo per la cache dei file ISV, sono disponibili due opzioni:

1. **Specificare un nome host del server HTTP ISV**, ovvero la cache dei file ISV
    
    Questo approccio configura il client WU per eseguire richieste di download al server HTTP specificato nei criteri.

2. **Specificare localhost**
 
    Questo approccio configura il client WU per eseguire richieste di download a localhost. In tal modo, l'agente client ISV gestisce le richieste e le instrada nel modo appropriato per soddisfare la richiesta di download.

> [!IMPORTANT]
> Per la cache dei file ISV è necessario quanto segue:                                                          
> - Il server deve essere conforme a HTTP 1.1 in base alla specifica RFC: <http://www.w3.org/Protocols/rfc2616/rfc2616.html>                                                                                                                                                                
> In particolare, il server Web deve supportare                                                                                                                                                                                                                                       le richieste [**HEAD**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) e [**GET**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.htm)<br>                                                                                                                                                                                                                                                                                                  - Richieste di intervallo parziale<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   - Keep-alive<br>                                                                                                                                                                                                                                                                                                                                                                                                                            - Non usare "Transfer-Encoding:chunked"                                                                                                 

#### <a name="populate-the-isv-file-cache"></a>Popolare la cache dei file ISV

La cache dei file ISV deve essere popolata con i file associati agli aggiornamenti da installare nei client gestiti. 

**Per popolare la cache dei file ISV:**

1. Usa le [API di WSUS](https://msdn.microsoft.com/library/windows/desktop/microsoft.updateservices.administration.updatefile(v=vs.85).aspx) per accedere al percorso e al nome file dell'aggiornamento per il servizio MU.

    I metadati di ogni aggiornamento presente nel server WSUS contengono il percorso e il nome file dell'aggiornamento in Microsoft Update come indicato di seguito (nome host di Microsoft Update in grassetto, seguito da percorso e nome file): **<http://download.windowsupdate.com>** /c/msdownload/update/software/updt/2016/09/windows10.0-kb3195781-x64_0c06079bccc35cba35a48bd2b1ec46f818bd2e74.msu

2. Scarica i file da Microsoft Update e archiviali nella cache dei file ISV usando uno dei due metodi seguenti: 

   - Archivia i file usando lo **stesso percorso di cartella del servizio MU**

   - Archivia i file usando un **percorso di cartella definito dall'ISV**

     Fai in modo che il server HTTP (o localhost) reindirizzi le richieste **GET HTTP**, che fanno riferimento al percorso e al nome file della cartella MU, al percorso dei file ISV.

### <a name="BKMK_3"></a>Passaggio 3: Configurare un agente client ISV per indirizzare le operazioni del client WU

L'agente client ISV orchestra il download e l'installazione degli aggiornamenti approvati usando il flusso di lavoro consigliato seguente:

1.  L'agente client ISV chiama il client WU per eseguire un'analisi sul server WSUS

2.  L'analisi restituisce il set di aggiornamenti applicabili al client WU

3.  Il client ISV determina quali aggiornamenti approvare, scaricare e installare

4.  L'agente client ISV chiama il client WU per scaricare gli aggiornamenti approvati

5.  Dopo aver scaricato gli aggiornamenti, l'agente client ISV chiama il client WU per installare gli aggiornamenti approvati

Per altre informazioni sull'uso del client WU per l'analisi, il download e l'installazione degli aggiornamenti, vedi [Ricerca, download e installazione degli aggiornamenti](https://msdn.microsoft.com/library/windows/desktop/aa387102(v=vs.85).aspx).

### <a name="download-workflow-options"></a>Scaricare le opzioni del flusso di lavoro

Di seguito sono illustrate le due opzioni del flusso di lavoro di download da una cache di file ISV:

![Flusso di lavoro 1](../../media/express-update-delivery-isv-support/image1.png)

![Flusso di lavoro 2](../../media/express-update-delivery-isv-support/image2.png)
### <a name="how-express-download-works"></a>Funzionamento del download Express

- Per gli aggiornamenti del sistema operativo che supportano Express, esistono due versioni del payload di file archiviate nel servizio:

  - **Versione file completo**, che sostituisce essenzialmente le versioni locali dei file binari di aggiornamento.

  - **Versione Express**, che contiene i file differenziali necessari come patch dei file binari esistenti nel dispositivo. 

    Un riferimento a entrambe le versioni (file completo ed Express) è disponibile nei metadati dell'aggiornamento, scaricati nel client durante la fase di analisi. 

    **Il download Express funziona come segue:**

    Il client WU proverà a scaricare prima Express e, se necessario, in determinate situazioni passerà alla versione file completo, ad esempio se deve attraversare un proxy che non supporta le richieste di intervalli di byte.

  1. Quando avvia un download Express, **il client WU scarica prima uno stub**, che fa parte del pacchetto Express.

  2. **Il client WU passa lo stub a Windows Installer**, che lo usa per creare un inventario locale, confrontando le differenze del file nel dispositivo con gli elementi necessari per ottenere l'ultima versione del file offerto.

  3. **Windows Installer richiede quindi al client WU di scaricare gli intervalli** che sono determinati come necessari.

  4. **Il client WU scarica questi intervalli e li passa a Windows Installer**, che li applica e determina se ne sono necessari altri. Questo processo si ripete fino a quando Windows Installer non comunica al client WU che tutti gli intervalli necessari sono stati scaricati.

  A questo punto, il download è completato e l'aggiornamento è pronto per l'installazione.

### <a name="how-delivery-optimization-reduces-bandwidth-consumption"></a>Riduzione dell'utilizzo della larghezza di banda con Ottimizzazione recapito

Ottimizzazione recapito è una soluzione di cache distribuita con organizzazione automatica per le aziende che puntano a ridurre l'utilizzo della larghezza di banda per ogni tipo di aggiornamento del sistema operativo e per le applicazioni. Ottimizzazione recapito consente ai client di scaricare gli elementi da origini alternative, ad esempio altri peer di rete, in combinazione con il percorso di download specificato (in questo scenario, la cache dei file ISV).

Per impostazione predefinita, in Windows 10 Enterprise ed Education la soluzione Ottimizzazione recapito consente la condivisione peer-to-peer solo nella rete dell'organizzazione, ma puoi configurarla in modo diverso usando le impostazioni di Criteri di gruppo e della gestione di dispositivi mobili (MDM).

Per altre informazioni su Ottimizzazione recapito, vedi [Configurare Ottimizzazione recapito per gli aggiornamenti di Windows 10](https://technet.microsoft.com/itpro/windows/manage/waas-delivery-optimization).
