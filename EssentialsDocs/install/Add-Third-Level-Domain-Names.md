---
title: Aggiungere nomi di dominio di terzo livello
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5b4a362-1881-4024-ae4e-cc3b05e50103
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 64bf24e45155fdd981e2061b3de7ebce1c53b36c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="add-third-level-domain-names"></a>Aggiungere nomi di dominio di terzo livello

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile aggiungere la possibilità per gli utenti di richiedere nomi di dominio di terzo livello nella configurazione guidata nome dominio. Farlo creando e installando un assembly di codice che viene utilizzato da Gestione dominio nel sistema operativo.  
  
## <a name="create-a-provider-of-third-level-domain-names"></a>Creare un provider di nomi di dominio di terzo livello  
 È possibile rendere disponibili nomi di dominio di terzo livello creando e installando un assembly di codice che fornisce i nomi di dominio per la procedura guidata. A tale scopo, completare le attività seguenti:  
  
-   [Aggiunta di un'implementazione dell'interfaccia IDomainSignupProvider all'assembly](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup)  
  
-   [Aggiunta di un'implementazione dell'interfaccia IDomainMaintenanceProvider all'assembly](Add-Third-Level-Domain-Names.md#BKMK_DomainMaintenance)  
  
-   [Una firma Authenticode all'assembly](Add-Third-Level-Domain-Names.md#BKMK_SignAssembly)  
  
-   [Installare l'assembly nel computer di riferimento](Add-Third-Level-Domain-Names.md#BKMK_InstallAssembly)  
  
-   [Riavviare il servizio di gestione nome di dominio di Windows Server](Add-Third-Level-Domain-Names.md#BKMK_RestartService)  
  
###  <a name="BKMK_DomainSignup"></a>Aggiunta di un'implementazione dell'interfaccia IDomainSignupProvider all'assembly  
 L'interfaccia IDomainSignupProvider viene utilizzato per aggiungere offerte di domini alla procedura guidata.  
  
##### <a name="to-add-the-idomainsignupprovider-code-to-the-assembly"></a>Per aggiungere il codice IDomainSignupProvider all'assembly  
  
1.  Aprire Visual Studio 2008 come amministratore facendo clic con il programma di **Start** menu e selezionando **Esegui come amministratore**.  
  
2.  Fare clic su **File**, fare clic su **New**, quindi fare clic su **progetto**.  
  
3.  Nel **nuovo progetto** la finestra di dialogo, fare clic su **Visual c#**, fare clic su **libreria di classi**, immettere un nome per la soluzione e quindi fare clic su **OK**.  
  
4.  Rinominare il file Class1.cs. Ad esempio, MyDomainNameProvider.cs  
  
5.  Aggiungi i riferimenti ai file Wssg.Web.DomainManagerObjectModel.dll, CertManaged.dll, WssgCertMgmt.dll e WssgCommon.dll.  
  
6.  Aggiungere le seguenti istruzioni using.  
  
    ```c#  
  
    using System.Collections.ObjectModel;  
    using System.Net;  
    using System.Net.Sockets;  
    using Microsoft.WindowsServerSolutions.Certificates;  
    using Microsoft.WindowsServerSolutions.CertificateManagement;  
    using Microsoft.WindowsServerSolutions.Common;  
    using Microsoft.Win32;  
    ```  
  
7.  Modificare lo spazio dei nomi e l'intestazione della classe in base all'esempio seguente.  
  
    ```c#  
    namespace Microsoft.WindowsServerSolutions.RemoteAccess.Domains  
    {  
        public class MyDomainNameProvider : IDomainSignupProvider  
        {  
        }  
    }  
    ```  
  
8.  Aggiungere il metodo Initialize e le variabili richieste alla classe che definisce le offerte presentate nella procedura guidata.  
  
    > [!NOTE]
    >  Il metodo Initialize definisce un identificatore per il provider di dominio che deve essere univoco. Un metodo tipico per eseguire questa operazione consiste nel definire un GUID come identificatore. Per ulteriori informazioni sulla creazione di un GUID, vedere [crea Guid (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098).  
  
     Esempio di codice seguente mostra il metodo Initialize.  
  
    ```c#  
  
    static readonly Guid MyID = new Guid("8C999DF5-696A-47af-822D-94F1673D3397");  
    public Guid ID { get { return MyID; } }  
    public string Name { get { return "My Provider"; } }  
    List<Offering> offerings = new List<Offering>();  
  
    public void Initialize(DomainProviderSettings config)  
    {  
       var offer1 = new Offering()  
       {  
          Description = "My Domain Provider",  
          Name = "Offering 1",  
          ProviderID = ID,  
          MoreInfoUrl = new Uri("http://www.contoso.com"),  
          MembershipServiceName = "My Membership",  
          EulaUrl = new Uri("http://www.contoso.com"),  
       };  
  
       this.offerings.Add(offer1);  
       RegistryKey key =   
          Registry.LocalMachine.CreateSubKey(@"Software\Microsoft\Windows Server\Domain Manager\Settings");  
       key.Close();  
    }  
    ```  
  
9. Aggiungere il metodo GetOfferings, che restituisce l'elenco di offerte inizializzato nel passaggio precedente. Esempio di codice seguente mostra il metodo GetOfferings.  
  
    ```c#  
  
    public ReadOnlyCollection<Offering> GetOfferings()  
    {  
       return this.offerings.AsReadOnly();  
    }  
    ```  
  
10. Aggiungere il metodo FindOfferingForDomain, che restituisce l'offerta dall'elenco. Esempio di codice seguente mostra il metodo FindOfferingForDomain.  
  
    ```  
  
    public Offering FindOfferingForDomain(string domain)  
    {  
       // Return the offering that has the domain name.  
       return offerings[0];  
    }  
  
    ```  
  
11. Aggiungere il metodo SetCredentials, che definisce le credenziali necessarie per accedere alle offerte. Esempio di codice seguente mostra il metodo SetCredentials.  
  
    ```c#  
  
    private string currentUser { get; set; }  
    private string currentPassword { get; set; }  
  
    public bool SetCredentials(DomainNameRequest request,   
       DomainProviderCredentials credentials, bool validate)  
    {  
       currentUser = credentials.UserName;  
       currentPassword = credentials.Password;  
       if (validate)  
       {  
          return ValidateCredentials();  
       }  
  
       return true;  
    }  
    ```  
  
12. Aggiungere il metodo ValidateCredentials, che convalida le credenziali definite da SetCredentials. Esempio di codice seguente mostra il metodo ValidateCredentials.  
  
    ```c#  
  
    public static readonly string offerUser = "user1@contoso.com";  
    public static readonly string offerPassword = "password1";  
  
    public bool ValidateCredentials()  
    {  
       if (IsUser())  
          return string.Equals(currentPassword, offerPassword);  
       else  
          return false;  
    }   
  
    private bool IsUser()  
    {  
       return string.Equals(currentUser, offerUser, StringComparison.OrdinalIgnoreCase);  
    }  
    ```  
  
13. Aggiungere il metodo GetAvailableDomainRoots, che restituisce l'elenco di nomi di dominio radice supportati dall'offerta specificata nella richiesta. Questo elenco di nomi di dominio radice non deve essere vuoto. Esempio di codice seguente mostra il metodo GetAvailableDomainRoots.  
  
    ```c#  
  
    public ReadOnlyCollection<string> GetAvailableDomainRoots(DomainNameRequest request)  
    {  
       List<string> list = new List<string>();  
       list.Add("domain1.com");  
       list.Add("domain1.org");  
  
       return list.AsReadOnly();  
    }  
    ```  
  
14. Aggiungere il metodo GetUserDomainNames, che restituisce un elenco di nomi di dominio che appartiene l'utente corrente già, relazione all'offerta corrente. Questo elenco può essere vuoto. Esempio di codice seguente mostra il metodo GetUserDomainNames.  
  
    ```c#  
  
    public static readonly string AvailableDomain1 = "available.domain1.com",  
       AvailableDomain2 = "available.domain2.com";  
    public static readonly string OccupiedDomain1 = "occupied.domain1.com",  
       OccupiedDomain2 = "occupied.domain2.com";  
  
    public ReadOnlyCollection<string> GetUserDomainNames(DomainNameRequest request)  
    {  
       var userDomains = new List<string>();  
       userDomains.Add(OccupiedDomain1);  
       userDomains.Add(AvailableDomain1);  
  
       return userDomains.AsReadOnly();  
    }  
    ```  
  
15. Aggiungere il metodo GetUserDomainQuota, che restituisce il numero massimo di domini che consente di offerta specificata. Se non è applicabile un massimo, questo metodo deve restituire 0. L'esempio seguente mostra il metodo GetUserDomainQuota.  
  
    ```c#  
  
    public int GetUserDomainQuota(DomainNameRequest request)  
    {  
       return 0;  
    }  
    ```  
  
16. Aggiungere il metodo CheckDomainAvailability, che verifica la disponibilità del nome di dominio richiesto e può restituire un elenco di suggerimenti. Esempio di codice seguente mostra il metodo CheckDomainAvailability.  
  
    ```c#  
  
    public bool CheckDomainAvailability(DomainNameRequest request,   
       out ReadOnlyCollection<string> suggestions)  
    {  
       suggestions = null;  
  
       return true;  
    }  
    ```  
  
17. Aggiungere il metodo CommitDomain, che conferma il nome di dominio richiesto. Completamento di questo metodo implica che il nome di dominio è associato l'account utente, il provider di manutenzione verrà chiamato immediatamente per recuperare il certificato se lo stato è completamente operativo e il provider e dell'offerta. Esempio di codice seguente mostra il metodo CommitDomain.  
  
    ```c#  
  
    public DomainStatus CommitDomain(DomainNameRequest request)  
    {              
       ReadOnlyCollection<string> suggestions;  
       if (!CheckDomainAvailability(request, out suggestions))  
       {  
          throw new DomainException(FailureReason.InvalidDomainName, null, null);  
       }  
  
       return DomainStatus.Ready;  
    }  
    ```  
  
18. Aggiungere il metodo ReleaseDomain, che informa il provider che l'utente vuole rilasciare il nome di dominio. Esempio di codice seguente mostra il metodo ReleaseDomain.  
  
    ```c#  
  
    public bool ReleaseDomain(DomainNameRequest request)  
    {  
       return true;  
    }  
    ```  
  
19. Aggiungere il metodo GetProviderLandingUrl, che restituisce l'URL per la pagina di destinazione nel flusso di lavoro dominio. Esempio di codice seguente mostra il metodo GetProviderLandingUrl.  
  
    ```c#  
  
    public Url GetProviderLandingUrl(DomainNameRequest request)  
    {  
       return new Url("www.contoso.com");  
    }  
    ```  
  
20. Aggiungere il metodo GetDomainMaintenanceProvider, che restituisce un'istanza di IDomainMaintenanceProvider utilizzata per attività di manutenzione del dominio. Questo metodo viene chiamato dopo il completamento del metodo CommitDomain e all'avvio il gestore di dominio. Esempio di codice seguente mostra il metodo GetDomainMaintenanceProvider.  
  
    ```c#  
  
    public IDomainMaintenanceProvider GetDomainMaintenanceProvider()  
    {  
       return new MyDomainMaintenanceProvider();  
    }  
    ```  
  
21. Salvare il progetto e chiuderlo, poiché aggiungerà la procedura successiva. Non sarà in grado di creare il progetto per completare la procedura successiva.  
  
###  <a name="BKMK_DomainMaintenance"></a>Aggiunta di un'implementazione dell'interfaccia IDomainMaintenanceProvider all'assembly  
 IDomainMaintenanceProvider viene utilizzato per gestire il dominio dopo averlo creato.  
  
##### <a name="to-add-the-idomainmaintenanceprovider-code-to-the-assembly"></a>Per aggiungere il codice IDomainMaintenanceProvider all'assembly  
  
1.  Aggiungere l'intestazione della classe per il provider di manutenzione del dominio. Assicurarsi che il nome definito per il provider corrisponda al nome nel metodo GetDomainMaintenanceProvider precedentemente definito.  
  
    ```c#  
  
    public class MyDomainMaintenanceProvider : IDomainMaintenanceProvider  
    {  
    }  
    ```  
  
2.  Aggiungere il metodo Activate, che imposta il provider attivo. Esempio di codice seguente mostra il metodo Activate.  
  
    ```c#  
  
    string DomainName { get; set; }  
    protected DomainProviderSettings Settings { get; set; }  
  
    public void Activate(DomainProviderSettings settings,   
       DomainNameConfiguration config, DomainProviderCredentials credentials)  
    {  
       Settings = settings;  
       SetCredentials(credentials);  
       DomainName = config.AutoConfiguredAnywhereAccessFullName.Punycode;  
    }  
    ```  
  
3.  Aggiungere il metodo Deactivate, che consente di disattivare tutte le azioni. Esempio di codice seguente mostra il metodo Deactivate.  
  
    ```  
  
    public void Deactivate()  
    {  
       //Deactivate all actions  
    }  
  
    ```  
  
4.  Aggiungere il metodo SetCredentials, che aggiorna le credenziali dell'utente. Ad esempio, questo metodo può essere chiamato per aggiornare le credenziali che non sono più valide. Esempio di codice seguente mostra il metodo SetCredentials.  
  
    ```c#  
  
    protected DomainProviderCredentials Credentials { get; set; }  
  
    public bool SetCredentials(DomainProviderCredentials credentials)  
    {  
       this.Credentials = credentials;  
  
       return true;  
    }  
    ```  
  
5.  Aggiungere il metodo ValidateCredentials, che convalida le credenziali specificate. Esempio di codice seguente mostra il metodo ValidateCredentials.  
  
    ```c#  
  
    public static readonly string offerUser = "user1@contoso.com";  
    public static readonly string offerPassword = "password1";  
  
    public bool ValidateCredentials()  
    {  
       if (string.Equals(this.Credentials.UserName,   
          offerUser,   
          StringComparison.OrdinalIgnoreCase) &&   
             string.Equals(this.Credentials.Password, offerPassword))  
          return true;  
  
       return false;  
    }  
    ```  
  
6.  Aggiungere il metodo GetPublicAddress, che restituisce l'indirizzo IP esterno del server. Esempio di codice seguente mostra il metodo GetPublicAddress.  
  
    ```c#  
  
    public IPAddress GetPublicIPAddress()  
    {  
       string PublicIP = "0.0.0.0";  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          PublicIP = (key == null) ? "0.0.0.0" : key.GetValue("PublicIP", "0.0.0.0").ToString();  
       }  
       IPAddress ip = IPAddress.Parse(PublicIP);  
  
       if (PublicIP == "0.0.0.0")  
       {  
          string strHostName = Dns.GetHostName();  
          IPHostEntry ipEntry = Dns.GetHostEntry(strHostName);  
  
          IPAddress[] addr = ipEntry.AddressList;  
          foreach (IPAddress add in addr)  
          {  
             if (add.AddressFamily == AddressFamily.InterNetwork)  
             {  
                return add;  
             }  
          }  
       }  
       else  
       {  
          return IPAddress.Parse(PublicIP);  
       }  
  
       return null;    
    }  
    ```  
  
7.  Aggiungere il metodo SubmitCertificateRequest, che invia la richiesta di certificato per il nome di dominio attualmente configurato.  
  
    ```c#  
  
    string cert=null;  
  
    public void SubmitCertificateRequest(string certificateRequest)  
    {  
       cert = CertManaged.SubmitRequest(certificateRequest, CertCommon.CAServerFQDN + "\\" +      
          CertCommon.CAName,   
          Microsoft.WindowsServerSolutions.CertificateManagement.CRFlags.Base64Header,   
          CertCommon.CATemplate,   
          EncodingFlags.Base64);  
    }  
    ```  
  
8.  Aggiungere il metodo GetCertificateResponse, che restituisce la risposta di certificato se lo stato del dominio è completamente operativo. Questo metodo viene chiamato per entrambe le nuove richieste di certificato e per le richieste di rinnovo certificato. Esempio di codice seguente mostra il metodo GetCertificateResponse.  
  
    ```c#  
  
    public string GetCertificateResponse(bool renew)  
    {  
       return cert;  
    }  
    ```  
  
9. Aggiungere il metodo SubmitRenewCertificateRequest, che elabora il rinnovo del certificato. Esempio di codice seguente mostra il metodo SubmitRenewCertificateRequest.  
  
    ```c#  
  
    public void SubmitRenewCertificateRequest()  
    {  
       // Add certificate renewal code   
    }  
    ```  
  
10. Aggiungere il metodo UpdateDNSRecords, che aggiorna i record DNS archiviati dal provider. Esempio di codice seguente mostra il metodo UpdateDNS.  
  
    ```c#  
  
    public bool UpdateDnsRecords(IList<DnsRecord> records)  
    {  
       string UpdateDNS = "true";  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          UpdateDNS = (key == null) ? "true" : key.GetValue("UpdateDNS", "true").ToString();  
       }  
  
       return UpdateDNS == "true";  
    }  
  
    ```  
  
11. Aggiungere il metodo TestConnection, che tenta di stabilire una connessione al servizio back-end. Se questo metodo richiede l'autenticazione, deve essere generata un'eccezione appropriata. Esempio di codice seguente mostra il metodo TestConnection.  
  
    ```c#  
  
    public bool TestConnection()  
    {  
       // Add code to test connection  
  
       return true;  
    }  
    ```  
  
12. Aggiungere il metodo GetDomainState, che restituisce lo stato corrente del dominio. Esempio di codice seguente mostra il metodo GetDomainState.  
  
    ```c#  
  
    public DomainState GetDomainState()  
    {  
       string domainstatus = "FullyOperational";  
       long expirationDate = 365;  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          domainstatus = (key == null) ? "Ready" : key.GetValue("DomainStatus", "Ready").ToString();  
          expirationDate = Int64.Parse(key.GetValue("ExpirationDate", "365").ToString());  
       }  
  
       switch (domainstatus)  
       {  
          case "Failed":  
             return new DomainState(DomainStatus.Failed,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "Ready":  
             return new DomainState(DomainStatus.Ready,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "InRenewal":  
             return new DomainState(DomainStatus.InRenewal,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "InRenewalCustomerInterventionRequired":  
             return new DomainState(DomainStatus.InRenewalCustomerInterventionRequired,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "Pending":  
             return new DomainState(DomainStatus.Pending,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "PendingCustomerInterventionRequired":  
             return new DomainState(DomainStatus.PendingCustomerInterventionRequired,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "RenewalFailed":  
             return new DomainState(DomainStatus.RenewalFailed,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          default:  
             return new DomainState(DomainStatus.Unknown,   
                null,   
                DateTime.Now.AddDays(expirationDate));                   
          }  
    }  
    ```  
  
13. Aggiungere il metodo GetCertificateState, che restituisce lo stato corrente del certificato. Esempio di codice seguente mostra il metodo GetCertificateState.  
  
    ```c#  
  
    public CertificateState GetCertificateState(bool renew)  
    {  
       return new CertificateState(CertificateStatus.Ready, string.Empty);  
    }  
    ```  
  
14. Salvare e generare la soluzione.  
  
###  <a name="BKMK_SignAssembly"></a>Una firma Authenticode all'assembly  
 È necessario applicarvi una firma Authenticode all'assembly per poter essere utilizzato nel sistema operativo. Per ulteriori informazioni sulla firma dell'assembly, vedere [firma e il controllo del codice con Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
###  <a name="BKMK_InstallAssembly"></a>Installare l'assembly nel computer di riferimento  
 Collocare l'assembly in una cartella nel computer di riferimento. Prendere nota del percorso della cartella, dal momento che dovrà essere immesso nel Registro di sistema nel passaggio successivo.  
  
### <a name="add-a-key-to-the-registry"></a>Aggiungere una chiave del Registro di sistema  
 Aggiungere una voce del Registro di sistema per definire le caratteristiche e la posizione dell'assembly.  
  
##### <a name="to-add-a-key-to-the-registry"></a>Per aggiungere una chiave al Registro di sistema  
  
1.  Nel computer di riferimento, fare clic su **Start**, immettere **regedit**, quindi premere **invio**.  
  
2.  Nel riquadro sinistro, espandere **HKEY_LOCAL_MACHINE**, espandere **SOFTWARE**, espandere **Microsoft**, espandere **Windows Server**, espandere **Gestione dominio**, quindi espandere **provider**.  
  
3.  Fare doppio clic su **provider**, scegliere **New**, quindi fare clic su **chiave**.  
  
4.  Digitare l'identificatore per il provider come nome della chiave. L'identificatore è il GUID definito per il provider nel passaggio 8 di [aggiunta di un'implementazione dell'interfaccia IDomainSignupProvider all'assembly](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup).  
  
5.  Fare clic sulla chiave appena creato, quindi fare clic su **valore stringa**.  
  
6.  Tipo **Assembly** per il nome della stringa e quindi premere **invio**.  
  
7.  Fare doppio clic su nuovo **Assembly** stringa nel riquadro di destra, quindi fare clic su **modifica**.  
  
8.  Digitare il percorso completo del file di assembly precedentemente creato e quindi fare clic su **OK**.  
  
9. Fare doppio clic su chiave nuovo, quindi fare clic su **valore stringa**.  
  
10. Tipo **abilitato** per il nome della stringa e quindi premere **invio**.  
  
11. Fare doppio clic su nuovo **abilitato** stringa nel riquadro di destra, quindi fare clic su **modifica**.  
  
12. Tipo **True**, quindi fare clic su **OK**.  
  
13. Fare doppio clic su chiave nuovo, quindi fare clic su **valore stringa**.  
  
14. Tipo **tipo** per il nome della stringa e quindi premere **invio**.  
  
15. Fare doppio clic su nuovo **tipo** stringa nel riquadro di destra, quindi fare clic su **modifica**.  
  
16. Digitare il nome completo della classe del provider definito nell'assembly, quindi fare clic su **OK**.  
  
###  <a name="BKMK_RestartService"></a>Riavviare il servizio di gestione nome di dominio di Windows Server  
 È necessario riavviare il servizio di gestione di dominio di Windows Server per il provider diventi disponibile per il sistema operativo.  
  
##### <a name="restart-the-service"></a>Riavviare il servizio  
  
1.  Fare clic su **Start**, tipo **mmc**, quindi premere **invio**.  
  
2.  Se lo snap-in servizi non è elencato nella console, aggiungerlo completando i passaggi seguenti:  
  
    1.  Fare clic su **File**, quindi fare clic su **Aggiungi/Rimuovi Snap-in**.  
  
    2.  Nel **snap-in disponibili** elenco, fare clic su **servizi**, quindi fare clic su **Aggiungi**.  
  
    3.  Nel **servizi** finestra di dialogo, verificare che **computer locale** sia selezionata e quindi fare clic su **fine**.  
  
    4.  Fare clic su **OK** per chiudere la **Aggiungi/Rimuovi snap-in** la finestra di dialogo.  
  
3.  Fare doppio clic su **servizi**, scorrere verso il basso e selezionare **Gestione dominio di Windows Server**, quindi fare clic su **riavviare il servizio**.  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)