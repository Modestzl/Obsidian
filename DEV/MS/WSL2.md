[[Windows]]

> [!success]+ Включите WSL и платформу виртуальных машин командой
> ```json
> dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
>dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
> ```

-  Важно: виртуальный жесткий диск (`ext4.vhdx`) 

> [!success]+ Посмотреть точное расположение в реестре (самый верный)
> ```json
> # Получаем путь к виртуальному диску для конкретного дистрибутива
>(Get-ItemProperty "HKCU:\Software\Microsoft\Windows\CurrentVersion\Lxss\*" | Where-Object DistributionName -like "*Ubuntu*").BasePath
> ```

> [!success]- Посмотреть точное расположение и сразу открыть
> ```json
> # Получаем путь к виртуальному диску для конкретного дистрибутива
># Открыть папку с файлами дистрибутива в Проводнике
>explorer (Get-ItemProperty "HKCU:\Software\Microsoft\Windows\CurrentVersion\Lxss\*" | Where-Object DistributionName -like "*Ubuntu*").BasePath
> ```


> [!warning]- Удалить вместе с данными
> ```json
> # Проверям список
> wsl -l -v
># Удаляем
> wsl --unregister Ubuntu-22.04
> # Можно удалить вручную.
> Remove-Item -Path "D:\WSL\Ubuntu" -Recurse -Force
> ```

> [!impotant]- Два дистрибутива в списке 
>### Как это связано с вашим Ubuntu?
Ваш дистрибутив `Ubuntu` **полностью независим**. Docker не работает _внутри_ вашего Ubuntu (если вы только сами не решили его туда установить). Вместо этого Docker работает _рядом_.
Магия заключается в том, что Docker Desktop автоматически «прокидывает» свой клиентскую команду `docker` и сокет из своего дистрибутива `docker-desktop` в ваш `Ubuntu`.
>- Когда вы открываете терминал вашего `Ubuntu` и набираете `docker ps`, вы обращаетесь к серверу, который физически запущен в дистрибутиве `docker-desktop`.    
>- Именно поэтому вы можете управлять Docker из привычного Linux-окружения.
>- 
>  **Зачем разделять?**  
Главная цель — **безболезненное обновление**. Когда выходит новая версия Docker Desktop, она может полностью снести и пересоздать дистрибутив `docker-desktop`, и все ваши образы и данные останутся нетронутыми в `docker-desktop-data`. Ваш Ubuntu этим процессом вообще не затрагивается.


> [!success]- Экспорт WSL на другой диск
> ```json
> wsl --shutdown
>wsl --export Ubuntu "D:\WSL-backup\Ubuntu.tar"
># Если статус не `Stopped`, остановите принудительно
>wsl --terminate Ubuntu
>wsl --export Ubuntu D:\Backups\ubuntu-backup.tar
>wsl --import Ubuntu "D:\WSL\Ubuntu" "D:\Backups\ubuntu-backup.tar" --version 2
> ```
> 

>[!impotant]- Важные нюансы
>- **Резервная копия.** Перед любыми манипуляциями сделайте резервную копию важных данных. [pureinfotech.com](https://pureinfotech.com/move-wsl-distros-different-drive-windows/)[winitpro.ru](https://winitpro.ru/index.php/2022/01/10/wsl-perenos/)
> - **Имена.** При импорте дайте дистрибутиву новое имя (например, MovedUbuntu), чтобы не возникло конфликта с уже существующим дистрибутивом. [superuser.com](https://tr-page.yandex.ru/translate?lang=en-ru&url=https%3A%2F%2Fsuperuser.com%2Fquestions%2F1550622%2Fmove-wsl2-file-system-to-another-drive)
> - **Пользователь по умолчанию.** После импорта в дистрибутиве по умолчанию может быть пользователь  root. Чтобы вернуть привычного пользователя, отредактируйте файл  /etc/wsl.conf  внутри папки дистрибутива и добавьте строку                                      [user] default=ваше_имя . [dev.to](https://dev.to/ahmadtheswe/move-your-wsl2-to-another-drive-32on)
> - **Несколько дистрибутивов.** Если у вас несколько дистрибутивов, советую на новом диске создать отдельную папку для каждого — иначе при импорте могут возникнуть конфликты имён. [pureinfotech.com](https://pureinfotech.com/move-wsl-distros-different-drive-windows/)

> [!success]- Скачать, установить, настроить.
> ```json
> # Скачал ubuntu-26.04-wsl-amd64.wsl Сайт Ubuntu
># Создал папку ubuntu-26.04 На диске. D.
># Запустил. Powershell под Administrator. И делаю импорт.
>wsl --import Ubuntu-26.04 D:\WSL\Ubuntu-26.04  D:\WSL\ubuntuubuntu-26.04-wsl-amd64.wsl
># Установить версию по умолчанию. На всякий случай.
>wsl --set-default-version 2
># Запустите новый дистрибутив.
>wsl -d Ubuntu-26.04
># Сразу добавьте нового пользователя.
>useradd -m -G sudo -s /bin/bash ваше_имя
>passwd ваше_имя
># Остановить дистрибутив и зайти под своим именем.
>wsl --terminate Ubuntu-26.04
>wsl -d Ubuntu-26.04 -u ваше_имя
># Что бы обратно вернуться в root
>sudo -i
># И обратно.
>sudo -adminvrn
>> ```

![[docker WSL Resours.png]]



