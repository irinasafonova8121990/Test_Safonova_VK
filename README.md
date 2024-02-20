# Test_Safonova_VK
## Назначение инструкции
Цель инструкции — пояснение процесса и методов диагностики ошибок при создании кластера Kubernetes с использованием веб-интерфейса и инструментов, таких как консоль разработчика, объекты magnum и heat в OpenStack. Инструкция состоит из:
* [глоссария] (),
* описания настройки,
* перечисления дополнительных средств для поиска и анализа ошибок в Kubernetes.

## Глоссарий
|  Термин 	| Определение 	| 
|:---	|:---
| **Кластер** | Совокупность master-node и worker-node. Кластер создается на основе шаблона кластера. |
| **Контейнер** |
| **Служба оркестрации (heat)** | Оркестрация на базе шаблонов для автоматического создания и провижининга ресурсов. |
| **Служба управления инфраструктурой контейнеров (magnum)** | Служба API OpenStack, предоставляющая платформы по оркестрации контейнерами, такие как Docker Swarm, Kubernetes и Mesos, как ресурсы первого уровня OpenStack. |  
| **Kubernetes (K8s)** |  Портативная расширяемая платформа с открытым исходным кодом для управления контейнеризованными рабочими нагрузками и сервисами, которая облегчает как декларативную настройку, так и автоматизацию. |
| **OpenStack** | Комплекс проектов свободного программного обеспечения, который может быть использован для создания инфраструктурных облачных сервисов и облачных хранилищ, при том как публичных, так и частных. |
| **Worker Node** | Сервер, на котором работают два главных процесса K8s, благодаря которым запускаются и работают контейнеризированные приложения. |
## Настройка
1. Откройте веб-интерфейс Kubernetes и создайте кластера Kubernetes выбранной версии.
2. При ошибках на этапе создания кластера в веб-интерфейсе откройте консоль разработчика по клавише F12 и проанализируйте ошибки.
3. Проверьте квоты проекта. Если ошибка в консоли указывает на их недостаток, то при необходимости увеличьте квоты или удалите ненужные объекты в проекте.
4. Проверьте, что создались объекты magnum-кластера и heat-кластера после начала создания кластера.
5. Проверьте, что создались объекты OpenStack, в том числе:
   * сети,
   * порты,
   * балансировщики нагрузки (loadbalancer),
   * диски,
   * инстансы.
8. Для диагностики и устранения ошибок на этапах создания объектов OpenStack используйте инструменты Heat.

## Дополнительные средства для поиска и анализа ошибок в Kubernetes
Дополнительно можно использовать следующие средства:
| Средство | Назначение
|:---	|:--- 
| **Alertmanager** |  Компонент Prometheus. Компонент обрабатывает оповещения, отправляемые сервером Prometheus, и отправляет уведомления в соответствии с настройками маршрутизации.
| **Custom Metrics** |  Пользовательские метрики, которые можно собирать и экспонировать через специальные агенты или SDK.
| **Elastic Stack (ELK Stack)**|  Аббревиатура, используемая для описания стека из трех популярных проектов: Elasticsearch, Logstash и Kibana. Стек ELK, зачастую именуемый Elasticsearch, предоставляет возможность собирать журналы всех ваших систем и приложений, анализировать их и создавать визуализации, чтобы мониторить приложения и инфраструктуры, быстрее устранять неполадки, анализировать систему безопасности и многое другое.
| **Grafana** |  Свободная программная система визуализации данных, ориентированная на данные систем ИТ-мониторинга. Реализована как веб-приложение в стиле «приборных панелей» с диаграммами, графиками, таблицами, предупреждениями.
| **Kubernetes Events** |   Функционал, позволяющий просматривать события в Kubernetes. Функционал вызывается по команде `kubectl get events`.
| **Metrics Server**    |
| **Prometheus**        |   
