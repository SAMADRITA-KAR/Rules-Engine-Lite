# Rules-Engine-Lite

PROJECT OVERVIEW :-
A modular fraud-risk scoring and decision evaluation system built using Python. It dynamically loads rules written in JSON, evaluates incoming transaction/customer data,
calculates a risk score, and returns a PASS/FAIL decision with explainable reasoning.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
FEATURES :-
1.Loads rules dynamically from JSON.
2.Supports multiple comparison operators (>, <, >=, <=, ==, !=).
3.Handles nested fields (e.g., transaction.amount).
4.Case-insensitive string matching.
5.Provides score, decision result, and trace logs.
6.Logging for debugging and auditing.
7.Flexible architecture for easily adding more rules.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
TECH STACK :-
1.Language: Python 3.12.12
2.Libraries: Standard Library (operator, json, logging, os).
3.Environment: Works on Linux/Windows/Mac & Google Colab.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
APPROACH :-
1.The Rules Engine evaluates financial transactions using deterministic rule-based scoring.
2.Each rule contributes positively or negatively to a risk score.
3.Total score determines the final decision tier (Approve/Review/Deny).
4.Operators support nested attributes and case-insensitive matching to handle varied data sources.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
HOW TO RUN :-
1.Install dependencies : 
pip install -r requirements.txt
2.Run the main script :
python main.py   
[This will load rules from 'src/decision_engine/rules.json', evaluate them against a sample payload, and print the result].
3.Sample code :
from decision_engine.rule_evaluator import load_rules_from_json, evaluate_rules 
payload = {"transaction": {"amount": 200000, "country": "USA"}, 
"customer": {"account_age_days": 90}}
rules = load_rules_from_json("src/decision_engine/rules.json") 
result = evaluate_rules(rules, payload) 
print(result)
4.Running automated tests :
pytest
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
SAMPLE INPUTS & OUTPUTS :-
1st Test Case --
Rules :
{"pass_score": 200, "rules": [{"field": "transaction.amount", "operator": "<=", "value": 500000, "score": 100}, 
{"field": "transaction.country", "operator": "!=", "value": "sanctioned", "score": 200}, 
{"field": "customer.account_age_days", "operator": ">=", "value": 30, "score": 50}]}

Payload :
{"transaction": {"amount": 200000, "country": "USA"}, "customer": {"account_age_days": 90}}

Output :
{'decision': 'PASS',
 'score': 350,
 'trace': ['Rule 1: transaction.amount <= 500000 (+100)',
  'Rule 2: transaction.country != sanctioned (+200)',
  'Rule 3: customer.account_age_days >= 30 (+50)']}

2nd Test Case --
Rules :
{"pass_score": 200, "rules": [{"field": "transaction.amount", "operator": "<=", "value": 500000, "score": 100},
{"field": "transaction.country", "operator": "!=", "value": "sanctioned", "score": 200},
{"field": "customer.account_age_days", "operator": ">=", "value": 30, "score": 50}]}

Payload :
{"transaction": {"amount": 500000, "country": "Sanctioned"}, 
"customer": {"account_age_days": 29}}

Output :
{'decision': 'FAIL',
 'score': 100,
 'trace': ['Rule 1: transaction.amount <= 500000 (+100)',
  'Rule 2: transaction.country != sanctioned',
  'Rule 3: customer.account_age_days >= 30']}

Assumptions :
High-value transactions (>$500k) → High-risk
2️.“Sanctioned” countries/flags → Regulatory denial
3️.New accounts (<30 days old) → Increased fraud probability
4️.Missing data → Treated as risk → Score not awarded
5️.System is auditable → Full trace + log records retained
HOW TO RUN :-
1.Install dependencies : 
pip install -r requirements.txt
pip install pytest
2.Run the main script :
python main.py   
[This will load rules from 'src/decision_engine/rules.json', evaluate them against a sample payload, and print the result].
3.Sample code :
from decision_engine.rule_evaluator import load_rules_from_json, evaluate_rules 
payload = {"transaction": {"amount": 200000, "country": "USA"}, 
"customer": {"account_age_days": 90}}
rules = load_rules_from_json("src/decision_engine/rules.json") 
result = evaluate_rules(rules, payload) 
print(result)
4.Running automated tests :
pytest
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
SAMPLE INPUTS & OUTPUTS :-
1st Test Case --
Rules :
{"pass_score": 200, "rules": [{"field": "transaction.amount", "operator": "<=", "value": 500000, "score": 100}, 
{"field": "transaction.country", "operator": "!=", "value": "sanctioned", "score": 200}, 
{"field": "customer.account_age_days", "operator": ">=", "value": 30, "score": 50}]}

Payload :
{"transaction": {"amount": 200000, "country": "USA"}, "customer": {"account_age_days": 90}}

Output :
{'decision': 'PASS',
 'score': 350,
 'trace': ['Rule 1: transaction.amount <= 500000 (+100)',
  'Rule 2: transaction.country != sanctioned (+200)',
  'Rule 3: customer.account_age_days >= 30 (+50)']}

2nd Test Case --
Rules :
{"pass_score": 200, "rules": [{"field": "transaction.amount", "operator": "<=", "value": 500000, "score": 100},
{"field": "transaction.country", "operator": "!=", "value": "sanctioned", "score": 200},
{"field": "customer.account_age_days", "operator": ">=", "value": 30, "score": 50}]}

Payload :
{"transaction": {"amount": 500000, "country": "Sanctioned"}, 
"customer": {"account_age_days": 29}}

Output :
{'decision': 'FAIL',
 'score': 100,
 'trace': ['Rule 1: transaction.amount <= 500000 (+100)',
  'Rule 2: transaction.country != sanctioned',
  'Rule 3: customer.account_age_days >= 30']}

Assumptions :
High-value transactions (>$500k) → High-risk
2️.“Sanctioned” countries/flags → Regulatory denial
3️.New accounts (<30 days old) → Increased fraud probability
4️.Missing data → Treated as risk → Score not awarded
5️.System is auditable → Full trace + log records retained
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
USAGES OF SAMPLE TEST CASES :-
1.Fraud detection.
2.AML (Anti-Money Laundering) compliance checks.
3.Digital banking risk scoring.
