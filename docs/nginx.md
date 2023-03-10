# Nginx

For blog visit [dev.to](https://dev.to/nick_langat/how-to-deploy-a-fastapi-app-to-aws-ec2-server-46d4).

## Step by step Implementation :

### Step 1: LOCAL SETUP

We start by creating a local workspace on our machine so navigate to desktop and create a folder hello_fastapi and open
it using your code editor.
Next let's create a virtual environment to hold our dependencies.

!!! note

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.


For Linux

```commandline
Virtualenv env
```

For Windows

```commandline
python -m venv env
```

Then activate it using

```commandline
env/Scripts/activate on windows
source env/bin/activate on linux/unix
```

Now that we have our environment set up let's install what we need.

```commandline
pip install fastapi uvicorn gunicorn psycopg2-binary
```

You may add the deps depending on what your project will need in order to work.
With the deps installed we then create a file that will hold them all.

```commandline
pip install fastapi uvicorn gunicorn psycopg2-binary
```

Next create a file called main.py at the root of the project and add the following code to it.

```Python hl_lines="1"
import uvicorn

if __name__ == "__main__":
    uvicorn.run("app:app", host="0.0.0.0", port=8000, reload=True)
```

Then create another file at the root called app.py with the following code:

```python 
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

origins = ["*"]

app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

@app.get("/", tags=["Root"])
async def read_root():
    return {"message": "Welcome to the API!"}
```

Let's test the app to see if it is running well on our local browser:
Run the server by:

```commandline
uvicorn  app:app --reload
```

### Step 2: VERSION CONTROL

While still in the root of the project we can issue the following commands to push our code to GitHub:

```commandline
git init
git add .
git commit -m 'initial'
git remote add origin REMOTE URL HERE
git push -u origin master
```

### Step 3: DEPLOYING TO AWS EC2

The assumption is making here is that you have an AWS account and have created a free tier EC2 server running on Ubuntu.
Feel free to check out docs on how to do that [here](https://docs.aws.amazon.com/efs/latest/ug/gs-step-one-create-ec2-resources.html).
These steps will work on any Ubuntu machine though so if you are on DigitalOcean, Linode or Oracle it should work well.
Let's ssh into our remote machine then:

```text
ssh -i 'example.pem' ubuntu@ec2-IP-ADDRESS.us-east-2.compute.amazonaws.com
```

Once you are inside the remote machine we need to install Python and NGINX.
Issue the following commands on the terminal:

```commandline
sudo apt update
sudo apt install python3-pip python3-dev libpq-dev postgresql postgresql-contrib nginx curl
sudo -H pip3 install --upgrade pip
```

Next create a folder that will carry our files:
```text
mkdir apiv1 && cd apiv1
```
Then create and activate a virtual environment like we did before.
After that initiate git inside the folder and pull your code from gitHub.

```commandline
git init
git remote add origin REPO URL
git pull origin master
```
Now we install our deps like so:

```commandline
pip install -r requirements.txt
```
Deactivate the virtualenv after running the local server and ensuring no errors are reported.
Next up we need to bind our application to a gunicorn module that serves as an interface to your application, translating client requests from HTTP to Python calls that your application can process.
So we start by creating a socket file:

```commandline
sudo nano /etc/systemd/system/gunicorn.socket
```

Add the following code to it:
```text
[Unit]
Description=gunicorn socket

[Socket]
ListenStream=/run/gunicorn.sock

[Install]
WantedBy=sockets.target
```
Save and close that file. Next we create a service file:
```commandline
sudo nano /etc/systemd/system/gunicorn.service
```

Add the following code to it:

``` yaml
# (1)
[Unit]
Description=gunicorn daemon
Requires=gunicorn.socket
After=network.target

[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/apiv1
ExecStart=/home/ubuntu/apiv1/env/bin/gunicorn \
          --access-logfile - \
          --workers 5 \
          --bind unix:/run/gunicorn.sock \
          --worker-class uvicorn.workers.UvicornWorker \
          app:app

[Install]
WantedBy=multi-user.target
```
test.1. :

Save and close as well.
Next start the Gunicorn socket:

```commandline
sudo systemctl start gunicorn.socket
```
Then enable it by:

```commandline
sudo systemctl enable gunicorn.socket
```

Now that Gunicorn is set up, next we’ll configure Nginx to pass traffic to the process.
Start by creating and opening a new server block in Nginx’s sites-available directory:

```commandline
sudo nano /etc/nginx/sites-enabled/api
```

Add the following lines to it:
```text
server {
    listen 80;
    server_name server_domain_or_IP;
    location / {
        proxy_pass http://unix:/run/gunicorn.sock;
    }
}
```
Save this file as well. Test that the config is okay by:

```commandline
sudo nginx -t
```
If all is well then we need to restart the services so go ahead and:

```commandline
sudo systemctl daemon-reload
sudo systemctl restart gunicorn
sudo systemctl restart nginx
```
One last step is to ensure that you have allowed HTTP access via port 80 on your instance's security group section. Do the same for port 443 if your app is served over HTTPS.
If all the above is done then go to your instance's public IP address or domain, and hopefully you will see your app being displayed there.



