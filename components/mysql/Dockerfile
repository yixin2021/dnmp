ARG MYSQL_VERSION
FROM mysql:${MYSQL_VERSION}

# 删除预安装的数据库文件
RUN rm -rf /var/lib/mysql/*


VOLUME  ["/etc/mysql", "/var/lib/mysql"]
CMD ["mysqld"]
EXPOSE 3306