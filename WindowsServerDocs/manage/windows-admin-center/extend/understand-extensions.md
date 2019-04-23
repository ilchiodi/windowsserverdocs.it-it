---
title: Informazioni sulle estensioni di Windows Admin Center
description: Informazioni sulle estensioni di Windows Admin Center (Project Honolulu)
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: b00ee847088d038e59266154bcbbe9499bfe47fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850112"
---
# <a name="understanding-windows-admin-center-extensions"></a>Informazioni sulle estensioni di Windows Admin Center

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

Nel caso in cui non si abbia ancora familiarità con il funzionamento di Windows Admin Center, iniziamo con l'architettura di alto livello. Windows Admin Center è composto da due componenti principali:

- Il **servizio Web** Lightweight che serve le pagine Web dell'interfaccia utente di Windows Admin Center alle richieste del browser Web.
- Il **componente di gateway** che ascolta le richieste API REST dalle pagine Web e inoltra le chiamate WMI o gli script PowerShell da eseguire su un server o cluster di destinazione.

![Architettura di Windows Admin Center](../media/understand-extensions/wac-architecture-500px.png)

Le pagine Web dell'interfaccia utente di Windows Admin Center offerte dal servizio Web includono due componenti principali dell'interfaccia utente da una prospettiva di estensibilità, soluzioni e strumenti, implementati come estensioni e un terzo tipo di estensione denominato plug-in gateway.

## <a name="solution-extensions"></a>Estensioni della soluzione

Nella schermata principale di Windows Admin Center, per impostazione predefinita, puoi aggiungere connessioni di quattro tipi: connessioni a Windows Server, connessioni a PC Windows, connessioni cluster di failover e connessioni cluster iper-convergenti. Una volta aggiunta una connessione, il nome e il tipo di connessione verranno visualizzati nella schermata principale. Facendo clic sul nome della connessione verrà eseguito il tentativo di connettersi al server o al cluster di destinazione e quindi caricare l'interfaccia utente per la connessione.

![Architettura di Windows Admin Center](../media/understand-extensions/solutions-ui.png)

Ciascuno di questi tipi di connessione viene mappato su una soluzione e le soluzioni vengono definite tramite un tipo di estensione chiamata estensione della "soluzione". Le soluzioni in genere definiscono un tipo univoco di oggetto che desideri gestire tramite Windows Admin Center, come server, PC o cluster di failover. Puoi anche definire una nuova soluzione per la connessione e la gestione di altri dispositivi come switch di rete e server Linux o anche servizi come Servizi Desktop remoto.

## <a name="tool-extensions"></a>Estensioni dello strumento

Quando fai clic su una connessione nella schermata iniziale di Windows Admin Center e ti connetti, viene caricata l'estensione della soluzione per il tipo di connessione selezionato e viene quindi visualizzata l'interfaccia utente della soluzione che include un elenco di strumenti nel riquadro di spostamento a sinistra. Quando fai clic su uno strumento, l'interfaccia utente dello strumento viene caricata e visualizzata nel riquadro di destra.

![Architettura dell'interfaccia utente di Windows Admin Center](../media/understand-extensions/ui-architecture.png)

Ognuno di questi strumenti è definito attraverso un secondo tipo di estensione chiamata estensione dello "strumento". Quando uno strumento viene caricato, può eseguire chiamate WMI o script PowerShell su un server o cluster di destinazione e visualizzare informazioni nell'interfaccia utente o eseguire comandi in base all'input dell'utente. Un'estensione dello strumento definisce le soluzioni per cui deve essere visualizzata, generando un diverso set di strumenti per ciascuna soluzione. Se stai creando una nuova estensione per la soluzione, è necessario anche scrivere una o più estensioni dello strumento che forniscono funzionalità per la soluzione.

![Elenco degli strumenti per ogni soluzione](../media/understand-extensions/tools-for-solutions.png)

## <a name="gateway-plugins"></a>Plug-in di gateway

Il servizio gateway espone le API REST per l'interfaccia utente per chiamare e inoltrare comandi e script da eseguire sulla destinazione. Il servizio gateway può essere esteso da plug-in gateway che supportano diversi protocolli. Windows Admin Center è preconfigurato con due plug-in di gateway, uno per l'esecuzione degli script PowerShell e l'altro per i comandi WMI. Se è necessario comunicare con la destinazione tramite un protocollo diverso da PowerShell o WMI, come REST, puoi creare un plug-in gateway.

## <a name="next-steps"></a>Passaggi successivi

A seconda delle funzionalità che vuoi creare in Windows Admin Center, la [creazione di un'estensione dello strumento](develop-tool.md) per una soluzione cluster o server esistente può essere sufficiente e costituire il primo passaggio più semplice per la creazione delle estensioni. Tuttavia, se la tua funzione è per la gestione di un dispositivo, un servizio o qualcosa di completamente nuovo, piuttosto che un server o un cluster, è necessario prendere in considerazione la [creazione di un'estensione della soluzione](develop-solution.md) con uno o più strumenti. Infine, se è necessario comunicare con la destinazione tramite un protocollo diverso da WMI o PowerShell, dovrai [creare un plug-in del gateway](develop-gateway-plugin.md). [Continua la lettura in](developing-extensions.md) per informazioni su come impostare l'ambiente di sviluppo e avviare la scrittura della prima estensione.
