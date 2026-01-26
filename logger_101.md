# Logger python 101

To use logger in python `import logging`.

Levels of logging:

- Debug (10): for debugging and diagnostics, lowest level;
- Info (20): expected behaviour from code;
- Warning (30): events that require attention and can cause mulfanction in the
future, default loggin;
- Error (40): error messages about code modules that mulfanction;
- Critical (50): errors that can cause disruption in work of application.

```python
logging.debug("A DEBUG Message")
logging.info("An INFO")
logging.warning("A WARNING")
```

## Loggin into a file

```python
logging.basicConfig(level=logging.INFO, filename="py_log.log", filemode="w")
logging.debug("A DEBUG Message")
```

- level - is a level where logging starts, meaning `debug` will be ignored;
- filename - place where to log;
- filemode - "w" overwrites logs every start of the logger, "a" appends
to the existing logs and is default.

## Logging output

`<logging-level>:<name-of-the-logger>:<message>`, default logger name is `root`.

## Add time stemp

```python
logging.basicConfig(level=logging.INFO, filename="py_log.log", filemode="w",
                    format="%(asctime)s %(levelname)s %(message)s")
logging.debug("A DEBUG Message")
logging.info("An INFO")
```

## Base example of logging

```python
x = 3
y = 4

logging.info(f"The values of x and y are {x} adn {y}.")
try: 
    x/y
    logging.info(f"x/y succesful with result: {x/y}.")
except ZeroDivisionError as err:
    logging.error("ZeroDivisionError", exc_info=True)
```

to log exceptions the following availbable:

```python
...
except ZeroDivisionError as err:
    logging.exception("ZeroDivisionError")
```

## Setup custom user logger

```python
import logging

logger2 = logging.getLogger(__name__)
logger2.setLevel(logging.INFO)

# нфстройка обработчика и форматировщика для 
handler2 = logging.FileHandler(f"{__name__}.log", mode="w")
fomatter2 = logging.Formatter("%(name)s %(asctime)s %(levelname)s %(message)s")

# добавление форматировщика к обработчику
handler2.setFormatter(formatter2)
# добавление обработчика к логгеру
logger2.addHandler(handler2)

logger2.info(f"Testing the custom logger for module {__name__} ...")

def test_devision(x,y):
    try:
        x/y
        logger2.info(f"x/y successful with result: {x/y}.")
    except ZeroDivisionError as err:
        logger2.exception("ZerDivisionError")
```

## Comments

- Set appropriate level of logging and do not clutter the log file;
- Setup loggers for each seperate module and pass names of modules
to logs in order to indentify the error location;
- Create a unified logging;
- Add time stamp to logs;
- For large projects and files use `logging.handler.RotatingFileHandler(
filename, maxBytes, backupCount)`.
- For better logging and clearer error description use `sentry` integrated
with logger.
