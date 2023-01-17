# Neptun-API
A Neptun Egyetemi Rendszer API dokumentációja

A dokumentáció rendszeres frissítés alatt van, ahogy haladok az endpointok tesztelésében úgy frissítem.

Néha ha valamivel haladok Twitterre kiposztolom, ha érdekel ahogy szenvedek check it out: https://twitter.com/GreG_Coding

Az endpoint-ok (és tbh minden ami itt található) a Neptun Androidos alkalmazásból lett *kimókolva*.

- A projekt elkezdésében sokat segített **@RuzsaGergely** valamint a **Poszeidon** proxy-ja: https://github.com/RuzsaGergely/Poszeidon (thanks pal)

Az egyetemek linkjeihez is lesz itt egy link amire POST requestet kell küldeni, viszont mivel az SSL tanúsítványa lejárt ezért vicces. Az egyetemedhez megfelelő linket egyelőre megtalálod a repo-ban lévő GetInstitues.json-ból.

A link így néz ki: <br>
```https://<neptun-link>/<hallgato-api>/MobileService.msc/```

Az endpoint-okat a linkek végére kell írni. A végeredmény:<br>
```https://<neptun-link>/<hallgato-api>/MobileService.msc/GetMessages```

Minden HTTP request-et POST-ként kell küldeni. Ha egyéb dolgot nem ír, akkor ezt a default query-t kell küldeni:

```json
{
    "TotalRowCount": -1,
    "ExceptionsEnum": 0,
    "UserLogin": "dummy",
    "Password": "dummypwd",
    "NeptunCode": "",
    "CurrentPage": 0,
    "StudentTrainingID": null,
    "LCID": 1038,
    "ErrorMessage": "",
    "MobileVersion": "1.5"
}
```
Jelentésuk:
- **TotalRowCount**: i have no idea
- **ExceptionsEnum**: i have no idea
- **UserLogin**: Neptunkód
- **Password**: Neptunos jelszó
- **CurrentPage**: Ha több oldalas a lekérés, actually még nem tudtam hol használni
- **StudentTrainingID**: i have no idea
- **LCID**: nyelvkód, az 1038 a magyar
- **ErrorMessage**: Ha hiba van ide adja vissza, hogy nekem ide miért kell küldenem bármit is azt nem tudom
- **MobileVersion**: A mobil alkalmazás verziója, i guess ha frissül a mobil app akk át kell írni

Minden api call egy json-al tér vissza, az érdemleges információkat tartozó adattagokat odaírom az endpointok dokumentációihoz

Itt az összes endpoint amit találtam:
## GetMessages
- **Adattag**: MessagesList

[//]: # (endoflist)
    Üzenet objektumokat ad vissza a következő adattagokkal:
- **Detail**: Az üzenet tartalma
- **Id**: Az üzenet id-je
- **IsNew**: i have no idea
- **Name**: Küldő
- **NeptunCode**: i have no idea, mindig üres stringet ad
- **PersonMessageId**: i have no idea
- **SendDate**: Üzenet küldési ideje epoch timestamp formátumban
- **Subject**: Az üzenet témája/címe

## GetSentMessages
- **Adattag**: MessagesList

[//]: # (endoflist)
    Mivel életemben nem küldtem egy neptunos üzenetet sem, így ezt nem tudom dokumentálni rendesen, probalby ugyanaz, mint a GetMessages

## GetPeriodTerms
- **Adattag**: PeriodTermsList

[//]: # (endoflist)
    Szemeszter objektumokat ad vissza a következő adattagokkal:
- **Id**: A félév id-je
- **TermName**: A félév megnevezése

## GetPeriods
- **Adattag**: PeriodList

[//]: # (endoflist)
Plusz adattag amit a POST requesthez kell adni:
- **PeriodTermID**: A szemeszter id-je, amit a **GetPeriodTerms** segítségével kérhetsz le


[//]: # (endoflist)
Időszak objektumokat ad vissza a következő adattagokkal:
- **FromDate**: Mikortól kezdődik az időszak
- **OrgAdmins**: i have no idea, mindig öres stringet ad
- **PeriodName**: Időszak neve
- **ToDate**: Meddig tart az időszak
- **TrainingTermIntervalId**: i have no idea, valószínűleg szimplán az időszak id-je

## GetAddedSubjects
- **Adattag**: AddedSubjectsList

[//]: # (endoflist)
Plusz adattag amit a POST requesthez kell adni:
- **TermId**: A szemeszter id-je, amit a **GetPeriodTerms** segítségével kérhetsz le

[//]: # (endoflist)
Tárgy objektumokat ad vissza a következő adattagokkal?
- **Courses**: Egy tömb, amiben a kurzusok adatai találhatók, még nem tudom hogyan kell használni
- **CurriculumtemplateID**: A tanterv id-je, amiben benne van
- **SubjectCode**: A tárgy kódja
- **SubjectComplianceResult**: A tárgy eredménye. Üres string, ha nincs jegy, ha akkor string, pl: ```"Jó (4)", "Közepes (3)"``` 
- **SubjectComplianceResultType**: i have no idea
- **SubjectCredit**: A tárgy kreditértéke
- **SubjectID**: A tárgy id-je
- **SubjectName**: A tárgy neve
- **SubjectRequirement**: A tárgy követelménye(?), pl ```"Kollokvium", "Gyakorlati jegy"```
- **SubjectType**: A tárgy típusa, pl ```"Kötelező", "Kötelezően választható", "Szabadon választható"```
- **TermId**: A félév id-je (fogalmam sincs miért adja vissza, amikor azt nekünk kellett megadni)

## Dokumentálatlan endpoint-ok
- GetPartners
- RemovePartners
- GetDirectoryGroups
- AddNewPartner
- GetForums
- GetFavouriteForums
- GetVirtualSpaceForums
- GetForumPosts
- AddToFavouriteFourm
- RemoveFromFavouriteForum
- AddNewForumPost
- GetCalendarData
- GetCashinData
- RemoveMessage
- RemoveSentMessage
- SendMessage
- GetSubjects
- GetMarkbookData
- GetTrainings
- GetExams
- GetExamDetails
- RemoveExam
- SetExamSigning
- SetTrainings
- GetOrganizations
- SetReadedMessage
- GetCourses
- GetCurriculums
- SaveSubject
- DoCashinData
- CheckCashinData
- ImpositionGetAllowedPaymentTypes
- ImpositionGetTerms
- ImpositionGetFeesAndSubjects
- ImpositionInsertNew
- CalendarNewAppointment
- GetOrganizationEventDetails
- GetTutorClassEventDetails
- GetStudentClassEventDetails
- GetTutorExamEventDetails
- GetStudentExamEventDetails
- GetTutorTaskEventDetails
- GetStudentTaskEventDetails
- GetMeetingEventDetails
- GetRegisterListEventDetails
- GetFilesToDocument
- SetNewPassword
- GetTermData
- GetMarkbookTermData
- GetGDPRInfo
- SetGDPRAcceptance

A jelentésüket valamint használatukat folyamatosan frissíteni fogom ahogy haladok velük

k thanks bye
