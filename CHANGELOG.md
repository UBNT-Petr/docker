# Changelog

Items starting with `DEPRECATE` are important deprecation notices. For more
information on the list of deprecated flags and APIs please have a look at
https://docs.docker.com/engine/deprecated/ where target removal dates can also
be found.

## 1.11.1 (2016-04-26)

### Distribution

- Fix schema2 manifest media type to be of type `application/vnd.docker.container.image.v1+json` ([#21949](https://github.com/docker/docker/pull/21949))

### Documentation

+ Add missing API documentation for changes introduced with 1.11.0 ([#22048](https://github.com/docker/docker/pull/22048))

### Builder

* Append label passed to `docker build` as arguments as an implicit `LABEL` command at the end of the processed `Dockerfile` ([#22184](https://github.com/docker/docker/pull/22184))

### Networking

- Fix a panic that would occur when forwarding DNS query ([#22261](https://github.com/docker/docker/pull/22261))
- Fix an issue where OS threads could end up within an incorrect network namespace when using user defined networks ([#22261](https://github.com/docker/docker/pull/22261))

### Runtime

- Fix a bug preventing labels configuration to be reloaded via the config file ([#22299](https://github.com/docker/docker/pull/22299))
- Fix a regression where container mounting `/var/run` would prevent other containers from being removed ([#22256](https://github.com/docker/docker/pull/22256))
- Fix an issue where it would be impossible to update both `memory-swap` and `memory` value together ([#22255](https://github.com/docker/docker/pull/22255))
- Fix a regression from 1.11.0 where the `/auth` endpoint would not initialize `serveraddress` if it is not provided ([#22254](https://github.com/docker/docker/pull/22254))
- Add missing cleanup of container temporary files when cancelling a schedule restart ([#22237](https://github.com/docker/docker/pull/22237))
- Removed scary error message when no restart policy is specified ([#21993](https://github.com/docker/docker/pull/21993))
- Fix a panic that would occur when the plugins were activated via the json spec ([#22191](https://github.com/docker/docker/pull/22191))
- Fix restart backoff logic to correctly reset delay if container ran for at least 10secs ([#22125](https://github.com/docker/docker/pull/22125))
- Remove error message when a container restart get cancelled ([#22123](https://github.com/docker/docker/pull/22123))
- Fix an issue where `docker` would not correcly clean up after `docker exec` ([#22121](https://github.com/docker/docker/pull/22121))
- Fix a panic that could occur when servicing concurrent `docker stats` commands ([#22120](https://github.com/docker/docker/pull/22120))`
- Revert deprecation of non-existing host directories auto-creation ([#22065](https://github.com/docker/docker/pull/22065))
- Hide misleading rpc error on daemon shutdown ([#22058](https://github.com/docker/docker/pull/22058))

## 1.11.0 (2016-04-13)

**IMPORTANT**: With Docker 1.11, a Linux docker installation is now made of 4 binaries (`docker`, [`docker-containerd`](https://github.com/docker/containerd), [`docker-containerd-shim`](https://github.com/docker/containerd) and [`docker-runc`](https://github.com/opencontainers/runc)). If you have scripts relying on docker being a single static binaries, please make sure to update them. Interaction with the daemon stay the same otherwise, the usage of the other binaries should be transparent. A Windows docker installation remains a single binary, `docker.exe`.

### Builder

- Fix a bug where Docker would not use the correct uid/gid when processing the `WORKDIR` command ([#21033](https://github.com/docker/docker/pull/21033))
- Fix a bug where copy operations with userns would not use the proper uid/gid ([#20782](https://github.com/docker/docker/pull/20782), [#21162](https://github.com/docker/docker/pull/21162))

### Client

* Usage of the `:` separator for security option has been deprecated. `=` should be used instead ([#21232](https://github.com/docker/docker/pull/21232))
+ The client user agent is now passed to the registry on `pull`, `build`, `push`, `login` and `search` operations ([#21306](https://github.com/docker/docker/pull/21306), [#21373](https://github.com/docker/docker/pull/21373))
* Allow setting the Domainname and Hostname separately through the API ([#20200](https://github.com/docker/docker/pull/20200))
* Docker info will now warn users if it can not detect the kernel version or the operating system ([#21128](https://github.com/docker/docker/pull/21128))
- Fix an issue where `docker stats --no-stream` output could be all 0s ([#20803](https://github.com/docker/docker/pull/20803))
- Fix a bug where some newly started container would not appear in a running `docker stats` command ([#20792](https://github.com/docker/docker/pull/20792))
* Post processing is no longer enabled for linux-cgo terminals ([#20587](https://github.com/docker/docker/pull/20587))
- Values to `--hostname` are now refused if they do not comply with [RFC1123](https://tools.ietf.org/html/rfc1123) ([#20566](https://github.com/docker/docker/pull/20566))
+ Docker learned how to use a SOCKS proxy ([#20366](https://github.com/docker/docker/pull/20366), [#18373](https://github.com/docker/docker/pull/18373))
+ Docker now supports external credential stores ([#20107](https://github.com/docker/docker/pull/20107))
* `docker ps` now supports displaying the list of volumes mounted inside a container ([#20017](https://github.com/docker/docker/pull/20017))
* `docker info` now also reports Docker's root directory location ([#19986](https://github.com/docker/docker/pull/19986))
- Docker now prohibits login in with an empty username (spaces are trimmed) ([#19806](https://github.com/docker/docker/pull/19806))
* Docker events attributes are now sorted by key ([#19761](https://github.com/docker/docker/pull/19761))
* `docker ps` no longer shows exported port for stopped containers ([#19483](https://github.com/docker/docker/pull/19483))
- Docker now cleans after itself if a save/export command fails ([#17849](https://github.com/docker/docker/pull/17849))
* Docker load learned how to display a progress bar ([#17329](https://github.com/docker/docker/pull/17329), [#120078](https://github.com/docker/docker/pull/20078))

### Distribution

- Fix a panic that occurred when pulling an image with 0 layers ([#21222](https://github.com/docker/docker/pull/21222))
- Fix a panic that could occur on error while pushing to a registry with a misconfigured token service ([#21212](https://github.com/docker/docker/pull/21212))
+ All first-level delegation roles are now signed when doing a trusted push ([#21046](https://github.com/docker/docker/pull/21046))
+ OAuth support for registries was added ([#20970](https://github.com/docker/docker/pull/20970))
* `docker login` now handles token using the implementation found in [docker/distribution](https://github.com/docker/distribution) ([#20832](https://github.com/docker/docker/pull/20832))
* `docker login` will no longer prompt for an email ([#20565](https://github.com/docker/docker/pull/20565))
* Docker will now fallback to registry V1 if no basic auth credentials are available ([#20241](https://github.com/docker/docker/pull/20241))
* Docker will now try to resume layer download where it left off after a network error/timeout ([#19840](https://github.com/docker/docker/pull/19840))
- Fix generated manifest mediaType when pushing cross-repository ([#19509](https://github.com/docker/docker/pull/19509))
- Fix docker requesting additional push credentials when pulling an image if Content Trust is enabled ([#20382](https://github.com/docker/docker/pull/20382))

### Logging

- Fix a race in the journald log driver ([#21311](https://github.com/docker/docker/pull/21311))
* Docker syslog driver now uses the RFC-5424 format when emitting logs ([#20121](https://github.com/docker/docker/pull/20121))
* Docker GELF log driver now allows to specify the compression algorithm and level via the `gelf-compression-type` and `gelf-compression-level` options ([#19831](https://github.com/docker/docker/pull/19831))
* Docker daemon learned to output uncolorized logs via the `--raw-logs` options ([#19794](https://github.com/docker/docker/pull/19794))
+ Docker, on Windows platform, now includes an ETW (Event Tracing in Windows) logging driver named `etwlogs` ([#19689](https://github.com/docker/docker/pull/19689))
* Journald log driver learned how to handle tags ([#19564](https://github.com/docker/docker/pull/19564))
+ The fluentd log driver learned the following options: `fluentd-address`, `fluentd-buffer-limit`, `fluentd-retry-wait`, `fluentd-max-retries` and `fluentd-async-connect` ([#19439](https://github.com/docker/docker/pull/19439))
+ Docker learned to send log to Google Cloud via the new `gcplogs` logging driver. ([#18766](https://github.com/docker/docker/pull/18766))


### Misc

+ When saving linked images together with `docker save` a subsequent `docker load` will correctly restore their parent/child relationship ([#21385](https://github.com/docker/docker/pull/c))
+ Support for building the Docker cli for OpenBSD was added ([#21325](https://github.com/docker/docker/pull/21325))
+ Labels can now be applied at network, volume and image creation ([#21270](https://github.com/docker/docker/pull/21270))
* The `dockremap` is now created as a system user ([#21266](https://github.com/docker/docker/pull/21266))
- Fix a few response body leaks ([#21258](https://github.com/docker/docker/pull/21258))
- Docker, when run as a service with systemd, will now properly manage its processes cgroups ([#20633](https://github.com/docker/docker/pull/20633))
* `docker info` now reports the value of cgroup KernelMemory or emits a warning if it is not supported ([#20863](https://github.com/docker/docker/pull/20863))
* `docker info` now also reports the cgroup driver in use ([#20388](https://github.com/docker/docker/pull/20388))
* Docker completion is now available on PowerShell ([#19894](https://github.com/docker/docker/pull/19894))
* `dockerinit` is no more ([#19490](https://github.com/docker/docker/pull/19490),[#19851](https://github.com/docker/docker/pull/19851))
+ Support for building Docker on arm64 was added ([#19013](https://github.com/docker/docker/pull/19013))
+ Experimental support for building docker.exe in a native Windows Docker installation ([#18348](https://github.com/docker/docker/pull/18348))

### Networking

- Fix panic if a node is forcibly removed from the cluster ([#21671](https://github.com/docker/docker/pull/21671))
- Fix "error creating vxlan interface" when starting a container in a Swarm cluster ([#21671](https://github.com/docker/docker/pull/21671))
* `docker network inspect` will now report all endpoints whether they have an active container or not ([#21160](https://github.com/docker/docker/pull/21160))
+ Experimental support for the MacVlan and IPVlan network drivers has been added ([#21122](https://github.com/docker/docker/pull/21122))
* Output of `docker network ls` is now sorted by network name ([#20383](https://github.com/docker/docker/pull/20383))
- Fix a bug where Docker would allow a network to be created with the reserved `default` name ([#19431](https://github.com/docker/docker/pull/19431))
* `docker network inspect` returns whether a network is internal or not ([#19357](https://github.com/docker/docker/pull/19357))
+ Control IPv6 via explicit option when creating a network (`docker network create --ipv6`). This shows up as a new `EnableIPv6` field in `docker network inspect` ([#17513](https://github.com/docker/docker/pull/17513))
* Support for AAAA Records (aka IPv6 Service Discovery) in embedded DNS Server ([#21396](https://github.com/docker/docker/pull/21396))
- Fix to not forward docker domain IPv6 queries to external servers ([#21396](https://github.com/docker/docker/pull/21396))
* Multiple A/AAAA records from embedded DNS Server for DNS Round robin ([#21019](https://github.com/docker/docker/pull/21019))
- Fix endpoint count inconsistency after an ungraceful dameon restart ([#21261](https://github.com/docker/docker/pull/21261))
- Move the ownership of exposed ports and port-mapping options from Endpoint to Sandbox ([#21019](https://github.com/docker/docker/pull/21019))
- Fixed a bug which prevents docker reload when host is configured with ipv6.disable=1 ([#21019](https://github.com/docker/docker/pull/21019))
- Added inbuilt nil IPAM driver ([#21019](https://github.com/docker/docker/pull/21019))
- Fixed bug in iptables.Exists() logic [#21019](https://github.com/docker/docker/pull/21019)
- Fixed a Veth interface leak when using overlay network ([#21019](https://github.com/docker/docker/pull/21019))
- Fixed a bug which prevents docker reload after a network delete during shutdown ([#20214](https://github.com/docker/docker/pull/20214))
- Make sure iptables chains are recreated on firewalld reload ([#20419](https://github.com/docker/docker/pull/20419))
- Allow to pass global datastore during config reload ([#20419](https://github.com/docker/docker/pull/20419))
- For anonymous containers use the alias name for IP to name mapping, ie:DNS PTR record ([#21019](https://github.com/docker/docker/pull/21019))
- Fix a panic when deleting an entry from /etc/hosts file  ([#21019](https://github.com/docker/docker/pull/21019))
- Source the forwarded DNS queries from the container net namespace  ([#21019](https://github.com/docker/docker/pull/21019))
- Fix to retain the network internal mode config for bridge networks on daemon reload ([#21780] (https://github.com/docker/docker/pull/21780))
- Fix to retain IPAM driver option configs on daemon reload ([#21914] (https://github.com/docker/docker/pull/21914))

### Plugins

- Fix a file descriptor leak that would occur every time plugins were enumerated ([#20686](https://github.com/docker/docker/pull/20686))
- Fix an issue where Authz plugin would corrupt the payload body when faced with a large amount of data ([#20602](https://github.com/docker/docker/pull/20602))

### Runtime

- Fix a panic that could occur when cleanup after a container started with invalid parameters ([#21716](https://github.com/docker/docker/pull/21716))
- Fix a race with event timers stopping early ([#21692](https://github.com/docker/docker/pull/21692))
- Fix race conditions in the layer store, potentially corrupting the map and crashing the process ([#21677](https://github.com/docker/docker/pull/21677))
- Un-deprecate auto-creation of host directories for mounts. This feature was marked deprecated in ([#21666](https://github.com/docker/docker/pull/21666))  
  Docker 1.9, but was decided to be too much of a backward-incompatible change, so it was decided to keep the feature.
+ It is now possible for containers to share the NET and IPC namespaces when `userns` is enabled ([#21383](https://github.com/docker/docker/pull/21383))
+ `docker inspect <image-id>` will now expose the rootfs layers ([#21370](https://github.com/docker/docker/pull/21370))
+ Docker Windows gained a minimal `top` implementation ([#21354](https://github.com/docker/docker/pull/21354))
* Docker learned to report the faulty exe when a container cannot be started due to its condition ([#21345](https://github.com/docker/docker/pull/21345))
* Docker with device mapper will now refuse to run if `udev sync` is not available ([#21097](https://github.com/docker/docker/pull/21097))
- Fix a bug where Docker would not validate the config file upon configuration reload ([#21089](https://github.com/docker/docker/pull/21089))
- Fix a hang that would happen on attach if initial start was to fail ([#21048](https://github.com/docker/docker/pull/21048))
- Fix an issue where registry service options in the daemon configuration file were not properly taken into account ([#21045](https://github.com/docker/docker/pull/21045))
- Fix a race between the exec and resize operations ([#21022](https://github.com/docker/docker/pull/21022))
- Fix an issue where nanoseconds were not correctly taken in account when filtering Docker events ([#21013](https://github.com/docker/docker/pull/21013))
- Fix the handling of Docker command when passed a 64 bytes id ([#21002](https://github.com/docker/docker/pull/21002))
* Docker will now return a `204` (i.e http.StatusNoContent) code when it successfully deleted a network ([#20977](https://github.com/docker/docker/pull/20977))
- Fix a bug where the daemon would wait indefinitely in case the process it was about to killed had already exited on its own ([#20967](https://github.com/docker/docker/pull/20967)
* The devmapper driver learned the `dm.min_free_space` option. If the mapped device free space reaches the passed value, new device creation will be prohibited. ([#20786](https://github.com/docker/docker/pull/20786))
+ Docker can now prevent processes in container to gain new privileges via the `--security-opt=no-new-privileges` flag ([#20727](https://github.com/docker/docker/pull/20727))
- Starting a container with the `--device` option will now correctly resolves symlinks ([#20684](https://github.com/docker/docker/pull/20684))
+ Docker now relies on [`containerd`](https://github.com/docker/containerd) and [`runc`](https://github.com/opencontainers/runc) to spawn containers. ([#20662](https://github.com/docker/docker/pull/20662))
- Fix docker configuration reloading to only alter value present in the given config file ([#20604](https://github.com/docker/docker/pull/20604))
+ Docker now allows setting a container hostname via the `--hostname` flag when `--net=host` ([#20177](https://github.com/docker/docker/pull/20177))
+ Docker now allows executing privileged container while running with `--userns-remap` if both `--privileged` and the new `--userns=host` flag are specified ([#20111](https://github.com/docker/docker/pull/20111))
- Fix Docker not cleaning up correctly old containers upon restarting after a crash ([#19679](https://github.com/docker/docker/pull/19679))
* Docker will now error out if it doesn't recognize a configuration key within the config file ([#19517](https://github.com/docker/docker/pull/19517))
- Fix container loading, on daemon startup, when they depends on a plugin running within a container ([#19500](https://github.com/docker/docker/pull/19500))
* `docker update` learned how to change a container restart policy ([#19116](https://github.com/docker/docker/pull/19116))
* `docker inspect` now also returns a new `State` field containing the container state in a human readable way (i.e. one of `created`, `restarting`, `running`, `paused`, `exited` or `dead`)([#18966](https://github.com/docker/docker/pull/18966))
+ Docker learned to limit the number of active pids (i.e. processes) within the container via the `pids-limit` flags. NOTE: This requires `CGROUP_PIDS=y` to be in the kernel configuration. ([#18697](https://github.com/docker/docker/pull/18697))
- `docker load` now has a `--quiet` option to suppress the load output ([#20078](https://github.com/docker/docker/pull/20078))
- Fix a bug in neighbor discovery for IPv6 peers ([#20842](https://github.com/docker/docker/pull/20842))
- Fix a panic during cleanup if a container was started with invalid options ([#21802](https://github.com/docker/docker/pull/21802))
- Fix a situation where a container cannot be stopped if the terminal is closed ([#21840](https://github.com/docker/docker/pull/21840))

### Security

* Object with the `pcp_pmcd_t` selinux type were given management access to `/var/lib/docker(/.*)?` ([#21370](https://github.com/docker/docker/pull/21370))
* `restart_syscall`, `copy_file_range`, `mlock2` joined the list of allowed calls in the default seccomp profile ([#21117](https://github.com/docker/docker/pull/21117), [#21262](https://github.com/docker/docker/pull/21262))
* `send`, `recv` and `x32` were added to the list of allowed syscalls and arch in the default seccomp profile ([#19432](https://github.com/docker/docker/pull/19432))
* Docker Content Trust now requests the server to perform snapshot signing ([#21046](https://github.com/docker/docker/pull/21046))
* Support for using YubiKeys for Content Trust signing has been moved out of experimental ([#21591](https://github.com/docker/docker/pull/21591))

### Volumes

* Output of `docker volume ls` is now sorted by volume name ([#20389](https://github.com/docker/docker/pull/20389))
* Local volumes can now accept options similar to the unix `mount` tool ([#20262](https://github.com/docker/docker/pull/20262))
- Fix an issue where one letter directory name could not be used as source for volumes ([#21106](https://github.com/docker/docker/pull/21106))
+ `docker run -v` now accepts a new flag `nocopy`. This tells the runtime not to copy the container path content into the volume (which is the default behavior) ([#21223](https://github.com/docker/docker/pull/21223))

## 1.10.3 (2016-03-10)

### Runtime

- Fix Docker client exiting with an "Unrecognized input header" error [#20706](https://github.com/docker/docker/pull/20706)
- Fix Docker exiting if Exec is started with both `AttachStdin` and `Detach` [#20647](https://github.com/docker/docker/pull/20647)

### Distribution

- Fix a crash when pushing multiple images sharing the same layers to the same repository in parallel [#20831](https://github.com/docker/docker/pull/20831)
- Fix a panic when pushing images to a registry which uses a misconfigured token service [#21030](https://github.com/docker/docker/pull/21030)

### Plugin system

- Fix issue preventing volume plugins to start when SELinux is enabled [#20834](https://github.com/docker/docker/pull/20834)
- Prevent Docker from exiting if a volume plugin returns a null response for Get requests [#20682](https://github.com/docker/docker/pull/20682)
- Fix plugin system leaking file descriptors if a plugin has an error [#20680](https://github.com/docker/docker/pull/20680)

### Security

- Fix linux32 emulation to fail during docker build [#20672](https://github.com/docker/docker/pull/20672)
  It was due to the `personality` syscall being blocked by the default seccomp profile.
- Fix Oracle XE 10g failing to start in a container [#20981](https://github.com/docker/docker/pull/20981)
  It was due to the `ipc` syscall being blocked by the default seccomp profile.
- Fix user namespaces not working on Linux From Scratch [#20685](https://github.com/docker/docker/pull/20685)
- Fix issue preventing daemon to start if userns is enabled and the `subuid` or `subgid` files contain comments [#20725](https://github.com/docker/docker/pull/20725)

## 1.10.2 (2016-02-22)

### Runtime

- Prevent systemd from deleting containers' cgroups when its configuration is reloaded [#20518](https://github.com/docker/docker/pull/20518)
- Fix SELinux issues by disregarding `--read-only` when mounting `/dev/mqueue` [#20333](https://github.com/docker/docker/pull/20333)
- Fix chown permissions used during `docker cp` when userns is used [#20446](https://github.com/docker/docker/pull/20446)
- Fix configuration loading issue with all booleans defaulting to `true` [#20471](https://github.com/docker/docker/pull/20471)
- Fix occasional panic with `docker logs -f` [#20522](https://github.com/docker/docker/pull/20522)

### Distribution

- Keep layer reference if deletion failed to avoid a badly inconsistent state [#20513](https://github.com/docker/docker/pull/20513)
- Handle gracefully a corner case when canceling migration [#20372](https://github.com/docker/docker/pull/20372)
- Fix docker import on compressed data [#20367](https://github.com/docker/docker/pull/20367)
- Fix tar-split files corruption during migration that later cause docker push and docker save to fail [#20458](https://github.com/docker/docker/pull/20458)

### Networking

- Fix daemon crash if embedded DNS is sent garbage [#20510](https://github.com/docker/docker/pull/20510)

### Volumes

- Fix issue with multiple volume references with same name [#20381](https://github.com/docker/docker/pull/20381)

### Security

- Fix potential cache corruption and delegation conflict issues [#20523](https://github.com/docker/docker/pull/20523)

## 1.10.1 (2016-02-11)

### Runtime

* Do not stop daemon on migration hard failure [#20156](https://github.com/docker/docker/pull/20156)
- Fix various issues with migration to content-addressable images [#20058](https://github.com/docker/docker/pull/20058)
- Fix ZFS permission bug with user namespaces [#20045](https://github.com/docker/docker/pull/20045)
- Do not leak /dev/mqueue from the host to all containers, keep it container-specific [#19876](https://github.com/docker/docker/pull/19876) [#20133](https://github.com/docker/docker/pull/20133)
- Fix `docker ps --filter before=...` to not show stopped containers without providing `-a` flag [#20135](https://github.com/docker/docker/pull/20135)

### Security

- Fix issue preventing docker events to work properly with authorization plugin [#20002](https://github.com/docker/docker/pull/20002)

### Distribution

* Add additional verifications and prevent from uploading invalid data to registries [#20164](https://github.com/docker/docker/pull/20164)
- Fix regression preventing uppercase characters in image reference hostname [#20175](https://github.com/docker/docker/pull/20175)

### Networking

- Fix embedded DNS for user-defined networks in the presence of firewalld [#20060](https://github.com/docker/docker/pull/20060)
- Fix issue where removing a network during shutdown left Docker inoperable [#20181](https://github.com/docker/docker/issues/20181) [#20235](https://github.com/docker/docker/issues/20235)
- Embedded DNS is now able to return compressed results [#20181](https://github.com/docker/docker/issues/20181)
- Fix port-mapping issue with `userland-proxy=false` [#20181](https://github.com/docker/docker/issues/20181)

### Logging

- Fix bug where tcp+tls protocol would be rejected [#20109](https://github.com/docker/docker/pull/20109)

### Volumes

- Fix issue whereby older volume drivers would not receive volume options [#19983](https://github.com/docker/docker/pull/19983)

### Misc

- Remove TasksMax from Docker systemd service [#20167](https://github.com/docker/docker/pull/20167)

## 1.10.0 (2016-02-04)

**IMPORTANT**: Docker 1.10 uses a new content-addressable storage for images and layers.
A migration is performed the first time docker is run, and can take a significant amount of time depending on the number of images present.
Refer to this page on the wiki for more information: https://github.com/docker/docker/wiki/Engine-v1.10.0-content-addressability-migration
We also released a cool migration utility that enables you to perform the migration before updating to reduce downtime.
Engine 1.10 migrator can be found on Docker Hub: https://hub.docker.com/r/docker/v1.10-migrator/

### Runtime

+ New `docker update` command that allows updating resource constraints on running containers [#15078](https://github.com/docker/docker/pull/15078)
+ Add `--tmpfs` flag to `docker run` to create a tmpfs mount in a container [#13587](https://github.com/docker/docker/pull/13587)
+ Add `--format` flag to `docker images` command [#17692](https://github.com/docker/docker/pull/17692)
+ Allow to set daemon configuration in a file and hot-reload it with the `SIGHUP` signal [#18587](https://github.com/docker/docker/pull/18587)
+ Updated docker events to include more meta-data and event types [#18888](https://github.com/docker/docker/pull/18888)
  This change is backward compatible in the API, but not on the CLI.
+ Add `--blkio-weight-device` flag to `docker run` [#13959](https://github.com/docker/docker/pull/13959)
+ Add `--device-read-bps` and `--device-write-bps` flags to `docker run` [#14466](https://github.com/docker/docker/pull/14466)
+ Add `--device-read-iops` and `--device-write-iops` flags to `docker run` [#15879](https://github.com/docker/docker/pull/15879)
+ Add `--oom-score-adj` flag to `docker run` [#16277](https://github.com/docker/docker/pull/16277)
+ Add `--detach-keys` flag to `attach`, `run`, `start` and `exec` commands to override the default key sequence that detaches from a container  [#15666](https://github.com/docker/docker/pull/15666)
+ Add `--shm-size` flag to `run`, `create` and `build` to set the size of `/dev/shm` [#16168](https://github.com/docker/docker/pull/16168)
+ Show the number of running, stopped, and paused containers in `docker info` [#19249](https://github.com/docker/docker/pull/19249)
+ Show the `OSType` and `Architecture` in `docker info` [#17478](https://github.com/docker/docker/pull/17478)
+ Add `--cgroup-parent` flag on `daemon` to set cgroup parent for all containers [#19062](https://github.com/docker/docker/pull/19062)
+ Add `-L` flag to docker cp to follow symlinks [#16613](https://github.com/docker/docker/pull/16613)
+ New `status=dead` filter for `docker ps` [#17908](https://github.com/docker/docker/pull/17908)
* Change `docker run` exit codes to distinguish between runtime and application errors [#14012](https://github.com/docker/docker/pull/14012)
* Enhance `docker events --since` and `--until` to support nanoseconds and timezones [#17495](https://github.com/docker/docker/pull/17495)
* Add `--all`/`-a` flag to `stats` to include both running and stopped containers [#16742](https://github.com/docker/docker/pull/16742)
* Change the default cgroup-driver to `cgroupfs` [#17704](https://github.com/docker/docker/pull/17704)
* Emit a "tag" event when tagging an image with `build -t` [#17115](https://github.com/docker/docker/pull/17115)
* Best effort for linked containers' start order when starting the daemon [#18208](https://github.com/docker/docker/pull/18208)
* Add ability to add multiple tags on `build` [#15780](https://github.com/docker/docker/pull/15780)
* Permit `OPTIONS` request against any url, thus fixing issue with CORS [#19569](https://github.com/docker/docker/pull/19569)
- Fix the `--quiet` flag on `docker build` to actually be quiet [#17428](https://github.com/docker/docker/pull/17428)
- Fix `docker images --filter dangling=false` to now show all non-dangling images [#19326](https://github.com/docker/docker/pull/19326)
- Fix race condition causing autorestart turning off on restart [#17629](https://github.com/docker/docker/pull/17629)
- Recognize GPFS filesystems [#19216](https://github.com/docker/docker/pull/19216)
- Fix obscure bug preventing to start containers [#19751](https://github.com/docker/docker/pull/19751)
- Forbid `exec` during container restart [#19722](https://github.com/docker/docker/pull/19722)
- devicemapper: Increasing `--storage-opt dm.basesize` will now increase the base device size on daemon restart [#19123](https://github.com/docker/docker/pull/19123)

### Security

+ Add `--userns-remap` flag to `daemon` to support user namespaces (previously in experimental) [#19187](https://github.com/docker/docker/pull/19187)
+ Add support for custom seccomp profiles in `--security-opt` [#17989](https://github.com/docker/docker/pull/17989)
+ Add default seccomp profile [#18780](https://github.com/docker/docker/pull/18780)
+ Add `--authorization-plugin` flag to `daemon` to customize ACLs [#15365](https://github.com/docker/docker/pull/15365)
+ Docker Content Trust now supports the ability to read and write user delegations [#18887](https://github.com/docker/docker/pull/18887)
  This is an optional, opt-in feature that requires the explicit use of the Notary command-line utility in order to be enabled.
  Enabling delegation support in a specific repository will break the ability of Docker 1.9 and 1.8 to pull from that repository, if content trust is enabled.
* Allow SELinux to run in a container when using the BTRFS storage driver [#16452](https://github.com/docker/docker/pull/16452)

### Distribution

* Use content-addressable storage for images and layers [#17924](https://github.com/docker/docker/pull/17924)
  Note that a migration is performed the first time docker is run; it can take a significant amount of time depending on the number of images and containers present.
  Images no longer depend on the parent chain but contain a list of layer references.
  `docker load`/`docker save` tarballs now also contain content-addressable image configurations.
  For more information: https://github.com/docker/docker/wiki/Engine-v1.10.0-content-addressability-migration
* Add support for the new [manifest format ("schema2")](https://github.com/docker/distribution/blob/master/docs/spec/manifest-v2-2.md) [#18785](https://github.com/docker/docker/pull/18785)
* Lots of improvements for push and pull: performance++, retries on failed downloads, cancelling on client disconnect [#18353](https://github.com/docker/docker/pull/18353), [#18418](https://github.com/docker/docker/pull/18418), [#19109](https://github.com/docker/docker/pull/19109), [#18353](https://github.com/docker/docker/pull/18353)
* Limit v1 protocol fallbacks [#18590](https://github.com/docker/docker/pull/18590)
- Fix issue where docker could hang indefinitely waiting for a nonexistent process to pull an image [#19743](https://github.com/docker/docker/pull/19743)

### Networking

+ Use DNS-based discovery instead of `/etc/hosts` [#19198](https://github.com/docker/docker/pull/19198)
+ Support for network-scoped alias using `--net-alias` on `run` and `--alias` on `network connect` [#19242](https://github.com/docker/docker/pull/19242)
+ Add `--ip` and `--ip6` on `run` and `network connect` to support custom IP addresses for a container in a network [#19001](https://github.com/docker/docker/pull/19001)
+ Add `--ipam-opt` to `network create` for passing custom IPAM options [#17316](https://github.com/docker/docker/pull/17316)
+ Add `--internal` flag to `network create` to restrict external access to and from the network [#19276](https://github.com/docker/docker/pull/19276)
+ Add `kv.path` option to `--cluster-store-opt` [#19167](https://github.com/docker/docker/pull/19167)
+ Add `discovery.heartbeat` and `discovery.ttl` options to `--cluster-store-opt` to configure discovery TTL and heartbeat timer [#18204](https://github.com/docker/docker/pull/18204)
+ Add `--format` flag to `network inspect` [#17481](https://github.com/docker/docker/pull/17481)
+ Add `--link` to `network connect` to provide a container-local alias [#19229](https://github.com/docker/docker/pull/19229)
+ Support for Capability exchange with remote IPAM plugins [#18775](https://github.com/docker/docker/pull/18775)
+ Add `--force` to `network disconnect` to force container to be disconnected from network [#19317](https://github.com/docker/docker/pull/19317)
* Support for multi-host networking using built-in overlay driver for all engine supported kernels: 3.10+ [#18775](https://github.com/docker/docker/pull/18775)
* `--link` is now supported on `docker run` for containers in user-defined network [#19229](https://github.com/docker/docker/pull/19229)
* Enhance `docker network rm` to allow removing multiple networks [#17489](https://github.com/docker/docker/pull/17489)
* Include container names in `network inspect` [#17615](https://github.com/docker/docker/pull/17615)
* Include auto-generated subnets for user-defined networks in `network inspect` [#17316](https://github.com/docker/docker/pull/17316)
* Add `--filter` flag to `network ls` to hide predefined networks [#17782](https://github.com/docker/docker/pull/17782)
* Add support for network connect/disconnect to stopped containers [#18906](https://github.com/docker/docker/pull/18906)
* Add network ID to container inspect [#19323](https://github.com/docker/docker/pull/19323)
- Fix MTU issue where Docker would not start with two or more default routes [#18108](https://github.com/docker/docker/pull/18108)
- Fix duplicate IP address for containers [#18106](https://github.com/docker/docker/pull/18106)
- Fix issue preventing sometimes docker from creating the bridge network [#19338](https://github.com/docker/docker/pull/19338)
- Do not substitute 127.0.0.1 name server when using `--net=host` [#19573](https://github.com/docker/docker/pull/19573)

### Logging

+ New logging driver for Splunk [#16488](https://github.com/docker/docker/pull/16488)
+ Add support for syslog over TCP+TLS [#18998](https://github.com/docker/docker/pull/18998)
* Enhance `docker logs --since` and `--until` to support nanoseconds and time [#17495](https://github.com/docker/docker/pull/17495)
* Enhance AWS logs to auto-detect region [#16640](https://github.com/docker/docker/pull/16640)

### Volumes

+ Add support to set the mount propagation mode for a volume [#17034](https://github.com/docker/docker/pull/17034)
* Add `ls` and `inspect` endpoints to volume plugin API [#16534](https://github.com/docker/docker/pull/16534)
  Existing plugins need to make use of these new APIs to satisfy users' expectation
  For that, please use the new MIME type `application/vnd.docker.plugins.v1.2+json` [#19549](https://github.com/docker/docker/pull/19549)
- Fix data not being copied to named volumes [#19175](https://github.com/docker/docker/pull/19175)
- Fix issues preventing volume drivers from being containerized [#19500](https://github.com/docker/docker/pull/19500)
- Fix `docker volumes ls --dangling=false` to now show all non-dangling volumes [#19671](https://github.com/docker/docker/pull/19671)
- Do not remove named volumes on container removal [#19568](https://github.com/docker/docker/pull/19568)
- Allow external volume drivers to host anonymous volumes [#19190](https://github.com/docker/docker/pull/19190)

### Builder

+ Add support for `**` in `.dockerignore` to wildcard multiple levels of directories [#17090](https://github.com/docker/docker/pull/17090)
- Fix handling of UTF-8 characters in Dockerfiles [#17055](https://github.com/docker/docker/pull/17055)
- Fix permissions problem when reading from STDIN [#19283](https://github.com/docker/docker/pull/19283)

### Client

+ Add support for overriding the API version to use via an `DOCKER_API_VERSION` environment-variable [#15964](https://github.com/docker/docker/pull/15964)
- Fix a bug preventing Windows clients to log in to Docker Hub [#19891](https://github.com/docker/docker/pull/19891)

### Misc

* systemd: Set TasksMax in addition to LimitNPROC in systemd service file [#19391](https://github.com/docker/docker/pull/19391)

### Deprecations

* Remove LXC support. The LXC driver was deprecated in Docker 1.8, and has now been removed [#17700](https://github.com/docker/docker/pull/17700)
* Remove `--exec-driver` daemon flag, because it is no longer in use [#17700](https://github.com/docker/docker/pull/17700)
* Remove old deprecated single-dashed long CLI flags (such as `-rm`; use `--rm` instead) [#17724](https://github.com/docker/docker/pull/17724)
* Deprecate HostConfig at API container start [#17799](https://github.com/docker/docker/pull/17799)
* Deprecate docker packages for newly EOL'd Linux distributions: Fedora 21 and Ubuntu 15.04 (Vivid) [#18794](https://github.com/docker/docker/pull/18794), [#18809](https://github.com/docker/docker/pull/18809)
* Deprecate `-f` flag for docker tag [#18350](https://github.com/docker/docker/pull/18350)

## 1.9.1 (2015-11-21)

### Runtime

- Do not prevent daemon from booting if images could not be restored (#17695)
- Force IPC mount to unmount on daemon shutdown/init (#17539)
- Turn IPC unmount errors into warnings (#17554)
- Fix `docker stats` performance regression (#17638)
- Clarify cryptic error message upon `docker logs` if `--log-driver=none` (#17767)
- Fix seldom panics (#17639, #17634, #17703)
- Fix opq whiteouts problems for files with dot prefix (#17819)
- devicemapper: try defaulting to xfs instead of ext4 for performance reasons (#17903, #17918)
- devicemapper: fix displayed fs in docker info (#17974)
- selinux: only relabel if user requested so with the `z` option (#17450, #17834)
- Do not make network calls when normalizing names (#18014)

### Client

- Fix `docker login` on windows (#17738)
- Fix bug with `docker inspect` output when not connected to daemon (#17715)
- Fix `docker inspect -f {{.HostConfig.Dns}} somecontainer` (#17680)

### Builder

- Fix regression with symlink behavior in ADD/COPY (#17710)

### Networking

- Allow passing a network ID as an argument for `--net` (#17558)
- Fix connect to host and prevent disconnect from host for `host` network (#17476)
- Fix `--fixed-cidr` issue when gateway ip falls in ip-range and ip-range is
  not the first block in the network (#17853)
- Restore deterministic `IPv6` generation from `MAC` address on default `bridge` network (#17890)
- Allow port-mapping only for endpoints created on docker run (#17858)
- Fixed an endpoint delete issue with a possible stale sbox (#18102)

### Distribution

- Correct parent chain in v2 push when v1Compatibility files on the disk are inconsistent (#18047)

## 1.9.0 (2015-11-03)

### Runtime

+ `docker stats` now returns block IO metrics (#15005)
+ `docker stats` now details network stats per interface (#15786)
+ Add `ancestor=<image>` filter to `docker ps --filter` flag to filter
containers based on their ancestor images (#14570)
+ Add `label=<somelabel>` filter to `docker ps --filter` to filter containers
based on label (#16530)
+ Add `--kernel-memory` flag to `docker run` (#14006)
+ Add `--message` flag to `docker import` allowing to specify an optional
message (#15711)
+ Add `--privileged` flag to `docker exec` (#14113)
+ Add `--stop-signal` flag to `docker run` allowing to replace the container
process stopping signal (#15307)
+ Add a new `unless-stopped` restart policy (#15348)
+ Inspecting an image now returns tags (#13185)
+ Add container size information to `docker inspect` (#15796)
+ Add `RepoTags` and `RepoDigests` field to `/images/{name:.*}/json` (#17275)
- Remove the deprecated `/container/ps` endpoint from the API (#15972)
- Send and document correct HTTP codes for `/exec/<name>/start` (#16250)
- Share shm and mqueue between containers sharing IPC namespace (#15862)
- Event stream now shows OOM status when `--oom-kill-disable` is set (#16235)
- Ensure special network files (/etc/hosts etc.) are read-only if bind-mounted
with `ro` option (#14965)
- Improve `rmi` performance (#16890)
- Do not update /etc/hosts for the default bridge network, except for links (#17325)
- Fix conflict with duplicate container names (#17389)
- Fix an issue with incorrect template execution in `docker inspect` (#17284)
- DEPRECATE `-c` short flag variant for `--cpu-shares` in docker run (#16271)

### Client

+ Allow `docker import` to import from local files (#11907)

### Builder

+ Add a `STOPSIGNAL` Dockerfile instruction allowing to set a different
stop-signal for the container process (#15307)
+ Add an `ARG` Dockerfile instruction and a `--build-arg` flag to `docker build`
that allows to add build-time environment variables (#15182)
- Improve cache miss performance (#16890)

### Storage

- devicemapper: Implement deferred deletion capability (#16381)

## Networking

+ `docker network` exits experimental and is part of standard release (#16645)
+ New network top-level concept, with associated subcommands and API (#16645)
  WARNING: the API is different from the experimental API
+ Support for multiple isolated/micro-segmented networks (#16645)
+ Built-in multihost networking using VXLAN based overlay driver (#14071)
+ Support for third-party network plugins (#13424)
+ Ability to dynamically connect containers to multiple networks (#16645)
+ Support for user-defined IP address management via pluggable IPAM drivers (#16910)
+ Add daemon flags `--cluster-store` and `--cluster-advertise` for built-in nodes discovery (#16229)
+ Add `--cluster-store-opt` for setting up TLS settings (#16644)
+ Add `--dns-opt` to the daemon (#16031)
- DEPRECATE following container `NetworkSettings` fields in API v1.21: `EndpointID`, `Gateway`,
  `GlobalIPv6Address`, `GlobalIPv6PrefixLen`, `IPAddress`, `IPPrefixLen`, `IPv6Gateway` and `MacAddress`.
  Those are now specific to the `bridge` network. Use `NetworkSettings.Networks` to inspect
  the networking settings of a container per network.

### Volumes

+ New top-level `volume` subcommand and API (#14242)
- Move API volume driver settings to host-specific config (#15798)
- Print an error message if volume name is not unique (#16009)
- Ensure volumes created from Dockerfiles always use the local volume driver
(#15507)
- DEPRECATE auto-creating missing host paths for bind mounts (#16349)

### Logging

+ Add `awslogs` logging driver for Amazon CloudWatch (#15495)
+ Add generic `tag` log option to allow customizing container/image
information passed to driver (e.g. show container names) (#15384)
- Implement the `docker logs` endpoint for the journald driver (#13707)
- DEPRECATE driver-specific log tags (e.g. `syslog-tag`, etc.) (#15384)

### Distribution

+ `docker search` now works with partial names (#16509)
- Push optimization: avoid buffering to file (#15493)
- The daemon will display progress for images that were already being pulled
by another client (#15489)
- Only permissions required for the current action being performed are requested (#)
+ Renaming trust keys (and respective environment variables) from `offline` to
`root` and `tagging` to `repository` (#16894)
- DEPRECATE trust key environment variables
`DOCKER_CONTENT_TRUST_OFFLINE_PASSPHRASE` and
`DOCKER_CONTENT_TRUST_TAGGING_PASSPHRASE` (#16894)

### Security

+ Add SELinux profiles to the rpm package (#15832)
- Fix various issues with AppArmor profiles provided in the deb package
(#14609)
- Add AppArmor policy that prevents writing to /proc (#15571)

## 1.8.3 (2015-10-12)

### Distribution

- Fix layer IDs lead to local graph poisoning (CVE-2014-8178)
- Fix manifest validation and parsing logic errors allow pull-by-digest validation bypass (CVE-2014-8179)
+ Add `--disable-legacy-registry` to prevent a daemon from using a v1 registry

## 1.8.2 (2015-09-10)

### Distribution

- Fixes rare edge case of handling GNU LongLink and LongName entries.
- Fix ^C on docker pull.
- Fix docker pull issues on client disconnection.
- Fix issue that caused the daemon to panic when loggers weren't configured properly.
- Fix goroutine leak pulling images from registry V2.

### Runtime

- Fix a bug mounting cgroups for docker daemons running inside docker containers.
- Initialize log configuration properly.

### Client:

- Handle `-q` flag in `docker ps` properly when there is a default format.

### Networking

- Fix several corner cases with netlink.

### Contrib

- Fix several issues with bash completion.

## 1.8.1 (2015-08-12)

### Distribution

* Fix a bug where pushing multiple tags would result in invalid images

## 1.8.0 (2015-08-11)

### Distribution

+ Trusted pull, push and build, disabled by default
* Make tar layers deterministic between registries
* Don't allow deleting the image of running containers
* Check if a tag name to load is a valid digest
* Allow one character repository names
* Add a more accurate error description for invalid tag name
* Make build cache ignore mtime

### Cli

+ Add support for DOCKER_CONFIG/--config to specify config file dir
+ Add --type flag  for docker inspect command
+ Add formatting options to `docker ps` with `--format`
+ Replace `docker -d` with new subcommand `docker daemon`
* Zsh completion updates and improvements
* Add some missing events to bash completion
* Support daemon urls with base paths in `docker -H`
* Validate status= filter to docker ps
* Display when a container is in --net=host in docker ps
* Extend docker inspect to export image metadata related to graph driver
* Restore --default-gateway{,-v6} daemon options
* Add missing unpublished ports in docker ps
* Allow duration strings in `docker events` as --since/--until
* Expose more mounts information in `docker inspect`

### Runtime

+ Add new Fluentd logging driver
+ Allow `docker import` to load from local files
+ Add logging driver for GELF via UDP
+ Allow to copy files from host to containers with `docker cp`
+ Promote volume drivers from experimental to master
+ Add rollover options to json-file log driver, and --log-driver-opts flag
+ Add memory swappiness tuning options
* Remove cgroup read-only flag when privileged
* Make /proc, /sys, & /dev readonly for readonly containers
* Add cgroup bind mount by default
* Overlay: Export metadata for container and image in `docker inspect`
* Devicemapper: external device activation
* Devicemapper: Compare uuid of base device on startup
* Remove RC4 from the list of registry cipher suites
* Add syslog-facility option
* LXC execdriver compatibility with recent LXC versions
* Mark LXC execriver as deprecated (to be removed with the migration to runc)

### Plugins

* Separate plugin sockets and specs locations
* Allow TLS connections to plugins

### Bug fixes

- Add missing 'Names' field to /containers/json API output
- Make `docker rmi` of dangling images safe while pulling
- Devicemapper: Change default basesize to 100G
- Go Scheduler issue with sync.Mutex and gcc
- Fix issue where Search API endpoint would panic due to empty AuthConfig
- Set image canonical names correctly
- Check dockerinit only if lxc driver is used
- Fix ulimit usage of nproc
- Always attach STDIN if -i,--interactive is specified
- Show error messages when saving container state fails
- Fixed incorrect assumption on --bridge=none treated as disable network
- Check for invalid port specifications in host configuration
- Fix endpoint leave failure for --net=host mode
- Fix goroutine leak in the stats API if the container is not running
- Check for apparmor file before reading it
- Fix DOCKER_TLS_VERIFY being ignored
- Set umask to the default on startup
- Correct the message of pause and unpause a non-running container
- Adjust disallowed CpuShares in container creation
- ZFS: correctly apply selinux context
- Display empty string instead of <nil> when IP opt is nil
- `docker kill` returns error when container is not running
- Fix COPY/ADD quoted/json form
- Fix goroutine leak on logs -f with no output
- Remove panic in nat package on invalid hostport
- Fix container linking in Fedora 22
- Fix error caused using default gateways outside of the allocated range
- Format times in inspect command with a template as RFC3339Nano
- Make registry client to accept 2xx and 3xx http status responses as successful
- Fix race issue that caused the daemon to crash with certain layer downloads failed in a specific order.
- Fix error when the docker ps format was not valid.
- Remove redundant ip forward check.
- Fix issue trying to push images to repository mirrors.
- Fix error cleaning up network entrypoints when there is an initialization issue.

## 1.7.1 (2015-07-14)

#### Runtime

- Fix default user spawning exec process with `docker exec`
- Make `--bridge=none` not to configure the network bridge
- Publish networking stats properly
- Fix implicit devicemapper selection with static binaries
- Fix socket connections that hung intermittently
- Fix bridge interface creation on CentOS/RHEL 6.6
- Fix local dns lookups added to resolv.conf
- Fix copy command mounting volumes
- Fix read/write privileges in volumes mounted with --volumes-from

#### Remote API

- Fix unmarshalling of Command and Entrypoint
- Set limit for minimum client version supported
- Validate port specification
- Return proper errors when attach/reattach fail

#### Distribution

- Fix pulling private images
- Fix fallback between registry V2 and V1

## 1.7.0 (2015-06-16)

#### Runtime
+ Experimental feature: support for out-of-process volume plugins
* The userland proxy can be disabled in favor of hairpin NAT using the daemon’s `--userland-proxy=false` flag
* The `exec` command supports the `-u|--user` flag to specify the new process owner
+ Default gateway for containers can be specified daemon-wide using the `--default-gateway` and `--default-gateway-v6` flags
+ The CPU CFS (Completely Fair Scheduler) quota can be set in `docker run` using `--cpu-quota`
+ Container block IO can be controlled in `docker run` using`--blkio-weight`
+ ZFS support
+ The `docker logs` command supports a `--since` argument
+ UTS namespace can be shared with the host with `docker run --uts=host`

#### Quality
* Networking stack was entirely rewritten as part of the libnetwork effort
* Engine internals refactoring
* Volumes code was entirely rewritten to support the plugins effort
+ Sending SIGUSR1 to a daemon will dump all goroutines stacks without exiting

#### Build
+ Support ${variable:-value} and ${variable:+value} syntax for environment variables
+ Support resource management flags `--cgroup-parent`, `--cpu-period`, `--cpu-quota`, `--cpuset-cpus`, `--cpuset-mems`
+ git context changes with branches and directories
* The .dockerignore file support exclusion rules

#### Distribution
+ Client support for v2 mirroring support for the official registry

#### Bugfixes
* Firewalld is now supported and will automatically be used when available
* mounting --device recursively

## 1.6.2 (2015-05-13)

####  Runtime
- Revert change prohibiting mounting into /sys

## 1.6.1 (2015-05-07)

####  Security
- Fix read/write /proc paths (CVE-2015-3630)
- Prohibit VOLUME /proc and VOLUME / (CVE-2015-3631)
- Fix opening of file-descriptor 1 (CVE-2015-3627)
- Fix symlink traversal on container respawn allowing local privilege escalation (CVE-2015-3629)
- Prohibit mount of /sys

#### Runtime
- Update AppArmor policy to not allow mounts

## 1.6.0 (2015-04-07)

#### Builder
+ Building images from an image ID
+ Build containers with resource constraints, ie `docker build --cpu-shares=100 --memory=1024m...`
+ `commit --change` to apply specified Dockerfile instructions while committing the image
+ `import --change` to apply specified Dockerfile instructions while importing the image
+ Builds no longer continue in the background when canceled with CTRL-C

#### Client
+ Windows Support

#### Runtime
+ Container and image Labels
+ `--cgroup-parent` for specifying a parent cgroup to place container cgroup within
+ Logging drivers, `json-file`, `syslog`, or `none`
+ Pulling images by ID
+ `--ulimit` to set the ulimit on a container
+ `--default-ulimit` option on the daemon which applies to all created containers (and overwritten by `--ulimit` on run)

## 1.5.0 (2015-02-10)

#### Builder
+ Dockerfile to use for a given `docker build` can be specified with the `-f` flag
* Dockerfile and .dockerignore files can be themselves excluded as part of the .dockerignore file, thus preventing modifications to these files invalidating ADD or COPY instructions cache
* ADD and COPY instructions accept relative paths
* Dockerfile `FROM scratch` instruction is now interpreted as a no-base specifier
* Improve performance when exposing a large number of ports

#### Hack
+ Allow client-side only integration tests for Windows
* Include docker-py integration tests against Docker daemon as part of our test suites

#### Packaging
+ Support for the new version of the registry HTTP API
* Speed up `docker push` for images with a majority of already existing layers
- Fixed contacting a private registry through a proxy

#### Remote API
+ A new endpoint will stream live container resource metrics and can be accessed with the `docker stats` command
+ Containers can be renamed using the new `rename` endpoint and the associated `docker rename` command
* Container `inspect` endpoint show the ID of `exec` commands running in this container
* Container `inspect` endpoint show the number of times Docker auto-restarted the container
* New types of event can be streamed by the `events` endpoint: ‘OOM’ (container died with out of memory), ‘exec_create’, and ‘exec_start'
- Fixed returned string fields which hold numeric characters incorrectly omitting surrounding double quotes

#### Runtime
+ Docker daemon has full IPv6 support
+ The `docker run` command can take the `--pid=host` flag to use the host PID namespace, which makes it possible for example to debug host processes using containerized debugging tools
+ The `docker run` command can take the `--read-only` flag to make the container’s root filesystem mounted as readonly, which can be used in combination with volumes to force a container’s processes to only write to locations that will be persisted
+ Container total memory usage can be limited for `docker run` using the `--memory-swap` flag
* Major stability improvements for devicemapper storage driver
* Better integration with host system: containers will reflect changes to the host's `/etc/resolv.conf` file when restarted
* Better integration with host system: per-container iptable rules are moved to the DOCKER chain
- Fixed container exiting on out of memory to return an invalid exit code

#### Other
* The HTTP_PROXY, HTTPS_PROXY, and NO_PROXY environment variables are properly taken into account by the client when connecting to the Docker daemon

## 1.4.1 (2014-12-15)

#### Runtime
- Fix issue with volumes-from and bind mounts not being honored after create

## 1.4.0 (2014-12-11)

#### Notable Features since 1.3.0
+ Set key=value labels to the daemon (displayed in `docker info`), applied with
  new `-label` daemon flag
+ Add support for `ENV` in Dockerfile of the form:
  `ENV name=value name2=value2...`
+ New Overlayfs Storage Driver
+ `docker info` now returns an `ID` and `Name` field
+ Filter events by event name, container, or image
+ `docker cp` now supports copying from container volumes
- Fixed `docker tag`, so it honors `--force` when overriding a tag for existing
  image.

## 1.3.3 (2014-12-11)

#### Security
- Fix path traversal vulnerability in processing of absolute symbolic links (CVE-2014-9356)
- Fix decompression of xz image archives, preventing privilege escalation (CVE-2014-9357)
- Validate image IDs (CVE-2014-9358)

#### Runtime
- Fix an issue when image archives are being read slowly

#### Client
- Fix a regression related to stdin redirection
- Fix a regression with `docker cp` when destination is the current directory

## 1.3.2 (2014-11-20)

#### Security
- Fix tar breakout vulnerability
* Extractions are now sandboxed chroot
- Security options are no longer committed to images

#### Runtime
- Fix deadlock in `docker ps -f exited=1`
- Fix a bug when `--volumes-from` references a container that failed to start

#### Registry
+ `--insecure-registry` now accepts CIDR notation such as 10.1.0.0/16
* Private registries whose IPs fall in the 127.0.0.0/8 range do no need the `--insecure-registry` flag
- Skip the experimental registry v2 API when mirroring is enabled

## 1.3.1 (2014-10-28)

#### Security
* Prevent fallback to SSL protocols < TLS 1.0 for client, daemon and registry
+ Secure HTTPS connection to registries with certificate verification and without HTTP fallback unless `--insecure-registry` is specified

#### Runtime
- Fix issue where volumes would not be shared

#### Client
- Fix issue with `--iptables=false` not automatically setting `--ip-masq=false`
- Fix docker run output to non-TTY stdout

#### Builder
- Fix escaping `$` for environment variables
- Fix issue with lowercase `onbuild` Dockerfile instruction
- Restrict environment variable expansion to `ENV`, `ADD`, `COPY`, `WORKDIR`, `EXPOSE`, `VOLUME` and `USER`

## 1.3.0 (2014-10-14)

#### Notable features since 1.2.0
+ Docker `exec` allows you to run additional processes inside existing containers
+ Docker `create` gives you the ability to create a container via the CLI without executing a process
+ `--security-opts` options to allow user to customize container labels and apparmor profiles
+ Docker `ps` filters
- Wildcard support to COPY/ADD
+ Move production URLs to get.docker.com from get.docker.io
+ Allocate IP address on the bridge inside a valid CIDR
+ Use drone.io for PR and CI testing
+ Ability to setup an official registry mirror
+ Ability to save multiple images with docker `save`

## 1.2.0 (2014-08-20)

#### Runtime
+ Make /etc/hosts /etc/resolv.conf and /etc/hostname editable at runtime
+ Auto-restart containers using policies
+ Use /var/lib/docker/tmp for large temporary files
+ `--cap-add` and `--cap-drop` to tweak what linux capability you want
+ `--device` to use devices in containers

#### Client
+ `docker search` on private registries
+ Add `exited` filter to `docker ps --filter`
* `docker rm -f` now kills instead of stop
+ Support for IPv6 addresses in `--dns` flag

#### Proxy
+ Proxy instances in separate processes
* Small bug fix on UDP proxy

## 1.1.2 (2014-07-23)

#### Runtime
+ Fix port allocation for existing containers
+ Fix containers restart on daemon restart

#### Packaging
+ Fix /etc/init.d/docker issue on Debian

## 1.1.1 (2014-07-09)

#### Builder
* Fix issue with ADD

## 1.1.0 (2014-07-03)

#### Notable features since 1.0.1
+ Add `.dockerignore` support
+ Pause containers during `docker commit`
+ Add `--tail` to `docker logs`

#### Builder
+ Allow a tar file as context for `docker build`
* Fix issue with white-spaces and multi-lines in `Dockerfiles`

#### Runtime
* Overall performance improvements
* Allow `/` as source of `docker run -v`
* Fix port allocation
* Fix bug in `docker save`
* Add links information to `docker inspect`

#### Client
* Improve command line parsing for `docker commit`

#### Remote API
* Improve status code for the `start` and `stop` endpoints

## 1.0.1 (2014-06-19)

#### Notable features since 1.0.0
* Enhance security for the LXC driver

#### Builder
* Fix `ONBUILD` instruction passed to grandchildren

#### Runtime
* Fix events subscription
* Fix /etc/hostname file with host networking
* Allow `-h` and `--net=none`
* Fix issue with hotplug devices in `--privileged`

#### Client
* Fix artifacts with events
* Fix a panic with empty flags
* Fix `docker cp` on Mac OS X

#### Miscellaneous
* Fix compilation on Mac OS X
* Fix several races

## 1.0.0 (2014-06-09)

#### Notable features since 0.12.0
* Production support

## 0.12.0 (2014-06-05)

#### Notable features since 0.11.0
* 40+ various improvements to stability, performance and usability
* New `COPY` Dockerfile instruction to allow copying a local file from the context into the container without ever extracting if the file is a tar file
* Inherit file permissions from the host on `ADD`
* New `pause` and `unpause` commands to allow pausing and unpausing of containers using cgroup freezer
* The `images` command has a `-f`/`--filter` option to filter the list of images
* Add `--force-rm` to clean up after a failed build
* Standardize JSON keys in Remote API to CamelCase
* Pull from a docker run now assumes `latest` tag if not specified
* Enhance security on Linux capabilities and device nodes

## 0.11.1 (2014-05-07)

#### Registry
- Fix push and pull to private registry

## 0.11.0 (2014-05-07)

#### Notable features since 0.10.0

* SELinux support for mount and process labels
* Linked containers can be accessed by hostname
* Use the net `--net` flag to allow advanced network configuration such as host networking so that containers can use the host's network interfaces
* Add a ping endpoint to the Remote API to do healthchecks of your docker daemon
* Logs can now be returned with an optional timestamp
* Docker now works with registries that support SHA-512
* Multiple registry endpoints are supported to allow registry mirrors

## 0.10.0 (2014-04-08)

#### Builder
- Fix printing multiple messages on a single line. Fixes broken output during builds.
- Follow symlinks inside container's root for ADD build instructions.
- Fix EXPOSE caching.

#### Documentation
- Add the new options of `docker ps` to the documentation.
- Add the options of `docker restart` to the documentation.
- Update daemon docs and help messages for --iptables and --ip-forward.
- Updated apt-cacher-ng docs example.
- Remove duplicate description of --mtu from docs.
- Add missing -t and -v for `docker images` to the docs.
- Add fixes to the cli docs.
- Update libcontainer docs.
- Update images in docs to remove references to AUFS and LXC.
- Update the nodejs_web_app in the docs to use the new epel RPM address.
- Fix external link on security of containers.
- Update remote API docs.
- Add image size to history docs.
- Be explicit about binding to all interfaces in redis example.
- Document DisableNetwork flag in the 1.10 remote api.
- Document that `--lxc-conf` is lxc only.
- Add chef usage documentation.
- Add example for an image with multiple for `docker load`.
- Explain what `docker run -a` does in the docs.

#### Contrib
- Add variable for DOCKER_LOGFILE to sysvinit and use append instead of overwrite in opening the logfile.
- Fix init script cgroup mounting workarounds to be more similar to cgroupfs-mount and thus work properly.
- Remove inotifywait hack from the upstart host-integration example because it's not necessary any more.
- Add check-config script to contrib.
- Fix fish shell completion.

#### Hack
* Clean up "go test" output from "make test" to be much more readable/scannable.
* Exclude more "definitely not unit tested Go source code" directories from hack/make/test.
+ Generate md5 and sha256 hashes when building, and upload them via hack/release.sh.
- Include contributed completions in Ubuntu PPA.
+ Add cli integration tests.
* Add tweaks to the hack scripts to make them simpler.

#### Remote API
+ Add TLS auth support for API.
* Move git clone from daemon to client.
- Fix content-type detection in docker cp.
* Split API into 2 go packages.

#### Runtime
* Support hairpin NAT without going through Docker server.
- devicemapper: succeed immediately when removing non-existing devices.
- devicemapper: improve handling of devicemapper devices (add per device lock, increase sleep time and unlock while sleeping).
- devicemapper: increase timeout in waitClose to 10 seconds.
- devicemapper: ensure we shut down thin pool cleanly.
- devicemapper: pass info, rather than hash to activateDeviceIfNeeded, deactivateDevice, setInitialized, deleteDevice.
- devicemapper: avoid AB-BA deadlock.
- devicemapper: make shutdown better/faster.
- improve alpha sorting in mflag.
- Remove manual http cookie management because the cookiejar is being used.
- Use BSD raw mode on Darwin. Fixes nano, tmux and others.
- Add FreeBSD support for the client.
- Merge auth package into registry.
- Add deprecation warning for -t on `docker pull`.
- Remove goroutine leak on error.
- Update parseLxcInfo to comply with new lxc1.0 format.
- Fix attach exit on darwin.
- Improve deprecation message.
- Retry to retrieve the layer metadata up to 5 times for `docker pull`.
- Only unshare the mount namespace for execin.
- Merge existing config when committing.
- Disable daemon startup timeout.
- Fix issue #4681: add loopback interface when networking is disabled.
- Add failing test case for issue #4681.
- Send SIGTERM to child, instead of SIGKILL.
- Show the driver and the kernel version in `docker info` even when not in debug mode.
- Always symlink /dev/ptmx for libcontainer. This fixes console related problems.
- Fix issue caused by the absence of /etc/apparmor.d.
- Don't leave empty cidFile behind when failing to create the container.
- Mount cgroups automatically if they're not mounted already.
- Use mock for search tests.
- Update to double-dash everywhere.
- Move .dockerenv parsing to lxc driver.
- Move all bind-mounts in the container inside the namespace.
- Don't use separate bind mount for container.
- Always symlink /dev/ptmx for libcontainer.
- Don't kill by pid for other drivers.
- Add initial logging to libcontainer.
* Sort by port in `docker ps`.
- Move networking drivers into runtime top level package.
+ Add --no-prune to `docker rmi`.
+ Add time since exit in `docker ps`.
- graphdriver: add build tags.
- Prevent allocation of previously allocated ports & prevent improve port allocation.
* Add support for --since/--before in `docker ps`.
- Clean up container stop.
+ Add support for configurable dns search domains.
- Add support for relative WORKDIR instructions.
- Add --output flag for docker save.
- Remove duplication of DNS entries in config merging.
- Add cpuset.cpus to cgroups and native driver options.
- Remove docker-ci.
- Promote btrfs. btrfs is no longer considered experimental.
- Add --input flag to `docker load`.
- Return error when existing bridge doesn't match IP address.
- Strip comments before parsing line continuations to avoid interpreting instructions as comments.
- Fix TestOnlyLoopbackExistsWhenUsingDisableNetworkOption to ignore "DOWN" interfaces.
- Add systemd implementation of cgroups and make containers show up as systemd units.
- Fix commit and import when no repository is specified.
- Remount /var/lib/docker as --private to fix scaling issue.
- Use the environment's proxy when pinging the remote registry.
- Reduce error level from harmless errors.
* Allow --volumes-from to be individual files.
- Fix expanding buffer in StdCopy.
- Set error regardless of attach or stdin. This fixes #3364.
- Add support for --env-file to load environment variables from files.
- Symlink /etc/mtab and /proc/mounts.
- Allow pushing a single tag.
- Shut down containers cleanly at shutdown and wait forever for the containers to shut down. This makes container shutdown on daemon shutdown work properly via SIGTERM.
- Don't throw error when starting an already running container.
- Fix dynamic port allocation limit.
- remove setupDev from libcontainer.
- Add API version to `docker version`.
- Return correct exit code when receiving signal and make SIGQUIT quit without cleanup.
- Fix --volumes-from mount failure.
- Allow non-privileged containers to create device nodes.
- Skip login tests because of external dependency on a hosted service.
- Deprecate `docker images --tree` and `docker images --viz`.
- Deprecate `docker insert`.
- Include base abstraction for apparmor. This fixes some apparmor related problems on Ubuntu 14.04.
- Add specific error message when hitting 401 over HTTP on push.
- Fix absolute volume check.
- Remove volumes-from from the config.
- Move DNS options to hostconfig.
- Update the apparmor profile for libcontainer.
- Add deprecation notice for `docker commit -run`.

## 0.9.1 (2014-03-24)

#### Builder
- Fix printing multiple messages on a single line. Fixes broken output during builds.

#### Documentation
- Fix external link on security of containers.

#### Contrib
- Fix init script cgroup mounting workarounds to be more similar to cgroupfs-mount and thus work properly.
- Add variable for DOCKER_LOGFILE to sysvinit and use append instead of overwrite in opening the logfile.

#### Hack
- Generate md5 and sha256 hashes when building, and upload them via hack/release.sh.

#### Remote API
- Fix content-type detection in `docker cp`.

#### Runtime
- Use BSD raw mode on Darwin. Fixes nano, tmux and others.
- Only unshare the mount namespace for execin.
- Retry to retrieve the layer metadata up to 5 times for `docker pull`.
- Merge existing config when committing.
- Fix panic in monitor.
- Disable daemon startup timeout.
- Fix issue #4681: add loopback interface when networking is disabled.
- Add failing test case for issue #4681.
- Send SIGTERM to child, instead of SIGKILL.
- Show the driver and the kernel version in `docker info` even when not in debug mode.
- Always symlink /dev/ptmx for libcontainer. This fixes console related problems.
- Fix issue caused by the absence of /etc/apparmor.d.
- Don't leave empty cidFile behind when failing to create the container.
- Improve deprecation message.
- Fix attach exit on darwin.
- devicemapper: improve handling of devicemapper devices (add per device lock, increase sleep time, unlock while sleeping).
- devicemapper: succeed immediately when removing non-existing devices.
- devicemapper: increase timeout in waitClose to 10 seconds.
- Remove goroutine leak on error.
- Update parseLxcInfo to comply with new lxc1.0 format.

## 0.9.0 (2014-03-10)

#### Builder
- Avoid extra mount/unmount during build. This fixes mount/unmount related errors during build.
- Add error to docker build --rm. This adds missing error handling.
- Forbid chained onbuild, `onbuild from` and  `onbuild maintainer` triggers.
- Make `--rm` the default for `docker build`.

#### Documentation
- Download the docker client binary for Mac over https.
- Update the titles of the install instructions & descriptions.
* Add instructions for upgrading boot2docker.
* Add port forwarding example in OS X install docs.
- Attempt to disentangle repository and registry.
- Update docs to explain more about `docker ps`.
- Update sshd example to use a Dockerfile.
- Rework some examples, including the Python examples.
- Update docs to include instructions for a container's lifecycle.
- Update docs documentation to discuss the docs branch.
- Don't skip cert check for an example & use HTTPS.
- Bring back the memory and swap accounting section which was lost when the kernel page was removed.
- Explain DNS warnings and how to fix them on systems running and using a local nameserver.

#### Contrib
- Add Tanglu support for mkimage-debootstrap.
- Add SteamOS support for mkimage-debootstrap.

#### Hack
- Get package coverage when running integration tests.
- Remove the Vagrantfile. This is being replaced with boot2docker.
- Fix tests on systems where aufs isn't available.
- Update packaging instructions and remove the dependency on lxc.

#### Remote API
* Move code specific to the API to the api package.
- Fix header content type for the API. Makes all endpoints use proper content type.
- Fix registry auth & remove ping calls from CmdPush and CmdPull.
- Add newlines to the JSON stream functions.

#### Runtime
* Do not ping the registry from the CLI. All requests to registries flow through the daemon.
- Check for nil information return in the lxc driver. This fixes panics with older lxc versions.
- Devicemapper: cleanups and fix for unmount. Fixes two problems which were causing unmount to fail intermittently.
- Devicemapper: remove directory when removing device. Directories don't get left behind when removing the device.
* Devicemapper: enable skip_block_zeroing. Improves performance by not zeroing blocks.
- Devicemapper: fix shutdown warnings. Fixes shutdown warnings concerning pool device removal.
- Ensure docker cp stream is closed properly. Fixes problems with files not being copied by `docker cp`.
- Stop making `tcp://` default to `127.0.0.1:4243` and remove the default port for tcp.
- Fix `--run` in `docker commit`. This makes `docker commit --run` work again.
- Fix custom bridge related options. This makes custom bridges work again.
+ Mount-bind the PTY as container console. This allows tmux/screen to run.
+ Add the pure Go libcontainer library to make it possible to run containers using only features of the Linux kernel.
+ Add native exec driver which uses libcontainer and make it the default exec driver.
- Add support for handling extended attributes in archives.
* Set the container MTU to be the same as the host MTU.
+ Add simple sha256 checksums for layers to speed up `docker push`.
* Improve kernel version parsing.
* Allow flag grouping (`docker run -it`).
- Remove chroot exec driver.
- Fix divide by zero to fix panic.
- Rewrite `docker rmi`.
- Fix docker info with lxc 1.0.0.
- Fix fedora tty with apparmor.
* Don't always append env vars, replace defaults with vars from config.
* Fix a goroutine leak.
* Switch to Go 1.2.1.
- Fix unique constraint error checks.
* Handle symlinks for Docker's data directory and for TMPDIR.
- Add deprecation warnings for flags (-flag is deprecated in favor of --flag)
- Add apparmor profile for the native execution driver.
* Move system specific code from archive to pkg/system.
- Fix duplicate signal for `docker run -i -t` (issue #3336).
- Return correct process pid for lxc.
- Add a -G option to specify the group which unix sockets belong to.
+ Add `-f` flag to `docker rm` to force removal of running containers.
+ Kill ghost containers and restart all ghost containers when the docker daemon restarts.
+ Add `DOCKER_RAMDISK` environment variable to make Docker work when the root is on a ramdisk.

## 0.8.1 (2014-02-18)

#### Builder

- Avoid extra mount/unmount during build. This removes an unneeded mount/unmount operation which was causing problems with devicemapper
- Fix regression with ADD of tar files. This stops Docker from decompressing tarballs added via ADD from the local file system
- Add error to `docker build --rm`. This adds a missing error check to ensure failures to remove containers are detected and reported

#### Documentation

* Update issue filing instructions
* Warn against the use of symlinks for Docker's storage folder
* Replace the Firefox example with an IceWeasel example
* Rewrite the PostgresSQL example using a Dockerfile and add more details to it
* Improve the OS X documentation

#### Remote API

- Fix broken images API for version less than 1.7
- Use the right encoding for all API endpoints which return JSON
- Move remote api client to api/
- Queue calls to the API using generic socket wait

#### Runtime

- Fix the use of custom settings for bridges and custom bridges
- Refactor the devicemapper code to avoid many mount/unmount race conditions and failures
- Remove two panics which could make Docker crash in some situations
- Don't ping registry from the CLI client
- Enable skip_block_zeroing for devicemapper. This stops devicemapper from always zeroing entire blocks
- Fix --run in `docker commit`. This makes docker commit store `--run` in the image configuration
- Remove directory when removing devicemapper device. This cleans up leftover mount directories
- Drop NET_ADMIN capability for non-privileged containers. Unprivileged containers can't change their network configuration
- Ensure `docker cp` stream is closed properly
- Avoid extra mount/unmount during container registration. This removes an unneeded mount/unmount operation which was causing problems with devicemapper
- Stop allowing tcp:// as a default tcp bin address which binds to 127.0.0.1:4243 and remove the default port
+ Mount-bind the PTY as container console. This allows tmux and screen to run in a container
- Clean up archive closing. This fixes and improves archive handling
- Fix engine tests on systems where temp directories are symlinked
- Add test methods for save and load
- Avoid temporarily unmounting the container when restarting it. This fixes a race for devicemapper during restart
- Support submodules when building from a GitHub repository
- Quote volume path to allow spaces
- Fix remote tar ADD behavior. This fixes a regression which was causing Docker to extract tarballs

## 0.8.0 (2014-02-04)

#### Notable features since 0.7.0

* Images and containers can be removed much faster
* Building an image from source with docker build is now much faster
* The Docker daemon starts and stops much faster
* The memory footprint of many common operations has been reduced, by streaming files instead of buffering them in memory, fixing memory leaks, and fixing various suboptimal memory allocations
* Several race conditions were fixed, making Docker more stable under very high concurrency load. This makes Docker more stable and less likely to crash and reduces the memory footprint of many common operations
* All packaging operations are now built on the Go language’s standard tar implementation, which is bundled with Docker itself. This makes packaging more portable across host distributions, and solves several issues caused by quirks and incompatibilities between different distributions of tar
* Docker can now create, remove and modify larger numbers of containers and images graciously thanks to more aggressive releasing of system resources. For example the storage driver API now allows Docker to do reference counting on mounts created by the drivers
With the ongoing changes to the networking and execution subsystems of docker testing these areas have been a focus of the refactoring.  By moving these subsystems into separate packages we can test, analyze, and monitor coverage and quality of these packages
* Many components have been separated into smaller sub-packages, each with a dedicated test suite. As a result the code is better-tested, more readable and easier to change

* The ADD instruction now supports caching, which avoids unnecessarily re-uploading the same source content again and again when it hasn’t changed
* The new ONBUILD instruction adds to your image a “trigger” instruction to be executed at a later time, when the image is used as the base for another build
* Docker now ships with an experimental storage driver which uses the BTRFS filesystem for copy-on-write
* Docker is officially supported on Mac OS X
* The Docker daemon supports systemd socket activation

## 0.7.6 (2014-01-14)

#### Builder

* Do not follow symlink outside of build context

#### Runtime

- Remount bind mounts when ro is specified
* Use https for fetching docker version

#### Other

* Inline the test.docker.io fingerprint
* Add ca-certificates to packaging documentation

## 0.7.5 (2014-01-09)

#### Builder

* Disable compression for build. More space usage but a much faster upload
- Fix ADD caching for certain paths
- Do not compress archive from git build

#### Documentation

- Fix error in GROUP add example
* Make sure the GPG fingerprint is inline in the documentation
* Give more specific advice on setting up signing of commits for DCO

#### Runtime

- Fix misspelled container names
- Do not add hostname when networking is disabled
* Return most recent image from the cache by date
- Return all errors from docker wait
* Add Content-Type Header "application/json" to GET /version and /info responses

#### Other

* Update DCO to version 1.1
+ Update Makefile to use "docker:GIT_BRANCH" as the generated image name
* Update Travis to check for new 1.1 DCO version

## 0.7.4 (2014-01-07)

#### Builder

- Fix ADD caching issue with . prefixed path
- Fix docker build on devicemapper by reverting sparse file tar option
- Fix issue with file caching and prevent wrong cache hit
* Use same error handling while unmarshalling CMD and ENTRYPOINT

#### Documentation

* Simplify and streamline Amazon Quickstart
* Install instructions use unprefixed Fedora image
* Update instructions for mtu flag for Docker on GCE
+ Add Ubuntu Saucy to installation
- Fix for wrong version warning on master instead of latest

#### Runtime

- Only get the image's rootfs when we need to calculate the image size
- Correctly handle unmapping UDP ports
* Make CopyFileWithTar use a pipe instead of a buffer to save memory on docker build
- Fix login message to say pull instead of push
- Fix "docker load" help by removing "SOURCE" prompt and mentioning STDIN
* Make blank -H option default to the same as no -H was sent
* Extract cgroups utilities to own submodule

#### Other

+ Add Travis CI configuration to validate DCO and gofmt requirements
+ Add Developer Certificate of Origin Text
* Upgrade VBox Guest Additions
* Check standalone header when pinging a registry server

## 0.7.3 (2014-01-02)

#### Builder

+ Update ADD to use the image cache, based on a hash of the added content
* Add error message for empty Dockerfile

#### Documentation

- Fix outdated link to the "Introduction" on www.docker.io
+ Update the docs to get wider when the screen does
- Add information about needing to install LXC when using raw binaries
* Update Fedora documentation to disentangle the docker and docker.io conflict
* Add a note about using the new `-mtu` flag in several GCE zones
+ Add FrugalWare installation instructions
+ Add a more complete example of `docker run`
- Fix API documentation for creating and starting Privileged containers
- Add missing "name" parameter documentation on "/containers/create"
* Add a mention of `lxc-checkconfig` as a way to check for some of the necessary kernel configuration
- Update the 1.8 API documentation with some additions that were added to the docs for 1.7

#### Hack

- Add missing libdevmapper dependency to the packagers documentation
* Update minimum Go requirement to a hard line at Go 1.2+
* Many minor improvements to the Vagrantfile
+ Add ability to customize dockerinit search locations when compiling (to be used very sparingly only by packagers of platforms who require a nonstandard location)
+ Add coverprofile generation reporting
- Add `-a` to our Go build flags, removing the need for recompiling the stdlib manually
* Update Dockerfile to be more canonical and have less spurious warnings during build
- Fix some miscellaneous `docker pull` progress bar display issues
* Migrate more miscellaneous packages under the "pkg" folder
* Update TextMate highlighting to automatically be enabled for files named "Dockerfile"
* Reorganize syntax highlighting files under a common "contrib/syntax" directory
* Update install.sh script (https://get.docker.io/) to not fail if busybox fails to download or run at the end of the Ubuntu/Debian installation
* Add support for container names in bash completion

#### Packaging

+ Add an official Docker client binary for Darwin (Mac OS X)
* Remove empty "Vendor" string and added "License" on deb package
+ Add a stubbed version of "/etc/default/docker" in the deb package

#### Runtime

* Update layer application to extract tars in place, avoiding file churn while handling whiteouts
- Fix permissiveness of mtime comparisons in tar handling (since GNU tar and Go tar do not yet support sub-second mtime precision)
* Reimplement `docker top` in pure Go to work more consistently, and even inside Docker-in-Docker (thus removing the shell injection vulnerability present in some versions of `lxc-ps`)
+ Update `-H unix://` to work similarly to `-H tcp://` by inserting the default values for missing portions
- Fix more edge cases regarding dockerinit and deleted or replaced docker or dockerinit files
* Update container name validation to include '.'
- Fix use of a symlink or non-absolute path as the argument to `-g` to work as expected
* Update to handle external mounts outside of LXC, fixing many small mounting quirks and making future execution backends and other features simpler
* Update to use proper box-drawing characters everywhere in `docker images -tree`
* Move MTU setting from LXC configuration to directly use netlink
* Add `-S` option to external tar invocation for more efficient spare file handling
+ Add arch/os info to User-Agent string, especially for registry requests
+ Add `-mtu` option to Docker daemon for configuring MTU
- Fix `docker build` to exit with a non-zero exit code on error
+ Add `DOCKER_HOST` environment variable to configure the client `-H` flag without specifying it manually for every invocation

## 0.7.2 (2013-12-16)

#### Runtime

+ Validate container names on creation with standard regex
* Increase maximum image depth to 127 from 42
* Continue to move api endpoints to the job api
+ Add -bip flag to allow specification of dynamic bridge IP via CIDR
- Allow bridge creation when ipv6 is not enabled on certain systems
* Set hostname and IP address from within dockerinit
* Drop capabilities from within dockerinit
- Fix volumes on host when symlink is present the image
- Prevent deletion of image if ANY container is depending on it even if the container is not running
* Update docker push to use new progress display
* Use os.Lstat to allow mounting unix sockets when inspecting volumes
- Adjust handling of inactive user login
- Add missing defines in devicemapper for older kernels
- Allow untag operations with no container validation
- Add auth config to docker build

#### Documentation

* Add more information about Docker logging
+ Add RHEL documentation
* Add a direct example for changing the CMD that is run in a container
* Update Arch installation documentation
+ Add section on Trusted Builds
+ Add Network documentation page

#### Other

+ Add new cover bundle for providing code coverage reporting
* Separate integration tests in bundles
* Make Tianon the hack maintainer
* Update mkimage-debootstrap with more tweaks for keeping images small
* Use https to get the install script
* Remove vendored dotcloud/tar now that Go 1.2 has been released

## 0.7.1 (2013-12-05)

#### Documentation

+ Add @SvenDowideit as documentation maintainer
+ Add links example
+ Add documentation regarding ambassador pattern
+ Add Google Cloud Platform docs
+ Add dockerfile best practices
* Update doc for RHEL
* Update doc for registry
* Update Postgres examples
* Update doc for Ubuntu install
* Improve remote api doc

#### Runtime

+ Add hostconfig to docker inspect
+ Implement `docker log -f` to stream logs
+ Add env variable to disable kernel version warning
+ Add -format to `docker inspect`
+ Support bind-mount for files
- Fix bridge creation on RHEL
- Fix image size calculation
- Make sure iptables are called even if the bridge already exists
- Fix issue with stderr only attach
- Remove init layer when destroying a container
- Fix same port binding on different interfaces
- `docker build` now returns the correct exit code
- Fix `docker port` to display correct port
- `docker build` now check that the dockerfile exists client side
- `docker attach` now returns the correct exit code
- Remove the name entry when the container does not exist

#### Registry

* Improve progress bars, add ETA for downloads
* Simultaneous pulls now waits for the first to finish instead of failing
- Tag only the top-layer image when pushing to registry
- Fix issue with offline image transfer
- Fix issue preventing using ':' in password for registry

#### Other

+ Add pprof handler for debug
+ Create a Makefile
* Use stdlib tar that now includes fix
* Improve make.sh test script
* Handle SIGQUIT on the daemon
* Disable verbose during tests
* Upgrade to go1.2 for official build
* Improve unit tests
* The test suite now runs all tests even if one fails
* Refactor C in Go (Devmapper)
- Fix OS X compilation

## 0.7.0 (2013-11-25)

#### Notable features since 0.6.0

* Storage drivers: choose from aufs, device-mapper, or vfs.
* Standard Linux support: docker now runs on unmodified Linux kernels and all major distributions.
* Links: compose complex software stacks by connecting containers to each other.
* Container naming: organize your containers by giving them memorable names.
* Advanced port redirects: specify port redirects per interface, or keep sensitive ports private.
* Offline transfer: push and pull images to the filesystem without losing information.
* Quality: numerous bugfixes and small usability improvements. Significant increase in test coverage.

## 0.6.7 (2013-11-21)

#### Runtime

* Improve stability, fixes some race conditions
* Skip the volumes mounted when deleting the volumes of container.
* Fix layer size computation: handle hard links correctly
* Use the work Path for docker cp CONTAINER:PATH
* Fix tmp dir never cleanup
* Speedup docker ps
* More informative error message on name collisions
* Fix nameserver regex
* Always return long id's
* Fix container restart race condition
* Keep published ports on docker stop;docker start
* Fix container networking on Fedora
* Correctly express "any address" to iptables
* Fix network setup when reconnecting to ghost container
* Prevent deletion if image is used by a running container
* Lock around read operations in graph

#### RemoteAPI

* Return full ID on docker rmi

#### Client

+ Add -tree option to images
+ Offline image transfer
* Exit with status 2 on usage error and display usage on stderr
* Do not forward SIGCHLD to container
* Use string timestamp for docker events -since

#### Other

* Update to go 1.2rc5
+ Add /etc/default/docker support to upstart

## 0.6.6 (2013-11-06)

#### Runtime

* Ensure container name on register
* Fix regression in /etc/hosts
+ Add lock around write operations in graph
* Check if port is valid
* Fix restart runtime error with ghost container networking
+ Add some more colors and animals to increase the pool of generated names
* Fix issues in docker inspect
+ Escape apparmor confinement
+ Set environment variables using a file.
* Prevent docker insert to erase something
+ Prevent DNS server conflicts in CreateBridgeIface
+ Validate bind mounts on the server side
+ Use parent image config in docker build
* Fix regression in /etc/hosts

#### Client

+ Add -P flag to publish all exposed ports
+ Add -notrunc and -q flags to docker history
* Fix docker commit, tag and import usage
+ Add stars, trusted builds and library flags in docker search
* Fix docker logs with tty

#### RemoteAPI

* Make /events API send headers immediately
* Do not split last column docker top
+ Add size to history

#### Other

+ Contrib: Desktop integration. Firefox usecase.
+ Dockerfile: bump to go1.2rc3

## 0.6.5 (2013-10-29)

#### Runtime

+ Containers can now be named
+ Containers can now be linked together for service discovery
+ 'run -a', 'start -a' and 'attach' can forward signals to the container for better integration with process supervisors
+ Automatically start crashed containers after a reboot
+ Expose IP, port, and proto as separate environment vars for container links
* Allow ports to be published to specific ips
* Prohibit inter-container communication by default
- Ignore ErrClosedPipe for stdin in Container.Attach
- Remove unused field kernelVersion
* Fix issue when mounting subdirectories of /mnt in container
- Fix untag during removal of images
* Check return value of syscall.Chdir when changing working directory inside dockerinit

#### Client

- Only pass stdin to hijack when needed to avoid closed pipe errors
* Use less reflection in command-line method invocation
- Monitor the tty size after starting the container, not prior
- Remove useless os.Exit() calls after log.Fatal

#### Hack

+ Add initial init scripts library and a safer Ubuntu packaging script that works for Debian
* Add -p option to invoke debootstrap with http_proxy
- Update install.sh with $sh_c to get sudo/su for modprobe
* Update all the mkimage scripts to use --numeric-owner as a tar argument
* Update hack/release.sh process to automatically invoke hack/make.sh and bail on build and test issues

#### Other

* Documentation: Fix the flags for nc in example
* Testing: Remove warnings and prevent mount issues
- Testing: Change logic for tty resize to avoid warning in tests
- Builder: Fix race condition in docker build with verbose output
- Registry: Fix content-type for PushImageJSONIndex method
* Contrib: Improve helper tools to generate debian and Arch linux server images

## 0.6.4 (2013-10-16)

#### Runtime

- Add cleanup of container when Start() fails
* Add better comments to utils/stdcopy.go
* Add utils.Errorf for error logging
+ Add -rm to docker run for removing a container on exit
- Remove error messages which are not actually errors
- Fix `docker rm` with volumes
- Fix some error cases where a HTTP body might not be closed
- Fix panic with wrong dockercfg file
- Fix the attach behavior with -i
* Record termination time in state.
- Use empty string so TempDir uses the OS's temp dir automatically
- Make sure to close the network allocators
+ Autorestart containers by default
* Bump vendor kr/pty to commit 3b1f6487b `(syscall.O_NOCTTY)`
* lxc: Allow set_file_cap capability in container
- Move run -rm to the cli only
* Split stdout stderr
* Always create a new session for the container

#### Testing

- Add aggregated docker-ci email report
- Add cleanup to remove leftover containers
* Add nightly release to docker-ci
* Add more tests around auth.ResolveAuthConfig
- Remove a few errors in tests
- Catch errClosing error when TCP and UDP proxies are terminated
* Only run certain tests with TESTFLAGS='-run TestName' make.sh
* Prevent docker-ci to test closing PRs
* Replace panic by log.Fatal in tests
- Increase TestRunDetach timeout

#### Documentation

* Add initial draft of the Docker infrastructure doc
* Add devenvironment link to CONTRIBUTING.md
* Add `apt-get install curl` to Ubuntu docs
* Add explanation for export restrictions
* Add .dockercfg doc
* Remove Gentoo install notes about #1422 workaround
* Fix help text for -v option
* Fix Ping endpoint documentation
- Fix parameter names in docs for ADD command
- Fix ironic typo in changelog
* Various command fixes in postgres example
* Document how to edit and release docs
- Minor updates to `postgresql_service.rst`
* Clarify LGTM process to contributors
- Corrected error in the package name
* Document what `vagrant up` is actually doing
+ improve doc search results
* Cleanup whitespace in API 1.5 docs
* use angle brackets in MAINTAINER example email
* Update archlinux.rst
+ Changes to a new style for the docs. Includes version switcher.
* Formatting, add information about multiline json
* Improve registry and index REST API documentation
- Replace deprecated upgrading reference to docker-latest.tgz, which hasn't been updated since 0.5.3
* Update Gentoo installation documentation now that we're in the portage tree proper
* Cleanup and reorganize docs and tooling for contributors and maintainers
- Minor spelling correction of protocoll -> protocol

#### Contrib

* Add vim syntax highlighting for Dockerfiles from @honza
* Add mkimage-arch.sh
* Reorganize contributed completion scripts to add zsh completion

#### Hack

* Add vagrant user to the docker group
* Add proper bash completion for "docker push"
* Add xz utils as a runtime dep
* Add cleanup/refactor portion of #2010 for hack and Dockerfile updates
+ Add contrib/mkimage-centos.sh back (from #1621), and associated documentation link
* Add several of the small make.sh fixes from #1920, and make the output more consistent and contributor-friendly
+ Add @tianon to hack/MAINTAINERS
* Improve network performance for VirtualBox
* Revamp install.sh to be usable by more people, and to use official install methods whenever possible (apt repo, portage tree, etc.)
- Fix contrib/mkimage-debian.sh apt caching prevention
+ Add Dockerfile.tmLanguage to contrib
* Configured FPM to make /etc/init/docker.conf a config file
* Enable SSH Agent forwarding in Vagrant VM
* Several small tweaks/fixes for contrib/mkimage-debian.sh

#### Other

- Builder: Abort build if mergeConfig returns an error and fix duplicate error message
- Packaging: Remove deprecated packaging directory
- Registry: Use correct auth config when logging in.
- Registry: Fix the error message so it is the same as the regex

## 0.6.3 (2013-09-23)

#### Packaging

* Add 'docker' group on install for ubuntu package
* Update tar vendor dependency
* Download apt key over HTTPS

#### Runtime

- Only copy and change permissions on non-bindmount volumes
* Allow multiple volumes-from
- Fix HTTP imports from STDIN

#### Documentation

* Update section on extracting the docker binary after build
* Update development environment docs for new build process
* Remove 'base' image from documentation

#### Other

- Client: Fix detach issue
- Registry: Update regular expression to match index

## 0.6.2 (2013-09-17)

#### Runtime

+ Add domainname support
+ Implement image filtering with path.Match
* Remove unnecessary warnings
* Remove os/user dependency
* Only mount the hostname file when the config exists
* Handle signals within the `docker login` command
- UID and GID are now also applied to volumes
- `docker start` set error code upon error
- `docker run` set the same error code as the process started

#### Builder

+ Add -rm option in order to remove intermediate containers
* Allow multiline for the RUN instruction

#### Registry

* Implement login with private registry
- Fix push issues

#### Other

+ Hack: Vendor all dependencies
* Remote API: Bump to v1.5
* Packaging: Break down hack/make.sh into small scripts, one per 'bundle': test, binary, ubuntu etc.
* Documentation: General improvements

## 0.6.1 (2013-08-23)

#### Registry

* Pass "meta" headers in API calls to the registry

#### Packaging

- Use correct upstart script with new build tool
- Use libffi-dev, don`t build it from sources
- Remove duplicate mercurial install command

## 0.6.0 (2013-08-22)

#### Runtime

+ Add lxc-conf flag to allow custom lxc options
+ Add an option to set the working directory
* Add Image name to LogEvent tests
+ Add -privileged flag and relevant tests, docs, and examples
* Add websocket support to /container/<name>/attach/ws
* Add warning when net.ipv4.ip_forwarding = 0
* Add hostname to environment
* Add last stable version in `docker version`
- Fix race conditions in parallel pull
- Fix Graph ByParent() to generate list of child images per parent image.
- Fix typo: fmt.Sprint -> fmt.Sprintf
- Fix small \n error un docker build
* Fix to "Inject dockerinit at /.dockerinit"
* Fix #910. print user name to docker info output
* Use Go 1.1.2 for dockerbuilder
* Use ranged for loop on channels
- Use utils.ParseRepositoryTag instead of strings.Split(name, ":") in server.ImageDelete
- Improve CMD, ENTRYPOINT, and attach docs.
- Improve connect message with socket error
- Load authConfig only when needed and fix useless WARNING
- Show tag used when image is missing
* Apply volumes-from before creating volumes
- Make docker run handle SIGINT/SIGTERM
- Prevent crash when .dockercfg not readable
- Install script should be fetched over https, not http.
* API, issue 1471: Use groups for socket permissions
- Correctly detect IPv4 forwarding
* Mount /dev/shm as a tmpfs
- Switch from http to https for get.docker.io
* Let userland proxy handle container-bound traffic
* Update the Docker CLI to specify a value for the "Host" header.
- Change network range to avoid conflict with EC2 DNS
- Reduce connect and read timeout when pinging the registry
* Parallel pull
- Handle ip route showing mask-less IP addresses
* Allow ENTRYPOINT without CMD
- Always consider localhost as a domain name when parsing the FQN repos name
* Refactor checksum

#### Documentation

* Add MongoDB image example
* Add instructions for creating and using the docker group
* Add sudo to examples and installation to documentation
* Add ufw doc
* Add a reference to ps -a
* Add information about Docker`s high level tools over LXC.
* Fix typo in docs for docker run -dns
* Fix a typo in the ubuntu installation guide
* Fix to docs regarding adding docker groups
* Update default -H docs
* Update readme with dependencies for building
* Update amazon.rst to explain that Vagrant is not necessary for running Docker on ec2
* PostgreSQL service example in documentation
* Suggest installing linux-headers by default.
* Change the twitter handle
* Clarify Amazon EC2 installation
* 'Base' image is deprecated and should no longer be referenced in the docs.
* Move note about officially supported kernel
- Solved the logo being squished in Safari

#### Builder

+ Add USER instruction do Dockerfile
+ Add workdir support for the Buildfile
* Add no cache for docker build
- Fix docker build and docker events output
- Only count known instructions as build steps
- Make sure ENV instruction within build perform a commit each time
- Forbid certain paths within docker build ADD
- Repository name (and optionally a tag) in build usage
- Make sure ADD will create everything in 0755

#### Remote API

* Sort Images by most recent creation date.
* Reworking opaque requests in registry module
* Add image name in /events
* Use mime pkg to parse Content-Type
* 650 http utils and user agent field

#### Hack

+ Bash Completion: Limit commands to containers of a relevant state
* Add docker dependencies coverage testing into docker-ci

#### Packaging

+ Docker-brew 0.5.2 support and memory footprint reduction
* Add new docker dependencies into docker-ci
- Revert "docker.upstart: avoid spawning a `sh` process"
+ Docker-brew and Docker standard library
+ Release docker with docker
* Fix the upstart script generated by get.docker.io
* Enabled the docs to generate manpages.
* Revert Bind daemon to 0.0.0.0 in Vagrant.

#### Register

* Improve auth push
* Registry unit tests + mock registry

#### Tests

* Improve TestKillDifferentUser to prevent timeout on buildbot
- Fix typo in TestBindMounts (runContainer called without image)
* Improve TestGetContainersTop so it does not rely on sleep
* Relax the lo interface test to allow iface index != 1
* Add registry functional test to docker-ci
* Add some tests in server and utils

#### Other

* Contrib: bash completion script
* Client: Add docker cp command and copy api endpoint to copy container files/folders to the host
* Don`t read from stdout when only attached to stdin

## 0.5.3 (2013-08-13)

#### Runtime

* Use docker group for socket permissions
- Spawn shell within upstart script
- Handle ip route showing mask-less IP addresses
- Add hostname to environment

#### Builder

- Make sure ENV instruction within build perform a commit each time

## 0.5.2 (2013-08-08)

* Builder: Forbid certain paths within docker build ADD
- Runtime: Change network range to avoid conflict with EC2 DNS
* API: Change daemon to listen on unix socket by default

## 0.5.1 (2013-07-30)

#### Runtime

+ Add `ps` args to `docker top`
+ Add support for container ID files (pidfile like)
+ Add container=lxc in default env
+ Support networkless containers with `docker run -n` and `docker -d -b=none`
* Stdout/stderr logs are now stored in the same file as JSON
* Allocate a /16 IP range by default, with fallback to /24. Try 12 ranges instead of 3.
* Change .dockercfg format to json and support multiple auth remote
- Do not override volumes from config
- Fix issue with EXPOSE override

#### API

+ Docker client now sets useragent (RFC 2616)
+ Add /events endpoint

#### Builder

+ ADD command now understands URLs
+ CmdAdd and CmdEnv now respect Dockerfile-set ENV variables
- Create directories with 755 instead of 700 within ADD instruction

#### Hack

* Simplify unit tests with helpers
* Improve docker.upstart event
* Add coverage testing into docker-ci

## 0.5.0 (2013-07-17)

#### Runtime

+ List all processes running inside a container with 'docker top'
+ Host directories can be mounted as volumes with 'docker run -v'
+ Containers can expose public UDP ports (eg, '-p 123/udp')
+ Optionally specify an exact public port (eg. '-p 80:4500')
* 'docker login' supports additional options
- Don't save a container`s hostname when committing an image.

#### Registry

+ New image naming scheme inspired by Go packaging convention allows arbitrary combinations of registries
- Fix issues when uploading images to a private registry

#### Builder

+ ENTRYPOINT instruction sets a default binary entry point to a container
+ VOLUME instruction marks a part of the container as persistent data
* 'docker build' displays the full output of a build by default

## 0.4.8 (2013-07-01)

+ Builder: New build operation ENTRYPOINT adds an executable entry point to the container.  - Runtime: Fix a bug which caused 'docker run -d' to no longer print the container ID.
- Tests: Fix issues in the test suite

## 0.4.7 (2013-06-28)

#### Remote API

* The progress bar updates faster when downloading and uploading large files
- Fix a bug in the optional unix socket transport

#### Runtime

* Improve detection of kernel version
+ Host directories can be mounted as volumes with 'docker run -b'
- fix an issue when only attaching to stdin
* Use 'tar --numeric-owner' to avoid uid mismatch across multiple hosts

#### Hack

* Improve test suite and dev environment
* Remove dependency on unit tests on 'os/user'

#### Other

* Registry: easier push/pull to a custom registry
+ Documentation: add terminology section

## 0.4.6 (2013-06-22)

- Runtime: fix a bug which caused creation of empty images (and volumes) to crash.

## 0.4.5 (2013-06-21)

+ Builder: 'docker build git://URL' fetches and builds a remote git repository
* Runtime: 'docker ps -s' optionally prints container size
* Tests: improved and simplified
- Runtime: fix a regression introduced in 0.4.3 which caused the logs command to fail.
- Builder: fix a regression when using ADD with single regular file.

## 0.4.4 (2013-06-19)

- Builder: fix a regression introduced in 0.4.3 which caused builds to fail on new clients.

## 0.4.3 (2013-06-19)

#### Builder

+ ADD of a local file will detect tar archives and unpack them
* ADD improvements: use tar for copy + automatically unpack local archives
* ADD uses tar/untar for copies instead of calling 'cp -ar'
* Fix the behavior of ADD to be (mostly) reverse-compatible, predictable and well-documented.
- Fix a bug which caused builds to fail if ADD was the first command
* Nicer output for 'docker build'

#### Runtime

* Remove bsdtar dependency
* Add unix socket and multiple -H support
* Prevent rm of running containers
* Use go1.1 cookiejar
- Fix issue detaching from running TTY container
- Forbid parallel push/pull for a single image/repo. Fixes #311
- Fix race condition within Run command when attaching.

#### Client

* HumanReadable ProgressBar sizes in pull
* Fix docker version`s git commit output

#### API

* Send all tags on History API call
* Add tag lookup to history command. Fixes #882

#### Documentation

- Fix missing command in irc bouncer example

## 0.4.2 (2013-06-17)

- Packaging: Bumped version to work around an Ubuntu bug

## 0.4.1 (2013-06-17)

#### Remote Api

+ Add flag to enable cross domain requests
+ Add images and containers sizes in docker ps and docker images

#### Runtime

+ Configure dns configuration host-wide with 'docker -d -dns'
+ Detect faulty DNS configuration and replace it with a public default
+ Allow docker run <name>:<id>
+ You can now specify public port (ex: -p 80:4500)
* Improve image removal to garbage-collect unreferenced parents

#### Client

* Allow multiple params in inspect
* Print the container id before the hijack in `docker run`

#### Registry

* Add regexp check on repo`s name
* Move auth to the client
- Remove login check on pull

#### Other

* Vagrantfile: Add the rest api port to vagrantfile`s port_forward
* Upgrade to Go 1.1
- Builder: don`t ignore last line in Dockerfile when it doesn`t end with \n

## 0.4.0 (2013-06-03)

#### Builder

+ Introducing Builder
+ 'docker build' builds a container, layer by layer, from a source repository containing a Dockerfile

#### Remote API

+ Introducing Remote API
+ control Docker programmatically using a simple HTTP/json API

#### Runtime

* Various reliability and usability improvements

## 0.3.4 (2013-05-30)

#### Builder

+ 'docker build' builds a container, layer by layer, from a source repository containing a Dockerfile
+ 'docker build -t FOO' applies the tag FOO to the newly built container.

#### Runtime

+ Interactive TTYs correctly handle window resize
* Fix how configuration is merged between layers

#### Remote API

+ Split stdout and stderr on 'docker run'
+ Optionally listen on a different IP and port (use at your own risk)

#### Documentation

* Improve install instructions.

## 0.3.3 (2013-05-23)

- Registry: Fix push regression
- Various bugfixes

## 0.3.2 (2013-05-09)

#### Registry

* Improve the checksum process
* Use the size to have a good progress bar while pushing
* Use the actual archive if it exists in order to speed up the push
- Fix error 400 on push

#### Runtime

* Store the actual archive on commit

## 0.3.1 (2013-05-08)

#### Builder

+ Implement the autorun capability within docker builder
+ Add caching to docker builder
+ Add support for docker builder with native API as top level command
+ Implement ENV within docker builder
- Check the command existence prior create and add Unit tests for the case
* use any whitespaces instead of tabs

#### Runtime

+ Add go version to debug infos
* Kernel version - don`t show the dash if flavor is empty

#### Registry

+ Add docker search top level command in order to search a repository
- Fix pull for official images with specific tag
- Fix issue when login in with a different user and trying to push
* Improve checksum - async calculation

#### Images

+ Output graph of images to dot (graphviz)
- Fix ByParent function

#### Documentation

+ New introduction and high-level overview
+ Add the documentation for docker builder
- CSS fix for docker documentation to make REST API docs look better.
- Fix CouchDB example page header mistake
- Fix README formatting
* Update www.docker.io website.

#### Other

+ Website: new high-level overview
- Makefile: Swap "go get" for "go get -d", especially to compile on go1.1rc
* Packaging: packaging ubuntu; issue #510: Use goland-stable PPA package to build docker

## 0.3.0 (2013-05-06)

#### Runtime

- Fix the command existence check
- strings.Split may return an empty string on no match
- Fix an index out of range crash if cgroup memory is not

#### Documentation

* Various improvements
+ New example: sharing data between 2 couchdb databases

#### Other

* Vagrant: Use only one deb line in /etc/apt
+ Registry: Implement the new registry

## 0.2.2 (2013-05-03)

+ Support for data volumes ('docker run -v=PATH')
+ Share data volumes between containers ('docker run -volumes-from')
+ Improve documentation
* Upgrade to Go 1.0.3
* Various upgrades to the dev environment for contributors

## 0.2.1 (2013-05-01)

+ 'docker commit -run' bundles a layer with default runtime options: command, ports etc.
* Improve install process on Vagrant
+ New Dockerfile operation: "maintainer"
+ New Dockerfile operation: "expose"
+ New Dockerfile operation: "cmd"
+ Contrib script to build a Debian base layer
+ 'docker -d -r': restart crashed containers at daemon startup
* Runtime: improve test coverage

## 0.2.0 (2013-04-23)

- Runtime: ghost containers can be killed and waited for
* Documentation: update install instructions
- Packaging: fix Vagrantfile
- Development: automate releasing binaries and ubuntu packages
+ Add a changelog
- Various bugfixes

## 0.1.8 (2013-04-22)

- Dynamically detect cgroup capabilities
- Issue stability warning on kernels <3.8
- 'docker push' buffers on disk instead of memory
- Fix 'docker diff' for removed files
- Fix 'docker stop' for ghost containers
- Fix handling of pidfile
- Various bugfixes and stability improvements

## 0.1.7 (2013-04-18)

- Container ports are available on localhost
- 'docker ps' shows allocated TCP ports
- Contributors can run 'make hack' to start a continuous integration VM
- Streamline ubuntu packaging & uploading
- Various bugfixes and stability improvements

## 0.1.6 (2013-04-17)

- Record the author an image with 'docker commit -author'

## 0.1.5 (2013-04-17)

- Disable standalone mode
- Use a custom DNS resolver with 'docker -d -dns'
- Detect ghost containers
- Improve diagnosis of missing system capabilities
- Allow disabling memory limits at compile time
- Add debian packaging
- Documentation: installing on Arch Linux
- Documentation: running Redis on docker
- Fix lxc 0.9 compatibility
- Automatically load aufs module
- Various bugfixes and stability improvements

## 0.1.4 (2013-04-09)

- Full support for TTY emulation
- Detach from a TTY session with the escape sequence `C-p C-q`
- Various bugfixes and stability improvements
- Minor UI improvements
- Automatically create our own bridge interface 'docker0'

## 0.1.3 (2013-04-04)

- Choose TCP frontend port with '-p :PORT'
- Layer format is versioned
- Major reliability improvements to the process manager
- Various bugfixes and stability improvements

## 0.1.2 (2013-04-03)

- Set container hostname with 'docker run -h'
- Selective attach at run with 'docker run -a [stdin[,stdout[,stderr]]]'
- Various bugfixes and stability improvements
- UI polish
- Progress bar on push/pull
- Use XZ compression by default
- Make IP allocator lazy

## 0.1.1 (2013-03-31)

- Display shorthand IDs for convenience
- Stabilize process management
- Layers can include a commit message
- Simplified 'docker attach'
- Fix support for re-attaching
- Various bugfixes and stability improvements
- Auto-download at run
- Auto-login on push
- Beefed up documentation

## 0.1.0 (2013-03-23)

Initial public release

- Implement registry in order to push/pull images
- TCP port allocation
- Fix termcaps on Linux
- Add documentation
- Add Vagrant support with Vagrantfile
- Add unit tests
- Add repository/tags to ease image management
- Improve the layer implementation
