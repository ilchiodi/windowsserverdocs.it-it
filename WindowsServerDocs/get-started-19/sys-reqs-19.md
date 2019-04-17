---
title: Requisiti di sistema di Windows Server 2019
description: Requisiti minimi per archiviazione, CPU, rete, memoria e RAM in un'installazione pulita di Windows Server 2019.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a8b42d7-9fe5-4efe-9ea1-ace2131f860e
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 82a42cd219e41330fe4215124c21e799a41e412c
ms.sourcegitcommit: fcc26ec5a2cc73b59c5752377b39c070d288655e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/19/2018
ms.locfileid: "8976657"
---
# Requisiti di sistema

>Si applica a: Windows Server 2019 

In questo argomento descrive i requisiti minimi di sistema per eseguire Windows Server&reg; 2019.

## Verificare i requisiti di sistema  
Di seguito sono i requisiti di sistema stimati Windows Server 2019. Se le risorse del computer sono inferiori ai requisiti minimi indicati, non sarà possibile installare il prodotto in modo corretto. I requisiti effettivi possono variare in base alla configurazione del sistema, nonché in base alle applicazioni e alle funzionalità selezionate per l'installazione.

Se non diversamente specificato, i requisiti minimi di sistema si applicano a tutte le opzioni di installazione (Server Core, Server con Esperienza desktop e Nano Server) e a entrambe le edizioni Standard e Datacenter.  

> [!IMPORTANT]  
> La gamma estremamente diversificata delle potenziali distribuzioni rende poco realistico dichiarare i requisiti di sistema applicabili a livello generale come "consigliati". Consultare la documentazione relativa a ogni ruolo del server che si prevede di distribuire per ulteriori dettagli sulle esigenze a livello di risorse per gli specifici ruoli del server. Per ottenere risultati ottimali, eseguire distribuzioni di prova per stabilire i requisiti di sistema appropriati per ogni scenario di distribuzione specifico.  


## Processore  
Oltre che dalla frequenza di clock del processore, le prestazioni del processore dipendono dal numero di core del processore e dalle dimensioni della cache del processore. Di seguito sono indicati i requisiti del processore per questo prodotto:  

**Minimo**:  
- Processore a 64 bit da 1,4 GHz  
- Compatibile con il set di istruzioni x64  
- Supporto per NX e Protezione esecuzione programmi  
- Supporto per CMPXCHG16b, LAHF/SAHF e PrefetchW  
- Supporto per SLAT (Second-Level Address Translation) (EPT o NPT)  

[Coreinfo](https://technet.microsoft.com/sysinternals/cc835722.aspx) è uno strumento che puoi usare per verificare quali di queste funzionalità della CPU.

## RAM  
Di seguito sono indicati i requisiti di RAM stimati per questo prodotto:  

**Minimo**:  
- 512 MB (2 GB per l'opzione di installazione Server con Esperienza desktop)
- Tipo ECC (codice di correzione errore) o tecnologia simile, per le distribuzioni di host fisico

> [!IMPORTANT]  
> Il programma di installazione avrà esito negativo se si crea una macchina virtuale con i parametri hardware minimi supportati (1 core processore e 512 MB di RAM) e quindi si tenta di installare questa versione nella macchina virtuale.  
>   
> Per evitare il problema, eseguire una delle operazioni seguenti:  
>   
> -   Allocare più di 800 MB di RAM alla macchina virtuale in cui si intende installare questa versione. Al termine dell'installazione, è possibile modificare l'allocazione fino a un minimo di 512 MB di RAM, a seconda della configurazione effettiva del server.  
> -   Interrompere il processo di avvio di questa versione nella macchina virtuale con MAIUSC+F10. Nel prompt dei comandi aperto utilizzare Diskpart.exe per creare e formattare una partizione di installazione. Eseguire **Wpeutil createpagefile /path=C:\pf.sys** (supponendo che la partizione di installazione creata sia C:). Chiudere il prompt dei comandi e procedere con l'installazione.  

## Requisiti di spazio su disco e del controller di archiviazione  
I computer che eseguono Windows Server 2019 devono includere un adattatore di archiviazione che è conforme alla specifica di architettura PCI Express. I dispositivi di archiviazione permanente sul server classificati come unità disco rigido non devono essere di tipo PATA. Windows Server 2019 non consente EIDE/ATA/PATA/IDE per le unità di avvio, il paging o i dati.  

Di seguito sono indicati i requisiti **minimi** di spazio su disco stimati per la partizione di sistema.  

**Minimo**: 32 GB  

   > [!NOTE]  
    > Tenere presente che 32 GB deve essere considerato un *valore minimo assoluto* affinché l'installazione venga completata. Questo requisito minimo dovrebbe consentire l'installazione di Windows Server 2019 in modalità Server Core con il ruolo server servizi Web (IIS). Un server con l'installazione dei componenti di base del server ha dimensioni di circa 4 GB inferiori rispetto allo stesso server nella modalità server con GUI. 
    >   
    > La partizione di sistema richiederà spazio aggiuntivo in presenza di una o più delle circostanze seguenti:  
    >   
    > -   Se si installa il sistema in una rete.  
    > -   Nei computer con più di 16 GB di RAM è necessario ulteriore spazio su disco per i file di paging, di ibernazione e di dettagli.  

## Requisiti della scheda di rete  

Le schede di rete usate con questa versione devono includere queste funzionalità:  

**Minimo**:  
- Una scheda Ethernet con una velocità effettiva minima pari a gigabit  
- Conforme alla specifica di architettura PCI Express.  

Una scheda di rete che supporta il debugging di rete (KDNet) è utile, ma non costituisce un requisito minimo.   

Una scheda di rete che supporta la pre-boot Execution Environment (PXE) è utile, ma non un requisito minimo.



## Altri requisiti  
I computer che eseguono questa versione devono disporre anche di quanto segue:  

-   Unità DVD (se si intende installare il sistema operativo da supporti DVD)  

Gli elementi seguenti non sono effettivamente obbligatori, ma necessari per alcune funzionalità:  

- Sistema UEFI 2.3.1c-based e firmware che supporta l'avvio protetto  
- Trusted Platform Module  

-   Dispositivo di grafica e monitor compatibile con la risoluzione Super VGA (1024 x 768) o superiore  

-   Tastiera e mouse Microsoft&reg; (o dispositivo di puntamento compatibile)  

-   Accesso a Internet (la connessione potrebbe essere a pagamento)  

>[!NOTE]  
> Un chip Trusted Platform Module (TPM) non è strettamente necessario per installare questa versione, anche se è necessario per usare determinate funzionalità come Crittografia unità BitLocker. Se il computer usa TPM, è necessario soddisfare questi requisiti:  
>  
>- I TPM basati su hardware devono implementare la versione 2.0 delle specifiche TPM.  
>- I TPM che implementano la versione 2.0 devono disporre di un certificato EK che viene sottoposto al provisioning anticipato per il TPM dal fornitore dell'hardware o essere in grado di essere recuperati dal dispositivo durante il primo avvio.  
>- I TPM che implementano la versione 2.0 devono disporre delle banche SHA-256 PCR e implementare PCR tra 0 e 23 per SHA-256. È possibile distribuire i TPM con una singola banca PCR commutabile che può essere utilizzata per le misurazioni SHA-1 e SHA-256.  
>- La presenza di un'opzione UEFI per disattivare il TPM non costituisce un requisito.  
