## Data from scratch(1): Building a Barebones Data Ingestion Package

I am still setting up this blog, so I will be writing about some of the things I am learning as I go along.
---

### Do we need another data ingestion package?

Apart from filling up yet another name in the Pypi repository, I don't think *miniduct* is going to accomplish much. In fact, i am sure that there are countless packages that do exactly what miniduct does. And surely better. and in a very elegant way. In fact, i could just run an instance of Airflow or Dagster, and i'd be done with it, having at my disposal a plethora of features that any data enthusiast in a corporate environment loves to have, like:

- Scheduling
- Monitoring
- Logging
- Error handling
- Backfilling
- parallel execution

the list is huge. Quite huge in fact, that for a new data engineer it can be quite overwhelming. Why do we need all these features anyway? what pushed engineers to build such complex systems?

#### Starting simple

Engineers build on top of the work of others, and we compose systems that solve very tangible problems that a lot of people had in their careers. 

I have 'designing data intensive applications' by Martin Kleppmann in my libraty, and i ofter get back to reading it. whenever i learn about some old or new technology, i can find the roots of their implementation in the concepts described in the book. I love the approach that the book has in starting from simple systems, like a data storage build on a few bash commands, and then explains what additional requirements shape the miriad of implementations of data systems that we have today.

So miniduct, and other packages that i will be building, will start from humble origins, and will expand o

### What is miniduct?

Let's start from this problem:

```
We need a way to ingest data from an API, and save it to a folder in the host filesystem.
```

in python, we can to this with a few lines of code:

```python
import requests
import os

def fetch_data(api_url, save_path):
    response = requests.get(api_url)
    response.raise_for_status()  # Ensure we raise an error for bad responses
    with open(save_path, 'w') as file:
        file.write(response.text)

```

now every time another developer needs data from the API, they can just call this function, and the data will be saved to the specified path.

```
fetch_data('https://api.cats.com/names', '/workdir/cats/names.txt')
```

