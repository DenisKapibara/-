# DEMO2022-2023-linux-only
(Свободный проект альтруистов) 100% выполненное задание (публичный вариант) к демонстрационному экзамену по сетевому и системному администрированию. Полностью и только на Debian. Основано на первоисточнике: https://github.com/storm39mad/DEMO2022/tree/main/azure

В формате docx: [Very Easy Linux.docx](https://github.com/cupespresso22/DEMO2022-2023-linux-only/files/8994172/Very.Easy.Linux.docx)

![изображение](https://user-images.githubusercontent.com/28905300/175996163-e7dbdb7b-2d18-4150-906a-ac925f3a66c7.png)

## Таблица 1. Характеристики ВМ

|Name VM         |ОС                  |RAM             |CPU             |IP                    |Additionally                       |
|  ------------- | -------------      | -------------  |  ------------- |  -------------       |  -------------                    |  
|RTR-L           |Debian 11/CSR       |2 GB            |2/4             |4.4.4.100/24          |                                   |
|                |                    |                |                |192.168.100.254/24    |                                   |
|RTR-R           |Debian 11/CSR       |2 GB            |2/4             |5.5.5.100/24          |                                   |
|                |                    |                |                |172.16.100.254 /24    |                                   |
|SRV             |Debian 11/Win 2019  |2 GB /4 GB      |2/4             |192.168.100.200/24    |Доп диски 2 шт по 5 GB             |
|WEB-L           |Debian 11           |2 GB            |2               |192.168.100.100/24    |                                   |
|WEB-R           |Debian 11           |2 GB            |2               |172.16.100.100/24     |                                   |
|ISP             |Debian 11           |2 GB            |2               |4.4.4.1/24            |                                   |
|                |                    |                |                |5.5.5.1/24            |                                   |
|                |                    |                |                |3.3.3.1/24            |                                   |
|CLI             |Win 10              |4 GB            |4               |3.3.3.10/24           |                                   |

## Виртуальные машины и коммутация.
Необходимо выполнить создание и базовую конфигурацию виртуальных
машин.

1. На основе предоставленных ВМ или шаблонов ВМ создайте отсутствующие виртуальные машины в соответствии со схемой.  
   -	Характеристики ВМ установите в соответствии с Таблицей 1;
   -	Коммутацию (если таковая не выполнена) выполните в соответствии со схемой сети.	 
2.  Имена хостов в созданных ВМ должны быть установлены в соответствии со схемой.
3.  Адресация должна быть выполнена в соответствии с Таблицей 1;
4.  Обеспечьте ВМ дополнительными дисками, если таковое необходимо в соответствии с Таблицей 1;

###### #(Если изначально доступна работа из под пользователя с обычными привилегиями, то повышение привилегий, то есть работа из под root, выполняется через команду *sudo -i*).
###### #На текущем этапе, пока что выполняется настройка имён хостов и IP-адресации.
![изображение](https://user-images.githubusercontent.com/28905300/175999607-fa8103f4-050f-4a61-a7a6-735731709549.png)
## МОНТИРОВАНИЕ ОБРАЗОВ И УСТАНОВКА ПАКЕТОВ
###### #На демо-экзамене предоставляются ISO-образы с дополнительным ПО. Установка пакетов через них. Выход в реальный интернет не предоставляется.
###### #К примеру, на виртуальной машине RTR-L нужно установить необходимые по заданию пакеты. Для этого необходимо вмонтировать в VM один из ISO-образов и добавить его в source.list (необязательно все, только самый первый).
![изображение](https://user-images.githubusercontent.com/28905300/176000139-50f97299-94b3-4d96-a349-003523110e04.png)
![изображение](https://user-images.githubusercontent.com/28905300/176000201-503b0625-7428-411b-bfb7-2b394a02a946.png)
![изображение](https://user-images.githubusercontent.com/28905300/176000248-83bb3cfb-6008-4d29-b180-d2b50f4ac576.png)
![изображение](https://user-images.githubusercontent.com/28905300/176000267-133e6c76-a612-4c3d-8721-a7edff571fa9.png)
###### #Одновременно с добавлением образа в source.list, производится обновление списка доступных пакетов от каждого смонтированного образа.
###### #Если требуется установить ПО, связанные с которым пакеты находятся на нескольких образах, то в proxmox надо так же выбрать уже другой образ, по образцу выше. После этого в Debian, для обновления пакетов от другого образа, нужно выполнить следующие команды:
![изображение](https://user-images.githubusercontent.com/28905300/176000405-83164754-85d4-4971-88ae-53d2270d1373.png)
###### #А далее снова *apt-cdrom add*.
###### #Добавляем автомонтирование образа с ПО при перезагрузке виртуальной машины.
![изображение](https://user-images.githubusercontent.com/28905300/176000659-9f98a2c6-6d0f-4b64-9cd5-2475ea9da2db.png)
###### #(в nano сохранение изменений на *CTRL+S*, выход — *CTRL+X*)
![изображение](https://user-images.githubusercontent.com/28905300/176000740-01433dbe-56ae-4c8c-984d-7715ecd47251.png)
##### Очень полезные сочетания клавиш в nano: 
##### Alt + 6 — копировать одну или несколько строк.
##### Ctrl + U — вставить строку или строки.
##### Ctrl + K — вырезать (удалить) строку.
##### Alt + T — удалить ВСЁ ниже курсора.
##### Ctrl + S — сохранить изменения.
##### Ctrl + X — выход.
##### Длинные пробелы ставятся при нажатии на TAB.

###### #Далее нужно настроить статическую IP-адресацию на каждой машине. Адресация представлена в конце комплекта оценочной документации. Для сопоставления MAC-адресов с IP-адресами на каждой машине, нужно проделать следующее:
![изображение](https://user-images.githubusercontent.com/28905300/176004597-94496239-f144-43e1-81dc-73aabf8415f3.png)
###### #На proxmox, при выборе виртуальной машины и просмотре списка его оборудования, нужно обратить внимание на MAC-адрес и имя моста (bridge). У другой машины, присоединённой к текущей (пример ISP), имя моста будет идентичным.
![изображение](https://user-images.githubusercontent.com/28905300/176004698-98cf415c-d184-455c-bd1d-d4a3f1bfd1dd.png)
###### #По одинаковому названию моста можно увидеть связность конкретных машин. По их MAC-адресам — связь с IP-адресами.
#### ПЕРВЫЙ ВАРИАНТ НАСТРОЙКИ
###### #Настройка IP-адресов каждой машины (кроме CLI). Каждый интерфейс настраивается через *nmcli* одной длинной командой:
#### ISP
![изображение](https://user-images.githubusercontent.com/28905300/176007506-05cc4003-ebd2-47e6-9809-bf8318c5ff4b.png)
###### #Названия интерфейсов можно посмотреть и через *nmcli connection*. Если наименования не содержат *Wired connection* и они имеют красный цвет, то нужно ввести команду *systemctl restart NetworkManager* и заново ввести *nmcli connection*.
![изображение](https://user-images.githubusercontent.com/28905300/176008687-f14b23de-f5f9-4f39-b75d-e4a7aa45b04c.png)
###### #Элементы команд полностью не нужно вписывать (кроме имён и цифр). Достаточно вбить начало команды (пример: ipv4.ad), как в терминале Cisco, и через нажатие на TAB произойдёт автодописывание (ipv4.addresses).
###### #Пример соответствует самой первой введёной строке:
###### nmc(TAB) co(TAB) mod(TAB) Wi(TAB) 1 ipv4.a(TAB) 5.5.5.1/24 ipv4.me(TAB) m(TAB) au(TAB) y(TAB) con-n(TAB) ISP-RTRR.
#### RTR-R
![изображение](https://user-images.githubusercontent.com/28905300/176009508-25841047-bf5d-4204-b272-df236b50be8a.png)
#### RTR-L
![изображение](https://user-images.githubusercontent.com/28905300/176009613-22e770a6-8af6-4019-a9fa-eb30aeaa4587.png)
#### WEB-L
![изображение](https://user-images.githubusercontent.com/28905300/176009673-ef765062-bba4-4aa8-a785-1173aa87d463.png)
#### SRV
![изображение](https://user-images.githubusercontent.com/28905300/176009754-57c53c09-cf9b-4f66-8984-2c6415919950.png)
#### WEB-R
![изображение](https://user-images.githubusercontent.com/28905300/176009814-47699208-042a-4e48-a9a5-20cc429be05d.png)
#### CLI
###### #На виртуальной машине CLI, по заданию, операционная система Windows 10. Порядок изменения имени хоста и IP-адреса там другой:
![изображение](https://user-images.githubusercontent.com/28905300/176009988-c0fa9d7a-74e4-4a4b-878b-08aa0375e485.png)
![изображение](https://user-images.githubusercontent.com/28905300/176010008-f28839a2-39ce-4bb8-85be-a6c83f67fb57.png)
![изображение](https://user-images.githubusercontent.com/28905300/176010035-149e5876-2d9e-4884-b2e8-330a842ab685.png)
![изображение](https://user-images.githubusercontent.com/28905300/176010052-77bd725e-8e6c-4ef2-82a6-3a9d37e7209e.png)
![изображение](https://user-images.githubusercontent.com/28905300/176010074-29212295-b515-4b54-8521-74e93d16bd90.png)
###### #Жмём на OK, заходим в PowerShell и пингуем шлюз по умолчанию (ISP). Далее меняем имя хоста:
![изображение](https://user-images.githubusercontent.com/28905300/176010140-0332983c-106e-4a4b-8efc-50fc888bbc06.png)
![изображение](https://user-images.githubusercontent.com/28905300/176010153-90806f3e-b203-4b85-a7d7-fd4654fef1c5.png)
![изображение](https://user-images.githubusercontent.com/28905300/176010174-468b241f-463d-481c-8f46-aba1c53c740c.png)
###### #Запрос о перезагрузке подтверждаем.
#### ВТОРОЙ ВАРИАНТ НАСТРОЙКИ
###### #(короткие команды в nmcli):
#### ISP
