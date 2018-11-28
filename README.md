# .NET Core daemon

Test the app and check if shut down gracefully.

`dotnet build mydaemon`  
`dotnet run --project mydaemon --Daemon:DaemonName="Cool Daemon"`

```shell
Application started. Press Ctrl+C to shut down.
infoHosting environment: Production
Content root path: C:\Projects\POC\netcore-daemon-docker\mydaemon\bin\Debug\netcoreapp2.1\
: mydaemon.DaemonService[0]
      Starting daemon: Cool Daemon
info: mydaemon.DaemonService[0]
      Stopping daemon.
info: mydaemon.DaemonService[0]
      Disposing...
```

## Build and execution in Docker

Let's build this image, start a container, and then immediately stop it.

```shell
$ docker build -t mydaemon .
Sending build context to Docker daemon   1.38MB
Step 1/10 : FROM microsoft/dotnet:sdk AS build-env
 ---> 540aa875e6c2
...
Successfully built e602b6c46e70
Successfully tagged mydaemon:latest

$ docker run -d --name myapp mydaemon
d84f7164e3183fff544fa1c1ba405f3360f37b4265da0af1927da3346dd1bf16

$ docker inspect -f '{{.State.ExitCode}}' d84f7164e318
0
```

Our script received the SIGTERM sent by the docker stop command and exited cleanly with a 0 status.
