# dnmp
docker build lnmp
#
添加加速仓库地址
#
sudo mkdir -p /etc/docker
#
sudo tee /etc/docker/daemon.json <<-'EOF'
#
{
#
   "registry-mirrors": ["https://nlclafq2.mirror.aliyuncs.com"]
#    
}
#
EOF
#
sudo systemctl daemon-reload
#
sudo systemctl restart docker
#
先拉取基础镜像
#
docker pull registry.cn-hangzhou.aliyuncs.com/mengh/nginx
#
docker pull registry.cn-hangzhou.aliyuncs.com/mengh/php-5.6
#
docker pull registry.cn-hangzhou.aliyuncs.com/mengh/memcached
#
docker pull registry.cn-hangzhou.aliyuncs.com/mengh/mysql
#
修改tag
#
docker  tag a1c7de60a1bc dnmp_nginx    
#
docker  tag a6d260ec1ba8 dnmp_php-5.6 
#
docker  tag c1eb398ad98c dnmp_memcached 
#
docker  tag 57b9ce70d5ba dnmp_mysql 
#
克隆项目
#
git clone https://github.com/wbzxjpzgzt/dnmp
#
进入目录
#
cd dnmp 
#
修改配置文件
#
其中
#
mysql:
#  
  build: ./other/mysql/
#
  image: dnmp_mysql:latest
#  
  ports:
#    
    - "3306:3306"
#  
  volumes:
#
    - ./conf/mysql/my.cnf:/etc/mysql/my.cnf:ro
#    
    - ./mysql/:/var/lib/mysql/:rw
#    
    - ./log/mysql/:/var/log/mysql/:rw
#  
  environment:
#    
    MYSQL_ROOT_PASSWORD: "123456"
#
密码默认为123456，启动容器后登陆
#
# 更新一下用户的密码 root用户密码为newpassword  
# 5.7.24
#
mysql> ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
# 5.6 
#
update mysql.user set password=password("newpassword") where user="root";
#  flush privileges;
#
docker-compose build
#
构建基础镜像
#
修改docker-compose.yml
#
其中nginx开启了ssl的话，证书放在
#
  ./conf/nginx/conf.d文件夹下
#
修改完后
#
  docker-compose up -d
#
修改完成后，出现bug pfm 9000 端口被占用，pkill杀掉fpm,然后重启
#
pkill php-fpm
#
/usr/sbin/php-fpm
#
访问完成！
#
添加定时任务
## 宿主机安装（centos7）crontab
#
yum install vixie-cron
#
service crond start
#
chkconfig crond on
#
yum install cronie
#
yum install vixie-cron
#
yum install cronie
#
yum -y install vixie-cron
yum -y install crontabs
## 参考博客
https://segmentfault.com/a/1190000015198153
