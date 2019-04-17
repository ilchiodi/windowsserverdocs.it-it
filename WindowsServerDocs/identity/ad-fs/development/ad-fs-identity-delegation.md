---
title: "Scenario di delega di identità con AD FS"
description: "Questo scenario descrive un'applicazione che richiede l'accesso a risorse di back-end che richiedono la catena di delega di identità per eseguire controlli di controllo di accesso."
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b82d5fd749ac874d09bc54123727aaf902c4d778
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/28/2018
---
# <a name="identity-delegation-scenario-with-ad-fs"></a>Scenario di delega di identità con AD FS


[A partire da .NET Framework 4.5, Windows Identity Foundation (WIF) è stato completamente integrato in .NET Framework. La versione di WIF discusso in questo argomento, WIF 3.5 è deprecata e deve essere utilizzata solo quando si sviluppa su .NET Framework 3.5 SP1 o .NET Framework 4. Per ulteriori informazioni su WIF in .NET Framework 4.5, noto anche come WIF 4.5, vedere la documentazione di Windows Identity Foundation nella Guida allo sviluppo di .NET Framework 4.5.] 

Questo scenario descrive un'applicazione che richiede l'accesso a risorse di back-end che richiedono la catena di delega di identità per eseguire controlli di controllo di accesso. Le informazioni sul chiamante iniziale e l'identità del chiamante immediato include in genere una catena di delega di identità semplice.

Con il modello di delega Kerberos nella piattaforma Windows oggi, le risorse di back-end hanno accesso solo all'identità del chiamante immediato e non a quello del chiamante iniziale. Questo modello è comunemente noti come modello di sottosistema attendibile. WIF mantiene l'identità del chiamante iniziale, nonché il chiamante immediato nella catena di delega con la proprietà attore.

Il diagramma seguente illustra uno scenario di delega di identità tipici in cui un dipendente di Fabrikam accede a risorse esposte in un'applicazione Contoso.com.

![Identità](media/ad-fs-identity-delegation/id1.png)

Gli utenti fittizi che fanno parte di questo scenario sono:

- Frank: Dipendente di Fabrikam che desiderano accedere alle risorse di Contoso.
- Daniel: Contoso sviluppatore di un'applicazione che implementa le modifiche necessarie nell'applicazione.
- Davide: Contoso amministratore IT.

I componenti coinvolti in questo scenario sono:

- Web1: un'applicazione Web con collegamenti a risorse di back-end che richiedono l'identità del chiamante iniziale delegato. Questa applicazione viene compilata con ASP.NET.
- Un servizio Web che accede a un Server SQL, che richiede l'identità del chiamante iniziale, insieme a quella del chiamante immediato delegato. Questo servizio è stato compilato con WCF.
- STS1: un servizio token di sicurezza nel ruolo di provider di attestazioni, che emette attestazioni previsti per l'applicazione (web1). Che ha stabilito una relazione di trust con Fabrikam.com e anche con l'applicazione.
- sts2: un servizio token di sicurezza che il ruolo di provider di identità per Fabrikam.com e fornisce un punto finale che al dipendente di Fabrikam utilizza per l'autenticazione. Relazione di trust con Contoso.com è stabilito in modo che i dipendenti Fabrikam sono autorizzati ad accedere alle risorse in Contoso.com.

>[!NOTE] 
>Il termine "ActAs token", che viene spesso utilizzato in questo scenario, si riferisce a un token emesso da un servizio token di sicurezza che contiene l'identità dell'utente. La proprietà attore contiene l'identità del servizio token di sicurezza.

Come illustrato nel diagramma precedente, il flusso in questo scenario è:


1. L'applicazione di Contoso è configurato per ottenere un token ActAs che contiene sia identità del dipendente di Fabrikam e l'identità del chiamante immediato nella proprietà attore. Daniel ha implementato queste modifiche all'applicazione.
2. L'applicazione di Contoso è configurato per passare il token ActAs al servizio back-end. Daniel ha implementato queste modifiche all'applicazione.
3. Il servizio Web di Contoso è configurato per convalidare il token ActAs chiamando sts1. Davide è abilitata sts1 elaborare le richieste di delega.
4. Utente Fabrikam Frank accede l'applicazione di Contoso e di accedere alle risorse di back-end.

## <a name="set-up-the-identity-provider-ip"></a>Configurare il Provider di identità (IP)

Sono disponibili tre opzioni disponibili per l'amministratore Fabrikam.com Frank:


1. Acquistare e installare un prodotto servizio token di sicurezza, ad esempio Active Directory® Federation Services (ADFS).
2. Sottoscrivere un prodotto di servizio token di sicurezza cloud, ad esempio LiveID servizio token di sicurezza.
3. Creare un servizio token di sicurezza personalizzato mediante WIF.

Per questo scenario di esempio, partiamo dal presupposto che Frank seleziona l'opzione 1 e installa ADFS come IP-STS. Ezio configura inoltre un punto finale, denominato \windowsauth, per autenticare gli utenti. Per fare riferimento alla documentazione del prodotto AD FS e parlare di Davide, l'amministratore IT di Contoso, Frank stabilisce relazioni di trust con il dominio Contoso.com.

## <a name="set-up-the-claims-provider"></a>Configurare il Provider di attestazioni

Le opzioni disponibili per l'amministratore Contoso.com, Davide, sono uguali a quelli descritti in precedenza per il provider di identità. Per questo scenario di esempio, partiamo dal presupposto che Adam seleziona l'opzione 1 e installa AD FS 2.0 come RP-STS.

## <a name="set-up-trust-with-the-ip-and-application"></a>Configurare la relazione di Trust con l'applicazione e IP

Facendo riferimento alla documentazione di AD FS, Davide stabilisce relazioni di trust tra Fabrikam.com e l'applicazione.

## <a name="set-up-delegation"></a>Configurare la delega

ADFS offre l'elaborazione di delega. Facendo riferimento alla documentazione di AD FS, Davide consente l'elaborazione dei token ActAs.

## <a name="application-specific-changes"></a>Modifiche specifiche dell'applicazione

Per aggiungere il supporto per la delega di identità a un'applicazione esistente, è necessario effettuare le seguenti modifiche. Daniel Usa WIF per apportare queste modifiche.


- Memorizzare nella cache il token di bootstrap tale web1 ricevuti da sts1.
- Utilizzare CreateChannelActingAs con il token emesso per creare un canale al servizio back-end Web.
- Chiama il metodo del servizio back-end.

## <a name="cache-the-bootstrap-token"></a>Memorizzare nella cache il Token di Bootstrap

Il token bootstrap è l'iniziale token rilasciato dal servizio token di sicurezza e l'applicazione estrae le attestazioni da quest'ultimo. In questo scenario di esempio, questo token viene rilasciato da sts1 per l'utente Frank e l'applicazione memorizza nella cache. Esempio di codice seguente mostra come recuperare un bootstrap token in un'applicazione ASP.NET:

```
// Get the Bootstrap Token
SecurityToken bootstrapToken = null;

IClaimsPrincipal claimsPrincipal = Thread.CurrentPrincipal as IClaimsPrincipal;
if ( claimsPrincipal != null )
{
    IClaimsIdentity claimsIdentity = (IClaimsIdentity)claimsPrincipal.Identity;
    bootstrapToken = claimsIdentity.BootstrapToken;
}
```
WIF fornisce un metodo, [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx), che consente di creare un canale del tipo specificato che contribuisce a migliorare le richieste di rilascio di token con il token di sicurezza specificato come un elemento ActAs. È possibile passare il token di bootstrap a questo metodo e quindi chiama il metodo di servizio necessario sul canale restituito. In questo scenario di esempio, identità di Ezio dispone di [attore](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.iclaimsidentity.actor.aspx) proprietà impostata identità del web1.

Il frammento di codice seguente viene illustrata la chiamata al servizio Web con [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx) e quindi chiamare uno dei metodi del servizio, ComputeResponse, sul canale restituito:

```
// Get the channel factory to the backend service from the application state
ChannelFactory<IService2Channel> factory = (ChannelFactory<IService2Channel>)Application[Global.CachedChannelFactory];

// Create and setup channel to talk to the backend service
IService2Channel channel;
lock (factory)
{
// Setup the ActAs to point to the caller's token so that we perform a 
// delegated call to the backend service
// on behalf of the original caller.
    channel = factory.CreateChannelActingAs<IService2Channel>(callerToken);
}

string retval = null;

// Call the backend service and handle the possible exceptions
try
{
    retval = channel.ComputeResponse(value);
    channel.Close();
} catch (Exception exception)
{
    StringBuilder sb = new StringBuilder();
    sb.AppendLine("An unexpected exception occurred.");
    sb.AppendLine(exception.StackTrace);
    channel.Abort();
    retval = sb.ToString();
}

```
## <a name="web-service-specific-changes"></a>Modifiche specifiche del servizio Web

Poiché il servizio Web è integrato con WCF e abilitato per WIF, dopo aver configurato il binding con IssuedSecurityTokenParameters con l'indirizzo dell'autorità di certificazione corretta, la convalida del ActAs viene gestita automaticamente da WIF. 

Il servizio Web espone i metodi specifici necessari per l'applicazione. Non sono presenti modifiche codice specifici necessari nel servizio. Esempio di codice seguente mostra la configurazione del servizio Web con IssuedSecurityTokenParameters:

```
// Configure the issued token parameters with the correct settings
IssuedSecurityTokenParameters itp = new IssuedSecurityTokenParameters( "http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1" );
itp.IssuerMetadataAddress = new EndpointAddress( "http://localhost:6000/STS/mex" );
itp.IssuerAddress = new EndpointAddress( "http://localhost:6000/STS" );

// Create the security binding element
SecurityBindingElement sbe = SecurityBindingElement.CreateIssuedTokenForCertificateBindingElement( itp );
sbe.MessageSecurityVersion = MessageSecurityVersion.WSSecurity11WSTrust13WSSecureConversation13WSSecurityPolicy12BasicSecurityProfile10;

// Create the HTTP transport binding element
HttpTransportBindingElement httpBE = new HttpTransportBindingElement();

// Create the custom binding using the prepared binding elements
CustomBinding binding = new CustomBinding( sbe, httpBE );

using ( ServiceHost host = new ServiceHost( typeof( Service2 ), new Uri( "http://localhost:6002/Service2" ) ) )
{
    host.AddServiceEndpoint( typeof( IService2 ), binding, "" );
    host.Credentials.ServiceCertificate.SetCertificate( "CN=localhost", StoreLocation.LocalMachine, StoreName.My );

// Enable metadata generation via HTTP GET
    ServiceMetadataBehavior smb = new ServiceMetadataBehavior();
    smb.HttpGetEnabled = true;
    host.Description.Behaviors.Add( smb );
    host.AddServiceEndpoint( typeof( IMetadataExchange ), MetadataExchangeBindings.CreateMexHttpBinding(), "mex" );

// Configure the service host to use WIF
    ServiceConfiguration configuration = new ServiceConfiguration();
    configuration.IssuerNameRegistry = new TrustedIssuerNameRegistry();

    FederatedServiceCredentials.ConfigureServiceHost( host, configuration );

    host.Open();

    Console.WriteLine( "Service2 started, press ENTER to stop ..." );
    Console.ReadLine();

    host.Close();
}
```

## <a name="next-steps"></a>Passaggi successivi
[Sviluppare AD FS](../../ad-fs/AD-FS-Development.md)  
