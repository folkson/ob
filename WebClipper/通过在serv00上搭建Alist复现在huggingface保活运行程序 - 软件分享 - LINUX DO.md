[通过在serv00上搭建Alist复现在huggingface保活运行程序 - 软件分享 - LINUX DO](https://linux.do/t/topic/166344) 

 [通过在serv00上搭建Alist复现在huggingface保活运行程序 - 软件分享 - LINUX DO](https://linux.do/t/topic/166344) 

 [通过在serv00上搭建Alist复现在huggingface保活运行程序 - 软件分享 - LINUX DO](https://linux.do/t/topic/166344) 

 [](#p-1267787-huggingfaceserv00-1)本文主要集中在huggingface保活，在serv00部署的所有服务都可以参考下面方式进行保活
----------------------------------------------------------------------------------

[![](https://linux.do/uploads/default/optimized/3X/f/c/fcb542d8db619163a6727a6d49e6dcbff30a35d4_2_690x401.jpeg)
](https://linux.do/uploads/default/original/3X/f/c/fcb542d8db619163a6727a6d49e6dcbff30a35d4.jpeg "image")

[![](https://linux.do/uploads/default/optimized/3X/a/2/a2aaf32a1287e2292ecb1371aff272dd8c36e5b2_2_659x500.jpeg)
](https://linux.do/uploads/default/original/3X/a/2/a2aaf32a1287e2292ecb1371aff272dd8c36e5b2.jpeg "image")

域名托管不在这里叙述，可以看这部分，也可以使用CF进行托管

**创建Website**

[![](https://linux.do/uploads/default/optimized/3X/f/8/f806b69c9d0bb6a20f4c70ef353f661eb7a91dc5_2_690x399.png)
](https://linux.do/uploads/default/original/3X/f/8/f806b69c9d0bb6a20f4c70ef353f661eb7a91dc5.png "image")

记住你创建的数据库，等会需要用  

[![](https://linux.do/uploads/default/optimized/3X/7/5/75d911cd75d7244b7a419ac317e4a7685111e602_2_690x344.png)
](https://linux.do/uploads/default/original/3X/7/5/75d911cd75d7244b7a419ac317e4a7685111e602.png "image")

进入 `public_html` 文件下

```null
cd /usr/home/你的serv名字/domains/域名/public_html

```

下载alist包

```null
wget https://github.com/uubulb/alist-freebsd/releases/download/v3.36.0/alist

```

在下载`alist`同级目录创建`data`文件，并进入data文件在其下创建`config.json`文本

[![](https://linux.do/uploads/default/optimized/3X/9/d/9ded4a660caa19f7a4b2cef669b777ae4b3591fc_2_690x412.png)
](https://linux.do/uploads/default/original/3X/9/d/9ded4a660caa19f7a4b2cef669b777ae4b3591fc.png "image")

[![](https://linux.do/uploads/default/optimized/3X/8/0/80adde7c84a9fbb86a7c7e4489cfc3bf52cb2858_2_690x279.png)
](https://linux.do/uploads/default/original/3X/8/0/80adde7c84a9fbb86a7c7e4489cfc3bf52cb2858.png "image")

把代码放入其中

```null
{
  "force": false,
  "site_url": "",
  "cdn": "",
  "jwt_secret": "random_generated",
  "token_expires_in": 48,
  "database": {
    "type": "sqlite3",
    "host": "",
    "port": 0,
    "user": "",
    "password": "",
    "name": "",
    "db_file": "data\\data.db",
    "table_prefix": "x_",
    "ssl_mode": "",
    "dsn": ""
  },
  "meilisearch": {
    "host": "http://localhost:7700",
    "api_key": "",
    "index_prefix": ""
  },
  "scheme": {
    "address": "0.0.0.0",
    "http_port": 5244,
    "https_port": -1,
    "force_https": false,
    "cert_file": "",
    "key_file": "",
    "unix_file": "",
    "unix_file_perm": ""
  },
  "temp_dir": "data\\temp",
  "bleve_dir": "data\\bleve",
  "dist_dir": "",
  "log": {
    "enable": true,
    "name": "data\\log\\log.log",
    "max_size": 50,
    "max_backups": 30,
    "max_age": 28,
    "compress": false
  },
  "delayed_start": 0,
  "max_connections": 0,
  "tls_insecure_skip_verify": true,
  "tasks": {
    "download": {
      "workers": 5,
      "max_retry": 1
    },
    "transfer": {
      "workers": 5,
      "max_retry": 2
    },
    "upload": {
      "workers": 5,
      "max_retry": 0
    },
    "copy": {
      "workers": 5,
      "max_retry": 2
    }
  },
  "cors": {
    "allow_origins": [
      "*"
    ],
    "allow_methods": [
      "*"
    ],
    "allow_headers": [
      "*"
    ]
  },
  "s3": {
    "enable": false,
    "port": 5246,
    "ssl": false
  }
}

```

修改

```null
database": {
    "type": "sqlite3",
    "host": "",
    "port": 0,
    "user": "",
    "password": "",
    "name": "",

```

`"site_url": "",`为你你自己的：格式：`https://xxx.com`  
`type`改为`mysql`，余信息填写你前面创建的mysql数据库  
`"http_port": 5244,`改为你自己放行的端口  

[![](https://linux.do/uploads/default/optimized/3X/3/e/3e69b8675a74425823d52e82f1fcc1af6edb6f09_2_280x500.png)
](https://linux.do/uploads/default/original/3X/3/e/3e69b8675a74425823d52e82f1fcc1af6edb6f09.png "image")

赋权并执行alist

```null
chmod +x alist && ./alist server

```

获取你的密码并保存（`只出现一次`）

进入后台并更改你的账号密码  
初始账号：admin  
密码：前面获取到的  

[![](https://linux.do/uploads/default/optimized/3X/5/3/53f6b9138fc4a6b4a8fd733ea8f196c09c52bdbf_2_609x500.jpeg)
](https://linux.do/uploads/default/original/3X/5/3/53f6b9138fc4a6b4a8fd733ea8f196c09c52bdbf.jpeg "image")

[![](https://linux.do/uploads/default/optimized/3X/5/6/56e451de6bdfdddf6023b14d3efc701ed27726f8_2_690x295.png)
](https://linux.do/uploads/default/original/3X/5/6/56e451de6bdfdddf6023b14d3efc701ed27726f8.png "image")

[](#p-1267787-h-8)下面是保活的重点
--------------------------

在alist同级目录创建`start.sh`文件，这个是启动文件，执行`./alist server`命令（**因为alist本身不自带，需要自行创建**）其余项目类似

[![](https://linux.do/uploads/default/optimized/3X/8/f/8f9f1a46ca9bc62f11259704cbddf2691362d7af_2_607x500.png)
](https://linux.do/uploads/default/original/3X/8/f/8f9f1a46ca9bc62f11259704cbddf2691362d7af.png "image")

```null
#!/bin/bash

cd /usr/home/Serv00账号名/domains/域名/public_html
chmod +x alist
./alist server

```

路径对应你自己的，并将其中的`serv00`和`域名`换为你自己的，下面同理

在alist同级目录创建`restart.sh`文件，这个文件是保活文件，用于检测`start.sh`是否运行，没运行则重启服务

```null
#!/bin/bash


SCRIPT_PATH="/usr/home/Serv00账号名/domains/域名/public_html/start.sh"
WORK_DIR="/usr/home/Serv00账号名/domains/域名/public_html"


if ! pgrep -f "$SCRIPT_PATH" > /dev/null
then
    
    cd "$WORK_DIR"
    nohup ./start.sh > /dev/null 2>&1 &
    echo "Restarted start.sh at $(date)" >> "$WORK_DIR/restart_log.txt"
fi

```

```null
chmod +x start.sh restart.sh

```

[![](https://linux.do/uploads/default/optimized/3X/0/b/0b652ea21713f997544a09773be6f074cad76aee_2_690x232.png)
](https://linux.do/uploads/default/original/3X/0/b/0b652ea21713f997544a09773be6f074cad76aee.png "image")

  

[![](https://linux.do/uploads/default/optimized/3X/4/4/44615803c58ed3326b8c22c1501561900a05d84d_2_401x500.png)
](https://linux.do/uploads/default/original/3X/4/4/44615803c58ed3326b8c22c1501561900a05d84d.png "image")

  

[![](https://linux.do/uploads/default/optimized/3X/1/4/14d1ec205ec7a44b64c58d8a06884d4924a70481_2_690x357.png)
](https://linux.do/uploads/default/original/3X/1/4/14d1ec205ec7a44b64c58d8a06884d4924a70481.png "image")

放入以下代码：

```null
FROM python:3.8-slim-buster

WORKDIR /app

RUN apt-get update && apt-get install -y openssh-client
RUN pip install paramiko schedule flask

COPY vps_monitor.py .

EXPOSE 8080

CMD ["python", "-u", "vps_monitor.py"]

```

[![](https://linux.do/uploads/default/optimized/3X/9/1/9132eeba8f19a0e4e10fb18aa2978f41c06fd79c_2_454x500.png)
](https://linux.do/uploads/default/original/3X/9/1/9132eeba8f19a0e4e10fb18aa2978f41c06fd79c.png "image")

编辑 `README.md，`在`README.md` 添加`app_port: 8080`

[![](https://linux.do/uploads/default/original/3X/8/d/8d18bff890a849a03355d0d6e5ae7c918c3781ce.png)
](https://linux.do/uploads/default/original/3X/8/d/8d18bff890a849a03355d0d6e5ae7c918c3781ce.png "image")

  

[![](https://linux.do/uploads/default/optimized/3X/5/5/5513a28922fc9a036af831db718e092bf85572a0_2_475x500.png)
](https://linux.do/uploads/default/original/3X/5/5/5513a28922fc9a036af831db718e092bf85572a0.png "image")

创建一个名为`vps_monitor.py`的文本

[![](https://linux.do/uploads/default/optimized/3X/a/5/a5938a6a90b6b0cc3ad3cb6d5b5867dbb969ff94_2_690x240.png)
](https://linux.do/uploads/default/original/3X/a/5/a5938a6a90b6b0cc3ad3cb6d5b5867dbb969ff94.png "image")

放入以下代码并保存

```null
import paramiko
import schedule
import time
import os
import sys
from flask import Flask, jsonify, render_template_string
from threading import Thread
import logging

app = Flask(__name__)

vps_status = {}


logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s',
    handlers=[
        logging.StreamHandler(sys.stdout),
        logging.StreamHandler(sys.stderr)
    ]
)
logger = logging.getLogger()

def get_vps_configs():
    configs = []
    index = 1
    while True:
        hostname = os.environ.get(f'HOSTNAME_{index}')
        if not hostname:
            break
        
        config = {
            'index': index,
            'hostname': hostname,
            'username': os.environ.get(f'USERNAME_{index}'),
            'password': os.environ.get(f'PASSWORD_{index}'),
            'script_path': os.environ.get(f'SCRIPT_PATH_{index}')
        }
        configs.append(config)
        
        logger.info(f"Config {index}: {config['hostname']}, {config['username']}, {config['script_path']}")
        
        index += 1
    return configs

def check_and_run_script(config):
    logger.info(f"Checking VPS {config['index']}: {config['hostname']}")
    client = None
    try:
        client = paramiko.SSHClient()
        client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
        
        client.connect(
            hostname=config['hostname'],
            username=config['username'],
            password=config['password'],
            port=22
        )
        
        script_path = config['script_path']
        script_name = os.path.basename(script_path)
        
        
        last_pid = vps_status.get(config['hostname'], {}).get('pid', None)
        
        
        if last_pid:
            check_command = f"ps -p {last_pid} -o pid,etime,args"
        else:
            check_command = f"ps -eo pid,etime,args | grep {script_name} | grep -v grep"
        
        stdin, stdout, stderr = client.exec_command(check_command)
        output = stdout.read().decode('utf-8').strip()
        
        if output and (last_pid or script_path in output):
            parts = output.split()
            pid = last_pid if last_pid else parts[0]
            runtime = parts[1] if len(parts) > 1 else "Unknown"
            status = "Running"
            logger.info(f"Script is running. PID: {pid}, Runtime: {runtime}")
        else:
            logger.info(f"Script not running on {config['hostname']}. Executing restart script.")
            restart_command = f"nohup /bin/sh {script_path} > /dev/null 2>&1 & echo $!"
            stdin, stdout, stderr = client.exec_command(restart_command)
            new_pid = stdout.read().decode('utf-8').strip()
            
            if new_pid.isdigit():
                pid = new_pid
                runtime = "Just started"
                status = "Restarted"
                logger.info(f"Script restarted. New PID: {pid}")
            else:
                pid = "N/A"
                runtime = "N/A"
                status = "Restart Failed"
                logger.error(f"Failed to restart script")
        
        vps_status[config['hostname']] = {
            'index': config['index'],
            'status': status,
            'last_check': time.strftime('%Y-%m-%d %H:%M:%S'),
            'username': config['username'],
            'runtime': runtime,
            'pid': pid
        }
        
    except Exception as e:
        logger.error(f"Error occurred while checking VPS {config['index']} - {config['hostname']}: {str(e)}")
        vps_status[config['hostname']] = {
            'index': config['index'],
            'status': f"Error: {str(e)}",
            'last_check': time.strftime('%Y-%m-%d %H:%M:%S'),
            'username': config['username'],
            'runtime': "N/A",
            'pid': "N/A"
        }
    finally:
        if client:
            client.close()

def check_all_vps():
    logger.info("Starting VPS check")
    vps_configs = get_vps_configs()
    for config in vps_configs:
        check_and_run_script(config)
    
    
    table = "+---------+-----------------------+----------+-------------------------+----------+----------+-------+\n"
    table += "| Index   | Hostname              | Status   | Last Check              | Username | Runtime  | PID   |\n"
    table += "+---------+-----------------------+----------+-------------------------+----------+----------+-------+\n"
    
    
    for hostname, status in vps_status.items():
        table += "| {:<7} | {:<21} | {:<8} | {:<23} | {:<8} | {:<8} | {:<5} |\n".format(
            status['index'],
            hostname[:21],
            status['status'][:8],
            status['last_check'],
            status['username'][:8],
            status['runtime'][:8],
            status['pid'][:5]
        )
        table += "+---------+-----------------------+----------+-------------------------+----------+----------+-------+\n"
    
    logger.info("\n" + table)

@app.route('/')
def index():
    html = '''
    <h1>VPS Status Overview</h1>
    <table border="1">
        <tr>
            <th>Index</th>
            <th>Hostname</th>
            <th>Status</th>
            <th>Last Check</th>
            <th>Username</th>
            <th>Runtime</th>
            <th>PID</th>
        </tr>
        {% for hostname, data in vps_status.items() %}
        <tr>
            <td>{{ data.index }}</td>
            <td><a href="/status/{{ hostname }}">{{ hostname }}</a></td>
            <td>{{ data.status }}</td>
            <td>{{ data.last_check }}</td>
            <td>{{ data.username }}</td>
            <td>{{ data.runtime }}</td>
            <td>{{ data.pid }}</td>
        </tr>
        {% endfor %}
    </table>
    '''
    return render_template_string(html, vps_status=vps_status)

@app.route('/status/<hostname>')
def vps_status_detail(hostname):
    if hostname in vps_status:
        return jsonify(vps_status[hostname])
    else:
        return jsonify({"error": "VPS not found"}), 404

@app.route('/health')
def health_check():
    return jsonify({"status": "healthy", "uptime": time.time() - start_time}), 200

def run_flask():
    app.run(host='0.0.0.0', port=8080)

def main():
    global start_time
    start_time = time.time()
    
    logger.info("===== VPS monitoring script is starting =====")
    
    flask_thread = Thread(target=run_flask)
    flask_thread.start()
    logger.info("Flask server started in background")

    vps_configs = get_vps_configs()
    logger.info(f"Found {len(vps_configs)} VPS configurations")

    logger.info("Running initial VPS check")
    check_all_vps()

    schedule.every(15).minutes.do(check_all_vps)
    logger.info("Scheduled VPS check every 15 minutes")
    
    logger.info("===== VPS monitoring script is running =====")
    
    heartbeat_count = 0
    while True:
        schedule.run_pending()
        time.sleep(60)
        heartbeat_count += 1
        if heartbeat_count % 5 == 0:  
            logger.info(f"Heartbeat: Script is still running. Uptime: {heartbeat_count} minutes")

if __name__ == "__main__":
    main()

```

[![](https://linux.do/uploads/default/optimized/3X/e/1/e1c82cbf58c21093a6873f85b07ee47ee93c784c_2_655x499.png)
](https://linux.do/uploads/default/original/3X/e/1/e1c82cbf58c21093a6873f85b07ee47ee93c784c.png "image")

[![](https://linux.do/uploads/default/optimized/3X/d/f/dfb2f26ce80334ca44b14770eb970b6da7e8c280_2_417x499.png)
](https://linux.do/uploads/default/original/3X/d/f/dfb2f26ce80334ca44b14770eb970b6da7e8c280.png "image")

```null
USERNAME_X   
HOSTNAME_X
PASSWORD_X
SCRIPT_PATH_X

```

X为**数字编号** 如（1，2，3），每个主机4项要相同才能对应  
如：

```null
USERNAME_1     
HOSTNAME_1
PASSWORD_1
SCRIPT_PATH_1

```

`USERNAME` 为serv00用户名，`HOSTNAME` 为主机名（如：`s3.serv00.com` ），`PASSWORD` 为serv00密码，`SCRIPT_PATH` 为执行脚本的绝对路径（如：`/usr/home/serv00用户名/domains/域名/public_html/restart.sh` ）  

[![](https://linux.do/uploads/default/optimized/3X/a/1/a1266d5ec078fd989317b47fd2a7364a9cfcc669_2_690x264.png)
](https://linux.do/uploads/default/original/3X/a/1/a1266d5ec078fd989317b47fd2a7364a9cfcc669.png "image")

`running`后点击`logs`可以查看进度

[![](https://linux.do/uploads/default/optimized/3X/1/5/1532b29c040f57b87ea9e80b7f61806f3d8c4554_2_690x499.png)
](https://linux.do/uploads/default/original/3X/1/5/1532b29c040f57b87ea9e80b7f61806f3d8c4554.png "image")

点击`app`可看到服务器正常启动，这时你可以访问你的网址进入你部署的项目

[![](https://linux.do/uploads/default/optimized/3X/6/9/69f996b42921513614ae83ff04a977be201f32bd_2_690x254.png)
](https://linux.do/uploads/default/original/3X/6/9/69f996b42921513614ae83ff04a977be201f32bd.png "image")

[](#p-1267787-huggingface-wink-14)在huggingface部署的重启脚本避免了后台被清除，因为他是远程访问的 ![](https://linux.do/images/emoji/apple/wink.png?v=12)

-------------------------------------------------------------------------------------------------------------------------------

他可以保活账号并且保活运行的程序，可以多个账号一起保活，前面的4个变量序号一致就行

*   [GitHub - uubulb/alist-freebsd: Automatically build alist (FreeBSD version)](https://github.com/uubulb/alist-freebsd)
    
*   [配置文件 | AList文档](https://alist.nn.ci/zh/config/configuration.html#%E5%88%9D%E5%A7%8B%E9%85%8D%E7%BD%AE)