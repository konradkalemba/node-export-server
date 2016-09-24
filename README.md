# Highcharts Node.js Export Server

The export server can be ran as both a CLI converter, and as a HTTP server.

## Install
    
    npm install highcharts-export-server -g

OR:
    
    git clone https://github.com/highcharts/node-export-server
    npm install


## Running
    
    highcharts-export-server <arguments>

## Command Line Arguments
    
  * `--infile`: Specify the input file.
  * `--instr`: Specify the input as a string.
  * `--outfile`: Specify the output filename.
  * `--type`: The type of the exported file. Valid options are `jpg png pdf svg`.
  * `--scale`: The scale of the chart.
  * `--width`: Override the width of the chart.
  * `--constr`: The constructor to use. Either `Chart` or `StockChart`.
  * `--callback`: File containing JavaScript to call in the constructor of Highcharts.
  * `--resources`: Stringified JSON.
  * `--host`: The hostname to run a server on.
  * `--port`: The port to listen for incoming requests on.
  * `--tmpdir`: The path to temporary output files.

`-` and `--` can be used interchangeably when using the CLI.

## Injecting the Highcharts dependency

Todo. Using CDN right now.

## As a Node module

Todo.

## Server Test

Run the below in a terminal after running `highcharts-export-server --enableServer`.
    
    # Generate a chart and save it to mychart.png    
    curl -H "Content-Type: application/json" -X POST -d '{"infile":{"title": {"text": "Steep Chart"}, "xAxis": {"categories": ["Jan", "Feb", "Mar"]}, "series": [{"data": [29.9, 71.5, 106.4]}]}}' 127.0.0.1:7801 -o mychart.png

## Using as a Node.js Module

The export server can be included as a module:
    
    const exporter = require('highcharts-export-server');

    //Set up a pool of PhantomJS workers
    exporter.initPool();

    //Perform an export
    exporter.export(<export settings>, <chart>, function (err, res) {
        ...
        exporter.killPool();
    });

## Performance Notice

In cases of batch exports, it's faster to use the HTTP server than the CLI.
This is due to the overhead of starting PhantomJS for each job when using the CLI. 

As a concrete example, running the CLI with `testcharts/basic.json` averages about
449ms. Posting the same configuration to the HTTP server averages less than 100ms.

So it's better to write a bash script that starts the server and then
performs a set of POSTS to it through e.g. curl if not wanting to host the
export server as a service.

## License

[MIT](LICENSE).