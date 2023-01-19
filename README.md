# Neptun-API
A Neptun Egyetemi Rendszer API dokumentációja

A dokumentáció rendszeres frissítés alatt van, ahogy haladok az endpointok tesztelésében úgy frissítem.

Néha ha valamivel haladok Twitterre kiposztolom, ha érdekel ahogy szenvedek check it out: https://twitter.com/GreG_Coding

Az endpoint-ok (és tbh minden ami itt található) a Neptun Androidos alkalmazásból lett *kimókolva*.

- A projekt elkezdésében sokat segített **@RuzsaGergely** valamint a **Poszeidon** proxy-ja: https://github.com/RuzsaGergely/Poszeidon (thanks pal)

Az egyetemek linkjeihez is lesz itt egy link amire POST requestet kell küldeni, viszont mivel az SSL tanúsítványa lejárt ezért vicces. Az egyetemedhez megfelelő linket egyelőre megtalálod a repo-ban lévő [GetInstitues.json](https://github.com/GreGamingHUN/Neptun-API/blob/main/GetInstitutes.json)-ból.

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
Visszaadja az hallgató bejövő üzeneteit.
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
Visszaadja a hallgató elküldött üzeneteit.
- **Adattag**: MessagesList

[//]: # (endoflist)
    Mivel életemben nem küldtem egy neptunos üzenetet sem, így ezt nem tudom dokumentálni rendesen, probalby ugyanaz, mint a GetMessages

## GetPeriodTerms
Visszaadja a szemesztereket
- **Adattag**: PeriodTermsList

[//]: # (endoflist)
    Szemeszter objektumokat ad vissza a következő adattagokkal:
- **Id**: A félév id-je
- **TermName**: A félév megnevezése

## GetPeriods
Visszaadja az adott szemeszter időszakait
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
Visszaadja a hallgató megadott félévben lévő felvett tárgyait
- **Adattag**: AddedSubjectsList

[//]: # (endoflist)
Plusz adattag amit a POST requesthez kell adni:
- **TermId**: A szemeszter id-je, amit a **GetPeriodTerms** segítségével kérhetsz le

[//]: # (endoflist)
Tárgy objektumokat ad vissza a következő adattagokkal:
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

## GetExams
Visszaadja az adott félév vizsgáit
- **Adattag**: ExamsList

[//]: # (endoflist)
Plusz adattagok amit a POST requesthez kell adni:
- **filter**: Ezen belül kell a következő adattagokat berakni:

[//]: # (endoflist)
- **ExamType**: A vizsga típusa, ```0```, ha bármi más string-et akarok megadni meghal az egész, inkább kérj le mindent, majd a lekért adatokat szűrd saját kóddal
- **Term**: Szemeszter, ```0```, ha az összes szemeszet vizsgáit le akarod kérni, egyébként pedig a **GetPeriodTerms**-el lekért szemeszter kódja (fun fact, a Neptun nem tárolja az előző félévek vizsgaidőpontjait, szóval kb mindig maradhat 0-n)
- **SubjectID**: Tantrágy ID, ```0```, ha az összes tárgy vizsgáját le akarod kérni, egyébként pedig a **GetAddedSubjects**-el lekért tárgy kódja
- **ExamStart**: Vizsga kezdési ideje, ```/Date(-62135596800000)/```, vagy a dátum, ahonnan nézve le akarod kérni a vizsgákat (pl. ```"Date(<A dátumod epoch formátumban>)"```)
- **ExamTypeSpinner**: i have no idea mit jelent, ```0```
- **IsFromSearch**: i have no idea, ```false```
- **SubjectName**: Tárgy neve, viszont ide tök mindegy mit adok meg, mindent kidob. Ezt csak visszadobja majd a POST request
- **CourseCode**: same
- **KurzusOktato**: i have no idea, a POST request semmit sem ad vissza ezzel kapcsolatban, akármit is adok meg

[//]: # (endoflist)
Vizsga objektumokkal ad vissza a következő adattagokkal:
- **CourseCode**: Kurzus kódja
- **CourseExam_ExamSigninID**: i have no idea
- **CourseID**: Kurzus id-je
- **ExamType**: Vizsga típusa (pl.: ```"CooSpace-es teszt", "Írásbeli"```)
- **FromDate**: A vizsga kezdésének ideje
- **SignedIn**: i have no idea, mindig **null**
- **SubjectCode**: Tárgy kódja
- **SubjectID**: tárgy id-je
- **SubjectName**: tárgy neve
- **ToDate**: Vizsga elkezdésének vége

## GetCurriculums
Visszaadja a hallgató mintatanterveit
- **Adattag**: CurriculumList

[//]: # (endoflist)
Tanterv objektumokat ad vissza a következő adattagokkal:
- **CurriculumName**: A mintatanterv neve
- **ID**: A mintatanterv id-je

## GetCourses
Visszaadja a az adott tárgy kurzusait
- **Adattag**: CourseList

[//]: # (endoflist)
Plusz adattagok amit a POST requesthez kell adni:
- **filter**: Ezen belül kell a következő adattagokat berakni:
- **Id**: Tantárgy id-je, amit a **GetSubjects**-el, vagy **GetAddedSubjects**-el tudsz lekérni
- **SubjectType**: ```0```, Bármi mást írsz meghal az egész
- **CurriculumID**: Mintatanterv id-je, amit a **GetCurriculums**-al tudsz lekérni
- **TermID**: Szemeszter id-je, amit a **GetTermPeriods**-el tudsz lekérni

[//]: # (endoflist)
Kurzus objektumokat ad vissza a következő adattagokkal:
- **CourseClass**: i have no idea, mindig ```null```
- **CourseCode**: Kurzus kódja
- **CourseTimeTableInfo**: Kurzus órarendi infói (pl.: ```"SZE:15:00-16:00(PL-169-3 - Pelda 169 PC-terem (IR-169-3))"```)
- **CourseTutor**: Kurzust tartó neve
- **CourseType**: kurzus típus id-je (?)
- **CourseType_DNAME**: kurzus típus neve
- **Id**: Kurzus id-je
- **IsJovahagyasos**: Jóváhagyásos-e a kurzus (```boolean```)
- **IsRangosoros**: Rangsoros-e a kurzus (```boolean```)
- **IsVarolistas**: Várólistás-e a kurzus (```boolean```)
- **Letszamok**: Létszámok (Fő/Várólista/Limit)
- **RangsorPontszamok**: i have no idea, probably a rangosoros jelentkezésnél a helyezésed

## GetSubjects
Visszaadja az adott félév tantárgyait
- **Adattag**: SubjectList

[//]: # (endoflist)
Plusz adattagok amit a POST requesthez kell adni:
- **filter**: Ezen belül kell a következő adattagokat berakni:
- **CurriculumID**: Mintatanterv id-je (```0```, ha nem akarsz mintatantervet megadni)
- **SubjectType**: i have no idea
- **TermId**: félév id-je
- **SubjectName**: ```null```, vagy a tárgy neve, amire keresni szeretnél (Úgy működik mint a Neptunos tárgykereső, ha félig írod be az is működik)
- **SubjectCode**: ```null```, vagy a tárgy kódja amire keresni szeretnél (Úgy működik mint a Neptunos tárgykereső, ha félig írod be az is működik)
- **CourseTutor**: ```null```, vagy az oktató akire keresni szeretnél (Úgy működik mint a Neptunos tárgykereső, ha félig írod be az is működik)
- **CourseCode**: ```null```, vagy a kurzus kódja amire keresni szeretnél (Úgy működik mint a Neptunos tárgykereső, ha félig írod be az is működik)

[//]: # (endoflist)
Van egy kis quirkje a dolognak, ugyanis az eddig használatos ```"CurrentPage: 0"``` egy ```Index -10 is either negative or above rows count.``` errort ad, szóval a ```CurrentPage``` adattaggal kell lépegetni az "oldalakon", hogy ne haljon meg

Tantárgy objektumokat ad vissza a következő adattagokkal:
- **Completed**: Teljesítetted-e a tárgyat (```boolean```)
- **Credit**: Hány kreditet ér a tárgy
- **CurriculumTemplateID**: A tárgy mintatantervének id-je
- **CurriculumTemplatelineID**: i have no idea
- **IsOnSubject**: Jelenleg fel vagy-e jelentkezve a tárgyra
- **SubjectCode**: A tárgy kódja
- **SubjectId**: A tárgy id-je
- **SubjectName**: A tárgy neve
- **SubjectRequirement**: A tárgy követelménye(?),  (pl.: ```"Kollokvium", "Gyakorlati jegy"```)
- **SubjectSignupType**: A tárgyfeliratkozás típusa (pl.: ```"Kötelező", "Kötelezően választható"```)
- **TermID**:  A tárgy félévének id-je

## GetTrainings
Visszaadja a hallgató képzéseit
- **Adattag**: TrainingList

[//]: # (endoflist)
Képzés objektumokat ad vissza a következő adattagokkal:
- **Code**: A képzés kódja
- **Description**: A képzés megnevezése
- **Id**: A képzés id-je

## GetCalendarData
Visszaadja a hallgató naptárban lévő eseményeit
- **Adattag**: calendarData
 
[//]: # (endoflist)
Plusz adattagok amit a POST requesthez kell adni:
- **needAllDaylong**: i have no idea (```boolean```, legyen ```true```)
- **Time**: i have no idea (```boolean```, legyen ```true```)
- **Exam**: Kilistázza-e a vizsgákat (```boolean```)
- **Task**: Kilistázza-e a teendőket (?) (```boolean```)
- **Apointment**: Kilistázza-e az egyeztetett időpntokat (?) (```boolean```, legyen ```true```)
- **RegisterList**: i have no idea (```boolean```, legyen ```true```)
- **Consultation**: i have no idea (```boolean```, legyen ```true```)
- **startDate**: Milyen dátumtól nézze az eseményeket (pl.: ```"/Date(1674136168)/"```, ahol a szám epoch formátumban van)
- **endDate**: Meddig nézze az eseményeket (pl.: ```"/Date(1674136168)/"```, ahol a szám epoch formátumban van)
- **entityLimit**: i have no idea (```int```, legyen ```0```)

[//]: # (endoflist)
Naptáradat objektumokat ad vissza a következő adattagokkal:
- **allDayLong**: i have no idea, nekem mindig ```false```
- **description**: az esemény leírása
- **end**: Meddig tart az esemény epoch formátumban
- **eventColor**: esemény színe a naptárban (```{ a: 255, b:45, g: 125, r: 0 }``` formátumban)
- **id**: Esemény id-je
- **location**: Esemény helye
- **start**: Esemény kezdete epoch formátumban
- **title**: Esemény címe
- **type**: i have no idea

## SetReadedMessage
A megadott üzenetet olvasottá teszi
- **Adattag**: nincs

[//]: # (endoflist)
Plusz adattagok amit a POST requesthez kell adni:
- **PersonMessageId**: Az üzenet id-je, amit a **GetMessages**-el tudsz lekérni

## Dokumentálatlan endpoint-ok
### Priority:
- SaveSubject
- GetExamDetails
- GetTermData
- RemoveMessage
- SetNewPassword


### Egyéb:

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
- GetCashinData
- RemoveSentMessage
- SendMessage
- GetMarkbookData
- RemoveExam
- SetExamSigning
- SetTrainings
- GetOrganizations
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
- GetMarkbookTermData
- GetGDPRInfo
- SetGDPRAcceptance

A jelentésüket valamint használatukat folyamatosan frissíteni fogom ahogy haladok velük

k thanks bye
