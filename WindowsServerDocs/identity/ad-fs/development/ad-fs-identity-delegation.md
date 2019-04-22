---
title: Scenario con delega di identità con AD FS
description: Questo scenario descrive un'applicazione che deve accedere alle risorse back-end che richiedono la catena di delega di identità per eseguire controlli di controllo di accesso.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b82d5fd749ac874d09bc54123727aaf902c4d778
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819852"
---
# <a name="identity-delegation-scenario-with-ad-fs"></a>Scenario con delega di identità con AD FS


[A partire da .NET Framework 4.5, Windows Identity Foundation (WIF) è stato completamente integrato in .NET Framework. La versione di Windows Identity Foundation discusso in questo argomento, Windows Identity Foundation 3.5, è deprecata e deve essere usata solo quando lo sviluppo in .NET Framework 3.5 SP1 o .NET Framework 4. Per altre informazioni su WIF in .NET Framework 4.5, anche noto come Windows Identity Foundation 4.5, vedere la documentazione di Windows Identity Foundation nella Guida di sviluppo di .NET Framework 4.5.] 

Questo scenario descrive un'applicazione che deve accedere alle risorse back-end che richiedono la catena di delega di identità per eseguire controlli di controllo di accesso. In genere, una catena di delega di identità semplice è costituito da alle informazioni sul chiamante iniziale e l'identità del chiamante immediato.

Con il modello di delega Kerberos nella piattaforma Windows oggi stesso, le risorse di back-end hanno accesso solo all'identità del chiamante immediato e non a quello del chiamante iniziale. Questo modello è noto come modello di sottosistema attendibile. WIF viene mantenuta l'identità del chiamante iniziale, nonché il chiamante immediato nella catena di delega usando la proprietà dell'attore.

Il diagramma seguente illustra uno scenario di delega di identità tipico in cui un dipendente di Fabrikam accede alle risorse esposte in un'applicazione di Contoso.com.

![identità](media/ad-fs-identity-delegation/id1.png)

Gli utenti fittizi che fanno parte di questo scenario sono:

- Frank: Un dipendente di Fabrikam che vogliono accedere alle risorse di Contoso.
- Daniel: Uno sviluppatore di applicazioni di Contoso che implementa le modifiche necessarie nell'applicazione.
- ADAM: L'amministratore IT di Contoso.

I componenti coinvolti in questo scenario sono:

- web1: Un'applicazione Web con collegamenti alle risorse back-end che richiedono l'identità delegata al chiamante iniziale. Questa applicazione viene compilata con ASP.NET.
- Un servizio Web che accede a un Server SQL, che richiede l'identità del chiamante iniziale, insieme a quello del chiamante immediato delegata. Questo servizio viene compilato con WCF.
- sts1: Un servizio token di sicurezza che si trova nel ruolo di provider di attestazioni e genera le attestazioni previste dall'applicazione (web1). Ha stabilito una relazione di trust con Fabrikam.com e anche con l'applicazione.
- sts2: Un servizio token di sicurezza che si trova nel ruolo di provider di identità per Fabrikam.com e fornisce un endpoint che il dipendente Fabrikam Usa per eseguire l'autenticazione. Relazione di trust con Contoso.com è determinato in modo che i dipendenti di Fabrikam possono accedere alle risorse in Contoso.com.

>[!NOTE] 
>Il termine "Token ActAs", che viene spesso usato in questo scenario, si riferisce a un token emesso dal servizio token di sicurezza e contiene l'identità dell'utente. La proprietà di attore contiene l'identità del servizio token di sicurezza.

Come illustrato nel diagramma precedente, il flusso in questo scenario è:


1. L'applicazione Contoso è configurato per ottenere un token ActAs contenente sia l'identità del dipendente Fabrikam che l'identità del chiamante immediato nella proprietà dell'attore. Daniel ha implementato queste modifiche all'applicazione.
2. L'applicazione Contoso è configurato per passare il token ActAs al servizio back-end. Daniel ha implementato queste modifiche all'applicazione.
3. Il servizio Web di Contoso è configurato per convalidare il token ActAs chiamando sts1. Davide ha abilitato sts1 elaborare le richieste di delega.
4. Utente di Fabrikam Frank accede all'applicazione Contoso e viene concesso l'accesso alle risorse back-end.

## <a name="set-up-the-identity-provider-ip"></a>Configurare il Provider di identità (IP)

Sono disponibili tre opzioni per l'amministratore di Fabrikam.com, Frank:


1. Acquistare e installare un prodotto del servizio token di sicurezza, ad esempio Active Directory® Federation Services (ADFS).
2. Sottoscrivere un prodotto di servizio token di sicurezza cloud come servizio token di sicurezza di Windows Live ID.
3. Creare un servizio token di sicurezza personalizzato mediante WIF.

Per questo scenario di esempio, si presuppone che Frank seleziona opzione1 e viene installato AD FS come IP-STS. Ezio configura inoltre un endpoint, denominato \windowsauth, per autenticare gli utenti. Che fa riferimento alla documentazione del prodotto AD FS e comunica con Adam, l'amministratore IT di Contoso, Frank stabilisce relazioni di trust con il dominio Contoso.com.

## <a name="set-up-the-claims-provider"></a>Configurare il Provider di attestazioni

Le opzioni disponibili per l'amministratore di Contoso.com, Adam, sono uguali a quelli descritti in precedenza per il provider di identità. Per questo scenario di esempio, si presuppone che Adam seleziona l'opzione 1 e viene installato AD FS 2.0 come applicazione relying Party-STS.

## <a name="set-up-trust-with-the-ip-and-application"></a>Impostare relazioni di Trust con l'indirizzo IP e dell'applicazione

Facendo riferimento alla documentazione di AD FS, Adam stabilisce relazioni di trust tra Fabrikam.com e l'applicazione.

## <a name="set-up-delegation"></a>Configurare la delega

ADFS offre l'elaborazione di delega. Facendo riferimento alla documentazione di AD FS, Adam consente l'elaborazione dei token ActAs.

## <a name="application-specific-changes"></a>Modifiche specifiche dell'applicazione

Per aggiungere il supporto per la delega dell'identità a un'applicazione esistente, è necessario effettuare le seguenti modifiche. Daniel viene utilizzato WIF per apportare queste modifiche.


- Memorizzare nella cache il token di bootstrap che web1 ricevuti da sts1.
- Usare CreateChannelActingAs con il token emesso per creare un canale al servizio Web back-end.
- Chiamare il metodo del servizio back-end.

## <a name="cache-the-bootstrap-token"></a>Memorizzare nella cache il Token di Bootstrap

Token di bootstrap è il token iniziale generato dal servizio token di sicurezza e l'applicazione estrae le attestazioni da quest'ultimo. In questo scenario di esempio, questo token viene emesso da sts1 per l'utente Frank e l'applicazione memorizza nella cache. Esempio di codice seguente mostra come recuperare un bootstrap token in un'applicazione ASP.NET:

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
WIF fornisce un metodo, [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx), che consente di creare un canale del tipo specificato che aumenta le richieste di rilascio dei token con token di sicurezza specificato come elemento ActAs. È possibile passare il token di bootstrap per questo metodo e quindi chiamare il metodo di servizio necessario nel canale restituito. In questo scenario di esempio identità di Frank ha il [attore](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.iclaimsidentity.actor.aspx) proprietà è impostata su identità di web1.

Il frammento di codice seguente viene illustrato come chiamare il servizio Web con [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx) e quindi chiamare uno dei metodi del servizio, ComputeResponse, nel canale restituito:

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

Poiché il servizio Web viene compilato con WCF e abilitato per WIF, dopo l'associazione è configurata con IssuedSecurityTokenParameters con l'indirizzo dell'autorità di certificazione corretta, la convalida del ActAs viene gestita automaticamente da WIF. 

Il servizio Web espone i metodi specifici necessari per l'applicazione. Non sono presenti modifiche di codice specifici necessari nel servizio. Esempio di codice seguente illustra la configurazione del servizio Web con IssuedSecurityTokenParameters:

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
