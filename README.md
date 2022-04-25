# Recreation of Results from *Applying interpretable deep learning models to identify chronic cough patients using EHR data*

## Code requires AWS access

### To recreate, run in this order

1. create_cohort.ipynb
2. feat_eng.ipynb
3. sklearn_models.ipynb
4. BertTraining.ipynb


To recreate these csv's in Athena...

adm = pd.read_csv('s3://athena-output-mimic/admissions/2022/04/01/0d87fbd0-5127-43aa-a6a9-74535f99b093.csv')
pats = pd.read_csv('s3://athena-output-mimic/patient_age/2022/04/01/af54b7c1-5c76-412e-adcc-54bf649d92fe.csv')
notes = pd.read_csv('s3://athena-output-mimic/noteevents/2022/04/01/a4ec2a40-3dd5-42fa-b6da-b22f3dc96880.csv')
scripts = pd.read_csv('s3://athena-output-mimic/prescriptions/2022/04/01/a1ef6eec-e3b0-47d0-be6d-42616c00547f.csv')

adm: select subject_id, hadm_id, admittime from admissions

pats: select subject_id, dob, dod from patients

notes: select subject_id, hadm_id, chartdate, text from noteevents
where
    text like '%cough%' or
    text like '%Cough%' or
    text like '%Coughed%' or
    text like '%coughed%' or
    text like '%Coughing%' or
    text like '%coughing%' or
    text like '%Coughs%' or
    text like '%coughs%' or
    text like '%Expectorate%' or
    text like '%expectorate%' or
    text like '%Expectorated%' or
    text like '%expectorated%' or
    text like '%Expectorates%' or
    text like '%expectorates%' or
    text like '%Expectorating%' or
    text like '%expectorating%' or
    text like '%Expectoration%' or
    text like '%expectoration%'
    
scripts: select subject_id, hadm_id, startdate, enddate, drug from prescriptions
where drug like '%Dextromethorphan%' or drug like '%dextromethorphan%'
