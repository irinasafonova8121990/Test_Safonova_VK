# Test_Safonova_VK (OpenStack)
## Назначение инструкции
Цель инструкции — пояснение процесса и методов диагностики ошибок при создании кластера Kubernetes с использованием веб-интерфейса и инструментов, таких как консоль разработчика, объекты magnum и heat в OpenStack. Инструкция состоит из:
* [глоссария](https://github.com/irinasafonova8121990/Test_Safonova_VK/blob/main/README.md#%D0%B3%D0%BB%D0%BE%D1%81%D1%81%D0%B0%D1%80%D0%B8%D0%B9),
* [описания взаимодействия Kubernetes и OpenStack](https://github.com/irinasafonova8121990/Test_Safonova_VK#%D0%BA%D0%B0%D0%BA-%D0%B2%D0%B7%D0%B0%D0%B8%D0%BC%D0%BE%D0%B4%D0%B5%D0%B9%D1%81%D1%82%D0%B2%D1%83%D1%8E%D1%82-kubernetes-%D0%B8-openstack),
* [описания настройки кластера Kubernetes](https://github.com/irinasafonova8121990/Test_Safonova_VK/blob/main/README.md#%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-%D0%BA%D0%BB%D0%B0%D1%81%D1%82%D0%B5%D1%80%D0%B0-%D0%B2-kubernetes),
* [перечисления дополнительных средств для поиска и анализа ошибок в Kubernetes](https://github.com/irinasafonova8121990/Test_Safonova_VK/blob/main/README.md#%D0%B4%D0%BE%D0%BF%D0%BE%D0%BB%D0%BD%D0%B8%D1%82%D0%B5%D0%BB%D1%8C%D0%BD%D1%8B%D0%B5-%D1%81%D1%80%D0%B5%D0%B4%D1%81%D1%82%D0%B2%D0%B0-%D0%B4%D0%BB%D1%8F-%D0%BF%D0%BE%D0%B8%D1%81%D0%BA%D0%B0-%D0%B8-%D0%B0%D0%BD%D0%B0%D0%BB%D0%B8%D0%B7%D0%B0-%D0%BE%D1%88%D0%B8%D0%B1%D0%BE%D0%BA-%D0%B2-kubernetes).
## Глоссарий
|  Термин 	| Определение 	| 
|:---	|:--- 
| **Балансировщик нагрузки (loadbalancer)** | Средство для распределения входящего трафика между несколькими виртуальными бэкенд-серверами. |
| **БД** | База данных. |
|**Виртуальная машина (ВМ)**| Программная и/или аппаратная система, эмулирующая аппаратное обеспечение компьютера. ВМ исполняет программы для guest-платформы на host-платформе, виртуализирует и создает среды платформы, которые изолируют друг от друга программы. ВМ состоит из виртуального процессора, памяти, жестких дисков и сетевого адаптера. ВМ запускает одну операционную систему внутри другой. |
|**Вычислительные ресурсы**| Виртуальные процессоры и ОЗУ. |
| **Диск** | Физическое или виртуальное хранилище данных. |
| **Инстанс** | Набор контейнеров или ВМ, которые связаны с одним горизонтально масштабируемым сервисом. |
| **Квоты** | Применяемые к проекту ограничения на использование виртуальных вычислительных ресурсов и создание объектов в конкретных сервисах. |
| **Кластер Kubernetes** | Совокупность master-node и worker-node. Кластер создается на основе шаблона кластера. |
| **Контейнер** | Высокопроизводительный механизм изоляции ресурсов. С помощью контейнеров можно изолировать различные процессы в отдельные модули, используя при этом одну операционную среду. Также контейнер — средство для развертывания и запуска приложений на сервере. Контейнер позволяет разработчикам упаковать своё приложение и все зависимости в один пакет.|
| ** Контейнеризированные приложения в Kubernetes** | Приложения, которые упакованы в контейнеры. |
| **Нода** | ВМ в кластере Kubernetes. |
| **ОЗУ** | Оперативная память. |
| **Под (pod)** | Минимальная вычислительная единица, которую можно запустить в кластере Kubernetes. |
| **Порт в кластере** | Точка подключения для коммуникации между компонентами внутри и снаружи кластера. |
| **Проект** |  Структурная единица внутри облака, которая владеет ресурсами: ВМ, БД, кластерами Kubernetes и другими. |
| **Служба управления инфраструктурой контейнеров (magnum)** | Служба API OpenStack, предоставляющая платформы по оркестрации контейнерами, такие как Docker Swarm, Kubernetes и Apache Mesos, как ресурсы первого уровня OpenStack. | 
| **API** | Механизмы, которые позволяют двум программным компонентам взаимодействовать друг с другом, используя набор определений и протоколов. |
| **Apache Mesos** | Централизованная отказоустойчивая система управления кластером. |
| **Docker хост** | Операционная система, на которую устанавливают Docker и на которой он работает. |
| **Docker Swarm** | Оркестратор от компании Docker, который позволяет объединять несколько Docker-хостов в единый кластер и автоматически управлять запуском и масштабированием контейнеров. |
| **HEAT** | Сервис оркестрации для OpenStack. Сервис позволяет пользователям автоматизировать создание и управление ресурсами облака с помощью шаблонов, описанных в формате YAML при помощи синтаксиса HOT (Heat Orchestration Template) или AWS CloudFormation. | **Heat Orchestration Template (HOT)** | Формат шаблона, используемый в OpenStack Heat для автоматизации развертывания ВМ, сетей и других ресурсов в облачной среде.  
| **Kibana** | Web-панель для работы с логами, расширяемый пользовательский интерфейс. Он позволяет визуализировать проиндексированные данные в системе Elasticsearch в виде графиков и диаграмм. |
| **Kubelet** | Утилита для отслеживания, чтобы контейнеры, созданные Kubernetes, запустились в поде |
|**Kubernetes (K8s)**| Открытое программное обеспечение для автоматизации развёртывания, масштабирования и управления контейнеризированными приложениями.
| **Logstash** | Утилита для парсинга данных (логов событий) одновременно из множества источников ввода и их обработки для дальнейшего использования в Elasticsearch.|
| **Master-node** | Главная нода, на котором выполняется три основных процесса в кластере: `kube-apiserver`, `kube-controller-manager` и `kube-scheduler`.
| **OpenStack** | Комплекс проектов свободного программного обеспечения, который может быть использован для создания инфраструктурных облачных сервисов и облачных хранилищ, при том как публичных, так и частных. |
| **SDK** | Набор инструментов для разработки программного обеспечения, объединённый в одном пакете. Набор содержит комплект необходимых библиотек, компилятор, отладчик, иногда — интегрированную среду разработки.
| **Worker Node** | Сервер, на котором работают два главных процесса Kubernetes, благодаря которым запускаются и работают контейнеризированные приложения. |
| **YAML** | Язык для сериализации данных, который отличается простым синтаксисом и позволяет хранить сложноорганизованные данные в компактном и читаемом формате.
## Как взаимодействуют Kubernetes и OpenStack?
Kubernetes и OpenStack — две различные, но взаимодополняющие технологии, предназначенные для управления облачными ресурсами и контейнеризированными приложениями. Подробнее о работе с Kubernetes в [официальной документации](https://kubernetes.io/docs/home/), о работе с OpenStack — в [официальной документации](https://docs.openstack.org/2023.2/).
* OpenStack предоставляет инфраструктуру: ВМ, сети и другие ресурсы, необходимые для работы Kubernetes.
* Kubernetes управляет контейнерами: Kubernetes может быть развернут на ВМ, предоставляемых OpenStack, и управлять контейнеризированными приложениями на этой инфраструктуре.
* Интеграция через API:  Kubernetes может взаимодействовать с OpenStack через его API для автоматического создания и управления ресурсами, например, ВМ, сетями и хранилищами данных.
* OpenStack обеспечивает гибкость и масштабируемость на уровне инфраструктуры, Kubernetes — на уровне контейнерных приложений.
* Кластер Kubernetes создается в OpenStack.
## Настройка кластера в Kubernetes
1. Откройте веб-интерфейс Kubernetes и создайте кластер выбранной версии.
2. При ошибках на этапе создания кластера в веб-интерфейсе откройте консоль разработчика по клавише F12 и проанализируйте ошибки.
3. Проверьте квоты проекта. Если ошибка в консоли указывает на их недостаток, то при необходимости увеличьте квоты или удалите ненужные объекты в проекте.
4. Проверьте, что создались объекты magnum-кластера и heat-кластера после начала создания кластера.
5. Проверьте, что создались:
   * сети,
   * порты,
   * балансировщики нагрузки,
   * диски,
   * инстансы.
8. Для диагностики и устранения ошибок на этапах создания объектов OpenStack используйте инструменты Heat или дополнительные средства.

## Дополнительные средства для поиска и анализа ошибок в Kubernetes
Дополнительно можно использовать средства для поиска и анализа ошибок в Kubernetes.
* **Prometheus**. Cистема мониторинга и оповещения с открытым исходным кодом, которая используется в Kubernetes. Система собирает метрики со всех узлов и подов в кластере м предоставляет язык запросов для их анализа. Подробнее о работе с Prometheus в [официальной документации](https://prometheus.io/docs/introduction/overview/).
* **Alertmanager**. Компонент Prometheus. Компонент обрабатывает оповещения, отправляемые сервером Prometheus, и отправляет уведомления в соответствии с настройками маршрутизации. Подробнее о работе с Prometheus в [официальной документации](https://prometheus.io/docs/alerting/latest/alertmanager/).
* **Custom Metrics**. Пользовательские метрики, которые можно собирать и экспонировать через специальные агенты или SDK. Подробнее о работе с Custom Metrics в [официальной документации](https://docs.datadoghq.com/metrics/custom_metrics/).
* **Elastic Stack (ELK Stack)**. Аббревиатура, используемая для описания стека из трех популярных проектов: Elasticsearch, Logstash и Kibana. Стек ELK (Elasticsearch) позволяет собирать журналы систем и приложений, анализировать их и создавать визуализации, чтобы мониторить приложения и инфраструктуры, быстрее устранять неполадки, анализировать систему безопасности и многое другое. Подробнее о работе с  ELK Stack в [официальной документации](https://www.elastic.co/guide/index.html).
* **Grafana**. Свободная программная система визуализации данных, ориентированная на данные систем ИТ-мониторинга. Реализована как веб-приложение в стиле «приборных панелей» с диаграммами, графиками, таблицами, предупреждениями. Подробнее о работе с Grafana в [официальной документации](https://grafana.com/docs/).
* **Kubernetes Events**. Функционал, позволяющий просматривать события в Kubernetes. Функционал вызывается по команде `kubectl get events`. Подробнее о работе с  Kubernetes Events в [официальной документации](https://kubernetes.io/docs/reference/kubernetes-api/cluster-resources/event-v1/).
* **Metrics Server**. Установленный по умолчанию сервис в Kubernetes. Сервис обирает метрики из `Kubelets` и экспонирует их через Kubernetes API для использования Horizontal Pod Autoscaler и других инструментов. Подробнее о работе с Metrics Server в [официальной документации](https://github.com/kubernetes-sigs/metrics-server).
Выбранные средства должны быть подключены к Kubernetes.

## Способы обращения в техническую поддержку
Подать обращение в техническую поддержку можно по доступным каналам связи:
|  Канал 	| Информация 	
|:---	|:--- 
|Портал | https://support.mcs.mail.ru | 
| Бот в ТГ | [@vk_tech_support_bot](https://t.me/vk_tech_support_bot)
| Почта | support@mcs.mail.ru
| Телефон | +7 (499) 350-97-03

Каналы связи круглосуточные.
