Task 1 — Classify and Handle PII Fields:

Direct PII :
  1.full_name        -Direct PII    -Drop          -Name can identify a person and reveals it.
  2.email            -Direct PII    -Drop          -Email can be transparent so drop that.
  3.date_of_birth    -Indirect PII  -Mask          -When it is combine with other attributes it may reveals the person.
  4.zip_code         -Indirect PII  -Mask          -It may provide the area of location if it combined with others.
  5.job_title        -Indirect PII  -Mask          -We can also categorize because instead of uniques.
  6.diagnosis_notes  -Indirect PII  -Pseudonymize  -It is a sensitive data so we can hash the values.

  Task 2 — Audit the API Script for Ethical Compliance

  1.Never hardcode your api_keys:Because it can used by all and it is easily exposed.

  import os 
  import requests
  API_URL = "https://healthstats-api.example.com/records"
  API_KEY = os.getenv("api_key")
  records = []
  for page in range(1, 101):
      response = requests.get(API_URL, params={"page": page, "key": API_KEY})
      data = response.json()
      records.extend(data["results"])
  save_to_database(records)

  2.It reterieves many data in a sec: so many requests without limits so it may violates the api terms and conditions.

  import os 
  import requests
  import time
  API_URL = "https://healthstats-api.example.com/records"
  API_KEY = os.getenv("api_key")
  records=[]
  page=1
  while True:
    response = requests.get(API_URL, params={"page": page, "key": API_KEY})
    if response.status_code==429:
      time.sleep(5)
      continue
    if response.status_code!=200:
      break
    data = response.json()
    if not data["resuts"]:
      break
    records+=data["results"]
    page+=1
  save_to_database(records)
