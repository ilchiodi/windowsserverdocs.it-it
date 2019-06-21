---
title: Creazione di un metodo di autenticazione personalizzato per ADFS in Windows Server
description: Questo scenario viene descritto come creare un metodo di autenticazione personalizzato per ADFS in Windows Server.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 05/23/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a638ec25be4fc99b4eccd1d9fa541e640ef9e15c
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280657"
---
# <a name="build-a-custom-authentication-method-for-ad-fs-in-windows-server"></a>Creazione di un metodo di autenticazione personalizzato per ADFS in Windows Server

Questa procedura dettagliata fornisce istruzioni per l'implementazione di un metodo di autenticazione personalizzato per ADFS in Windows Server 2012 R2. Per altre informazioni, vedere [metodi di autenticazione aggiuntivi](https://msdn.microsoft.com/library/dn758113\(v=msdn.10\)).


> [!WARNING]
> L'esempio che è possibile compilare qui è&nbsp;esclusivamente per scopi didattici. &nbsp;Queste istruzioni sono valide per l'implementazione più semplice, più minima possibile esporre gli elementi necessari del modello.&nbsp; Non sono back-end di autenticazione, elaborazione degli errori o i dati di configurazione. 
> <P></P>



## <a name="setting-up-the-development-box"></a>Impostare la casella di sviluppo

Questa procedura dettagliata Usa Visual Studio 2012.  Il progetto può essere compilato usando qualsiasi ambiente di sviluppo che è possibile creare una classe .NET per Windows. Il progetto deve avere come destinazione .NET 4.5 in quanto il **BeginAuthentication** e **TryEndAuthentication** metodi usano il tipo **System.Security.Claims.Claim**, parte di .NET 4.5.There versione Framework è un riferimento necessario per il progetto:


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Dll di riferimento</strong></p></td>
<td><p><strong>Dove si trovano</strong></p></td>
<td><p><strong>Obbligatorio per</strong></p></td>
</tr>
<tr class="even">
<td><p>Microsoft.IdentityServer.Web.dll</p></td>
<td><p>La dll si trova in %windir%\ADFS in un server di Windows Server 2012 R2 in cui è stato installato ADFS.</p>
<p></p>
<p>Questa dll deve essere copiata in computer di sviluppo e un riferimento esplicito creati nel progetto.</p></td>
<td><p>Tipi di interfaccia inclusi IAuthenticationContext, IProofData</p></td>
</tr>
</tbody>
</table>


## <a name="create-the-provider"></a>Creare il provider

1.  In Visual Studio 2012: Scegliere File -\>New -\>progetto...

2.  Selezionare una libreria di classi e assicurarsi che si sono destinate a .NET 4.5.

    ![creare il provider](media/ad-fs-build-custom-auth-method/Dn783423.71a57ae1-d53d-462b-a846-5b3c02c7d3f2(MSDN.10).jpg "creare il provider")

3.  Creare una copia del **Microsoft.IdentityServer.Web.dll** % windir %\\ad FS nel server di Windows Server 2012 R2 in cui ADFS è stato installato e incollarlo nella cartella del progetto nel computer di sviluppo.

4.  Nella **Esplora soluzioni**, fare clic destro **riferimenti** e **Aggiungi riferimento...**

5.  Passare alla copia locale del **Microsoft.IdentityServer.Web.dll** e **Aggiungi...**

6.  Fare clic su **OK** per confermare il nuovo riferimento:

    ![creare il provider](media/ad-fs-build-custom-auth-method/Dn783423.f18df353-9259-4744-b4b6-dd780ce90951(MSDN.10).jpg "creare il provider")

    Dovrebbe ora essere configuri risolvere tutti i tipi necessari per il provider. 

7.  Aggiungere una nuova classe al progetto (fare clic con il pulsante destro del progetto, **Aggiungi... Classe...** ) e assegnargli un nome come **MyAdapter**, come illustrato di seguito:

    ![creare il provider](media/ad-fs-build-custom-auth-method/Dn783423.6b6a7a8b-9d66-40c7-8a86-a2e3b9e14d09(MSDN.10).jpg "creare il provider")

8.  Nel nuovo file MyAdapter.cs, sostituire il codice esistente con il codice seguente:

        using System;
         using System.Collections.Generic;
         using System.Linq;
         using System.Text;
         using System.Threading.Tasks;
         using System.Globalization;
         using System.IO;
         using System.Net;
         using System.Xml.Serialization;
         using Microsoft.IdentityServer.Web.Authentication.External;
         using Claim = System.Security.Claims.Claim;

         namespace MFAadapter
         {
         class MyAdapter : IAuthenticationAdapter
         {

         }
         }

    A questo punto dovrebbe essere possibile per F12 (pulsante destro del mouse fare clic su-Vai a definizione) su IAuthenticationAdapter per visualizzare il set di membri di interfaccia necessaria. 

    Successivamente, è possibile eseguire una semplice implementazione di questi.

9.  Sostituire l'intero contenuto della classe con quanto segue:

        namespace MFAadapter
         {
         class MyAdapter : IAuthenticationAdapter
         {
         public IAuthenticationAdapterMetadata Metadata
         {
         //get { return new <instance of IAuthenticationAdapterMetadata derived class>; }
         }

         public IAdapterPresentation BeginAuthentication(Claim identityClaim, HttpListenerRequest request, IAuthenticationContext authContext)
         {
         //return new instance of IAdapterPresentationForm derived class

         }

         public bool IsAvailableForUser(Claim identityClaim, IAuthenticationContext authContext)
         {
         return true; //its all available for now

         }

         public void OnAuthenticationPipelineLoad(IAuthenticationMethodConfigData configData)
         {
         //this is where AD FS passes us the config data, if such data was supplied at registration of the adapter

         }

         public void OnAuthenticationPipelineUnload()
         {

         }

         public IAdapterPresentation OnError(HttpListenerRequest request, ExternalAuthenticationException ex)
         {
         //return new instance of IAdapterPresentationForm derived class

         }

         public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
         {
         //return new instance of IAdapterPresentationForm derived class

         }

         }
         }

10. Non siamo pronti a compilare ancora... sono disponibili due interfacce più passare.

    Aggiungere altre due classi al progetto: uno è per i metadati e l'altro per il formato di presentazione.  È possibile aggiungere questi metodi entro lo stesso file come la classe precedente.

        class MyMetadata : IAuthenticationAdapterMetadata
         {

         }

         class MyPresentationForm : IAdapterPresentationForm
         {

         }

11. Successivamente, è possibile aggiungere i membri obbligatori per ognuno. Innanzitutto, i metadati (con commenti utili inline)

        class MyMetadata : IAuthenticationAdapterMetadata
         {
         //Returns the name of the provider that will be shown in the AD FS management UI (not visible to end users)
         public string AdminName
         {
         get { return "My Example MFA Adapter"; }
         }

         //Returns an array of strings containing URIs indicating the set of authentication methods implemented by the adapter 
         /// AD FS requires that, if authentication is successful, the method actually employed will be returned by the
         /// final call to TryEndAuthentication(). If no authentication method is returned, or the method returned is not
         /// one of the methods listed in this property, the authentication attempt will fail.
         public virtual string[] AuthenticationMethods 
         {
         get { return new[] { "http://example.com/myauthenticationmethod1", "http://example.com/myauthenticationmethod2" }; }
         }

         /// Returns an array indicating which languages are supported by the provider. AD FS uses this information
         /// to determine the best language\locale to display to the user.
         public int[] AvailableLcids
         {
         get
         {
         return new[] { new CultureInfo("en-us").LCID, new CultureInfo("fr").LCID};
         }
         }

         /// Returns a Dictionary containing the set of localized friendly names of the provider, indexed by lcid. 
         /// These Friendly Names are displayed in the "choice page" offered to the user when there is more than 
         /// one secondary authentication provider available.
         public Dictionary<int, string> FriendlyNames
         {
         get
         {
         Dictionary<int, string> _friendlyNames = new Dictionary<int, string>();
         _friendlyNames.Add(new CultureInfo("en-us").LCID, "Friendly name of My Example MFA Adapter for end users (en)");
         _friendlyNames.Add(new CultureInfo("fr").LCID, "Friendly name translated to fr locale");
         return _friendlyNames;
         }
         }

         /// Returns a Dictionary containing the set of localized descriptions (hover over help) of the provider, indexed by lcid. 
         /// These descriptions are displayed in the "choice page" offered to the user when there is more than one 
         /// secondary authentication provider available.
         public Dictionary<int, string> Descriptions
         {
         get 
         {
         Dictionary<int, string> _descriptions = new Dictionary<int, string>();
         _descriptions.Add(new CultureInfo("en-us").LCID, "Description of My Example MFA Adapter for end users (en)");
         _descriptions.Add(new CultureInfo("fr").LCID, "Description translated to fr locale");
         return _descriptions; 
         }
         }

         /// Returns an array indicating the type of claim that the adapter uses to identify the user being authenticated.
         /// Note that although the property is an array, only the first element is currently used.
         /// MUST BE ONE OF THE FOLLOWING
         /// "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"
         /// "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
         /// "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"
         /// "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid"
         public string[] IdentityClaims
         {
         get { return new[] { "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" }; }
         }

         //All external providers must return a value of "true" for this property.
         public bool RequiresIdentity
         {
         get { return true; }
         }
        }

    Passaggio successivo, il formato di presentazione:

        class MyPresentationForm : IAdapterPresentationForm
         {
         /// Returns the HTML Form fragment that contains the adapter user interface. This data will be included in the web page that is presented
         /// to the cient.
         public string GetFormHtml(int lcid)
         {
         string htmlTemplate = Resources.FormPageHtml; //todo we will implement this
         return htmlTemplate;
         }

         /// Return any external resources, ie references to libraries etc., that should be included in 
         /// the HEAD section of the presentation form html. 
         public string GetFormPreRenderHtml(int lcid)
         {
         return null;
         }

         //returns the title string for the web page which presents the HTML form content to the end user
         public string GetPageTitle(int lcid)
         {
         return "MFA Adapter";
         }


~~~
     }
~~~

12. Si noti che l'attività' ' per il **Resources.FormPageHtml** elemento precedente. 

   È possibile risolverlo in un minuto, ma prima è possibile aggiungere le istruzioni return obbligatorie finale, in base ai tipi appena implementati, alla classe MyAdapter iniziale.  A tale scopo, aggiungere gli elementi in *corsivo* sotto per l'implementazione IAuthenticationAdapter esistente:

       classe MyAdapter: IAuthenticationAdapter {IAuthenticationAdapterMetadata Metadata pubblici {//get {restituito nuovo <instance of IAuthenticationAdapterMetadata derived class>;}     get {return nuovo MyMetadata();}     }

        public IAdapterPresentation BeginAuthentication(Claim identityClaim, HttpListenerRequest request, IAuthenticationContext authContext)
        {
        //return new instance of IAdapterPresentationForm derived class
        return new MyPresentationForm();
        }

        public bool IsAvailableForUser(Claim identityClaim, IAuthenticationContext authContext)
        {
        return true; //its all available for now
        }

        public void OnAuthenticationPipelineLoad(IAuthenticationMethodConfigData configData)
        {
        //this is where AD FS passes us the config data, if such data was supplied at registration of the adapter

        }

        public void OnAuthenticationPipelineUnload()
        {

        }

        public IAdapterPresentation OnError(HttpListenerRequest request, ExternalAuthenticationException ex)
        {
        //return new instance of IAdapterPresentationForm derived class
        return new MyPresentationForm();
        }

        public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
        {
        //return new instance of IAdapterPresentationForm derived class
        outgoingClaims = new Claim[0];
        return new MyPresentationForm();
        }

        }

13. Ora del file di risorse contenente il frammento di html. Creare un nuovo file di testo nella cartella del progetto con il contenuto seguente:

       <div id="loginArea">
        <form method="post" id="loginForm" >
        <!-- These inputs are required by the presentation framework. Do not modify or remove -->
        <input id="authMethod" type="hidden" name="AuthMethod" value="%AuthMethod%"/>
        <input id="context" type="hidden" name="Context" value="%Context%"/>
        <!-- End inputs are required by the presentation framework. -->
        <p id="pageIntroductionText">Questo contenuto viene fornito dall'adapter di esempio di autenticazione a più fattori. Gli input di richiesta di verifica dovrebbero essere visualizzati di sotto.</p>
        <label for="challengeQuestionInput" class="block">Testo della domanda</label>
        <input id="challengeQuestionInput" name="ChallengeQuestionAnswer" type="text" value="" class="text" placeholder="Answer placeholder" />
        <div id="submissionArea" class="submitMargin">
        <input id="submitButton" type="submit" name="Submit" value="Submit" onclick="return AuthPage.submitAnswer()"/>
        </div>
        </form>
        <div id="intro" class="groupMargin">
        <p id="supportEmail">Informazioni di supporto</p>
        </div>
        <script type="text/javascript" language="JavaScript">
        //<![CDATA[
        function AuthPage() { }
        AuthPage.submitAnswer = function () { return true; };
        //]]>
        </script></div>

14. Quindi, selezionare **progetto -\>Add Component... Le risorse** file e denominare il file **risorse**, fare clic su **Add:**

   ![creare il provider](media/ad-fs-build-custom-auth-method/Dn783423.3369ad8f-f65f-4f36-a6d5-6a3edbc1911a(MSDN.10).jpg "creare il provider")

15. Quindi, all'interno di **Resources. resx** del file, scegliere **Aggiungi risorsa... Aggiungere file esistente**.  Passare al file di testo (contenente il frammento di html) che si è salvato in precedenza.

   Verificare che il codice GetFormHtml risolve il nome della nuova risorsa correttamente dal prefisso del nome file (file con estensione resx) alle risorse seguito dal nome della risorsa stessa:

       public string GetFormHtml(int lcid)    {     string htmlTemplate = Resources.MfaFormHtml; //Resxfilename.resourcename     return htmlTemplate;    }

   È ora possibile compilare.

## <a name="build-the-adapter"></a>Creare l'adattatore

L'adapter deve essere compilato in un assembly .NET con nome sicuro che può essere installato nella Global Assembly Cache in Windows.  Per ottenere questo risultato in un progetto di Visual Studio, completare i passaggi seguenti:

1.  Pulsante destro del mouse sul nome del progetto in Esplora soluzioni e fare clic su **proprietà**.

2.  Nel **Signing** scheda, verificare **firmare l'assembly** e scegliere **\<New... \>** sotto **Scegli un file chiave con nome sicuro:**  Immettere un nome di file di chiave e una password e fare clic su **OK**.  Assicurarsi quindi **firmare l'assembly** sia selezionata e **solo firma ritardata** sia deselezionata.  La proprietà **firma** pagina dovrebbe essere simile al seguente:

    ![il provider di compilazione](media/ad-fs-build-custom-auth-method/Dn783423.0b1a1db2-d64e-4bb8-8c01-ef34296a2668(MSDN.10).jpg "il provider di compilazione")

3.  Quindi compilare la soluzione.

## <a name="deploy-the-adapter-to-your-ad-fs-test-machine"></a>Distribuisce l'adapter nel computer di test di AD FS

Prima di un provider esterno può essere richiamato da AD FS, deve essere registrato nel sistema.  Provider di adapter deve fornire un programma di installazione che esegue le azioni di installazione necessari inclusa l'installazione nella Global Assembly Cache e il programma di installazione deve supportare la registrazione in AD FS.  Se che non viene eseguita, l'amministratore deve eseguire la procedura di Windows PowerShell seguente.  Questi passaggi sono utilizzabile nel lab per abilitare il test e debug.

### <a name="prepare-the-test-ad-fs-machine"></a>Preparare il computer di test AD FS

Copiare i file e aggiungere alla Global Assembly Cache.

1.  Assicurarsi di che disporre di una macchina virtuale o computer Windows Server 2012 R2.

2.  Installare il servizio ruolo ADFS e configurare una farm con almeno un nodo.

    Per istruzioni dettagliate per configurare un server federativo in un ambiente lab, vedere la [Guida alla distribuzione di Windows Server 2012 R2 AD FS](https://msdn.microsoft.com/library/dn486820\(v=msdn.10\)).

3.  Copiare gli strumenti Gacutil.exe al server.

    Gacutil.exe è reperibile nel **% homedrive %\\Program Files (x86)\\Microsoft SDKs\\Windows\\v8.0A\\bin\\NETFX 4.0 Tools\\** in un computer Windows 8.  È necessario il **gacutil.exe** file stesso, nonché **1033**, **en-US**e l'altra cartella di risorse localizzate riportato di seguito il **NETFX 4.0 Tools** posizione.

4.  Copiare i file del provider (file con estensione DLL di firmato con nome sicuro di una o più) nello stesso percorso di cartella come **gacutil.exe** (il percorso è semplicemente per comodità)

5.  Aggiungere i file con estensione dll nella Global Assembly Cache in ogni server federativo di ADFS nella farm:

    Esempio: utilizzo GACutil.exe strumento da riga di comando per aggiungere una dll alla Global Assembly Cache: `C:\>.\gacutil.exe /if .\<yourdllname>.dll`

    Per visualizzare la voce risultante nella Global Assembly Cache:`C:\>.\gacutil.exe /l <yourassemblyname>`

6.  

### <a name="register-your-provider-in-ad-fs"></a>Registrare il provider in AD FS

Una volta soddisfatti i prerequisiti sopra, aprire una finestra di comando di Windows PowerShell nel server federativo e immettere i comandi seguenti (si noti che se si Usa server farm federativa che usa Database interno di Windows, è necessario eseguire questi comandi di server federativo primario della farm):

1.  `Register-AdfsAuthenticationProvider –TypeName YourTypeName –Name “AnyNameYouWish” [–ConfigurationFilePath (optional)]`

    YourTypeName dove è il nome di tipo sicuro di .NET: "YourDefaultNamespace.YourIAuthenticationAdapterImplementationClassName, Nomeassembly, versione = YourAssemblyVersion, Culture = neutral, PublicKeyToken = YourPublicKeyTokenValue, processorArchitecture = MSIL"

    Questo verrà registrato il provider esterno in AD FS con il nome specificato come AnyNameYouWish precedente.

2.  Riavviare il servizio AD FS (utilizzando lo snap-in servizi di Windows, ad esempio).

3.  Eseguire il comando seguente: `Get-AdfsAuthenticationProvider`.

    Il provider viene illustrato come uno dei provider nel sistema.

    Esempio:

        PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”
        PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter”
        PS C:\>net stop adfssrv
        PS C:\>net start adfssrv

    Se hai il servizio registrazione dispositivo abilitato anche nel proprio ambiente di ADFS, eseguire le operazioni seguenti:  `PS C:\>net start drs`

    Per verificare il provider registrato, usare il comando seguente:`PS C:\>Get-AdfsAuthenticationProvider`.

    Il provider viene illustrato come uno dei provider nel sistema.

### <a name="create-the-ad-fs-authentication-policy-that-invokes-your-adapter"></a>Creare i criteri di autenticazione di AD FS che richiama l'adapter

#### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>Creare i criteri di autenticazione utilizzando lo snap-in Gestione AD FS

1.  Aprire lo snap-in Gestione AD FS (da Server Manager **strumenti** menu).

2.  Fare clic su **i criteri di autenticazione**.

3.  Nel riquadro centrale, sotto **multi-Factor Authentication**, fare clic sui **modificare** collegamento a destra di **impostazioni globali**.

4.  Sotto **selezionare i metodi di autenticazione aggiuntivo** nella parte inferiore della pagina, selezionare la casella per AdminName del provider. Fare clic su **Applica**.

5.  Per fornire un "trigger" per richiamare MFA tramite l'adattatore, sotto **posizioni** controllare entrambi **Extranet** e **Intranet**, ad esempio. Fare clic su **OK**. (Per configurare i trigger per ogni componente di terze parti, vedere "Creare i criteri di autenticazione tramite Windows PowerShell" di seguito.)

6.  Controllare i risultati usando i comandi seguenti:

    Utilizzare innanzitutto `Get-AdfsGlobalAuthenticationPolicy`. Il provider nome dovrebbe essere visualizzato come uno dei valori AdditionalAuthenticationProvider.

    Usare quindi `Get-AdfsAdditionalAuthenticationRule`. Si devono visualizzare le regole per Extranet e Intranet configurati come risultato la selezione dei criteri nell'amministratore dell'interfaccia utente.

#### <a name="create-the-authentication-policy-using-windows-powershell"></a>Creare i criteri di autenticazione tramite Windows PowerShell

1.  Innanzitutto, abilitare il provider nei criteri globali:

    `PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider “YourAuthProviderName”`


~~~
> [!NOTE]
> Note that the value provided for the AdditionalAuthenticationProvider parameter corresponds to the value you provided for the “Name” parameter in the Register-AdfsAuthenticationProvider cmdlet above and to the “Name” property from Get-AdfsAuthenticationProvider cmdlet output. 
> <P></P>


Example:`PS C:\>Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider “MyMFAAdapter”`
~~~

2. Successivamente, configurare le regole globali o della relying party specifica-per attivare autenticazione a più fattori:

   Esempio 1: creare la regola globale per richiedere l'autenticazione MFA per le richieste esterne:`PS C:\>Set-AdfsAdditionalAuthenticationRule –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'`

   Esempio 2: creare MFA regole per richiedere l'autenticazione a più fattori per le richieste esterne a un componente specifico di terze parti.  (Si noti che i singoli provider non può essere connessa a singoli relying party in ADFS in Windows Server 2012 R2).

       PS C:\>$rp = Get-AdfsRelyingPartyTrust –Name <Relying Party Name>
       PS C:\>Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'

### <a name="authenticate-with-mfa-using-your-adapter"></a>Eseguire l'autenticazione con MFA tramite l'adapter

Infine, seguire la procedura seguente per l'adattatore di test:

1.  Verificare che il tipo di autenticazione primaria globale di AD FS è configurato come autenticazione basata su form per Extranet e Intranet (in questo modo la dimostrazione più semplice eseguire l'autenticazione come utente specifico)

    1.  In AD FS snap-in, in **i criteri di autenticazione**, nella **autenticazione primaria** area, fare clic su **modifica** accanto a **impostazioni globali**.

        1.  Oppure fare clic sul **primario** dalla scheda le **criteri di multi-factor** dell'interfaccia utente.

2.  Assicurarsi **autenticazione basata su form** è l'unica opzione è selezionata per la rete Extranet e il metodo di autenticazione Intranet.  Fare clic su **OK**.

3.  Aprire il provider di identità avviato pagina html sign-on (https://\<fsname\>/adfs/ls/idpinitiatedsignon.htm) e accedere come un utente di Active Directory valido nell'ambiente di test.

4.  Immettere le credenziali per l'autenticazione principale.

5.  Dovrebbe essere visualizzata la pagina con le domande di richiesta di esempio i moduli di autenticazione a più fattori. 

    Se si dispone di più di una scheda configurata, si verrà visualizzata la pagina scelta di autenticazione a più fattori con il nome descrittivo in precedenza.

    ![eseguire l'autenticazione con l'adapter](media/ad-fs-build-custom-auth-method/Dn783423.c98d2712-cbd3-4cb9-ac03-2838b81c4f63(MSDN.10).jpg "autenticazione tramite scheda")

    ![eseguire l'autenticazione con l'adapter](media/ad-fs-build-custom-auth-method/Dn783423.fd3aefc0-ef6c-4a8c-a737-4914c78ff2d2(MSDN.10).jpg "autenticazione tramite scheda")

A questo punto si dispone di un'implementazione di lavoro dell'interfaccia e si conosce il funzionamento del modello. È possibile trym come esempio aggiuntivo per impostare punti di interruzione nel BeginAuthentication, nonché il TryEndAuthentication.  Si noti come BeginAuthentication viene eseguito quando l'utente immette innanzitutto il modulo di autenticazione a più fattori, mentre TryEndAuthentication viene attivato a ogni invio del form.

## <a name="update-the-adapter-for-successful-authentication"></a>Aggiornare l'adapter per l'autenticazione ha esito positivo

Ma wait-l'adapter di esempio eseguirà mai correttamente l'autenticazione\!  Questo avviene perché nulla nel codice per TryEndAuthentication restituisce null.

Completando le procedure indicate in precedenza, è stato creato un'implementazione dell'adattatore di base e aggiunto a un server AD FS.  È possibile ottenere la pagina di moduli di autenticazione a più fattori, ma non ancora autenticato perché non si è ancora inserire la logica corretta nell'implementazione TryEndAuthentication.  Quindi, aggiungiamo questo SID.

È importante ricordare l'implementazione TryEndAuthentication:

    public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
     {
     //return new instance of IAdapterPresentationForm derived class
     outgoingClaims = new Claim[0];
     return new MyPresentationForm();

     }

È possibile aggiornarlo in modo che non restituirà sempre MyPresentationForm().  Per questo è possibile creare una semplice utilità metodo all'interno della classe:

    static bool ValidateProofData(IProofData proofData, IAuthenticationContext authContext)
     {
     if (proofData == null || proofData.Properties == null || !proofData.Properties.ContainsKey("ChallengeQuestionAnswer"))
     {
     throw new ExternalAuthenticationException("Error - no answer found", authContext);
     }

     if ((string)proofData.Properties["ChallengeQuestionAnswer"] == "adfabric")
     {
     return true;
     }
     else
     {
     return false;
     }
     }

Aggiornare quindi TryEndAuthentication come indicato di seguito:

    public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
     {
     outgoingClaims = new Claim[0];
     if (ValidateProofData(proofData, authContext))
     {
     //authn complete - return authn method
     outgoingClaims = new[] 
     {
     // Return the required authentication method claim, indicating the particulate authentication method used.
     new Claim( "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", 
     "http://example.com/myauthenticationmethod1" )
     };
     return null;
     }
     else
     {
     //authentication not complete - return new instance of IAdapterPresentationForm derived class
     return new MyPresentationForm();
     }
     }

A questo punto è necessario aggiornare l'adapter nella finestra di test.  È necessario prima di tutto annullare il criterio di AD FS, quindi annullare la registrazione di AD FS e riavviare AD FS, quindi rimuovere il file dll dalla Global Assembly Cache, quindi aggiungere la nuova DLL alla Global Assembly Cache, quindi registrarlo in AD FS, riavviare AD FS e riconfigurare i criteri di ADFS.

## <a name="deploy-and-configure-the-updated-adapter-on-your-test-ad-fs-machine"></a>Distribuire e configurare l'adapter aggiornato al test computer ADFS

### <a name="clear-ad-fs-policy"></a>Cancellare i criteri di AD FS

Cancella MFA tutte le relative caselle di controllo nella UI di MFA, mostrato di seguito, quindi fare clic su OK.

![cancellare i criteri](media/ad-fs-build-custom-auth-method/Dn783423.c111b4e7-5b05-413c-8b0f-222a0e91ac1f(MSDN.10).jpg "cancellare i criteri")

### <a name="unregister-provider-windows-powershell"></a>Annullare la registrazione del provider (Windows PowerShell)

`PS C:\> Unregister-AdfsAuthenticationProvider –Name “YourAuthProviderName”`

Esempio:`PS C:\> Unregister-AdfsAuthenticationProvider –Name “MyMFAAdapter”`

Si noti che il valore passato per "Name" è lo stesso valore "Name" è fornito per il cmdlet Register-AdfsAuthenticationProvider.  È anche la proprietà "Name" che è l'output di Get-AdfsAuthenticationProvider.

Si noti che, prima annullare la registrazione di un provider, è necessario rimuovere il provider da AdfsGlobalAuthenticationPolicy (o deselezionando le caselle di controllo che è selezionata nello snap-in Gestione AD FS con Windows PowerShell.)

Si noti che dopo questa operazione è necessario riavviare il servizio AD FS.

### <a name="remove-assembly-from-gac"></a>Rimuovere assembly dalla Global Assembly Cache

1.  In primo luogo, usare il comando seguente per trovare il nome completo sicuro della voce:`C:\>.\gacutil.exe /l <yourAdapterAssemblyName>`

    Esempio:`C:\>.\gacutil.exe /l mfaadapter`

2.  Quindi, usare il comando seguente per rimuoverlo dalla Global Assembly Cache:`.\gacutil /u “<output from the above command>”`

    Esempio:`C:\>.\gacutil /u “mfaadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

### <a name="add-the-updated-assembly-to-gac"></a>Aggiungere l'assembly aggiornato alla Global Assembly Cache

Assicurarsi di che incollare innanzitutto la DLL aggiornati in locale. `C:\>.\gacutil.exe /if .\MFAAdapter.dll`

### <a name="view-assembly-in-the-gac-cmd-line"></a>Assembly di visualizzazione nella Global Assembly Cache (riga di comando)

`C:\> .\gacutil.exe /l mfaadapter`

### <a name="register-your-provider-in-ad-fs"></a>Registrare il provider in AD FS

1.  `PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.1, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

2.  `PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter1”`

3.  Riavviare il servizio AD FS.

### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>Creare i criteri di autenticazione utilizzando lo snap-in Gestione AD FS

1.  Aprire lo snap-in Gestione AD FS (da Server Manager **strumenti** menu).

2.  Fare clic su **i criteri di autenticazione**.

3.  Sotto **multi-Factor Authentication**, fare clic sui **modificare** collegamento a destra di **impostazioni globali**.

4.  Sotto **selezionare i metodi di autenticazione aggiuntivo**, selezionare la casella per AdminName del provider. Fare clic su **Applica**.

5.  Per fornire un "trigger" per richiamare MFA tramite l'adapter, in posizioni selezionare entrambe **Extranet** e **Intranet**, ad esempio. Fare clic su **OK**.

### <a name="authenticate-with-mfa-using-your-adapter"></a>Eseguire l'autenticazione con MFA tramite l'adapter

Infine, seguire la procedura seguente per l'adattatore di test:

1.  Verificare che il tipo di autenticazione primaria globale di AD FS sia configurato come **autenticazione basata su form** per Extranet e Intranet (questo rende più semplice eseguire l'autenticazione come utente specifico).

    1.  In Gestione di AD FS snap-in, in **i criteri di autenticazione**, nella **autenticazione primaria** area, fare clic su **modifica** accanto a **leimpostazioniglobali**.

        1.  Oppure fare clic sui **primario** scheda dal criterio a più fattori dell'interfaccia utente.

2.  Assicurarsi **autenticazione basata su form** è l'unica opzione è selezionata per entrambe le **Extranet** e il **Intranet** metodo di autenticazione.  Fare clic su **OK**.

3.  Aprire il provider di identità avviato pagina html sign-on (https://\<fsname\>/adfs/ls/idpinitiatedsignon.htm) e accedere come un utente di Active Directory valido nell'ambiente di test.

4.  Immettere le credenziali per l'autenticazione principale.

5.  Dovrebbe essere visualizzata la pagina con il testo di richiesta di esempio i moduli di autenticazione a più fattori.

    1.  Se si dispone di più di una scheda configurata, si verrà visualizzata la pagina scelta di autenticazione a più fattori con il nome descrittivo.

Verrà visualizzato un accesso aggiuntivo ha esito positivo quando si immette "adfabric" pagina di autenticazione MFA.

![accedere con l'adapter](media/ad-fs-build-custom-auth-method/Dn783423.630d8a91-3bfe-4cba-8acf-03eae21530ee(MSDN.10).jpg "accedere con l'adapter")

![accedere con l'adapter](media/ad-fs-build-custom-auth-method/Dn783423.c340fa73-f70f-4870-b8dd-07900fea4469(MSDN.10).jpg "accedere con l'adapter")

## <a name="see-also"></a>Vedere anche

#### <a name="other-resources"></a>Risorse aggiuntive

[Metodi di autenticazione aggiuntivi](https://msdn.microsoft.com/library/dn758113\(v=msdn.10\))  
[Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](https://msdn.microsoft.com/library/dn280949\(v=msdn.10\))

