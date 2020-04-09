---
title: 'Ripristino della foresta di Active Directory: configurare il servizio server DNS'
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 144a45f2a835d9cca60b5be5aac7569809c45b7c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824174"
---
# <a name="ad-forest-recovery---configuring-the-dns-server-service"></a>Ripristino della foresta di Active Directory-configurazione del servizio server DNS

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Se il ruolo server DNS non è installato nel controller di dominio ripristinato dal backup, è necessario installare e configurare il server DNS. 

## <a name="install-and-configure-the-dns-server-service"></a>Installare e configurare il servizio server DNS

Completare questo passaggio per ogni controller di dominio ripristinato che non è in esecuzione come server DNS dopo il completamento del ripristino. 

> [!NOTE]
> Se il controller di dominio ripristinato dal backup esegue Windows Server 2008 R2, è necessario connettere il controller di dominio a una rete isolata per poter installare il server DNS. Connettere quindi ogni server DNS ripristinato a una rete isolata e condivisa a vicenda. Eseguire Repadmin/Replsum. per verificare che la replica sia funzionante tra i server DNS ripristinati. Dopo aver verificato la replica, è possibile connettere i controller di dominio ripristinati alla rete di produzione se il ruolo server DNS è già installato, è possibile applicare un hotfix che rende possibile l'avvio di un server DNS mentre il server non è connesso ad alcuna rete. È necessario integrare l'hotfix nell'immagine di installazione del sistema operativo durante i processi di compilazione automatici. Per ulteriori informazioni sull'hotfix, vedere l' [articolo 975654](https://go.microsoft.com/fwlink/?LinkId=184691) della Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=184691). 

Completare i passaggi di installazione e configurazione seguenti.

### <a name="to-install-and-the-dns-server-service-using-server-manager"></a>Per installare e il servizio server DNS utilizzando Server Manager  

1. Aprire Server Manager e fare clic su **Aggiungi ruoli e funzionalità**. 
2. Nell'Aggiunta guidata ruoli, se viene visualizzata la pagina **Prima di iniziare**, fare clic su **Avanti**. 
3. Nella schermata **tipo di installazione** selezionare **installazione basata su ruoli o basata su funzionalità** e quindi fare clic su **Avanti**.
4. Nella schermata **Selezione server** selezionare il server e fare clic su **Avanti**.
5. Nella schermata **ruoli server** selezionare **server DNS**, se richiesto, fare clic su **Aggiungi funzionalità** e quindi su **Avanti**.
6. Nella schermata **funzionalità** fare clic su **Avanti**.
7. Leggere le informazioni nella pagina **server DNS** e quindi fare clic su **Avanti**.
   Server DNS ![](media/AD-Forest-Recovery-Configure-DNS/dns1.png)  
8. Nella pagina **conferma** verificare che il ruolo server DNS venga installato e quindi fare clic su **Installa**. 

### <a name="to-configure-the-dns-server-service"></a>Per configurare il servizio server DNS

1. Aprire Server Manager, fare clic su **strumenti** e quindi su **DNS**.
   Server DNS ![](media/AD-Forest-Recovery-Configure-DNS/dns2.png)
2. Creare zone DNS per gli stessi nomi di dominio DNS ospitati nei server DNS prima del malfunzionamento critico. Per ulteriori informazioni, vedere la pagina relativa all'aggiunta di una zona di ricerca diretta ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).
3. Configurare i dati DNS esistenti prima del malfunzionamento critico. Ad esempio,  

   - Configurare le zone DNS da archiviare in servizi di dominio Active Directory. Per ulteriori informazioni, vedere Modificare il tipo di zona ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).
   - Configurare la zona DNS autorevole per i record di risorse del localizzatore controller di dominio (DC Locator) per consentire l'aggiornamento dinamico protetto. Per ulteriori informazioni, vedere Consenti solo aggiornamenti dinamici protetti ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).

4. Verificare che la zona DNS padre contenga i record di risorse di delega (server dei nomi (NS) e i record di risorse host (A) Glue) per la zona figlio ospitata in questo server DNS. Per ulteriori informazioni, vedere la pagina relativa alla creazione di una delega di zona ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).
5. Dopo aver configurato il DNS, è possibile velocizzare la registrazione dei record di NETLOGON.

   > [!NOTE]
   > Gli aggiornamenti dinamici protetti funzionano solo quando è disponibile un server di catalogo globale. 

   Al prompt dei comandi digitare il comando seguente, quindi premere INVIO:  

   **NET stop netlogon**  

6. Digitare il comando seguente e quindi premere INVIO:  

   **NET START NETLOGON**  

   ![Server DNS](media/AD-Forest-Recovery-Configure-DNS/dns3.png)  

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta di Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)
