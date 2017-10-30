# runc-demo
the demo of runc usage
# environment
Ubuntu 16.04
# run a container
```
git clone https://github.com/willstudy/runc-demo.git
cd runc-demo/busybox
runc run busybox
```
# capabilities demo
At first, run a container by
```
cd runc-demo/busybox
runc run busybox
```
Then, some errors are encountered when running `hostname qiniu`
```
/ # hostname qiniu
hostname: sethostname: Operation not permitted
```
Now, we add `CAP_SYS_ADMIN` capability for this container by modifying `config.json`
```
  "CAP_KILL",
+ "CAP_SYS_ADMIN",
  "CAP_NET_BIND_SERVICE"
```
After that, running this container again, we can set hostname in this container.
# mount demo
we can mount some devices for the new container by modifying `mounts` field in the `config.json`. For example, we can mount `/sys/fs/cgroup` for the new container by adding the following configuration.
```
	{
		"destination": "/sys",
		"type": "sysfs",
		"source": "sysfs",
		"options": [
			"nosuid",
			"noexec",
			"nodev",
			"ro"
		]
+	},
+	{
+		"destination": "/sys/fs/cgroup",
+		"type": "cgroup",
+		"source": "cgroup",
+		"options": [
+			"nosuid",
+			"noexec",
+			"nodev",
+			"relatime",
+			"ro"
+		]
	}
```
# namespace demo
we can add this container into the existing namespaces by `namespaces` field of the `config.json`. For example 
```
"type": "pid",
"path": "/proc/15428/ns/pid"
```
this container will be added into the PID namespace of the 15428 PID process.
