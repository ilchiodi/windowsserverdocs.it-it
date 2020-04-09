---
title: Scenario di delega dell'identità con AD FS
description: Questo scenario descrive un'applicazione che deve accedere alle risorse back-end che richiedono la catena di delega dell'identità per eseguire i controlli di controllo di accesso.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c36de1c3565be7f0f0e6c6203a21345f3d227e96
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857324"
---
# <a name="identity-delegation-scenario-with-ad-fs"></a>Scenario di delega dell'identità con AD FS


[A partire da .NET Framework 4,5, Windows Identity Foundation (WIF) è stato completamente integrato nel .NET Framework. La versione di WIF richiamata da questo argomento, WIF 3,5, è deprecata e deve essere utilizzata solo per lo sviluppo con .NET Framework 3,5 SP1 o con .NET Framework 4. Per ulteriori informazioni su WIF nella .NET Framework 4,5, nota anche come WIF 4,5, vedere la documentazione di Windows Identity Foundation nella Guida allo sviluppo di .NET Framework 4,5. 

Questo scenario descrive un'applicazione che deve accedere alle risorse back-end che richiedono la catena di delega dell'identità per eseguire i controlli di controllo di accesso. Una semplice catena di delega di identità è costituita in genere dalle informazioni sul chiamante iniziale e dall'identità del chiamante immediato.

Con il modello di delega Kerberos attualmente presente nella piattaforma Windows, le risorse back-end possono accedere solo all'identità del chiamante immediato e non a quella del chiamante iniziale. Questo modello è comunemente noto come modello di sottosistema attendibile. WIF mantiene l'identità del chiamante iniziale e il chiamante immediato nella catena di delega usando la proprietà Actor.

Il diagramma seguente illustra uno scenario tipico di delega delle identità in cui un dipendente Fabrikam accede alle risorse esposte in un'applicazione Contoso.com.

![Identity](media/ad-fs-identity-delegation/id1.png)

Gli utenti fittizi che partecipano a questo scenario sono:

- Frank: un dipendente Fabrikam che desidera accedere alle risorse di contoso.
- Daniel: uno sviluppatore di applicazioni Contoso che implementa le modifiche necessarie nell'applicazione.
- Adam: amministratore IT di contoso.

I componenti interessati da questo scenario sono:

- web1: un'applicazione Web con collegamenti alle risorse back-end che richiedono l'identità delegata del chiamante iniziale. Questa applicazione viene compilata con ASP.NET.
- Servizio Web che accede a una SQL Server, che richiede l'identità delegata del chiamante iniziale, insieme al chiamante immediato. Questo servizio è compilato con WCF.
- sts1: un servizio token di servizio che fa parte del ruolo di provider di attestazioni e genera attestazioni previste dall'applicazione (Web1). Ha stabilito una relazione di trust con Fabrikam.com e anche con l'applicazione.
- sts2: servizio STS che è incluso nel ruolo di provider di identità per Fabrikam.com e fornisce un endpoint che il dipendente Fabrikam USA per l'autenticazione. Ha stabilito una relazione di trust con Contoso.com in modo che i dipendenti Fabrikam possano accedere alle risorse in Contoso.com.

>[!NOTE] 
>Il termine "token ActAs", che viene usato spesso in questo scenario, fa riferimento a un token emesso da un STS e contiene l'identità dell'utente. La proprietà Actor contiene l'identità del servizio token di servizio.

Come illustrato nel diagramma precedente, il flusso in questo scenario è:


1. L'applicazione Contoso è configurata in modo da ottenere un token ActAs che contiene l'identità del dipendente Fabrikam e l'identità del chiamante immediato nella proprietà Actor. Daniel ha implementato queste modifiche nell'applicazione.
2. L'applicazione Contoso è configurata per passare il token ActAs al servizio back-end. Daniel ha implementato queste modifiche nell'applicazione.
3. Il servizio Web di Contoso è configurato per convalidare il token ActAs chiamando sts1. Davide ha abilitato sts1 per elaborare le richieste di delega.
4. L'utente di Fabrikam ha accesso all'applicazione Contoso e viene concesso l'accesso alle risorse back-end.

## <a name="set-up-the-identity-provider-ip"></a>Configurare il provider di identità (IP)

Sono disponibili tre opzioni per l'amministratore di Fabrikam.com, Frank:


1. Acquistare e installare un prodotto STS, ad esempio Active Directory&reg; Federation Services (AD FS).
2. Sottoscrivere un prodotto STS cloud come LiveID STS.
3. Creare un STS personalizzato usando WIF.

Per questo scenario di esempio si presuppone che Frank selezioni opzione1 e installa AD FS come IP-STS. Configura inoltre un endpoint, denominato \windowsauth, per autenticare gli utenti. Facendo riferimento alla documentazione del prodotto AD FS e parlando con Adam, l'amministratore IT di Contoso, Frank stabilisce una relazione di trust con il dominio Contoso.com.

## <a name="set-up-the-claims-provider"></a>Configurare il provider di attestazioni

Le opzioni disponibili per l'amministratore di Contoso.com, Adam, sono le stesse descritte in precedenza per il provider di identità. Per questo scenario di esempio si presuppone che Davide selezioni l'opzione 1 e installa AD FS 2,0 come RP-STS.

## <a name="set-up-trust-with-the-ip-and-application"></a>Configurare la relazione di trust con l'IP e l'applicazione

Facendo riferimento alla documentazione di AD FS, Davide stabilisce la relazione di trust tra Fabrikam.com e l'applicazione.

## <a name="set-up-delegation"></a>Configurare la delega

AD FS fornisce l'elaborazione della delega. Facendo riferimento alla documentazione di AD FS, davide consente l'elaborazione dei token ActAs.

## <a name="application-specific-changes"></a>Modifiche specifiche dell'applicazione

Per aggiungere il supporto per la delega dell'identità a un'applicazione esistente, è necessario apportare le modifiche seguenti. Daniel USA WIF per apportare queste modifiche.


- Memorizzare nella cache il token di bootstrap ricevuto da web1 da sts1.
- Usare CreateChannelActingAs con il token emesso per creare un canale per il servizio Web back-end.
- Chiamare il metodo del servizio back-end.

## <a name="cache-the-bootstrap-token"></a>Memorizzare nella cache il token bootstrap

Il token di bootstrap è il token iniziale emesso dal servizio token di servizio e l'applicazione estrae le attestazioni. In questo scenario di esempio, questo token viene emesso da sts1 per l'utente Frank e l'applicazione lo memorizza nella cache. Nell'esempio di codice seguente viene illustrato come recuperare un token bootstrap in un'applicazione ASP.NET:

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
WIF fornisce un metodo, [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx), che crea un canale del tipo specificato che aumenta le richieste di rilascio di token con il token di sicurezza specificato come elemento ActAs. È possibile passare il token di bootstrap a questo metodo e quindi chiamare il metodo del servizio necessario sul canale restituito. In questo scenario di esempio, l'identità di Frank ha la proprietà [Actor](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.iclaimsidentity.actor.aspx) impostata su web1's Identity.

Il frammento di codice seguente mostra come chiamare il servizio Web con [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx) e quindi chiamare uno dei metodi del servizio, ComputeResponse, sul canale restituito:

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

Poiché il servizio Web viene compilato con WCF e abilitato per WIF, una volta che l'associazione viene configurata con IssuedSecurityTokenParameters con l'indirizzo dell'autorità emittente appropriato, la convalida di ActAs viene gestita automaticamente da WIF. 

Il servizio Web espone i metodi specifici richiesti dall'applicazione. Non sono necessarie modifiche al codice specifiche per il servizio. Nell'esempio di codice seguente viene illustrata la configurazione del servizio Web con IssuedSecurityTokenParameters:

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
[Sviluppo di AD FS](../../ad-fs/AD-FS-Development.md)  
