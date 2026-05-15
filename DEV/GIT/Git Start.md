`$ssh -T git@github.com` 
> Проверяем доступность Git по ключам(этого компа C://User/.ssh).

cat ~/.ssh/id_ed25519.pub
> Вам нужно дать GitHub публичную часть вашего ключа. 


eval "$(ssh-agent -s)"
- В Xterm сработал, а в PowerShell нет. Проверял доступность SSH.
- In the Powershell  Сработала следующая команда. `ssh-agent -s`




