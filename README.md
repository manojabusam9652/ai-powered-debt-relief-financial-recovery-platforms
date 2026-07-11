AI-Debt-Relief/
│
├── app.py
├── model.py
├── database.db
├── requirements.txt
├── templates/
│   ├── index.html
│   ├── login.html
│   ├── dashboard.html
│   └── result.html
│
├── static/
│   ├── style.css
│   └── script.js
│
└── README.md
CREATE TABLE users(
id INTEGER PRIMARY KEY AUTOINCREMENT,
name TEXT,
email TEXT,
password TEXT
);
CREATE TABLE finance(
id INTEGER PRIMARY KEY AUTOINCREMENT,
user_id INTEGER,
income REAL,
expenses REAL,
loan REAL,
interest REAL,
emi REAL,
savings REAL
);
from flask import Flask,render_template,request
import sqlite3

app=Flask(__name__)

@app.route('/')
def home():
    return render_template("index.html")

@app.route('/predict',methods=['POST'])
def predict():

    income=float(request.form['income'])
    expenses=float(request.form['expenses'])
    loan=float(request.form['loan'])

    ratio=loan/income

    if ratio<1:
        risk="Low"
        score=90
    elif ratio<3:
        risk="Medium"
        score=65
    else:
        risk="High"
        score=35

    return render_template("result.html",
                           risk=risk,
                           score=score)

app.run(debug=True)
def financial_score(income,expense,loan):

    savings=income-expense

    if savings>income*0.30:
        return 95

    elif savings>income*0.15:
        return 75

    else:
        return 45
<form action="/predict" method="POST">

<input type="number" name="income"
placeholder="Monthly Income">

<input type="number" name="expenses"
placeholder="Monthly Expenses">

<input type="number" name="loan"
placeholder="Total Loan">

<button type="submit">
Analyze
</button>

</form>
<h2>Financial Analysis</h2>

<h3>Risk Level:
{{risk}}
</h3>

<h3>Financial Score:
{{score}}%
</h3>