---
title: Use local resources on Hyper-V virtual machine with VMConnect
description: Descrive i requisiti per l'uso delle risorse locali con VMConnect
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 18eface5-7518-4c6b-9282-93e2e3e87492
author: kbdazure
ms.author: kathydav
ms.date: 12/06/2016
ms.openlocfilehash: 40dd4076a4d1a57c8a1e999669e589dadeb88cbe
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "81650119"
---
# <a name="use-local-resources-on-hyper-v-virtual-machine-with-vmconnect"></a>Use local resources on Hyper-V virtual machine with VMConnect

>Si applica a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2

Virtual Manager Connection (VMConnect) consente di usare risorse locali del computer, come una stampante o un'unità flash USB rimovibile, in una macchina virtuale. La modalità sessione avanzata consente anche di ridimensionare la finestra VMConnect. Questo articolo illustra come configurare l'host e concedere alla macchina virtuale l'accesso a una risorsa locale.

La modalità sessione avanzata e la funzione Incolla qui il testo degli Appunti sono disponibili solo per le macchine virtuali che eseguono sistemi operativi Windows recenti. \(Per informazioni, vedere la sezione [Requisiti per l'uso delle risorse locali](#requirements-for-using-local-resources) più avanti.\) 

Per le macchine virtuali che eseguono Ubuntu, vedere [Modifica risoluzione dello schermo di Ubuntu in una macchina Virtuale Hyper-V](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/). 
  
## <a name="turn-on-enhanced-session-mode-on-a-hyper-v-host"></a>Attivare la modalità sessione avanzata in un host Hyper-V  
Se l'host Hyper-V esegue Windows 10 o Windows 8.1, la modalità sessione avanzata è attivata per impostazione predefinita ed è possibile ignorare questo passaggio, passando direttamente alla sezione successiva. Se invece l'host esegue Windows Server 2016 o Windows Server 2012 R2, eseguire prima questa operazione. 
  
Attivare la modalità sessione avanzata:

1.  Connettersi al computer che ospita la macchina virtuale.  
  
2.  In Hyper-V Manager, selezionare nome del computer dell'host.  
  
    ![Schermata che mostra un host come nome del computer elencato in Gestione Hyper-V nel riquadro a sinistra.](media/Hyper-V-HyperVManager-HostNameSelected.png)  
  
3.  Selezionare **Impostazioni Hyper-V**.  
  
    ![Schermata che mostra l'opzione Impostazioni Hyper-V in azioni nel riquadro di destra.](media/HyperV-ActionsHyperVSettings.png)  
  
4.  Nella sezione **Server**, selezionare **Criteri modalità sessione avanzata**.  
  
    ![Schermata che mostra l'opzione di criteri modalità sessione avanzata nella sezione sicurezza.](media/Hyper-V-Settings-ServerEnhancedSessionModePolicy.png)  
  
5.  Selezionare la casella di controllo **Consenti modalità sessione avanzata** .  
  
    ![Schermata che mostra la Consenti migliorato sessione modalità casella di controllo criteri modalità sessione avanzata.](media/Hyper-V-Settings-EnhancedSessionModePolicyCheckBox.png)  
  
6.  Nella sezione **Utente**, selezionare **Modalità sessione avanzata**.  
  
    ![Schermata che mostra l'opzione della modalità sessione avanzata nella sezione utente. ](media/Hyper-V-Settings-UserEnhancedSessionMode.png)  
  
7.  Selezionare la casella di controllo **Consenti modalità sessione avanzata** .  
  
8.  Fare clic su **OK**.  
  
## <a name="choose-a-local-resource"></a>Scegliere una risorsa locale

Le risorse locali includono stampanti, gli Appunti e un'unità locale nel computer in cui è in esecuzione VMConnect. Per informazioni più dettagliate, vedere la sezione [Requisiti per l'uso delle risorse locali](#requirements-for-using-local-resources) più avanti.  
  
Per scegliere una risorsa locale:
  
1.  Aprire VMConnect.  
  
2.  Spegnere la macchina virtuale a cui ci si vuole connettere.  
  
3.  Fare clic su **Mostra opzioni**.  
  
    ![Schermata che mostra le opzioni nella parte inferiore sinistra della finestra di dialogo chiama.](media/HyperV-VMConnect-DisplayConfig.png)  
  
4.  Selezionare **Risorse locali**.  
  
    ![Schermata in cui viene visualizzato nella scheda risorse locali.](media/HyperV-VMConnect-DisplayConfig-LocalResources.png)  
  
5.  Fare clic su **Altro**.  
  
    ![Schermata in cui viene visualizzato il pulsante altro.](media/HyperV-VMConnect-DisplayConfig-LocalResourcesMore.png)  
  
6.  Selezionare l'unità che si vuole usare nella macchina virtuale e fare clic su **Ok**.  
  
    ![Schermata che mostra le risorse locali e le unità che è possibile selezionare.](media/HyperV-VMConnect-Settings-LocalResourcesDrives.png)  
  
7.  Selezionare **Salva impostazioni per connessioni future alla macchina virtuale**.  
  
    ![Schermata che chiama la casella di controllo per selezionare questa opzione.](media/HyperV-VMConnect-SaveSettings.png)  
  
8.  Fare clic su **Connetti**.  
  
## <a name="edit-vmconnect-settings"></a>Modificare le impostazioni di VMConnect

È possibile modificare facilmente le impostazioni di connessione per VMConnect eseguendo il comando seguente in Windows PowerShell o nel prompt dei comandi:  
  
`VMConnect.exe <ServerName> <VMName> /edit`  
  
> [!Note]
> Può essere necessaria una finestra del prompt dei comandi con privilegi elevati.
  
## <a name="requirements-for-using-local-resources"></a>Requisiti per l'uso delle risorse locali

Per poter usare le risorse locali del computer in una macchina virtuale:  
  
-   È necessario aver attivato le impostazioni **Criteri modalità sessione avanzata** e **Modalità sessione avanzata** dell'host Hyper-V.  
  
-   Il computer in cui si usa VMConnect deve eseguire Windows 10, Windows 8.1, Windows Server 2016 o Windows Server 2012 R2.  
  
-   La macchina virtuale deve avere Servizi Desktop remoto abilitato ed eseguire Windows 10, Windows 8.1, Windows Server 2016 o Windows Server 2012 R2 come sistema operativo guest.  
  
Se il computer che esegue VMConnect e la macchina virtuale soddisfano entrambi i requisiti previsti, è possibile usare una qualsiasi delle risorse locali seguenti, se disponibili:  
  
-   Configurazione dello schermo  
  
-   Audio
  
-   Stampanti  
  
-   Appunti per copiare e incollare  
  
-   Smart card  
  
-   Dispositivi USB  
  
-   Unità  
  
-   Dispositivi Plug and Play supportati  
  
## <a name="why-use-a-computers-local-resources"></a>Perché utilizzare le risorse locali del computer?
È possibile utilizzare le risorse locali del computer per:  
  
-   Risolvere i problemi di una macchina virtuale senza una connessione di rete alla macchina virtuale.  
  
-   Copiare e incollare i file da e verso la macchina virtuale con la stessa procedura usata con una Connessione Desktop remoto.  
  
-   Accedere alla macchina virtuale usando una smart card.  
  
-   Eseguire la stampa da una macchina virtuale su una stampante locale.  
  
-   Testare e risolvere i problemi delle applicazioni dello sviluppatore che richiedono il reindirizzamento dell'audio e dei dispositivi USB senza usare RDP.  
  
## <a name="see-also"></a>Vedi anche  
[Connettersi a una macchina virtuale](https://technet.microsoft.com/library/cc742407.aspx)  
[È necessario creare una macchina virtuale di generazione 1 o 2 in Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)


