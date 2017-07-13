
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

hadoop的序列化办法好像更加高效。

 **Chapter names:**
*   ch03 - The Hadoop Distributed Filesystem    
    FileSystem用法: get,create,listStatus  
    IOUtils: copyBytes  
*   ch04 - Hadoop I/O  
    CompressionCodecFactory: removeSuffix  getCodec   
    CompressionCodec: getDefaultExtension  createInputStream和createOutputStream是两个互为逆运算。   
    逆运算：A的任意两个元素a和b，通过所给的运算，可以得到一个结果c．反过来，如果已知元素c，以及元素a，b中的一个，按照某种法则，可以得到另一个元素，这样的法则也定义了一种运算，这样的运算叫做原来运算的逆运算  
    重建索引(MapFileFixer.java)：MapFile.fix(fs, map, keyClass, valueClass, false, conf)
    创建MapFile(MapFileWriteDemo.java)：MapFile.Writer writer = new MapFile.Writer(conf, fs, uri,key.getClass(), value.getClass());  
                 writer.append(key, value);   
                 IOUtils.closeStream(writer);  
    压缩输出(MaxTemperatureWithCompression.java)：FileOutputFormat.setCompressOutput(job, true);
             FileOutputFormat.setOutputCompressorClass(job, GzipCodec.class);    
             或者（MaxTemperatureWithMapOutputCompression.java）：可以更改mapred.output.compress为true，mapped.output.compression.codec为想要使用的codec的类名就可以了   
             Configuration conf = new Configuration();   
             conf.setBoolean("mapred.compress.map.output", true);   
             conf.setClass("mapred.map.output.compression.codec",GzipCodec.class, CompressionCodec.class);   
             Job job=new Job(conf);   
  CodecPool(PooledStreamCompressor.java): 有点象连接迟，这样你就无需频繁的去创建codec对象.     
  Compressor compressor = CodecPool.getCompressor(ReflectionUtils.newInstance(codecClass, conf));
  ......  
  CodecPool.returnCompressor(compressor);
  对比：读写MapFile和SequenceFile文件：MapFileWriteDemo.java和SequenceFileReadDemo.java SequenceFileWriteDemo.java  
  读取SequenceFile（SequenceFileReadDemo.java）   ：     
  SequenceFile.Reader reader = new SequenceFile.Reader(fs, path, conf);   
  Writable key = (Writable) ReflectionUtils.newInstance(reader.getKeyClass(), conf);     
  Writable value = (Writable)ReflectionUtils.newInstance(reader.getValueClass(), conf);    
  long position = reader.getPosition();    
  reader.next(key, value)；
  IOUtils.closeStream(reader);      
  写入SequenceFile（SequenceFileWriteDemo.java）：     
  SequenceFile.Writer  writer = SequenceFile.createWriter(fs, conf, path,key.getClass(), value.getClass());
  writer.append(key, value);
  IOUtils.closeStream(writer);   
  CompressionCodec封装一个流（StreamCompressor.java）：      
  CompressionCodec codec = (CompressionCodec)ReflectionUtils.newInstance(codecClass, conf);    
  CompressionOutputStream out = codec.createOutputStream(System.out);
  IOUtils.copyBytes(System.in, out, 4096, false);
   out.finish();
