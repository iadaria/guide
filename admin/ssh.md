() > ssh-keygen > (/Users/name/.ssh/test) > (password) > skip (enter) > skip (enter) > (got: test test.pub)
(enter into the remote server)> ssh username@host -p 22 > yes > password
(get public key from local machine)> cat ~/.ssh/name.pub > copy all
(save it into authorized_keys file in ~/.ssh folder)> mkdir ~/.ssh && vim ~/.ssh/authorized_keys > paste public key> :wq
(if didn't work set local private ssh key file) > ssh username@host -p 22 -i /Users/name/.ssh/test

() > vim ~/.ssh/config
Host 4\*.1**.28.2**
User gay
Port 22
IdentityFile ~/.ssh/one
