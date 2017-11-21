# docker-gui-host – a GUI-enabled Docker host VM
This Vagrant VM configuration is set up for experimenting with Docker images
with GUIs, by being a VM config running Docker and also having the GUI enabled
for Virtualbox. See for example
[my own attempts](https://hub.docker.com/r/fkrull/desktop-base).

## Usage
Start the VM:
```
vagrant up
```
The `vagrant` user can run Docker without `sudo`. The Docker daemon is also
exposed on the host's port 2376. To tell your local Docker client to
connect to the VM's daemon, either specify the `-H` parameter:
```
docker -H tcp://localhost:2376 ...
```
or by setting the `DOCKER_HOST` environment variable to `tcp://localhost:2376`.

A [Portainer instance](https://portainer.io) is running at
http://localhost:9000. The VM can also transparently run non-amd64 binaries,
using the
[fkrull/qemu-user-static](https://hub.docker.com/r/fkrull/qemu-user-static)
Docker image. This allows building and running foreign-architecture Docker
images, for example.

## License
Copyright (c) 2017 Felix Krull. All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

1. Redistributions of source code must retain the above copyright notice, this
list of conditions and the following disclaimer.

2. Redistributions in binary form must reproduce the above copyright notice,
this list of conditions and the following disclaimer in the documentation
and/or other materials provided with the distribution.

3. Neither the name of the copyright holder nor the names of its contributors
may be used to endorse or promote products derived from this software without
specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE
FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
