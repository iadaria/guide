> go run <package-name>
> go bukd <file.go>

### Variable
var <name-of-variable> <type-of-variable>

### Generate mod
[sctructure](./imgs/mod.png)
> go mod init projectname
import (
	"projectname/package1"
)

### add other public packages
> go get <name>