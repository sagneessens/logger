# Logger

A Go package that extends the built-in _log_ package. It enables the user to log to either or both a file and standard output or standard error. The user can also enable 4 different types of loggers (Debug, Info, Warning and Error).

## Details

It uses an initialisation function _init_ to configure everything the way the user wants to:

```
func Init(fileName string, multi bool, logFlag int, levelFlag int) {}
```

The first argument is the name of file where to logger will write to. This is just a string.

The second argument is a boolean that defines if the output should be written to the standard output and standard error as well as to the file specified in the previous argument.

The third argument is a flag that is passed as an integer. These flags define which text to prefix to each log entry generated by the Logger. See <https://golang.org/pkg/log/#pkg-constants> for more info. To use the standard prefix, pass `log.LstdFlags` to the _Init_ function.

The fourth argument is another flag passed as an integer. It is used to define which loggers should be enabled. The package uses the built-in _log_ package to make 4 new loggers (Debug, Info, Warning and Error):

```
// These flags define which loggers should be enabled.
const (
    LDebug = 1 << iota
    LInfo
    LWarning
    LError
)
```

So for example to enable _Warning_ and _Error_, you would have to pass `logger.LWarning|logger.LError` to the level flag parameter of the _Init_ function.

## Usage

```
package main

import (
  "log"

  "github.com/bullettime/logger"
)

func main() {
  // Initialize the logger to write the output to 'my_log_file.log' and the the standard output and standard error.
  // The logs will be prefixed with '2017/10/23 01:23:23 test.go: message'
  // All loggers except for Info are enabled.
  logger.Init("my_log_file.log", true, log.Ldate|log.Ltime|log.Lshortfile, logger.LDebug|logger.LWarning|logger.LError)

  // Should output something like 'Debug: 2017/10/23 01:23:23 test.go:16: Debug log test'
  logger.Debug.Println("Debug log test")

  // Should not output anything, this one isn't enabled!
  logger.Info.Println("Info log test")

  // Should output something like 'Debug: 2017/10/23 01:23:23 test.go:22: Warning log test'
  logger.Warning.Println("Warning log test")

  // Should output something like 'Debug: 2017/10/23 01:23:23 test.go:25: Error log test'
  logger.Error.Println("Error log test")
}
```