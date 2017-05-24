## WordPress Files
* WordPress 4.7.5 Files
* Patched to support Azure MySQL service
* WordPress config (wp-config.php) added

## WordPress Patch
* html/wp-includes/wp-db.php is patched to support SSL to Azure MySQL  
    Make sure to patch the file if you use a separate repo as this file is part of the WordPress distribution
```
if ( defined('MYSQL_SSL_CA')) {
        mysqli_ssl_set($this->dbh,NULL,NULL,MYSQL_SSL_CA,NULL,NULL);
}
```

## WordPress Config
* Added html/wp-config.php
* wp-config is not included in the standard WordPress distribution

* wp-config defines MYSQL_SSL_CA to support SSL to the Azure MySQL service
```
define('MYSQL_SSL_CA', '/etc/ssl/certs/Baltimore_CyberTrust_Root.pem');
```

* wp-config uses environment variables for MySQL connection information  
```
define('DB_NAME', getenv('WORDPRESS_DB_NAME'));
define('DB_USER', getenv('WORDPRESS_DB_USER'));
define('DB_PASSWORD', getenv('WORDPRESS_DB_PASSWORD'));
define('DB_HOST', getenv('WORDPRESS_DB_HOST'));
```

* wp-config adds support for x-arr-ssl headers (used by App Services for Linux)
```
if (isset($_SERVER['HTTP_X_ARR_SSL'])) {
	$_SERVER['HTTPS'] = 'on';
}
```

* wp-config can force SSL (including x-arr-ssl support)  
    Use the FORCE_SSL environment variable = true to force redirects to HTTPS
```
if(strtolower(getenv('FORCE_SSL')) == 'true' && $_SERVER['HTTPS'] != 'on' && empty($_SERVER['HTTP_X_ARR_SSL'])){
    $redirect = 'https://' . $_SERVER['HTTP_HOST'] . $_SERVER['REQUEST_URI'];
    header('HTTP/1.1 301 Moved Permanently');
    header('Location: ' . $redirect);
    exit();
}
```


