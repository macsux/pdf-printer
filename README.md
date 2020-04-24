# PDF Printer

Contents of this folder is obtained from original repo: https://github.com/fritsvt/puppeteer-html-to-pdf-converter  

License:  [MIT](LICENSE)


## Deploying to PCF

The project has been modified to run on PCF

(1) Copy the contents of [config-example.json](config-example.json) and place them in a new config.json
```
cp config-example.json config.json 
```
(2) Fill in the config.json with your configuration

config.json
```
{
    "PORT": 8080,
    "BASE_URL": "https://pdf-printer.cfapps.io",
    "EXPIRES_IN": 900,
    "EXPIRE_CHECK_INTERVAL": 10
}
```

(3) Deploy the app using CF CLI

```
cf push -f manifest.yml
```

 


## Run Locally

Aside from the option to deploy this project on Heroku the instruction below is meant for either a local or standalone setup.

 (1) Install the npm dependencies
```
npm install
```
 (2) Copy the contents of [config-example.json](config-example.json) and place them in a new config.json
```
cp config-example.json config.json
```
 (3) Fill in the config.json with your configuration

config.json
```
{
    "PORT": 3000,
    "BASE_URL": "http://localhost:3000",
    "EXPIRES_IN": 900,
    "EXPIRE_CHECK_INTERVAL": 10
}
```

 (4) Run the app
```
npm run start
```




## Usage
### `POST /generate`

#### Parameters

The api doesn't care much how you send the parameters. Wether it's form-data form-urlencoded or as raw json. It's all welcome here.

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `url` | `string` | **Required if no html**. The url of the webpage to convert to pdf |
| `html` | `string` | **Required if no url**. The html to convert to pdf |
| `scale` | `string` | **Optional**. Scale of the webpage rendering. Defaults to 1. Scale amount must be between 0.1 and 2 |
| `displayHeaderFooter` | `boolean` | **Optional**. Display header and footer. Defaults to `false ` |
| `headerTemplate` | `string` | **Optional**. HTML template for the print header. Should be valid HTML markup with following classes used to inject printing values into them: `date`, `title`, `url`, `pageNumber`, `totalPages` |
| `footerTemplate` | `string` | **Optional**. HTML template for the print footer. Should use the same format as the `headerTemplate` |
| `printBackground` | `boolean` | **Optional**. Print background graphics. Defaults to `false` |
| `landscape` | `boolean` | **Optional**. Paper orientation. Defaults to `false` |
| `pageRanges` | `string` | **Optional**. Paper ranges to print, e.g., '1-5, 8, 11-13'. Defaults to the empty string, which means print all pages |
| `format` | `string` | **Optional**. Paper format. If set, takes priority over width or height options. Defaults to 'Letter' |
| `width` | `integer` | **Optional**. Paper width, accepts values labeled with units |
| `height` | `integer` | **Optional**. Paper height, accepts values labeled with units |
| `margin.top` | `integer` | **Optional**. Top margin, accepts values labeled with units |
| `margin.right` | `integer` | **Optional**. Right margin, accepts values labeled with units |
| `margin.bottom` | `integer` | **Optional**. Bottom margin, accepts values labeled with units |
| `margin.left` | `integer` | **Optional**. Left margin, accepts values labeled with units |
| `preferCSSPageSize` | `boolean` | **Optional**. Give any CSS `@page` size declared in the page priority over what is declared in `width` and `height` or `format` options. Defaults to `false`, which will scale the content to fit the paper size |

#### Example Request

```
{
    "url": "https://www.google.com",
    "printBackground": true
}
```

#### Response

If the request was succesful the response will look like this:
```
{
    "success": true,
    "url": "http://localhost:3000/exports/1578634803-coqc.pdf",
    "path": "/exports/1578634803-coqc.pdf",
    "expires": 1578635703
}
```

If one of the parameters was invalid the request will look something like:
```
{
    "success": true,
    "errors": {
        "errors": [
            {
                "msg": "Must provide either url or html",
                "param": "url_html",
                "location": "body"
            }
        ]
    }
}
```

  