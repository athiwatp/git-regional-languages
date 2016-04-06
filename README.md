# Git Regional Languages

More famous languages may be written everywhere 
in the world, yet the languages with lesser users 
may probably tend to be epidemic or popular 
in some particular regions. Let's find out.

---

## Getting started

Prerequisites:

- [x] Github API access token
- [x] GoogleMap API key
- [x] MongoDB
- [x] Python 3.4+
- [x] Node.js 4.x+


Since it uses [github API](https://developer.github.com/v3), 
you'll need to supply your access token. 
Create a text file named `API-TOKEN` in the root directory 
of this repository and put your Github login ID 
and your access token in the following line like so:

```
johndoeagain
f00ba478293cbbae0a6552
```

Also, [GoogleMap API key](https://developers.google.com/maps/documentation/geocoding/get-api-key) is also required for geocoding 
API access. You'll need to generate your own API key 
can place it in a text file named `GOOGLE-API-KEY`, 
put it in the root directory of the repo.

>Obtain your Google API key here: https://console.developers.google.com/project/_/apiui/credential. Also make sure you have **GoogleMap API** enabled for this credential.

### Prepare your MongoDB

To set your data storage, create a DB named `gitlang`. 
Don't need to create any initial collection because 
the script will do it for you.

>**CAVEAT** It really requires a huge capacity to 
store the data and you can make [up to 5000 requests](https://developer.github.com/v3/#rate-limiting) 
in a span of one hour.

---

## Download GitHub data

Run the following script to download a batch of repository data 
via Github API:

```
$ python3 download.py
```

**NOTE:** Adjust the number of batch, the range of repos to download 
right in the file `download.py` yourself. 
The script will download the batch repository data and 
store them in the MongoDB.

| DB/Collection | data storage |
|------------------|----------------|
| gitlang/repos | Github repositories data with the location of owners |


---

## Process the repositories

We have a script to process the entire bulk of downloaded repository 
data in MongoDB, generate the geospatial distribution of languages 
written in each repository. Execute the following script:

```
$ node process.js
```

>**Why Node?** 
>
> In case a question has popped up in your mind, 
> Node.js natively communicates with `MongoDB` and 
> JavaScript works well with `GoogleMAP API`. That's why.

### What process.js does?

Following tasks are sequentially run:

1. Maps repository data from the collection `repos` to distributions of languages by regions, store them in the collection `distLangByRegion` and list all regions in `regions`.

2. Fetches *Geolocations* of all regions of repos.

3. Saves all Geolocations to MongoDB: collection `geo`.

4. Aligns all languages available in `distLangByRegion` with the geospatial distribution data. Maps the output in to the JS file under `html\js\dist.js`.

5. Saves the **GoogleAPI Key** to `html/js/gapi.js`.

---















