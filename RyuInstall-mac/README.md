# RyuInstall
How to install ryu on mac？

#### 1. Install the virtualenv
`$ pip install virtualenv`

#### 2. Setup virtualenv
+ Create a new virtualenv called "ryu_venv":
  <br>
  `$ virtualenv ryu_venv`

+ Activate the newly created virtualenv:
  <br>
  `$ source ./ryu_venv/bin/activate`
+ Display
  <br>
  `(ryu_venv)$`

#### 3. Install the Ryu packages
```
(ryu_venv)$ pip install greenlet
(ryu_venv)$ pip install ryu
```
#### 4. Starting Ryu
+ Start ryu:
  <br>
  `(ryu_venv)$ ryu-manager`
+ You may need to clone official source code for running sample application.
  <br>
  `$ git clone https://github.com/osrg/ryu.git`
+ Running a sample application
  <br>
  `(ryu_venv)$ ryu-manager ./ryu/ryu/ryu/app/simple_switch_13.py`
#### 5. Deactivate virtualenv
+ Leave python virtualenv
  <br>
  `(ryu_venv)$ deactivate`

#### Reference
[RyuInstallHelper](https://github.com/sdnds-tw/ryuInstallHelper/blob/master/macosx/README.md)
