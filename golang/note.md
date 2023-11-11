### := and =

= инициализируем глобальную переменнюу
:= инициализируем локальную переменную

### auto build

article: https://freddixx.medium.com/automatic-reloading-during-development-with-go-31d370cbdf12

install air: https://github.com/cosmtrek/air/

> curl -sSfL https://raw.githubusercontent.com/cosmtrek/air/master/install.sh | sh -s -- -b $(go env GOPATH)/bin

> vim ~/.zshrc
> add 'alias air='~/go/bin/air'

> source ~/.zshrc

> cd project
> air init
> air

> npx nodemon --exec "go run" ./main.go --signal SIGTERM - Пересобираем и перезапускаем автоматом

### install dependencies

> go get all

> go build .
> ./projectName

### go doc

> vim ~/.zshrc
> add $HOME/go/bin:$PATH
> source ~/.zshrc
> godoc help
> godoc -http=:8001

### breakpoint

> go mod init name.com (/v1)
> go mod tidy

- add breakpoints and add launching in VS code.
