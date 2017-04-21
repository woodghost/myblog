---
layout: post
title: json 解析练习
date: 2017-04-13 16:43:24.000000000 +09:00
tags: javascript
---


将下面一段json解析成另一种格式

original json
```
{
  "data": {
    "_id": "5833b999fe85cdb80db8516f",
    "desc": "",
    "schemata": [
      {
        "child": [
          {
            "title": "http://d1h06o39vyn1mi.cloudfront.net/product/upload/40/aa/40aada9316033c487acef159c6b462bd",
            "name": "1",
            "id": "e61e0c98-a306-4488-c56c-9afe5979a0e2"
          },
          {
            "title": "http://d1h06o39vyn1mi.cloudfront.net/product/upload/0e/ee/0eeeca55d41e350dbbb97a849433c473",
            "name": "2",
            "id": "01aa3afd-9b2a-f736-3eb9-e4bfc352f9d6"
          },
          {
            "title": "http://d1h06o39vyn1mi.cloudfront.net/product/upload/6a/a9/6aa95216eee350b0dcb4f46f89bb0167",
            "name": "3",
            "id": "ae6548b0-e972-b1af-6d4d-f1f9ec81aea3"
          },
          {
            "title": "http://d1h06o39vyn1mi.cloudfront.net/product/upload/fb/c5/fbc5d1a3f1ac971dc3dfd15f457394b7",
            "name": "4",
            "id": "d7e3e864-def6-36b4-b44d-e9e7274116be"
          },
          {
            "title": "http://d1h06o39vyn1mi.cloudfront.net/product/upload/70/20/7020e5a0b425670c8f888010df6fe16c",
            "name": "5",
            "id": "55bb37a2-1e95-b924-5a68-498687494d4d"
          }
        ],
        "required": true,
        "title": "",
        "name": "slider",
        "id": "bbb54f72-d663-7284-92ac-ce2599430825"
      },
      {
        "id": "6e0f1aef-2c80-176b-6daf-4c15e0124273",
        "name": "desc",
        "title": "Layer your lace top with a boyfriend blazer to add in coolness in the outfit.",
        "required": true,
        "child": [
          {
            "name": "slider_key",
            "title": "1",
            "id": "379d20cb-d3d1-32bd-4c74-f9450492fde6"
          }
        ]
      }
    ],
    "title": "How to Pull Off a Classy Outfit with a Lace Top",
    "sid": "Ekgq2uihbz"
  },
  "code": 1,
  "msg": "success"
}
```
> 该json是将数据存入 DDMS 的 form后得到的格式。详见http://ddms.mozat.com/apidocs.html

这段json主要是为了渲染一个上半部分为可滑动的slider，每一张图对应一个description和若干件衣服数据，数据存储然后导出的格式如上，为了符合页面渲染规则需要解析json使每一张大图和其description以及衣服一一对应，成为一个个新object

解析为：

```
{
  "ret": 0,
  "msg": "OK",
  "data": [
    {
      "id": "689035",
      "image": "http://d1h06o39vyn1mi.cloudfront.net/product/street_style/00/c3/00c3eec3f8e5ca952efba4d3bf8d535fd857ad4c_001.jpg",
      "desc":"You should also be mindful of hiding body",
      "clothes": [
        {
          "id": "1",
          "name": "Stripe Sateen Midi Skirt Co-ord",
          "image": "http://office.mozat.com:8081/storage//product/detail/7c/4b/132a27d9b2d082ab068ee7509f3f25c2_new_white.jpg",
        },
        {
          "id": "2",
          "name": "Kirsten skirted hem blazer",
          "image": "http://office.mozat.com:8081/storage//product/detail/86/af/86af64d61c43a2315c97954a5715f8a4_new_white.jpg",

        }]
    }
}
```

代码如下:

```
//parse objects in Array shcemata
function parseSchemata(meta){
    var obj = {};
    obj.title = meta.title;
    data.child.forEach(function(key){
        obj[key.name] = key.title;
    });
    return obj;
}
```




