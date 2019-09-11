---
title: Creare desktop virtuali di Windows 10 Enterprise per le stazioni
description: Informazioni su come creare desktop Windows Server 2016 per la stazione
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 63f08b5b-c735-41f4-b6c8-411eff85a4ab
author: evaseydl
ms.author: evas
manager: scottman
ms.openlocfilehash: e68412808e037b788d5b25c1c2c7b14253e40ea6
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871733"
---
# <a name="create-windows-10-enterprise-virtual-desktops-for-stations"></a>Creare desktop virtuali di Windows 10 Enterprise per le stazioni
Questa configurazione facoltativa in servizi MultiPoint è destinata principalmente in situazioni in cui un'applicazione essenziale richiede un'istanza di un sistema operativo client per ciascun utente. Sono esempi di applicazioni che non possono essere installate in Windows Server e applicazioni che non vengono eseguiranno più istanze nello stesso computer host.  
  
> [!NOTE]  
> Questi desktop virtuali, noto anche come VDI, sono molto più risorse rispetto alle sessioni di desktop ai servizi MultiPoint predefinito, pertanto si consiglia di utilizzare le sessioni di servizi MultiPoint predefinito quando possibile.  
  
## <a name="prerequisites"></a>Prerequisiti  
Per prepararsi alla creazione stazione di desktop virtuali, assicurarsi che i servizi MultiPoint sistema soddisfi i requisiti seguenti:      
  
|Hardware|Requisiti|         |
|------------|----------------|----------------| 
|CPU (multimedia)|1 core o thread per ogni macchina virtuale|  
|Unità di stato solido (SSD)|Capacità > = 20GB per stazione + 40GB per i servizi MultiPoint host del sistema operativo<br /><br />Lettura casuale\/scrivere IOPS > = 3K per stazione|  
|RAM|2GB per stazione + 2GB per il sistema operativo host Windows MultiPoint Server|  
|Grafica|DX11|  
|BIOS|Impostazione del BIOS CPU configurato per abilitare la virtualizzazione – secondo SLAT Level Address Translation)|  
  
-   **Le stazioni** -impostare stazioni per il sistema di servizi MultiPoint. Per ulteriori informazioni, vedere [collegare stazioni aggiuntive ai servizi MultiPoint](Attach-additional-stations-to-your-MultiPoint-services-computer.md).  
  
-   **Dominio** : In un ambiente di dominio, il computer Windows MultiPoint Server è stato aggiunto al dominio e un utente di dominio è stato aggiunto al gruppo Administrators locale nel sistema operativo host servizi MultiPoint.  
  
## <a name="procedures"></a>Procedure  
Usare le procedure seguenti per:  
  
-   [Creare un modello per i desktop virtuali](#create-a-template-for-virtual-desktops)  
  
-   [Creare desktop virtuali dal modello](#create-virtual-machine-desktops-from-the-template)  
  
-   [Copiare un modello di desktop virtuale esistente](#copy-an-existing-virtual-desktop-template)  
  
### <a name="create-a-template-for-virtual-desktops"></a>Creare un modello per i desktop virtuali  
Prima di poter creare un modello per i desktop virtuali, è necessario abilitare la funzionalità di Desktop virtuali in MultiPoint Server.  
  
##### <a name="to-enable-the-virtual-desktop-feature"></a>Per abilitare la funzionalità di Desktop virtuali  
  
1.  Effettuare l'accesso con il sistema operativo host MultiPoint Server con un account amministratore locale o in un dominio, con un account di dominio che è un membro del gruppo Administrators locale.  
  
2.  Dal **avviare** schermata, aprire Gestione MultiPoint.  
  
3.  Fare clic sul **desktop virtuali** scheda, fare clic su **attivare desktop virtuali**, quindi fare clic su **OK**, e attendere il riavvio del sistema.  
  
Il passaggio successivo consiste nel creare un modello di Desktop virtuali. Si crea un file di disco rigido virtuale (VHD) che è possibile utilizzare come modello per creare i desktop virtuali stazione di gestione MultiPoint letteralmente. È possibile utilizzare il supporto di installazione fisica per Windows o un oggetto. File di immagine ISO a come origine per il modello. È inoltre possibile utilizzare un. Disco rigido Virtuale dell'installazione di Windows. Si noti che per utilizzare un disco di installazione fisico, è necessario inserire il disco prima di iniziare la procedura guidata.  
  
##### <a name="to-create-a-virtual-desktop-template"></a>Per creare un modello di Desktop virtuali  
  
1.  Effettuare l'accesso con il sistema operativo host MultiPoint Server con un account amministratore locale o, nel dominio, un account di dominio che è un membro del gruppo Administrators locale.  
  
2.  Dal **avviare** schermata, aprire Gestione MultiPoint.  
  
3.  Fare clic sui **desktop virtuali** scheda.  
  
4.   Copiare un file con estensione ISO di Windows 10 Enterprise SSD locale.  
  
5.  Nella scheda desktop virtuali, fare clic su **Crea modello di desktop virtuale.**   
  
6.  In **prefisso**, immettere un prefisso da utilizzare per identificare il modello e i desktop virtuali creati con il modello. Il prefisso predefinito è il nome del computer host.  
  
    Il prefisso viene usato per assegnare un nome al modello e alle stazioni di desktop virtuali. Il modello sarà <*prefisso*>-t. Le stazioni desktop virtuale verranno denominate <*prefisso*>-*n*, dove *n* è l'identificatore di espansione.  
  
7.  Immettere un nome utente e password da utilizzare per l'account amministratore locale per il modello. In un dominio, immettere le credenziali dell'account di dominio che verrà aggiunto al gruppo Administrators locale. Questo account consente di accedere al modello e tutte le stazioni desktop virtuale creata dal modello.  
  
8. Fare clic su **OK**, e attendere la creazione del modello per il completamento.  
  
9. Il nuovo modello verrà elencato nella **desktop virtuali** scheda. Il modello verrà disattivato.  
  
Il passaggio successivo consiste nel configurare il modello con il software e l'impostazione che si desidera includere i desktop virtuali. È necessario farlo prima di creare qualsiasi desktop virtuali dal modello.  
  
##### <a name="to-customize-a-virtual-desktop-template"></a>Per personalizzare un modello di desktop virtuale  
  
1.  Effettuare l'accesso con il sistema operativo host MultiPoint server con un account amministratore locale o in un dominio, con un account di dominio del gruppo Administrators locale.  
  
2.  Dal **avviare** schermata, aprire Gestione MultiPoint.  
  
3.  Fare clic sui **desktop virtuali** scheda.  
  
4.  Selezionare il modello che si desidera personalizzare, fare clic su **Personalizza modello**, quindi fare clic su **OK**.  
  
    > [!NOTE]  
    > Sono disponibili solo i modelli che non sono stati utilizzati per creare le stazioni di desktop virtuali. Se si desidera aggiornare un modello che è già in uso, è necessario eseguire una copia del modello utilizzando il **Importa modello** attività, descritto più avanti in [copiare un modello di desktop virtuale esistente](#copy-an-existing-virtual-desktop-template).  
  
    Il modello verrà aperto in Hyper-V **VM connettersi** finestra e l'accesso automatico viene eseguita utilizzando l'account Administrator predefinito.  
  
5.  A questo punto è possibile installare gli aggiornamenti software e applicazioni, modificare le impostazioni e aggiornare il profilo amministratore. Tutte le modifiche apportate al profilo amministratore predefinito del modello verranno copiate nel profilo utente predefinito nelle stazioni desktop virtuali create dal modello.  
  
    Se ci si connette le stazioni in un dominio, si consiglia creare un account utente locale e aggiungerlo al gruppo Administrators locale durante la personalizzazione.  
  
    > [!NOTE]  
    > Se il riavvio del sistema mentre è da personalizzare un modello, utilizzando l'account Administrator predefinito l'accesso automatico potrebbe non riuscire dopo il riavvio del sistema. Per risolvere questo problema, accedere manualmente utilizzando l'account amministratore locale che è stato creato, modificare la password dell'account amministratore predefinito, disconnettersi e quindi eseguire nuovamente l'accesso utilizzando l'account Administrator predefinito e la nuova password. (È necessario eliminare il profilo creato quando l'utente connesso utilizzando l'account amministratore locale.)  
  
6.  Al termine della configurazione del sistema, fare doppio clic sul collegamento **CompleteCustomization** sul desktop dell'amministratore per eseguire Sysprep, quindi arrestare il modello. Durante la personalizzazione, lo strumento Sysprep rimuove tutte le informazioni di sistema univoco per preparare l'installazione di Windows per l'Imaging.  
  
### <a name="create-virtual-machine-desktops-from-the-template"></a>Creare desktop delle macchine virtuali dal modello  
Con il modello di desktop virtuale configurato nel modo desiderato i desktop sia, si è pronti per iniziare la creazione di desktop virtuali. Verrà creato un desktop virtuale per ogni stazione di cui è collegata al computer MultiPoint Server. Alla successiva che un utente accede a una stazione visualizzano il desktop virtuale anziché il desktop basato sulla sessione che è stato visualizzato prima.  
  
> [!NOTE]  
> Questa procedura funziona solo quando MultiPoint Server si trova in *modalità stazione*. Se il sistema è *modalità console*, è possibile passare alla modalità stazione da Gestione MultiPoint. Se si utilizzano impostazioni MultiPoint predefinito, è inoltre possibile avviare la modalità di espansione riavviando il computer. Per impostazione predefinita, il computer MultiPoint Server viene sempre avviato in modalità stazione  
  
##### <a name="to-create-virtual-desktops-for-your-stations"></a>Per creare i desktop virtuali per le stazioni  
  
1.  Accedere al server di Windows MultiPoint da una stazione remota (ad esempio, da un computer Windows utilizzando connessione Desktop remoto) utilizza un amministratore locale di account o, in un dominio, un account di dominio del gruppo Administrators locale.  
  
    > [!NOTE]  
    > In alternativa, è possibile accedere al server di utilizzo di una workstation locale. Tuttavia, quando si crea un desktop virtuale stazione, sarà necessario disconnettersi stazione di utilizzato per creare la sessione di desktop virtuali per potersi connettere altre stazioni sul nuovo desktop virtuale.  
  
2.  Dal **avviare** schermata, aprire Gestione MultiPoint.  
  
3.  Se il computer è in modalità console, passare alla modalità di espansione:  
  
    1.  Nel **Home** scheda, fare clic su **passa alla modalità stazione**.  
  
    2.  Al riavvio del computer, accedere come amministratore.  
  
4.  Fare clic sui **desktop virtuali** scheda.  
  
5.  Selezionare il modello di desktop virtuale che si desidera utilizzare con le stazioni, fare clic su **creare stazioni desktop virtuale**, quindi fare clic su **OK**.  
  
Al completamento dell'attività, ciascuna stazione locale si connetterà a un desktop virtuale basato su macchina virtuale.  
  
> [!NOTE]  
> Se un account utente è connesso a uno dei posti di locale, è necessario disconnettersi dalla sessione per ottenere la stazione a connettersi a uno dei desktop virtuali stazione appena creato.  
  
### <a name="copy-an-existing-virtual-desktop-template"></a>Copiare un modello di desktop virtuale esistente  
Utilizzare la procedura seguente per creare una copia di un modello di desktop virtuale esistente che è possibile personalizzare e utilizzare. Ciò può risultare utile nelle situazioni seguenti:  
  
-   Per copiare un modello master da una condivisione di rete in un computer host MultiPoint Server in modo che le stazioni di desktop virtuali possono essere create da del modello master.  
  
-   Per creare una copia di un modello che è attualmente in uso in modo da poter apportare ulteriori personalizzazioni.  
  
##### <a name="to-import-a-virtual-desktop-template"></a>Per importare un modello di desktop virtuale  
  
1.  Accedere al server MultiPoint come amministratore.  
  
2.  Dal **avviare** schermata, aprire Gestione MultiPoint.  
  
3.  Fare clic sui **desktop virtuali** scheda.  
  
4.  Fare clic su **modello desktop virtuale importazione**, e utilizzare **Sfoglia** per selezionare il file con estensione vhd (modello) che si desidera importare. Quando si importa un modello, viene creata una copia di file con estensione vhd originale. Per impostazione predefinita, ai servizi MultiPoint vengono archiviati i file con estensione vhd nell'unità c:\\utenti\\Pubblica\\documenti\\Hyper\-V\\i dischi rigidi virtuali\\ cartella.  
  
5.  Immettere un prefisso per il nuovo modello e quindi fare clic su **OK**.  
  
6.  Se si apportano ulteriori personalizzazioni in un modello locale, è possibile modificare il nome del prefisso incrementando il numero di versione alla fine del prefisso. In alternativa, se si importa un modello master, si desidera aggiungere la versione del modello master alla fine del nome del prefisso predefinito.  
  
7.  Al completamento dell'attività, è possibile personalizzare il modello o utilizzarla come creare le stazioni.  
