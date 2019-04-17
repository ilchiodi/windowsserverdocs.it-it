---
ms.assetid: 206b8072-1d0c-4a0b-ba8a-35a868d67b4c
title: Creazione di una progettazione di collegamento di sito
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9eb54781035424c9a5210e11fbdeafc55496c6c3
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-site-link-design"></a>Creazione di una progettazione di collegamento di sito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Creare una progettazione di collegamento di sito per connettere i siti con collegamenti di sito. I collegamenti di sito riflettono la connettività tra siti e il metodo utilizzato per trasferire il traffico di replica. È necessario connettere siti con collegamenti di sito in modo che i controller di dominio in ogni sito possono replicare le modifiche di Active Directory.  
  
## <a name="connecting-sites-with-site-links"></a>Connessione di siti con collegamenti di sito  
Per connettere siti con collegamenti di sito, identificare i siti di membro che si desidera connettersi con il collegamento di sito, creare un oggetto collegamento di sito nel rispettivo contenitore trasporti tra siti e quindi assegnare un nome collegamento di sito. Dopo aver creato il collegamento di sito, è possibile procedere per impostare le proprietà di collegamento di sito.  
  
Durante la creazione di collegamenti di sito, assicurarsi che ogni sito è incluso in un collegamento di sito. Inoltre, assicurarsi che tutti i siti connessi tra loro tramite altri collegamenti di sito in modo che le modifiche possono essere replicate dal controller di dominio in qualsiasi sito in tutti gli altri siti. Se non è possibile eseguire questa operazione, viene generato un messaggio di errore nel registro del servizio di Directory in Visualizzatore eventi che informa che la topologia del sito non è connesso.  
  
Quando si aggiungono siti a un nuovo collegamento di sito, determinare se il sito viene aggiunto è un membro di altri collegamenti di sito e modificare l'appartenenza al collegamento di sito del sito se necessario. Ad esempio, se si apportano di un sito è un membro del Default-First-Site-Link a quando si crea il sito, assicurarsi di rimuovere il sito dal Default-First-Site-Link dopo aver aggiunto il sito a un nuovo collegamento di sito. Se si rimuove il sito dal Default-First-Site-Link, il controllo di coerenza informazioni (KCC) verrà prendere decisioni di routing in base l'appartenenza di entrambi i collegamenti di sito, che può portare il routing non corretto.  
  
Per identificare i siti di membro che si desidera connettersi con un collegamento di sito, utilizzare l'elenco di percorsi e percorsi collegati che sono state registrate nel foglio di lavoro (DSSTOPO_1.doc) "Geografica percorsi e collegamenti di comunicazione". Se più siti dispongono della stessa connettività e disponibilità tra di loro, è possibile connettersi con lo stesso collegamento di sito.  
  
Il contenitore trasporti tra siti fornisce i mezzi per il mapping di collegamenti di sito per il trasporto che utilizza il collegamento. Quando si crea un oggetto collegamento di sito, creare il contenitore IP, che associa il collegamento di sito con la chiamata di procedura remota (RPC) su trasporto IP, o il contenitore Simple Mail Transfer Protocol (SMTP), che associa il collegamento di sito basato sul trasporto SMTP.  
  
> [!NOTE]  
> La replica SMTP non sarà supportata nelle versioni future di servizi di dominio Active Directory (AD DS); di conseguenza, la creazione di oggetti di collegamenti di sito nel contenitore SMTP non è consigliata.  
  
Quando si crea un oggetto collegamento di sito nel rispettivo contenitore trasporti tra siti, servizi di dominio Active Directory utilizza RPC su IP per il trasferimento tra siti sia all'interno del sito di replica tra controller di dominio. Per proteggere dati in transito, replica RPC su IP utilizza sia le Kerberos authentication protocol e crittografia dei dati.  
  
Quando una connessione IP diretta non è disponibile, è possibile configurare la replica tra siti di utilizzare SMTP. Tuttavia, la funzionalità di replica SMTP è limitata e richiede un'autorità di certificazione (CA). SMTP può replicare solo la configurazione, dello schema e partizioni di directory applicative e non supporta la replica delle partizioni di directory dominio.  
  
Assegnare un nome di collegamenti di sito, utilizzare uno schema di denominazione coerente, ad esempio nome_del_sito1 nome_del_sito2. Registrare l'elenco dei siti, siti e i nomi dei collegamenti di sito che connette questi siti in un foglio di lavoro. Per un foglio di lavoro facilitare la registrazione di nomi di sito e collegamento di sito associato, vedere processo Aid per Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, e Aprire (DSSTOPO_5.doc) "Siti e collegamenti di sito associati".  
  
## <a name="in-this-guide"></a>In questa Guida  
[Impostazione delle proprietà di collegamento di sito](Setting-Site-Link-Properties.md)  
  


