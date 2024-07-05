# Link to GitHub: https://github.com/DominateProbabilitywithconcent/InsecLabs/tree/main
# Task 1: normal transaction with CRSF vulnerability
 
 Firstly, start the terminal in the folder where locates these 2 files.<br>
 ![alt text](./SecLabImages/Screenshot%202024-07-05%20214703.png)<br>
 Then, running __python target.py__ command on the terminal.<br>
 ![a](./SecLabImages/csrfstart.png)<br>
 In the terminal, it will show the address of the transaction website, which is http://127.0.0.1:5000. We will access to this website using Microsoft Edge.<br>
 ![a](./SecLabImages/Screenshot%202024-07-05%20215400.png)<br>

## 1.1: Login, Check balance
 
 Access to login screen through __/login__ url.<br>
 ![a](./SecLabImages/Screenshot%202024-07-05%20215546.png)<br>
Check account username and password in the program. It show Alice's username and password which is __'alice'__.<br>
![a](./SecLabImages/Screenshot%202024-07-05%20215659.png)<br>
 Login as Alice.<br>
 ![a](./SecLabImages/Screenshot%202024-07-05%20215950.png)<br>
 ![a](./SecLabImages/Screenshot%202024-07-05%20220046.png)<br>
Divert url to __/balance__ and check balance.<br>
![a](./SecLabImages/Screenshot%202024-07-05%20220242.png)<br>
## 1.2: Doing the transaction
Divert to transaction page via __/transfer__ url.<br>
![a](./SecLabImages/Screenshot%202024-07-05%20221143.png)<br>
Transfer 1234$ to Bob account.<br>
![a](./SecLabImages/Screenshot%202024-07-05%20221223.png)<br>
Notifcation that transaction is successful.<br>
![a](./SecLabImages/Screenshot%202024-07-05%20221228.png)<br>
Check Alice's balance.<br>
![a](./SecLabImages/Screenshot%202024-07-05%20221235.png)<br>
Log out.<br>
![alt text](./SecLabImages/Screenshot%202024-07-05%20232214.png)<br>
Chcek Bob's balance.<br>
![alt text](./SecLabImages/Screenshot%202024-07-05%20232315.png)<br>
![alt text](./SecLabImages/Screenshot%202024-07-05%20232323.png)<br>
## 1.3: Tranfer money illegitimately
Activate the __hidden_form.html__.<br>
![alt text](./SecLabImages/Screenshot%202024-07-05%20233914.png)<br>
Result: The current login account which is Alice got hacked. The html code automatically ran the moment user open the __hidden_form.html__.<br>
![alt text](./SecLabImages/Screenshot%202024-07-05%20232836.png)<br>
Check Alice's balance.<br>
![a](./SecLabImages/Screenshot%202024-07-05%20234222.png)<br>

# Task 2: CSRF Countermeasure implementation

## 2.1: Solution 1:
### __CSRF Token__

 First, we import __flask_wtf__ to handle CSRF token and __os__ for creating random byte string of 32 bytes (256 bits).<br>
![a](./SecLabImages/Screenshot%202024-07-06%20010434.png)<br>
These strings are unpredictable enough while tieing with user's session. Potential candidates as a secret key <br>
![a](./SecLabImages/Screenshot%202024-07-06%20010512.png)<br>
 Then, Add __<input type="hidden" name="csrf_token" value="{{ csrf_token() }}">__ into the html part of __transfer__ function to start deploying.<br>
![a](./SecLabImages/Screenshot%202024-07-06%20004909.png)<br>
Result when trying to attack via hideen_form.html file:<br>
![a](./SecLabImages/Screenshot%202024-07-06%20011220.png)<br>
 

## 2.2: Solution 2: