Toml Config 
===

从toml格式文件中读取配置

```go
package main
import(
    "fmt"
    "log"
    "github.com/BurnSushi/toml"
)


type Config struct{
    Name string
    Awake bool
    Hungry bool
}

func main(){
    c := Config{}
    _, err := toml.DecodeFile("config.toml", &c)
    if err!= nil{
        log.Fatal(err)
    }

    fmt.Printf("+v\n",c)
}

```