---
title: "Structlog with Django is Awesome!"
date: 2024-03-06T12:04:59-05:00
draft: false
---

## Structlog
If you haven't heard about [structlog](https://www.structlog.org) yet, you are missing out! 
Structlog is this awesome python library that improves the standard python logging features.
You can struct your logs very easily as key=value or json, it also has a neat way of showing logs
when doing local development, like this:

![Console Renderer](/console_renderer.png)

## Django
It turns out it is very simple to have it working on Django as well, all you need is to add the following
to your `settings.py`:

```python
import structlog

LOGGING = {
    "version": 1,
    "disable_existing_loggers": False,
    "formatters": {
        "json_formatter": {
            "()": structlog.stdlib.ProcessorFormatter,
            "processor": structlog.processors.JSONRenderer(),
        },
        "plain_console": {
            "()": structlog.stdlib.ProcessorFormatter,
            "processor": structlog.dev.ConsoleRenderer(),
        },
        "key_value": {
            "()": structlog.stdlib.ProcessorFormatter,
            "processor": structlog.processors.KeyValueRenderer(key_order=['timestamp', 'level', 'event', 'logger']),
        },
    },
    "handlers": {
        "console": {
            "class": "logging.StreamHandler",
            "formatter": "plain_console"
        },
    },
    "root": {
        "handlers": ["console"],
        "level": "INFO",
    },
}

structlog.configure(
    processors=[
        structlog.stdlib.filter_by_level,
        structlog.processors.TimeStamper(fmt="iso"),
        structlog.stdlib.add_logger_name,
        structlog.stdlib.add_log_level,
        structlog.stdlib.PositionalArgumentsFormatter(),
        structlog.processors.StackInfoRenderer(),
        structlog.processors.format_exc_info,
        structlog.processors.UnicodeDecoder(),
        structlog.stdlib.ProcessorFormatter.wrap_for_formatter,
    ],
    context_class=dict,
    logger_factory=structlog.stdlib.LoggerFactory(),
    cache_logger_on_first_use=True
)
```

To check the other formatters you just need to change the formatter in the console handler.
This should give you enough to get started, and if you want to add more features like automatic
context make sure to check out [django-structlog](https://django-structlog.readthedocs.io/en/latest/index.html).

Now all you have to do is when you want to log something just make sure to use `structlog.get_logger` function
to get your configured logger, like this:

```python
import structlog

logger = structlog.get_logger()

logger.info("User logged in", user_id=request.user.id)
```

## Do you want to hire me?
Are you in need of a seasoned web developer with over 15 years of experience? Look no further! 
I've worked with numerous startups, playing a pivotal role in scaling engineering teams from 10 to 150 engineers. 
My expertise spans the entire software development lifecycle, from gathering requirements to implementing solutions, 
conducting automated testing, and setting up automated infrastructure and deployment processes.

Whether you're looking to tackle a specific project or need ongoing support, I'm here to help. 
Reach out to me at [hire@loop0.sh](mailto:hire@loop0.sh) to learn more about how my skills and experience can benefit your company. 
Let's discuss how I can contribute to your team's success!


`#python` `#django` `#go` `#rest` `#api` `#aws` `#kubernetes` `#cicd` `#redis` `#postgresql` `#mysql` `#linux`