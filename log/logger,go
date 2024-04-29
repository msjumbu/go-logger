// can you find the errors in this?

package log

import (
	"fmt"
	"log"
	"os"
	"runtime"
	"strings"
)

type LogLevel int

const (
	DebugLevel LogLevel = iota
	InfoLevel
	WarnLevel
	ErrorLevel
	FatalLevel
	PanicLevel
)

var LogLevelString = "INFO"

func setLogLevel(level string) {
	switch strings.ToUpper(level) {
	case "DEBUG":
		LogLevelString = "DEBUG"
	case "INFO":
		LogLevelString = "INFO"
	case "WARN":
		LogLevelString = "WARN"
	case "ERROR":
		LogLevelString = "ERROR"
	case "FATAL":
		LogLevelString = "FATAL"
	}
}

func logMessage(logger *log.Logger, lvl LogLevel, format string, args ...interface{}) {
	if !shouldLog(lvl) {
		return
	}

	var output string
	if len(args) > 0 {
		output = fmt.Sprintf(format, args...)
	} else {
		output = format
	}

	_, f, l, _ := runtime.Caller(2)
	logger.Output(2, fmt.Sprintf("%s:%d %s", f, l, output))
}

func shouldLog(lvl LogLevel) bool {
	switch LogLevelString {
	case "DEBUG":
		return true
	case "INFO":
		return lvl >= InfoLevel
	case "WARN":
		return lvl >= WarnLevel
	case "ERROR":
		return lvl >= ErrorLevel
	default:
		return false // Or any desired default behavior
	}
}

// NewLogger creates a logger with the specified level
func NewLogger(lvl string) *Logger {
	LogLevelString = strings.ToUpper(lvl)
	return &Logger{
		debugLogger: log.New(os.Stdout, "DEBUG: ", log.LstdFlags),
		infoLogger:  log.New(os.Stdout, "INFO: ", log.LstdFlags),
		warnLogger:  log.New(os.Stdout, "WARN: ", log.LstdFlags),
		errorLogger: log.New(os.Stdout, "ERROR: ", log.LstdFlags),
	}
}

// Logger struct to hold a logger instance
type Logger struct {
	debugLogger *log.Logger
	infoLogger  *log.Logger
	warnLogger  *log.Logger
	errorLogger *log.Logger
}

// Methods for calling log levels on the Logger struct
func (l *Logger) Debug(format string, args ...interface{}) {
	logMessage(l.debugLogger, DebugLevel, format, args...)
}

func (l *Logger) Info(format string, args ...interface{}) {
	logMessage(l.infoLogger, InfoLevel, format, args...)
}

func (l *Logger) Warn(format string, args ...interface{}) {
	logMessage(l.warnLogger, WarnLevel, format, args...)
}

func (l *Logger) Error(format string, args ...interface{}) {
	logMessage(l.errorLogger, ErrorLevel, format, args...)
}

func (l *Logger) Fatal(format string, args ...interface{}) {
	logMessage(l.errorLogger, FatalLevel, format, args...)
	os.Exit(1)
}

func (l *Logger) Panic(format string, args ...interface{}) {
	logMessage(l.errorLogger, FatalLevel, format, args...)
	panic(fmt.Sprintf(format, args...))
}
