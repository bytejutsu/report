## 2.5 Application Server

{% hint style="working" %}

Please note that an Application Server is broad term and its role depends on the framework or the script that is used on the server side.

Below are some examples of Application Servers for different programming languages

* For **Java**: Apache Tomcat is an Application Server that acts as a JAVA servlets container environment, and it manages concurrency through the use of thread workers from a thread pool.


* For **Python**: Gunicorn (Green Unicorn) is a WSGI Application Server that handles concurrency through a pre-fork worker model. It utilizes multiple worker processes to handle concurrent requests efficiently.


* For **PHP**: there is no standalone component known as an Application Server, However managing concurrency can be achieved with the use of a php process manager such as php-fpm (PHP FastCGI Process Manager)


{% endhint %}

{% hint style='info' %}

* Please note that in some cases, a production-ready Application Server comes embedded in the framework. So it is a part of the executed code itself. An example of that is the **ASP.NET** framework that comes with the **Kestrel** embedded production-ready Application Server.

{% endhint %}

{% hint style='danger'%}
Please note that I am talking about production-ready Application Servers and not Development Servers that comes shipped with most frameworks but can't handle concurrency.
{% endhint %}
