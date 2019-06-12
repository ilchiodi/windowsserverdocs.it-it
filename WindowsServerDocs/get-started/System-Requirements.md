---
title: Requisiti di sistema
description: Requisiti minimi per archiviazione, CPU, rete, memoria e RAM in un'installazione pulita di ogni opzione di installazione.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 10/17/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a8b42d7-9fe5-4efe-9ea1-ace2131fe068
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: d089af3562467aa1c222b17d9a1ad69d9c1b5008
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810592"
---
# <a name="system-requirements"></a>Requisiti di sistema

>Si applica a: Windows Server (canale semestrale), Windows Server 2016 

Questo argomento illustra i requisiti minimi di sistema per l'esecuzione di Windows Server&reg; 2016 o Windows Server, versione 1709.

> [!NOTE]  
> In questa versione è consigliabile eseguire installazioni pulite.  

> [!NOTE]  
> Se al momento dell'installazione si sceglie di procedere con l'opzione Server Core è necessario essere consapevoli che nessun componente GUI è installato e che non sarà possibile installare o disinstallare tali elementi con Server Manager. Se sono necessarie funzionalità GUI, quando si installa Windows Server 2016 assicurarsi di scegliere l'opzione "Server con Esperienza desktop". Per altre informazioni, vedere [Installare Nano Server](Getting-Started-with-Nano-Server.md)  


## <a name="review-system-requirements"></a>Verificare i requisiti di sistema  
Di seguito sono indicati i requisiti di sistema stimati per Windows Server 2016. Se le risorse del computer sono inferiori ai requisiti minimi indicati, non sarà possibile installare il prodotto in modo corretto. I requisiti effettivi possono variare in base alla configurazione del sistema, nonché in base alle applicazioni e alle funzionalità selezionate per l'installazione.

Se non diversamente specificato, i requisiti minimi di sistema si applicano a tutte le opzioni di installazione (Server Core, Server con Esperienza desktop e Nano Server) e a entrambe le edizioni Standard e Datacenter.  

> [!IMPORTANT]  
> La gamma estremamente diversificata delle potenziali distribuzioni rende poco realistico dichiarare i requisiti di sistema applicabili a livello generale come "consigliati". Consultare la documentazione relativa a ogni ruolo del server che si prevede di distribuire per ulteriori dettagli sulle esigenze a livello di risorse per gli specifici ruoli del server. Per ottenere risultati ottimali, eseguire distribuzioni di prova per stabilire i requisiti di sistema appropriati per ogni scenario di distribuzione specifico.  


## <a name="processor"></a>Processore  
Oltre che dalla frequenza di clock del processore, le prestazioni del processore dipendono dal numero di core del processore e dalle dimensioni della cache del processore. Di seguito sono indicati i requisiti del processore per questo prodotto:  

**Minimo**:  
- Processore a 64 bit da 1,4 GHz  
- Compatibile con il set di istruzioni x64  
- Supporto per NX e Protezione esecuzione programmi  
- Supporto per CMPXCHG16b, LAHF/SAHF e PrefetchW  
- Supporto per SLAT (Second-Level Address Translation) (EPT o NPT)  

[Coreinfo](https://technet.microsoft.com/sysinternals/cc835722.aspx) è uno strumento è possibile usare per verificare quali di queste funzionalità è la CPU.

## <a name="ram"></a>RAM  
Di seguito sono indicati i requisiti di RAM stimati per questo prodotto:  

**Minimo**:  
- 512 MB (2 GB per l'opzione di installazione Server con Esperienza desktop)
- Tipo ECC (codice di correzione errore) o tecnologia simile  

> [!IMPORTANT]  
> Il programma di installazione avrà esito negativo se si crea una macchina virtuale con i parametri hardware minimi supportati (1 core processore e 512 MB di RAM) e quindi si tenta di installare questa versione nella macchina virtuale.  
>   
> Per evitare il problema, eseguire una delle operazioni seguenti:  
>   
> -   Allocare più di 800 MB di RAM alla macchina virtuale in cui si intende installare questa versione. Al termine dell'installazione, è possibile modificare l'allocazione fino a un minimo di 512 MB di RAM, a seconda della configurazione effettiva del server.  
> -   Interrompere il processo di avvio di questa versione nella macchina virtuale con MAIUSC+F10. Nel prompt dei comandi aperto utilizzare Diskpart.exe per creare e formattare una partizione di installazione. Eseguire **Wpeutil createpagefile /path=C:\pf.sys** (supponendo che la partizione di installazione creata sia C:). Chiudere il prompt dei comandi e procedere con l'installazione.  

## <a name="storage-controller-and-disk-space-requirements"></a>Requisiti di spazio su disco e del controller di archiviazione  
I computer che eseguono Windows Server 2016 devono includere un adattatore di archiviazione conforme alla specifica di architettura PCI Express. I dispositivi di archiviazione permanente sul server classificati come unità disco rigido non devono essere di tipo PATA. Windows Server 2016 non supporta EIDE/ATA/PATA/IDE per l'avvio, il paging o le unità dati.  

Di seguito sono indicati i requisiti **minimi** di spazio su disco stimati per la partizione di sistema.  

**Minimo**: 32 GB  

> [!NOTE]
> Tenere presente che 32 GB deve essere considerato un *valore minimo assoluto* affinché l'installazione venga completata. Questo requisito minimo dovrebbe consentire l'installazione di Windows Server 2016 in modalità Server Core con il ruolo server Servizi Web (IIS). Un server con l'installazione dei componenti di base del server ha dimensioni di circa 4 GB inferiori rispetto allo stesso server nella modalità server con GUI. 
> 
> La partizione di sistema richiederà spazio aggiuntivo in presenza di una o più delle circostanze seguenti:  
> 
> -   Se si installa il sistema in una rete.  
> -   Nei computer con più di 16 GB di RAM è necessario ulteriore spazio su disco per i file di paging, di ibernazione e di dettagli.  

## <a name="network-adapter-requirements"></a>Requisiti della scheda di rete  

Le schede di rete usate con questa versione devono includere queste funzionalità:  

**Minimo**:  
- Una scheda Ethernet con una velocità effettiva minima pari a gigabit  
- Conforme alla specifica di architettura PCI Express.  
- Supporto per Pre-Boot Execution Environment (PXE).  

Una scheda di rete che supporta il debugging di rete (KDNet) è utile, ma non costituisce un requisito minimo.   

## <a name="other-requirements"></a>Altri requisiti  
I computer che eseguono questa versione devono disporre anche di quanto segue:  


-   Unità DVD (se si intende installare il sistema operativo da supporti DVD)  

Gli elementi seguenti non sono effettivamente obbligatori, ma necessari per alcune funzionalità:  

- Sistema UEFI 2.3.1c-based e firmware che supporta l'avvio protetto  
- Trusted Platform Module  

-   Dispositivo di grafica e monitor compatibile con la risoluzione Super VGA (1024 x 768) o superiore  

-   Tastiera e mouse Microsoft&reg; (o dispositivo di puntamento compatibile)  

-   Accesso a Internet (la connessione potrebbe essere a pagamento)  

> [!NOTE]  
> Un chip Trusted Platform Module (TPM) non è strettamente necessario per installare questa versione, anche se è necessario per usare determinate funzionalità come Crittografia unità BitLocker. Se il computer usa TPM, è necessario soddisfare questi requisiti:  
>  
> - I TPM basati su hardware devono implementare la versione 2.0 delle specifiche TPM.  
> - I TPM che implementano la versione 2.0 devono disporre di un certificato EK che viene sottoposto al provisioning anticipato per il TPM dal fornitore dell'hardware o essere in grado di essere recuperati dal dispositivo durante il primo avvio.  
> - I TPM che implementano la versione 2.0 devono disporre delle banche SHA-256 PCR e implementare PCR tra 0 e 23 per SHA-256. È possibile distribuire i TPM con una singola banca PCR commutabile che può essere utilizzata per le misurazioni SHA-1 e SHA-256.  
> - La presenza di un'opzione UEFI per disattivare il TPM non costituisce un requisito.  

## <a name="installation-of-nano-server"></a>Installazione di Nano Server  
Per passaggi dettagliati per installare Windows Server 2016 come Nano Server, vedere [Installare Nano Server](Getting-Started-with-Nano-Server.md).

## <a name="additional-resources"></a>Risorse aggiuntive
- [Requisiti del processore di Windows](https://docs.microsoft.com/windows-hardware/design/minimum/windows-processor-requirements)
- [Confronto delle edizioni Standard e Datacenter di Windows Server 2016](https://docs.microsoft.com/windows-server/get-started/2016-edition-comparison)
- [Requisiti di sistema di Windows 10](https://www.microsoft.com/windows/windows-10-specifications#system-specifications)
- [Scarica il foglio dati sulle licenze di Windows Server 2016](http://download.microsoft.com/download/7/2/9/7290EA05-DC56-4BED-9400-138C5701F174/WS2016LicensingDatasheet.pdf)
