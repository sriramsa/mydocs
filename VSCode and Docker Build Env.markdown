---
layout: post
title: Vscode debugging inside docker
---


Building and debugging inside Docker container
----------------------------------------------
A very useful docker pattern is to use docker for building your services that needs compilation. Docker provides a consistent set of dependencies and environment for builds to come out consistent across your dev and build systems.

We use this pattern all across our projects with good success.

However, building inside the container isn't support natively by IDEs yet. Specifically VS Code in its current format does not support running builds/debugging sessions inside the containers. But is easy to set it up as such.

### Setting up build task to run inside container.
Add the below task to your **tasks.json**. DOCKER_BUILD_IMAGE_NAME would be the name of the build container.
```
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "BuildInDocker",
            "type": "shell",
            "command": "docker",
            "args": [
                "run",
                "--rm",
                "--net=host",
                "--volume=\"${HOME}\":\"${HOME}\"",
                "<DOCKER_BUILD_IMAGE_NAME>",
                "/bin/bash",
                "-c",
                "'mkdir -p ${workspaceFolder}/_build && cd ${workspaceFolder}/_build && cmake .. && make'"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
    ]
}
```
The task above starts the build container and runs your build command. The above example uses cmake to build and execute. 
