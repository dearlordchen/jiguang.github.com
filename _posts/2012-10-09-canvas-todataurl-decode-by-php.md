---
layout: post
title: "Canvas toDataURL decoded by PHP"
description: ""
category:
tags: [canvas, PHP]
---
{% include JB/setup %}

����֪�� Canvas �� [toDataURL](https://developer.mozilla.org/en-US/docs/DOM/HTMLCanvasElement) ������������ base64 ����� dataURL ��ʽ��ͼƬ��ҳ���У����磺

    function test() {
         var canvas = document.getElementById("canvas");
         var url = canvas.toDataURL();

         var newImg = document.createElement("img");
         newImg.src = url;
         document.body.appendChild(newImg);
    }

��ô����ν����ɺ��ͼƬ���������أ�

����ڿͻ��˵Ļ�����򵥵ķ�ʽ��������Ҽ�->���Ϊ����ô�ڷ���������α����أ�

�����������ʹ�õ���PHP����ô����ʹ��PHP�� [base64_decode](http://php.net/manual/en/function.base64-decode.php) �����������м�����Ҫע�⣺

1����Ҫ���ո�ת��Ϊ�Ӻţ�

    $encodedData = str_replace(' ','+',$encodedData);

2����Ҫȥ��ǰ���ǰ׺��

    $encodedData = preg_replace('/^data:image\/(png|jpg);base64,/','',$encodedData);

3��ǰ�����������ڿͻ���ʹ�� JavaScript ��ɣ�Ȼ�󽫴���������� post �� PHP ҳ�棬�ٵ��� base64_decode ���ɣ�

    $decocedData = base64_decode($encodedData);

4������󣬿��Խ�ͼƬֱ����ʾ������

    header("Content-type: image/png");
    echo $decocedData;