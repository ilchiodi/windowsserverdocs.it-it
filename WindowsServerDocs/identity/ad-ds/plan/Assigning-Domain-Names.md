---
ms.assetid: 73897497-b189-4305-b234-e057ffda163a
title: L'assegnazione di nomi di dominio
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0d5ec9b76798f21503f650527a7961cff0b592b4
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="assigning-domain-names"></a>L'assegnazione di nomi di dominio

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Nel piano di, è necessario assegnare un nome per ogni dominio. Domini di Active Directory Domain Services (AD DS) includono due tipi di nomi: nomi di sistema DNS (Domain Name) e NetBIOS. In generale, entrambi i nomi sono visibili agli utenti finali. I nomi DNS dei domini di Active Directory includono due parti, un prefisso e un suffisso. Durante la creazione di nomi di dominio, determinare innanzitutto il prefisso DNS. Questa è la prima etichetta del nome DNS del dominio. Il suffisso è determinato quando si seleziona il nome del dominio radice della foresta. La tabella seguente elenca il prefisso regole di denominazione per i nomi DNS.  
  
|Regola|Spiegazione|  
|--------|---------------|  
|Selezionare un prefisso che non è probabilmente diventare obsoleti.|Evitare nomi, ad esempio una linea di prodotti o un sistema operativo che potrebbero cambiare in futuro. Ti consigliamo di usare nomi geografici.|  
|Selezionare un prefisso che include solo caratteri standard Internet.|A-Z, a-z, 0-9 e (-), ma non completamente numerico.|  
|Includere i 15 caratteri o meno del prefisso.|Se si sceglie una lunghezza del prefisso di 15 caratteri o meno, il nome NetBIOS è quello utilizzato per il prefisso.|  
  
Per ulteriori informazioni, vedere le convenzioni di denominazione in Active Directory per computer, domini, siti e unità organizzative ([https://go.microsoft.com/fwlink/?LinkId=106629](https://go.microsoft.com/fwlink/?LinkId=106629)).  
  
> [!NOTE]  
>  Anche se Dcpromo.exe in Windows Server 2008 e Windows Server 2003 consente di creare un nome dominio DNS con etichetta singola, non utilizzare un nome DNS con etichetta singola per un dominio per diversi motivi. In Windows Server 2008 R2, Dcpromo.exe non consente di creare un nome DNS con etichetta singola per un dominio. Per ulteriori informazioni, vedere [https://go.microsoft.com/fwlink/?LinkId=92467.](https://go.microsoft.com/fwlink/?LinkId=92467)   
  
Se il nome NetBIOS corrente del dominio non è appropriato rappresentare l'area geografica o ha esito negativo soddisfare il prefisso regole di denominazione, selezionare un nuovo prefisso. In questo caso, il nome NetBIOS del dominio è diverso dal prefisso del DNS del dominio.  
  
Per ogni nuovo dominio da distribuire, selezionare un prefisso che è appropriato per l'area geografica e che soddisfa le regole di denominazione di prefisso. È consigliabile che il nome NetBIOS del dominio sia lo stesso prefisso del DNS.  
  
Documentare il prefisso DNS e i nomi NetBIOS selezionato per ogni dominio della foresta. È possibile aggiungere le informazioni di nome DNS e NetBIOS al foglio di lavoro "Pianificazione del dominio" creato per documentare il piano per i domini nuovi e aggiornati. Per aprirlo, scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip dal processo Aid per Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) e Apri "pianificazione del dominio (DSSLOGI_5.doc).  
  


