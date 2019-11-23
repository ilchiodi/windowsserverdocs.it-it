---
title: 'Ripristino della foresta di Active Directory: esecuzione di un ripristino completo del server'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 1a1182a6-4462-4a13-806e-0e642a0d5db2
ms.technology: identity-adds
ms.openlocfilehash: 1ade1f2e316387fbe84209c1bc7a986fff6f2a71
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390544"
---
# <a name="ad-forest-recovery---performing-a-full-server-recovery"></a>Ripristino della foresta di Active Directory: esecuzione di un ripristino completo del server 

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Utilizzare la procedura seguente per eseguire un ripristino completo del server per Windows Server 2016, 2012 R2 o 2012. 

## <a name="active-directory-full-server-recovery"></a>Active Directory il ripristino completo del server

Un ripristino completo del server è necessario se si esegue il ripristino in un hardware diverso o in un'istanza diversa del sistema operativo. Tieni presenti i punti seguenti:

- Il numero di unità nel server di destinazione deve essere uguale al numero nel backup e deve essere di dimensioni uguali o superiori.
- Per accedere all'opzione **Ripristina il computer** , il server di destinazione deve essere avviato dal DVD del sistema operativo. 
- Se il controller di dominio di destinazione è in esecuzione in una macchina virtuale in Hyper-V e il backup viene archiviato in un percorso di rete, è necessario installare una scheda di rete legacy. 
- Dopo aver eseguito un ripristino completo del server, è necessario eseguire separatamente un ripristino autorevole di SYSVOL, come descritto nel [ripristino della foresta di Active Directory-esecuzione di una sincronizzazione autorevole di SYSVOL con replica di DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

A seconda dello scenario, utilizzare una delle procedure riportate di seguito per eseguire un ripristino completo. 
  
## <a name="perform-a-full-server-restore-with-a-local-backup-with-the-latest-image"></a>Eseguire un ripristino completo del server con un backup locale con l'immagine più recente
  
1. Avviare Installazione di Windows, specificare il formato di lingua, ora e valuta e le opzioni della tastiera, quindi fare clic su **Avanti**. 
2. Fai clic su **Ripristina il computer**.
   Ripristino del server ![](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore1.png)
3. Fai clic su **Risoluzione dei problemi**.</br>
   Ripristino del server ![](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore2.png)
4. Fare clic su **Ripristino immagine del sistema**.</br>
   Ripristino del server ![](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore3.png)
5. Fare clic su **Windows Server 2016**. 
   Ripristino del server ![](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore4.png)
6. Se si ripristina il backup locale più recente, fare clic su **Usa l'immagine del sistema più recente disponibile (scelta consigliata)** e fare clic su **Avanti**.
   Ripristino del server ![](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore5.png)
7. A questo punto, verrà fornita un'opzione per:
   -  Formattare e partizionare i dischi
   -  Installare i driver
   -  Deselezionare le funzionalità **Avanzate** per il riavvio automatico e il controllo degli errori del disco. Queste sono abilitate per impostazione predefinita.
   Ripristino del server ![](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore6.png)
8. Fai clic su **Next**.
9. Fare clic su **Fine**. Verrà chiesto se si è certi di voler continuare. fare clic su **Sì**. 
   Ripristino del server ![](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore11.png) 
10. Al termine dell'operazione, eseguire un ripristino autorevole di SYSVOL, come descritto in [ad Forest Recovery-esecuzione di una sincronizzazione autorevole di DFSR-Replicated SYSVOL](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

## <a name="perform-a-full-server-restore-with-any-image-local-or-remote"></a>Eseguire un ripristino completo del server con qualsiasi immagine locale o remota

1. Avviare Installazione di Windows, specificare il formato di lingua, ora e valuta e le opzioni della tastiera, quindi fare clic su **Avanti**. 
2. Fai clic su **Ripristina il computer**.</br>
3. Fare clic su **risoluzione dei problemi**, fare clic su **Ripristino immagine del sistema**e quindi su **Windows Server 2016**. 
4. Se si ripristina il backup locale più recente, fare clic su **selezionare un'immagine del sistema** e fare clic su **Avanti**.
5. A questo punto è possibile selezionare il percorso del backup che si desidera ripristinare. Se l'immagine è locale, è possibile selezionarla dall'elenco. 
6. Se l'immagine si trova in una condivisione di rete, selezionare **Avanzate**. È anche possibile selezionare **Avanzate** se è necessario installare un driver.
   Ripristino del server ![](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore7.png)
7. Se si esegue il ripristino dalla rete dopo aver fatto clic su **Avanzate** , selezionare **Cerca un'immagine del sistema in rete**. Potrebbe essere richiesto di ripristinare la connettività di rete. Fare clic su OK. </br>
   Ripristino del server ![](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore8.png)
8. Digitare il percorso UNC del percorso della condivisione di backup, ad esempio \\\server1\backups, quindi fare clic su **OK**. È anche possibile digitare l'indirizzo IP del server di destinazione, ad esempio \\\192.168.1.3\backups. 
   Ripristino del server ![](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore9.png)
9. Digitare le credenziali necessarie per accedere alla condivisione e fare clic su OK. 
10. **Selezionare ora la data e l'ora dell'immagine del sistema da ripristinare** e fare clic su **Avanti**.
11. A questo punto, verrà fornita un'opzione per:
    - Formattare e partizionare i dischi
    - Installare i driver
    - Deselezionare le funzionalità **Avanzate** per il riavvio automatico e il controllo degli errori del disco. Queste sono abilitate per impostazione predefinita.
12. Fai clic su **Next**.
13. Fare clic su **Fine**. Verrà chiesto se si è certi di voler continuare. fare clic su **Sì**.  
14. Al termine dell'operazione, eseguire un ripristino autorevole di SYSVOL, come descritto in [ad Forest Recovery-esecuzione di una sincronizzazione autorevole di DFSR-Replicated SYSVOL](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

## <a name="enabling-the-network-adapter-for-a-network-backup"></a>Abilitazione della scheda di rete per un backup di rete

Se è necessario abilitare una scheda di rete dal prompt dei comandi per eseguire il ripristino da una condivisione di rete, attenersi alla procedura riportata di seguito.

1. Avviare Installazione di Windows, specificare il formato di lingua, ora e valuta e le opzioni della tastiera, quindi fare clic su **Avanti**. 
2. Fai clic su **Ripristina il computer**. I
3. Fare clic su **Risolvi**, quindi su **prompt dei comandi**. 
4. Digitare il comando seguente e quindi premere INVIO:  

   ```  
   wpeinit  
   ```

5. Per confermare il nome della scheda di rete, digitare:  

   ```  
   show interfaces  
   ```  

   Digitare i comandi seguenti e premere INVIO dopo ogni comando:  

   ```  
   netsh  
   ```  

   ```  
   interface  
   ```  
  
   ```  
   tcp  
   ```  

   ```  
   ipv4  
   ```  
  
   ```  
   set address "Name of Network Adapter" static IPv4 Address SubnetMask IPv4 Gateway Address 1  
   ```  

   Ad esempio:  
  
   ```  
   set address "Local Area Connection" static 192.168.1.2 255.0.0.0 192.168.1.1 1  
   ```  

   Digitare `quit` per tornare al prompt dei comandi. Digitare `ipconfig /all` per verificare che la scheda di rete disponga di un indirizzo IP e provare a effettuare il ping dell'indirizzo IP del server che ospita la condivisione di backup per confermare la connettività. Al termine, chiudere il prompt dei comandi. 

6. Ora che la scheda di rete funziona, selezionare i passaggi precedenti per completare il ripristino.

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta di Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)
