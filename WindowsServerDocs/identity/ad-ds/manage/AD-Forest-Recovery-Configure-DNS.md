---
title: Ripristino della foresta Active Directory - servizio configurare Server DNS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2c37428a0fb685e6a7fa4875366f3cd13401bd9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842962"
---
# <a name="ad-forest-recovery---configuring-the-dns-server-service"></a>Ripristino della foresta Active Directory - configurazione del servizio Server DNS

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Se il ruolo server DNS non è installato nel controller di dominio che si ripristina da backup, è necessario installare e configurare il server DNS. 

## <a name="install-and-configure-the-dns-server-service"></a>Installare e configurare il servizio Server DNS

Completare questo passaggio per ogni controller di dominio ripristinato che non è in esecuzione come server DNS dopo aver completato il ripristino. 

> [!NOTE]
> Se il controller di dominio che è stato ripristinato da un backup è in esecuzione Windows Server 2008 R2, è necessario connettersi il controller di dominio a una rete isolata per installare il server DNS. Connettere quindi ogni dei server DNS ripristinato a una rete isolata, si escludono a vicenda condivisa. Esegue repadmin /replsum. per verificare che la replica stia funzionando tra i server DNS ripristinati. Dopo aver verificato la replica, è possibile connettersi i controller di dominio ripristinato nella rete di produzione, se il ruolo server DNS è già installato, è possibile applicare un hotfix che rende possibile per un server DNS avviare mentre il server non è connesso ad alcuna rete. È consigliabile integrare l'aggiornamento rapido nell'immagine di installazione del sistema operativo durante i processi di compilazione automatizzato. Per altre informazioni sull'hotfix, vedere [articolo 975654](https://go.microsoft.com/fwlink/?LinkId=184691) della Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=184691). 

Completare la procedura di installazione e la configurazione seguente.

### <a name="to-install-and-the-dns-server-service-using-server-manager"></a>Installare e il servizio Server DNS utilizzando Server Manager  

1. Aprire Server Manager e fare clic su **Aggiungi ruoli e funzionalità**. 
2. Nell'Aggiunta guidata ruoli, se viene visualizzata la pagina **Prima di iniziare**, fare clic su **Avanti**. 
3. Nel **tipo di installazione** schermata selezionare **basata su ruoli o installazione basata su funzionalità** e fare clic su **Avanti**.
4. Nel **selezione del Server** dello schermo selezionare il server e fare clic su **successivo**.
5. Nel **ruoli predefiniti del Server** schermata select **Server DNS**, se viene richiesto fare clic su **Aggiungi funzionalità** e fare clic su **successivo**.
6. Nel **caratteristiche** dello schermo fare clic su **successivo**.
7. Leggere le informazioni nella **Server DNS** pagina e quindi fare clic su **successivo**.
   ![Server DNS](media/AD-Forest-Recovery-Configure-DNS/dns1.png)  
8. Nel **conferma** pagina, verificare che il ruolo Server DNS verrà installato e quindi fare clic su **installare**. 

### <a name="to-configure-the-dns-server-service"></a>Per configurare il servizio Server DNS

1. Aprire Server Manager, fare clic su **degli strumenti** e fare clic su **DNS**.
   ![Server DNS](media/AD-Forest-Recovery-Configure-DNS/dns2.png)
2. Creare zone DNS per gli stessi nomi di dominio DNS ospitate sui server DNS prima il malfunzionamento critico. Per altre informazioni, vedere Aggiunta di una zona di ricerca diretta ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).
3. Configurare i dati DNS esistente prima di malfunzionamento critico. Ad esempio:  

   - Configurare le zone DNS di essere archiviati in Active Directory Domain Services. Per altre informazioni, vedere Modifica il tipo di zona ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).
   - Configurare la zona DNS autorevole per record di risorse (DC Locator) del localizzatore di controller di dominio consentire l'aggiornamento dinamico sicuro. Per altre informazioni, vedere consentire solo aggiornamenti dinamici protetti ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).

4. Verificare che la zona DNS padre contenga risorse record di delega (assegna un nome server (NS) e glue host (A) resource record) per la zona figlio che è ospitato in questo server DNS. Per altre informazioni, vedere come creare una delega di Zone ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).
5. Dopo la configurazione DNS, è possibile velocizzare la registrazione dei record di NETLOGON.

   > [!NOTE]
   > Gli aggiornamenti dinamici protetti funzionano solo quando è disponibile un server di catalogo globale. 

   Al prompt dei comandi digitare il comando seguente e quindi premere INVIO:  

   **net stop netlogon**  

6. Digitare il comando seguente e quindi premere INVIO:  

   **comando Net start netlogon**  

   ![Server DNS](media/AD-Forest-Recovery-Configure-DNS/dns3.png)  

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta AD](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
