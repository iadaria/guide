> go run <package-name>
> go bukd <file.go>

* defer - выполняем функцию в конце программы
```golang
func main() {
    defer finish()
    fmt.Println("Program has been started")
    fmt.Println("Program is working")
}
 
func finish(){
    fmt.Println("Program has been finished")
}
```

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

### range
* Для перебора массивов можно использовать следующую форму цикла for:
```golang
for индекс, значение := range массив{
    // действия
}
```


### slice - срезы
* Срез определяется также, как и массив, за тем исключением, что у него не указывается длина:
>var users []string
* С помощью функции make() можно создать срез из нескольких элементов, которые будут иметь значения по умолчанию:
```golang
var users []string = make([]string, 3)
users[0] = "Tom"
users[1] = "Alice"
users[2] = "Bob"
```
* Для добавления в срез применяется встроенная функция append(slice, value). Первый параметр функции - срез, в который надо добавить, а второй параметр - значение, которое нужно добавить.\
Результатом функции является увеличенный срез.
```golang
users := []string{"Tom", "Alice", "Kate"}
users = append(users, "Bob")
     
for _, value := range users{
    fmt.Println(value)
}
```
* Оператор среза s[i:j] создает из последовательности s новый срез, который содержит элементы последовательности s с i по j-1. При этом должно соблюдаться условие 0 <= i <= j <= cap(s). 
В качестве исходной последовательности, из которой берутся элементы, может использоваться массив, указатель на массив или другой срез. В итоге в полученном срезе будет j-i элементов.
Если значение i не указано, то применяется по умолчанию значение 0. Если значение j не указано, то вместо него используется длина исходной последовательности s.
```golang
// исходный массив
initialUsers := [8]string{"Bob", "Alice", "Kate", "Sam", "Tom", "Paul", "Mike", "Robert"}
users1 := initialUsers[2:6]     // с 3-го по 6-й 
users2 := initialUsers[:4]      // с 1-го по 4-й
users3 := initialUsers[3:]      // с 4-го до конца
     
fmt.Println(users1)     // ["Kate", "Sam", "Tom", "Paul"]
fmt.Println(users2)     // ["Bob", "Alice", "Kate", "Sam"]
fmt.Println(users3)     // ["Sam", "Tom", "Paul", "Mike", "Robert"]
```
* Что делать, если необходимо удалить какой-то определенный элемент? В этом случае мы можем комбинировать функцию append и оператор среза:
```golang
users := []string{"Bob", "Alice", "Kate", "Sam", "Tom", "Paul", "Mike", "Robert"}
//удаляем 4-й элемент
var n = 3
users = append(users[:n], users[n+1:]...)   
fmt.Println(users)      //["Bob", "Alice", "Kate", "Tom", "Paul", "Mike", "Robert"]
```



```golang
```
```golang
```
```golang
```
```golang
```
```golang
```
```golang
```