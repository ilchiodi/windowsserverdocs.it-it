---
ms.assetid: 206b8072-1d0c-4a0b-ba8a-35a868d67b4c
title: Creazione di un progetto di collegamenti di sito
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4e0607cf66d41e1747b108a3ecc10562120d9174
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861932"
---
# <a name="creating-a-site-link-design"></a>Creazione di un progetto di collegamenti di sito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Creare una progettazione di collegamento di sito per connettere i siti con collegamenti di sito. I collegamenti di sito riflettono la connettività tra siti e il metodo utilizzato per trasferire il traffico di replica. È necessario connettere siti con collegamenti di sito in modo che i controller di dominio in ogni sito possono replicare le modifiche di Active Directory.  
  
## <a name="connecting-sites-with-site-links"></a>Connettere i siti con collegamenti di sito

Per connettere siti con collegamenti di sito, identificare i siti di membro che si desidera connettersi con il collegamento di sito, creare un oggetto collegamento di sito nel rispettivo contenitore trasporti tra siti e quindi assegnare un nome al collegamento di sito. Dopo aver creato il collegamento di sito, è possibile impostare proprietà dei collegamenti di sito.  
  
Quando si creano i collegamenti di sito, assicurarsi che ogni sito è incluso in un collegamento di sito. Inoltre, assicurarsi che tutti i siti sono connessi tra loro tramite altri collegamenti di sito in modo che le modifiche possano essere replicate dal controller di dominio in un sito qualsiasi per tutti gli altri siti. Se non si riesce a eseguire questa operazione, viene generato un messaggio di errore nel log del servizio Directory nel Visualizzatore eventi che informa che la topologia del sito non è connesso.  
  
Ogni volta che si aggiungono siti a un collegamento di sito appena creato, determinare se il sito viene aggiunto è un membro di altri collegamenti di sito e modificare l'appartenenza al collegamento di sito del sito se necessario. Ad esempio, se creiamo un sito di un membro di predefinito-primo-sito-Link quando si crea inizialmente il sito, assicurarsi di rimuovere il sito da predefinito-primo-sito-Link dopo aver aggiunto il sito a un nuovo collegamento di sito. Se non si rimuove il sito da predefinito-primo-sito-Link, il controllo di coerenza informazioni (KCC) sarà prendere decisioni di routing basate sull'appartenenza a entrambi i collegamenti di sito, che potrebbe comportare il routing non corretto.  
  
Per identificare i siti di membro che si desidera connettersi con un collegamento di sito, usare l'elenco di percorsi e i percorsi collegati che sono state registrate nel foglio di lavoro "Geografica posizioni e collegamenti di comunicazione" (DSSTOPO_1.doc). Se più siti hanno la stessa connettività e disponibilità tra loro, è possibile connettere tali elementi con lo stesso collegamento di sito.  
  
Il contenitore dei trasporti tra siti fornisce i mezzi per il mapping di collegamenti di sito per il trasporto che utilizza il collegamento. Quando si crea un oggetto collegamento di sito, si crea il contenitore IP, che associa il collegamento di sito con la chiamata di procedura remota (RPC) tramite il trasporto IP, o il contenitore Simple Mail Transfer Protocol (SMTP), che associa il collegamento di sito con il protocollo SMTP trasporto.  
  
> [!NOTE]  
> La replica SMTP non sarà supportata nelle versioni future di Active Directory Domain Services (AD DS); Pertanto, non è consigliabile creare gli oggetti di collegamenti di sito nel contenitore SMTP.  
  
Quando si crea un oggetto collegamento di sito nel rispettivo contenitore trasporti tra siti, per il trasferimento tra siti sia all'interno del sito di replica tra controller di dominio Active Directory Domain Services Usa RPC su IP. Per proteggere i dati in transito, replica RPC su IP utilizza sia il Kerberos authentication protocol e crittografia dei dati.  
  
Quando una connessione IP diretta non è disponibile, è possibile configurare la replica tra siti per utilizzare il protocollo SMTP. Tuttavia, funzionalità di replica è limitata e richiede un'autorità di certificazione (CA). SMTP possono replicare solo la configurazione, dello schema e directory applicative e non supporta la replica delle partizioni di directory dominio.  
  
Per denominare i collegamenti di sito, usare uno schema di denominazione coerente, ad esempio nome_del_sito1 nome_del_sito2. Registrare l'elenco di siti, siti collegati e i nomi dei collegamenti di sito la connessione di questi siti in un foglio di lavoro. Per un foglio di lavoro semplificare la registrazione di nomi dei siti e i nomi di collegamento di sito associati, vedere [processo di supporto per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, e aprire "Siti e collegamenti di sito associati" (DSSTOPO_5.doc).  
  
## <a name="in-this-guide"></a>Contenuto della guida

[Impostazione delle proprietà di collegamento di sito](Setting-Site-Link-Properties.md)  
