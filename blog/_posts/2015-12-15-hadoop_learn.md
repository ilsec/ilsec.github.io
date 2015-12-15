---
layout: post
title: hadoop learn
author: ilsec
---

# WordCount

## code [WordCount.java]

```
package cn.illa.wordcount;

import java.io.IOException;
import java.util.*;

import org.apache.hadoop.fs.Path;
import org.apache.hadoop.conf.*;
import org.apache.hadoop.io.*;
import org.apache.hadoop.mapreduce.*;
import org.apache.hadoop.mapreduce.lib.input.*;
import org.apache.hadoop.mapreduce.lib.output.*;
import org.apache.hadoop.util.*;

public class WordCount extends Configured implements Tool {

  public static class Map extends Mapper<LongWritable, Text, Text, IntWritable> {
    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();
    public void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {
      String line = value.toString();
      StringTokenizer tokenizer = new StringTokenizer(line);
      while (tokenizer.hasMoreTokens()) {
        word.set(tokenizer.nextToken());
        context.write(word, one);
      }
    }
  }

  public static class Reduce extends Reducer<Text, IntWritable, Text, IntWritable> {
    public void reduce(Text key, Iterable<IntWritable> values, Context context) throws IOException, InterruptedException {
      int sum = 0;
      for (IntWritable val : values) {
        sum += val.get();
      }
      context.write(key, new IntWritable(sum));
    }
  }

  public int run(String [] args) throws Exception {
    Job job = new Job(getConf());
    job.setJarByClass(WordCount.class);
    job.setJobName("wordcount");

    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);

    job.setMapperClass(Map.class);
    job.setReducerClass(Reduce.class);

    job.setInputFormatClass(TextInputFormat.class);
    job.setOutputFormatClass(TextOutputFormat.class);

    FileInputFormat.setInputPaths(job, new Path(args[0]));
    FileOutputFormat.setOutputPath(job, new Path(args[1]));

    boolean success = job.waitForCompletion(true);

    return success ? 0 : 1;
  }

  public static void main (String[] args) throws Exception {
    int ret = ToolRunner.run(new WordCount(), args);
    System.exit(ret);
  }

}

```

# 编译环境配置
将三个依赖包的路径添加到**CLASSPATH**环境变量里:

```
$HADOOP_HOME$/share/hadoop/common/hadoop-common-x.x.x.jar
$HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-client-core-x.x.x.jar
$HADOOP_HOME/share/hadoop/common/lib/commons-cli-1.2.jar
```
如下图:

![img](https://github.com/ilsec/images/blob/master/2015-12-15-hadoop_learn_01.png?raw=true)

然后就可以生成jar包:

```
javac -d output WordCount.java
jar -cvf wordcount.jar -C output/ .
```

上传文件:

```
hdfs dfs -mkdir input // 创建input目录
hdfs dfs -rm -r output // 删除output目录，不删除报错。

hdfs dfs -put input/file0* input //第一个input是本地目录，第二个input是hdfs目录
```

执行hadoop命令

```
hadoop jar wordcount.jar cn.illa.wordcount.WordCount input output
```

查看输出

```
hdfs dfs -get output/part-r-00000 ./
cat part-r-00000
```
