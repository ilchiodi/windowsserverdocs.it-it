---
title: Il client Desktop remoto si disconnette e non riesce a riconnettersi alla stessa sessione
description: Risoluzione di un problema per cui il client Desktop remoto si disconnette e non riesce a riconnettersi alla stessa sessione.
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 2c54442a5ce1322f4157de5dbf82d1c662c6f91d
ms.sourcegitcommit: 2a15de216edde8b8e240a4aa679dc6d470e4159e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77465595"
---
# <a name="remote-desktop-client-disconnects-and-cant-reconnect-to-the-same-session"></a>Il client Desktop remoto si disconnette e non riesce a riconnettersi alla stessa sessione

Dopo aver perso la connessione al desktop remoto, il client Desktop remoto non riesce a riconnettersi immediatamente. L'utente riceve un messaggio di errore simile ai seguenti:

  - Impossibile connettersi al server desktop remoto a causa di un errore di sicurezza. Assicurarsi di aver eseguito l'accesso alla rete e quindi provare di nuovo a connettersi.
  - Desktop remoto disconnesso. Impossibile connettersi al computer remoto. Errore di sicurezza. Verificare di essere connessi alla rete, quindi provare a di nuovo a connettersi.

Quando il client Desktop remoto si riconnette, il server Host sessione Desktop remoto riconnette il client a una nuova sessione invece che alla sessione originale. Quando, tuttavia, controlli il server Host sessione Desktop remoto, viene segnalato che la sessione originale è ancora attiva e non è stata attivata la disconnessione.

Per ovviare a questo problema, puoi abilitare il criterio **Configura intervallo keep-alive della connessione** nella cartella di Criteri di gruppo **Configurazione computer\\Modelli amministrativi\\Componenti di Windows\\Servizi Desktop remoto\\Host sessione Desktop remoto\\Connessioni**. Se abiliti questo criterio, devi immettere un intervallo keep-alive. Tale intervallo determina con quale frequenza, in minuti, il server verifica lo stato della sessione.

Questo problema può essere corretto anche riconfigurando le impostazioni di autenticazione e configurazione. Puoi riconfigurare queste impostazioni a livello di server oppure usando oggetti Criteri di gruppo (GPO). Ecco come riconfigurare le impostazioni: cartella di Criteri di gruppo **Configurazione computer\\Modelli amministrativi\\Componenti di Windows\\Servizi Desktop remoto\\Host sessione Desktop remoto\\Sicurezza**.

1. Nel server Host sessione Desktop remoto apri **Configurazione host sessione Desktop remoto**.
2. In **Connessioni** fai clic con il pulsante destro del mouse sul nome della connessione e quindi scegli **Proprietà**.
3. Nella finestra di dialogo **Proprietà** per la connessione, nella scheda **Generale**, nel livello **Sicurezza** seleziona un metodo di sicurezza.
4. Vai a **Livello di crittografia** e seleziona il livello desiderato. Puoi selezionare **Basso**, **Compatibile con client**, **Alto** o **Conforme FIPS**.

> [!NOTE]  
>  - Se le comunicazioni tra i client e i server Host sessione Desktop remoto richiedono il livello di crittografia più alto, usa la crittografia Conforme FIPS.
>  - Le impostazioni del livello di crittografia configurate in Criteri di gruppo hanno priorità su quelle definite usando lo strumento Configurazione di Servizi Desktop remoto. Inoltre, se abiliti il criterio [Crittografia di sistema: utilizza algoritmi FIPS compatibili per crittografia, hash e firma](https://docs.microsoft.com/windows/security/threat-protection/security-policy-settings/system-cryptography-use-fips-compliant-algorithms-for-encryption-hashing-and-signing), questa impostazione esegue l'override del criterio **Imposta livello di crittografia connessione client**. Il criterio di crittografia di sistema è disponibile nella cartella **Configurazione computer\\Impostazioni di Windows\\Impostazioni sicurezza\\Criteri locali\\Opzioni di sicurezza**.
>  - Quando si cambia il livello di crittografia, il nuovo valore impostato viene applicato al successivo accesso di un utente. Se sono necessari più livelli di crittografia in un server, installa più schede di rete e configura ciascuna scheda separatamente.
>  - Per verificare che il certificato abbia una chiave privata corrispondente, in Configurazione di Servizi Desktop remoto fai clic con il pulsante destro del mouse sulla connessione per la quale vuoi visualizzare il certificato, scegli **Generale** e quindi fai clic su **Modifica**. Successivamente seleziona **Visualizza certificato**. Quando passi alla scheda **Generale** dovrebbe essere visualizzata la dicitura "L'utente possiede una chiave privata corrispondente al certificato", se è presente una chiave. Puoi visualizzare queste informazioni anche con lo snap-in Certificati.
>  - La crittografia Conforme FIPS (il criterio **Crittografia di sistema: utilizza algoritmi FIPS compatibili per crittografia, hash e firma** o l'impostazione **Conforme FIPS** in Configurazione Server Desktop remoto) crittografa e decrittografa i dati scambiati tra il server e il client con gli algoritmi di crittografia FIPS (Federal Information Processing Standard) 140-1 che usano i moduli crittografici Microsoft. Per altre informazioni, vedi [Convalida FIPS 140](https://docs.microsoft.com/windows/security/threat-protection/fips-140-validation).
>  - L'impostazione **Alto** crittografa i dati inviati tra il server e il client usando la crittografia avanzata a 128 bit.
>  - L'impostazione **Compatibile con client** crittografa i dati inviati tra il client e il server con il massimo livello di sicurezza della chiave supportato dal client.
>  - L'impostazione **Basso** crittografa i dati inviati dal client al server usando la crittografia a 56 bit.
