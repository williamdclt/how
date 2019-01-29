update user set plugin='mysql_native_password' where user='birdie';
ALTER USER 'birdie'@'localhost' IDENTIFIED WITH mysql_native_password BY 'birdie';
