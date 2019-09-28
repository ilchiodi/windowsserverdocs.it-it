---
title: Procedure consigliate per l'esecuzione di FreeBSD in Hyper-V
description: Fornisce consigli per l'esecuzione di FreeBSD in macchine virtuali
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c66f1c8-2606-43a3-b4cc-166acaaf2d2a
author: shirgall
ms.author: kathydav
ms.date: 01/09/2017
ms.openlocfilehash: 1d284b38e1bdb642aa40ecbb8e82caa7712f7aad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365634"
---
# <a name="best-practices-for-running-freebsd-on-hyper-v"></a>Procedure consigliate per l'esecuzione di FreeBSD in Hyper-V

>Si applica a: Windows Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

Questo argomento contiene un elenco di consigli per l'esecuzione di FreeBSD come sistema operativo guest in una macchina virtuale Hyper-V.

## <a name="enable-carp-in-freebsd-102-on-hyper-v"></a>Abilitare CARP in FreeBSD 10,2 in Hyper-V

Il protocollo di ridondanza degli indirizzi comuni (CARP) consente a più host di condividere lo stesso indirizzo IP e l'ID host virtuale (VHID) per garantire la disponibilità elevata per uno o più servizi. Se uno o più host hanno esito negativo, gli altri host accettano in modo trasparente gli eventuali errori del servizio. Per usare CARP in FreeBSD 10,2, seguire le istruzioni nel [Manuale di FreeBSD](https://www.freebsd.org/doc/en/books/handbook/carp.html) ed eseguire le operazioni seguenti nella console di gestione di Hyper-V.

* Verificare che la macchina virtuale disponga di una scheda di rete a cui è assegnato un Commuter virtuale. Selezionare la macchina virtuale e selezionare **azioni** > **Impostazioni**.

![Screenshot delle impostazioni della macchina virtuale con la scheda di rete selezionata](media/Hyper-V_Settings_NetworkAdapter.png)

* Abilitare lo spoofing degli indirizzi MAC. Per eseguire questa operazione,

   1. Selezionare la macchina virtuale e selezionare **azioni** > **Impostazioni**.

   2. Espandere **scheda di rete** e selezionare **funzionalità avanzate**.

   3. Selezionare **Abilita spoofing indirizzi Mac**.

## <a name="create-labels-for-disk-devices"></a>Creare etichette per i dispositivi disco

Durante l'avvio, i nodi del dispositivo vengono creati quando vengono individuati nuovi dispositivi. Questo può significare che i nomi dei dispositivi possono cambiare quando vengono aggiunti nuovi dispositivi. Se si verifica un errore di montaggio principale durante l'avvio, è necessario creare etichette per ogni partizione IDE per evitare conflitti e modifiche. Per informazioni, vedere [assegnazione di etichette ai dispositivi disco](https://www.freebsd.org/doc/handbook/geom-glabel.html). Di seguito sono riportati alcuni esempi. 

> [!IMPORTANT]
> Eseguire una copia di backup del fstab prima di apportare qualsiasi modifica.

1. Riavviare il sistema in modalità utente singolo. Questa operazione può essere eseguita selezionando l'opzione del menu di avvio 2 per FreeBSD 10.3 + (opzione 4 per FreeBSD 8. x) o eseguendo "boot-s" dal prompt di avvio.

2. In modalità utente singolo creare etichette GEOM per ogni partizione del disco IDE elencata in fstab (radice e scambio). Di seguito è riportato un esempio di FreeBSD 10,3.

   ```bash
   # cat  /etc/fstab
   # Device           Mountpoint      FStype  Options   Dump   Pass#
   /dev/da0p2         /               ufs     rw        1       1
   /dev/da0p3         none            swap    sw        0       0

   # glabel  label rootfs  /dev/da0p2
   # glabel  label swap   /dev/da0p3
   # exit
   ```

   Altre informazioni sulle etichette GEOM sono disponibili all'indirizzo: [Assegnazione di etichette ai dispositivi disco](https://www.freebsd.org/doc/handbook/geom-glabel.html).

3. Il sistema continuerà con l'avvio multiutente. Al termine dell'avvio, modificare/etc/fstab e sostituire i nomi di dispositivo convenzionali con le rispettive etichette. Il/etc/fstab finale sarà simile al seguente:

   ```
   # Device                Mountpoint      FStype  Options         Dump    Pass#
   /dev/label/rootfs       /               ufs     rw              1       1
   /dev/label/swap         none            swap    sw              0       0
   ```

4. Il sistema può ora essere riavviato. Se tutto funzionasse correttamente, il montaggio verrà visualizzato in genere:

   ```
   # mount
   /dev/label/rootfs on / (ufs, local, journaled soft-updates)
   devfs on /dev (devfs, local, mutilabel)
   ```

## <a name="use-a-wireless-network-adapter-as-the-virtual-switch"></a>Usare una scheda di rete wireless come Commuter virtuale

Se il commutire virtuale nell'host è basato su una scheda di rete wireless, ridurre l'ora di scadenza ARP a 60 secondi con il comando seguente. In caso contrario, la rete della VM potrebbe smettere di funzionare dopo un periodo di tempo.


```
   # sysctl net.link.ether.inet.max_age=60
```


Vedere anche

* [Macchine virtuali FreeBSD supportate in Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)
