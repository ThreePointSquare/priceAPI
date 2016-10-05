# priceAPI

## Description

A python API to retrieve and search prices of construction materials and services. On startup, it builds several default prices sources from the files found in the data folder. Each of these sources can be easily rebuilt when updated data becomes available online.

Data sources contain a couple of generic fields such as Name, Month, Year, Currency, an URL to download price data when rebuilding, and an URL to check for updated contents. Price data itself is in the form of a table with 4 columns: Code (not very useful, each source uses its own coding system), description, price and unit.

New data sources can be added by instantiating the base source class, and writing a new build() method.

## Current sources:

* PMSP (Prefeitura Municipal de São Paulo)
* FDE-SP (Secretaria da Educação do Estado de São Paulo)
* SINAPI-SP (Sistema de pesquisa de preços unitários da Caixa Federal - Estado de São Paulo)
* SEINFRA-CE (Secretaria de Infraestrutura do Estado do Ceará)

## Installation

The following python modules must be present to rebuild the sources. They are not needed to simply run a search:

* csv
* urllib2
* xlrd
* openpyxl

## Command line usage

Usage: priceapi.py \[OPTIONS\] searchterm1|alternativeterm1 searchterm2 ...

Separate search term by a space to retrieve only entries that contain all
the search terms. Use a | character to separate alternative search term
(entries containing one OR the other will be retrieved).

**Options:** 

`--location=XXX`: Specify a city or country name to limit the search to.

`--source=XXX`: Specify a source name or comma-separated list of source names to limit the search to.
                         
`--code=XXX`: Searches for a specific code. Dots and hyphens are ignored.

`--cub=XXXX` : Converts prices from the CUB value stored in the source to the given CUB value

Example: `priceapi.py --source=PMSP alvenaria 14cm`

The API can also be used from python:

```
import priceapi
priceapi.search("alvenaria 14cm")
```

Which will display a table like this:

```
Origin    Code      Description                                                             Price     Unit

FDE-SP    04.01.031 ALVENARIA DE BLOCOS DE CONCRETO E=14CM                                  73.99     M2

SINAPI-SP 87449     ALVENARIA DE VEDAÇÃO DE BLOCOS VAZADOS DE CONCRETO DE 14X19X39CM (ESPE  57.62     M2
                    SSURA 14CM) DE PAREDES COM ÁREA LÍQUIDA MENOR QUE 6M² SEM VÃOS E ARGAM
                    ASSA DE ASSENTAMENTO COM PREPARO EM BETONEIRA. AF_06/2014
...
```

To search for several terms, just use a space between them.

## Web interface

The webprice.py provides a web interface for the priceAPI. The functionality is about the same as the command line
interface. You can search for terms, and optionally specify location and source. See a working example 
[here](http://www.uncreated.net/priceapi/webapi.py).

To use the web interface, you need a web server that supports running python files. Simply copy priceapi.py,
webprice.py and the data folder to a location on your web server, and point your browser to webprice.py.

You might need to change the permission of the two .py files, make sure they are both executable by all users.

If there are files named favicon.ico and stylesheet.css in the same folder as webprice.py, they will be loaded
automatically.

## To Do

* ~~Allow to search alternative terms (term1 OR term2)~~
* ~~Allow to search by location~~
* ~~Allow to search by code~~
* ~~Add computer-readable output~~
* ~~Allow to use the script from the command line~~
* ~~Allow to convert/update prices by using an index value like CUB in Brazil~~
* Add more sources (outside Brazil if possible)
* ~~Build a GUI?~~
* Parse the SINAPI pdfs (currently using an xlsx from a website where a kind soul provides a conversion)
