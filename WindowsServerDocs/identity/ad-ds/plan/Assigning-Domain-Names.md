---
ms.assetid: 73897497-b189-4305-b234-e057ffda163a
title: Assegnazione di nomi di dominio
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ba5a7ee8b7d9728c48e2798853aab8d55047e86f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866562"
---
# <a name="assigning-domain-names"></a>Assegnazione di nomi di dominio

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È necessario assegnare un nome per tutti i domini nel piano. Domini di Active Directory Domain Services (AD DS) includono due tipi di nomi di: I nomi di Domain Name System (DNS) e i nomi NetBIOS. In generale, entrambi i nomi sono visibili agli utenti finali. I nomi DNS di domini di Active Directory includono due parti, un prefisso e suffisso. Durante la creazione di nomi di dominio, determinare innanzitutto il prefisso DNS. Questa è la prima etichetta il nome DNS del dominio. Il suffisso è determinato quando si seleziona il nome del dominio radice della foresta. La tabella seguente elenca il prefisso di regole di denominazione per i nomi DNS.  
  
|Regola|Spiegazione|  
|--------|---------------|  
|Selezionare un prefisso che non è destinato a diventare obsoleto.|Evitare nomi, ad esempio una linea di prodotti o sistema operativo che potrebbe cambiare in futuro. È consigliabile usare i nomi geografici.|  
|Selezionare un prefisso che include solo caratteri standard Internet.|Alla Z, a-z, 0-9 e (-), ma non è totalmente numerica.|  
|Includere i 15 caratteri o meno nel prefisso.|Se si sceglie una lunghezza del prefisso di 15 caratteri o meno, il nome NetBIOS è lo stesso come prefisso.|  
  
Per altre informazioni, vedere le convenzioni di denominazione in Active Directory per computer, domini, siti e unità organizzative ([https://go.microsoft.com/fwlink/?LinkId=106629](https://go.microsoft.com/fwlink/?LinkId=106629)).  
  
> [!NOTE]  
>  Anche se Dcpromo.exe in Windows Server 2008 e Windows Server 2003 consentono di creare un nome di dominio DNS con etichetta singola, questa scelta non è consigliata per diversi motivi. In Windows Server 2008 R2, Dcpromo.exe non consente di creare un nome di dominio DNS con etichetta singola. Per altre informazioni, vedere [ https://go.microsoft.com/fwlink/?LinkId=92467.](https://go.microsoft.com/fwlink/?LinkId=92467)   
  
Se il nome NetBIOS corrente del dominio non è appropriato rappresentare l'area o non riesce a soddisfare le regole di denominazione dei prefissi, selezionare un nuovo prefisso. In questo caso, il nome NetBIOS del dominio è diverso dal prefisso DNS del dominio.  
  
Per ogni nuovo dominio da distribuire, selezionare un prefisso che è appropriato per l'area e che soddisfa le regole di denominazione dei prefissi. È consigliabile che il nome NetBIOS del dominio coincidere con il prefisso DNS.  
  
Documentare il prefisso DNS e i nomi NetBIOS selezionato per ogni dominio nella foresta. È possibile aggiungere le informazioni sul nome NetBIOS e DNS per il foglio di lavoro "Pianificazione di dominio" creato per documentare il piano per i domini di nuovi e aggiornati. Per aprirlo, scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip dal processo di supporto per Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) e aprire "Pianificazione Domain" (DSSLOGI_5.doc).  
  


