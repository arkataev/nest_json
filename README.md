# Task

1. Given an input as json array (each element is a flat dictionary) write a program that will parse this json, and return a nested dictionary of dictionaries of arrays, with keys specified in command line arguments and the leaf values as arrays of flat dictionaries matching appropriate groups

```python nest.py nesting_level_1 nesting_level_2 … nesting_level_n```

Example input for json can be found here:  http://jsonformatter.org/74f158

For example, when invoked like this:

```cat input.json | python nest.py currency country city```

the output should be in json format and look like this: http://jsonformatter.org/615048

The program should support an arbitrary number of arguments, that is arbitrary levels of nesting.

2. Create a REST service from the first task. Make sure your methods support basic auth. Input json should be received via POST request, nesting parameters should be specified as request parameters.


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