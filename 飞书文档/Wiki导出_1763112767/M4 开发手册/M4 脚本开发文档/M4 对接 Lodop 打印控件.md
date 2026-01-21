# M4 对接 Lodop 打印控件

## M4 对接 Lodop 打印控件
对于有打印需求的项目，可使用 Lodop（Web打印控件）。
## 第一步：安装软件

### CLodop_WinNT_6.628_SHXG.zip
在客户需要操作打印的 PC 机上，安装 CLodop，安装成功后启动，如下图。
## image.png

## 第二步：激活 Licenses 配置
在 M4 程序的 ui-ext 路径下，放入 LodopFuncs.js 文件。 
## LodopFuncs.js
在 js 中 179 行，空白位置，添加 Licenses 信息。
## image.png

若不添加 Licenses ，打印会有试用版的水印。
## image.png

## 第三步：自定义打印模版
http://127.0.0.1:8000/CLodopDemos/PrintSampIndex.html
根据 Lodop 官网介绍，参考打印模版设计，自定义模板后，点击生成程序代码，将代码粘到第四步。
## image.png

## 第四步：打印配置
在 M4 程序的 ui-ext 路径下，main.js 文件中添加以下代码。

const ele = document.createElement("script")
ele.src = 'http://127.0.0.1:8000/Lodopfuncs.js'
document.body.appendChild(ele)
在 M4 中根据不同的需求，创建自定义扩展按钮，前端扩展代码中，声明变量和调用方法（第2-3行）必须要有。
## image.png

### console.log("发送打印~",ctx.ev)
## var LODOP; //声明变量
### LODOP = getLodop(); //调用
const d = new Date();
const year = d.getFullYear().toString().slice(-2);
const month = (d.getMonth() + 1) > 9 ? String.fromCharCode(64 + (d.getMonth() + 1 - 9)) : d.getMonth() + 1;
const day = d.getDate().toString().padStart(2, '0');
const prodDate = `${year}${month}${day}`;
for (const line of ctx.ev.btLines) {
    LODOP.SET_PRINT_MODE("WINDOW_DEFPRINTER",'ZDesigner ZT411-300dpi ZPL');
    LODOP.PRINT_INITA(-9,-42,800,600,"打印控件功能演示_Lodop功能_空白练习");
    LODOP.ADD_PRINT_TEXT(23,62,156,26,"客户名称：汉威科技集团");
    LODOP.ADD_PRINT_TEXT(50,60,87,25,"供应商代码：");
    LODOP.ADD_PRINT_TEXT(52,130,88,23,ctx.ev.LIFNR);
    LODOP.ADD_PRINT_TEXT(76,64,76,21,"订单编号：");
    LODOP.ADD_PRINT_TEXT(76,130,87,25,ctx.ev.id);
    LODOP.ADD_PRINT_TEXT(96,63,46,23,"品号：");
    LODOP.ADD_PRINT_TEXT(99,131,90,20,line.MATNR);
    LODOP.ADD_PRINT_TEXT(115,62,49,25,"数量：");
    LODOP.ADD_PRINT_TEXT(120,130,90,20,line.MENGE);
    LODOP.ADD_PRINT_TEXT(140,61,47,25,"批次：");
    LODOP.ADD_PRINT_TEXT(140,130,90,24,line.CHARG);
    LODOP.ADD_PRINT_TEXT(164,60,51,25,"规格：");
    LODOP.ADD_PRINT_TEXT(160,129,88,25,line.MAKTX);
    const code = `${ctx.ev.LIFNR}${line.MATNR}${prodDate}${line.CHARG}`
    LODOP.ADD_PRINT_BARCODE(31,218,120,101,"QRCode",code);
    LODOP.PRINT();
## }
### __jk.toast.toastSuccess("成功")
## image.png

## 效果图：
## image.png
