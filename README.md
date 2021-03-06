Backup repository for the bs2023 website. Just recreate it in case that I am out of town.

```bash
$ yum -y install python3
$ yum -y install git supervisor nginx
$ cd /home
$ git clone https://github.com/ian-quinn/biscuit.git
$ cd biscuit
$ python3 -m venv venv
$ source venv/bin/activate
(venv) $ pip install -r requirements.txt
# check the environment by: flask run
(venv) $ flask db init
(venv) $ flask db migrate -m "init"
(venv) $ flask db upgrade
# check 127.0.0.1:5000 works smoothly
(venv) $ pip install gunicorn
(venv) $ deactivate
$ cp /home/biscuit/deployment/supervisor/biscuit.ini /etc/supervisord.d/biscuit.ini
$ cp /home/biscuit/deployment/nginx/biscuit.conf /etc/nginx/conf.d/biscuit.conf
# $ chmod o+w /etc/nginx/conf.d if you dont have accessibility
# make sure the path in .conf directs to right SSL certificate
# make sure the port 80 443 are not blocked by the firewalld, SeLinux, or your server provider
$ systemctl start supervisord
$ systemctl start nginx
```


requirements.txt
```
Flask==2.1.1
Flask-Migrate==3.1.0
Flask-SQLAlchemy==2.5.1
Flask-WTF==1.0.1
Flask-Mail==0.9.1
python-dotenv==0.20.0
email-validator==1.1.3
```