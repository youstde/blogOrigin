---
title: Js学习笔记-fileReader
date: 2017-09-08 14:23:04
tags:
---

直接上代码：（上传图片格式为base64的）

```javascript
<script type="text/javascript">  
            function showPreview(source) {  
                var file = source.files[0];  
                // 进行校验，确保上传的为图片
			    if(!/image\/\w+/.test(file.type)){  
			        alert("请上传图片！");  
			        return false;  
			    }
                if(window.FileReader) {  
                    var fr = new FileReader();
                    console.log(fr)
                    fr.onloadend = function(e) {  
                        document.getElementById("portrait").src = e.target.result;  
                    };  
                    fr.readAsDataURL(file);  
                }  
            }  
        </script>  
      <!-- accept="image/*" 可以限制只能选择图片上传，但是会使按钮变的迟钝 -->
    <input type="file" name="file" onchange="showPreview(this)"/>  
    <img id="portrait" src="" width="70" height="75"> 
```

