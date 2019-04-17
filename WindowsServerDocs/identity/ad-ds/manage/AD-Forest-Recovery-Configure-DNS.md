---
title: Ripristino della foresta Active Directory - il servizio di configurazione Server DNS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f24570965fd8b3f3e050779c42758865cbee2728
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---configuring-the-dns-server-service"></a>Ripristino della foresta Active Directory - configurazione del servizio Server DNS 

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2
 
Se il ruolo server DNS non è installato sul controller di dominio che si ripristina da backup, è necessario installare e configurare il server DNS.  
  

## <a name="install-and-configure-the-dns-server-service"></a>Installare e configurare il servizio Server DNS  
Completare questo passaggio per ogni controller di dominio ripristinato che non è in esecuzione come server DNS dopo il ripristino è stato completato.  
  
> [!NOTE]
>  Se il controller di dominio che è stato ripristinato da backup è in esecuzione Windows Server 2008 R2, è necessario connettersi al controller di dominio a una rete isolata per installare il server DNS. Connettere quindi ognuno dei server DNS ripristinato a una rete reciprocamente condivisa e isolata. Eseguire repadmin /replsum per verificare che la replica sta funzionando tra i server DNS ripristinati. Dopo avere verificato la replica, è possibile connettersi i controller di dominio ripristinato nella rete di produzione, se il ruolo server DNS è già installato, è possibile applicare un hotfix che rende possibile per un server DNS avviare mentre il server non è connessa ad alcuna rete. È necessario preinstallare gli aggiornamenti rapidi nell'immagine di installazione sistema operativo durante i processi di compilazione automatizzata. Per ulteriori informazioni sull'hotfix, vedere [articolo 975654](https://go.microsoft.com/fwlink/?LinkId=184691) della Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=184691). 

Completare la procedura di installazione e la configurazione seguente.
  
### <a name="to-install-and-the-dns-server-service-using-server-manager"></a>Per installare e il servizio Server DNS tramite Server Manager  
  
1.  Aprire Server Manager e fare clic su **Aggiungi ruoli e funzionalità**.  
2.  Nell'Aggiunta guidata ruoli, se il **prima di iniziare** verrà visualizzata la pagina, fare clic su **Avanti**.  
3.  Nel **tipo di installazione** schermata Seleziona **basata su ruoli o funzionalità in base a installazione** e fare clic su **Avanti**.
4.  Nel **selezione dei Server** dello schermo, selezionare il server e fare clic su **Avanti**.
5.  Nel **ruoli Server** schermata Seleziona **Server DNS**, se viene richiesto fare clic su **Aggiungi funzionalità** e fare clic su **Avanti**.
6.  Nel **funzionalità** fare clic su schermo **Avanti**.
7.  Leggere le informazioni sul **Server DNS** pagina e quindi fare clic su **Avanti**.
![Server DNS](media/AD-Forest-Recovery-Configure-DNS/dns1.png)  
8.  Nel **conferma** verificare che verrà installato il ruolo Server DNS e quindi fare clic su **installare**.  
  
     
### <a name="to-configure-the-dns-server-service"></a>Per configurare il servizio Server DNS 
1.  Aprire Server Manager fare clic su **strumenti** e fare clic su **DNS**.
![Server DNS](media/AD-Forest-Recovery-Configure-DNS/dns2.png)    
2.  Creare le zone DNS per gli stessi nomi di dominio DNS che sono stati ospitati nei server DNS prima il malfunzionamento critico. Per ulteriori informazioni, vedere Aggiunta di una zona di ricerca diretta ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).  
3.  Configurare i dati DNS esistente prima il malfunzionamento critico. Per esempio:  
  
    -   Configurare le zone DNS a essere archiviati in Active Directory. Per ulteriori informazioni, vedere Modifica il tipo di zona ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).  
  
    -   Configurare la zona DNS che è autorevole per record di risorse localizzatore controller di dominio di controller di dominio consentire l'aggiornamento dinamico sicuro. Per ulteriori informazioni, vedere consentire solo aggiornamenti dinamici protetti ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).  
  
4. Assicurarsi che la zona DNS padre contiene delega record di risorse (nome server (NS) e associa host (A) resource record) per la zona figlio che è ospitato su questo server DNS. Per ulteriori informazioni, vedere come creare una delega di Zone ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).  
5. Dopo la configurazione DNS, è possibile velocizzare la registrazione dei record di NETLOGON.  
  
    > [!NOTE]
    >  Gli aggiornamenti dinamici protetti funzionano solo quando è disponibile un server di catalogo globale.  
  
     Al prompt dei comandi, digitare il comando seguente e quindi premere INVIO:  
  
     **Net stop netlogon**  
  
6. Digitare il comando seguente e quindi premere INVIO:  
  
     **Net start netlogon**  

![Server DNS](media/AD-Forest-Recovery-Configure-DNS/dns3.png)  

## <a name="next-steps"></a>Passaggi successivi

- [Guida di ripristino della foresta Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
