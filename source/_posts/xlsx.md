---
title: 使用js-xlsx库，前端读取Excel报表文件
categories: javascript
tags: javascript
---

html代码：

	<input type="file" id="excel-file">
	<textarea id="js_con" style="width: 800px;height: 800px"></textarea>
	
JS代码：

    $('#excel-file').change(function(e) {
        var files = e.target.files;

        var fileReader = new FileReader();
        fileReader.onload = function(ev) {
            try {
                var data = ev.target.result,
                        workbook = XLSX.read(data, {
                            type: 'binary'
                        }), // 以二进制流方式读取得到整份excel表格对象
                        persons = []; // 存储获取到的数据
            } catch (e) {
                console.log('文件类型不正确');
                return;
            }

            // 表格的表格范围，可用于判断表头是否数量是否正确
            var fromTo = '';
            // 遍历每张表读取
            for (var sheet in workbook.Sheets) {
                if (workbook.Sheets.hasOwnProperty(sheet)) {
                    fromTo = workbook.Sheets[sheet]['!ref'];
                    console.log(fromTo);
                    persons = persons.concat(XLSX.utils.sheet_to_json(workbook.Sheets[sheet]));
                    // break; // 如果只取第一张表，就取消注释这行
                }
            }
            console.log(persons);
            $('#js_con').text(JSON.stringify(persons))
        };

        // 以二进制方式打开文件
        fileReader.readAsBinaryString(files[0]);
    });
	
	
> 插件地址：[js-xlsx](https://github.com/SheetJS/js-xlsx)