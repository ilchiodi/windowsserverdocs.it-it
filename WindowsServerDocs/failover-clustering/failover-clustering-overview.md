---
ms.assetid: c9844427-27cf-4d76-b5bb-e06368b092f7
title: Clustering di failover
ms.prod: windows-server-threshold
layout: LandingPage
ms.topic: landing-page
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 03/08/2019
ms.localizationpriority: high
ms.openlocfilehash: 445de065ff5b68b83481ee5bd83ebf18fdd180a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848652"
---
# <a name="failover-clustering-in-windows-server"></a>Clustering di failover in Windows Server

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale)

>[!TIP]
> Cerchi informazioni sull'installazione delle versioni precedenti di Windows Server? Vedi le nostre altre [librerie di Windows Server](/previous-versions/windows/) in docs.microsoft.com. Puoi anche [cercare nel sito](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) informazioni specifiche.

<hr />

Un cluster di failover è costituito da un gruppo di computer indipendenti che interagiscono tra di loro per migliorare la disponibilità e la scalabilità dei ruoli cluster, noti in precedenza come servizi e applicazioni nel cluster. I server inclusi nel cluster (detti nodi) sono connessi mediante cavi fisici e software. Se in uno o più nodi del cluster si verifica un errore, il servizio verrà garantito dagli altri nodi (un processo noto come failover). I ruoli cluster, inoltre, vengono monitorati in modo proattivo per verificare che funzionino correttamente. Se non funzionano, vengono riavviati o spostati in un altro nodo.

I cluster di failover includono inoltre la funzionalità Volume condiviso del cluster, che fornisce uno spazio dei nomi uniforme e distribuito che può essere utilizzato dai ruoli del cluster per accedere allo spazio di archiviazione condiviso da tutti i nodi. Con la funzionalità Clustering di failover, per gli utenti l'interruzione del servizio risulterà minima.

Clustering di failover offre numerose applicazioni pratiche, tra cui:
* Archiviazione su condivisione file con disponibilità continua ed elevata per applicazioni come Microsoft SQL Server e macchine virtuali Hyper-V
* Ruoli cluster a disponibilità elevata eseguiti su server fisici o su macchine virtuali installate in server che eseguono Hyper-V

<hr />

<ul class="cardsF panelContent">
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-whats-new.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h2><a href="whats-new-in-failover-clustering.md">Quali sono le novità nel Clustering di Failover</a></h2>
                                        </div>
                                    </div>
                                </div>
                             </div>
                          </a>
                        </li>
                     </ul>
<HR />

<ul class="cardsF panelContent">

<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Esaminare con attenzione</h3>
<HR />
                                        <p><a href="sofs-overview.md">File Server di scalabilità orizzontale per dati di applicazioni</a></p>
<HR />
                                        <p><a href="../storage/storage-spaces/understand-quorum.md">Quorum del cluster e pool</a></p>
<HR />
                                        <p><a href="fault-domains.md">Consapevolezza di dominio di errore</a></p>
<HR />
                                        <p><a href="smb-multichannel.md">Reti di cluster di SMB multicanale e multi-NIC semplificate</a></p>
<HR />
                                        <p><a href="vm-load-balancing-overview.md">Bilanciamento del carico della macchina virtuale</a></p>
<HR />
                                        <p><a href="../storage/storage-spaces/cluster-sets.md">Set di cluster</a></p>
<HR />
                                        <p><a href="cluster-affinity.md">Affinità di cluster</a></p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>

<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Pianificazione</h3>
<HR />
                                        <p><a href="clustering-requirements.md">Requisiti Hardware di Clustering di failover e opzioni di archiviazione</a></p>
<HR />
                                        <p><a href="failover-cluster-csvs.md">Volumi condivisi del Cluster di utilizzo</a></p>               
<HR />
                                        <p><a href="../storage/storage-spaces/storage-spaces-direct-in-vm.md">Uso di cluster di macchine virtuali guest con spazi di archiviazione diretta</a></p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Distribuzione</a></h3> 
<HR />
                                        <p><a href="prestage-cluster-adds.md">Pre-installare oggetti Computer del Cluster in servizi di dominio Active Directory</a></p>
<HR />
                                        <p><a href="create-failover-cluster.md">Creazione di un Cluster di Failover</a></p> 
<HR />
                                        <p><a href="deploy-two-node-clustered-file-server.md">Distribuire un server di file di due nodi</a></p> 
<HR />
                                        <p><a href="manage-cluster-quorum.md">Gestire il quorum e i server di controllo</a></p> 
<HR />
                                        <p><a href="deploy-cloud-witness.md">Distribuire un cloud di controllo</a></p>
<HR />
                                        <p><a href="file-share-witness.md">Distribuire una condivisione file di controllo</a></p>
<HR />
                                        <p><a href="cluster-operating-system-rolling-upgrade.md">Aggiornamenti in sequenza del sistema operativo del cluster</a></p> 
<HR />
                                        <p><a href="upgrade-option-same-hardware.md">Aggiornamento di un cluster di failover sullo stesso hardware</a></p>
<HR />
                                        <p><a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265970\(v%3dws.11\)">Distribuire un Cluster scollegato Active Directory</a></p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
                     </ul>
<HR />
<ul class="cardsF panelContent">
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Gestisci</h3>
<HR />
                                        <p><a href="cluster-aware-updating.md">Aggiornamento compatibile con cluster</a></p> 
<HR />
                                        <p><a href="health-service-overview.md">Servizio integrità</a></p>
<HR />
                                        <p><a href="cluster-domain-migration.md">Migrazione di cluster di dominio</a></p>
<HR />
                                        <p><a href="troubleshooting-using-wer-reports.md">Risoluzione dei problemi tramite segnalazione errori Windows</a></p> 
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Strumenti e impostazioni</a></h3>
<HR />
                                        <p><a href="https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps">Cmdlet di PowerShell di Clustering di failover</a></p> 
<HR />
                                        <p><a href="https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps">Cmdlet di PowerShell ad aggiornamento compatibile con cluster</a></p> 
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Risorse della community</a></h3>
<HR />
                                        <p><a href="https://go.microsoft.com/fwlink/p/?LinkId=230641">Forum dedicato alla disponibilità elevata (clustering)</a></p> 
<HR />
                                        <p><a href="http://blogs.msdn.com/b/clustering/">Clustering di failover e carico di rete blog del Team di bilanciamento del carico</a></p> 
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
</ul>
