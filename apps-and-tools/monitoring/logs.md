# 👨‍💻 Logs

Logging refers to the act of recording during the execution of an application. Events could be user-generated or system-generated.

#### links

- [common log format](https://en.wikipedia.org/wiki/Common_Log_Format)

## Log levels

**Debug** logs are used to diagnose applications

**Info** is used to log important information about an application. It is used to log service start, service stop, configurations, assumptions.

**Warn** logs are considered to be the first level of application failure. Not affect the user at this point.

**Error** log level is used for more critical problems. These kinds of issues usually affect the result of the operation but do not terminate the program. Errors are considered to be the second level of application failures.

**Fatal** is the third level of application failures. It is used to indicate a much more serious error that causes the termination of the program. Such a message may say that the program totally failed.
