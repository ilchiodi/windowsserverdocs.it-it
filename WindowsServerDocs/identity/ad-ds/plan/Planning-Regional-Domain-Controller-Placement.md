---
ms.assetid: eb600904-24b8-4488-a278-c1c971dc2f2d
title: Pianificazione del posizionamento del controller di dominio regionale
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 2508476f35462516f32877365cb15be919b5b6df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408731"
---
# <a name="planning-regional-domain-controller-placement"></a>Pianificazione del posizionamento del controller di dominio regionale

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per garantire l'efficienza dei costi, pianificare la collocazione del minor numero possibile di controller di dominio regionali. Per prima cosa, vedere il foglio di controllo "posizioni geografiche e collegamenti di comunicazione" (DSSTOPO_1. doc) usato nella [raccolta di informazioni di rete](../../ad-ds/plan/Collecting-Network-Information.md) per determinare se una località è un hub.  
  
Pianificare il posizionamento dei controller di dominio regionali per ogni dominio rappresentato in ogni posizione dell'hub. Dopo aver posizionato i controller di dominio regionali in tutte le posizioni degli hub, valutare la necessità di collocare i controller di dominio regionali in posizioni satellite. Eliminando i controller di dominio regionali non necessari da percorsi satellite si riducono i costi di supporto necessari per gestire un'infrastruttura server remota.  
  
Inoltre, garantire la sicurezza fisica dei controller di dominio nelle posizioni Hub e satellite, in modo che il personale non autorizzato non possa accedervi. Non collocare i controller di dominio scrivibili in percorsi Hub e satellite in cui non è possibile garantire la sicurezza fisica del controller di dominio. Una persona con accesso fisico a un controller di dominio scrivibile può attaccare il sistema:  
  
- Per accedere ai dischi fisici, avviare un sistema operativo alternativo in un controller di dominio.  
- Rimozione e possibilmente sostituzione di dischi fisici in un controller di dominio.  
- Recupero e modifica di una copia di un backup dello stato del sistema del controller di dominio.  
  
Aggiungere controller di dominio locali scrivibili solo alle posizioni in cui è possibile garantire la sicurezza fisica.  
  
In posizioni con sicurezza fisica inadeguata, la soluzione consigliata è la distribuzione di un controller di dominio di sola lettura (RODC). Ad eccezione delle password degli account, un controller di dominio di sola lettura include tutti gli oggetti e gli attributi Active Directory contenuti in un controller di dominio scrivibile. Tuttavia, non è possibile apportare modifiche al database archiviato nel RODC. Le modifiche devono essere apportate in un controller di dominio scrivibile e quindi replicate nel controller di dominio di sola lettura.  
  
Per autenticare gli accessi client e l'accesso ai file server locali, la maggior parte delle organizzazioni posiziona i controller di dominio regionali per tutti i domini regionali rappresentati in una determinata posizione. Tuttavia, è necessario prendere in considerazione molte variabili quando si valuta se un percorso aziendale richiede che i client dispongano dell'autenticazione locale oppure i client possono basarsi sull'autenticazione ed eseguire query su un collegamento di Wide Area Network (WAN). Nella figura seguente viene illustrato come determinare se collocare i controller di dominio in posizioni satellite.  
  
![pianificare il posizionamento del controller di dominio regionale](media/Planning-Regional-Domain-Controller-Placement/49892c8c-2c99-4aab-92ba-808dbc8048e2.gif)  
  
## <a name="onsite-technical-expertise-availability"></a>Disponibilità di competenze tecniche in sede

I controller di dominio devono essere gestiti in modo continuo per diversi motivi. Posizionare un controller di dominio regionale solo nelle posizioni che includono il personale che può amministrare il controller di dominio o assicurarsi che il controller di dominio possa essere gestito in remoto.  
  
Negli ambienti di succursale con una scarsa sicurezza fisica e personale con scarse conoscenze tecnologiche, la distribuzione di un controller di dominio di sola lettura è spesso la soluzione consigliata. Le autorizzazioni amministrative locali per un controller di dominio di sola lettura possono essere delegate a qualsiasi utente di dominio senza concedere a tale utente diritti utente per il dominio o altri controller di dominio. Questo consente a un utente del ramo locale di accedere a un controller di sola lettura e di eseguire operazioni di manutenzione sul server, ad esempio l'aggiornamento di un driver. Tuttavia, l'utente del ramo non può accedere a qualsiasi altro controller di dominio o eseguire altre attività amministrative nel dominio. In questo modo, l'utente del ramo può delegare la capacità di gestire in modo efficace il controller di dominio di sola lettura nella succursale senza compromettere la sicurezza del resto del dominio o della foresta.  
  
## <a name="wan-link-availability"></a>Disponibilità collegamento WAN

Se il percorso non include un controller di dominio in grado di autenticare gli utenti, i collegamenti WAN che presentano frequenti interruzioni possono causare una significativa perdita di produttività per gli utenti. Se la disponibilità del collegamento WAN non è pari al 100% e i siti remoti non sono in grado di tollerare un'interruzione del servizio, collocare un controller di dominio a livello di area in posizioni in cui gli utenti richiedono la possibilità di accedere o di Exchange Server quando il collegamento WAN è inattivo.  
  
## <a name="authentication-availability"></a>Disponibilità autenticazione

Alcune organizzazioni, ad esempio le banche, richiedono l'autenticazione degli utenti in qualsiasi momento. Posizionare un controller di dominio a livello di area in un percorso in cui la disponibilità del collegamento WAN non sia pari al 100%, ma gli utenti richiedono sempre l'autenticazione.  
  
## <a name="logon-performance-over-wan-links"></a>Prestazioni di accesso su collegamenti WAN

Se la disponibilità del collegamento WAN è altamente affidabile, l'inserimento di un controller di dominio nella posizione dipende dai requisiti di prestazioni di accesso sul collegamento WAN. I fattori che influenzano le prestazioni di accesso sulla rete WAN includono velocità di collegamento e larghezza di banda disponibile, numero di utenti e profili di utilizzo e la quantità di traffico di rete di accesso rispetto al traffico di replica.  
  
### <a name="wan-link-speed-and-bandwidth-utilization"></a>Velocità di collegamento WAN e utilizzo della larghezza di banda

Le attività di un singolo utente possono compromettere un collegamento WAN lento. Posizionare un controller di dominio in un percorso se le prestazioni di accesso sul collegamento WAN sono inaccettabili.  
  
La percentuale media di utilizzo della larghezza di banda indica il modo in cui un collegamento di rete è congestionato. Se un collegamento di rete ha un utilizzo medio della larghezza di banda superiore a un valore accettabile, inserire un controller di dominio in tale posizione.  
  
### <a name="number-of-users-and-usage-profiles"></a>Numero di utenti e profili di utilizzo

Il numero di utenti e i relativi profili di utilizzo in una determinata posizione possono contribuire a determinare se è necessario inserire controller di dominio regionali in tale posizione. Per evitare la perdita di produttività in caso di errore di un collegamento WAN, collocare un controller di dominio a livello di area in una posizione con 100 o più utenti.  
  
I profili di utilizzo indicano il modo in cui gli utenti usano le risorse di rete. Non è necessario inserire un controller di dominio in un percorso che contenga solo pochi utenti che non accedono spesso a risorse di rete.  
  
### <a name="logon-network-traffic-vs-replication-traffic"></a>Traffico di rete di accesso rispetto al traffico di replica

Se un controller di dominio non è disponibile nella stessa posizione del client Active Directory, il client crea il traffico di accesso sulla rete. La quantità di traffico di rete di accesso creato sulla rete fisica è influenzata da diversi fattori, tra cui l'appartenenza ai gruppi; numero e dimensioni degli oggetti Criteri di gruppo (GPO); script di accesso; funzionalità quali le cartelle offline, il reindirizzamento delle cartelle e i profili mobili.  
  
D'altra parte, un controller di dominio posizionato in una determinata posizione genera il traffico di replica sulla rete. La frequenza e la quantità di aggiornamenti effettuati sulle partizioni ospitate nei controller di dominio influiscono sulla quantità di traffico di replica creato sulla rete. I diversi tipi di aggiornamenti che è possibile apportare alle partizioni ospitate nei controller di dominio includono l'aggiunta o la modifica di utenti e attributi utente, la modifica delle password e l'aggiunta o la modifica di gruppi, stampanti o volumi globali.  
  
Per determinare se è necessario collocare un controller di dominio a livello di area in una posizione, confrontare il costo del traffico di accesso creato da una posizione senza un controller di dominio rispetto al costo del traffico di replica creato inserendo un controller di dominio nella posizione.  
  
Si consideri, ad esempio, una rete con succursali connesse tramite collegamenti lenti alla sede centrale e in cui è possibile aggiungere facilmente i controller di dominio. Se l'accesso giornaliero e il traffico di ricerca nella directory di alcuni utenti di siti remoti causano un maggiore traffico di rete rispetto alla replica di tutti i dati aziendali nel ramo, è consigliabile aggiungere un controller di dominio al ramo.  
  
Se la riduzione del costo di gestione dei controller di dominio è più importante del traffico di rete, centralizzare i controller di dominio per tale dominio e non collocare controller di dominio regionali nella posizione o provare a collocare RODC nel percorso.  
  
Per un foglio di lavoro che assiste l'utente nella documentazione del posizionamento dei controller di dominio regionali e del numero di utenti per ogni dominio rappresentato in ogni posizione, vedere la pagina relativa ai [supporti per i processi per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), scaricare Job_Aids_Designing_and_ Deploying_Directory_and_Security_Services. zip e aprire "posizione controller di dominio" (DSSTOPO_4. doc).  
  
È necessario fare riferimento alle informazioni sui percorsi in cui è necessario posizionare i controller di dominio regionali quando si distribuiscono i domini regionali. Per ulteriori informazioni sulla distribuzione di domini regionali, vedere [la distribuzione di Windows Server 2008 domini regionali](https://technet.microsoft.com/library/cc755118.aspx).  
