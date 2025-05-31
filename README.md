# Wiki-Socks

A dataset of Wikipedia Sockpuppet Investigations collected for my masters thesis, *Detecting Sockpuppetry on Wikipedia using Meta-Learning*. Sockpuppets are accounts that are controlled by the same user, typically deployed to create the illusion of majority support for political or vandalistic purposes. Detecting sockpuppets is imperative to reducing the spread of misinformation and distrust in online communities.

The dataset was constructed using the English Wikipedia API, and contains the Wikipedia contributions of all confirmed sockpuppets listed [here](https://en.wikipedia.org/wiki/Category:Wikipedia_sockpuppets). It also contains randomly sampled contributions from non-sockpuppet users drawn from the same time and page distribution. If you are a non-sockpuppet user included in this dataset and would like your contributions removed, please submit a pull request or send me an email and I will be happy to remove them.

## Structure

The dataset is structured as a set of CSV files. Each file represents an individual sockpuppet investigation. Each row represents a contribution by a user to Wikipedia. A description of each feature and its data type is provided below:

| Feature   | Type      | Description                                                                   |
|-----------|-----------|-------------------------------------------------------------------------------|
| timestamp | Timestamp | The Coordinated Universal Time (UTC) of the contribution in ISO 8601 format.  |
| revid     | Int       | The revision id representing the contribution.                                |
| parentid  | Int       | The revision id representing the parent (preceding) contribution.             |
| sock      | Int       | A boolean value representing whether the user is a sockpuppet (1) or not (0). |
| user      | String    | The username of the user making the contribution as plain text.               |
| page      | String    | The Wikipedia page to which the contribution was made as plain text.          |
| message   | String    | The description of the contribution provided by the user as plain text.       |

## Empty Investigations

Several investigations that were scrapped contained no sockpuppet contributions. Either the banned sockpuppets made none, or these contributions no longer appear on their user contributions page. A list of these 984 investigations is included in `empty-investigations.txt`.

## Problematic Investigations

Please note that some investigations may have a limited number of positive or negative samples, or empty fields. This may be problematic for some learning architecture. We provide a `stats.csv` file containing information about each investigation, and suggest that this file be used to create a suitable distribution of tasks for a given architecture.

## Reading the Data

Each investigation is easily read using pandas. To ensure the data is read to the desired datatype, you may use the function below.

```python
import pandas as pd

def read_investigation(fn):

    data = pd.read_csv(
        fn, 
        dtype={
            'page': str, 
            'message': str
        }, 
        lineterminator='\n', 
        parse_dates=['timestamp'], 
        date_format="%Y-%m-%dT%H:%M:%S%z"
    )

    data['message'] = data['message'].fillna('')
    data['page'] = data['page'].fillna('')
    
    return data
```
