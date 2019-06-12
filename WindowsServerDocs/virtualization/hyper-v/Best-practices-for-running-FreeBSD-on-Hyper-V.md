---
title: Procedure consigliate per l'esecuzione di FreeBSD in Hyper-V
description: Fornisce indicazioni per l'esecuzione di FreeBSD in macchine virtuali
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c66f1c8-2606-43a3-b4cc-166acaaf2d2a
author: shirgall
ms.author: kathydav
ms.date: 01/09/2017
ms.openlocfilehash: 6320ceb86093146592a54ab34b013f334f43ddb4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447771"
---
# <a name="best-practices-for-running-freebsd-on-hyper-v"></a>Procedure consigliate per l'esecuzione di FreeBSD in Hyper-V

>Si applica a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

In questo argomento contiene un elenco di raccomandazioni per l'esecuzione di FreeBSD come sistema operativo guest in una macchina virtuale Hyper-V.

## <a name="enable-carp-in-freebsd-102-on-hyper-v"></a>Abilitare CARP in FreeBSD 10.2 in Hyper-V

Il Common indirizzo ridondanza protocollo CARP () consente a più host da condividere lo stesso indirizzo IP e virtuale Host ID (VHID) per garantire un'elevata disponibilità per uno o più servizi. Se uno o più host non riescono, gli altri host in modo trasparente intervenire in modo che gli utenti non noteranno un errore del servizio. Per usare CARP in FreeBSD 10.2, seguire le istruzioni di [manuale di FreeBSD](https://www.freebsd.org/doc/en/books/handbook/carp.html) effettuare i passaggi seguenti nella console di gestione Hyper-V.

* Verificare la macchina virtuale ha una scheda di rete e ha assegnato un commutatore virtuale. Selezionare la macchina virtuale e selezionare **azioni** > **impostazioni**.

![Screenshot delle impostazioni di macchine virtuali con scheda di rete selezionata](media/Hyper-V_Settings_NetworkAdapter.png)

* Abilitare lo spoofing degli indirizzi MAC. A tale scopo,

   1. Selezionare la macchina virtuale e selezionare **azioni** > **impostazioni**.

   2. Espandere **scheda di rete** e selezionare **funzionalità avanzate**.

   3. Selezionare **lo spoofing degli indirizzi MAC abilitare**.

## <a name="create-labels-for-disk-devices"></a>Creare le etichette per i dispositivi disco

Durante l'avvio, i nodi di dispositivo vengono creati come vengono individuati i nuovi dispositivi. Questo può significare che possono modificare i nomi dei dispositivi quando vengono aggiunti nuovi dispositivi. Se si verifica un errore di montaggio radice durante l'avvio, è necessario creare le etichette per ogni partizione IDE evitare conflitti e modifiche. Per altre informazioni, vedere [assegnazione di etichette dispositivi disco](https://www.freebsd.org/doc/handbook/geom-glabel.html). Di seguito sono riportati esempi. 

> [!IMPORTANT]
> Creare una copia di backup di fstab prima di apportare modifiche.

1. Riavviare il sistema in modalità utente singolo. Questa operazione può essere eseguita selezionando l'opzione di menu di avvio 2 per FreeBSD 10.3 e versioni successive (opzione 4 per FreeBSD 8. x), o l'esecuzione di un "-s di avvio' dal prompt di avvio.

2. In modalità utente singolo, creare le etichette di geometria per ognuna delle partizioni del disco IDE elencate nella finestra di fstab (radice e lo scambio). Di seguito è riportato un esempio di FreeBSD 10.3.

   ```bash
   # cat  /etc/fstab
   # Device           Mountpoint      FStype  Options   Dump   Pass#
   /dev/da0p2         /               ufs     rw        1       1
   /dev/da0p3         none            swap    sw        0       0

   # glabel  label rootfs  /dev/da0p2
   # glabel  label swap   /dev/da0p3
   # exit
   ```

   Informazioni aggiuntive sulle etichette di geometria sono reperibile in: [L'assegnazione di etichette dispositivi disco](https://www.freebsd.org/doc/handbook/geom-glabel.html).

3. Il sistema continua con l'avvio di più utenti. Dopo aver completato il processo di avvio, modificare /etc/fstab e sostituire i nomi dei dispositivi convenzionali, con le rispettive etichette. /Etc/fstab. finale avrà un aspetto simile al seguente:

   ```
   # Device                Mountpoint      FStype  Options         Dump    Pass#
   /dev/label/rootfs       /               ufs     rw              1       1
   /dev/label/swap         none            swap    sw              0       0
   ```

4. È ora possibile riavviare il sistema. Se tutto ciò che ebbe successo, viene attivata la norma e montaggio mostrerà:

   ```
   # mount
   /dev/label/rootfs on / (ufs, local, journaled soft-updates)
   devfs on /dev (devfs, local, mutilabel)
   ```

## <a name="use-a-wireless-network-adapter-as-the-virtual-switch"></a>Utilizzare una scheda di rete del commutatore virtuale

Se il commutatore virtuale sull'host è basato sulla scheda di rete wireless, ridurre i tempi di scadenza ARP su 60 secondi per il comando seguente. In caso contrario, la funzionalità di rete della macchina virtuale smette di funzionare dopo un periodo di tempo.


```
   # sysctl net.link.ether.inet.max_age=60
```


Vedere anche

* [Macchine virtuali FreeBSD supportate in Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)
