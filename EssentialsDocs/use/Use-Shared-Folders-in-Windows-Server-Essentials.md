---
title: Utilizzare le cartelle condivise in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb7f3d7d-4225-409a-9f6b-34a106e8dd24
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: eda19a5117a70fbaff04ec3fb3b89aa6e212f962
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="use-shared-folders-in-windows-server-essentials"></a>Utilizzare le cartelle condivise in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Windows Server Essentials fornisce una posizione centrale per tutti i dati e i file tramite le cartelle condivise che si trovano nel server.  
  
 Esistono molti modi diversi, è possibile accedere alle cartelle condivise in Windows Server Essentials da un dispositivo è connesso al server:  
  

-   [Utilizzando la finestra di avvio di Windows Server Essentials](Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_UsingLaunchpad)  
  
-   [Tramite accesso Web remoto](Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_UsingRWA)  
  
-   [Uso dell'app My Server per Windows Phone](Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_Phone)  
  
-   [Uso dell'app My Server per Windows 8](Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_App)  
  
##  <a name="BKMK_UsingLaunchpad"></a>Utilizzando la finestra di avvio di Windows Server Essentials  
 È possibile utilizzare la finestra di avvio da qualsiasi computer connesso al server tramite la connessione del Computer per la creazione guidata Server. Per ulteriori informazioni sulla connessione del computer al server, vedere [connettere i computer al server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

-   [Utilizzando la finestra di avvio di Windows Server Essentials](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_UsingLaunchpad)  
  
-   [Tramite accesso Web remoto](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_UsingRWA)  
  
-   [Uso dell'app My Server per Windows Phone](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_Phone)  
  
-   [Uso dell'app My Server per Windows 8](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md#BKMK_App)  
  
##  <a name="BKMK_UsingLaunchpad"></a>Utilizzando la finestra di avvio di Windows Server Essentials  
 È possibile utilizzare la finestra di avvio da qualsiasi computer connesso al server tramite la connessione del Computer per la creazione guidata Server. Per ulteriori informazioni sulla connessione del computer al server, vedere [connettere i computer al server](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

  
 Dopo aver connesso il computer al server, un collegamento alla finestra di avvio viene aggiunto all'area di notifica del desktop. Fare doppio clic sull'icona di finestra di avvio e immettere le credenziali di rete per accedere alle cartelle condivise mediante la finestra di avvio. Nella finestra di avvio, utilizzando il collegamento cartelle condivise, puoi caricare o scaricare file in qualsiasi delle cartelle condivise che sono elencate, trascinando i file tra il computer locale e le cartelle condivise. Le cartelle condivise consentono di trasmettere musica e video, riprodurre presentazioni o registrare programmi TV in qualsiasi computer connesso al server, oppure di riprodurre una presentazione per visualizzare le immagini.  
  
 Per ulteriori informazioni sulla finestra di avvio, vedere [Panoramica della finestra di avvio](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_Launchpad"></a>Copiare o spostare file condivisi o cartelle con la finestra di avvio  
 Quando si desidera copiare o spostare file condivisi in Windows Server Essentials tramite la finestra di avvio, fare clic su di **cartelle condivise** scheda nella finestra di avvio.  
  
 Se si desidera spostare un file o una cartella da una posizione a un'altra in **cartelle condivise**, è possibile utilizzare il metodo di trascinamento e in modo analogo a come si usa per spostare file e cartelle nel computer. Aprire la cartella che contiene il file o la cartella che si desidera spostare. Aprire la cartella in cui vuoi spostarla in un'altra finestra. Posizionare il windows side-by-side sul desktop in modo che è possibile visualizzare il contenuto di entrambi e quindi trascina il file o la cartella dalla prima alla seconda cartella.  
  
> [!NOTE]
>  Quando si usa il metodo di trascinamento della selezione, si potrebbe notare che a volte il file o cartella è **copiati**e in altri casi è **spostato**. Se si trascina un elemento tra due cartelle archiviate nello stesso disco rigido, l'elemento viene spostato in modo che non vengono create due copie dello stesso file o della cartella nello stesso percorso. Se si trascina un elemento in una cartella che si trova in un percorso diverso (ad esempio un altro computer) o su supporti rimovibili, ad esempio un'unità flash USB, l'elemento viene copiato.  
  
 Se si desidera copiare file o cartelle da una posizione a un'altra in **cartelle condivise**, è possibile utilizzare la copia e Incolla nello stesso modo come copiare file nel computer. Aprire la cartella che contiene i file che si desidera copiare. Fare doppio clic sui file che si desidera copiare e quindi fare clic su **copia**. Fare clic sulla cartella in cui si desidera incollare i file copiati e quindi fare clic su **incollare**.  
  
##  <a name="BKMK_UsingRWA"></a>Tramite accesso Web remoto  

 È possibile accedere cartelle e file condivisi da qualsiasi computer remoto utilizzando il sito Web accesso Web remoto. Da un computer all'interno della rete del server, per accedere al sito accesso Web remoto, aprire il browser Internet e digitare https://<servername\>/remote. Tramite accesso Web remoto, è possibile visualizzare e gestire i file nelle cartelle condivise. Per istruzioni dettagliate, vedere [usare accesso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  

 È possibile accedere cartelle e file condivisi da qualsiasi computer remoto utilizzando il sito Web accesso Web remoto. Da un computer all'interno della rete del server, per accedere al sito accesso Web remoto, aprire il browser Internet e digitare https://<servername\>/remote. Tramite accesso Web remoto, è possibile visualizzare e gestire i file nelle cartelle condivise. Per istruzioni dettagliate, vedere [usare accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  

  
> [!NOTE]
>  Accesso Web remoto nel server deve essere attivata per accedere al sito Web accesso Web remoto. Per informazioni sulla gestione di accesso Web remoto, vedere [gestire accesso Web remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_2"></a>Creare, rinominare, spostare, eliminare o copiare file e cartelle in accesso Web remoto  

 È possibile usare accesso Web remoto per creare nuove cartelle in una cartella condivisa esistente, per rinominare file e cartelle, per spostare o copiare file e cartelle e per eliminare file e cartelle nel server. Per ulteriori informazioni, vedere la sezione creare, rinominare, spostare, eliminare o copiare file e cartelle in accesso Web remoto? Nell'argomento [usare accesso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_3"></a>Caricare e scaricare file in accesso Web remoto  
 In accesso Web remoto **cartelle condivise** scheda, è possibile caricare e scaricare i file. Per ulteriori informazioni, vedere la sezione caricare e scaricare file in accesso Web remoto? Nell'argomento [usare accesso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  

 È possibile usare accesso Web remoto per creare nuove cartelle in una cartella condivisa esistente, per rinominare file e cartelle, per spostare o copiare file e cartelle e per eliminare file e cartelle nel server. Per ulteriori informazioni, vedere la sezione creare, rinominare, spostare, eliminare o copiare file e cartelle in accesso Web remoto? Nell'argomento [usare accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_3"></a>Caricare e scaricare file in accesso Web remoto  
 In accesso Web remoto **cartelle condivise** scheda, è possibile caricare e scaricare i file. Per ulteriori informazioni, vedere la sezione caricare e scaricare file in accesso Web remoto? Nell'argomento [usare accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  

  
##  <a name="BKMK_Phone"></a>Uso dell'app My Server per Windows Phone  
 È possibile accedere alle cartelle condivise tramite il Windows Phone tramite l'app My Server per Windows Phone. È possibile scaricare questa app dal [Marketplace per Windows Phone](http://www.windowsphone.com/apps/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a).  
  
##  <a name="BKMK_App"></a>Uso dell'app My Server per Windows 8  
 È possibile accedere alle cartelle condivise tramite Windows 8 utilizzando l'app My Server per Windows 8. È possibile scaricare questa app dal [store di App per Windows 8](https://windows.microsoft.com/windows-8/apps).  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire le cartelle Server](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md)  
  
-   [Gestire l'archiviazione Server](../manage/Manage-Server-Storage-in-Windows-Server-Essentials.md)  
  

-   [Connessione](Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Lavorare in modalità remota](Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Riprodurre file multimediali digitali](Play-Digital-Media-in-Windows-Server-Essentials.md)

-   [Connessione](../use/Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Lavorare in modalità remota](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Riprodurre file multimediali digitali](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)

