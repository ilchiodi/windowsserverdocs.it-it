---
ms.date: 01/07/2019
contributor: damaerteMSFT
author: maertendmsft
title: OpenSSH in Windows
ms.product: windows-server
ms.openlocfilehash: 57126f38245f4547a04ea3a51f58aee865b5eb97
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80852034"
---
# <a name="openssh-in-windows"></a>OpenSSH in Windows

OpenSSH è la versione open source degli strumenti SSH (Secure Shell) usati dagli amministratori di Linux e altri prodotti non Windows per la gestione multipiattaforma dei sistemi remoti. OpenSSH è stato aggiunto a Windows dall'autunno 2018 ed è incluso in Windows 10 e Windows Server 2019. 

SSH si basa su un'architettura client-server dove il sistema su cui opera l'utente è il client e il sistema remoto gestito è il server. OpenSSH include una gamma di componenti e strumenti progettati per offrire un approccio semplice e sicuro all'amministrazione remota del sistema, tra cui:

* sshd.exe, ovvero il componente server SSH che deve essere in esecuzione nel sistema gestito in remoto 
* ssh.exe, ovvero il componente client SSH in esecuzione nel sistema locale dell'utente
* ssh-keygen.exe, che genera, gestisce e converte le chiavi di autenticazione per SSH 
* ssh-agent.exe, che archivia le chiavi private usate per l'autenticazione con chiave pubblica
* ssh-add.exe, che aggiunge chiavi private all'elenco consentito dal server
* ssh-keyscan.exe, che aiuta a raccogliere le chiavi host SSH pubbliche da un numero di host
* sftp.exe, ovvero il servizio che fornisce Secure File Transfer Protocol e viene eseguito tramite SSH
* scp.exe, ovvero un'utilità di copia file che viene eseguita in SSH

La documentazione in questa sezione descrive l'uso di OpenSSH in Windows e fornisce informazioni sull'installazione e i casi di utilizzo e configurazione specifici di Windows. Ecco gli argomenti:
* Installazione e disinstallazione di OpenSSH per Windows Server 2019 e Windows 10

La documentazione dettagliata aggiuntiva per le funzionalità di OpenSSH più comuni è disponibile online all'indirizzo [OpenSSH.com](https://www.openssh.com/manual.html). 

Il [progetto open source OpenSSH](https://www.openssh.com) principale è gestito dagli sviluppatori del progetto OpenBSD. Il fork Microsoft di questo progetto si trova in [GitHub](https://github.com/PowerShell/openssh-portable).
È gradito il feedback su Windows OpenSSH che può essere fornito creando ticket GitHub nel [repository GitHub relativo a OpenSSH](https://github.com/PowerShell/openssh-portable). 
