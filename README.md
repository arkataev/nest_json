# Json grouper
Group your json list of dictionaries on values defined by arbitraty number of keys, 
ignoring unknown keys.


## CLI usage
```bash
cat import.json | python nest.py arg_1 arg_2 ...
# or 
python nest.py import.json arg_1 arg_2 ...
```

## API usage
```bash
cd /path/to/nest_json
docker-compose up
curl -d @input.json -X POST http://localhost:5000/api/v1/nestify?group_by=currency,city,country -H "Content-Type: application/json" --user admin:admin
```

## Test
```bash
cd /path/to/nest_json
docker-compose -f docker-compose-test.yml run nest_app
```

### Example

INPUT 

````json
[
  {
    "country": "US",
    "city": "Boston",
    "currency": "USD",
    "amount": 100
  },
  {
    "country": "FR",
    "city": "Paris",
    "currency": "EUR",
    "amount": 20
  },
  {
    "country": "FR",
    "city": "Lyon",
    "currency": "EUR",
    "amount": 11.4
  },
  {
    "country": "ES",
    "city": "Madrid",
    "currency": "EUR",
    "amount": 8.9
  },
  {
    "country": "UK",
    "city": "London",
    "currency": "GBP",
    "amount": 12.2
  },
  {
    "country": "UK",
    "city": "London",
    "currency": "FBP",
    "amount": 10.9
  }
]
````

OUTPUT
```json
{
  "EUR": {
    "ES": {
      "Madrid": [
        {
          "amount": 8.9
        }
      ]
    },
    "FR": {
      "Lyon": [
        {
          "amount": 11.4
        }
      ],
      "Paris": [
        {
          "amount": 20
        }
      ]
    }
  },
  "FBP": {
    "UK": {
      "London": [
        {
          "amount": 10.9
        }
      ]
    }
  },
  "GBP": {
    "UK": {
      "London": [
        {
          "amount": 12.2
        }
      ]
    }
  },
  "USD": {
    "US": {
      "Boston": [
        {
          "amount": 100
        }
      ]
    }
  }
}
```