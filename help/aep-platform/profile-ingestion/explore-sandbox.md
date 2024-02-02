---
title: Få åtkomst till och utforska AEP-sandlådan
description: Lär dig hur du kommer åt och utforskar sandlådan Experience Platform.
exl-id: 62c21615-4b03-4900-a1ad-8f809c836491
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '737'
ht-degree: 0%

---

# Få åtkomst till och utforska AEP-sandlådan

Denna artikel omfattar följande:

* Skillnaderna mellan en befintlig partnersandlådeorganisation för Adobe Exchange och den delade AEP-sandlådan.
* Begär åtkomst till den delade AEP-sandlådan.
* Tar emot en e-postinbjudan till AEP-sandlådan.
* Bjuda in nya användare i [!DNL Admin Console].
* Navigera i AEP-gränssnittet.

En allmän översikt över sandlådeteknologin i AEP finns här [artikel](https://docs.adobe.com/content/help/en/experience-platform/sandbox/home.html).

## Den delade AEP-sandlådan

Exchange-partners får åtkomst till olika Adobe [!DNL Experience Cloud] produkter (icke-AEP-produkter som [!DNL Analytics], [!DNL Target], plattformstaggar och så vidare) via sin egen Adobe [!DNL Experience Cloud] Org (ej delad). Partners får systemadministratörsbehörighet för sin egen organisation för att hantera användare och andra behörigheter. Adobe [!DNL Experience Platform] (AEP) behandlas annorlunda än andra Adobe sandlådor. Här är de viktigaste skillnaderna:

* Åtkomst till AEP kommer INTE att ske via partners Adobe [!DNL Experience Cloud] sandbox Org.
* Åtkomst till AEP sker via en delad Adobe Exchange-organisation.
* Många andra partnerföretag i Adobe Exchange använder AEP med samma organisation
   * Via AEP-sandlådefunktionen kan data och aktiviteter i den här delade organisationen inte ses eller ändras av de andra partnerna. Varje partner har tillgång till en annan sandlåda i den delade organisationen.
* Administrationsrättigheterna i den här delade organisationen är mycket begränsade.
* Efter att ha fått tillgång till en sandlåda på AEP, kommer partners att se två Orgs på Org-väljaren i det övre högra hörnet av användargränssnittet när de är på Admin Console eller Experience Cloud huvudstartsidan. När du loggar in på AEP bör bara den delade organisationen vara synlig.

## Begär åtkomst till den delade AEP-sandlådan

Skicka en [supportförfrågan](https://adobeexchangeec.zendesk.com/hc/se-sv/requests/new) med följande information:

* E-postadress
* Angående: AEP Sandbox-begäran
* Produkt: General Provisioning/Sandbox
* Biljetyp: Programsupport - Exchange-program / Frågor om provisioneringsbegäran
* Beskrivning: Ange en kort beskrivning av användningsfall för integrering som kräver att en AEP-sandlåda används
* Var noga med att även ange alla användarnamn och e-postmeddelanden som ska läggas till i AEP-sandlådan. Ytterligare användare kan läggas till efter att begäran har gjorts, men Adobe måste lägga till dem via en extra biljett (se nedan).

## Ta emot e-postinbjudan

Den primära kontakten som begärde AEP-sandlådan får ett automatiskt e-postmeddelande med en inbjudan att&quot;komma igång&quot; med Adobe [!DNL Experience Platform]. Den primära kontakten har även vissa administrationsbehörigheter som beskrivs i nästa avsnitt.

I stället för att klicka på knappen&quot;kom igång&quot; i e-postmeddelandet går du direkt till `https://platform.adobe.com.` Logga in med den Adobe ID som är associerad med e-postadressen i inbjudan eller skapa en om den inte är associerad med en Adobe ID.

## Bjud in ytterligare användare

Skicka en [supportförfrågan](https://adobeexchangeec.zendesk.com/hc/se-sv/requests/new) med följande information:

* E-postadress till begärande
* Ämne: AEP Sandbox - Lägg till administratör/användare
* Produkt: General Provisioning/Sandbox
* Biljetyp: Programsupport - Exchange-program / Frågor om provisioneringsbegäran
* Beskrivning: Lista med användare som ska läggas till (namn och e-post)

## Navigera i AEP-gränssnittet

Titta på AEP-gränssnittet [introduktionsvideo](https://docs.adobe.com/content/help/en/platform-learn/tutorials/intro-to-platform/interface-tour.html)

Det finns tolv primära områden i AEP-gränssnittet som kan navigeras via den vänstra panelen. De viktigaste avsnitten för den här typen av integration är Scheman, Datauppsättningar och Profiler.

* Hem - landningsskärmen

   * Föreslår några komma igång-aktiviteter
   * Innehåller länkar till utbildningsmaterial
   * Ger en instrumentpanelsvy för några av de viktigaste AEP-objekten, till exempel Scheman, Datauppsättningar och Profiler

* Arbetsflöden - starta in i gemensamma arbetsflöden för att lägga in data i AEP
* Anslutningar/källor - hantera datakällor som kommer in i AEP
* Anslutningar/mål - hantera anslutningarna för att skicka data till externa system
* Profiler - visa och hantera enskilda kundprofiler
* Segment - bläddra bland, skapa och ändra kundsegment
* Identiteter - bläddra, skapa och ändra identitetsnamnutrymmen; det här är de typer av primära ID som används för att unikt identifiera en kund
* Modeller (datavetenskap) - deltar i datavetenskapliga verksamheter, inklusive användning av en inbäddad Jupyter-miljö för bärbara datorer
* Tjänster (datavetenskap) - publicera datavetenskapningar som tjänster
* Scheman - bläddra, skapa och ändra scheman; det här är de detaljerade datadefinitioner som används för att hålla ordning på data
* DataSets - bläddra, skapa och hantera DataSets; en DataSet definieras av ett schema och är där data finns i AEP
* Frågor - söka, skapa, ändra och använda en databas med frågor för att få insikter från data i DataSets
* Övervakning - visa status för alla data som flyttas in och ut från AEP för både Batch och Streaming
