# libwrapdroid-a10plus

System V and Posix shared memory wrapper
for chrooted environments _(i.e.: PRoot)_
under Android.
Supposed to be an extended version of
<https://github.com/termux/libandroid-shmem>

** Forked by 26uk247 to attempt POSIX shared memory emulation in Debian 11 on Android 14 (via Linux Deploy chroot), in order to use the Ham Radio program wsjtx
** Forked from <https://github.com/green-green-avk/libwrapdroid>

* Android 10 and higher support (not relied upon _ashmem_):
  see <https://developer.android.com/about/versions/10/behavior-changes-10#shared-memory>
* Resouces are linked to a server instance
  and will be freed at the instance exit.
* Each server instance provides its own namespace.
* System V shared memory calls:
  `shmget()`, `shmat()`, `shmdt()`, `shmctl()`:
  * There are some problems around process termination and `fork()`
  but it serves Xwayland + XFCE + Firefox without any visible problems
  even in its current state.
  <br/>_(Full support will be added in the next version.)_
* Posix shared memory calls:
  `shm_open()`, `shm_unlink()`.


## Build

Under PRoot:
```sh
make PREFIX=/opt/shm install
```


## Parts

* `libwrapdroid-server` — is to be started to create a resource namespace.
* `libwrapdroid-shm-posix.so` — Posix shared memory API; to be added to `LD_PRELOAD`.
* `libwrapdroid-shm-sysv.so` — System V shared memory API; to be added to `LD_PRELOAD`.


## Environment variables

* `LIBWRAPDROID_SOCKET_NAME` — communication socket name
  in the abstract namespace.
  <br/>_(must be set per server instance)_
* `LIBWRAPDROID_AUTH_KEY` — authentication key
  (at least 16 hex digits length).
  <br/>_(must be set per server instance)_
