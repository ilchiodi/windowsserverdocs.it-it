---
ms.assetid: 4b844404-36ba-4154-aa5d-237a3dd644be
title: Panoramica di Deduplicazione dati
ms.technology: storage-deduplication
ms.prod: windows-server
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 05/09/2017
ms.openlocfilehash: 1050c63d77db66c8e280ea1bea9503390c5d0bae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386304"
---
# <a name="data-deduplication-overview"></a>Panoramica di Deduplicazione dati

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale), 

## <a name="what-is-dedup"></a>Che cos'è la deduplicazione dei dati?

La deduplicazione dei dati, spesso denominata deduplicazione per brevità, è una funzionalità che consente di ridurre l'effetto dei dati ridondanti sui costi di archiviazione. Quando è abilitata, la deduplicazione dei dati consente di ottimizzare lo spazio disponibile in un volume cercando parti duplicate nel volume stesso. Le parti duplicate del set di dati del volume vengono archiviate una sola volta e, facoltativamente, possono essere compresse per ottenere un risparmio di spazio aggiuntivo. La deduplicazione dei dati consente di ottimizzare le ridondanze senza compromettere la fedeltà o l'integrità dei dati. Per altre informazioni sul funzionamento della deduplicazione dei dati, vedere la sezione [How does Data Deduplication work?](understand.md#how-does-dedup-work) (Come funziona la deduplicazione dei dati?) della pagina [Understanding Data Deduplication](understand.md) (Informazioni sulla deduplicazione dei dati).

> [!Important]  
> [KB4025334](https://support.microsoft.com/kb/4025334) contiene un rollup di correzioni per la deduplicazione dei dati, incluse importanti correzioni di affidabilità, ed è consigliabile installarlo quando si usa la deduplicazione dati con windows server 2016 e windows server 2019.

## <a name="why-is-dedup-useful"></a>Perché la deduplicazione dei dati è utile?

La deduplicazione dei dati consente agli amministratori di archiviazione di ridurre i costi associati alla duplicazione dei dati. I set di dati di grandi dimensioni hanno spesso **<u>molti</u>** dati duplicati, che aumentano i costi di archiviazione. Ad esempio:

- Le condivisioni file utente possono contenere molte copie dello stesso file o di file simili.
- I guest di virtualizzazione possono essere quasi identici da una macchina virtuale a un'altra.
- Gli snapshot di backup possono presentare differenze trascurabili tra un backup giornaliero e un altro.

Il risparmio di spazio ottenibile con la deduplicazione dei dati dipende dal set di dati o dal carico di lavoro del volume. I set di dati con duplicazione elevata potrebbero registrare percentuali di ottimizzazione fino al 95% o una riduzione di 20 volte dell'uso dello spazio di archiviazione. Nella tabella seguente sono illustrati i vantaggi in termini di risparmio di spazio offerti dalla deduplicazione per vari tipi di contenuto:

| Scenario       | Contenuto                                        | Risparmio di spazio tipico |
|----------------|------------------------------------------------|-----------------------|
| Documenti degli utenti | Documenti aziendali, foto, musica, video e così via  | 30-50%                |
| Condivisioni di distribuzione | File binari del software, file CAB, simboli e così via | 70-80%                |
| Librerie di virtualizzazione | File ISO, file disco rigido virtuale e così via  | 80-95%                |
| Condivisioni file generali | Tutti gli elementi elencati sopra                           | 50-60%                |

## <a id="when-can-dedup-be-used"></a>Quando è possibile usare la deduplicazione dei dati?  
<table>
    <tbody>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-clustered-gpfs.png" alt="Illustration of file servers" /></td>
            <td style="vertical-align:top">
                <b>File server per utilizzo generico</b><br />
I file server per utilizzo generico sono file server che possono contenere i tipi di condivisioni seguenti: <ul>
                    <li>Condivisioni del team</li>
                    <li>Home directory utente</li>
                    <li><a href="https://technet.microsoft.com/library/dn265974.aspx">Cartelle di lavoro</a></li>
                    <li>Condivisioni per lo sviluppo di software</li>
                </ul>
I file server per utilizzo generico sono buoni candidati per la deduplicazione dei dati, vista la tendenza degli utenti ad avere molte copie o versioni dello stesso file. Le condivisioni di sviluppo software possono trarre molti vantaggi dalla deduplicazione perché molti file binari restano sostanzialmente invariati da una build all'altra. 
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-vdi.png" alt="Illustration of VDI servers" /></td>
            <td style="vertical-align:top">
                <b>Distribuzioni di VDI (Virtual Desktop Infrastructure)</b><br />
I server VDI, ad esempio <a href="https://technet.microsoft.com/library/cc725560.aspx">Servizi Desktop remoto</a>, costituiscono un’opzione poco impegnativa per le organizzazioni per eseguire il provisioning di computer desktop agli utenti. Esistono molti motivi per cui le organizzazioni si affidano a questa tecnologia: <ul>
                    <li><b>Distribuzione di applicazioni</b>: è possibile distribuire rapidamente le applicazioni nell'organizzazione. Ciò è molto utile quando si hanno applicazioni che vengono aggiornate di frequente, usate raramente o difficili da gestire.</li>
                    <li><b>Consolidamento delle applicazioni</b>: quando si installano ed eseguono applicazioni da un set di macchine virtuali gestite centralmente, si elimina la necessità di aggiornare le applicazioni nei computer client. Questa opzione riduce anche la quantità di larghezza di banda necessaria per accedere alle applicazioni.</li>
                    <li><b>Accesso remoto</b>: gli utenti possono accedere alle applicazioni aziendali da dispositivi quali computer di casa, chioschi multimediali, hardware a basso consumo e sistemi operativi diversi da Windows.</li>
                    <li><b>Accesso delle succursali</b>: le distribuzioni VDI possono offrire prestazioni ottimizzate per i dipendenti delle succursali che necessitano dell'accesso agli archivi dati centralizzati. Le applicazioni a elevato uso di dati talvolta non hanno protocolli client/server ottimizzati per connessioni a bassa velocità.</li>
                </ul>
Le distribuzioni VDI sono ottime candidate per la deduplicazione dei dati perché i dischi rigidi virtuali che consentono il funzionamento dei desktop remoti per gli utenti sono essenzialmente identici. La deduplicazione dei dati può essere utile anche per il cosiddetto <em>boot storm</em> della VDI, la riduzione delle prestazioni di archiviazione quando molti utenti eseguono l'accesso al desktop nello stesso momento, all'inizio della giornata.
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-backup.png" alt="Illustration of backup applications" /></td>
            <td style="vertical-align:top">
                <b>Destinazioni di backup, ad esempio le applicazioni di backup virtualizzate</b><br />
Le applicazioni di backup, ad esempio <a href="https://technet.microsoft.com/library/hh758173.aspx">Microsoft Data Protection Manager (DPM)</a>, sono ottime candidate per la deduplicazione dei dati, vista la duplicazione significativa tra gli snapshot di backup.
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-other.png" alt="Illustration of other workloads" /></td>
            <td style="vertical-align:top">
                <b>Altri carichi di lavoro</b><br />
                <a href="install-enable.md#enable-dedup-candidate-workloads" data-raw-source="[Other workloads may also be excellent candidates for Data Deduplication](install-enable.md#enable-dedup-candidate-workloads)">Anche altri carichi di lavoro possono essere adatti alla deduplicazione dei dati</a>.
            </td>
        </tr>
    </tbody>
</table>
