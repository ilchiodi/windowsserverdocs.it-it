---
title: Comandi Netsh per l'interfaccia portproxy
description: Usare i comandi netsh interface portproxy per fungere da proxy tra le reti e le applicazioni IPv4 e IPv6.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: 6244213ea689f07230ce53288e52959972112037
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405506"
---
# <a name="netsh-interface-portproxy-commands"></a>Comandi del proxy delle porte dell'interfaccia di Netsh

Usare i comandi **netsh interface portproxy** per fungere da proxy tra le reti e le applicazioni IPv4 e IPv6. È possibile usare questi comandi per stabilire il servizio proxy nei modi seguenti:

-   Messaggi dell'applicazione e del computer configurati con IPv4 inviati ad altre applicazioni e computer configurati con IPv4.

-   Messaggi dell'applicazione e del computer configurati con IPv4 inviati a applicazioni e computer configurati in IPv6.

-   Messaggi dell'applicazione e del computer configurati con IPv6 inviati a computer e applicazioni configurati in IPv4.

-   Messaggi dell'applicazione e del computer configurati con IPv6 inviati ad altre applicazioni e computer configurati per IPv6.

Quando si scrivono file batch o script usando questi comandi, ogni comando deve iniziare con **netsh interface portproxy**. Ad esempio, quando si usa il comando **delete v4tov6** per specificare che il server portproxy Elimina una porta e un indirizzo IPv4 dall'elenco di indirizzi IPv4 per cui il server è in ascolto, il file batch o lo script deve usare la sintassi seguente:

```PowerShell
netsh interface portproxy delete v4tov6listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address| HostName}] [[protocol=]tcp]
```

I comandi netsh interface portproxy disponibili sono:

-   [Aggiungi v4tov4](#add-v4tov4)

-   [Aggiungi v4tov6](#add-v4tov6)

-   [Aggiungi v6tov4](#add-v6tov4)

-   [Aggiungi v6tov6](#add-v6tov6)

-   [Elimina v4tov4](#delete-v4tov4)

-   [Elimina v4tov6](#delete-v4tov6)

-   [Elimina v6tov6](#delete-v6tov6)

-   [reimpostazione](#reset)

-   [imposta v4tov4](#set-v4tov4)

-   [imposta v4tov6](#set-v4tov6)

-   [setv6tov4](#set-v6tov4)

-   [imposta v6tov6](#set-v6tov6)

-   [Mostra tutto](#show-all)

-   [Mostra v4tov4](#show-v4tov4)

-   [Mostra v4tov6](#show-v4tov6)

-   [Mostra v6tov4](#show-v6tov4)

-   [Mostra v6tov6](#show-v6tov6)


## <a name="add-v4tov4"></a>Aggiungi v4tov4

Il server portproxy è in ascolto dei messaggi inviati a una porta e un indirizzo IPv4 specifici ed esegue il mapping di una porta e di un indirizzo IPv4 per inviare i messaggi ricevuti dopo aver stabilito una connessione TCP separata.

### <a name="syntax"></a>Sintassi

```PowerShell
add v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName}] [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName}] [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri


|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Specifica la porta IPv4, il numero di porta o il nome del servizio su cui restare in attesa.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv4 a cui connettersi. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non si specifica un indirizzo, il valore predefinito è il computer locale. |
|  **connectport**   |       Specifica la porta IPv4, il numero di porta o il nome del servizio a cui connettersi. Se **connectport** non è specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv4 per il quale restare in ascolto. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non si specifica un indirizzo, il valore predefinito è il computer locale. |
|    **protocollo**    |                                                                                  Specifica il protocollo da utilizzare.                                                                                   |

## <a name="add-v4tov6"></a>Aggiungi v4tov6

Il server portproxy è in ascolto dei messaggi inviati a una porta e un indirizzo IPv4 specifici ed esegue il mapping di una porta e di un indirizzo IPv6 per inviare i messaggi ricevuti dopo aver stabilito una connessione TCP separata.

### <a name="syntax"></a>Sintassi

```PowerShell
add v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Specifica la porta IPv4, il numero di porta o il nome del servizio su cui restare in attesa.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv6 a cui connettersi. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non si specifica un indirizzo, il valore predefinito è il computer locale. |
|  **connectport**   |       Specifica la porta IPv6, il numero di porta o il nome del servizio a cui connettersi. Se **connectport** non è specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv4 su cui restare in attesa. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non si specifica un indirizzo, il valore predefinito è il computer locale.  |
|    **protocollo**    |                                                                                  Specifica il protocollo da utilizzare.                                                                                   |

## <a name="add-v6tov4"></a>Aggiungi v6tov4

Il server portproxy è in ascolto dei messaggi inviati a una porta e un indirizzo IPv6 specifici ed esegue il mapping di una porta e di un indirizzo IPv4 a cui inviare i messaggi ricevuti dopo aver stabilito una connessione TCP separata.

### <a name="syntax"></a>Sintassi

```PowerShell
add v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Specifica la porta IPv6, il numero di porta o il nome del servizio su cui restare in attesa.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv4 a cui connettersi. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non si specifica un indirizzo, il valore predefinito è il computer locale. |
|  **connectport**   |       Specifica la porta IPv4, il numero di porta o il nome del servizio a cui connettersi. Se **connectport** non è specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv6 su cui restare in attesa. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non si specifica un indirizzo, il valore predefinito è il computer locale.  |
|    **protocollo**    |                                                                                  Specifica il protocollo da utilizzare.                                                                                   |

## <a name="add-v6tov6"></a>Aggiungi v6tov6

Il server portproxy è in ascolto dei messaggi inviati a una porta e un indirizzo IPv6 specifici ed esegue il mapping di una porta e di un indirizzo IPv6 a cui inviare i messaggi ricevuti dopo aver stabilito una connessione TCP separata.

### <a name="syntax"></a>Sintassi

```PowerShell
add v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Specifica la porta IPv6, il numero di porta o il nome del servizio su cui restare in attesa.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv6 a cui connettersi. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non si specifica un indirizzo, il valore predefinito è il computer locale. |
|  **connectport**   |       Specifica la porta IPv6, il numero di porta o il nome del servizio a cui connettersi. Se **connectport** non è specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv6 su cui restare in attesa. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non si specifica un indirizzo, il valore predefinito è il computer locale.  |
|    **protocollo**    |                                                                                  Specifica il protocollo da utilizzare.                                                                                   |

## <a name="delete-v4tov4"></a>Elimina v4tov4

Il server portproxy Elimina un indirizzo IPv4 dall'elenco di porte e indirizzi IPv4 per cui il server è in ascolto.

### <a name="syntax"></a>Sintassi

```PowerShell
delete v4tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Specifica la porta IPv4 da eliminare.                                    |
| **listenaddress** | Specifica l'indirizzo IPv4 da eliminare. Se non si specifica un indirizzo, il valore predefinito è il computer locale. |
|   **protocollo**    |                                      Specifica il protocollo da utilizzare.                                      |

## <a name="delete-v4tov6"></a>Elimina v4tov6

Il server portproxy Elimina una porta e un indirizzo IPv4 dall'elenco di indirizzi IPv4 per cui il server è in ascolto.

### <a name="syntax"></a>Sintassi

```PowerShell 
delete v4tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Specifica la porta IPv4 da eliminare.                                    |
| **listenaddress** | Specifica l'indirizzo IPv4 da eliminare. Se non si specifica un indirizzo, il valore predefinito è il computer locale. |
|   **protocollo**    |                                      Specifica il protocollo da utilizzare.                                      |

## <a name="delete-v6tov4"></a>Elimina v6tov4

Il server portproxy Elimina una porta e un indirizzo IPv6 dall'elenco degli indirizzi IPv6 per cui il server è in ascolto.

### <a name="syntax"></a>Sintassi

```PowerShell
delete v6tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Specifica la porta IPv6 da eliminare.                                    |
| **listenaddress** | Specifica l'indirizzo IPv6 da eliminare. Se non si specifica un indirizzo, il valore predefinito è il computer locale. |
|   **protocollo**    |                                      Specifica il protocollo da utilizzare.                                      |

## <a name="delete-v6tov6"></a>Elimina v6tov6

Il server portproxy Elimina un indirizzo IPv6 dall'elenco degli indirizzi IPv6 per cui il server è in ascolto.

### <a name="syntax"></a>Sintassi

```PowerShell
delete v6tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Specifica la porta IPv6 da eliminare.                                    |
| **listenaddress** | Specifica l'indirizzo IPv6 da eliminare. Se non si specifica un indirizzo, il valore predefinito è il computer locale. |
|   **protocollo**    |                                      Specifica il protocollo da utilizzare.                                      |

## <a name="reset"></a>Reimpostazione

Reimposta lo stato di configurazione di IPv6.

### <a name="syntax"></a>Sintassi

`reset`

## <a name="set-v4tov4"></a>imposta v4tov4

Modifica i valori dei parametri di una voce esistente nel server portproxy creato con il comando **add v4tov4** oppure aggiunge una nuova voce all'elenco che esegue il mapping delle coppie porta/indirizzo.

### <a name="syntax"></a>Sintassi

```PowerShell
set v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Specifica la porta IPv4, il numero di porta o il nome del servizio su cui restare in attesa.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv4 a cui connettersi. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non si specifica un indirizzo, il valore predefinito è il computer locale. |
|  **connectport**   |       Specifica la porta IPv4, il numero di porta o il nome del servizio a cui connettersi. Se **connectport** non è specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv4 per il quale restare in ascolto. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non si specifica un indirizzo, il valore predefinito è il computer locale. |
|    **protocollo**    |                                                                                  Specifica il protocollo da utilizzare.                                                                                   |

## <a name="set-v4tov6"></a>imposta v4tov6

Modifica i valori dei parametri di una voce esistente nel server portproxy creato con il comando **add v4tov6** oppure aggiunge una nuova voce all'elenco che esegue il mapping delle coppie porta/indirizzo.

### <a name="syntax"></a>Sintassi

```PowerShell
set v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Specifica la porta IPv4, il numero di porta o il nome del servizio su cui restare in attesa.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv6 a cui connettersi. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non si specifica un indirizzo, il valore predefinito è il computer locale. |
|  **connectport**   |       Specifica la porta IPv6, il numero di porta o il nome del servizio a cui connettersi. Se **connectport** non è specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv4 su cui restare in attesa. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non si specifica un indirizzo, il valore predefinito è il computer locale.  |
|    **protocollo**    |                                                                                  Specifica il protocollo da utilizzare.                                                                                   |

## <a name="set-v6tov4"></a>imposta v6tov4

Modifica i valori dei parametri di una voce esistente nel server portproxy creato con il comando **add v6tov4** oppure aggiunge una nuova voce all'elenco che esegue il mapping delle coppie porta/indirizzo.

### <a name="syntax"></a>Sintassi

```PowerShell
set v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Specifica la porta IPv6, il numero di porta o il nome del servizio su cui restare in attesa.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv4 a cui connettersi. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non si specifica un indirizzo, il valore predefinito è il computer locale. |
|  **connectport**   |       Specifica la porta IPv4, il numero di porta o il nome del servizio a cui connettersi. Se **connectport** non è specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv6 su cui restare in attesa. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non si specifica un indirizzo, il valore predefinito è il computer locale.  |
|    **protocollo**    |                                                                                  Specifica il protocollo da utilizzare.                                                                                   |

## <a name="set-v6tov6"></a>imposta v6tov6

Modifica i valori dei parametri di una voce esistente nel server portproxy creato con il comando **add v6tov6** oppure aggiunge una nuova voce all'elenco che esegue il mapping delle coppie porta/indirizzo.

### <a name="syntax"></a>Sintassi

```PowerShell 
set v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                    |                                                                                                                                                                                                    |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                            Specifica la porta IPv6, il numero di porta o il nome del servizio su cui restare in attesa.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv6 a cui connettersi. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non si specifica un indirizzo, il valore predefinito è il computer locale.  |
|  **connectport**   |        Specifica la porta IPv6, il numero di porta o il nome del servizio a cui connettersi. Se **connectport** non è specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv6 su cui restare in attesa. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non si specifica un indirizzo, il valore predefinito è il computer locale. |
|    **protocollo**    |                                                                                   Specifica il protocollo da utilizzare.                                                                                   |

## <a name="show-all"></a>visualizzazione dell'elenco completo

Visualizza tutti i parametri di portproxy, incluse le coppie porta/indirizzo per v4tov4, v4tov6, v6tov4 e v6tov6.

### <a name="syntax"></a>Sintassi

`show all`


## <a name="show-v4tov4"></a>Mostra v4tov4

Visualizza i parametri portproxy di v4tov4.

### <a name="syntax"></a>Sintassi

`show v4tov4`

## <a name="show-v4tov6"></a>Mostra v4tov6

Visualizza i parametri portproxy di v4tov6.

### <a name="syntax"></a>Sintassi

`show v4tov6`

## <a name="show-v6tov4"></a>Mostra v6tov4

Visualizza i parametri portproxy di v6tov4.

### <a name="syntax"></a>Sintassi

`show v6tov4`


## <a name="show-v6tov6"></a>Mostra v6tov6

Visualizza i parametri portproxy di v6tov6.

### <a name="syntax"></a>Sintassi

`show v6tov6`

---
