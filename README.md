# godotenv

[![Go Reference](https://pkg.go.dev/badge/github.com/sonnt85/godotenv.svg)](https://pkg.go.dev/github.com/sonnt85/godotenv)

A Go port of the Ruby dotenv library — loads environment variables from `.env` files.

## Installation

```bash
go get github.com/sonnt85/godotenv
```

## Features

- Load `.env` files into the process environment (`os.Setenv`)
- Overload mode to override existing environment variables
- Parse `.env` content from `io.Reader` or string into a `map[string]string`
- Support for `export KEY=VALUE` syntax and YAML-style `KEY: VALUE`
- Single and double quote handling with escape sequences and variable expansion
- Write/marshal environment maps back to `.env` format
- Execute commands with env loaded from a shell script's stdout
- Merge env maps with current process environment
- Run a command after loading env files (`Exec`)

## Usage

```go
package main

import (
    "fmt"
    "os"

    "github.com/sonnt85/godotenv"
)

func main() {
    // Load .env (does not override existing vars)
    godotenv.Load()

    // Load specific files
    godotenv.Load("production.env", "secrets.env")

    // Override existing environment variables
    godotenv.Overload(".env.local")

    // Read into map without setting env
    envMap, err := godotenv.Read(".env")
    if err == nil {
        fmt.Println(envMap["MY_VAR"])
    }

    // Parse from string
    envMap, _ = godotenv.Unmarshal("FOO=bar\nBAZ=qux")

    // Serialize map back to .env format
    content, _ := godotenv.Marshal(envMap)
    fmt.Println(content)

    // Load env from a shell script's stdout
    godotenv.LoadStdoutScript(`echo "DYNAMIC_VAR=hello"`)

    fmt.Println(os.Getenv("DYNAMIC_VAR"))
}
```

## API

- `Load(filenames ...string) error` — load `.env` file(s); does not override existing vars
- `Overload(filenames ...string) error` — load `.env` file(s), overriding existing vars
- `Read(filenames ...string) (map[string]string, error)` — read files into a map
- `Parse(r io.Reader) (map[string]string, error)` — parse env content from a reader
- `Unmarshal(str string) (map[string]string, error)` — parse env content from a string
- `Marshal(envMap map[string]string) (string, error)` — serialize map to dotenv format
- `Write(envMap map[string]string, filename string) error` — write map to file
- `Exec(filenames []string, cmd string, cmdArgs []string) error` — run a command after loading env files
- `LoadStdoutCmd(cmd string, cmdArgs ...string) error` — load env from a command's stdout
- `LoadStdoutScript(script string, overloads ...bool) error` — load env from a shell script's stdout
- `ParseStdoutScript(script string) (map[string]string, error)` — parse env from a shell script's stdout
- `ExeEnvFromStdoutScript(script, cmd string, cmdArgs ...string) error` — run a command with env from a shell script
- `EnvironmentMergeWithCurrentEnv(envMap map[string]string) []string` — merge map with current env as `KEY=VALUE` slice

## Author

**sonnt85** — [thanhson.rf@gmail.com](mailto:thanhson.rf@gmail.com)

## License

MIT License - see [LICENSE](LICENSE) for details.
