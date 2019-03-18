### RDS自检工具使用方法

```bash
# 出包方法

cd ../Tools/
sh package.sh demine pack
# 生成  demine_tools.tar.gz 同步现场
```

* .demine_tools使用方法
  
```
scp demine_tools.tar.gz dmsag/ops1:/root
ssh dmsag/ops1
mkdir -p /root/rds
tar -xf /root/rds/demine_tools.tar.gz -C /root/rds
cd /root/rds/demine_tools
### v2 版本专有云使用方法
/home/tops/bin/python main.pyc -v v2 -r {R表绝对路径} -c {container表绝对路径} -m {minirds元数据库配置文件绝对路径} -n {项目名称}
# example:
##  /home/tops/bin/python main.pyc -v v2 -r /apsarapangu/L1root/L1tools/main/config/rtable.csv -c /apsarapangu/L1root/L1tools/main/config/container_arrangement.csv -m /apsarapangu/L1root/L1workdir/minirds-db-conf.yml -n BGM

### v3 版本专有云使用方法
/home/tops/bin/python main.pyc -v v3 -n {项目名称} -t {天基api配置文件}

# example：
##   /home/tops/bin/python main.pyc -v v3 -n xxx -t /cloud/data/bootstrap_controller/BootstrapController#/bootstrap_controller/tianji_dest.conf

```

* 工具正常执行结束后

```bash
# please donwload ./report.tar.gz and tar -xf open html/report.html on chrome
# please download ./report.json send it to rds team
# 会显示如上输出结果，
# 将report.tar.gz, report.json下载下来，给RDS团队，html文件为检测报告，可以用浏览器直接打开对照报告自查。
```

* ssh修改默认22端口方法

> 个别项目中ssh通道的默认22端口被修改，本工具默认22端口为ssh端口，如果各个项目有修改机器或者docker默认端口可以在文件中传入端口进行排查运行。
或者把所有默认端口都改为22端口跑排查工具

```bash
cd /root/rds/demine_tools
vim common/common.conf

[port]
mysql=22
docker=22
linux=22
metadb=22
proxy=22

#   修改当前项目上对于各个节点的调整，
#   mysql对应mysql物理机的SSH端口信息
#   docker对应rds docker容器的SSH端口信息
#   linux对应RDS服务所有引擎物理机节点例如pgsql，mssql，proxy，或者其他使用linux物理机的SSH端口信息
#   metadb对应minirds所在物理机的SSH端口信息
#   proxy对应proxy物理机的SSH端口信息
```