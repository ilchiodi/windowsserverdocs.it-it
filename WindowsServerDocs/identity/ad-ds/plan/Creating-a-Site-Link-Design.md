---
ms.assetid: 206b8072-1d0c-4a0b-ba8a-35a868d67b4c
title: Creazione di una progettazione di collegamenti di sito
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ae68cf05f9631df0f942cb65ccf29971f7bc17c8
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624329"
---
# <a name="creating-a-site-link-design"></a>Creazione di una progettazione di collegamenti di sito

> Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Creare una struttura di collegamento di sito per connettere i siti con i collegamenti di sito. I collegamenti di sito riflettono la connettività e il metodo tra siti usati per trasferire il traffico di replica. È necessario connettere i siti con collegamenti di sito in modo che i controller di dominio in ogni sito possano replicare Active Directory modifiche.

## <a name="connecting-sites-with-site-links"></a>Connettere i siti con collegamenti di sito

Per connettere siti con collegamenti di sito, identificare i siti membri che si desidera connettere al collegamento di sito, creare un oggetto collegamento di sito nel rispettivo contenitore di trasporti tra siti, quindi denominare il collegamento di sito. Dopo aver creato il collegamento di sito, è possibile procedere con l'impostazione delle proprietà del collegamento di sito.

Quando si creano i collegamenti di sito, verificare che ogni sito sia incluso in un collegamento di sito. Assicurarsi inoltre che tutti i siti siano connessi tra loro tramite altri collegamenti di sito, in modo che le modifiche possano essere replicate dai controller di dominio in qualsiasi sito a tutti gli altri siti. In caso contrario, viene generato un messaggio di errore nel log del servizio directory Visualizzatore eventi indicante che la topologia del sito non è connessa.

Ogni volta che si aggiungono siti a un collegamento di sito appena creato, determinare se il sito da aggiungere è un membro di altri collegamenti di sito e modificare l'appartenenza al collegamento di sito del sito, se necessario. Se, ad esempio, si imposta un sito come membro del collegamento predefinito-primo-sito quando si crea inizialmente il sito, assicurarsi di rimuovere il sito dal collegamento predefinito-primo-sito dopo aver aggiunto il sito a un nuovo collegamento di sito. Se il sito non viene rimosso dal collegamento predefinito-primo-sito, il controllo di coerenza informazioni (KCC) farà prendere decisioni di routing in base all'appartenenza di entrambi i collegamenti di sito, il che potrebbe comportare un routing errato.

Per identificare i siti membri a cui si desidera connettersi con un collegamento di sito, utilizzare l'elenco delle località e delle posizioni collegate registrate nel foglio di foglio "posizioni geografiche e collegamenti di comunicazione" (DSSTOPO_1. doc). Se più siti hanno la stessa connettività e disponibilità tra loro, è possibile connetterli con lo stesso collegamento di sito.

Il contenitore trasporti tra siti fornisce i mezzi per eseguire il mapping dei collegamenti di sito al trasporto utilizzato dal collegamento. Quando si crea un oggetto collegamento di sito, lo si crea nel contenitore IP, che associa il collegamento di sito a RPC (Remote Procedure Call) su trasporto IP o il contenitore Simple Mail Transfer Protocol (SMTP), che associa il collegamento di sito al trasporto SMTP.

> [!NOTE]
> La replica SMTP non sarà supportata nelle versioni future di Active Directory Domain Services (AD DS); Pertanto, non è consigliabile creare oggetti collegamenti di sito nel contenitore SMTP.

Quando si crea un oggetto collegamento di sito nel rispettivo contenitore trasporto tra siti, servizi di dominio Active Directory utilizza RPC su IP per trasferire la replica sia tra siti che tra siti tra i controller di dominio. Per garantire la sicurezza dei dati durante il transito, la replica RPC su IP utilizza sia il protocollo di autenticazione Kerberos che la crittografia dei dati.

Quando non è disponibile una connessione IP diretta, è possibile configurare la replica tra siti per l'utilizzo di SMTP. Tuttavia, la funzionalità di replica SMTP è limitata e richiede un'autorità di certificazione (CA) dell'organizzazione (Enterprise). SMTP può replicare solo le partizioni di directory di configurazione, dello schema e delle applicazioni e non supporta la replica delle partizioni di directory del dominio.

Per assegnare un nome ai collegamenti di sito, utilizzare uno schema di denominazione coerente, ad esempio name_of_site1-name_of_site2. Registrare l'elenco di siti, i siti collegati e i nomi dei collegamenti di sito che connettono questi siti in un foglio di un foglio di un foglio di. Per un foglio di lavoro che consente di registrare i nomi dei siti e i nomi dei collegamenti di sito associati, vedere l'argomento relativo ai processi di supporto [per Windows Server 2003 Deployment Kit](https://microsoft.com/download/details.aspx?id=9608), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip e aprire "siti e collegamenti di sito associati" (DSSTOPO_5. doc).

## <a name="in-this-guide"></a>Contenuto della guida

[Impostazione delle proprietà di collegamento di sito](Setting-Site-Link-Properties.md)
