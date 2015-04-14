---
layout: post
title: json.mjson
author: ilsec
---

##*json* 语法教程

在这个网站 [JSON 语法](http://www.json.org/) 上已经有详细教程，这里不再赘述。

##*mjson* 下载地址

[M's JSON parser.](http://sourceforge.net/projects/mjson/)

##*mjson* 读取教程

json例子：

`
{
    "country": [
        {
            "cn": "china",
            "hk": "xianggang"
        },
        {
            "tw": "taiwan",
            "us": "meiguo"
        }
    ]
}
`

1. 调用**json_stream_parse**获取*json*的root节点。
2. 调用**json_find_first_label**获取*json*的root节点上的名称。
3. 遍历带有array的具体代码如下

		void *test(){
		json_t *entry, *root, *head, *body, *label1, *label2, *value, *array, *next;
		root = NULL;
		FILE *fp = NULL;
		fp = fopen("country.json", "rb");
		if (JSON_OK == json_stream_parse(fp, &root)){
		head = json_find_first_label(root, "country");
		array = head->child;
		next = array->child;
		while (next){
			label1 = json_find_first_label(next, "cn");
			label2 = json_find_first_label(next, "hk");
			printf("%s-%s\n", label1->child->text, label2->child->text);
			next = next->next;
		}

		json_free_value(&root);
		}
		fclose(fp);
		}
