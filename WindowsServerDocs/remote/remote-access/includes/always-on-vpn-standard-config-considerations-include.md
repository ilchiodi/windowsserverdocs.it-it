## <a name="standard-configuration-considerations"></a>Considerazioni sulla configurazione standard

VPN Always On ha molte opzioni di configurazione. Tuttavia, si sceglie la configurazione VPN, tuttavia, includono le informazioni seguenti:

-   **Tipo di connessione.** La selezione del protocollo di connessione è importante e infine va di pari passo con il tipo di autenticazione che si userà. Per informazioni dettagliate sui protocolli di tunneling disponibili, vedere [tipi di connessione VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-connection-type).

-   **Routing.** In questo contesto, le regole di routing determinano se gli utenti possono usare altre route di rete durante la connessione alla VPN.

    -   _Lo split tunneling_ consente l'accesso simultaneo ad altre reti, ad esempio Internet.

    -   _Il tunneling forzato_ richiede tutto il traffico passi esclusivamente tramite la connessione VPN e non consente l'accesso simultaneo ad altre reti.

-   **Triggering.** _Attivazione_ determina come e quando viene avviata una connessione VPN (ad esempio, quando si apre un'app, quando il dispositivo è stato attiva, manualmente dall'utente). Per l'attivazione di opzioni, vedere la [connettività VPN](#vpn-connectivity).

-   **Autenticazione di utenti o dispositivi.** VPN Always On Usa i certificati del dispositivo e connessione avviata dal dispositivo tramite una funzionalità denominata [dispositivo Tunnel](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-device-tunnel-config). Tale connessione può essere avviata automaticamente e viene mantenuta, simili a una connessione con tunnel dell'infrastruttura DirectAccess.

>[!TIP]
>Durante la migrazione da DirectAccess a VPN Always On, è consigliabile iniziare con le opzioni di configurazione che sono paragonabili a ciò che dispone e quindi espandere da tale posizione.

Usando i certificati utente, il client VPN Always On si connette automaticamente, ma esegue l'operazione a livello di utente (dopo l'accesso dell'utente) anziché a livello di dispositivo (prima dell'accesso dell'utente). L'esperienza è comunque trasparente all'utente, ma supporta meccanismi di autenticazione più avanzati, come Windows Hello for Business.