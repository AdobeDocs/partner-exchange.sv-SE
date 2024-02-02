---
title: Åtkomst till den enhetliga profilen
description: Använd API:er för att komma åt den enhetliga profilen.
exl-id: c9d2fa2d-9ffe-4e66-996f-ad930bee22c6
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '675'
ht-degree: 0%

---

# Åtkomst till den enhetliga profilen med profilens API

Adobe [!DNL Experience Platform] har åtkomst till kundprofilen i realtid, [[!DNL Experience Platform] Kundprofil-API i realtid](https://adobe.ly/2TtDHWr) har utformats för att interagera med detta. Se det här [självstudiekurs](https://docs.adobe.com/content/help/en/experience-platform/profile/api/getting-started.html) för hur du får åtkomst till kundprofildata i realtid med hjälp av profils-API:t.

I den här artikeln hänvisas det i stort sett till den självstudiekurs som är länkad ovan.

The [Postman Collection](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman) refereras till i hela artikeln med tillhörande anrop per nummer. Mer information om hur du installerar och använder Postman-samlingen finns på Github [README](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) sida. Det finns också exempeldatauppsättningar med [lojalitet](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json) och [profil](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) data.

I det här avsnittet använder du Postman-mappen 5: Profilsökning, 5a: PROFILdata för realtidssökning ELLER 5b: EVENT-data för realtidssökning.

## Använda API:et

Följande avsnitt hjälper dig att autentisera dig för Experience Platform. Läs mer om API-sökväg, rubrikinformation med mera.

### Autentisera till [!DNL Platform]

Se [this](https://docs.adobe.com/content/help/en/experience-platform/tutorials/authentication.html) Självstudiekurs för autentisering innan du ringer något av samtalen nedan.

### API-sökväg

Plattformsgatewayens URL som krävs för kundprofilens API i realtid är: `https://platform.adobe.io/`

Bassökvägen för API är: `/data/core/ups/access/entities`

Ett exempel på en fullständig sökväg är: `https://platform.adobe.io/data/core/ups/access/entities`

### Rubrikinformation

Huvudet måste innehålla:

* Behörighet
* x-gw-ims-org-id - få åtkomst via console.adobe.io
* x-api-key - få åtkomst via console.adobe.io
* x-sandbox-name - hämtas från Adobe Integration Manager
* Content-Type: application/json

Mer information om rubriken finns i [självstudiekurs](https://adobe.ly/2PTHuKv).

## Få åtkomst till kundprofiler i realtid med hjälp av identiteter

Profil-API:t ger åtkomst till profiler med hjälp av identiteter via en GET-begäran. Avsnitten nedan följer detta [stödlinje](https://docs.adobe.com/content/help/en/experience-platform/profile/api/entities.html).

### Åtkomst till profildata med hjälp av identitet

API:t ger åtkomst till profilinformation med hjälp av identitet. Detta görs genom att en GET-begäran görs till /access/entities med enhets-ID som en av parametrarna och enhets-ID:ts namnutrymme. Obs! Tänk på att alla begäranden som returnerar 50 poster bara ger en 422 HTTP-status och ett meddelande som läser &quot;för många relaterade identiteter&quot; och sökningen måste begränsas med fler parametrar.

Begäran:

Följande begäran hämtar en kunds e-postadress och namn med hjälp av en identitet:

```
curl -X GET \
  'https://platform.adobe.io/data/core/ups/access/entities?schema.name=_xdm.context.profile&entityId=janedoe@example.com&entityIdNS=email&fields=identities,person.name,workEmail' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

Svar:

```
{
    "BVrqzwVv7o2p3naHvnsWpqZXv3KJgA": {
        "entityId": "BVrqzwVv7o2p3naHvnsWpqZXv3KJgA",
        "sources": [
            "1000000000"
        ],
        "entity": {
            "identities": [
                {
                    "id": "89149270342662559642753730269986316601",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "janedoe@example.com",
                    "namespace": {
                        "code": "email"
                    }
                },
                {
                    "id": "janesmith@example.com",
                    "namespace": {
                        "code": "email"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316604",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "58832431024964181144308914570411162539",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316602",
                    "namespace": {
                        "code": "ecid"
                    },
                    "primary": true
                }
            ],
            "person": {
                "name": {
                    "firstName": "Jane",
                    "middleName": "F",
                    "lastName": "Doe"
                }
            },
            "workEmail": {
                "primary": true,
                "address": "janedoe@example.com",
                "label": "Jane Doe",
                "type": "work",
                "status": "active"
            }
        },
        "lastModifiedAt": "2018-08-28T20:57:24Z"
    }
}
```

### Åtkomst till profiler per lista med identiteter

API:t ger åtkomst till profiler med hjälp av en lista över identiteter genom att använda en POST-begäran till slutpunkten /access/entities och tillhandahålla identiteterna i nyttolasten. Dessa identiteter består av ett ID-värde (entityId) och ett identity-namnutrymme (entityIdNS).

Begäran: Följande begäran hämtar namnen och e-postadresserna för flera kunder med hjälp av en lista över identiteter:

```
curl -X POST \
  https://platform.adobe.io/data/core/ups/access/entities \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "schema":{
        "name":"_xdm.context.profile"
    },
    "fields":["identities","person.name","workEmail"],
    "identities":[
        {
            "entityId":"89149270342662559642753730269986316601",
            "entityIdNS":{
                "code":"ECID"
            }
        },
        {
            "entityId":"89149270342662559642753730269986316900",
            "entityIdNS":{
                "code":"ECID"
            }
        },
        {
            "entityId":"89149270342662559642753730269986316602",
            "entityIdNS":{
                "code":"ECID"
            }
        }
    ]
}'
```

Respose: Ett lyckat svar returnerar de begärda fälten för entiteter som angetts i begärandetexten.

```
{
    "A29cgveD5y64ezlhxjUXNzcm": {
        "entityId": "A29cgveD5y64ezlhxjUXNzcm",
        "sources": [
            "1000000000"
        ],
        "entity": {
            "identities": [
                {
                    "id": "89149270342662559642753730269986316601",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "janedoe@example.com",
                    "namespace": {
                        "code": "email"
                    }
                },
                {
                    "id": "05DD23564EC4607F0A490D44",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316603",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "janesmith@example.com",
                    "namespace": {
                        "code": "email"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316604",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316700",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316701",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "58832431024964181144308914570411162539",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316602",
                    "namespace": {
                        "code": "ecid"
                    },
                    "primary": true
                }
            ],
            "person": {
                "name": {
                    "firstName": "Jane",
                    "middleName": "F",
                    "lastName": "Doe"
                }
            },
            "workEmail": {
                "primary": true,
                "address": "janedoe@example.com",
                "label": "Jane Doe",
                "type": "work",
                "status": "active"
            }
        },
        "lastModifiedAt": "2018-08-28T20:57:24Z"
    },
    "A29cgveD5y64e2RixjUXNzcm": {
        "entityId": "A29cgveD5y64e2RixjUXNzcm",
        "sources": [
            ""
        ],
        "entity": {},
        "lastModifiedAt": "1970-01-01T00:00:00Z"
    },
    "A29cgveD5y64ezphxjUXNzcm": {
        "entityId": "A29cgveD5y64ezphxjUXNzcm",
        "sources": [
            "1000000000"
        ],
        "entity": {
            "identities": [
                {
                    "id": "89149270342662559642753730269986316602",
                    "namespace": {
                        "code": "ecid"
                    },
                    "primary": true
                },
                {
                    "id": "janedoe@example.com",
                    "namespace": {
                        "code": "email"
                    }
                }
            ],
            "person": {
                "name": {
                    "firstName": "Jane",
                    "middleName": "F",
                    "lastName": "Doe"
                }
            },
            "workEmail": {
                "primary": true,
                "address": "janedoe@example.com",
                "label": "Jane Doe",
                "type": "work",
                "status": "active"
            }
        },
        "lastModifiedAt": "2018-08-27T23:25:52Z"
    }
}
```

## Tidsseriehändelser

Partners kan komma åt tidsseriehändelser via identiteten för den associerade profilentiteten genom att göra en GET-begäran till slutpunkten /access/entities.

### Åtkomst till tidsseriehändelser för en profil per identitet

Tidsseriehändelser nås av identiteten för deras associerade profilentitet genom att en GET-begäran görs till slutpunkten /access/entities. Den här identiteten består av ett ID-värde (entityId) och ett identitetsnamnutrymme (entityIdNS).

Begäran: I följande begäran hittas en profilentitet per ID och värdena för egenskaperna endUserID, web och channel hämtas **för alla** tidsseriehändelser som är associerade med entiteten.

```
curl -X GET \
  'https://platform.adobe.io/data/core/ups/access/entities?schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=89149270342662559642753730269986316900&relatedEntityIdNS=ECID&fields=endUserIDs,web,channel&startTime=1531260476000&endTime=1531260480000&limit=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

Svar:

Ett lyckat svar returnerar en numrerad lista över händelser i tidsserier och associerade fält som har angetts i parametrarna för begäran.

```
{
    "_page": {
        "orderby": "timestamp",
        "start": "c8d11988-6b56-4571-a123-b6ce74236036",
        "count": 1,
        "next": "c8d11988-6b56-4571-a123-b6ce74236037"
    },
    "children": [
        {
            "relatedEntityId": "A29cgveD5y64e2RixjUXNzcm",
            "entityId": "c8d11988-6b56-4571-a123-b6ce74236036",
            "timestamp": 1531260476000,
            "entity": {
                "endUserIDs": {
                    "_experience": {
                        "ecid": {
                            "id": "89149270342662559642753730269986316900",
                            "namespace": {
                                "code": "ecid"
                            }
                        }
                    }
                },
                "channel": {
                    "_type": "web"
                },
                "web": {
                    "webPageDetails": {
                        "name": "Fernie Snow",
                        "pageViews": {
                            "value": 1
                        }
                    }
                }
            },
            "lastModifiedAt": "2018-08-21T06:49:02Z"
        }
    ],
    "_links": {
        "next": {
            "href": "/entities?start=c8d11988-6b56-4571-a123-b6ce74236037&orderby=timestamp&schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=89149270342662559642753730269986316900&relatedEntityIdNS=ECID&fields=endUserIDs,web,channel&startTime=1531260476000&endTime=1531260480000&limit=1"
        }
    }
}
```

### Sidindelning för tidsseriehändelser för en profil

Resultaten sidnumreras när tidsseriehändelser hämtas. Om det finns efterföljande resultatsidor kommer svarets _page.next-parameter att innehålla ett ID. Dessutom innehåller svarets _links.next.href-parameter en URI med en begäran om att hämta efterföljande sida.

Begäran:

Följande begäran hämtar nästa resultatsida genom att använda URI:n _links.next.href som begärandesökväg.

```
curl -X GET \

  'https://platform.adobe.io/data/core/ups/access/entities?start=c8d11988-6b56-4571-a123-b6ce74236037&orderby=timestamp&schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=89149270342662559642753730269986316900&relatedEntityIdNS=ECID&fields=endUserIDs,web,channel&startTime=1531260476000&endTime=1531260480000&limit=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

Svar:

Ett godkänt svar returnerar nästa resultatsida. I det här exemplet visas ett svar där det inte finns några efterföljande resultatsidor, vilket anges av de tomma strängvärdena för _page.next och _links.next.href.

```
{
    "_page": {
        "orderby": "timestamp",
        "start": "c8d11988-6b56-4571-a123-b6ce74236037",
        "count": 1,
        "next": ""
    },
    "children": [
        {
            "relatedEntityId": "A29cgveD5y64e2RixjUXNzcm",
            "entityId": "c8d11988-6b56-4571-a123-b6ce74236037",
            "timestamp": 1531260477000,
            "entity": {
                "endUserIDs": {
                    "_experience": {
                        "ecid": {
                            "id": "89149270342662559642753730269986316900",
                            "namespace": {
                                "code": "ecid"
                            }
                        }
                    }
                },
                "channel": {
                    "_type": "web"
                },
                "web": {
                    "webPageDetails": {
                        "name": "Fernie Snow",
                        "pageViews": {
                            "value": 1
                        }
                    }
                }
            },
            "lastModifiedAt": "2018-08-21T06:50:01Z"
        }
    ],
    "_links": {
        "next": {
            "href": ""
        }
    }
}
```

## Referensartiklar

* [Kundprofil-API i realtid](https://adobe.ly/2TtDHWr)
* [Få åtkomst till kundprofildata i realtid med självstudiekursen för profil-API](https://docs.adobe.com/content/help/en/experience-platform/profile/api/getting-started.html)
* [[!DNL Experience Platform] Autentiseringshandbok](https://docs.adobe.com/content/help/en/experience-platform/tutorials/authentication.html)
