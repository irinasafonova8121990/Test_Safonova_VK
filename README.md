# Test_Safonova_VK
## Назначение инструкции
Цель инструкции — пояснение процесса и методов диагностики ошибок при создании кластера Kubernetes с использованием веб-интерфейса и инструментов, таких как консоль разработчика, объекты magnum и heat в OpenStack. Инструкция состоит из:
* [глоссария](#Глоссарий),
* [описания настройки кластера Kubernetes](#Настройка кластера Kubernetes),
* перечисления дополнительных средств для поиска и анализа ошибок в Kubernetes.
## Глоссарий
|  Термин 	| Определение 	| 
|:---	|:--- |
| **Балансировщик нагрузки (loadbalancer)** | Средство для равномерного распределения сетевого трафика в зависимости от заданного алгоритма. |
|**Виртуальная машина (ВМ)**| Программная и/или аппаратная система, эмулирующая аппаратное обеспечение компьютера. ВМ исполняет программы для guest-платформы на host-платформе, виртуализирует и создает среды платформы, которые изолируют друг от друга программы. ВМ состоит из процессора, памяти, диска и сетевого адаптера. ВМ запускает одну операционную систему внутри другой. |
| **Диск** | Физическое или виртуальное хранилище данных. |
| **Инстанс** | Набор контейнеров или ВМ, которые связаны с одним горизонтально масштабируемым сервисом. |
| **Кластер** | Совокупность master-node и worker-node. Кластер создается на основе шаблона кластера. |
| **Контейнер** | Высокопроизводительный механизм изоляции ресурсов. С помощью контейнеров можно изолировать различные процессы в отдельные модули, используя при этом одну операционную среду. |
| **Под (pod)** | Минимальная вычислительная единица, которую можно запустить в кластере Kubernetes. |
| **Проект** |  Структурная единица внутри облака, которая владеет ресурсами: ВМ, базами данных (БД), кластерами Kubernetes и другими. |
| **Служба управления инфраструктурой контейнеров (magnum)** | Служба API OpenStack, предоставляющая платформы по оркестрации контейнерами, такие как Docker Swarm, Kubernetes и Mesos, как ресурсы первого уровня OpenStack. | 
| **API** | Механизмы, которые позволяют двум программным компонентам взаимодействовать друг с другом, используя набор определений и протоколов. |
| **Apache Mesos** | Централизованная отказоустойчивая система управления кластером. |
| **Docker Swarm** | Оркестратор от компании Docker, который позволяет объединять несколько Docker-хостов в единый кластер и автоматически управлять запуском и масштабированием контейнеров. |
| **HEAT** | Сервис оркестрации для OpenStack. Сервис позволяет пользователям автоматизировать создание и управление ресурсами облака с помощью шаблонов, описанных в формате YAML при помощи синтаксиса HOT (Heat Orchestration Template) или AWS CloudFormation. |
| **Kibana** | Web-панель для работы с логами, расширяемый пользовательский интерфейс. Он позволяет визуализировать проиндексированные данные в системе Elasticsearch в виде графиков и диаграмм. |
| **Kubelet** | Утилита для отслеживания, чтобы контейнеры, созданные Kubernetes, запустились в поде |
| **Kubernetes (K8s)** |  Портативная расширяемая платформа с открытым исходным кодом для управления контейнеризованными рабочими нагрузками и сервисами, которая облегчает как декларативную настройку, так и автоматизацию. |
| **Logstash** | Утилита для парсинга данных (логов событий) одновременно из множества источников ввода и их обработки для дальнейшего использования в Elasticsearch.|
| **OpenStack** | Комплекс проектов свободного программного обеспечения, который может быть использован для создания инфраструктурных облачных сервисов и облачных хранилищ, при том как публичных, так и частных. |
| **Magnum** | Gроект открытого исходного кода, который обеспечивает интеграцию контейнерных оркестраторов, таких как Kubernetes, Docker Swarm и Apache Mesos, с облачной платформой OpenStack. Magnum позволяет управлять контейнерами и контейнерными кластерами как первоклассными ресурсами в OpenStack. |
| **Master-node** | Главная нода, на котором выполняется три основных процесса в кластере: `kube-apiserver`, `kube-controller-manager` и `kube-scheduler`.
| **Worker Node** | Сервер, на котором работают два главных процесса Kubernetes, благодаря которым запускаются и работают контейнеризированные приложения. |
## Настройка кластера в Kubernetes
1. Откройте веб-интерфейс Kubernetes и создайте кластер Kubernetes выбранной версии.
2. При ошибках на этапе создания кластера в веб-интерфейсе откройте консоль разработчика по клавише F12 и проанализируйте ошибки.
3. Проверьте квоты проекта. Если ошибка в консоли указывает на их недостаток, то при необходимости увеличьте квоты или удалите ненужные объекты в проекте.
4. Проверьте, что создались объекты magnum-кластера и heat-кластера после начала создания кластера.
5. Проверьте, что создались:
   * сети,
   * порты,
   * балансировщики нагрузки,
   * диски,
   * инстансы.
8. Для диагностики и устранения ошибок на этапах создания объектов OpenStack используйте инструменты Heat.

## Дополнительные средства для поиска и анализа ошибок в Kubernetes
Дополнительно можно использовать средства из таблицы. Выбранные средства должны быть подключены к Kubernetes.
| Средство | Описание
|:---	|:--- 
| **Alertmanager** |  Компонент Prometheus. Компонент обрабатывает оповещения, отправляемые сервером Prometheus, и отправляет уведомления в соответствии с настройками маршрутизации.
| **Custom Metrics** |  Пользовательские метрики, которые можно собирать и экспонировать через специальные агенты или SDK. |
| **Elastic Stack (ELK Stack)**|  Аббревиатура, используемая для описания стека из трех популярных проектов: Elasticsearch, Logstash и Kibana. Стек ELK (Elasticsearch) позволяет собирать журналы систем и приложений, анализировать их и создавать визуализации, чтобы мониторить приложения и инфраструктуры, быстрее устранять неполадки, анализировать систему безопасности и многое другое.
| **Grafana** |  Свободная программная система визуализации данных, ориентированная на данные систем ИТ-мониторинга. Реализована как веб-приложение в стиле «приборных панелей» с диаграммами, графиками, таблицами, предупреждениями.
| **Kubernetes Events** |   Функционал, позволяющий просматривать события в Kubernetes. Функционал вызывается по команде `kubectl get events`.
| **Metrics Server**    | Установленный по умолчанию сервис в Kubernetes. Сервис обирает метрики из `Kubelets` и экспонирует их через Kubernetes API для использования Horizontal Pod Autoscaler и других инструментов.
| **Prometheus**        | Cистема мониторинга и оповещения с открытым исходным кодом, которая используется в эKubernetes. Система собирает метрики со всех узлов и подов в кластере м предоставляет язык запросов для их анализа.
## Смотрите также
* Официальная документация Kubernetes
