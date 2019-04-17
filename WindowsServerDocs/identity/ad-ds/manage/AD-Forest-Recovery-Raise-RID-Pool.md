---
title: Ripristino della foresta Active Directory - i pool di RID generazione
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c37bc129-a5e0-4219-9ba7-b4cf3a9fc9a4
ms.technology: identity-adfs
ms.openlocfilehash: e6b5dc8b9c0b701fe2cd1b0c88f7edc22802c393
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---raising-the-value-of-available-rid-pools"></a>Ripristino della foresta Active Directory - il valore del pool di RID disponibile generazione 

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2
 
 Utilizzare la seguente procedura per aumentare il valore di ID relativi (RID) pool che si allocherà il master operazioni RID dopo il ripristino di tale controller di dominio. Per aumentare il valore dei pool di RID disponibile, è possibile garantire che nessun controller di dominio alloca un RID per un'entità di sicurezza creato dopo il backup che è stato utilizzato per ripristinare il dominio.  
 
## <a name="about-active-directory-rid-pools-and-ridavailablepool"></a>Su pool di RID di Active Directory e rIDAvailablePool
 Ogni dominio dispone di un oggetto **CN = RID Manager$, CN = System, DC**=<*nome_dominio*>. Questo oggetto dispone di un attributo denominato **rIDAvailablePool **. Valore di questo attributo mantiene lo spazio RID globale per un intero dominio. Il valore è un intero di grandi dimensioni con parti superiore e inferiore. La parte superiore definisce il numero di entità di sicurezza che può essere allocata per ogni dominio (0x3FFFFFFF o poco più di 1 miliardo). Nella parte inferiore è il numero di RID che sono stati assegnati nel dominio.  
  
> [!NOTE]
>  In Windows Server 2016 e 2012, il numero di entità di sicurezza che può essere allocata è aumentato a poco più 2 miliardi. Per ulteriori informazioni, vedere [emissioni di RID gestione ](https://technet.microsoft.com/library/jj574229.aspx).  
  
-   Il valore di esempio: 4611686014132422708  
  
-   La parte inferiore: 2100 (all'inizio del pool di RID successivo deve essere allocata)  
  
-   La parte superiore: 1073741823 (numero totale di RID che possono essere creati in un dominio)  
  
 Quando si aumenta il valore di numero intero grande, si aumenta il valore della parte bassa. Ad esempio, se si aggiungono 100.000 sul valore di esempio di 4611686014132422708 per un totale di 4611686014132522708, la nuova parte bassa è 102100. Ciò indica che il pool di RID successivo che sarà allocato dal master RID inizierà con 102100 anziché 2100.  
  
### <a name="to-raise-the-value-of-available-rid-pools-using-adsiedit-and-the-calculator--"></a>Per aumentare il valore del pool di RID disponibile utilizzando adsiedit e la calcolatrice '  
1.  Aprire Server Manager fare clic su **strumenti** e fare clic su **ADSI Edit **.    
2.  Pulsante destro del mouse, selezionare **connettersi** e connettersi eseguire il contesto dei nomi predefinito e fare clic su **OK **.
![Modifica ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi1.png) 
3. Selezionare il percorso di nome distinto seguenti: **CN = RID Manager$, CN = System, DC =<domain name>**.
![Modifica ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi2.png) 
3.  Pulsante destro del mouse e selezionare le proprietà di CN = RID Manager$.  
4.  Selezionare l'attributo **rIDAvailablePool**, fare clic su **modifica**e quindi copiare il valore di numero intero grande negli Appunti.
![Modifica ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi3.png)  
5.  Avvia la calcolatrice e dal **visualizzazione** dal menu **modalità scientifica **.  6.  Aggiungere 100.000 sul valore corrente.  
![Modifica ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi4.png) 
7.  Usando i tasti ctrl + c, o il **copia** comando il **modifica** menu, copia il valore negli Appunti.  
8.  Nella finestra di dialogo Modifica di adsiedit, incollare il nuovo valore. 
![Modifica ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi5.png) 
9. Fare clic su **OK** nella finestra di dialogo e **applica** nella finestra delle proprietà per aggiornare il **rIDAvailablePool** attributo.  
  
### <a name="to-raise-the-value-of-available-rid-pools-using-ldp"></a>Per aumentare il valore del pool di RID disponibile tramite LDP  
  
1.  Al prompt dei comandi, digitare il comando seguente e quindi premere INVIO:  
  
     **LDP**  
  
2.  Fare clic su **connessione**, fare clic su **Connetti**, digitare il nome del RID manager e quindi fare clic su **OK **.  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp1.png)
3.  Fare clic su **connessione**, fare clic su **binding**selezionare **binding con credenziali** e digitare le credenziali amministrative e quindi fare clic su **OK **.  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp2.png)
4.  Fare clic su **visualizzazione**, fare clic su **albero** e quindi digitare il percorso seguente nome distinto: CN = RID Manager$, CN = System, DC =*nome di dominio*  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp3.png)
5.  Fare clic su **Sfoglia**, quindi fare clic su **modifica **.  
6.  Aggiungere 100.000 all'oggetto corrente **rIDAvailablePool** valore e quindi digitare la somma in **valori **.  
7.  In **Dn**, tipo `cn=RID Manager$,cn=System,dc=`*< dominio name\ >*.  
8.  In **Modifica attributo voce**, tipo `rIDAvailablePool`.  
9. Selezionare **sostituire** come operazione e quindi fare clic su **invio**. </br>
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp4.png) 
10. Fare clic su **eseguire** per eseguire l'operazione.  Fare clic su **Chiudi**.
11. Per convalidare la modifica, fare clic su **visualizzazione**, fare clic su **albero**e quindi digitare il percorso seguente nome distinto: CN = RID Manager$, CN = System, DC =*nome di dominio*.    Controllare il **rIDAvailablePool** attributo.  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp5.png)

## <a name="next-steps"></a>Passaggi successivi

- [Guida di ripristino della foresta Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
 
