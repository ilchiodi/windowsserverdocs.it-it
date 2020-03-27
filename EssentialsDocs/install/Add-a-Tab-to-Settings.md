---
title: Aggiungere una scheda a Impostazioni
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aac6b7f3-9020-46c3-a83f-b81542300385
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: a37fd65b143e800a76bac9a77daa4b400426c805
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310159"
---
# <a name="add-a-tab-to-settings"></a>Aggiungere una scheda a Impostazioni

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile aggiungere una scheda a Impostazioni nel dashboard creando e installando un assembly del codice utilizzato da Gestione impostazioni nel sistema operativo.  
  
## <a name="add-a-tab-to-settings"></a>Aggiunta di una scheda a Impostazioni  
 È possibile aggiungere una scheda a Impostazioni facendo quanto segue:  
  
-   [Aggiungere un'implementazione dell'interfaccia ISettingsData all'assembly](Add-a-Tab-to-Settings.md#BKMK_ISettingsData).  
  
-   [Sign the assembly with an Authenticode signature](Add-a-Tab-to-Settings.md#BKMK_SignAssembly).  
  
-   [Installare l'assembly nel computer di riferimento](Add-a-Tab-to-Settings.md#BKMK_InstallAssembly).  
  
###  <a name="add-an-implementation-of-the-isettingsdata-interface-to-the-assembly"></a><a name="BKMK_ISettingsData"></a>Aggiungere un'implementazione dell'interfaccia Isettingsdata all'all'assembly  
 L'interfaccia ISettingsData è inclusa nello spazio dei nomi Microsoft.WindowsServerSolutions.Settings dell'assembly AdminCommon.dll, situato in \Program Files\Windows Server\Bin.  
  
##### <a name="to-add-the-isettingsdata-code-to-the-assembly"></a>Per aggiungere il codice ISettingsData all'assembly  
  
1.  Accedere a Visual Studio 2010 come amministratore facendo clic con il pulsante destro del mouse sul programma nel menu **Start**, quindi selezionando **Esegui come amministratore**.  
  
2.  Fare clic su **File**, quindi su **Nuovo** e infine su **Progetto**.  
  
3.  Nella finestra di dialogo **Nuovo progetto** fare clic su **Visual C#** e poi su **Libreria di classi**, immettere **DashboardSettingsPage** come nome della soluzione e quindi fare clic su **OK**.  
  
    > [!IMPORTANT]
    >  L'assembly installato sul server deve chiamarsi DashboardSettingsPage.dll e deve essere copiato in %ProgramFiles%\Windows Server\Bin\OEM.  
  
4.  Creare il controllo che si desidera utilizzare nella scheda. In questo esempio il controllo Settings è denominato MySettingsControl.  
  
5.  Rinominare il file Class1.cs. Ad esempio, MySettingTab.cs.  
  
6.  Aggiungere un riferimento al file AdminCommon.dll.  
  
7.  Aggiungere la seguente istruzione using:  
  
    ```c#  
    using Microsoft.WindowsServerSolutions.Settings;  
    ```  
  
8.  Modificare lo spazio dei nomi e l'intestazione della classe in modo tale che corrispondano all'esempio seguente:  
  
    ```  
  
    namespace DashboardSettingsPage  
    {  
        public class MySettingTab : ISettingsData  
        {  
        }  
    }  
  
    ```  
  
9. Creare un'istanza del controllo creato per la scheda. Per esempio:  
  
    ```c#  
    private MySettingsControl tab;  
    ```  
  
10. Aggiungere il costruttore per la classe. Nell'esempio di codice riportato di seguito viene illustrato il costruttore:  
  
    ```  
  
    public MySettingTab()  
    {  
       tab = new MySettingsControl();  
    }  
    ```  
  
11. Aggiungere il metodo Commit, che invia le modifiche apportate alle impostazioni. Nell'esempio di codice riportato di seguito viene illustrato il metodo Commit:  
  
    ```  
  
    void ISettingsData.Commit(bool dismissed)  
    {  
       // Implement the code that is required to submit your setting changes  
    }  
    ```  
  
12. Aggiungere il Metodo TabControl, che identifica il controllo per la scheda. Nell'esempio di codice seguente viene illustrato il Metodo TabControl:  
  
    ```  
  
    System.Windows.Forms.Control ISettingsData.TabControl  
    {  
       get { return tab; }  
    }  
    ```  
  
13. Aggiungere il metodo TabId, che fornisce un identificatore univoco per la scheda. Nell'esempio di codice seguente viene illustrato il metodo TabId:  
  
    ```  
  
    private Guid id = Guid.NewGuid();  
  
    Guid ISettingsData.TabId  
    {  
       get { return id; }  
    }  
    ```  
  
14. Aggiungere il metodo TabOrder, che restituisce l'ordine della scheda. Nell'esempio di codice seguente viene illustrato il metodo TabOrder:  
  
    ```  
  
    int ISettingsData.TabOrder  
    {  
       get { return 0; }  
    }  
    ```  
  
    > [!NOTE]
    >  L'ordine delle schede viene definito utilizzando i numeri a partire da 0. Le schede delle impostazioni predefinite Microsoft vengono visualizzate per prime, seguite poi dalle schede dell'utente visualizzate in base all'ordine da questo definito. Ad esempio, se esistono tre schede di impostazioni, l'utente specifica il numero d'ordine delle schede come 0, 1 e 2 in base all'ordine di visualizzazione desiderato.  
  
15. Aggiungere il metodo TabTitle, che fornisce il titolo della scheda. Nell'esempio di codice seguente viene illustrato il metodo TabTitle:  
  
    ```  
  
    string ISettingsData.TabTitle  
    {  
      get { return "My Settings Tab"; }  
    }  
    ```  
  
    > [!NOTE]
    >  Il testo del titolo può inoltre provenire da un file di risorse per rispondere alle esigenze di localizzazione.  
  
16. Salvare e generare la soluzione.  
  
###  <a name="sign-the-assembly-with-an-authenticode-signature"></a><a name="BKMK_SignAssembly"></a>Firmare l'assembly con una firma Authenticode  
 Per poter utilizzare l'assembly nel sistema operativo, è necessario applicarvi una firma Authenticode. Per altre informazioni sulla firma dell'assembly, vedere [Firma e verifica del codice con Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
###  <a name="install-the-assembly-on-the-reference-computer"></a><a name="BKMK_InstallAssembly"></a>Installare l'assembly nel computer di riferimento  
 Una volta generata la soluzione, copiare il file DashboardSettingsPage.dll nella seguente cartella sul computer di riferimento:  
  
 **Server\Bin\OEM%programmi%\Windows**  
  
## <a name="see-also"></a>Vedi anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](Testing-the-Customer-Experience.md)