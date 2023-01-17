# Neptun-API
A Neptun Egyetemi Rendszer API dokumentációja

A dokumentáció rendszeres frissítés alatt van, ahogy haladok az endpointok tesztelésében úgy frissítem.

Az endpoint-ok (és tbh minden ami itt található) a Neptun Androidos alkalmazásból lett *kimókolva*.

Minden HTTP request-et POST-ként kell küldeni. Ha egyéb dolgot nem ír, akkor ezt a default query-t kell küldeni:

```
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
- **NeptunCode**: i have no idea, mindig üres string-et ad
- **PersonMessageId**: i have no idea
- **SendDate**: Üzenet küldési ideje epoch timestamp formátumban (konvertáláshoz link: https://www.epochconverter.com/)
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
- GetPeriods
- GetSubjects
- GetMarkbookData
- GetAddedSubjects
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
