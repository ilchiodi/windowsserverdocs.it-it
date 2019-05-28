---
ms.assetid: 4b844404-36ba-4154-aa5d-237a3dd644be
title: Panoramica di Deduplicazione dati
ms.technology: storage-deduplication
ms.prod: windows-server-threshold
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 05/09/2017
ms.openlocfilehash: 4376dbb2c172a82c4ab64dc63acefbc37457110f
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476037"
---
# <a name="data-deduplication-overview"></a>Panoramica di Deduplicazione dati

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale), 

## <a name="what-is-dedup"></a>Che cos'è la deduplicazione dei dati?

La deduplicazione dei dati, spesso definito di deduplicazione per brevità, è una funzionalità che contribuisce di ridurre l'impatto dei dati ridondanti sui costi di archiviazione. Quando è abilitata, la deduplicazione dei dati consente di ottimizzare lo spazio disponibile in un volume cercando parti duplicate nel volume stesso. Le parti duplicate del set di dati del volume vengono archiviate una sola volta e, facoltativamente, possono essere compresse per ottenere un risparmio di spazio aggiuntivo. La deduplicazione dei dati consente di ottimizzare le ridondanze senza compromettere la fedeltà o l'integrità dei dati. Per altre informazioni sul funzionamento della deduplicazione dei dati, vedere la sezione [How does Data Deduplication work?](understand.md#how-does-dedup-work) (Come funziona la deduplicazione dei dati?) della pagina [Understanding Data Deduplication](understand.md) (Informazioni sulla deduplicazione dei dati).

> [!Important]  
> [KB4025334](https://support.microsoft.com/kb/4025334) contiene un'implementazione remota delle correzioni per la deduplicazione dei dati, tra cui affidabilità importanti correzioni e si consiglia fortemente di installarlo quando si usa la deduplicazione dei dati con Windows Server 2016 e Windows Server 2019.

## <a name="why-is-dedup-useful"></a>Perché è utile la deduplicazione dei dati?

La deduplicazione dei dati consente agli amministratori di archiviazione di ridurre i costi associati alla duplicazione dei dati. I set di dati di grandi dimensioni hanno spesso **<u>molti</u>** dati duplicati, che aumentano i costi di archiviazione. Ad esempio: 

- Le condivisioni file utente possono contenere molte copie dello stesso file o di file simili.
- I guest di virtualizzazione possono essere quasi identici da una macchina virtuale a un'altra.
- Gli snapshot di backup possono presentare differenze trascurabili tra un backup giornaliero e un altro.

Il risparmio di spazio ottenibile con la deduplicazione dei dati dipende dal set di dati o dal carico di lavoro del volume. I set di dati con duplicazione elevata potrebbero registrare percentuali di ottimizzazione fino al 95% o una riduzione di 20 volte dell'uso dello spazio di archiviazione. Nella tabella seguente sono illustrati i vantaggi in termini di risparmio di spazio offerti dalla deduplicazione per vari tipi di contenuto:

| Scenario       | Content                                        | Risparmio di spazio tipico |
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
                <b>File server generali</b><br />
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
                <b>Distribuzioni virtualizzate Desktop Infrastructure (VDI)</b><br />
I server VDI, ad esempio <a href="https://technet.microsoft.com/library/cc725560.aspx">Servizi Desktop remoto</a>, costituiscono un’opzione poco impegnativa per le organizzazioni per eseguire il provisioning di computer desktop agli utenti. Esistono molti motivi per cui le organizzazioni si affidano a questa tecnologia: <ul>
                    <li><b>Distribuzione dell'applicazione</b>: È possibile distribuire rapidamente le applicazioni nell'azienda. Ciò è molto utile quando si hanno applicazioni che vengono aggiornate di frequente, usate raramente o difficili da gestire.</li>
                    <li><b>Consolidamento delle applicazioni</b>: Quando si installa ed eseguire le applicazioni da un set di macchine virtuali gestite centralmente, si elimina la necessità di aggiornare le applicazioni nei computer client. Questa opzione riduce anche la quantità di larghezza di banda necessaria per accedere alle applicazioni.</li>
                    <li><b>Accesso remoto</b>: Gli utenti possono accedere le applicazioni aziendali da dispositivi, ad esempio computer di casa, chioschi multimediali, hardware a basso consumo e sistemi operativi diversi da Windows.</li>
                    <li><b>Accesso delle succursali</b>: Le distribuzioni VDI possono offrire prestazioni ottimizzate per il ramo impiegati, che devono accedere agli archivi dati centralizzati. Le applicazioni a elevato uso di dati talvolta non hanno protocolli client/server ottimizzati per connessioni a bassa velocità.</li>
                </ul>
Le distribuzioni VDI sono ottime candidate per la deduplicazione dei dati perché i dischi rigidi virtuali che consentono il funzionamento dei desktop remoti per gli utenti sono essenzialmente identici. La deduplicazione dei dati può essere utile anche per il cosiddetto *boot storm* della VDI, la riduzione delle prestazioni di archiviazione quando molti utenti eseguono l'accesso al desktop nello stesso momento, all'inizio della giornata.
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-backup.png" alt="Illustration of backup applications" /></td>
            <td style="vertical-align:top">
                <b>Destinazioni di backup, ad esempio applicazioni di backup virtualizzato</b><br />
Le applicazioni di backup, ad esempio <a href="https://technet.microsoft.com/library/hh758173.aspx">Microsoft Data Protection Manager (DPM)</a>, sono ottime candidate per la deduplicazione dei dati, vista la duplicazione significativa tra gli snapshot di backup.
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-other.png" alt="Illustration of other workloads" /></td>
            <td style="vertical-align:top">
                <b>Altri carichi di lavoro</b><br />
                [Anche altri carichi di lavoro possono essere adatti alla deduplicazione dei dati](install-enable.md#enable-dedup-candidate-workloads).
            </td>
        </tr>
    </tbody>
</table>
