# iot1
# Uploading values of IoT sensors in the Cloud

- To upload the values we will the basic http model of Request Reponse model
![Request Response model](https://www.researchgate.net/publication/311571526/figure/fig3/AS:438170157359106@1481479314691/HTTP-request-response-model.png)
*Figure basic Request Response Model*

- To achive this we will use a simple python library which comes with preinstalled http request reponse model as a web framework.
- Create simple web url in flask app which can access the post requests made by the IOT devices and give positive reponse.

***Python flask webframework code***
```python
from flask import Flask,request,jsonify,render_template,redirect

app = Flask(__name__)
gloabl_variable = 0

@app.route("/")
def hello_world():
    return redirect('/multidata')
    # return render_template('auto_update.html')

@app.route('/get_multiple_data')
def get_multidata():
    return jsonify(datas[::-1])

@app.route('/multidata')
def multidata():
    return render_template('multiple_ready.html')

@app.route('/set_data',methods=['GET','POST'])
def set_data():
    print(request.args.to_dict())
    global gloabl_variable
    gloabl_variable = request.args.get('distance')
    return 'thank you'

if __name__=='__main__':
    app.run(host='0.0.0.0',debug= True)
```
> Here the template that is renders

***basic html page syntax***

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Auto update page</title>
</head>
<body>
    <center>
        <h1>Distance</h1>
        <h2 id = 'rondechaka'>0</h2>
    </center>
    <script>

        var element = document.getElementById("rondechaka"); 
        setInterval(function() { 
            fetch('get_data')
                .then(res => res.json())
                .then(data => element.innerHTML=data.distance)
        }, 500);
    </script>
</body>
</html>
```
- Host this in the AWS EC2 server
- use the amazon ec2 server ip address to post the data from IOT using requests module.

***Example request module code***
```python
import requests
import random
import time
for i in range(100):
    x = requests.get(f'http://10.10.27.169:5000/set_data?distance={random.randint(0,99)}')
    print(x)
    time.sleep(1)
```
