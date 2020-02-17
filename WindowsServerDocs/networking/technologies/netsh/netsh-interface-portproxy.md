---
title: Comandi di Netsh per il proxy delle porte di interfaccia
description: Usa i comandi netsh interface portproxy per agire come proxy tra le reti e le applicazioni IPv4 e IPv6.
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
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405506"
---
# <a name="netsh-interface-portproxy-commands"></a>Comandi netsh interface portproxy

Usa i comandi **netsh interface portproxy** per agire come proxy tra le reti e le applicazioni IPv4 e IPv6. Puoi usare questi comandi per stabilire il servizio proxy nei modi seguenti:

-   Messaggi di computer e applicazioni configurati con IPv4 inviati ad altri computer e applicazioni configurati con IPv4.

-   Messaggi di computer e applicazioni configurati con IPv4 inviati a computer e applicazioni configurati con IPv6.

-   Messaggi di computer e applicazioni configurati con IPv6 inviati a computer e applicazioni configurati con IPv4.

-   Messaggi di computer e applicazioni configurati con IPv6 inviati ad altri computer e applicazioni configurati con IPv6.

Quando vengono scritti script o file batch usando questi comandi, ogni comando deve iniziare con **netsh interface portproxy**. Ad esempio, quando usi il comando **delete v4tov6** per specificare che il server proxy delle porte elimina una porta e un indirizzo IPv4 dall'elenco di indirizzi IPv4 per cui è in ascolto, lo script o il file batch deve usare la sintassi seguente:

```PowerShell
netsh interface portproxy delete v4tov6listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address| HostName}] [[protocol=]tcp]
```

I comandi netsh interface portproxy disponibili sono i seguenti:

-   [add v4tov4](#add-v4tov4)

-   [add v4tov6](#add-v4tov6)

-   [add v6tov4](#add-v6tov4)

-   [add v6tov6](#add-v6tov6)

-   [delete v4tov4](#delete-v4tov4)

-   [delete v4tov6](#delete-v4tov6)

-   [delete v6tov6](#delete-v6tov6)

-   [reset](#reset)

-   [set v4tov4](#set-v4tov4)

-   [set v4tov6](#set-v4tov6)

-   [setv6tov4](#set-v6tov4)

-   [set v6tov6](#set-v6tov6)

-   [show all](#show-all)

-   [show v4tov4](#show-v4tov4)

-   [show v4tov6](#show-v4tov6)

-   [show v6tov4](#show-v6tov4)

-   [show v6tov6](#show-v6tov6)


## <a name="add-v4tov4"></a>add v4tov4

Il server proxy delle porte è in ascolto dei messaggi inviati a una porta e a un indirizzo IPv4 specifici ed esegue il mapping di una porta e di un indirizzo IPv4 per inviare i messaggi ricevuti dopo aver stabilito una connessione TCP separata.

### <a name="syntax"></a>Sintassi

```PowerShell
add v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName}] [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName}] [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri


|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Specifica la porta IPv4, per numero di porta o nome di servizio, su cui restare in ascolto.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv4 a cui connettersi. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|  **connectport**   |       Specifica la porta IPv4, per numero di porta o nome di servizio, a cui connettersi. Se il parametro **connectport** non è specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv4 per cui restare in ascolto. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|    **protocol**    |                                                                                  Specifica il protocollo da usare.                                                                                   |

## <a name="add-v4tov6"></a>add v4tov6

Il server proxy delle porte è in ascolto dei messaggi inviati a una porta e a un indirizzo IPv4 specifici ed esegue il mapping di una porta e di un indirizzo IPv6 per inviare i messaggi ricevuti dopo aver stabilito una connessione TCP separata.

### <a name="syntax"></a>Sintassi

```PowerShell
add v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Specifica la porta IPv4, per numero di porta o nome di servizio, su cui restare in ascolto.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv6 a cui connettersi. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|  **connectport**   |       Specifica la porta IPv6, per numero di porta o nome di servizio, a cui connettersi. Se il parametro **connectport** non è specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv4 su cui restare in ascolto. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale.  |
|    **protocol**    |                                                                                  Specifica il protocollo da usare.                                                                                   |

## <a name="add-v6tov4"></a>add v6tov4

Il server proxy delle porte è in ascolto dei messaggi inviati a una porta e a un indirizzo IPv6 specifici ed esegue il mapping di una porta e di un indirizzo IPv4 a cui inviare i messaggi ricevuti dopo aver stabilito una connessione TCP separata.

### <a name="syntax"></a>Sintassi

```PowerShell
add v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Specifica la porta IPv6, per numero di porta o nome di servizio, su cui restare in ascolto.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv4 a cui connettersi. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|  **connectport**   |       Specifica la porta IPv4, per numero di porta o nome di servizio, a cui connettersi. Se il parametro **connectport** non è specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv6 su cui restare in ascolto. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale.  |
|    **protocol**    |                                                                                  Specifica il protocollo da usare.                                                                                   |

## <a name="add-v6tov6"></a>add v6tov6

Il server proxy delle porte è in ascolto dei messaggi inviati a una porta e a un indirizzo IPv6 specifici ed esegue il mapping di una porta e di un indirizzo IPv6 a cui inviare i messaggi ricevuti dopo aver stabilito una connessione TCP separata.

### <a name="syntax"></a>Sintassi

```PowerShell
add v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Specifica la porta IPv6, per numero di porta o nome di servizio, su cui restare in ascolto.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv6 a cui connettersi. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|  **connectport**   |       Specifica la porta IPv6, per numero di porta o nome di servizio, a cui connettersi. Se il parametro **connectport** non è specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv6 su cui restare in ascolto. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale.  |
|    **protocol**    |                                                                                  Specifica il protocollo da usare.                                                                                   |

## <a name="delete-v4tov4"></a>delete v4tov4

Il server proxy delle porte elimina un indirizzo IPv4 dall'elenco di porte e indirizzi IPv4 per cui il server resta in ascolto.

### <a name="syntax"></a>Sintassi

```PowerShell
delete v4tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Specifica la porta IPv4 da eliminare.                                    |
| **listenaddress** | Specifica l'indirizzo IPv4 da eliminare. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|   **protocol**    |                                      Specifica il protocollo da usare.                                      |

## <a name="delete-v4tov6"></a>delete v4tov6

Il server proxy delle porte elimina una porta e un indirizzo IPv4 dall'elenco di indirizzi IPv4 per cui il server resta in ascolto.

### <a name="syntax"></a>Sintassi

```PowerShell 
delete v4tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Specifica la porta IPv4 da eliminare.                                    |
| **listenaddress** | Specifica l'indirizzo IPv4 da eliminare. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|   **protocol**    |                                      Specifica il protocollo da usare.                                      |

## <a name="delete-v6tov4"></a>delete v6tov4

Il server proxy delle porte elimina una porta e un indirizzo IPv6 dall'elenco di indirizzi IPv6 per cui il server resta in ascolto.

### <a name="syntax"></a>Sintassi

```PowerShell
delete v6tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Specifica la porta IPv6 da eliminare.                                    |
| **listenaddress** | Specifica l'indirizzo IPv6 da eliminare. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|   **protocol**    |                                      Specifica il protocollo da usare.                                      |

## <a name="delete-v6tov6"></a>delete v6tov6

Il server proxy delle porte elimina un indirizzo IPv6 dall'elenco di indirizzi IPv6 per cui il server resta in ascolto.

### <a name="syntax"></a>Sintassi

```PowerShell
delete v6tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Specifica la porta IPv6 da eliminare.                                    |
| **listenaddress** | Specifica l'indirizzo IPv6 da eliminare. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|   **protocol**    |                                      Specifica il protocollo da usare.                                      |

## <a name="reset"></a>reset

Reimposta lo stato di configurazione di IPv6.

### <a name="syntax"></a>Sintassi

`reset`

## <a name="set-v4tov4"></a>set v4tov4

Modifica i valori dei parametri di una voce esistente nel server proxy delle porte di cui è stata eseguita la creazione con il comando **add v4tov4** oppure aggiunge una nuova voce all'elenco che esegue il mapping delle coppie porta/indirizzo.

### <a name="syntax"></a>Sintassi

```PowerShell
set v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Specifica la porta IPv4, per numero di porta o nome di servizio, su cui restare in ascolto.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv4 a cui connettersi. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|  **connectport**   |       Specifica la porta IPv4, per numero di porta o nome di servizio, a cui connettersi. Se il parametro **connectport** non è specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv4 per cui restare in ascolto. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|    **protocol**    |                                                                                  Specifica il protocollo da usare.                                                                                   |

## <a name="set-v4tov6"></a>set v4tov6

Modifica i valori dei parametri di una voce esistente nel server proxy delle porte di cui è stata eseguita la creazione con il comando **add v4tov6** oppure aggiunge una nuova voce all'elenco che esegue il mapping delle coppie porta/indirizzo.

### <a name="syntax"></a>Sintassi

```PowerShell
set v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Specifica la porta IPv4, per numero di porta o nome di servizio, su cui restare in ascolto.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv6 a cui connettersi. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|  **connectport**   |       Specifica la porta IPv6, per numero di porta o nome di servizio, a cui connettersi. Se il parametro **connectport** non è specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv4 su cui restare in ascolto. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale.  |
|    **protocol**    |                                                                                  Specifica il protocollo da usare.                                                                                   |

## <a name="set-v6tov4"></a>set v6tov4

Modifica i valori dei parametri di una voce esistente nel server proxy delle porte di cui è stata eseguita la creazione con il comando **add v6tov4** oppure aggiunge una nuova voce all'elenco che esegue il mapping delle coppie porta/indirizzo.

### <a name="syntax"></a>Sintassi

```PowerShell
set v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Specifica la porta IPv6, per numero di porta o nome di servizio, su cui restare in ascolto.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv4 a cui connettersi. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale. |
|  **connectport**   |       Specifica la porta IPv4, per numero di porta o nome di servizio, a cui connettersi. Se il parametro **connectport** non è specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv6 su cui restare in ascolto. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale.  |
|    **protocol**    |                                                                                  Specifica il protocollo da usare.                                                                                   |

## <a name="set-v6tov6"></a>set v6tov6

Modifica i valori dei parametri di una voce esistente nel server proxy delle porte di cui è stata eseguita la creazione con il comando **add v6tov6** oppure aggiunge una nuova voce all'elenco che esegue il mapping delle coppie porta/indirizzo.

### <a name="syntax"></a>Sintassi

```PowerShell 
set v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parametri

|                    |                                                                                                                                                                                                    |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                            Specifica la porta IPv6, per numero di porta o nome di servizio, su cui restare in ascolto.                                                            |
| **connectaddress** | Specifica l'indirizzo IPv6 a cui connettersi. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non viene specificato un indirizzo, il valore predefinito è il computer locale.  |
|  **connectport**   |        Specifica la porta IPv6, per numero di porta o nome di servizio, a cui connettersi. Se il parametro **connectport** non è specificato, il valore predefinito è il valore di **listenport** nel computer locale.        |
| **listenaddress**  | Specifica l'indirizzo IPv6 su cui restare in ascolto. I valori accettabili sono l'indirizzo IP, il nome NetBIOS del computer o il nome DNS del computer. Se non specifichi un indirizzo, il valore predefinito è il computer locale. |
|    **protocol**    |                                                                                   Specifica il protocollo da usare.                                                                                   |

## <a name="show-all"></a>visualizzazione dell'elenco completo

Visualizza tutti i parametri del proxy delle porte, incluse le coppie porta/indirizzo per v4tov4, v4tov6, v6tov4 e v6tov6.

### <a name="syntax"></a>Sintassi

`show all`


## <a name="show-v4tov4"></a>show v4tov4

Visualizza i parametri proxy delle porte di v4tov4.

### <a name="syntax"></a>Sintassi

`show v4tov4`

## <a name="show-v4tov6"></a>show v4tov6

Visualizza i parametri proxy delle porte di v4tov6.

### <a name="syntax"></a>Sintassi

`show v4tov6`

## <a name="show-v6tov4"></a>show v6tov4

Visualizza i parametri proxy delle porte di v6tov4.

### <a name="syntax"></a>Sintassi

`show v6tov4`


## <a name="show-v6tov6"></a>show v6tov6

Visualizza i parametri proxy delle porte di v6tov6.

### <a name="syntax"></a>Sintassi

`show v6tov6`

---
