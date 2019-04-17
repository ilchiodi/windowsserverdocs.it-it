---
title: Principali soluzioni di supporto per Windows Server
description: Ottenere i collegamenti alle soluzioni dei problemi di Windows Server
ms.prod: windows-server-threshold
ms.service: na
manager: alant
ms.technology: server-general
ms.date: 09/06/2017
ms.topic: article
author: kaushika-msft
ms.author: elizapo
ms.openlocfilehash: 070331c8886011035e712f485af45d36b77613fa
ms.sourcegitcommit: 58dde3f9ae761116b2c9f71c6917f96bff075af1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/08/2017
---
# <a name="top-support-solutions-for-windows-server-2016"></a>Principali soluzioni di supporto per Windows Server 2016

Microsoft rilascia regolarmente gli aggiornamenti e le soluzioni per Windows Server. Per garantire che i server ricevano gli aggiornamenti futuri, inclusi gli aggiornamenti della sicurezza, è importante mantenerli aggiornati. Consulta la pagina [Cronologia degli aggiornamenti di Windows 10 e Windows Server 2016](https://support.microsoft.com/en-us/help/4000825/windows-10-windows-server-2016-update-history) per un elenco completo di aggiornamenti rilasciati.

Ecco le principali soluzioni del Supporto tecnico Microsoft per la maggior parte dei problemi comuni rilevati quando si utilizza Windows Server 2016. I collegamenti seguenti includono quelli per articoli KB, aggiornamenti e articoli di libreria.

## <a name="solutions-for-installing-or-upgrading-windows-server"></a>Soluzioni per l'installazione o l'aggiornamento di Windows Server

- [Risolvere gli errori di aggiornamento di Windows 10: informazioni tecniche per i professionisti IT](\windows\deployment\upgrade\resolve-windows-10-upgrade-errors)
- [Manutenzione dello stack di aggiornamento per Windows 10 versione 1607 e Windows Server 2016: 8 agosto 2017](https://support.microsoft.com/en-US/help/4035631)
- [Aggiornamento di compatibilità per l'aggiornamento a Windows 10 versione 1607 e Windows Server 2016: 3 agosto 2017](https://support.microsoft.com/en-US/help/4033524)
- [Aggiornamento sul posto del sistema non supportato in macchine virtuali di Azure basate su Windows](https://support.microsoft.com/en-US/help/4014997)
- [Opzioni di aggiornamento e conversione per Windows Server 2016](..\get-started\supported-upgrade-paths.md)
- [Matrice di aggiornamento e migrazione dei ruoli server per Windows Server 2016](..\get-started\server-role-upgradeability-table.md)
- [Installazione e aggiornamento di Windows Server](..\get-started\installation-and-upgrade.md)
- [Note sulla versione: problemi importanti di Windows Server 2016](..\get-started\windows-server-2016-ga-release-notes.md)
- [Indicazioni per lo spostamento in Windows Server 2016](..\get-started\recommendations-moving-to-server2016.md)

## <a name="solutions-for-volume-activation"></a>Soluzioni per l'attivazione di contratti multilicenza
- [Attivazione di Windows Server 2016](../get-started/server-2016-activation.md)
- [Esaminare e selezionare i metodi di attivazione](https://technet.microsoft.com/library/jj134256(ws.11).aspx)
- [Codici di errore di attivazione per l'attivazione di contratti multilicenza](https://technet.microsoft.com/library/dn502528.aspx)
- [Come risolvere problemi relativi a Servizio di gestione delle chiavi](https://technet.microsoft.com/library/ee939272.aspx)
- [Risoluzione dei problemi relativi all'attivazione di contratti multilicenza](https://technet.microsoft.com/library/ff793439.aspx)
- [Codici di errore di attivazione](https://technet.microsoft.com/library/ff793399.aspx)
- [È possibile che l'installazione di Windows non riesca con il seguente errore: "Il codice Product Key immesso non corrisponde ad alcuna delle immagini di Windows disponibili per l'installazione. Immettere un altro codice Product Key.](https://support.microsoft.com/help/2796988/windows-8-or-windows-server-2012-installation-may-fail-with-error-mess)

## <a name="solutions-related-to-dcpromo-and-installing-domain-controllers"></a>Soluzioni correlate a DCPromo e installazione di controller di dominio
- [Requisiti relativi alle porte per Active Directory e Active Directory Domain Services](https://technet.microsoft.com/library/dd772723(v=ws.10).aspx)
- [Porte firewall per Active Directory: indicazioni semplificate](http://blogs.msmvps.com/acefekay/2011/11/01/active-directory-firewall-ports-let-s-try-to-make-this-simple/)
- [Supporto di Exchange Server per Windows Server 2016](https://technet.microsoft.com/library/ff728623(v=exchg.150).aspx)
- [Utilizzo dello strumento Ntdsutil.exe per assegnare o trasferire ruoli FSMO a un controller di dominio](http://support.microsoft.com/kb/255504)
- [Risoluzione dei problemi relativi alla distribuzione di controller di dominio](../identity/ad-ds/deploy/troubleshooting-domain-controller-deployment.md)
- [Risoluzione dei problemi relativi all'Installazione guidata di Active Directory](https://msdn.microsoft.com/library/bb727058.aspx)
- [Problemi noti per l'installazione e la rimozione di Active Directory Domain Services](https://technet.microsoft.com/library/cc754463(v=ws.10).aspx)

## <a name="solutions-for-active-directory-federation-services-ad-fs"></a>Soluzioni per Active Directory Federation Services (AD FS)
- [Come configurare la registrazione automatica di dispositivi aggiunti a un dominio di Windows con Azure Active Directory](/azure/active-directory/active-directory-conditional-access-automatic-device-registration-setup)
- [Configurare il rilascio delle attestazioni](/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup#step-2-setup-issuance-of-claims)
- [Configurare AD FS per l'autenticazione di utenti memorizzati nelle directory LDAP](../identity/ad-fs/operations/configure-ad-fs-to-authenticate-users-stored-in-ldap-directories.md)
- [Supporto di AD FS per l'associazione di nomi host alternativi per l'autenticazione del certificato](../identity/ad-fs/operations/ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)
- [Protezione contro gli attacchi verso le password](https://blogs.technet.microsoft.com/tspring/2017/01/20/federated-to-microsoft-cloud-and-account-lockouts/)
- [Aggiornamento ad AD FS in Windows Server 2016 tramite un database interno di Windows](../identity/ad-fs/deployment/upgrading-to-ad-fs-in-windows-server-2016.md)
- [Accesso a Windows 10: abilitare l'autenticazione del dispositivo con AD FS](../identity/ad-fs/operations/configure-device-based-conditional-access-on-premises.md)
- [Gestione di certificati SSL in AD FS e WAP in Windows Server 2016](../identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap-2016.md)
- [Criteri di controllo di accesso in AD FS per Windows Server 2016](../identity/ad-fs/operations/access-control-policies-in-ad-fs.md)

## <a name="solutions-related-to-active-directory-replication"></a>Soluzioni correlate alla replica di Active Directory

- [Risoluzione dei problemi di replica di Active Directory](../identity/ad-ds/manage/troubleshoot/troubleshooting-active-directory-replication-problems.md)
- [Download dello strumento Stato replica di Active Directory dall'Area Download Microsoft](http://www.microsoft.com/en-in/download/details.aspx?id=30005)
- [e2e: Come risolvere i problemi comuni relativi agli errori di replica di Active Directory](http://support.microsoft.com/kb/3108513)
- [Risoluzione dei problemi relativi all'errore di replica AD 8606: Attributi insufficienti per la creazione di un oggetto](http://support.microsoft.com/kb/2028495)
- [L'ID evento 2108 e l'ID evento 1084 si verificano durante la replica in ingresso di Active Directory in Windows 2000 Server e Windows Server 2003](http://support.microsoft.com/kb/837932)
- [Risoluzione dei problemi relativi all'errore di replica AD 8451: Errore del database rilevato durante la replica](http://support.microsoft.com/kb/2645996)
- [Risoluzione dei problemi relativi all'errore di replica AD 1127: Durante l'accesso al disco rigido, un'operazione su disco non è riuscita anche dopo diversi tentativi](http://support.microsoft.com/kb/2025726)
- [Pulizia dei metadati del server](https://technet.microsoft.com/en-us/library/cc816907.aspx)