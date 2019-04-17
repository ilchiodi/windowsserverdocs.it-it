---
title: Creare il File Cfg.ini
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 93a73556-22ef-402d-b8d4-582b74c22bcf
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 967db5f36ea27fb04eab9a6682a106ba0072d45d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="create-the-cfgini-file"></a>Creare il File Cfg.ini

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Il file cfg.ini consente di automatizzare un'installazione del sistema operativo nello scenario seguente:  
  
-   Quando si verifica l'esperienza dell'utente finale con un'immagine preinstallata sul computer di destinazione, la sezione di configurazione iniziale viene utilizzata per eseguire l'installazione in modalità manuale o automatica. A tale scopo, vedere [creare la sezione di configurazione iniziale](Create-the-Cfg.ini-File.md#BKMK_CreateInit2).  
  
##  <a name="BKMK_CreateInit2"></a>Creare la sezione di configurazione iniziale  
 Utilizzare la sezione di configurazione iniziale del file cfg.ini per eseguire l'installazione in modalità manuale o automatica.  
  
#### <a name="to-define-the-initial-configuration-section"></a>Per definire la sezione di configurazione iniziale  
  
1.  Aprire il file cfg.ini nel blocco note, se presente. in caso contrario, creare un nuovo file.  
  
2.  Aggiungere il testo seguente per creare una sezione InitialConfiguration.  
  
    ```  
  
    [InitialConfiguration]  
    ;Optional, display language can only be one of the installed language  
    Language=en-us  
    ;Optional, The name of a script that runs after setupComplete.cmd but before the initial configuration begins.  
    ;Optional  
    Locale=en-us  
    ;Optional  
    Country=US  
    ;Optional  
    Keyboard=0409:00000409  
    AcceptEula=true  
    ;This is only required on a server where an OEM EULA has been specified   
    ;by using the OOBE.xml file  
    AcceptOEMEula=true  
    ;Optional. Example: My Company Name  
    CompanyName=EnterCompanyName  
    ServerName=EnterServerName  
    ; Example: CONTOSO  
    NetbiosName=EnterNetbiosDomainName  
    ; Example: contoso.local  
    DNSName=EnterDNSDomain   
    ; Used to set the user name for the domain admin  
    UserName=EnterDomainAdminUserName  
    ;The password has to be strong and at least 8 characters  
    PlainTextPassword=EnterAdminPassword  
    ;. Used to set the user name for the domain standard user account. Ignored in migration mode.  
    StdUserName=EnterDomainStandardUserName  
    ;. The password for the domain standard user account has to be strong and at least 8 characters  
    StdUserPlainTextPassword=EnterStandardUserPassword  
    ;Controls the Watson and automatic update settings  
    Settings=All or Updates or None  
    WebDomainName=www.abc.com  
    TrustedCertFileName=c:\cert\a.pfx  
    TrustedCertPassword=Enteryourpassword  
    EnableVPN=true  
    EnableRWA=true  
    IPv4DNSForwarder=<IPV4Address,IPV4Address,¦>  
    IPv6DNSForwarder=<IPV6Address,IPV6Address,¦>  
    VpnIPv4StartAddress=<IPV4Address>  
    VpnIPv4EndAddress=<IPV4Address>  
    VpnBaseIPv6Address=<IPV6Address>  
    VpnIPv6PrefixLength=<number>  
    ;All these section are optional.  
     [PostOSInstall]  
    ;Optional, The name of a script that runs after setupComplete.cmd but before the initial configuration begins.  
  
    IsHosted=true  
    StaticIPv4Address=<IPV4Address>  
    StaticIPv4Gateway=<IPV4Address>  
    StaticIPv4SubnetMask=<IPV4SubnetMask>   
    StaticIPv6Address=<IPV6Address>  
    StaticIPv6SubnetPrefixLength=<number>  
    StaticIPv6Gateway=<IPV6Address>  
    ClientBackupOn=true  
    FileHistoryOn=true  
    LaunchPadHiddenTasks=<Microsoft.LaunchPad.AdminDashboard,Microsoft.LaunchPad.Backup>  
  
    ```  
  
    > [!NOTE]
    >  Non è disponibile un'opzione per selezionare una lingua diversa durante la configurazione iniziale. Se il sistema viene ripristinato, la lingua del sistema operativo sarà quella installata originariamente.  
  
    |Nome del parametro|Descrizione parametro|  
    |--------------------|---------------------------|  
    |*AcceptEula*|Indica che l'utente accetta le condizioni di licenza Software Microsoft. Il valore può essere True o False, ma l'installazione continua solo se è impostato su True.|  
    |*AcceptOEMEula*|(Facoltativo) Indica che l'utente accetta il contratto di licenza partner. Il valore può essere True o False. Questo campo è obbligatorio solo se il server è stato acquistato da un partner che ha fornito un contratto di licenza separati.|  
    |*CompanyName*|(Facoltativo) Nome della società. Il nome della società viene utilizzato per associare il server della società e personalizzare i rapporti aziendali. Può essere un massimo di 254 caratteri.|  
    |*Paese*|(Facoltativo) Stringa che rappresenta il paese/regione desiderato. Ad esempio: US per Stati Uniti.|  
    |*NomeServer*|Il nome del server identifica in modo univoco il server sulla rete. Il nome del server deve soddisfare i criteri seguenti:<br /><br /> -Può essere fino a 15 caratteri.<br /><br /> -Può contenere lettere, numeri e trattini (-).<br /><br /> -Non può iniziare con un trattino.<br /><br /> -Non deve contenere spazi.<br /><br /> -Non può contenere solo numeri.<br /><br /> Esempio: ContosoServer.|  
    |*DNSName*|Un dominio interno di gruppi di computer server e client per condividere un database comune di nomi utente, password e altre informazioni comuni. Gli utenti questo nome viene visualizzano quando accedono ai computer, ma viene utilizzato solo internamente e non è lo stesso nome di dominio Internet. Il nome di dominio interno deve soddisfare i criteri specificati per il *nomeserver*.<br /><br /> Esempio: contoso. Local.|  
    |*Nomenetbios*|Un nome NetBIOS viene utilizzato per identificare le risorse a cui sono in esecuzione sul server. Può essere fino a 15 caratteri. Esempio: Contoso.|  
    |*Lingua*|(Facoltativo) Specifica la lingua di visualizzazione. Può essere solo una delle lingue installate. Ad esempio: en-us per l'inglese utilizzato negli Stati Uniti.|  
    |*Impostazioni locali*|(Facoltativo) Specifica il formato ora e valuta usando un *LocaleID* formato. Ad esempio: en-us per valuta e l'ora visualizzate in inglese e formato che rispetta gli standard utilizzati negli Stati Uniti.|  
    |*Tastiera*|La tastiera può utilizzare i seguenti due formati:<br /><br /> - **layout di tastiera: lingua di input.** Ad esempio: 0409: 00000409, dove 0409 prima **:** è la lingua di input e **00000409** è il layout di tastiera. È possibile trovare l'elenco dei layout di tastiera nella chiave del Registro di sistema **layout HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Keyboard**.<br /><br /> - **lingua di input: l'identificatore IME.** Di seguito è riportato un elenco completo degli identificatori IME.<br /><br /> -Metodo di Input Amharico {e429b25a-e5d3-4d1f-9be3-0c608477e3a1 {8f96574e-c86c-4bd6-9666-3f7327d4cbe8}<br /><br /> -{81d4e9c9-1d3b-41bc-9e6c-4b40bf79e35e} {fa550b04-5ad7-411f-a5ac-ca038ec515d7} Microsoft Pinyin - Simple Fast (cinese semplificato)<br /><br /> -{531fdebf-9b4c-4A43-a2aa-960e8fcdc732} b2f9c502-1742-11D4-9790-0080c882687e} cinese (tradizionale) - nuova fonetica<br /><br /> -{531fdebf-9b4c-4A43-a2aa-960e8fcdc732} 4bdf9f03-c7d3-11D4-b2ab-0080c882687e} cinese (tradizionale) - ChangJei<br /><br /> -{531fdebf-9b4c-4A43-a2aa-960e8fcdc732} 6024b45f-5c54-11D4-b921-0080c882687e} cinese (tradizionale) - rapido<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{D38EFF65-AA46-4FD5-91A7-67845FB02F5B} matrice tradizionali cinese<br /><br /> -{E429b25a-e5d3-4d1f-9be3-0c608477e3a1} 037b2c25-480C-4d7f-b027-d6ca6b69788a} DaYi tradizionali cinese<br /><br /> -{03B5835F-F03C-411B-9CE2-AA23E1171E36}{A76C93D9-5523-4E90-AAFA-4DB112F9AC76} Microsoft IME (giapponese)<br /><br /> -{A028ae76-01b1-46c2-99c4-acd9858ae02f} {b5fe1f02-d5f2-4445-9c03-c568f23c99a1} Microsoft IME (coreano)<br /><br /> -{A1e2b86b-924a-4d43-80f6-8a820df7190f} {b60af051-257a-46bc-b9d3-84dad819bafb} Old Hangul IME (coreano)<br /><br /> -Metodo di Input Yi {E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{409C8376-007B-4357-AE8E-26316EE3FB0D}<br /><br /> -Metodo di Input Tigrinya {e429b25a-e5d3-4d1f-9be3-0c608477e3a1 {3cab88b7-cc3e-46a6-9765-b772ad7761ff}|  
    |*Impostazioni*|Imposta la selezione dell'utente per gli aggiornamenti. Utilizzare uno dei valori seguenti:<br /><br /> **-Tutti** è uguale a Usa impostazioni consigliata.<br /><br /> **-Aggiornamenti** è uguale a installare gli aggiornamenti importanti. solo<br /><br /> **-Nessuna** è uguale a controllare gli aggiornamenti.|  
    |*Nome utente*|-Il nome del nuovo account amministratore creato durante l'installazione. L'amministratore e i nomi di account utente standard devono soddisfare i criteri seguenti:<br /><br /> -Può essere fino a 19 caratteri.<br /><br /> -Non può contenere / \ [] & #124; < > + =;, ? *<br /><br /> -Non possono iniziare o terminare con un punto.<br /><br /> -Non può contenere due punti consecutivi.<br /><br /> -Deve essere lo stesso come il nome del server o il nome di dominio interno.<br /><br /> -Deve essere lo stesso come un nome predefinito, ad esempio amministratore o Guest.|  
    |*PlainTextPassword*|Questa è la password per il nuovo account amministratore creato durante l'installazione.<br /><br /> -Deve essere lunga almeno otto caratteri.<br /><br /> -Deve contenere almeno tre delle quattro categorie seguenti:<br /><br /> -Caratteri maiuscoli.<br /><br /> -Caratteri minuscoli.<br /><br /> -Numeri.<br /><br /> -Simboli.|  
    |*StdUserName*|Il nome del nuovo account utente standard creato durante l'installazione. Vedere il *UserName* parametro per i requisiti.|  
    |*StdUserPlainTextPassword*|La password per l'account utente standard creato durante l'installazione.|  
    |WebDomainName|(Facoltativo) Configurare il nome di dominio Internet del server. Questo file consente di configurare il nome di dominio simile al metodo utilizzato per la configurazione manuale della procedura guidata imposta nome di dominio.|  
    |TrustedCertFileName|(Facoltativo) Configura certificato attendibile per il nome di dominio. Ciò consente di inserire un. Certificato PFX che contiene la chiave privata.|  
    |TrustedCertPassword|(Facoltativo) La password per l'importazione di. PFX.|  
    |EnableVPN|(Facoltativo) Attivare VPN per impostazione predefinita.|  
    |VpnIPv4StartAddress|(Facoltativo) Impostare l'indirizzo iniziale VPN.|  
    |VpnIPv4EndAddress|(Facoltativo) Impostare l'indirizzo VPN di fine.|  
    |VpnBaseIPv6Address|(Facoltativo) Impostare l'indirizzo IPV6 di base per la VPN.|  
    |VpnIPv6PrefixLength|(Facoltativo) Impostare la lunghezza del prefisso dell'indirizzo IPv6 VPN.|  
    |IsHosted|(Facoltativo) Valore predefinito è false se non specificato. Impostare questo valore se si imposta questo in un ambiente ospitante. Disabiliterà la configurazione del router.|  
    |StaticIPv4Address|(Facoltativo) Specificare l'indirizzo IP statico se si desidera configurare un indirizzo IP statico piuttosto che dinamico.|  
    |StaticIPv4Gateway|(Facoltativo) Specificare l'indirizzo gateway predefinito se si desidera configurare un indirizzo IP statico piuttosto che dinamico.|  
    |StaticIPv4SubnetMask|(Facoltativo) Specificare la subnet mask se si desidera configurare un indirizzo IP statico piuttosto che dinamico.|  
    |StaticIPv6Address|(Facoltativo) Specificare l'indirizzo IP predefinito se si desidera configurare un indirizzo IP statico piuttosto che dinamico.|  
    |StaticIPv6SubnetPrefixLength|(Facoltativo) Specificare lunghezza prefisso subnet IPV6 predefinito se si desidera configurare un indirizzo IP statico piuttosto che dinamico.|  
    |StaticIPv6Gateway|(Facoltativo) Specificare l'indirizzo gateway predefinito se si desidera configurare un indirizzo IP statico piuttosto che dinamico.|  
    |ClientBackupOn|(Facoltativo) Disattivare Client backup per impostazione predefinita quando i nuovi client aggiunti al server.|  
    |FileHistoryOn|(Facoltativo) Disattiva cronologia File di backup per impostazione predefinita quando i nuovi client che eseguono Windows 8 Consumer Preview vengono aggiunti al server.|  
    |EnableRWA|Abiliterà accesso Web remoto quando si installa Windows Server Essentials, ma ignorerà la configurazione del router. È supportata solo in installazione pulita del prodotto. Il valore predefinito è false.|  
    |IPv4DNSForwarder|Impostare i server d'inoltro DNS IPv4.|  
    |IPv6DNSForwarder|Impostare i server d'inoltro DNS IPv6.|  
    |LaunchPadHiddenTasks|-È possibile nascondere la voce di Backup (facoltativo) o / e Dashboard di amministrazione voce nella finestra di avvio.<br /><br /> -Per disabilitare il dashboard: Admindashboard<br /><br /> -Per disabilitare il backup: Launchpadhiddentasks<br /><br /> -Per disabilitare il backup e il dashboard: Admindashboard|  
  
3.  Salvare il file. Assicurarsi di salvare il file come cfg.ini, non cfg.ini.txt.  
  
    > [!NOTE]
    >  È possibile salvare il file in un'unità flash USB, può essere usata per fasi specifiche dell'installazione, oppure il file cfg.ini può trovarsi nella directory principale di qualsiasi unità disco rigido del server di destinazione. È necessario assicurarsi che la codifica per il file è impostata su ANSI o Unicode, codifica UTF-8 non è supportata.  
  
> [!IMPORTANT]
>  La sezione di configurazione iniziale di cfg.ini il deve essere utilizzata solo dall'utente finale per personalizzare il server o da un partner per verificare l'esperienza utente del server utilizzando un file di risposte automatico. In questa sezione del file non deve essere utilizzato per la creazione dell'immagine.  
  
## <a name="see-also"></a>Vedere anche  

 [Guida introduttiva a Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)

 [Guida introduttiva a Windows Server Essentials ADK](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Creazione e personalizzazione dell'immagine](../install/Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](../install/Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](../install/Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](../install/Testing-the-Customer-Experience.md)

