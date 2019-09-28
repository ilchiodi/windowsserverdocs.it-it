---
title: Funzionalità rimosse o pianificate per la sostituzione a partire da Windows Server (versione 1709)
description: Caratteristiche e funzionalità rimosse o di cui è pianificata la rimozione nelle versioni.
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
ms.date: 08/22/2019
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 93f98a87512232fe0605a9fc90a102c87818c88d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391644"
---
# <a name="features-removed-or-planned-for-replacement-starting-with-windows-server-version-1709"></a>Funzionalità rimosse o pianificate per la sostituzione a partire da Windows Server, versione 1709

>Si applica a: Windows Server versione 1709

Di seguito è riportato un elenco di caratteristiche e funzionalità di Windows Server versione 1709 che sono state rimosse dal prodotto in tale versione o di cui si comincia a prendere in considerazione la potenziale sostituzione nelle versioni successive. Questo elenco è destinato ai professionisti IT responsabili dell'aggiornamento dei sistemi operativi in un ambiente commerciale. **L'elenco è soggetto a modifiche nelle versioni successive e potrebbe non includere tutte le caratteristiche o le funzionalità interessate.** 

> [!TIP]
> - Puoi accedere in anteprima alle build di Windows Server partecipando al [Programma Windows Insider](https://insider.windows.com): è un ottimo modo per sperimentare le modifiche relative alle funzionalità.
> - Se hai domande su altre versioni, vedi [Funzionalità rimosse o pianificate per la sostituzione in Windows Server](../get-started-19/removed-features.md).

## <a name="features-removed-from-windows-server-version-1709"></a>Funzionalità rimosse da Windows Server versione 1709

Windows Server versione 1709 contiene le stesse funzionalità presenti in Windows Server 2016. Tuttavia, questa versione offre opzioni di installazione diverse rispetto a Windows Server 2016:

- In quanto versione del Canale semestrale, Windows Server versione 1709 offre solo l'opzione di installazione dei componenti di base. Per informazioni dettagliate, vedere [Confronto dei canali di manutenzione](../get-started-19/servicing-channels-19.md).
- A partire da questa versione, Nano Server non è disponibile come sistema operativo host installabile. Al contrario, Nano server è disponibile come sistema operativo contenitore. Vedere [Modifiche a Nano Server in Windows Server, versione 1709](nano-in-semi-annual-channel.md).
- A partire da questa versione, Server Message Block (SMB) versione 1 non viene più installato per impostazione predefinita. Per informazioni dettagliate, vedere [SMBv1 non è installato per impostazione predefinita in Windows 10 Fall Creators Update e Windows Server, versione 1709 e versioni successive](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows).


## <a name="features-being-considered-for-replacement-starting-with-subsequent-releases"></a>Funzionalità prese in considerazione per la sostituzione a partire dalle versioni successive

Le seguenti caratteristiche e funzionalità vengono prese in considerazione per la sostituzione a partire dalle versioni successive a Windows Server, versione 1709. In seguito potrebbero essere completamente rimosse dall'immagine del prodotto installato e sostituite da altre funzionalità o caratteristiche (o installabili da altre fonti), ma sono comunque disponibili in questa versione, a volte con alcune funzionalità rimosse. È consigliabile iniziare ora a pianificare l'utilizzo di metodi alternativi o la futura sostituzione per l'utilizzo, il codice o le applicazioni che dipendono da queste funzionalità.

Se si hanno commenti e suggerimenti da condividere sulla sostituzione proposta di una di queste funzionalità, è possibile utilizzare l'[app Hub di Feedback](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). Anche se l'app viene eseguita in Windows 10, è possibile usarla anche per inviare il feedback sul prodotto (e la documentazione) Windows Server.

### <a name="iis-6-management-compatibility"></a>Compatibilità di gestione con IIS 6
Le funzionalità specifiche prese in considerazione per la sostituzione sono:

- Compatibilità metabase IIS 6 (Web-Metabase)
- Console di gestione IIS 6 (Web-Lgcy-Mgmt-Console)
- Strumenti di scripting di IIS 6 (Web-Lgcy-Scripting)
- Compatibilità WMI con IIS 6 (Web-WMI)

Invece di Compatibilità metabase IIS 6 (che agisce come un livello di emulazione tra script di metabase basati su IIS 6 e la configurazione basata su file utilizzata da IIS 7 o versioni successive) è consigliabile iniziare direttamente la migrazione degli script di gestione nella configurazione basata su file IIS di destinazione, usando strumenti come lo spazio dei nomi Microsoft.Web.Administration.

È consigliabile inoltre avviare la migrazione da IIS 6.0 o versioni precedenti e passare all'ultima versione di IIS, che è sempre disponibile nella versione più recente di Windows Server.


### <a name="iis-digest-authentication"></a>Autenticazione del digest IIS
Questo metodo di autenticazione è pianificato per la sostituzione. Si consiglia di iniziare a utilizzare altri metodi di autenticazione, ad esempio il mapping dei certificati client (vedere [Configurazione dei mapping di certificati client uno a uno](https://docs.microsoft.com/iis/manage/configuring-security/configuring-one-to-one-client-certificate-mappings)) o l'autenticazione Windows (vedere [lmpostazioni dell'applicazione](https://docs.microsoft.com/iis-administration/configuration/appsettings.json)).

### <a name="internet-storage-name-service-isns"></a>Internet Storage Name Service (iSNS)
iSNS è stato preso in considerazione per la sostituzione. La funzionalità Server Message Block (SMB) presenta fondamentalmente le stesse caratteristiche con funzionalità aggiuntive. Per informazioni di background su questa funzionalità, vedere [Panoramica di SMB (Server Message Block)](https://technet.microsoft.com/library/hh831795(v=ws.11).aspx).

### <a name="rsaaes-encryption-for-iis"></a>Crittografia RSA/AES per IIS 
Questo metodo di crittografia è stato preso in considerazione per la sostituzione, perché la modalità Cryptography API: Next Generation (CNG) è già disponibile. Per ulteriori informazioni sulla crittografia CNG, vedere [Informazioni su CNG](https://msdn.microsoft.com/library/windows/desktop/aa375276(v=vs.85).aspx).

### <a name="windows-powershell-20"></a>Windows PowerShell 2.0
Questa versione preliminare di Windows PowerShell è stata sostituita da diverse versioni più recenti. Per ottenere funzionalità e prestazioni migliori, eseguire la migrazione a Windows PowerShell 5.0 o versione successiva. Molte informazioni sono disponibili nella [documentazione di PowerShell](https://docs.microsoft.com/powershell/index?view=powershell-5.1).

