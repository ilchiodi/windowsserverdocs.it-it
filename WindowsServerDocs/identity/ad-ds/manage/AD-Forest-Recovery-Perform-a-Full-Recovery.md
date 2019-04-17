---
title: Ripristino della foresta Active Directory - eseguire un ripristino completo del server
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 1a1182a6-4462-4a13-806e-0e642a0d5db2
ms.technology: identity-adfs
ms.openlocfilehash: 5de71cc005e9ec4c1425f9fa805b3c043ab4ea36
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---performing-a-full-server-recovery"></a>Ripristino della foresta Active Directory - eseguire un ripristino completo del server 

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2
 
Utilizzare la procedura seguente per eseguire un ripristino completo del server per Windows Server 2016, 2012 R2 o 2012. 

## <a name="active-directory-full-server-recovery"></a>Ripristino completo del Server di Active Directory
Un ripristino completo del server è necessario se si desidera ripristinare per diversi tipi di hardware o un'istanza diversa del sistema operativo. Tenere presente quanto segue:

- Il numero di unità nelle esigenze di un server di destinazione sia uguale al numero nel backup e devono essere lo stesso dimensioni o versione successiva.
- Il server di destinazione deve essere avviato dal DVD del sistema operativo per eseguire l'accesso di **Ripristina il computer** opzione. 
- Se il controller di dominio è in esecuzione in una macchina virtuale in Hyper-V e il backup di destinazione è archiviato in un percorso di rete, è necessario installare una scheda di rete legacy.  
- Dopo aver eseguito un ripristino completo del server, è necessario eseguire separatamente un ripristino autorevole di SYSVOL, come descritto in [ripristino della foresta Active Directory - esecuzione di una sincronizzazione autorevole di SYSVOL con replica DFS](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).


A seconda della situazione, utilizzare una delle seguenti procedure per eseguire un ripristino completo.  
  
### <a name="perform-a-full-server-restore-with-a-local-backup-with-the-latest-image"></a>Eseguire un ripristino completo del server con una copia di backup locale con l'immagine più recente
  
1.  Avviare l'installazione di Windows, specificare il linguaggio, ora e valuta formato e opzioni della tastiera e fare clic su **Avanti**.  
2.  Fare clic su **Ripristina il computer**.</br>
![Ripristino del server](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore1.png)
3.  Fare clic su **risolvere**.</br>
![Ripristino del server](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore2.png)
4.  Fare clic su **Ripristino immagine del sistema**.</br>
![Ripristino del server](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore3.png)
5.  Fare clic su **Windows Server 2016**.  
![Ripristino del server](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore4.png)
6.  Se si desidera ripristinare il backup più recente locale, fare clic su **utilizzare l'immagine di sistema più recente (scelta consigliata)** e fare clic su **Avanti**.
![Ripristino del server](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore5.png)
7.  È ora possibile un'opzione per:
    -  Formatta e partiziona i dischi
    -  Installare i driver
    -  Annullare la selezione di **avanzate** funzionalità di automaticamente il riavvio e cercare gli errori sul disco.  Queste sono abilitate per impostazione predefinita.
![Ripristino del server](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore6.png)
8. Fare clic su **Avanti**.
9. Fare clic su **fine **.  Verrà richiesto in cui viene chiesto se si è certi che si desidera continuare.  Fare clic su **Sì**.  
![Ripristino del server](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore11.png) 
10. Una volta completata l'operazione eseguire un ripristino autorevole di SYSVOL, come descritto in [ripristino della foresta Active Directory - esecuzione di una sincronizzazione autorevole di SYSVOL con replica DFS](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).
 

### <a name="perform-a-full-server-restore-with-any-image-local-or-remote"></a>Eseguire un ripristino completo del server con un'immagine locale o remoto
1.  Avviare l'installazione di Windows, specificare il linguaggio, ora e valuta formato e opzioni della tastiera e fare clic su **Avanti**.  
2.  Fare clic su **Ripristina il computer**.</br>
3.  Fare clic su **risoluzione dei problemi**, fare clic su **Ripristino immagine del sistema**e fare clic su **Windows Server 2016**.  
4.  Se si desidera ripristinare il backup più recente locale, fare clic su **seleziona un'immagine del sistema** e fare clic su **Avanti**.

5.  Ora è possibile selezionare il percorso di backup che si desidera ripristinare.  Se l'immagine è locale è possibile selezionarlo dall'elenco.  
6.  Se l'immagine si trova in una condivisione di rete, selezionare **avanzate**.  È inoltre possibile selezionare **avanzate** se è necessario installare un driver.
![Ripristino del server](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore7.png)
7.  Se si desidera ripristinare dalla rete dopo aver fatto clic **avanzate** selezionare **cercare un'immagine del sistema in rete**.  Potrebbe essere richiesto per ripristinare la connettività di rete.  Fare clic su Ok. </br>
![Ripristino del server](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore8.png)
8. Digitare il percorso UNC per il percorso della condivisione di backup (ad esempio, \\\server1\backups) e fare clic su **OK**.  È anche possibile digitare l'indirizzo IP del server di destinazione, ad esempio \\\192.168.1.3\backups.  
![Ripristino del server](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore9.png)
10. Digitare le credenziali necessarie per accedere alla condivisione e fare clic su OK.  
11. Ora **selezionare la data e ora dell'immagine del sistema per ripristinare** e fare clic su **Avanti**.
12. È ora possibile un'opzione per:
    1.   Formatta e partiziona i dischi
    2.   Installare i driver
    3.   Annullare la selezione di **avanzate** funzionalità di automaticamente il riavvio e cercare gli errori sul disco.  Queste sono abilitate per impostazione predefinita.
13. Fare clic su **Avanti**.
14. Fare clic su **fine **.  Verrà richiesto in cui viene chiesto se si è certi che si desidera continuare.  Fare clic su **Sì**.   
15. Una volta completata l'operazione eseguire un ripristino autorevole di SYSVOL, come descritto in [ripristino della foresta Active Directory - esecuzione di una sincronizzazione autorevole di SYSVOL con replica DFS](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).


### <a name="enabling-the-network-adapter-for-a-network-backup"></a>Abilitare la scheda di rete per un backup di rete
Se è necessario abilitare una scheda di rete dal prompt dei comandi eseguire il ripristino da una condivisione di rete, utilizzare la procedura seguente.

1.  Avviare l'installazione di Windows, specificare il linguaggio, ora e valuta formato e opzioni della tastiera e fare clic su **Avanti**.  
2.  Fare clic su **Ripristina il computer**. Posso
3.  Fare clic su **risoluzione dei problemi**, fare clic su **prompt dei comandi**.  
4.  Digitare il comando seguente e premere INVIO:  
  
    ```  
    wpeinit  
    ```   
5.  Per verificare il nome della scheda di rete, digitare:  
  
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
  
     Per esempio:  
  
    ```  
    set address "Local Area Connection" static 192.168.1.2 255.0.0.0 192.168.1.1 1  
    ```  
  
     Tipo `quit`per tornare a un prompt dei comandi. Tipo `ipconfig /all`per verificare la rete scheda ha un indirizzo IP e provare a effettuare il ping l'indirizzo IP del server che ospita la condivisione di backup per verificare la connettività. Al termine, chiudere il prompt dei comandi.  
  
6.  Ora in cui funziona la scheda di rete, selezionare i passaggi precedenti per completare il ripristino.

## <a name="next-steps"></a>Passaggi successivi

- [Guida di ripristino della foresta Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
