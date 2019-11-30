# 1 Nginx基础使用

## 1.1 官网下载

[下载地址](http://nginx.org/en/download.html)

下面教程使用的版本是：nginx-1.14.2.zip。

正向代理代理的是客户端，而且客户端是知道目标的，而目标是不知道客户端是通过VPN访问的；正向代理类似一个跳板机，代理访问外部资源；客户端必须设置正向代理服务器，当然前提是要知道正向代理服务器的IP地址，还有代理程序的端口。

反向代理代理的是服务器端，而且这一个过程对于客户端而言是透明的；保证内网的安全，可以使用反向代理提供WAF功能，阻止web攻击；大型网站，通常将反向代理作为公网访问地址，Web服务器是内网。负载均衡，通过反向代理服务器来优化网站的负载。

## 1.2 在线安装

### 1.2.1 基础环境

> nginx是C语言开发，建议在linux上运行，本教程使用Centos6.4作为安装环境。安装redis需要先将官网下载的源码进行编译，编译依赖gcc、PCRE、zlib、openssl环境。

```properties
yum install -y gcc gcc-c++ pcre pcre-devel zlib zlib-devel openssl openssl-devel
```

### 1.2.2 进行安装

> 01解压源码
>
> 02进入解压后的目录
>
> 03参数设置，安装到指定目录
>
> 04编译安装

```properties
tar -zxvf nginx-1.11.13.tar.gz
cd /usr/local/nginx-1.11.13
./configure --prefix=/usr/local/nginx --with-stream
make && make install
```

### 1.2.3 Nginx.conf

#### 1.2.3.1 反向代理

```properties
# user  nobody;
worker_processes  1;

events {
    worker_connections  1024;
}

# 配置了tcp负载均衡和udp(dns)负载均衡的例子
stream {
    include ../server/stream/*.conf;
}

# HTTP服务代理（负载均衡、反向代理）
http {
    include ../server/http/*.conf;
    include       mime.types;
    default_type  application/octet-stream; 
    sendfile        on;
    # keepalive_timeout  0;
    keepalive_timeout  65;  
}
```

#### 1.2.3.2 HTTPS配置

```properties
# 开启缓存
proxy_cache_path cache levels=1:2 keys_zone=my_cache:10m;

# 通过http访问https
server {
    listen       80 default_server;
    listen       [::]:80 default_server;
    server_name  test.com;
    return 302 https://$server_name$request_uri;
}

# 配置https和反向代理
server {
    # https的默认端口是443
    listen       443;
    server_name  test.com;

    # 公钥私钥生成命令（生成的证书会提示不安全）：openssl req -x509 -newkey rsa:2048 -nodes -sha256 -keyout localhost-privkey.pem -out localhost-cert.pem
    ssl on;
    ssl_certificate_key  ../certs/localhost-privkey.pem;
    ssl_certificate      ../certs/localhost-cert.pem;

    location / {
        proxy_cache my_cache;
        proxy_pass http://127.0.0.1:8888;
        proxy_set_header Host $host;
    }
}

# http2配置（chrome://net-internals中可以查看http2；https://http2.akamai.com/demo/http2-lab.html可以比较性能）
server {
    listen       443 http2;
    server_name  test.com;

    http2_push_preload  on;

    ssl on;
    ssl_certificate_key  ../certs/localhost-privkey.pem;
    ssl_certificate      ../certs/localhost-cert.pem;

    location / {
        proxy_cache my_cache;
        proxy_pass http://127.0.0.1:8888;
        proxy_set_header Host $host;
    }
}
```

#### 1.2.3.3 Server节点配置

```properties
server {
    listen 8081;

    # 前台页面
    location ~* \.(html|htm|gif|jpg|jpeg|bmp|png|ico|txt|js|css|woff|ttf)$ {
        root   /home/www/;
        index  index.html login.html;
    }

    # 后台服务（URL中是否包含有URI，包含/是替换，不包含/是追加）
    location /isp/ {
        proxy_pass http://10.237.16.21:7122/;
    }
}
```

#### 1.2.3.4 Stream节点配置

```properties
worker_processes auto;
error_log logs/error.stream.log info;
events {
    worker_connections  1024;
}
stream {
    upstream backend {
        hash $remote_addr consistent;
        server 127.0.0.1:12346 weight=5;
        server 127.0.0.1:12347            max_fails=3 fail_timeout=30s;
        server 127.0.0.1:12348            max_fails=3 fail_timeout=30s;
    }
    upstream dns {
       server 17.61.29.79:53;
       server 17.61.29.80:53;
       server 17.61.29.81:53;
       server 17.61.29.82:53;
    }
    server {
        listen 12345;
        proxy_connect_timeout 1s;
        proxy_timeout 3s;
        proxy_pass backend;
    }
    server {
        listen 127.0.0.1:53 udp;
        proxy_responses 1;
        proxy_timeout 20s;
        proxy_pass dns;
    }
}
```

#### 1.2.3.5 Upstream负载均衡

```properties
# 方式一
upstream proxy_server01 {
    server 192.168.1.1:8001;
    server 192.168.1.2:8001;
    server 192.168.1.3:8001;
    server 192.168.1.4:8001;
}

# 方式二
upstream proxy_server02 {
    server http://192.168.1.1:8001/;
    server http://192.168.1.2:8001/;
    server http://192.168.1.3:8001/;
    server http://192.168.1.4:8001/;
}

http {
    server {
        listen 80;
        server_name aaa.com;
        location / {
            proxy_pass http://proxy_server01;
        }
    }

    server {
        listen 81;
        server_name bbb.com;
        location /server/ {
            proxy_pass proxy_server02;
        }
    }
}
```

#### 1.2.3.6 Root和Alias区别

一般情况下，在location的/中配置root，在location /other中配置alias是一个好习惯。

root指定的路径注意**真实的路径是root指定的值加上location指定的值**。

alias 正如其名，alias指定的路径是location的别名，不管location的值怎么写，资源的**真实路径都是 alias 指定的路径**。

区别：alias只能作用在location中，而root可以存在server、htp和location中；alias后面必须要用/结束，否则会找不到文件，而root则对/可有可无

```properties
location /c/ {
      alias /a/;
}
注：如果访问站点http://location/c访问的就是/a/目录下的站点信息，末尾必须加“/”。

location /c/ {
      root /a/;
}
注：如果访问站点http://location/c访问的就是/a/c目录下的站点信息，末尾“/”加不加无所谓。
```

## 1.3 离线安装

### 1.3.1 gcc-c++

> 把安装包解压到服务器上，先安装gcc，再安装g++。分别执行两个文件夹下的install.sh。
> 一般我们都需要先装pcre,zlib，前者用于url rewrite，后者用于gzip压缩，openssl用于后续可能升级到https时使用。

```properties

```

### 1.3.2 pcre安装

执行如下命令：

```properties
tar -zxvf pcre-8.42.tar.gz
cd pcre-8.42/
./configure
make && make install
```

### 1.3.3 zlib安装

执行如下命令：

```properties
tar -zxvf zlib-1.2.11.tar.gz
cd zlib-1.2.11/
./configure
make && make install
```

### 1.3.4 openssl安装

执行如下命令：

```properties
tar -zxvf openssl-1.1.0h.tar.gz
cd openssl-1.1.0h/
./config
make && make install
```

### 1.3.5 nginx安装

执行如下命令：

```properties
tar -zxvf nginx-1.14.0.tar.gz
cd nginx-1.14.0/
./configure --prefix=/usr/local/nginx --with-http_ssl_module --with-pcre=../pcre-8.42 --with-zlib=../zlib-1.2.11 --with-openssl=../openssl-1.1.0h
make && make install
```

## 1.4 启动使用

```properties
cd /usr/local/nginx/sbin
./nginx
```

### 1.4.1 基本命令

```properties
进入安装目录，不是源码目录：/usr/local/nginx
检查配置文件：./nginx -t
首次启动：./nginx -c conf/nginx.conf
停止：./nginx -s stop 
从新加载配置：./nginx -s reload
```

### 1.4.2 开机启动

```properties
# Nginx服务开机启动nginx.service
[Unit]
Description=nginx - high performance web server
After=network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s stop
```

## 1.5 nginx.service



# 2 Keepalived实现主备切换

TODO 待完善



# 3 业务使用

## 3.1 动静分离部署

### 3.1.1 前台页面配置

```javascript
var baseUrl = "192.168.100.12:11601";
```

> 注意：此处是配置Nginx的地址，不是后台部署服务的地址。

### 3.1.2 Nginx配置

1. 全局总配置文件nginx.conf

> 注意：测试为了以后添加方便进行配置分离，完全可以都写在一个文件中。

```properties

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    include ../server/file/*.conf;
    include ../server/http/*.conf;
}

```

2. 动静分离配置，在/usr/local/nginx/server/http目录下配置文件test.conf，名字随意，根据业务自行修改。

```properties
server {
    listen 8999;

    # 前台页面
    location ~* \.(html|htm|gif|jpg|jpeg|bmp|png|ico|txt|js|css|woff|ttf)$ {
        root   /home/kaifa/test/;
        index  index.html login.html;
    }

    # 后台服务（URL中是否包含有URI，包含/是替换，不包含是追加）
    location /test/ {
        proxy_pass http://192.168.100.12:11601/;
        # nginx默认文件上传是2M
        client_max_body_size    50m;
    }
}
```

## 3.2 Nginx文件上传

> 可以直接使用spring boot中自带的静态资源访问文件。

### 3.2.1 Java后台配置

1. 例如上传图片文件，在spring boot项目中的application.yml配置路径。

```properties
image:
  imgUrl: http://192.168.100.12:8000/
  imgPath: /home/kaifa/resource/
```

2. 提倡配置使用@ConfigurationProperties管理配置

```java
// TODO 待完善，下面代码还使用@Value形式。
```

3. 文件上传工具类（涉及代码较多，仅列举核心代码）

```java
/**
 * project - 
 *
 * @author guod
 * @version 1.0
 * @date 日期:2018/8/7 时间:17:46
 * @JDK 1.8
 * @Description 功能模块：文件上传功能
 */
@RestController
@RequestMapping(value = "/file")
public class FileUploadUtils {
    private Logger logger = LoggerFactory.getLogger(getClass());

    @Value("${image.imgUrl}")
    private String imgUrl;

    @Value("${image.imgPath}")
    private String imgPath;
    
        /**
     * 功能描述：提取上传方法为公共方法
     *
     * @param uploadDir 上传文件目录
     * @param file      上传对象
     * @throws Exception
     */
    private String executeUpload(String uploadDir, MultipartFile file) throws Exception {
        //文件后缀名
        String suffix = file.getOriginalFilename().substring(Objects.requireNonNull(file.getOriginalFilename()).lastIndexOf("."));
        //上传文件名
        String filename = UUID.randomUUID() + suffix;
        //服务器端保存的文件对象
        File serverFile = new File(uploadDir + filename);
        // 将上传的文件写入到服务器端文件内
        // file.transferTo(serverFile);
        FileUtils.copyInputStreamToFile(file.getInputStream(), serverFile);
        // 返回用户回显路径
        String returnFile = uploadDir + filename;
        logger.info("服务器图片地址：[{}]", serverFile);
        return returnFile;
    }
    
    /**
     * 功能描述：上传单个文件方法（多文件上传此处不再列举）
     *
     * @param file 前台上传的文件对象（注解请求头信息：enctype=multipart/form-data）
     * @return
     */
    @PostMapping(value = "/upload")
    public Json upload(HttpServletRequest request, MultipartFile file) {
        String returnImage = null;
        try {
            String format = new SimpleDateFormat("yyyy/MM/dd/").format(new Date());
            String uploadDir = imgPath + format + "upload/";
            logger.info("传的路径[{}]", uploadDir);
            //如果目录不存在，自动创建文件夹
            File dir = new File(uploadDir);
            if (!dir.exists()) {
                boolean mkdirs = dir.mkdirs();
            }
            //调用上传方法
            returnImage = executeUpload(uploadDir, file);
        } catch (Exception e) {
            //打印错误堆栈信息
            e.printStackTrace();
            return ResultUtils.errorJson("-200", "上传失败！");
        }
        return ResultUtils.successJson(returnImage);
    }
    
    /**
     * 功能描述：显示图片接口
     *
     * @param response
     * @throws Exception
     */
    @RequestMapping(value = "/image", method = RequestMethod.GET)
    public void imageDisplay(HttpServletResponse response) throws Exception {
        // 文件业务逻辑自行处理，替换下面
        File file = new File("/home/kaifa/resource/2019/02/18/upload/9a9bf096-ab71-                                                            48b3-93fd-b68f3ce255d6.png");
        // 进行文件下载的指定，设置强制下载不打开
        OutputStream outputStream = response.getOutputStream();
        // 设置显示图片
        response.setContentType("image/jpeg");
        // 设置缓存
        response.setHeader("Cache-Control", "max-age=604800");
        // outputStream写入到输出流
        outputStream.write(FileCopyUtils.copyToByteArray(file));
        outputStream.flush();
        outputStream.close();
    }
    
     /**
     * 功能描述：下载文件
     *
     * @param response
     * @throws IOException
     */
    @RequestMapping(value = "/downloadFile")
    public void downloadFile(HttpServletResponse response) throws IOException {
        // 文件业务逻辑自行处理，替换下面
        File file = new File("/home/kaifa/resource/2019/02/18/upload/9a9bf096-ab71-                                                           48b3-93fd-b68f3ce255d6.png");
        OutputStream outputStream = response.getOutputStream();
        // 进行文件下载的指定
        response.setContentType("application/x-download");
        response.setCharacterEncoding("utf-8");
        response.setHeader("Content-Disposition", "attachment;
        filename=" + new String(("下载名称").getBytes("gbk"), "iso8859-1") + ".xls");
        // outputStream写入到输出流
        outputStream.write(FileCopyUtils.copyToByteArray(file));
        outputStream.flush();
        outputStream.close();
    }
                           
     /**
     * 功能描述：直接返回文件流（简略书写，根据业务进行修改）
     *
     * @param path
     * @return
     * @throws Exception
     */
    @RequestMapping(value = "/showFile")
    public ResponseEntity<byte[]> showFile(@RequestParam("path") String path) 
                           throws Exception {
        if (StringUtils.isEmpty(path)) {
            return null;
        }
        String filepath = path;
        URL url = new URL(filepath);
        ResponseEntity r;
        try {
            byte[] bytes1 = IOUtils.toByteArray(url.openConnection().getInputStream());
            MultiValueMap<String, String> headers = new LinkedMultiValueMap<>();
            headers.add("Content-Disposition", "attachment;filename=" + URLEncoder.encode(org.springframework.util.StringUtils.getFilename(filepath), "utf-8"));
            r = new ResponseEntity(bytes1, headers, HttpStatus.OK);
            return r;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

### 3.2.2 Nginx配置

1. 在/usr/local/nginx/server/file目录下配置文件file.conf，名字随意，下面配置根据业务自行修改。

```properties
server {
    listen  8000;
    location / {
        root /home/kaifa/resource/;
    }
}
```

## 3.3 缓存配置

TODO 待完善

## 3.4重定向问题

**问题情况：**nginx监听的端口例如是8080，外网开放的端口为80，将80端口NAT到内部的8080端口，这时使用外网地址：http://mydomain/test访问时如果不在域名后面加/，那么域名地址会自动变为http://mydomain:8080/test/。

**解决方法：**这是因为nginx做了端口重定向，只需要在nginx.conf配置文件的http或server中添加。

```properties
port_in_redirect off;
```

