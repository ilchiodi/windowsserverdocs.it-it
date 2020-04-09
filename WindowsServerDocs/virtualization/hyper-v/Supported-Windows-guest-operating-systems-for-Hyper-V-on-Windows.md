---
title: Sistemi operativi guest supportati Windows per Hyper-V in Windows Server
description: Elenca i sistemi operativi Windows supportati per l'uso come guest in una macchina virtuale. Fornisce inoltre collegamenti ad articoli simili per le versioni precedenti di Hyper-V.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 06b35897-2192-48b7-8c2d-125c520b0786
author: lizap
ms.author: elizapo
ms.date: 01/08/2019
ms.openlocfilehash: 34183deefef3eea94c2b1da8dcb111c2c17efd8a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857974"
---
# <a name="supported-windows-guest-operating-systems-for-hyper-v-on-windows-server"></a>Sistemi operativi guest supportati Windows per Hyper-V in Windows Server

>Si applica a: Windows Server 2016, Windows Server 2019

Hyper-V supporta diverse versioni di distribuzioni di Windows Server, Windows e Linux per l'esecuzione in macchine virtuali, come sistemi operativi guest. Questo articolo descrive i Server di Windows e sistemi operativi guest di Windows. Per distribuzioni di Linux e FreeBSD, vedere [supportate Linux e FreeBSD macchine virtuali per Hyper-V in Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).  
    
Alcuni sistemi operativi sono disponibili i servizi di integrazione incorporati. Altri richiedono di installare o aggiornare i servizi di integrazione come passaggio separato, dopo aver configurato il sistema operativo nella macchina virtuale. Per ulteriori informazioni, vedere le sezioni riportate di seguito e  [Integration Services](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services).  
  
## <a name="supported-windows-server-guest-operating-systems"></a>Sistemi operativi guest di Windows Server supportati  

Di seguito sono riportate le versioni di Windows Server supportate come sistemi operativi guest per Hyper-V in Windows Server 2016 e Windows Server 2019. 
  
|Sistema operativo guest (server)|Numero massimo di processori virtuali|Servizi di integrazione|Note|  
|-------------------------------------|----------------------------------------|------------------------|---------| 
|Windows Server, versione 1909 |240 per la generazione 2;<br>64 per generazione 1|Predefinito|Per il supporto del processore virtuale superiore a 240 sono necessari sistemi operativi guest Windows Server, versione 1903 o versioni successive.| 
|Windows Server, versione 1903 |240 per la generazione 2;<br>64 per generazione 1|Predefinito||
|Windows Server, versione 1809 |240 per la generazione 2;<br>64 per generazione 1|Predefinito|| 
|Windows Server 2019 |240 per la generazione 2;<br>64 per generazione 1|Predefinito||
|Windows Server versione 1803 |240 per la generazione 2;<br>64 per generazione 1|Predefinito|| 
|Windows Server 2016 |240 per la generazione 2;<br>64 per generazione 1|Predefinito|| 
|Windows Server 2012 R2 |64|Predefinito||  
|Windows Server 2012 |64|Predefinito||  
|Windows Server 2008 R2 con Service Pack 1 (SP1)|64|Installare tutti gli aggiornamenti critici di Windows dopo aver configurato il sistema operativo guest.|Edizioni Datacenter, Enterprise, Standard e Web.|
|Windows Server 2008 con Service Pack 2 (SP2)|8|Installare tutti gli aggiornamenti critici di Windows dopo aver configurato il sistema operativo guest.|Edizioni Datacenter, Enterprise, Standard e Web (32 bit e 64 bit).|  
  
## <a name="supported-windows-client-guest-operating-systems"></a>Sistemi operativi guest supportati client Windows  

Di seguito sono riportate le versioni di client Windows supportate come sistemi operativi guest per Hyper-V in Windows Server 2016 e Windows Server 2019.
  
|Sistema operativo guest (client)|Numero massimo di processori virtuali|Servizi di integrazione|Note|  
|-------------------------------------|----------------------------------------|------------------------|---------|  
|Windows 10|32|Predefinito||  
|Windows 8.1|32|Predefinito||  
|Windows 7 con Service Pack 1 (SP1)|4|Aggiornare i servizi di integrazione dopo aver configurato il sistema operativo guest.|Edizioni Ultimate, Enterprise e Professional (32 bit e 64 bit).|  
  
## <a name="guest-operating-system-support-on-other-versions-of-windows"></a>Supporto del sistema operativo guest in altre versioni di Windows  

Nella tabella seguente vengono forniti i collegamenti alle informazioni sui sistemi operativi guest supportati per Hyper-V in altre versioni di Windows.  
  
|Sistema operativo host|Argomento|  
|-------------------------|---------|  
|Windows 10|[Sistemi operativi guest supportati per Hyper-V client in Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os)|  
|Windows Server 2012 R2 e Windows 8.1|-   [sistemi operativi guest Windows supportati per Hyper-V in Windows Server 2012 R2 e Windows 8.1](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792027(v=ws.11))<br />-   [macchine virtuali Linux e FreeBSD in Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)|  
|Windows Server 2012 e Windows 8|[Sistemi operativi guest Windows supportati per Hyper-V in Windows Server 2012 e Windows 8](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792028(v=ws.11))|  
|Windows Server 2008 e Windows Server 2008 R2|[Informazioni sulle macchine virtuali e sui sistemi operativi guest](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc794868(v=ws.10))|  
  
## <a name="how-microsoft-provides-support-for-guest-operating-systems"></a>Come Microsoft fornisce il supporto per i sistemi operativi guest  

Microsoft fornisce supporto per i sistemi operativi guest secondo le modalità seguenti:  
  
-   I problemi riscontrati nei sistemi operativi Microsoft e nei servizi di integrazione sono supportati direttamente da Microsoft.  
  
-   Per i problemi riscontrati in altri sistemi operativi certificati dal relativo fornitore per l'esecuzione in Hyper-V, il supporto è garantito dal fornitore.  
  
-   Per i problemi riscontrati in altri sistemi operativi, Microsoft sottopone il problema alla community di supporto dei fornitori, [TSANet](https://www.tsanet.org/).  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Macchine virtuali Linux e FreeBSD in Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  
  
-   [Sistemi operativi guest supportati per Hyper-V client in Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os)  
  



