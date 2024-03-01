# Test_Safonova (OpenStack)
## Назначение инструкции
**Цель инструкции** — пояснение процесса и методов диагностики ошибок при создании кластера Kubernetes с использованием веб-интерфейса и инструментов, например, консоли разработчика, объектов magnum и heat в OpenStack. Инструкция состоит из:
* [глоссария](https://github.com/irinasafonova8121990/Test_Safonova_VK/blob/main/README.md#%D0%B3%D0%BB%D0%BE%D1%81%D1%81%D0%B0%D1%80%D0%B8%D0%B9),
* [описания взаимодействия Kubernetes и OpenStack](https://github.com/irinasafonova8121990/Test_Safonova_VK#%D0%BA%D0%B0%D0%BA-%D0%B2%D0%B7%D0%B0%D0%B8%D0%BC%D0%BE%D0%B4%D0%B5%D0%B9%D1%81%D1%82%D0%B2%D1%83%D1%8E%D1%82-kubernetes-%D0%B8-openstack),
* [описания настройки кластера Kubernetes](https://github.com/irinasafonova8121990/Test_Safonova_VK/blob/main/README.md#%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-%D0%BA%D0%BB%D0%B0%D1%81%D1%82%D0%B5%D1%80%D0%B0-%D0%B2-kubernetes),
* [примеры прочих ошибок при создании кластера в Kubernetes](https://github.com/irinasafonova8121990/Test_Safonova_VK/blob/main/README.md#%D0%BF%D1%80%D0%B8%D0%BC%D0%B5%D1%80%D1%8B-%D0%BF%D1%80%D0%BE%D1%87%D0%B8%D1%85-%D0%BE%D1%88%D0%B8%D0%B1%D0%BE%D0%BA-%D0%BF%D1%80%D0%B8-%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B8-%D0%BA%D0%BB%D0%B0%D1%81%D1%82%D0%B5%D1%80%D0%B0-%D0%B2-kubernetes),
* [перечисления дополнительных средств мониторинга и логирования](https://github.com/irinasafonova8121990/Test_Safonova_VK/blob/main/README.md#%D0%B4%D0%BE%D0%BF%D0%BE%D0%BB%D0%BD%D0%B8%D1%82%D0%B5%D0%BB%D1%8C%D0%BD%D1%8B%D0%B5-%D1%81%D1%80%D0%B5%D0%B4%D1%81%D1%82%D0%B2%D0%B0-%D0%B4%D0%BB%D1%8F-%D0%BF%D0%BE%D0%B8%D1%81%D0%BA%D0%B0-%D0%B8-%D0%B0%D0%BD%D0%B0%D0%BB%D0%B8%D0%B7%D0%B0-%D0%BE%D1%88%D0%B8%D0%B1%D0%BE%D0%BA-%D0%B2-kubernetes).

При возникновении проблем с настройкой обратитесь в [техническую поддержку](https://github.com/irinasafonova8121990/Test_Safonova_VK/blob/main/README.md#%D1%81%D0%BF%D0%BE%D1%81%D0%BE%D0%B1%D1%8B-%D0%BE%D0%B1%D1%80%D0%B0%D1%89%D0%B5%D0%BD%D0%B8%D1%8F-%D0%B2-%D1%82%D0%B5%D1%85%D0%BD%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D1%83%D1%8E-%D0%BF%D0%BE%D0%B4%D0%B4%D0%B5%D1%80%D0%B6%D0%BA%D1%83).

## Глоссарий
|  Термин 	| Определение 	| 
|:---	|:--- 
| **Балансировщик нагрузки (loadbalancer)** | Средство для распределения входящего трафика между несколькими виртуальными бэкенд-серверами. |
| **Бэкенд-сервер** | Часть информационной системы, которая отвечает за серверную сторону приложения или веб-сервиса. |
| **БД** | База данных. Совокупность данных, хранимых в соответствии со схемой данных, манипулирование которыми выполняют в соответствии с правилами средств моделирования данных. |
|**Виртуальная машина (ВМ)**|  Виртуальный компьютер, который использует выделенные ресурсы реального компьютера. Эти ресурсы хранятся в облаке и позволяют ВМ работать автономно. ВМ состоит из виртуального процессора, памяти, дисков и сетевого адаптера. ВМ запускает одну операционную систему внутри другой. |
|**Вычислительные ресурсы**| Виртуальные процессоры и ОЗУ. |
| **Диск** | Физическое или виртуальное хранилище данных. |
| **Инстанс** | Набор контейнеров или ВМ, которые связаны с одним горизонтально масштабируемым сервисом. |
| **Квоты** | Применяемые к проекту ограничения на использование виртуальных вычислительных ресурсов и создание объектов в конкретных сервисах. |
| **Кластер Kubernetes** | Совокупность master-node и worker-node. Кластер создается на основе шаблона кластера. |
| **Контейнер** | Высокопроизводительный механизм изоляции ресурсов. С помощью контейнеров можно изолировать различные процессы в отдельные модули, используя при этом одну операционную среду. Также контейнер — средство для развертывания и запуска приложений на сервере. |
| **Контейнеризированные приложения в Kubernetes** | Приложения, которые упакованы в контейнеры. |
| **Нода (узел)** | ВМ в кластере Kubernetes. |
| **ОЗУ** | Оперативная память. |
| **Объекты в кластере Kubernetes** | Ноды, поды, сервисы и так далее. |
| **Оркестратор** | Система, которая управляет жизненным циклом контейнеров в масштабируемой и автоматизированной среде. | 
| **Под (pod)** | Минимальная вычислительная единица, которую можно запустить в кластере Kubernetes. |
| **Порт в кластере** | Точка подключения для коммуникации между компонентами внутри и снаружи кластера. |
| **Приложение в Kubernetes**| Набор координируемых и управляемых контейнеров, которые работают вместе для предоставления определённой функциональности или сервиса. |
| **Проект (namespace)**|  Структурная единица внутри облака, которая владеет ресурсами: ВМ, БД, кластерами Kubernetes и другими. |
| **Секретная переменная (Secret)** |  Объекты, которые позволяют хранить и управлять конфиденциальной информацией, например, пароли и ssh-ключи. |
| **Сервис (Service)** | Абстрактный объект, который определяет логический набор подов и политику доступа к ним. |
| **Сетевые политики** | Набор правил, которые определяют, какие поды или их группы могут взаимодействовать между собой через сетевые соединения внутри кластера. |
| **Служба управления инфраструктурой контейнеров (magnum)** | Служба API OpenStack, предоставляющая платформы по оркестрации контейнерами, такие как Docker Swarm, Kubernetes и Apache Mesos, как ресурсы первого уровня OpenStack. |
| **Технические лимиты** | Ограничения платформы, обусловленные особенностями архитектуры OpenStack. |
| **Хост** | Компьютер, подключённый к локальной или глобальной сети. |
| **Apache Lucene** | Свободная библиотека для высокопроизводительного полнотекстового поиска фонда Apache. |
| **Apache Mesos** | Централизованная отказоустойчивая система управления кластером. |
| **API** | Механизмы, которые позволяют двум программным компонентам взаимодействовать друг с другом, используя набор определений и протоколов. |
| **Docker** | Программное обеспечение для автоматизации развёртывания и управления приложениями в средах с поддержкой контейнеризации. |
| **Docker хост** | Операционная система, на которую устанавливают Docker и на которой он работает. |
| **Docker Swarm** | Оркестратор от компании Docker, который позволяет объединять несколько Docker-хостов в единый кластер и автоматически управлять запуском и масштабированием контейнеров. |
| **Elasticsearch** | Открытая распределенная система управления данными и поиска по ним, основанная на поисковом движке Apache Lucene. |
| **Elastic Stack (ELK Stack)** | Сочетание трех продуктов: Elasticsearch, Logstash и Kibana. ELK Stack может использоваться для централизованного сбора, хранения и анализа логов с узлов и подов Kubernetes. | 
| **Heat** | Сервис оркестрации для OpenStack. Сервис позволяет пользователям автоматизировать создание и управление ресурсами облака с помощью шаблонов, описанных в формате YAML при помощи синтаксиса HOT (Heat Orchestration Template) или AWS CloudFormation. |
| **Heat Orchestration Template (HOT)** | Формат шаблона, используемый в OpenStack Heat для автоматизации развертывания ВМ, сетей и других ресурсов в облачной среде. |
| **Kibana** | Web-панель для работы с логами, расширяемый пользовательский интерфейс. Он позволяет визуализировать проиндексированные данные в системе Elasticsearch в виде графиков и диаграмм. |
|**Kubernetes (K8s)**| Открытое программное обеспечение для автоматизации развёртывания, масштабирования и управления контейнеризированными приложениями.
| **Logstash** | Утилита для парсинга данных (логов событий) одновременно из множества источников ввода и их обработки для дальнейшего использования в Elasticsearch.|
| **Master-node** | Главная нода, на котором выполняется три основных процесса в кластере: `kube-apiserver`, `kube-controller-manager` и `kube-scheduler`. |
| **OpenStack** | Комплекс проектов свободного программного обеспечения, который может быть использован для создания инфраструктурных облачных сервисов и облачных хранилищ, при том как публичных, так и частных. |
| **SSH-ключ** | Пары криптографических ключей, которые обеспечивают безопасную связь между двумя устройствами. |
| **Worker Node** | Сервер, на котором работают два главных процесса Kubernetes, благодаря которым запускаются и работают контейнеризированные приложения. |
| **YAML** | Язык для сериализации данных, который отличается простым синтаксисом и позволяет хранить сложноорганизованные данные в компактном и читаемом формате. |
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
> Для создания кластера необходимы оответствующие права доступа в кластере Kubernetes.
3. Проверьте квоты проекта. Если ошибка в консоли указывает на их недостаток, то при необходимости увеличьте квоты или удалите ненужные объекты в проекте.
>  Для [редактирования квот](https://github.com/irinasafonova8121990/Test_Safonova_VK/blob/main/README.md#%D0%BA%D0%B0%D0%BA-%D0%B8%D0%B7%D0%BC%D0%B5%D0%BD%D0%B8%D1%82%D1%8C-%D0%BA%D0%B2%D0%BE%D1%82%D1%8B) необходимы соответствующие права доступа в кластере Kubernetes.
4. Проверьте, что создались объекты magnum-кластера и heat-кластера после начала создания кластера.
5. Проверьте, что создались:
   * сети,
   * порты,
   * балансировщики нагрузки,
   * диски,
   * инстансы.
6. Для диагностики и устранения ошибок на этапах создания объектов OpenStack используйте инструменты Heat или дополнительные средства.
### Как изменить квоты?
1. Получите список текущих квот по команде:
```bash
   kubectl get resourcequota -n <namespace_test>
```
2. Просмотрите детали существующей квоты по команде:
 ```bash
   kubectl describe resourcequota <quota-name> -n <namespace_test>
   ```
3. Откройте конфигурацию квоты по команде:
```bash
   kubectl edit resourcequota <quota-name> -n <namespace_test>
```
4. Измените параметры квоты.

**Внимание!** На ряд параметров могут быть установлены технические лимиты. Технические лимиты — ограничения платформы, обусловленные особенностями архитектуры OpenStack. Бывают жесткие и нежесткие лимиты. Одни лимиты не связаны с физическими ограничениями и базируются на эксплуатационных требованиях сервисов. Для некоторых технических лимитов не существует соответствующих им квот. Пример жесткого лимита — лимит на видеокарты у одного инстанса. Примеры нежестких лимитов — количество виртуальных процессоров и общий объем оперативной памяти в проекте.

### Примеры параметров
|  **Группа параметров** 	| **Параметр** 	| Команда установления количества
|:---	|:--- |:---
| Вычислительные ресурсы |  | 
| Вычислительные ресурсы | Количество CPU в проекте | `requests.cpu`
|  | Количество памяти  в проекте | `requests.memory`
|  | Лимит памяти для всех подов | `limits.memory`
|Хранилище данных  |  |
|  | Общий объем временного хранения | `requests.ephemeral-storage`
|  | Лимит временного хранения для всех подов | `limits.ephemeral-storage`
|  |  |
|  |  |
|  |  |
|  |  |



* **Вычислительные ресурсы**.
    - Количество CPU в проекте. Количество устанавливается по команде `requests.cpu`.
    - Количество памяти  в проекте. Количество устанавливается по команде `requests.memory`.
    - Максимальный лимит памяти для всех подов. Лимит устанавливается по команде `limits.memory`.
* **Хранилище данных**.
    - Общий объем временного хранения. Объем устанавливается по команде `requests.ephemeral-storage`.
    - Максимальный лимит временного хранения для всех подов. Лимит устанавливается по команде `limits.ephemeral-storage`.
* **Счётчики объектов**.
    - Максимальное количество подов. Количество устанавливается по команде `pods`.
    - Максимальное количество сервисов. Количество устанавливается по команде `services`.
    - Максимальное количество сервисов с типом LoadBalancer. Количество устанавливается по команде `services.loadbalancers`.
    - Максимальное количество секретных переменных. Количество устанавливается по команде `secrets`.
* **Инстансы**. Количество устанавливается по команде `kubectl scale`.

Пример конфигурации квоты:
```bash
apiVersion: v1
kind: ResourceQuota
metadata:
  name: my-quota
  namespace: namespace_test
spec:
  hard:
    requests.cpu: "2"
    requests.memory: 2Gi
    limits.memory: 1Gi
    pods: "5"
    services: "2"
    services.loadbalancers: "2"
    secrets: "6"
```
5. Чтобы применить эту квоту, сохраните YAML-файл и создайте ресурс в Kubernetes с помощью команды `kubectl`. В результате после редактирования конфигурации и сохранения файла изменения автоматически примяются к кластеру. Новые запросы на создание ресурсов проверяются на соответствие установленным квотам. Если запрос превышает доступные квоты, он отклоняется.
6. Проверьте обновленные квоты по команде:
  ```bash
   kubectl describe resourcequota <quota-name> -n <your-namespace>
  ```
## Примеры прочих ошибок при создании кластера в Kubernetes
* Неучет требований RBAC (Role-based access control), системы распределения прав доступа к различным объектам в кластере Kubernetes. При ошибках проверьте права доступа. Подробнее о работе с RBAC в [официальной документации Kubernetes](https://kubernetes.io/docs/reference/access-authn-authz/rbac/).
* Некорректная конфигурация сетевых политик. При ошибках проверьте конфигурацию. Подробнее о работе с сетевыми политиками в [официальной документации Kubernetes](https://kubernetes.io/docs/concepts/services-networking/network-policies/).
* Некорректное управление версиями и обновлениями кластера. Перед обновлением обязательно протестируйте работу кластера.
* Недостаточный мониторинг и логирование кластера. При ошибках проверьте настройки мониторинга и логирования. Также можно использовать [дополнительные средства](https://github.com/irinasafonova8121990/Test_Safonova_VK/blob/main/README.md#%D0%B4%D0%BE%D0%BF%D0%BE%D0%BB%D0%BD%D0%B8%D1%82%D0%B5%D0%BB%D1%8C%D0%BD%D1%8B%D0%B5-%D1%81%D1%80%D0%B5%D0%B4%D1%81%D1%82%D0%B2%D0%B0-%D0%B4%D0%BB%D1%8F-%D0%BF%D0%BE%D0%B8%D1%81%D0%BA%D0%B0-%D0%B8-%D0%B0%D0%BD%D0%B0%D0%BB%D0%B8%D0%B7%D0%B0-%D0%BE%D1%88%D0%B8%D0%B1%D0%BE%D0%BA-%D0%B2-kubernetes**).
* Неопределение лимитов и запросов для подов. При ошибках проверьте, указали ли вы лимиты и запросы. Подробнее об определении лимитов для подов в [официальной документации Kubernetes](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/). Подробнее об определении запросов для подов в [официальной документации Kubernetes](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/).

## Дополнительные средства мониторинга и логирования
* **Prometheus**. Cистема мониторинга и оповещения с открытым исходным кодом, которая используется в Kubernetes. Система собирает метрики со всех узлов и подов в кластере м предоставляет язык запросов для их анализа. Подробнее о работе с Prometheus в [официальной документации](https://prometheus.io/docs/introduction/overview/).
* **Alertmanager**. Компонент Prometheus. Компонент обрабатывает оповещения, отправляемые сервером Prometheus, и отправляет уведомления в соответствии с настройками маршрутизации. Подробнее о работе с Alertmanager в [официальной документации](https://prometheus.io/docs/alerting/latest/alertmanager/).
* **Custom Metrics**. Пользовательские метрики, которые можно собирать и экспонировать через специальные агенты или SDK. Подробнее о работе с Custom Metrics в [официальной документации](https://docs.datadoghq.com/metrics/custom_metrics/).
* **Elastic Stack (ELK Stack)**. Аббревиатура, используемая для описания стека из трех популярных проектов: Elasticsearch, Logstash и Kibana. Стек ELK (Elasticsearch) позволяет собирать журналы систем и приложений, анализировать их и создавать визуализации, чтобы мониторить приложения и инфраструктуры, быстрее устранять неполадки, анализировать систему безопасности и многое другое. Подробнее о работе с  ELK Stack в [официальной документации](https://www.elastic.co/guide/index.html).
* **Grafana**. Свободная программная система визуализации данных, ориентированная на данные систем ИТ-мониторинга. Реализована как веб-приложение в стиле «приборных панелей» с диаграммами, графиками, таблицами, предупреждениями. Подробнее о работе с Grafana в [официальной документации](https://grafana.com/docs/).
* **Kubernetes Events**. Функционал, позволяющий просматривать события в Kubernetes. Функционал вызывается по команде `kubectl get events`. Подробнее о работе с Kubernetes Events в [официальной документации](https://kubernetes.io/docs/reference/kubernetes-api/cluster-resources/event-v1/).
* **Metrics Server**. Установленный по умолчанию сервис в Kubernetes. Сервис обирает метрики из `Kubelets` и экспонирует их через Kubernetes API для использования Horizontal Pod Autoscaler и других инструментов. Подробнее о работе с Metrics Server в [официальной документации](https://github.com/kubernetes-sigs/metrics-server).

> Выбранные средства должны быть подключены к Kubernetes.

## Способы обращения в техническую поддержку
Подать обращение в техническую поддержку можно по доступным круглосуточным каналам связи:
|  Канал 	| Информация 	
|:---	|:--- 
|**Портал** | https://support.mts.ru |
| **Почта** | support@support.mts.ru |
| **Телефон** |  + 7 (800) 250-08-90 |
