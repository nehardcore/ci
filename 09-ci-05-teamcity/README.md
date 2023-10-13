# Домашнее задание к занятию 11 «Teamcity»  
  
## Подготовка к выполнению  
  
1. В Yandex Cloud создайте новый инстанс (4CPU4RAM) на основе образа `jetbrains/teamcity-server`.  
2. Дождитесь запуска teamcity, выполните первоначальную настройку.  
3. Создайте ещё один инстанс (2CPU4RAM) на основе образа `jetbrains/teamcity-agent`. Пропишите к нему переменную окружения `SERVER_URL: "http://<teamcity_url>:8111"`.  
4. Авторизуйте агент.  
5. Сделайте fork [репозитория](https://github.com/aragastmatb/example-teamcity).  
6. Создайте VM (2CPU4RAM) и запустите [playbook](./infrastructure).  
<img width="1318" alt="image" src="https://github.com/nehardcore/ci/assets/97674120/ec5cec07-b0ab-4834-8dfb-3c4b5ddfa7c6">  
  
  
  
## Основная часть  
  
1. Создайте новый проект в teamcity на основе fork.  
2. Сделайте autodetect конфигурации.  
3. Сохраните необходимые шаги, запустите первую сборку master.  
<img width="1153" alt="image" src="https://github.com/nehardcore/ci/assets/97674120/de1896e3-4b64-4d7e-93e1-9c7e81335042">


4. Поменяйте условия сборки: если сборка по ветке `master`, то должен происходит `mvn clean deploy`, иначе `mvn clean test`.  
5. Для deploy будет необходимо загрузить [settings.xml](./teamcity/settings.xml) в набор конфигураций maven у teamcity, предварительно записав туда креды для подключения к nexus.  
<img width="499" alt="image" src="https://github.com/nehardcore/ci/assets/97674120/fc95fdb9-bca1-42eb-b648-4d77a86a1b42">
<img width="691" alt="image" src="https://github.com/nehardcore/ci/assets/97674120/5c547841-0f5c-4617-b30d-445293277adb">


6. В pom.xml необходимо поменять ссылки на репозиторий и nexus.  
7. Запустите сборку по master, убедитесь, что всё прошло успешно и артефакт появился в nexus.  
<img width="1148" alt="image" src="https://github.com/nehardcore/ci/assets/97674120/150556a3-ddac-423a-bd03-270e187e12fb">
<img width="786" alt="image" src="https://github.com/nehardcore/ci/assets/97674120/d12deaf4-9598-42a5-a8af-d627ad23df16">



8. Мигрируйте `build configuration` в репозиторий.  
9. Создайте отдельную ветку `feature/add_reply` в репозитории.  
10. Напишите новый метод для класса Welcomer: метод должен возвращать произвольную реплику, содержащую слово `hunter`.  
11. Дополните тест для нового метода на поиск слова `hunter` в новой реплике.  
12. Сделайте push всех изменений в новую ветку репозитория.  
13. Убедитесь, что сборка самостоятельно запустилась, тесты прошли успешно.  
<img width="1133" alt="image" src="https://github.com/nehardcore/ci/assets/97674120/2e94b834-e30c-42a4-970c-c3b7032849e8">


14. Внесите изменения из произвольной ветки `feature/add_reply` в `master` через `Merge`.  
15. Убедитесь, что нет собранного артефакта в сборке по ветке `master`.  
```zsh
cch@MBP-Costas teamcity-example % git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
cch@MBP-Costas teamcity-example % git merge feature/add_reply
Updating 185ffa7..3bb57e8
Fast-forward
 src/main/java/plaindoll/Welcomer.java     | 3 +++
 src/test/java/plaindoll/WelcomerTest.java | 4 ++++
 2 files changed, 7 insertions(+)
```

16. Настройте конфигурацию так, чтобы она собирала `.jar` в артефакты сборки.  
17. Проведите повторную сборку мастера, убедитесь, что сбора прошла успешно и артефакты собраны.  
<img width="1082" alt="image" src="https://github.com/nehardcore/ci/assets/97674120/e043a7ba-3b12-4feb-8e65-c998ab2435c2">  
<img width="407" alt="image" src="https://github.com/nehardcore/ci/assets/97674120/7783504c-145d-4350-ad54-3136027e0607">


18. Проверьте, что конфигурация в репозитории содержит все настройки конфигурации из teamcity.  
19. В ответе пришлите ссылку на репозиторий.  
Teamcity-example: https://github.com/nehardcore/teamcity-example
Homework: https://github.com/nehardcore/ci/blob/main/09-ci-05-teamcity/README.md

  
---  
  
### Как оформить решение задания  
  
Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.  
  
---  
