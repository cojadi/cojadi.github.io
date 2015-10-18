---
layout: post
title:  "Auto-restart dnx server with dnx-watch"
---

I'm starting a new api project using .net 5.  The first thing I did was install use yeoman to generate my project:

```
> npm install -g yo
> npm install -g generator-aspnet
> yo aspnet 
```

Next thing was to run the default project, get .net5 beta8 since I didn't already have it, restore nuget packages, and run the server.

```
> cd project-folder
> dnvm upgrade
> dnu update
> dnx web
```

Using Fiddler, I can `GET http://localhost:5000/api/values':

```
HTTP/1.1 200 OK
Date: Sun, 18 Oct 2015 23:19:43 GMT
Content-Type: application/json; charset=utf-8
Server: Kestrel
Content-Length: 19

["value1","value2"]
```

Perfect.  I'm up and running.  Let's change `ValueController.cs` to make sure that I can make changes to the project:

```c#
[HttpGet]
public IEnumerable<string> Get()
{
  return new string[] { "value1", "value2", "value3" };
}
```

Unfortunately the result in Fiddler is exactly the same.  Next I tried running the web server using `dnx --watch web`.  Sounds like it should watch my `.cs` files for changes. 
It does, but the result is that the server just stops.

Turns out there has been a recent release for this - dnx-watch.

```
> dnu commands install Microsoft.Dnx.Watcher 1.0.0-beta8
> dnx-watch web
```

Now when I `GET http://localhost/api/values` I get an array with 3 values.  And if I add a fourth item to the array in `ValuesController.cs` the dnx server automatically restarts and I see my changes in Fiddler right away.
