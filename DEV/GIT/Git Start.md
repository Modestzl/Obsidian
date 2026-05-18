`$ssh -T git@github.com` 
> Проверяем доступность Git по ключам(этого компа C://User/.ssh).

`cat ~/.ssh/id_ed25519.pub`
> Вам нужно дать GitHub публичную часть вашего ключа. 


>[!danger]-
>eval "$(ssh-agent -s)"
>- В Xterm сработал, а в PowerShell нет. Проверял доступность SSH.
>- - In the Powershell  Сработала следующая команда. `ssh-agent -s`
>


`git config --global user.email "office@mail.ru"`
`git config --global user.name "office"`

`git add .` ->`git commit -m 'Добавил DEV'`  - > `git push origin master`



