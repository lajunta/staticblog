# Golang with gRPC

[speedscale links](https://speedscale.com/using-grpc-with-golang/)

### what is gRPC?

gRPC is a high-performance Remote Procedure call framework that allows developers to connect services across systems. Using gRPC, the client and server apps can communicate flawlessly.

Simply put, gRPC allows a client application to access and use methods on a server application (even in remote machines) as if it’s defined in the same application. It is a major building block for microservices.

### 前期准备

- GO： latest go release
- ProtocolBuffer compiler [protoc v3](https://grpc.io/docs/protoc-installation/)
- Go plugins:
  1. install plugin
     ```go
     $ go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.28
     $ go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.2
     ```
  1. make sure the plugin in the $PATH
     ```go
     $ export PATH="$PATH:$(go env GOPATH)/bin"
     ```

### Create GO Project

```
mkdir  notes
cd notes
go mod init github.com/lajunta/grpc-demo
go mod edit -require=google.golang.org/grpc@latest
go mod download
```

### 生成 notes.proto

```proto
syntax = "proto3";
package notes;

option go_package = "github.com/lajunta/grpc-demo/notes;notes";
```

上面最后一句话说明生成的代码属于哪个模块哪个包

```proto
syntax = "proto3";
package notes;

option go_package = "github.com/lajunta/grpc-demo/notes;notes";

service Notes {
  rpc Save (Note) returns (NoteSaveReply) {}
  rpc Load (NoteSearch) returns (Note) {}
}

message Note {
  string title = 1;
  bytes body  = 2;
}

message NoteSaveReply {
  bool saved = 1;
}

message NoteSearch {
  string keyword = 1;
}
```

### 用 protoc 生成相应的代码

```bash
protoc --go_out=. --go_opt=paths=source_relative  --go-grpc_out=. --go-grpc_opt=paths=source_relative --go-grpc_opt=require_unimplemented_servers=false notes/notes.proto
```

在当前目录下执行上面命令，会在 notes 子目录下生成`notes.pb.go` 这个文件用来处理数据的序列化和反序列化，`notes_grpc.pb.go` 用来处理 gRPC 协议相关的程序。

### Make grpc Service File

make `note.go` in notes file

```go
package notes

import (
        "errors"
        "io/ioutil"
        "log"
        "os"
        "path/filepath"
        "strings"
)

// Save a Note to the disk with the title as filename
func SaveToDisk(n *Note, folder string) error {
        filename := filepath.Join(folder, n.Title) //title should be sanitized
        return os.WriteFile(filename, n.Body, 0600)
}

// Scan files in a folder to find first occurrence of a keyword
func LoadFromDisk(keyword string, folder string) (*Note, error) {
        filename, err := searchKeywordInFilename(folder, keyword)
        if err != nil {
                return nil, err
        }
        body, err := os.ReadFile(filepath.Join(folder, filename))
        if err != nil {
                return nil, err
        }
        return &Note{Title: filename, Body: body}, nil
}

// Scan a directory and if a file name contains a substring, return the first one
func searchKeywordInFilename(folder string, keyword string) (string, error) {
        items, _ := ioutil.ReadDir(folder)
        for _, item := range items {

                // Read the whole file at once
                // this is the most inefficient search engine in the world
                // good enough for an example
                b, err := ioutil.ReadFile(filepath.Join(folder, item.Name()))
                if err != nil {
                        // This is not normal but we can safely ignore it
                        log.Printf("Could not read %v", item.Name())
                }
                s := string(b)

                if strings.Contains(s, keyword) {
                        return item.Name(), nil
                }
        }
        return "", errors.New("no file contains this keyword")
}
```

### Make Server file

make dir `server`

```go
package main

import (
        "context"
"flag"
        "fmt"
        "log"
        "net"

        "example/notes"
        "google.golang.org/grpc"
)

var (
        port = flag.Int("port", 50051, "The server port")
)

func main() {
        // parse arguments from the command line
        // this lets us define the port for the server
        flag.Parse()
        lis, err := net.Listen("tcp", fmt.Sprintf(":%d", *port))
        // Check for errors
        if err != nil {
                log.Fatalf("failed to listen: %v", err)
        }
        // Instantiate the server
        s := grpc.NewServer()

        // Register server method (actions the server will do)
  // TODO

        log.Printf("server listening at %v", lis.Addr())
        if err := s.Serve(lis); err != nil {
                log.Fatalf("failed to serve: %v", err)
        }
}
```

__Replace //TODO with following code:__

    notes.RegisterNotesServer(s, &notesServer{})

```go
type notesServer struct {
        notes.UnimplementedNotesServer
}

// Implement the notes.NotesServer interface
func (s *notesServer) Save(ctx context.Context, n *notes.Note) (*notes.NoteSaveReply, error) {
        log.Printf("Received a note to save: %v", n.Title)
        err := notes.SaveToDisk(n, "testdata")

        if err != nil {
                return &notes.NoteSaveReply{Saved: false}, err
        }

        return &notes.NoteSaveReply{Saved: true}, nil
}

// Implement the notes.NotesServer interface
func (s *notesServer) Load(ctx context.Context, search *notes.NoteSearch) (*notes.Note, error) {
        log.Printf("Received a note to load: %v", search.Keyword)
        n, err := notes.LoadFromDisk(search.Keyword, "testdata")

        if err != nil {
                return &notes.Note{}, err
        }

        return n, nil
}
```

Run the server 

    go run ./server/main.go

### Client Side

```go
package main

import (
	"context"
	"flag"
	"fmt"
	"log"
	"os"
	"time"

	"github.com/lajunta/grpc-demo/notes"
	"google.golang.org/grpc"
	"google.golang.org/grpc/credentials/insecure"
)

var (
	addr = flag.String("addr", "localhost:50051", "the address to connect to")
)

func main() {
	flag.Parse()
	// Set up a connection to the server.
	// TODO

	// Set up a connection to the server.
	conn, err := grpc.Dial(*addr, grpc.WithTransportCredentials(insecure.NewCredentials()))
	if err != nil {
		log.Fatalf("did not connect: %v", err)
	}
	defer conn.Close()
	c := notes.NewNotesClient(conn)

	// Define the context
	ctx, cancel := context.WithTimeout(context.Background(), time.Second)
	defer cancel()

	// define expected flag for save
	saveCmd := flag.NewFlagSet("save", flag.ExitOnError)
	saveTitle := saveCmd.String("title", "", "Give a title to your note")
	saveBody := saveCmd.String("content", "", "Type what you like to remember")

	//define expected flags for load
	loadCmd := flag.NewFlagSet("load", flag.ExitOnError)
	loadKeyword := loadCmd.String("keyword", "", "A keyword you'd like to find in your notes")

	if len(os.Args) < 2 {
		fmt.Println("expected 'save' or 'load' subcommands")
		os.Exit(1)
	}

	switch os.Args[1] {
	case "save":
		saveCmd.Parse(os.Args[2:])
		// Call the server
		// TODO

		_, err := c.Save(ctx, &notes.Note{
			Title: *saveTitle,
			Body:  []byte(*saveBody),
		})

		if err != nil {
			log.Fatalf("The note could not be saved: %v", err)
		}

		fmt.Printf("Your note was saved: %vn", *saveTitle)

	case "load":
		loadCmd.Parse(os.Args[2:])
		// Call the server
		// TODO
		note, err := c.Load(ctx, &notes.NoteSearch{
			Keyword: *loadKeyword,
		})

		if err != nil {
			log.Fatalf("The note could not be loaded: %v", err)
		}

		fmt.Printf("%vn", note)

	default:
		fmt.Println("Expected 'save' or 'load' subcommands")
		os.Exit(1)
	}
}

```

__Test__

    go run client/main.go save -title hello -content "Hello friend, where are you from?"