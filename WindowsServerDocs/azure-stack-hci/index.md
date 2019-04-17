---
title: Panoramica dello Stack HCI Azure
description: 'Azure Stack HCI è un cluster di Windows Server 2019 iperconvergente che usa convalidato hardware per l''esecuzione di carichi di lavoro virtualizzati in locale, facoltativamente, la connessione a servizi di Azure per il backup basato su cloud, il ripristino del sito e altro ancora. Azure Stack HCI soluzioni usano hardware convalidato Microsoft per garantire affidabilità e prestazioni ottimali e includono il supporto per le tecnologie, ad esempio le unità NVMe, memoria persistente e diretto a memoria remota (RDMA) l''accesso reti.'
ms.technology: storage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 03/26/2019
---

# Panoramica dello Stack HCI Azure

>Si applica a: Windows Server 2019

Azure Stack HCI è un cluster di Windows Server 2019 iperconvergente che usa convalidato hardware per l'esecuzione di carichi di lavoro virtualizzati in locale, facoltativamente, la connessione a servizi di Azure per il backup basato su cloud, il ripristino del sito e altro ancora. Azure Stack HCI soluzioni usano hardware convalidato Microsoft per garantire affidabilità e prestazioni ottimali e includono il supporto per le tecnologie, ad esempio le unità NVMe, memoria persistente e diretto a memoria remota (RDMA) l'accesso reti.

## La famiglia di Azure Stack

Azure Stack HCI fa parte di Azure e famiglia Azure Stack, usando lo stesso software-defined di calcolo, archiviazione e rete software come Azure Stack. Ecco un breve riepilogo delle soluzioni diverse:

- [Azure](https://azure.microsoft.com) - usare i servizi cloud pubblico
- [Azure Stack](https://azure.microsoft.com/overview/azure-stack) - operano locale di servizi cloud
- [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) - esecuzione virtualizzate di App in locale, con connessioni facoltative in Azure

![Azure e Azure Stack eseguire i servizi cloud, mentre viene eseguita Azure Stack HCI virtualizzato applicazioni locali](media/azure-and-azure-stack-family.png)

Per ulteriori informazioni:

- Eseguire la registrazione per il nostro [evento virtuale Cloud ibrido](https://info.microsoft.com/ww-landing-building-a-successful-hybrid-cloud-strategy.html) 28 marzo 2019.
- Scopri di più il nostro sito Web soluzioni [HCI dello Stack di Azure](https://azure.microsoft.com/overview/azure-stack/hci) .
- Guarda esperti Microsoft Jeff Woolsey e Vijay Tewari [illustrare le nuove soluzioni HCI dello Stack di Azure](https://aka.ms/AzureStackOverviewVideo).

## Partner hardware

Puoi acquistare soluzioni Azure Stack HCI convalidate che eseguono Windows Server 2019 dal 15 partner. Il partner Microsoft preferito ottiene è alto e in esecuzione senza progettazione lunga e la fase di compilazione e offre un singolo punto di contatto per i servizi di implementazione e supporto.

Visita il [sito Web di Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) per visualizzare le nostre 70 + Azure Stack HCI soluzioni attualmente disponibili da questi partner Microsoft: promettenti ASUS, Axellio, DataON, Dell EMC, Fujitsu, HPE, Hitachi, Huawei, Lenovo, NEC, soluzioni primeLine, QCT, SecureGUARD e Supermicro.

## Domande frequenti

### Cosa soluzioni Azure Stack e Azure Stack HCI hanno in comune? 
Soluzioni di Azure Stack HCI delle funzionalità di calcolo definita da software basati su Hyper-V stesso, archiviazione e tecnologie di rete come Azure Stack. Entrambe le offerte di soddisfano rigorosi criteri di test e la convalida per garantire affidabilità e compatibilità con la piattaforma hardware sottostante.

### Quali sono le differenze?
Con Azure Stack, eseguire cloud services in locale. È possibile eseguire IaaS di Azure e PaaS services in locale per compilare ed eseguire applicazioni cloud ovunque, gestite con il portale di Azure in locale in modo coerente.

Con Azure Stack HCI, puoi eseguire carichi di lavoro virtualizzati in locale, gestiti con Windows Admin Center e Windows Server familiari strumenti. Facoltativamente, è possibile connettersi ad Azure per gli scenari ibridi, ad esempio il ripristino del sito basato su cloud, monitoraggio e gli altri.

### Perché Microsoft è trasferimento relativo HCI disponibilità per la famiglia di Azure Stack? 
La tecnologia di Microsoft iperconvergente è già le basi di Azure Stack. 

Molti clienti Microsoft hanno ambienti IT complessi e il nostro obiettivo è necessario fornire soluzioni per soddisfare i loro in cui sono con la tecnologia appropriata per l'azienda a destra. Azure Stack HCI è un'evoluzione delle soluzioni basate su Windows Server 2016 Windows Server Software-Defined (WSSD) precedentemente disponibile dai nostri partner hardware. Abbiamo messa la famiglia di Azure Stack perché abbiamo iniziato a offrire nuove opzioni per la connessione con facilità con Azure per i servizi di gestione dell'infrastruttura. 

### Sarà in grado di eseguire l'aggiornamento da Azure Stack HCI allo Stack di Azure? 
No, ma i clienti possono eseguire la migrazione i carichi di lavoro da Azure Stack HCI Azure Stack o Azure.

### I servizi di Azure è possibile connettersi a Azure Stack HCI?

Per un elenco aggiornato dei servizi di Azure che è possibile connettere Azure Stack HCI a, vedi [La connessione di Windows Server per servizi di Azure ibrido](../azure-hybrid-services/index.md).

### Come è possibile acquistare soluzioni di Azure Stack HCI?
Segui questa procedura: 

1. Acquistare un sistema hardware convalidato Microsoft dal partner hardware preferita.
1. Installare Windows Server 2019 Datacenter edition e Windows Admin Center per la gestione e la possibilità di connettersi ad Azure per i servizi cloud
1. Facoltativamente, utilizzare l'account Azure per collegare i servizi di sicurezza e la gestione basata su cloud per i carichi di lavoro.