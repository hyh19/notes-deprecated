# Linux 修改文件属性和权限

修改文件的所属群组
```bash
chgrp users initial-setup-ks.cfg
```

修改文件的所有者
```bash
chown bin initial-setup-ks.cfg
```

同时修改文件的所有者和所属群组
```bash
chown root:root initial-setup-ks.cfg
```

用数字修改文件的权限
```bash
chmod 777 .bashrc
```

用符号修改文件的权限
```bash
chmod u=rwx,go=rx .bashrc
chmod a+w .bashrc
chmod a-x .bashrc
```