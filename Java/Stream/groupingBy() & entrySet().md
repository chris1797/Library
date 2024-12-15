# groupingBy(), entrySet() 활용

두 List의 date를 기준으로 그룹핑 후 공통 클래스로 매핑

<br/>

```java
List<DiaryDto> diaryList = diaryDao.fetchUserDiary(request.getUserNo());
List<CalendarVO> calendarList = diaryList.parallelStream()
                .collect(Collectors.groupingBy(diary -> diary.getDate().format(DateTimeFormatter.ofPattern("yyyy-MM-dd"))))
                .entrySet().stream()
                .map(entry -> new CalendarVO()
                        .setDate(entry.getKey())
                        .setDiary(entry.getValue()))
                .collect(Collectors.toList());

List<ChurchDiaryDto> churchDiaryList = churchDiaryService.fetchActiveChurchDiary(user.getChurchNo());
List<CalendarVO> churchCalendarList = churchDiaryList.parallelStream()
                .collect(Collectors.groupingBy(churchDiary -> churchDiary.getDate().format(DateTimeFormatter.ofPattern("yyyy-MM-dd"))))
                .entrySet().stream()
                .map(entry -> new CalendarVO()
                        .setDate(entry.getKey())
                        .setChurchDiary(entry.getValue()))
                .collect(Collectors.toList());

calendarList.addAll(churchCalendarList);

return calendarList.stream()
            .sorted(Comparator.comparing(o -> LocalDateTime.parse(o.getDate())))
            .collect(Collectors.toList());
```
