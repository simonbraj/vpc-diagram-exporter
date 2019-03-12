# Generates a diagram of IBM Cloud VPC resources

## Prerequisites

To use the tool and deploy the VPC examples, you will need:

* Python 2.7.1
* pip
* [graphviz](https://www.graphviz.org/) (`brew install graphviz`) on a mac
* *infrastructure-service* plugin for *ibmcloud* (`ibmcloud plugin install infrastructure-service`)

If you want to save yourself some time in the future, use [my IBM Cloud CLI docker image](https://github.com/l2fprod/bxshell) ;)

## Before you begin

The tool is written in Python with a small set of helpers to wrap the `ibmcloud` CLI so you can benefit from Python language features to interact with the VPC API.

1. Install Python requirements:

   ```sh
   pip install -r requirements.txt
   ```

1. Make sure your VPC environment is correctly configured:

   ```sh
   ibmcloud is vpcs
   ```

## A tool to generate a graph of your VPCs

### First, export all VPC resources into one big JSON file

`dump.py` exports all VPC resources into a big JSON files. It calls the *ibmcloud* command and requires the *is* plugin. If *ibmcloud is vpcs* works in your environment, the script should work too.

   ```sh
   ./dump.py
   ```

The script runs a few commands and produces `output/all.json`

### Then convert the JSON into GraphViz

`json2gv.py` produces a [Graphviz](https://www.graphviz.org/) diagram of the elements in `output/all.json`. It uses a Jinja2 template to convert the JSON exported earlier into one Graphviz file per VPC (*vpcname.gv*) under the `output` folder.

To get the Graphviz input:

   ```sh
   ./json2gv.py
   ```

### And finally images!

To generate a PNG for all VPCs (all graphvizs in the folder):

   ```sh
   find output -name '*.gv' -exec dot {} -Tpng -o{}.png \;
   ```

Or to generate a PNG for a specific VPC:

   ```sh
   dot vpcname.gv -Tpng -ovpcname.png
   ```

### One liner

**PNG**

   ```sh
   ./dump.py && rm -f output/*.gv output/*.gv.png && ./json2gv.py && find output -name '*.gv' -exec dot {} -Tpng -o{}.png \;
   ```

**SVG**

   ```sh
   ./dump.py && rm -f output/*.gv output/*.gv.svg && ./json2gv.py && find output -name '*.gv' -exec dot {} -Tsvg -o{}.svg \;
   ```
## License

This project is licensed under the Apache License Version 2.0 (http://www.apache.org/licenses/LICENSE-2.0).