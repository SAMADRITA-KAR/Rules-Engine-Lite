# RULES ENGINE LITE

## Project Overview :-
Deterministic rule-based decision engine to evaluate a JSON payload against a JSON ruleset and return a decision, a score, and an explainability trace.
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Features :-
1.Load rules from JSON files.
2.Support for >, <, >=, <=, ==, != operators.
3.Nested field lookup (e.g., transaction.amount).
4.Case-insensitive string comparisons.
5.PASS/REVIEW/FAIL decisions with scores and explainable traces.
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Project Structure :-
rules-engine-lite.zip
└── rules-engine-lite/
    ├── README.md
    ├── requirements.txt
    └── src/
        ├── decision_engine/
        │   ├── rule_evaluator.py
        │   ├── cli.py
        │   └── __init__.py
        ├── rules_eval/
        │   └── __main__.py
        ├── data/
        │   ├── rules_pass.json
        │   ├── rules_fail.json
        │   ├── payload_pass.json
        │   ├── payload_fail.json
        │   └── payload_tricky.json
        └── tests/
            ├── test_engine.py
            └── __init__.py
    └── pytest.ini 
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Approach :-
1.The Rules Engine evaluates financial transactions using deterministic rule-based scoring. 
2.Each rule contributes positively or negatively to a risk score. 
3.Total score determines the final decision tier (Approve/Review/Deny). 
4.Operators support nested attributes and case-insensitive matching to handle varied data sources.
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## How to run :-
1.Extract the zip file and run cd into the extracted folder.
2.Install dependencies :
pip install -r requirements.txt
pip install pytest 
3.Run the CLI using the packaged module :
python -m rules_eval --rules src/data/rules_pass.json --payload src/data/payload_pass.json
4.Run the tests :
pytest
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Sample Test Cases :-
1st Test Case -- 
Rules : {"pass_score": 200, 
"rules": [{"field": "transaction.amount", "operator": "<=", "value": 500000, "score": 100}, 
{"field": "transaction.country", "operator": "!=", "value": "sanctioned", "score": 200}, 
{"field": "customer.account_age_days", "operator": ">=", "value": 30, "score": 50}]
} 
Payload : {"transaction": {"amount": 200000, "country": "USA"}, "customer": {"account_age_days": 90}} 
Output : {'decision': 'PASS', 
'score': 350, 
'trace': ['Rule 1: transaction.amount <= 500000 (+100)', 'Rule 2: transaction.country != sanctioned (+200)', 'Rule 3: customer.account_age_days >= 30 (+50)']
} 

2nd Test Case --
Rules : {"pass_score": 200, 
"rules": [{"field": "transaction.amount", "operator": "<=", "value": 500000, "score": 100}, 
{"field": "transaction.country", "operator": "!=", "value": "sanctioned", "score": 200}, 
{"field": "customer.account_age_days", "operator": ">=", "value": 30, "score": 50}]
} 
Payload : {"transaction": {"amount": 500000, "country": "Sanctioned"}, "customer": {"account_age_days": 29}} 
Output : {'decision': 'FAIL', 
'score': 100, 
'trace': ['Rule 1: transaction.amount <= 500000 (+100)', 'Rule 2: transaction.country != sanctioned', 'Rule 3: customer.account_age_days >= 30']
}
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Assumptions :-
1.High-value transactions(>$500k) -> High-risk 
2."Sanctioned" countries/flags -> Regulatory denial 
3.New accounts(<30 days old) -> Increased fraud probability 
4.Missing data -> Treated as risk -> Score not awarded 
5.System is auditable -> Full trace + log records retained
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Usages Of Sample Test Cases :- 
1.Fraud detection. 
2.AML(Anti-Money Laundering) compliance checks. 
3.Digital banking risk scoring.
