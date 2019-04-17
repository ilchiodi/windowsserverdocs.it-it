---
title: Aggiungere una scheda a impostazioni
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aac6b7f3-9020-46c3-a83f-b81542300385
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9eaa1aa5a9c5e8d4c2e36f2000e0adecc83245d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-tab-to-settings"></a>Aggiungere una scheda a impostazioni

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile aggiungere una scheda a impostazioni nel Dashboard creando e installando un assembly di codice che viene utilizzato da Gestione impostazioni nel sistema operativo.  
  
## <a name="add-a-tab-to-settings"></a>Aggiungere una scheda a impostazioni  
 Aggiungere una scheda a impostazioni eseguendo le operazioni seguenti:  
  
-   [Aggiunta di un'implementazione dell'interfaccia ISettingsData all'assembly](Add-a-Tab-to-Settings.md#BKMK_ISettingsData).  
  
-   [La firma dell'assembly con una firma Authenticode](Add-a-Tab-to-Settings.md#BKMK_SignAssembly).  
  
-   [Installare l'assembly nel computer di riferimento](Add-a-Tab-to-Settings.md#BKMK_InstallAssembly).  
  
###  <a name="BKMK_ISettingsData"></a>Aggiunta di un'implementazione dell'interfaccia ISettingsData all'assembly  
 L'interfaccia ISettingsData è inclusa nello spazio dei nomi Microsoft dell'assembly AdminCommon.dll che si trova in \Program Files\Windows Server\Bin..  
  
##### <a name="to-add-the-isettingsdata-code-to-the-assembly"></a>Per aggiungere il codice ISettingsData all'assembly  
  
1.  Aprire Visual Studio 2010 come amministratore facendo clic con il programma di **Start** menu e selezionando **Esegui come amministratore**.  
  
2.  Fare clic su **File**, fare clic su **New**, quindi fare clic su **progetto**.  
  
3.  Nel **nuovo progetto** la finestra di dialogo, fare clic su **Visual c#**, fare clic su **libreria di classi**, immettere **DashboardSettingsPage** per il nome per la soluzione, quindi fare clic su **OK**.  
  
    > [!IMPORTANT]
    >  L'assembly installato sul server deve chiamarsi DashboardSettingsPage.dll e quindi copia il file dll in %ProgramFiles%\Windows server\bin\OEM..  
  
4.  Creare il controllo che si desidera utilizzare nella scheda. In questo esempio il controllo delle impostazioni è denominato MySettingsControl.  
  
5.  Rinominare il file Class1.cs. Ad esempio, MySettingTab.cs.  
  
6.  Aggiungere un riferimento al file AdminCommon.dll.  
  
7.  Aggiungere la seguente istruzione using:  
  
    ```c#  
    using Microsoft.WindowsServerSolutions.Settings;  
    ```  
  
8.  Modificare lo spazio dei nomi e l'intestazione della classe in base all'esempio seguente:  
  
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
  
10. Aggiungere il costruttore per la classe. L'esempio di codice seguente mostra il costruttore:  
  
    ```  
  
    public MySettingTab()  
    {  
       tab = new MySettingsControl();  
    }  
    ```  
  
11. Aggiungere il metodo Commit, che invia le modifiche alle impostazioni. L'esempio di codice seguente mostra il metodo Commit:  
  
    ```  
  
    void ISettingsData.Commit(bool dismissed)  
    {  
       // Implement the code that is required to submit your setting changes  
    }  
    ```  
  
12. Aggiungere il metodo TabControl, che identifica il controllo per la scheda. L'esempio di codice seguente mostra il metodo TabControl:  
  
    ```  
  
    System.Windows.Forms.Control ISettingsData.TabControl  
    {  
       get { return tab; }  
    }  
    ```  
  
13. Aggiungere il metodo TabId, che fornisce un identificatore univoco per la scheda. L'esempio di codice seguente mostra il metodo TabId:  
  
    ```  
  
    private Guid id = Guid.NewGuid();  
  
    Guid ISettingsData.TabId  
    {  
       get { return id; }  
    }  
    ```  
  
14. Aggiungere il metodo TabOrder, che restituisce l'ordine della scheda. L'esempio di codice seguente mostra il metodo TabOrder:  
  
    ```  
  
    int ISettingsData.TabOrder  
    {  
       get { return 0; }  
    }  
    ```  
  
    > [!NOTE]
    >  L'ordine di tabulazione è definito utilizzando i numeri a partire da 0. Le schede di impostazioni predefinite Microsoft vengono visualizzate per primi e quindi le schede visualizzate in base l'ordine di tabulazione definiti. Ad esempio, se si dispone di tre schede di impostazioni, specificare l'ordine di tabulazione come 0, 1 e 2 in base all'ordine che si desidera che le schede da visualizzare.  
  
15. Aggiungere il metodo TabTitle, che fornisce il titolo della scheda. L'esempio di codice seguente mostra il metodo TabTitle:  
  
    ```  
  
    string ISettingsData.TabTitle  
    {  
      get { return "My Settings Tab"; }  
    }  
    ```  
  
    > [!NOTE]
    >  Il testo del titolo può inoltre provenire da un file di risorse in base alle esigenze di localizzazione.  
  
16. Salvare e generare la soluzione.  
  
###  <a name="BKMK_SignAssembly"></a>Una firma Authenticode all'assembly  
 È necessario applicarvi una firma Authenticode all'assembly per poter essere utilizzato nel sistema operativo. Per ulteriori informazioni sulla firma dell'assembly, vedere [firma e il controllo del codice con Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
###  <a name="BKMK_InstallAssembly"></a>Installare l'assembly nel computer di riferimento  
 Dopo la corretta compilazione della soluzione, inserire una copia del file DashboardSettingsPage.dll nella seguente cartella sul computer di riferimento:  
  
 **%ProgramFiles%\Windows server\bin\OEM.**  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)