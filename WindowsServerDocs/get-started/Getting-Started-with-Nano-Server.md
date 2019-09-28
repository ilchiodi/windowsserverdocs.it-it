---
title: Installare Nano Server
description: Installazione pulita, aggiornamento, migrazione e valutazione di Nano Server
ms.prod: windows-server
ms.service: na
manager: dougkim
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 2c2fa45b-6f3b-4663-b421-2da6ecc463bf
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: d395c72a1e21cd8eda043eebf3b72bbd5c9a13e8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391799"
---
# <a name="install-nano-server"></a>Installare Nano Server

>Si applica a: Windows Server 2016

> [!IMPORTANT]
> A partire da Windows Server versione 1709, Nano Server sarà disponibile solo come [immagine del sistema operativo di base del contenitore](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Per informazioni, vedi [Modifiche apportate a Nano Server](nano-in-semi-annual-channel.md). 

Windows Server 2016 offre una nuova opzione di installazione: Nano Server. Nano Server è un sistema operativo server amministrato da postazione remota e ottimizzato per data center e cloud privati. È simile a Windows Server in modalità Server Core, ma notevolmente più piccolo, non dispone di alcuna funzionalità di accesso locale e supporta solo agenti, strumenti e applicazioni a 64 bit. Occupa meno spazio su disco, viene impostato molto più velocemente e richiede meno aggiornamenti e riavvii rispetto a Windows Server. Il riavvio del sistema viene eseguito molto più velocemente. L'opzione di installazione di Nano Server è disponibile per le edizioni Standard e Datacenter di Windows Server 2016.  

Nano Server è ideale per diversi scenari:  
  
-   Come host di "calcolo" per le macchine virtuali Hyper-V in cluster o meno  
  
-   Come host di archiviazione per il file server di scalabilità orizzontale  
  
-   Come server DNS  
  
-   Come server Web che esegue Internet Information Services (IIS)  
  
-   Come host per le applicazioni sviluppate usando modelli di applicazioni cloud ed eseguite in un contenitore o in un sistema operativo guest della macchina virtuale  
  
## <a name="important-differences-in-nano-server"></a>Differenze importanti in Nano Server

Poiché Nano Server è ottimizzato come sistema operativo leggero per l'esecuzione di applicazioni "cloud native" basate su contenitori e micro servizi o come host di data center agile e conveniente con un impatto notevolmente ridotto, esistono importanti differenze tra le installazioni Nano Server e Server Core o Server con Esperienza desktop:

- Nano Server è "headless;" non sono presenti funzionalità di accesso locale o interfacce utente grafiche.
- Sono supportati solo agenti, strumenti e applicazioni a 64 bit.
- Nano Server non può essere usato come controller di dominio di Active Directory.
- I Criteri di gruppo non sono supportati. È tuttavia possibile usare [Desired State Configuration](https://msdn.microsoft.com/powershell/dsc/nanoDsc) per applicare le impostazioni su vasta scala.
- Nano Server non può essere configurato per l'uso di un server proxy per accedere a Internet.
- Gruppo NIC, in particolare il bilanciamento del carico e il failover, non è supportato. È invece supportato Switch Embedded Teaming (SET).
- System Center Configuration Manager e System Center Data Protection Manager non sono supportati.
- I cmdlet Best Practices Analyzer (BPA) e l'integrazione di BPA con Server Manager non sono supportati.
- Nano Server non supporta le schede bus host (HBA) virtuali.
- Nano Server non deve essere attivato con un codice Product Key. Quando funge da host Hyper-V, Nano Server non supporta l'[attivazione automatica della macchina virtuale](https://technet.microsoft.com/library/dn303421%28v=ws.11%29.aspx) (AVMA). Le macchine virtuali in esecuzione in un host Nano Server possono essere attivate tramite il [Servizio di gestione delle chiavi](https://technet.microsoft.com/library/jj612867(v=ws.11).aspx) (KMS) con un codice Product Key per contratti multilicenza generico o mediante [l'attivazione basata su Active Directory](https://technet.microsoft.com/library/dn502534(v=ws.11).aspx).
- La versione di Windows PowerShell fornita con Nano Server presenta differenze importanti. Per informazioni dettagliate, vedere [PowerShell in Nano Server](PowerShell-on-Nano-Server.md).
- Nano Server è supportato solo sul modello CBB (Current Branch for Business). Attualmente non è disponibile alcuna versione LTSB (Long-Term Servicing Branch) per Nano Server. Per altre informazioni, vedere le sottosezioni seguenti.

### <a name="current-branch-for-business"></a>Current Branch for Business
Nano Server viene fornito con un modello più attivo, denominato CBB (Current Branch for Business), per supportare i clienti che stanno passando a una "pianificazione cloud" tramite cicli di sviluppo rapido. In questo modello, i rilasci degli aggiornamenti di funzionalità di Nano Server sono previsti due o tre volte l'anno. Questo modello richiede che [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx) per Nano Server sia distribuito e gestito nell'ambiente di produzione. Per gestire il supporto, gli amministratori devono usare CBB aggiornato ad almeno due versioni precedenti. Tuttavia, queste versioni non aggiornano automaticamente le distribuzioni esistenti; gli amministratori eseguono l'installazione manuale di una nuova versione di CBB in base alle esigenze. Per altre informazioni, vedere [Windows Server 2016 new Current Branch for Business servicing option](https://blogs.technet.microsoft.com/windowsserver/2016/07/12/windows-server-2016-new-current-branch-for-business-servicing-option/) (Nuove opzioni di manutenzione di Current Branch for Business per Windows Server 2016).

Le opzioni di installazione Server Core e Server con Esperienza desktop vengono ancora eseguite nel [modello LTSB (Long-Term Servicing Branch)](https://support.microsoft.com/lifecycle#gp%2Fgp_msl_policy), che comprende cinque anni di supporto Mainstream e cinque anni di supporto esteso.

## <a name="installation-scenarios"></a>Scenari di installazione

### <a name="evaluation"></a>Valutazione
È possibile ottenere una copia di valutazione con una licenza di 180 giorni di Windows Server da [Windows Server Valutazioni](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016). Per provare Nano Server, scegliere l' opzione **Nano Server | EXE a 64 bit** e quindi tornare a [Avvio rapido di Nano Server](Nano-Server-Quick-Start.md) o [Distribuire Nano Server](Deploy-Nano-Server.md) per iniziare.

### <a name="clean-installation"></a>Installazione pulita
Poiché l'installazione di Nano Server viene eseguita configurando un disco rigido virtuale, l'installazione pulita risulta essere il metodo di distribuzione più semplice e rapido.

- Per iniziare rapidamente con una distribuzione di base di Nano Server usando DHCP per ottenere un indirizzo IP, vedere [Avvio rapido di Nano Server](Nano-Server-Quick-Start.md). 
- Se si ha già familiarità con le nozioni di base di Nano Server, gli argomenti più dettagliati, partire da [Distribuire Nano Server](Deploy-Nano-Server.md), offrono un set completo di istruzioni per la personalizzazione delle immagini, l'utilizzo di domini, l'installazione di pacchetti per i ruoli del server, altre funzionalità online e offline e molto altro ancora.

> [!IMPORTANT]  
> Al termine dell'installazione e immediatamente dopo aver installato tutti i ruoli del server e le funzionalità necessarie, cercare e installare gli aggiornamenti disponibili per Windows Server 2016. Per Nano Server, vedere la sezione "Gestione degli aggiornamenti in Nano Server" di [Gestire Nano Server](Manage-Nano-Server.md).

### <a name="upgrade"></a>Aggiornamento
Poiché Nano Server è una novità di Windows Server 2016, non è disponibile un percorso di aggiornamento da versioni precedenti del sistema operativo di Nano Server.

### <a name="migration"></a>Migrazione
Poiché Nano Server è una novità di Windows Server 2016, non è disponibile un percorso di migrazione da versioni precedenti del sistema operativo di Nano Server.
  
-------------------------------------
Se è necessaria un'opzione di installazione diversa, è possibile passare alla [pagina principale di Windows Server 2016](windows-server-2016.md). 

  


 
