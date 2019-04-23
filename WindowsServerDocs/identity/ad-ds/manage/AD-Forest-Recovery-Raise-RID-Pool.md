---
title: Ripristino della foresta Active Directory - i pool di RID generazione
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c37bc129-a5e0-4219-9ba7-b4cf3a9fc9a4
ms.technology: identity-adds
ms.openlocfilehash: c8f91226e10ea6681933d5a5dc00b92f5ab2179c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862982"
---
# <a name="ad-forest-recovery---raising-the-value-of-available-rid-pools"></a>Ripristino della foresta Active Directory - genera il valore disponibile di pool di RID 

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Utilizzare la procedura seguente per generare il valore di ID relativi (RID) crea un pool che il master operazioni RID allocherà dopo il ripristino di questo controller di dominio. Per generare il valore dei pool di RID disponibile, è possibile garantire che nessun controller di dominio alloca un RID per un'entità di sicurezza che è stata creata dopo il backup utilizzato per ripristinare il dominio. 

## <a name="about-active-directory-rid-pools-and-ridavailablepool"></a>Sui pool di RID di Active Directory e rIDAvailablePool

Ogni dominio dispone di un oggetto **CN = RID Manager$, CN = System, DC**=<*nome_dominio*>. Questo oggetto ha un attributo denominato **rIDAvailablePool**. Il valore dell'attributo mantiene lo spazio RID globale per un intero dominio. Il valore è un numero intero grande con parti superiore e inferiore. Nella parte superiore definisce il numero di entità di sicurezza che può essere allocata per ogni dominio (0x3FFFFFFF o appena oltre 1 miliardo). Nella parte inferiore è il numero di RID che sono stati allocati nel dominio. 
  
> [!NOTE]
> In Windows Server 2016 e 2012, il numero di entità di sicurezza che possono essere allocati è aumentato a 2 miliardi di poco più. Per altre informazioni, vedere [gestione dei RID issuance](https://technet.microsoft.com/library/jj574229.aspx). 
  
- Valore di esempio: 4611686014132422708  
- Parte bassa: 2100 (all'inizio del pool di RID successivo deve essere allocata)  
- Parte superiore: 1073741823 (numero totale di RID che possono essere creati in un dominio)  
  
Quando si aumenta il valore dell'intero grande, si aumenta il valore della parte bassa. Ad esempio, se si aggiungono 100.000 per il valore del campione di 4611686014132422708 per una somma di 4611686014132522708, la nuova parte bassa è 102100. Ciò indica che il pool di RID successivo che verrà allocato dal master RID inizierà con 102100 anziché 2100. 
  
### <a name="to-raise-the-value-of-available-rid-pools-using-adsiedit-and-the-calculator"></a>Per aumentare il valore disponibile di pool di RID utilizzando adsiedit e calcolatrice

1. Aprire Server Manager, fare clic su **degli strumenti** e fare clic su **ADSI Edit**.
2. Pulsante destro del mouse, selezionare **connettersi a** e la connessione è il contesto dei nomi predefinito e fare clic su **OK**.
   ![Modifica ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi1.png) 
3. Passare al percorso di nome distinto seguenti: **CN = RID Manager$, CN = System, DC =<domain name>**.
   ![Modifica ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi2.png) 
3. Pulsante destro del mouse e selezionare le proprietà di CN = RID Manager$. 
4. Selezionare l'attributo **rIDAvailablePool**, fare clic su **modificare**e quindi copiare il valore intero di grandi dimensioni negli Appunti.
   ![Modifica ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi3.png)  
5. Avvia la calcolatrice e dal **View** dal menu **modalità scientifica**. 
6. Aggiungere 100.000 al valore corrente.
   ![Modifica ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi4.png) 
7. Utilizzando ctrl + c, o la **copia** dalle **modificare** menu, copiare il valore negli Appunti. 
8. Nella finestra di dialogo di modifica di adsiedit, incollare questo nuovo valore. 
   ![Modifica ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi5.png) 
9. Fare clic su **OK** nella finestra di dialogo, e **Apply** nella finestra delle proprietà per aggiornare il **rIDAvailablePool** attributo. 
  
### <a name="to-raise-the-value-of-available-rid-pools-using-ldp"></a>Per aumentare il valore di pool RID disponibili tramite LDP  
  
1. Al prompt dei comandi digitare il comando seguente e quindi premere INVIO:  
   **ldp**  
2. Fare clic su **Connection**, fare clic su **Connect**, digitare il nome della gestione di RID e quindi fare clic su **OK**. 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp1.png)
3. Fare clic su **Connection**, fare clic su **associare**, selezionare **binding con credenziali** e digitare le credenziali amministrative e quindi fare clic su **OK**. 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp2.png)
4. Fare clic su **View**, fare clic su **albero** e quindi digitare il seguente percorso di nome distinto:  CN = RID Manager$, CN = System, DC =*nome di dominio*  
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp3.png)
5. Fare clic su **esplorare**, quindi fare clic su **Modify**. 
6. Aggiungere 100.000 all'oggetto corrente **rIDAvailablePool** valore e quindi digitare la somma in **valori**. 
7. Nelle **Dn**, digitare `cn=RID Manager$,cn=System,dc=` *< nome di dominio\>*. 
8. Nelle **Modifica attributo voce**, tipo `rIDAvailablePool`. 
9. Selezionare **sostituire** come operazione e quindi fare clic su **invio**.
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp4.png) 
10. Fare clic su **eseguiti** per eseguire l'operazione. Fare clic su **Chiudi**.
11. Per convalidare la modifica, fare clic su **View**, fare clic su **albero**e quindi digitare il seguente percorso di nome distinto:   CN = RID Manager$, CN = System, DC =*nome di dominio*.   Verificare i **rIDAvailablePool** attributo. 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp5.png)

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta AD](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
