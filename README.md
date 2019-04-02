Quickly thrown together faucet. Currently set to give out 10 BTN a pop. Limiting is set to 3 per day.

## Running this
First make sure Bitcoinnovad is running and fully sync'd.
Then start `Bitcoinnova-service` with these args:

`./Bitcoinnova-service -w walletname -p walletpass --bind-address 127.0.0.1 --bind-port 8070 --rpc-password rpcpassword`


`pip3 install -r requirements.txt`
You'll need to create a file called 'faucet.ini'.
The file should look like this:
```ini

[uwsgi]
module = wsgi:application
socket = /tmp/uwsgi.sock
chmod-socket = 666
chdir  = /home/**YourUser**/bitcoinnova-faucet/
wsgi-file =wsgi.py
master = true
processes = 1
threads = 1
vacuum = true
die-on-term=true
http = :80

#environment
env=RECAPTCHA_PUBLIC_KEY=
env=RECAPTCHA_PRIVATE_KEY=
env=SECRET_KEY=random_string
env=WTF_CSRF_SECRET_KEY=random_string
env=FAUCET_ADDR=
env=RPC_PASS=

```

After that, run
```python
python3 -c 'from faucet import db;db.create_all()'
```
then `uwsgi --ini faucet.ini`. Make sure you have Bitcoinnovad and Bitcoinnova-service running.
I left in the google analytics because I couldn't find a way to add that at deployment. Enjoy :)
