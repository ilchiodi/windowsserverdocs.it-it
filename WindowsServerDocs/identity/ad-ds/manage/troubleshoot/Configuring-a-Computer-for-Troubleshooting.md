---
ms.assetid: 155abe09-6360-4913-8dd9-7392d71ea4e6
title: Configurazione di un Computer per la risoluzione dei problemi
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 915b8d1133b3bee050f5eedce9e7ac445f833048
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="configuring-a-computer-for-troubleshooting"></a>Configurazione di un Computer per la risoluzione dei problemi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
  <introduction>
    <para>Prima di usare tecniche di risoluzione avanzata per identificare e risolvere i problemi di Active Directory, configurare i computer per la risoluzione dei problemi. È inoltre una conoscenza di base <token>nextref_longhorincludes > Risoluzione dei problemi relativi concetti, procedure e strumenti. </para>
    <para>Per informazioni sugli strumenti di monitoraggio per Windows Server 2008, vedere il Step-by-Step Guida di monitoraggio delle prestazioni e affidabilità in Windows Server 2008 (<externalLink><linkText>https://go.microsoft.com/fwlink/?LinkId=123737</linkText><linkUri>https://go.microsoft.com/fwlink/?LinkId=123737</linkUri></externalLink>).</para>
  </introduction>
  <section>
    <title>Attività di configurazione per la risoluzione dei problemi</title>
    <content>
      <para>per configurare il computer per la risoluzione dei servizi di dominio Active Directory (AD DS), eseguire le attività seguenti:</para>
      <para>
        <link xlink:href="#BKMK_2">installare strumenti di amministrazione remota per Active Directory</link>
      </para>
      <para>
        <link xlink:href="#BKMK_3">configurare affidabilità e Performance Monitor</link>
      </para>
      <para>
        <link xlink:href="#BKMK_4">impostare livelli di registrazione</link>
      </para>
    </content>
    <sections>
      <section address="BKMK_2">
        <title>Installare strumenti di amministrazione remota del Server per AD DS</title>
        <content>
          <para>quando si installa Servizi di dominio Active Directory per creare un controller di dominio, gli strumenti di amministrazione utilizzabili per gestire servizi di dominio Active Directory vengono installati automaticamente. Se si desidera gestire i controller di dominio in modalità remota da un computer che non è un controller di dominio, è possibile installare gli strumenti di amministrazione in un server membro che esegue <token>nextref_longhorincludes > o in un computer che esegue Windows Vista con Service Pack 1 (SP1). Sui server membri che eseguono <token>nextref_longhorincludes >, usare la funzionalità Strumenti di amministrazione remota Server (RSAT) in Server Manager per installare strumenti di servizi di dominio di Active Directory. Amministrazione remota del server sostituisce gli strumenti di supporto di Windows in Windows Server 2003. È inoltre possibile installare strumenti di servizi di dominio di Active Directory in un computer che esegue Windows Vista con Service Pack 1 (SP1), scaricare gli strumenti per il computer.</para>
          <para>per informazioni sull'installazione di amministrazione remota del server, vedere <link xlink:href="610ba7d9-51b5-4e14-9232-0510a9091aba">installazione degli strumenti di amministrazione Server remota per Active Directory</link>.</para>
        </content>
      </section>
      <section address="BKMK_3">
        <title> Configurare l'affidabilità e Performance Monitor</title>
        <content>
          <para>Windows Server 2008 include il monitoraggio affidabilità e Performance Monitor, che è uno snap-in Microsoft Management Console (MMC) che combina la funzionalità degli strumenti autonomi precedenti, inclusi i registri di prestazioni e avvisi, Server Performance Advisor e Monitor di sistema. Questo snap-in fornisce un'interfaccia utente grafica (GUI) per la personalizzazione insiemi agenti di raccolta dati e sessioni di traccia eventi.</para>
          <para>affidabilità e Performance Monitor include anche Monitor affidabilità, uno snap-in MMC che tiene traccia delle modifiche al sistema e li confronta con le modifiche nella stabilità del sistema, fornendo una visualizzazione grafica della relazione.</para>
        </content>
      </section>
      <section address="BKMK_4">
        <title> Impostare i livelli di registrazione</title>
        <content>
          <para>se le informazioni che viene visualizzato nel registro del servizio Directory nel Visualizzatore eventi non sono sufficienti per la risoluzione dei problemi, aumentare i livelli di registrazione usando la voce del Registro di sistema appropriate nel <embeddedLabel>HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics</embeddedLabel>. </para>
          <para>per impostazione predefinita, i livelli di registrazione per tutte le voci sono impostati su <embeddedLabel>0</embeddedLabel>, che fornisce la quantità minima di informazioni. È il massimo livello di registrazione <embeddedLabel>5</embeddedLabel>. Aumentare il livello di una voce, eventi aggiuntivi essere registrati nel registro eventi del servizio Directory.</para>
          <para>è possibile utilizzare la procedura seguente per modificare il livello di registrazione per una voce di diagnostica.</para>
          <alert class="caution">
            <para>è consigliabile che non modificare direttamente il Registro di sistema a meno che non esiste alcuna altra alternativa. Le modifiche al Registro di sistema non vengono convalidate dall'editor del Registro di sistema o da Windows prima di applicarle, e di conseguenza, possono essere archiviati valori non corretti. Questo può causare errori irreversibili nel sistema. Se possibile, utilizzare criteri di gruppo o altri strumenti di Windows, ad esempio snap-in MMC, per eseguire attività, piuttosto che modificare direttamente il Registro di sistema. Se è necessario modificare il Registro di sistema, usare estrema cautela.</para>
          </alert>
          <para>
            <embeddedLabel>requisiti</embeddedLabel>
          </para>
          <list class="bullet">
            <listItem>
              <para>appartenenza al gruppo <embeddedLabel>Domain Admins</embeddedLabel>, o equivalente è il requisito minimo necessario per completare questa procedura. <token>review_detailincludes </para>
            </listItem>
            <listItem>
              <para>strumenti: Regedit.exe</para>
            </listItem>
          </list>
          <procedure>
            <title>per modificare il livello di registrazione per una voce di diagnostica</title>
            <steps class="ordered">
              <step>
                <content>
                  <para>fare clic su <ui>Start</ui>, fare clic su <ui>eseguire</ui>, tipo <userInput>regedit</userInput>e quindi fare clic su <ui>OK</ui>. </para>
                </content>
              </step>
              <step>
                <content>
                  <para>passare alla voce per il quale si desidera impostare la registrazione in <embeddedLabel>HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics</embeddedLabel>. </para>
                </content>
              </step>
              <step>
                <content>
                  <para>doppio clic sulla voce e in <embeddedLabel>Base</embeddedLabel>, fare clic su <embeddedLabel>decimale</embeddedLabel>. </para>
                </content>
              </step>
              <step>
                <content>
                  <para>In <embeddedLabel>valore</embeddedLabel>, digitare un numero intero da <embeddedLabel>0</embeddedLabel> tramite <embeddedLabel>5</embeddedLabel>, quindi fare clic su <ui>OK</ui>. </para>
                </content>
              </step>
            </steps>
          </procedure>
        </content>
      </section>
    </sections>
  </section>
  <relatedTopics />
</developerConceptualDocument>


