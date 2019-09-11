---
title: Creare un metodo di autenticazione personalizzato per AD FS in Windows Server
description: Questo scenario descrive come compilare un metodo di autenticazione personalizzato per AD FS in Windows Server.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 05/23/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fc71ca2b8d130ab00014f850ccae25e9138d501b
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867564"
---
# <a name="build-a-custom-authentication-method-for-ad-fs-in-windows-server"></a>Creare un metodo di autenticazione personalizzato per AD FS in Windows Server

In questa procedura dettagliata vengono fornite istruzioni per l'implementazione di un metodo di autenticazione personalizzato per AD FS in Windows Server 2012 R2. Per ulteriori informazioni, vedere [metodi di autenticazione aggiuntivi](https://msdn.microsoft.com/library/dn758113\(v=msdn.10\)).


> [!WARNING]
> L'esempio che è possibile compilare qui è&nbsp;esclusivamente a scopo didattico. &nbsp;Queste istruzioni sono relative all'implementazione più semplice e minima possibile per esporre gli elementi necessari del modello.&nbsp; Nessun back-end di autenticazione, elaborazione degli errori o dati di configurazione. 
> <P></P>



## <a name="setting-up-the-development-box"></a>Impostare la casella di sviluppo

Questa procedura dettagliata usa Visual Studio 2012.  Il progetto può essere compilato usando qualsiasi ambiente di sviluppo in grado di creare una classe .NET per Windows. Il progetto deve avere come destinazione .NET 4,5 perché i metodi **BeginAuthentication** e **TryEndAuthentication** usano il tipo **System. Security. Claims. Claim**, parte della .NET Framework versione 4.5. per il progetto è necessario un riferimento:


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Dll di riferimento</strong></p></td>
<td><p><strong>Dove trovarlo</strong></p></td>
<td><p><strong>Obbligatorio per</strong></p></td>
</tr>
<tr class="even">
<td><p>Microsoft. IdentityServer. Web. dll</p></td>
<td><p>La dll si trova in%windir%\ADFS in un server Windows Server 2012 R2 in cui è stato installato AD FS.</p>
<p></p>
<p>Questa dll deve essere copiata nel computer di sviluppo e un riferimento esplicito creato nel progetto.</p></td>
<td><p>Tipi di interfaccia, tra cui IAuthenticationContext, IProofData</p></td>
</tr>
</tbody>
</table>


## <a name="create-the-provider"></a>Creazione del provider

1.  In Visual Studio 2012: Scegliere file-\>nuovo-\>progetto...

2.  Selezionare libreria di classi e assicurarsi di avere come destinazione .NET 4,5.

    ![creazione del provider](media/ad-fs-build-custom-auth-method/Dn783423.71a57ae1-d53d-462b-a846-5b3c02c7d3f2(MSDN.10).jpg "creazione del provider")

3.  Creare una copia di **Microsoft. IdentityServer. Web. dll** da% windir%\\ADFS nel server Windows Server 2012 R2 in cui è stato installato ad FS e incollarlo nella cartella del progetto nel computer di sviluppo.

4.  In **Esplora soluzioni**fare clic con il pulsante destro del mouse su **riferimenti** e **Aggiungi riferimento...**

5.  Passare alla copia locale di **Microsoft. IdentityServer. Web. dll** e **aggiungere...**

6.  Fare clic su **OK** per confermare il nuovo riferimento:

    ![creazione del provider](media/ad-fs-build-custom-auth-method/Dn783423.f18df353-9259-4744-b4b6-dd780ce90951(MSDN.10).jpg "creazione del provider")

    A questo punto è necessario configurare per risolvere tutti i tipi necessari per il provider. 

7.  Aggiungere una nuova classe al progetto (fare clic con il pulsante destro del mouse sul progetto, quindi su **Aggiungi... Classe...** ) e assegnargli un nome come l' **adattatore**, illustrato di seguito:

    ![creazione del provider](media/ad-fs-build-custom-auth-method/Dn783423.6b6a7a8b-9d66-40c7-8a86-a2e3b9e14d09(MSDN.10).jpg "creazione del provider")

8.  Nel nuovo file MyAdapter.cs sostituire il codice esistente con il codice seguente:

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

    A questo punto dovrebbe essere possibile fare clic con il pulsante destro del mouse su Vai a definizione in IAuthenticationAdapter per visualizzare il set di membri di interfaccia necessari. 

    Successivamente, è possibile eseguire una semplice implementazione di questi.

9.  Sostituire l'intero contenuto della classe con il codice seguente:

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

10. Non è ancora possibile compilare... Ci sono altre due interfacce da usare.

    Aggiungere altre due classi al progetto: una per i metadati e l'altra per il form di presentazione.  È possibile aggiungerli nello stesso file della classe precedente.

        class MyMetadata : IAuthenticationAdapterMetadata
         {

         }

         class MyPresentationForm : IAdapterPresentationForm
         {

         }

11. Successivamente, è possibile aggiungere i membri necessari per ogni. Prima di tutto, i metadati (con utili commenti in linea)

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

    Successivamente, il form di presentazione:

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

12. Si noti il ' todo ' per l'elemento **Resources. FormPageHtml** sopra. 

   È possibile risolvere il problema in un minuto, ma prima di tutto aggiungere le istruzioni return obbligatorie finali, in base ai nuovi tipi implementati, alla classe dell'adattatore iniziale.  A tale scopo, aggiungere gli elementi in *corsivo* sotto all'implementazione di IAuthenticationAdapter esistente:

       Scheda classe: IAuthenticationAdapter {public IAuthenticationAdapterMetadata Metadata {//Get {Return New <instance of IAuthenticationAdapterMetadata derived class>;}     Get {restituire nuovi metadati ();}     }

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

13. Ora per il file di risorse contenente il frammento HTML. Creare un nuovo file di testo nella cartella del progetto con il contenuto seguente:

       <div id="loginArea">
        <form method="post" id="loginForm" >
        <!-- These inputs are required by the presentation framework. Do not modify or remove -->
        <input id="authMethod" type="hidden" name="AuthMethod" value="%AuthMethod%"/>
        <input id="context" type="hidden" name="Context" value="%Context%"/>
        <!-- End inputs are required by the presentation framework. -->
        <p id="pageIntroductionText">Questo contenuto viene fornito dall'adapter di esempio per l'autenticazione a più fattori. Gli input di richiesta devono essere presentati di seguito.</p>
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

14. Selezionare **quindi progetto-\>Aggiungi componente... File** di risorse e denominare le **risorse**del file e fare clic su **Aggiungi:**

   ![creazione del provider](media/ad-fs-build-custom-auth-method/Dn783423.3369ad8f-f65f-4f36-a6d5-6a3edbc1911a(MSDN.10).jpg "creazione del provider")

15. Quindi, nel file **Resources. resx** , scegliere **Aggiungi risorsa. Aggiungere un file esistente**.  Passare al file di testo (contenente il frammento HTML) salvato in precedenza.

   Verificare che il codice GetFormHtml risolva correttamente il nome della nuova risorsa dal prefisso del nome del file di risorse (file con estensione resx) seguito dal nome della risorsa stessa:

       public String GetFormHtml (int LCID) {String htmlTemplate = resources. MfaFormHtml;//resxFilename.ResourceName return htmlTemplate;    }

   A questo punto dovrebbe essere possibile compilare.

## <a name="build-the-adapter"></a>Compilare l'adapter

L'adapter deve essere incorporato in un assembly .NET con nome sicuro che può essere installato nella GAC di Windows.  Per ottenere questo risultato in un progetto di Visual Studio, completare i passaggi seguenti:

1.  Fare clic con il pulsante destro del mouse sul nome del progetto in Esplora soluzioni e scegliere **Proprietà**.

2.  Nella scheda **firma** selezionare **Firma assembly** e scegliere **\<nuovo... in\>** **scegliere un file di chiave con nome sicuro:**  Immettere un nome e una password per il file di chiave e fare clic su **OK**.  Assicurarsi quindi che **Firma assembly** sia selezionato e che **solo la firma ritardata** sia deselezionata.  La pagina di **firma** delle proprietà dovrebbe essere simile alla seguente:

    ![compilazione del provider](media/ad-fs-build-custom-auth-method/Dn783423.0b1a1db2-d64e-4bb8-8c01-ef34296a2668(MSDN.10).jpg "compilazione del provider")

3.  Quindi compilare la soluzione.

## <a name="deploy-the-adapter-to-your-ad-fs-test-machine"></a>Distribuire l'adapter nel computer AD FS test

Prima che un provider esterno possa essere richiamato da AD FS, deve essere registrato nel sistema.  I provider di adapter devono fornire un programma di installazione che esegua le azioni di installazione necessarie, inclusa l'installazione nella GAC, e il programma di installazione deve supportare la registrazione in AD FS.  Se questa operazione non viene eseguita, l'amministratore deve eseguire la procedura di Windows PowerShell riportata di seguito.  Questi passaggi possono essere usati nel Lab per abilitare il test e il debug.

### <a name="prepare-the-test-ad-fs-machine"></a>Preparare il computer di AD FS di test

Copiare i file e aggiungerli alla GAC.

1.  Assicurarsi di disporre di un computer Windows Server 2012 R2 o di una macchina virtuale.

2.  Installare il servizio ruolo AD FS e configurare una farm con almeno un nodo.

    Per informazioni dettagliate sulla procedura di configurazione di un server federativo in un ambiente lab, vedere la [Guida alla distribuzione di Windows server 2012 R2 ad FS](https://msdn.microsoft.com/library/dn486820\(v=msdn.10\)).

3.  Copiare gli strumenti di Gacutil. exe nel server.

    Gacutil. exe si trova in **\\% HOMEDRIVE% Program Files (x86)\\Microsoft SDK\\Windows\\v 8.0 a\\bin\\NetFx 4,0 Tools\\**  in un computer Windows 8.  Sono necessari il file **gacutil. exe** e l' **1033**, **en-US**e l'altra cartella delle risorse localizzata sotto il percorso **NetFx 4,0 Tools** .

4.  Copiare i file del provider (uno o più file con estensione dll firmati con nome sicuro) nello stesso percorso di cartella di **gacutil. exe** (la località è solo per praticità)

5.  Aggiungere i file con estensione dll alla GAC in ogni AD FS server federativo della farm:

    Esempio: utilizzo dello strumento da riga di comando GACutil. exe per aggiungere una dll alla GAC:`C:\>.\gacutil.exe /if .\<yourdllname>.dll`

    Per visualizzare la voce risultante nella GAC:`C:\>.\gacutil.exe /l <yourassemblyname>`

6.  

### <a name="register-your-provider-in-ad-fs"></a>Registrare il provider in AD FS

Una volta soddisfatti i prerequisiti precedenti, aprire una finestra di comando di Windows PowerShell nel server federativo e immettere i comandi seguenti. si noti che se si usa server farm di federazione che usa il database interno di Windows, è necessario eseguire questi comandi nel server federativo primario della farm:

1.  `Register-AdfsAuthenticationProvider –TypeName YourTypeName –Name “AnyNameYouWish” [–ConfigurationFilePath (optional)]`

    Dove YourTypeName è il nome del tipo sicuro .NET: "YourDefaultNamespace. YourIAuthenticationAdapterImplementationClassName, YourAssemblyName, Version = YourAssemblyVersion, Culture = neutral, PublicKeyToken = YourPublicKeyTokenValue, processorArchitecture = MSIL"

    Questa operazione registrerà il provider esterno in AD FS, con il nome specificato come AnyNameYouWish.

2.  Riavviare il servizio AD FS (ad esempio, utilizzando lo snap-in Servizi Windows).

3.  Eseguire il comando seguente: `Get-AdfsAuthenticationProvider`.

    Il provider verrà visualizzato come uno dei provider del sistema.

    Esempio:

        PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”
        PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter”
        PS C:\>net stop adfssrv
        PS C:\>net start adfssrv

    Se il servizio Registrazione dispositivi è abilitato nell'ambiente AD FS, eseguire anche le operazioni seguenti:`PS C:\>net start drs`

    Per verificare il provider registrato, utilizzare il comando seguente:`PS C:\>Get-AdfsAuthenticationProvider`.

    Il provider verrà visualizzato come uno dei provider del sistema.

### <a name="create-the-ad-fs-authentication-policy-that-invokes-your-adapter"></a>Creare i criteri di autenticazione AD FS che richiamano l'adapter

#### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>Creare i criteri di autenticazione tramite lo snap-in gestione AD FS

1.  Aprire lo snap-in gestione AD FS (dal menu **strumenti** Server Manager).

2.  Fare clic su **criteri di autenticazione**.

3.  Nel riquadro centrale, in **multi-factor authentication**, fare clic sul collegamento **modifica** a destra di **Impostazioni globali**.

4.  In **Seleziona metodi di autenticazione aggiuntivi** nella parte inferiore della pagina, selezionare la casella per l'amministratore del provider. Fare clic su **Applica**.

5.  Per fornire un "trigger" per richiamare l'autenticazione a più fattori usando l'adapter, in **percorsi** controllare sia **Extranet** che **Intranet**, ad esempio. Fare clic su **OK**. Per configurare i trigger per ogni relying party, vedere "creare i criteri di autenticazione con Windows PowerShell" di seguito.

6.  Controllare i risultati usando i comandi seguenti:

    Primo utilizzo `Get-AdfsGlobalAuthenticationPolicy`. Il nome del provider verrà visualizzato come uno dei valori AdditionalAuthenticationProvider.

    Usare `Get-AdfsAdditionalAuthenticationRule`quindi. Verranno visualizzate le regole per Extranet e Intranet configurate in seguito alla selezione dei criteri nell'interfaccia utente dell'amministratore.

#### <a name="create-the-authentication-policy-using-windows-powershell"></a>Creare i criteri di autenticazione con Windows PowerShell

1.  Per prima cosa, abilitare il provider in criteri globali:

    `PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider “YourAuthProviderName”`


~~~
> [!NOTE]
> Note that the value provided for the AdditionalAuthenticationProvider parameter corresponds to the value you provided for the “Name” parameter in the Register-AdfsAuthenticationProvider cmdlet above and to the “Name” property from Get-AdfsAuthenticationProvider cmdlet output. 
> <P></P>


Example:`PS C:\>Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider “MyMFAAdapter”`
~~~

2. Configurare quindi regole globali o specifiche delle entità per attivare l'autenticazione a più fattori:

   Esempio 1: creare una regola globale per richiedere l'autenticazione a più fattori per le richieste esterne:`PS C:\>Set-AdfsAdditionalAuthenticationRule –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'`

   Esempio 2: creare regole di autenticazione a più fattori per richiedere l'autenticazione a più fattori per le richieste esterne a una specifica relying party.  Si noti che i singoli provider non possono essere connessi a singole relying party in AD FS in Windows Server 2012 R2).

       PS C:\>$rp = Get-AdfsRelyingPartyTrust –Name <Relying Party Name>
       PS C:\>Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'

### <a name="authenticate-with-mfa-using-your-adapter"></a>Eseguire l'autenticazione con l'autenticazione a più fattori usando l'adapter

Infine, attenersi alla procedura seguente per testare l'adapter:

1.  Verificare che il AD FS tipo di autenticazione principale globale sia configurato come autenticazione basata su form per Extranet e Intranet. in questo modo la demo diventa più semplice da autenticare come utente specifico.

    1.  Nello snap-in AD FS, in **criteri di autenticazione**, nell'area **autenticazione primaria** , fare clic su **modifica** accanto a **Impostazioni globali**.

        1.  In alternativa, è sufficiente fare clic sulla scheda **primario** dall'interfaccia utente di criteri a più **fattori** .

2.  Verificare che l' **autenticazione basata su form** sia l'unica opzione selezionata per il metodo di autenticazione Extranet e Intranet.  Fare clic su **OK**.

3.  Aprire la pagina HTML di accesso IDP avviata (https://\<FSName\>/ADFS/LS/idpinitiatedsignon.htm) e accedere come un utente di Active Directory valido nell'ambiente di test.

4.  Immettere le credenziali per l'autenticazione principale.

5.  Verrà visualizzata la pagina dei moduli di autenticazione a più fattori con domande di esempio di verifica. 

    Se è stata configurata più di una scheda, viene visualizzata la pagina scelta autenticazione a più fattori con il nome descrittivo riportato sopra.

    ![eseguire l'autenticazione con l'adapter](media/ad-fs-build-custom-auth-method/Dn783423.c98d2712-cbd3-4cb9-ac03-2838b81c4f63(MSDN.10).jpg "eseguire l'autenticazione con l'adapter")

    ![eseguire l'autenticazione con l'adapter](media/ad-fs-build-custom-auth-method/Dn783423.fd3aefc0-ef6c-4a8c-a737-4914c78ff2d2(MSDN.10).jpg "eseguire l'autenticazione con l'adapter")

A questo punto si dispone di un'implementazione funzionante dell'interfaccia e si conoscono le modalità di funzionamento del modello. È possibile Trym come esempio aggiuntivo per impostare i punti di interruzioni in BeginAuthentication e TryEndAuthentication.  Si noti come BeginAuthentication viene eseguito quando l'utente immette il modulo di autenticazione a più fattori, mentre TryEndAuthentication viene attivato a ogni invio del modulo.

## <a name="update-the-adapter-for-successful-authentication"></a>Aggiornare l'adapter per l'autenticazione riuscita

Ma attendi: l'adapter di esempio non eseguirà mai l'autenticazione\!  Questo perché nulla nel codice restituisce null per TryEndAuthentication.

Completando le procedure descritte in precedenza, è stata creata un'implementazione di base dell'adapter che è stata aggiunta a un server AD FS.  È possibile ottenere la pagina dei moduli di autenticazione a più fattori, ma non è ancora possibile eseguire l'autenticazione perché non è stata ancora inserita la logica corretta nell'implementazione di TryEndAuthentication.  Quindi, aggiungiamo questo.

Richiamare l'implementazione di TryEndAuthentication:

    public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
     {
     //return new instance of IAdapterPresentationForm derived class
     outgoingClaims = new Claim[0];
     return new MyPresentationForm();

     }

Ora verrà aggiornata in modo che non restituisca sempre MyPresentationForm ().  A tale proposito è possibile creare un semplice metodo di utilità all'interno della classe:

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

A questo punto è necessario aggiornare la scheda nella casella di test.  È necessario innanzitutto annullare i criteri di AD FS e quindi annullare la registrazione da AD FS e riavviare AD FS, quindi rimuovere il file dll dalla GAC, quindi aggiungere la nuova dll alla GAC, quindi registrarla in AD FS, riavviare AD FS e riconfigurare i criteri di AD FS.

## <a name="deploy-and-configure-the-updated-adapter-on-your-test-ad-fs-machine"></a>Distribuire e configurare l'adapter aggiornato nel computer di AD FS di test

### <a name="clear-ad-fs-policy"></a>Cancella AD FS criteri

Deselezionare tutte le caselle di controllo relative a autenticazione a più fattori nell'interfaccia utente dell'autenticazione a più fattori, mostrata di seguito, quindi

![Cancella criteri](media/ad-fs-build-custom-auth-method/Dn783423.c111b4e7-5b05-413c-8b0f-222a0e91ac1f(MSDN.10).jpg "Cancella criteri")

### <a name="unregister-provider-windows-powershell"></a>Annulla registrazione provider (Windows PowerShell)

`PS C:\> Unregister-AdfsAuthenticationProvider –Name “YourAuthProviderName”`

Esempio`PS C:\> Unregister-AdfsAuthenticationProvider –Name “MyMFAAdapter”`

Si noti che il valore passato per "Name" corrisponde al valore "Name" specificato per il cmdlet Register-AdfsAuthenticationProvider.  È anche la proprietà "Name" che viene restituita da Get-AdfsAuthenticationProvider.

Si noti che prima di annullare la registrazione di un provider, è necessario rimuovere il provider dal AdfsGlobalAuthenticationPolicy (deselezionando le caselle di controllo controllate nello snap-in di gestione AD FS o usando Windows PowerShell).

Si noti che il servizio di AD FS deve essere riavviato dopo questa operazione.

### <a name="remove-assembly-from-gac"></a>Rimuovi assembly dalla GAC

1.  Usare prima di tutto il comando seguente per trovare il nome sicuro completo della voce:`C:\>.\gacutil.exe /l <yourAdapterAssemblyName>`

    Esempio`C:\>.\gacutil.exe /l mfaadapter`

2.  Usare quindi il comando seguente per rimuoverlo dalla GAC:`.\gacutil /u “<output from the above command>”`

    Esempio`C:\>.\gacutil /u “mfaadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

### <a name="add-the-updated-assembly-to-gac"></a>Aggiungere l'assembly aggiornato alla GAC

Prima di tutto, assicurarsi di incollare il file dll aggiornato localmente. `C:\>.\gacutil.exe /if .\MFAAdapter.dll`

### <a name="view-assembly-in-the-gac-cmd-line"></a>Visualizza assembly nella GAC (riga di comando)

`C:\> .\gacutil.exe /l mfaadapter`

### <a name="register-your-provider-in-ad-fs"></a>Registrare il provider in AD FS

1.  `PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.1, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

2.  `PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter1”`

3.  Riavviare il servizio AD FS.

### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>Creare i criteri di autenticazione tramite lo snap-in gestione AD FS

1.  Aprire lo snap-in gestione AD FS (dal menu **strumenti** Server Manager).

2.  Fare clic su **criteri di autenticazione**.

3.  In **multi-factor authentication**fare clic sul collegamento **modifica** a destra di **Impostazioni globali**.

4.  In **Seleziona metodi di autenticazione aggiuntivi**, selezionare la casella per l'amministratore del provider. Fare clic su **Applica**.

5.  Per fornire un "trigger" per richiamare l'autenticazione a più fattori usando l'adapter, in percorsi controllare sia **Extranet** che **Intranet**, ad esempio. Fare clic su **OK**.

### <a name="authenticate-with-mfa-using-your-adapter"></a>Eseguire l'autenticazione con l'autenticazione a più fattori usando l'adapter

Infine, attenersi alla procedura seguente per testare l'adapter:

1.  Verificare che il AD FS tipo di autenticazione primario globale sia configurato come **autenticazione basata su form** per Extranet e Intranet (questo semplifica l'autenticazione come utente specifico).

    1.  Nello snap-in gestione AD FS, in **criteri di autenticazione**, nell'area **autenticazione primaria** , fare clic su **modifica** accanto a **Impostazioni globali**.

        1.  In alternativa, è sufficiente fare clic sulla scheda **primario** dall'interfaccia utente di criteri a più fattori.

2.  Verificare che l' **autenticazione basata su form** sia l'unica opzione selezionata per il metodo di autenticazione **Extranet** e **Intranet** .  Fare clic su **OK**.

3.  Aprire la pagina HTML di accesso IDP avviata (https://\<FSName\>/ADFS/LS/idpinitiatedsignon.htm) e accedere come un utente di Active Directory valido nell'ambiente di test.

4.  Immettere le credenziali per l'autenticazione principale.

5.  Verrà visualizzata la pagina dei moduli di autenticazione a più fattori con testo di esempio di verifica.

    1.  Se è configurata più di una scheda, viene visualizzata la pagina scelta di autenticazione a più fattori con il nome descrittivo.

Quando si immette "adfabric" nella pagina autenticazione a più fattori, verrà visualizzato un accesso riuscito.

![Accedi con adapter](media/ad-fs-build-custom-auth-method/Dn783423.630d8a91-3bfe-4cba-8acf-03eae21530ee(MSDN.10).jpg "Accedi con adapter")

![Accedi con adapter](media/ad-fs-build-custom-auth-method/Dn783423.c340fa73-f70f-4870-b8dd-07900fea4469(MSDN.10).jpg "Accedi con adapter")

## <a name="see-also"></a>Vedere anche

#### <a name="other-resources"></a>Risorse aggiuntive

[Metodi di autenticazione aggiuntivi](https://msdn.microsoft.com/library/dn758113\(v=msdn.10\))  
[Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni con esigenze particolari](https://msdn.microsoft.com/library/dn280949\(v=msdn.10\))

