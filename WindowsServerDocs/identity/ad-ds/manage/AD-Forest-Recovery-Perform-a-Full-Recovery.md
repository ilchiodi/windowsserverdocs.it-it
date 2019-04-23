---
title: Ripristino della foresta Active Directory - esecuzione di un ripristino completo del server
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 1a1182a6-4462-4a13-806e-0e642a0d5db2
ms.technology: identity-adds
ms.openlocfilehash: 6f600ade3d07130d4e1fb3b1a254cb1073f592e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874232"
---
# <a name="ad-forest-recovery---performing-a-full-server-recovery"></a>Ripristino della foresta Active Directory - esecuzione di un ripristino completo del server 

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Utilizzare la procedura seguente per eseguire un ripristino completo del server per Windows Server 2016, 2012 R2 o 2012. 

## <a name="active-directory-full-server-recovery"></a>Ripristino completo del Server di Active Directory

Un ripristino completo del server è necessario se esegue il ripristino in un hardware diverso o in un'istanza diversa del sistema operativo. Tieni presenti i punti seguenti:

- Il numero di unità sulle esigenze di un server di destinazione sia uguale al numero nel backup e devono corrispondere a dimensioni pari o superiore.
- Il server di destinazione deve essere avviato dal DVD del sistema operativo per poter accedere al **Ripristina il computer** opzione. 
- Se la destinazione di che controller di dominio è in esecuzione in una macchina virtuale in Hyper-V e il backup viene archiviata in un percorso di rete, è necessario installare una scheda di rete legacy. 
- Dopo aver eseguito un ripristino completo del server, è necessario eseguire separatamente un ripristino autorevole di SYSVOL, come descritto in [ripristino della foresta Active Directory - esecuzione di una sincronizzazione autorevole di DFSR-replicated SYSVOL](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

A seconda dello scenario, usare una delle procedure seguenti per eseguire un ripristino completo. 
  
## <a name="perform-a-full-server-restore-with-a-local-backup-with-the-latest-image"></a>Eseguire un ripristino completo del server con un backup locale con l'immagine più recente
  
1. Avviare l'installazione di Windows, specificare la lingua, ora e formato di valuta e opzioni della tastiera e fare clic su **successivo**. 
2. Fai clic su **Ripristina il computer**.
   ![Ripristino del server](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore1.png)
3. Fai clic su **Risoluzione dei problemi**.</br>
   ![Ripristino del server](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore2.png)
4. Fare clic su **Ripristino immagine del sistema**.</br>
   ![Ripristino del server](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore3.png)
5. Fare clic su **Windows Server 2016**. 
   ![Ripristino del server](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore4.png)
6. Se si sta ripristinando i backup locali più recenti, fare clic su **usare l'immagine di sistema più recente (scelta consigliata)** e fare clic su **successivo**.
   ![Ripristino del server](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore5.png)
7. A questo punto sarà possibile un'opzione per:
   -  Formatta e partiziona i dischi
   -  Installare i driver
   -  Deselezionare i **avanzate** le funzionalità di automaticamente il riavvio e la ricerca di errori del disco. Queste opzioni sono abilitate per impostazione predefinita.
   ![Ripristino del server](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore6.png)
8. Fare clic su **Avanti**.
9. Scegliere **Fine**. Verrà richiesto che chiede se si è certi che si desidera continuare. Scegliere **Sì**. 
   ![Ripristino del server](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore11.png) 
10. Al termine, eseguire un ripristino autorevole di SYSVOL, come descritto nella [ripristino della foresta Active Directory - esecuzione di una sincronizzazione autorevole di DFSR-replicated SYSVOL](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

## <a name="perform-a-full-server-restore-with-any-image-local-or-remote"></a>Eseguire un ripristino completo del server con qualsiasi immagine locale o remoto

1. Avviare l'installazione di Windows, specificare la lingua, ora e formato di valuta e opzioni della tastiera e fare clic su **successivo**. 
2. Fai clic su **Ripristina il computer**.</br>
3. Fare clic su **risoluzione dei problemi**, fare clic su **Ripristino immagine del sistema**, fare clic su **Windows Server 2016**. 
4. Se si sta ripristinando i backup locali più recenti, fare clic su **selezionare un'immagine del sistema** e fare clic su **successivo**.
5. A questo punto è possibile selezionare la posizione del backup che si desidera ripristinare. Se l'immagine è locale è possibile selezionarlo dall'elenco. 
6. Se l'immagine si trova in una condivisione di rete, selezionare **avanzate**. È anche possibile selezionare **avanzate** se è necessario installare un driver.
   ![Ripristino del server](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore7.png)
7. Se si esegue il ripristino dalla rete dopo aver fatto clic **avanzate** selezionate **Cerca immagine del sistema in rete**. Potrebbe essere richiesto di ripristinare la connettività di rete. Fare clic su Ok. </br>
   ![Ripristino del server](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore8.png)
8. Digitare il percorso UNC per il percorso di condivisione di backup (ad esempio, \\\server1\backups) e fare clic su **OK**. È anche possibile digitare l'indirizzo IP del server di destinazione, ad esempio \\\192.168.1.3\backups. 
   ![Ripristino del server](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore9.png)
10. Digitare le credenziali necessarie per accedere alla condivisione e fare clic su OK. 
11. A questo punto **selezionare la data e ora dell'immagine del sistema da ripristinare** e fare clic su **successivo**.
12. A questo punto sarà possibile un'opzione per:
   - Formatta e partiziona i dischi
   - Installare i driver
   - Deselezionare i **avanzate** le funzionalità di automaticamente il riavvio e la ricerca di errori del disco. Queste opzioni sono abilitate per impostazione predefinita.
13. Fare clic su **Avanti**.
14. Scegliere **Fine**. Verrà richiesto che chiede se si è certi che si desidera continuare. Scegliere **Sì**.  
15. Al termine, eseguire un ripristino autorevole di SYSVOL, come descritto nella [ripristino della foresta Active Directory - esecuzione di una sincronizzazione autorevole di DFSR-replicated SYSVOL](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

## <a name="enabling-the-network-adapter-for-a-network-backup"></a>Abilitare la scheda di rete per un backup di rete

Se è necessario abilitare una scheda di rete dal prompt dei comandi eseguire il ripristino da una condivisione di rete, utilizzare la procedura seguente.

1. Avviare l'installazione di Windows, specificare la lingua, ora e formato di valuta e opzioni della tastiera e fare clic su **successivo**. 
2. Fai clic su **Ripristina il computer**. I
3. Fare clic su **risoluzione dei problemi**, fare clic su **prompt dei comandi**. 
4. Digitare il comando seguente e quindi premere INVIO:  

   ```  
   wpeinit  
   ```

5. Per verificare che il nome della scheda di rete, digitare:  

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

   Tipo `quit` per tornare al prompt dei comandi. Tipo `ipconfig /all` a verificare la rete scheda ha un indirizzo IP e si tenta il ping dell'indirizzo IP del server che ospita la condivisione di backup per verificare la connettività. Al termine, chiudere il prompt dei comandi. 

6. A questo punto in cui funziona la scheda di rete, selezionare la procedura descritta sopra per completare il ripristino.

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta AD](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
