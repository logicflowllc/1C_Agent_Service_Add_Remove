Добавление службы/кластера 1С:Предприятие в пределах одного сервера
================

При наличие нескольких баз в пределах одного кластера 1С:Предприятие, иногда может возникнуть необходимость разделить их, например, для удобства перезапуска: у Вас 10 баз, нужно перезапустить сервер 1С только для одной, а остальные не трогать.

Либо для снятия ограничения на размер **rphost** без приобритения КОРП лицензии.


## Добавление службы

Чтобы добавить службу/кластер необходимо запустить от имени Администратора скрипт **1_Add_1C_Agent_Service.cmd**, предварительно модифицировав его. 

```sc create "1C_Server_21240" binPath= "\"C:\Program Files\1cv8\8.3.18.1741\bin\ragent.exe\" -srvc -agent -port 21240 -regport 21241 -range 21260:21291 -d \"C:\Program Files\1cv8\srvinfo21240"" start= auto displayname= "Агент сервера 1С:Предприятия 8.3 (21240)" obj= ".\USR1CV8" password= XXXXXX```

1. ***1C_Server_21240*** - название создваваемой службы и раздела реестра, в котором хранится информация об этой службе
2. ***C:\Program Files\1cv8\8.3.18.1741\bin\ragent.exe\*** - полный путь к используемой платформе
3. ***21240*** - порт сервера 1С
4. ***21241*** - порт сервера 1С
5. ***21260:21291*** - порт сервера 1С (Для удобства мы берём порты по аналогии с оригинальными 1540, 1541, 1560-1591)
6. ***C:\Program Files\1cv8\srvinfo21240*** - путь к создаваемой папки для временных файлов и кэша.
7. ***Агент сервера 1С:Предприятия 8.3 (21240)*** - имя службы на русском языке
8. ***.\USR1CV8*** - пользователь Windows, от имеми которого будет запускаться создаваемая служба 1С
9. ***XXXXXX*** - пароль для пользователя Windows, от имеми которого будет запускаться создаваемая служба 1С
10. Если необходимо включить отладку, добавляем в конце строки ключ ***-debug***
11. Если необходимо включить отладку по HTTP, добавляем в конце строки два ключа ***-debug -http***

## Проверка созданной службы/кластера

После запуска CMD файла вы увидите новую службу в Диспетчере задач и в списке служб в реестре (_regedit_) по адресу: ```Компьютер\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\```

Чтобы добавить кластер в панель Администрирование серверов 1С:Предприятия, мы должны 
* Запустить новую службу
* Переместиться в корень сервера
* Создать > Центральный сервер 1С:Предприятие 8.3
* В качестве сервера указываем DNS имя сервера
* В качестве порта указываем порт из ***пункта 3***, то есть в данном примере это 21240

Чтобы пользваться новым кластером за пределами сервера не забудьте **открыть все указанные порты в брандмауэре**. В нашем случае это 21240, 21241, 21260-21291.

## Удаление созданной службы
Для удаление воспользуемся файлом **2_Remove_1C_Agent_Service.cmd**, предварительно модифицировав его.

В качестве имени службы для удаления выбираем имя службы из ***пункта 1***.
