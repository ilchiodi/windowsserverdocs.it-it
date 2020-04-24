---
title: Ottimizzazione delle prestazioni dei server Web
description: Consigli per l'ottimizzazione delle prestazioni per i server Web in Windows Server 16
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: davso; ericam; yashi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: ec36d87957e5bbe897597e330e766c3193cd30d0
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80851694"
---
# <a name="performance-tuning-web-servers"></a>Ottimizzazione delle prestazioni dei server Web


Questo argomento illustra i metodi e fornisce consigli per ottimizzare le prestazioni per i server Web Windows Server 2016.


## <a name="selecting-the-proper-hardware-for-performance"></a>Selezione dell'hardware appropriato per le prestazioni


È importante selezionare l'hardware appropriato per soddisfare il carico Web previsto, considerando il carico medio, il picco di carico, la capacità, i piani di crescita e i tempi di risposta. I colli di bottiglia dell'hardware limitano l'efficacia dell'ottimizzazione a livello software.

In [Ottimizzazione delle prestazioni per l'hardware del server](../../hardware/index.md) vengono forniti consigli relativi all'hardware per evitare i vincoli seguenti per le prestazioni:

-   Le CPU lente offrono una potenza di elaborazione limitata per i carichi di lavoro con utilizzo intensivo della CPU, come nel caso degli scenari ASP, ASP.NET e TLS.

-   Una cache del processore di piccole dimensioni L2 o L3/LLC può incidere negativamente sulle prestazioni.

-   Una quantità limitata di memoria incide sul numero di siti che possono essere ospitati, sul numero di script di contenuto dinamico (come ASP.NET) che possono essere archiviati e sul numero di pool di applicazioni o di processi di lavoro.

-   Le funzionalità di rete diventano un collo di bottiglia a causa di una scheda di rete non efficiente.

-   Il file system diventa un collo di bottiglia a causa di un sottosistema disco o di un adattatore di archiviazione non efficiente.

## <a name="operating-system-best-practices"></a>Procedure consigliate per il sistema operativo


Se possibile, iniziare con un'installazione pulita del sistema operativo. L'aggiornamento del software può lasciare impostazioni del Registro di sistema obsolete, indesiderate o non ottimali, nonché applicazioni e servizi installati in precedenza che consumano risorse se vengono avviati automaticamente. Se è installato un altro sistema operativo che è necessario mantenere, è consigliabile installare il nuovo sistema operativo in una partizione diversa. In caso contrario, la nuova installazione sovrascrive le impostazioni in %Program Files%\\File comuni.

Per ridurre le interferenze in fase di accesso al disco, collocare il file di paging di sistema, il sistema operativo, i dati Web, la cache dei modelli ASP e il registro di Internet Information Services (IIS) su dischi fisici separati, se possibile.

Per ridurre la contesa per le risorse di sistema, installare Microsoft SQL Server e IIS in server diversi, se possibile.

Evitare di installare servizi e applicazioni non essenziali. In alcuni casi potrebbe essere utile disabilitare i servizi non necessari in un sistema.

## <a name="ntfs-file-system-settings"></a>Impostazioni del file system NTFS

L'opzione globale di sistema **NtfsDisableLastAccessUpdate** (REG\_DWORD) 1 si trova in **HKLM\\System\\CurrentControlSet\\Control\\FileSystem** e per impostazione predefinita è impostata su 1. Questa opzione riduce le latenze e il carico di I/O su disco disabilitando l'aggiornamento della data e del timestamp per l'ultimo accesso a file o directory. Per impostazione predefinita, le installazioni pulite di Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008 abilitano tale impostazione e non è necessario modificarla. Questa chiave non viene impostata dalle versioni precedenti di Windows. Se il server esegue una versione precedente di Windows o è stato aggiornato a Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008, è consigliabile abilitare questa impostazione.

La disabilitazione degli aggiornamenti è efficace quando si usano set di dati di grandi dimensioni (o molti host) contenenti migliaia di directory. È invece consigliabile usare la registrazione IIS se si conservano queste informazioni solo per l'amministrazione Web.

>[!Warning]
> Alcune applicazioni, ad esempio le utilità di backup incrementale, si basano su queste informazioni di aggiornamento e non funzionano correttamente senza di esse.

## <a name="see-also"></a>Vedi anche
- [Ottimizzazione delle prestazioni di IIS 10.0](tuning-iis-10.md)
- [Ottimizzazione di HTTP 1.1/2](http-performance.md)


