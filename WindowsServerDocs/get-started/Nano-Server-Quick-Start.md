---
title: Avvio rapido di Nano Server
description: Passaggi per distribuire rapidamente un'istanza di base di Nano Server in macchine virtuali o fisiche
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/05/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 7c1623e365be71cac2fd58da5444ce4358d75309
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "66443565"
---
# <a name="nano-server-quick-start"></a>Avvio rapido di Nano Server

>Si applica a: Windows Server 2016

> [!IMPORTANT]
> A partire da Windows Server versione 1709, Nano Server sarà disponibile solo come [immagine del sistema operativo di base del contenitore](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Per informazioni, vedi [Modifiche apportate a Nano Server](nano-in-semi-annual-channel.md). 

Seguire la procedura riportata in questa sezione per iniziare rapidamente con una distribuzione di base di Nano Server usando DHCP allo scopo di ottenere un indirizzo IP. È possibile eseguire un VHD Nano Server in una macchina virtuale oppure è possibile avviarlo in un computer fisico. Le procedure per le due opzioni sono leggermente diverse.

Dopo aver appreso le nozioni di base con questa procedura di avvio rapido, vedere l'argomento [Distribuire Nano Server](Deploy-Nano-Server.md) per dettagli sulla creazione di immagini personalizzate, la gestione dei pacchetti con metodi diversi, le operazioni di dominio e molto altro.
  
**Nano Server in una macchina virtuale**  
  
Per creare un VHD Nano Server che verrà eseguito in una macchina virtuale, seguire questa procedura.  
  
## <a name="to-quickly-deploy-nano-server-in-a-virtual-machine"></a>Per distribuire rapidamente Nano Server in una macchina virtuale  
  
1. Copiare la cartella *NanoServerImageGenerator* dalla cartella \NanoServer dell'immagine ISO di Windows Server 2016 in una cartella presente sul disco rigido.  
  
2. Avvia Windows PowerShell come amministratore, passa alla directory in cui hai inserito la cartella NanoServerImageGenerator e quindi importa il modulo con `Import-Module .\NanoServerImageGenerator -Verbose`  
   >[!NOTE]  
   >Può essere necessario modificare i criteri di esecuzione di Windows PowerShell. `Set-ExecutionPolicy RemoteSigned` dovrebbe funzionare correttamente.  
  
3. Creare un VHD per l'edizione Standard che imposti un nome di computer e includa i **driver guest** Hyper-V eseguendo il comando riportato di seguito. Tale comando richiederà una password amministratore per il nuovo VHD:  
  
   `New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath <path to root of media> -BasePath .\Base -TargetPath .\NanoServerVM\NanoServerVM.vhd -ComputerName <computer name>`, dove  
  
   -   **-MediaPath <path to root of media\>** specifica un percorso alla radice del contenuto dell'immagine ISO di Windows Server 2016. Se ad esempio i contenuti dell'immagine ISO sono stati copiati in d:\TP5ISO, è necessario usare tale percorso.  
  
   -   **-BasePath**: (facoltativo) specifica una cartella in cui verranno copiati il file WIM e i pacchetti di Nano Server.  
  
   -   **-TargetPath**: specifica un percorso, inclusi nome ed estensione del file, in cui verrà creato il VHD o il VHDX risultante.  
  
   -   **Computer_name**: specifica il nome di computer che verrà assegnato alla macchina virtuale Nano Server che si sta creando.  
  
   **Esempio:** `New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath f:\ -BasePath .\Base -TargetPath .\Nano1\Nano.vhd -ComputerName Nano1`  
  
   Questo esempio crea un VHD da un'immagine ISO montata come f:\\. Quando si crea il VHD, questo userà una cartella denominata Base nella stessa directory in cui è stato eseguito New-NanoServerImage. Il VHD (denominato Nano.vhd) verrà posizionato in una cartella denominata Nano1 all'interno della cartella da cui viene eseguito il comando. Il nome del computer sarà Nano1. Il VHD risultante conterrà l'edizione Standard di Windows Server 2016 e potrà essere usato per la distribuzione di macchine virtuali Hyper-V. Se si vuole ottenere una macchina virtuale di prima generazione, creare un'immagine del VHD specificando l'estensione **.vhd** per -TargetPath. Per una macchina virtuale di seconda generazione, creare un'immagine del VHDX specificando l'estensione **.vhdx** per -TargetPath. È anche possibile generare direttamente un file WIM specificando l'estensione **.wim** per -TargetPath.  
  
   > [!NOTE]  
   > New-NanoServerImage è supportato in Windows 8.1, Windows 10, Windows Server 2012 R2 e Windows Server 2016.  
  
4. Nella console di gestione di Hyper-V creare una nuova macchina virtuale e usare il VHD creato al Passaggio 3.  
  
5. La macchina virtuale e la console di gestione di Hyper-V si connettono entrambe alla macchina virtuale.  
  
6. Accedere alla Console di ripristino di emergenza (vedere la sezione "Nano Server Recovery Console" (Console di ripristino di emergenza di Nano Server) di questa Guida) usando la password amministratore specificata per l'esecuzione dello script riportato al Passaggio 3.  
   > [!NOTE]  
   > La Console di ripristino di emergenza supporta solo le funzioni di base della tastiera. I LED della tastiera, le sezioni a 10 tasti e la commutazione del layout di tastiera, ad esempio BLOC MAIUSC e BLOC NUM, non sono supportati.
  
7. Ottenere l'indirizzo IP della macchina virtuale Nano Server e usare la comunicazione remota di Windows PowerShell o un altro strumento di gestione remota per connettersi e gestire in remoto la macchina virtuale.  
  
**Nano Server in un computer fisico**  
  
È anche possibile creare un VHD che eseguirà Nano Server in un computer fisico usando i driver di dispositivo preinstallati. Se per l'avvio o la connessione a una rete l'hardware richiede un driver non ancora disponibile, seguire la procedura riportata nella sezione "Integrazione di driver aggiuntivi" di questa Guida.  
  
## <a name="to-quickly-deploy-nano-server-on-a-physical-computer"></a>Per distribuire rapidamente Nano Server in un computer fisico  
  
1.  Copiare la cartella *NanoServerImageGenerator* dalla cartella \NanoServer dell'immagine ISO di Windows Server 2016 in una cartella presente sul disco rigido.  
  
2.  Avvia Windows PowerShell come amministratore, passa alla directory in cui hai inserito la cartella NanoServerImageGenerator e quindi importa il modulo con `Import-Module .\NanoServerImageGenerator -Verbose`  
  
>[!NOTE]  
>Può essere necessario modificare i criteri di esecuzione di Windows PowerShell. `Set-ExecutionPolicy RemoteSigned` dovrebbe funzionare correttamente.  
  
3. Creare un VHD che imposti un nome di computer e includa i driver OEM e Hyper-V eseguendo il comando riportato di seguito. Tale comando richiederà una password amministratore per il nuovo VHD:  
  
   `New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath <path to root of media> -BasePath .\Base -TargetPath .\NanoServerPhysical\NanoServer.vhd -ComputerName <computer name> -OEMDrivers -Compute -Clustering`, dove  
  
   -   **-MediaPath <path to root of media\>** specifica un percorso alla radice del contenuto dell'immagine ISO di Windows Server 2016. Se ad esempio i contenuti dell'immagine ISO sono stati copiati in d:\TP5ISO, è necessario usare tale percorso.  
  
   -   **BasePath**: specifica una cartella in cui verranno copiati il file WIM e i pacchetti di Nano Server WIM. Questo parametro è facoltativo.  
  
   -   **TargetPath**: specifica un percorso, inclusi nome ed estensione del file, in cui verrà creato il VHD o il VHDX risultante.  
  
   -   **Computer_name**: nome del computer Nano Server che si sta creando.  
  
   **Esempio:** `New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath F:\ -BasePath .\Base -TargetPath .\Nano1\NanoServer.vhd -ComputerName Nano-srv1 -OEMDrivers -Compute -Clustering`  
  
   Questo esempio crea un VHD da un'immagine ISO montata come F:\\. Quando si crea il VHD, questo userà una cartella denominata Base nella stessa directory in cui è stato eseguito New-NanoServerImage. Il VHD verrà posizionato in una cartella denominata Nano1 all'interno della cartella da cui viene eseguito il comando. Il nome del computer sarà Nano-srv1. Tale computer disporrà dei driver OEM per i dispositivi hardware più comuni. Avrà inoltre il ruolo Hyper-V e la funzionalità di clustering abilitati. Viene usata l'edizione Standard di Nano Server.  
  
4. Accedere come amministratore al computer fisico in cui si vuole eseguire il VHD Nano Server.  
  
5. Copiare il VHD creato da questo script nel computer fisico e configurare quest'ultimo per l'avvio da questo nuovo VHD. A tale scopo, seguire questa procedura:  
  
   1.  Montare il VHD generato. In questo esempio viene montato in D:\\.  
  
   2.  Eseguire **bcdboot d:\windows**.  
  
   3.  Smontare il VHD.  
  
6. Avviare il computer fisico nel VHD Nano Server.  
  
7. Accedere alla Console di ripristino di emergenza (vedere la sezione "Nano Server Recovery Console" (Console di ripristino di emergenza di Nano Server) di questa Guida) usando la password amministratore specificata per l'esecuzione dello script riportato al Passaggio 3.
   > [!NOTE]  
   > La Console di ripristino di emergenza supporta solo le funzioni di base della tastiera. I LED della tastiera, le sezioni a 10 tasti e la commutazione del layout di tastiera, ad esempio BLOC MAIUSC e BLOC NUM, non sono supportati. 
  
8. Ottenere l'indirizzo IP del computer Nano Server e usare la comunicazione remota di Windows PowerShell o un altro strumento di gestione remota per connettersi e gestire in remoto la macchina virtuale.  
