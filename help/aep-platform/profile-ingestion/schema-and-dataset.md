---
title: Skapa AEP-scheman och datauppsättningar
description: Skapa scheman och datauppsättningar i Experience Platform.
exl-id: a2773551-20a3-4a5b-ab53-60fa67e38ec0
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 1%

---

# Skapa scheman och datauppsättningar

The [Postman Collection](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman) refereras till i hela artikeln med tillhörande anrop per nummer. Mer information om hur du installerar och använder Postman-samlingen finns på Github [README](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) sida. Det finns också exempeldatauppsättningar med [lojalitet](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json) och [profil](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) data.

## Scheman

Ett schema är en uppsättning regler som representerar och validerar datastrukturen och dataformatet. På en hög nivå ger scheman en abstrakt definition av ett objekt i verkligheten (till exempel en person) och ger en översikt över vilka data som ska inkluderas i varje instans av objektet (till exempel förnamn, efternamn, födelsedag o.s.v.). Scheman kan skapas i användargränssnittet eller med [!DNL Experience Platform] API.

Se [den här dokumentationen](https://www.adobe.io/apis/experienceplatform/home/xdm/xdmservices.html#!api-specification/markdown/narrative/technical_overview/schema_registry/schema_composition/schema_composition.md) för mer information.

### Skapa ett schema

Partners kan skapa ett schema med användargränssnittet genom att följa detta [självstudiekurs](https://docs.adobe.com/content/help/en/experience-platform/xdm/tutorials/create-schema-ui.html). I det här exemplet används profilschemat för bonusprogram. Exemplet är ett profilschema, men händelsebaserade scheman kan användas med en liknande process.

För att kunna använda API:erna måste partners ha en befintlig Adobe I/O-integrering med [!DNL Experience Platform] behörigheter är aktiverade. Se den här guiden för [skapa en I/O-integrering](https://www.adobe.io/apis/experienceplatform/home/tutorials/alltutorials.html#!api-specification/markdown/narrative/tutorials/authenticate_to_acp_tutorial/authenticate_to_acp_tutorial.md).

Besök sedan [den här länken](https://docs.adobe.com/content/help/en/experience-platform/xdm/tutorials/create-schema-api.html) om du vill lära dig hur du skapar scheman med API:t.

Om du vill skapa ett schema via Postman använder du anropen i mapparna 1: Skapa schema, 1a: Skapa schema för PROFILE-data ELLER 1b: Skapa schema för EVENT-data.

## Datauppsättningar

Alla data som förs in i Adobe [!DNL Experience Platform] finns i datauppsättningar. En datauppsättning är en lagrings- och hanteringskonstruktion för en datamängd, vanligtvis en tabell, som innehåller ett schema (kolumner) och fält (rader). Datauppsättningar innehåller också metadata som beskriver olika aspekter av de data som lagras.

Katalogtjänsten är systemet för registrering av dataplatser och -rader inom [!DNL Experience Platform]och används för att skapa och hantera datauppsättningar. Katalogen spårar metadata för varje datauppsättning, som innehåller en referens till XDM-schemat (Experience Data Model) som datauppsättningen följer (förklaras i nästa avsnitt) och antalet poster som hämtas till den datauppsättningen.

Gå [här](https://docs.adobe.com/content/help/en/experience-platform/catalog/datasets/overview.html) om du vill ha en detaljerad översikt över datauppsättningen.

### Skapa en datauppsättning

![Skapar Animerad GIF för datauppsättning](images/creating_a_dataset.gif)

<!-- 
We don't yet support hover text in images (and we render it poorly when included). I removed "Creating a Dataset" from the above image link. We can add it back when we support it (Summer 2020?) -Bob
-->

Skapa en datauppsättning via användargränssnittet:

1. Klicka på **[!UICONTROL Create Dataset]**.

1. Klicka på **[!UICONTROL Create from schema]**.

1. Klicka på **[!UICONTROL Finish]**.

Gå [här](https://docs.adobe.com/content/help/en/experience-platform/catalog/datasets/user-guide.html) för en användarhandbok för datauppsättningar.

[Skapa en datauppsättning med API:erna](https://docs.adobe.com/content/help/en/experience-platform/catalog/datasets/create.html).

Om du vill skapa en datauppsättning via Postman använder du mapparna 2: Skapa datauppsättning, 2a: Skapa datauppsättning för PROFILE-data ELLER 2b: Skapa datauppsättning för EVENT-data.

## Bästa praxis för partners för schema och datamängd

* Partnerdata bör använda ett separat profilschema jämfört med att skapa en mix-in för en kunds befintliga profilschema och upplevelseschema.
* Partners bör om möjligt använda Adobe-klasser och mixins.
* Partners bör överföra sina data via en separat datauppsättning i stället för att försöka kombinera deras data till en befintlig datauppsättning.
* Partners kan för tillfället inte överföra sina scheman till det globala registret.
