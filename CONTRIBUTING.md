# Contributing Guidelines

## Working with Data

All raw data is in the `data/` directory.
Web pages are in `public/`.
If you want to add a new visualization, make a completely self-contained HTML file in `public/`.
Copy the data into a script block.
Put all your CSS and JS on the page.
If you want to use images, base64 encode them and put them inline.
If you want a third party JS library, copy it in or download it from a CDN.

Please make sure the HTML page has a useful `<title>` and an attribution link to your GitHub profile.
Also, append a link to your page on `index.html`.

Yes, there will be duplication; it's OK.
The main goal is to make it easy to contribute.
Code quality isn't as important as the data or their visualizations.

## Running it

Serve the `public/`.
If you have Docker installed, this should work.

```bash
docker run --rm -it -v "$PWD"/public:/usr/share/nginx/html -p 8080:80 nginx
```

Then, navigate to `http://localhost:8080/yourpage.html`.

## Data is missing

Since there are a lot of different ways to slice the data in [the survey](https://goo.gl/forms/rLTJhUM2bGXllMf93), I couldn't include them all.
If there's a combination you'd likt to see, open an issue and we can discuss it there.

Bonus points to whomever gives me [csvq](https://github.com/mithrandie/csvq) incantations (or some other CLI) to build the JSON.
Below is how I built `salaries_by_remote.json`.

```bash
csvq -d "\t" -f json 'select raw_salary - raw_salary % 10000 as salary, (case when raw_remote = "Yes" then TRUE else FALSE end) as remote from (select `What is your annual salary (not including bonuses or stock)?` as raw_salary, `Are you a remote worker?` as raw_remote from responses) order by salary' > data/salaries_by_remote.json
```
