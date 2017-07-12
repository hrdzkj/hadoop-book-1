
# Hadoop Book Example Code

This repository contains the example code for [Hadoop: The Definitive Guide, Fourth Edition](http://shop.oreilly.com/product/0636920033448.do)
by Tom White (O'Reilly, 2014).

Code for the [First], [Second], and [Third] Editions is also available.

Note that the chapter names and numbering has changed between editions, see
[Chapter Numbers By Edition](https://github.com/tomwhite/hadoop-book/wiki/Chapter-Numbers-By-Edition).

[First]: http://github.com/tomwhite/hadoop-book/tree/1e
[Second]: http://github.com/tomwhite/hadoop-book/tree/2e
[Third]: http://github.com/tomwhite/hadoop-book/tree/3e

# 气象数据下载
老师提供的ftp://ftp3.ncdc.noaa.gov/pub/data/noaa已经转到ftp://ftp.ncdc.noaa.gov/pub/data/noaa了
可以通过wget或者ftp方式下载
wget方式：wget -r -c ftp://ftp.ncdc.noaa.gov/pub/data/noaa/2008
github上也有一些： https://github.com/tomwhite/hadoop-book/tree/master/input/ncdc/all.


## Building and Running

To build the code, you will first need to have installed Maven and Java. Then type

```bash
% mvn package -DskipTests
```

This will do a full build and create example JAR files in the top-level directory (e.g. 
`hadoop-examples.jar`).

To run the examples from a particular chapter, first install the component 
needed for the chapter (e.g. Hadoop, Pig, Hive, etc), then run the command lines shown 
in the chapter.

Sample datasets are provided in the [input](input) directory, but the full weather dataset
is not contained there due to size restrictions. You can find information about how to obtain 
the full weather dataset on the book's website at [http://www.hadoopbook.com/]
(http://www.hadoopbook.com/).

## Hadoop Component Versions

This edition of the book works with Hadoop 2. It has not been tested extensively with 
Hadoop 1, although most of it should work.

For the precise versions of each component that the code has been tested with, see 
[book/pom.xml](book/pom.xml).

 **Chapter names**

