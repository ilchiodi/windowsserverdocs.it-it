---
title: Vantaggio Azure Hybrid per Windows Server
description: Utilizza le licenze per le istanze locali di Windows Server per eseguire il salvataggio in macchine virtuali di Azure
ms.prod: windows-server
ms.date: 11/10/2017
ms.technology: server-general
ms.topic: article
author: greg-lindsay
ms.author: greg-lindsay
ms.localizationpriority: high
ms.openlocfilehash: 62821abc6c9eec660fa6af832bb1aba151708021
ms.sourcegitcommit: 3883eebbba70bfea0221e510863ee1a724a5f926
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "5783673"
---
# Vantaggio Azure Hybrid per Windows Server

>Si applica a: Windows Server

## Descrizione del vantaggio, regole e casi d'uso

Il Vantaggio Azure Hybrid per Windows Server ti consente di risparmiare fino al 40% sulle macchine virtuali di Windows Server in Azure utilizzando le licenze per le istanze locali di Windows Server con Software Assurance.  Con questo vantaggio, i clienti devono pagare solo i costi infrastrutturali della macchina virtuale, poiché la gestione delle licenze per Windows Server è coperta dal vantaggio di Software Assurance.  Il vantaggio è applicabile alle edizioni Standard e Datacenter di Windows Server per le versioni 2008R2, 2012, 2012R2 e 2016  ed è disponibile in tutte le aree geografiche e i cloud sovrani.


![immagine 1](media/ahb01.png)

Tutto ciò di cui hai bisogno per qualificarti per questo vantaggio è una copertura Software Assurance o una licenza di sottoscrizione, ad esempio EAS, SCE o Open Value Subscription, attiva per le licenze per Windows Server.  

Ogni licenza per 2 processori di Windows Server con SA/sottoscrizione e ogni set di licenze per 16 core di Windows Server con SA/sottoscrizione consentono al cliente di utilizzare Windows Server in Microsoft Azure su un massimo di 16 unità di memoria centrale virtuale (vCore) allocate tra due o meno istanze di base di Azure (macchine virtuali). Ogni set di licenze per 8 core con SA/sottoscrizione ti consente di utilizzare fino a 8 unità di memoria centrale virtuale (vCore) e un'istanza di base (VM).

| Licenza con SA/sottoscrizione            | Macchine virtuali e core concessi            | Possibilità di utilizzo                                |
|-----------------------------------------|----------------------------------|-----------------------------------------------------|
| WS Datacenter (L per 16 core o 2 proc)  | Fino a due macchine virtuali e fino a 16 core | Esecuzione di macchine virtuali in locale e in Azure  |
| WS Standard (L per 16 core o 2 proc)    | Fino a due macchine virtuali e fino a 16 core | Esecuzione di macchine virtuali in locale o in Azure |

Le macchine virtuali che utilizzano il Vantaggio Azure Hybrid possono essere eseguite in Azure solo durante il periodo di validità della copertura SA/sottoscrizione. In prossimità della scadenza della copertura SA/sottoscrizione, il cliente può rinnovare la copertura SA/sottoscrizione, disattivare le funzionalità del Vantaggio Azure Hybrid per la macchina virtuale o effettuare il deprovisioning della macchina virtuale che usa tale vantaggio. 

### Esempi di risparmio 

![immagine 2](media/ahb02.png)
 
Di seguito è riportata una tabella di riferimento utile per comprendere più nel dettaglio le regole del vantaggio. La colonna verde indica la quantità di macchine virtuali dello stesso tipo e la riga blu indica la densità di core di ogni macchina virtuale. Le celle gialle mostrano il numero di licenze per 2 processori (o set di 16 core) da distribuire a un numero specifico di macchine virtuali di una determinata densità di core. 

Windows Server con tabella di riferimento dei requisiti di SA:

![immagine 3](media/ahb03.png)
 
Il Vantaggio Azure Hybrid per Windows Server offre inoltre la flessibilità di eseguire le configurazioni in base alle esigenze, nonché di combinare macchine virtuali di diverso tipo.

Configurazioni di esempio per varie posizioni di licenza:

![immagine 4](media/ahb04.png)
![immagine 5](media/ahb05.png)

 
Per ulteriori informazioni sul Vantaggio Azure Hybrid per Windows Server, accedi al sito Web Vantaggio Azure Hybrid.

## Come garantire la conformità

I clienti che desiderano applicare il Vantaggio Azure Hybrid alle macchine virtuali di Windows Server in uso devono verificare il numero di licenze idonee e il periodo di copertura del programma SA/delle sottoscrizioni a esse associati prima dell'attivazione di questo vantaggio e attenersi alle linee guida precedenti per una distribuzione del numero corretto di macchine virtuali con il vantaggio. Se già disponi di macchine virtuali in esecuzione con il Vantaggio Azure Hybrid, devi eseguire un inventario del numero di unità in esecuzione e verificare quante sono le licenze di SA attive disponibili.  Contatta uno specialista di gestione delle licenze Microsoft Enterprise Agreement per convalidare la posizione delle licenze di SA.
Per visualizzare e contare tutte le macchine virtuali distribuite con il Vantaggio Azure Hybrid per Windows Server in una sottoscrizione, effettua le seguenti operazioni:

1. Configura il portale di Microsoft Azure in modo da mostrare l'utilizzo del Vantaggio Azure Hybrid per Windows Server. Aggiungi la colonna "Vantaggio Azure Hybrid" nella visualizzazione elenco della sezione delle macchine virtuali nel portale di Microsoft Azure. 

    ![immagine 6](media/ahb06.png)

2.  Usa PowerShell per elencare l'utilizzo del Vantaggio Azure Hybrid per Windows Server

    ```
    $vms = Get-AzureRMVM 
    foreach ($vm in $vms) {"VM Name: " + $vm.Name, "   Azure Hybrid Benefit for Windows Server: "+ $vm.LicenseType}
    ```

3.  Esamina la fattura di Microsoft Azure per determinare il numero di macchine virtuali con il Vantaggio Azure Hybrid per Windows Server in esecuzione. Le informazioni sul numero di istanze con il vantaggio vengono mostrate in "Informazioni aggiuntive":

    ```
    "{"ImageType":"WindowsServerBYOL","ServiceType":"Standard_A1","VMName":"","UsageType":"ComputeHR"}" 
    ```

Tieni presente che la fatturazione non viene applicata in tempo reale. Dopo aver attivato una macchina virtuale con il Vantaggio Azure Hybrid sarà ad esempio necessario attendere qualche ora prima che venga visualizzata in fattura.
Puoi quindi inserire i risultati nello **strumento per il conteggio delle licenze di SA per il Vantaggio Azure Hybrid per Windows Server** riportato di seguito per ottenere il numero di licenze richieste per WS con copertura SA o con sottoscrizioni.

Assicurati di eseguire un inventario per ogni sottoscrizione di tua proprietà per generare una visualizzazione completa della posizione delle licenze.

[Strumento per il conteggio delle licenze di SA per il Vantaggio Azure Hybrid per Windows Server](http://download.microsoft.com/download/7/1/2/712FEFF0-155C-4ABF-96C0-CE4EC4DB0516/Azure_Hybrid_Benefit_Windows_Server_SA_Count_Tool.xlsx)

Se hai eseguito le operazioni precedenti e verificato di disporre di tutte le licenze necessarie per il numero di istanze del Vantaggio Azure Hybrid in esecuzione, non devi effettuare ulteriori azioni. Se pensi che il vantaggio possa coprire macchine virtuali incrementali, puoi ottimizzare ulteriormente i costi passando all'esecuzione delle istanze mediante il vantaggio, anziché al costo completo.

Se non disponi di un numero di licenze per Windows Server idonee sufficiente per il numero di macchine virtuali già distribuite, devi acquistare ulteriori licenze per le istanze locali di Windows Server con copertura Software Assurance tramite uno dei canali elencati di seguito, acquistare macchine virtuali di Windows Server a tariffe orarie regolari o disattivare la funzionalità di Hybrid Benefit per alcune macchine virtuali. Tieni presente che puoi acquistare le licenze con incrementi di 8 core, ai fini della qualificazione di ogni macchina virtuale con il Vantaggio Azure Hybrid aggiuntiva. 

La copertura Software Assurance e/o le sottoscrizioni per Windows Server sono disponibili per l'acquisto tramite una combinazione dei seguenti canali di gestione delle licenze Microsoft:

| Canale                      | Open     | OVS      | Select/ Select Plus  | MPSA       | EA/EAS   |
|------------------------------|----------|----------|-----------------------|-----------|----------|
| Dimensioni tipiche (n. di dispositivi)  | 5-250    | 5-250    | >250                  | >250      | >500     |
| SA/sottoscrizione            | Facoltativa | Inclusa | Facoltativa              | Facoltativa  | Inclusa |

Microsoft si riserva il diritto di controllare il cliente finale in qualsiasi momento per verificare l'idoneità all'utilizzo del Vantaggio Azure Hybrid. 

## Guida alla distribuzione 

Abbiamo garantito la disponibilità di immagini della raccolta predefinite per tutti i clienti con licenze idonee, indipendentemente dal luogo di acquisto, e abbiamo consentito ai partner di eseguire le distribuzioni per conto dei clienti. 

Puoi trovare [qui](https://azure.microsoft.com/pricing/hybrid-use-benefit/) le istruzioni per tutte le opzioni di distribuzione disponibili, tra cui: 
-   Video dettagliato che mette in evidenza la nuova esperienza di distribuzione tramite le immagini della raccolta predefinite
-   Istruzioni dettagliate sul caricamento di una macchina virtuale creata in modo personalizzato 
-   Istruzioni dettagliate sulla migrazione di macchine virtuali esistenti tramite Azure Site Recovery e PowerShell. 