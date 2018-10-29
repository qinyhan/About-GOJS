# About-GOJS
Gojs组件，前端图形化插件之利器

## 1、Gojs简介
  GoJS是一个功能丰富的JS库，在Web浏览器和平台上可实现自定义交互图和复杂的可视化效果，它用自定义模板和布局组件简化了节点、链接和分组等复杂的JS图表，给用户交互提供了许多先进的功能，如拖拽、复制、粘贴、文本编辑、工具提示、上下文菜单、自动布局、模板、数据绑定和模型、事务状态和撤销管理、调色板、概述、事件处理程序、命令和自定义操作的扩展工具系统。无需切换服务器和插件，GoJS就能实现用户互动并在浏览器中完全运行，呈现HTML5 Canvas元素或SVG，也不用服务器端请求。 GoJS不依赖于任何JS库或框架（例如bootstrap、jquery等），可与任何HTML或JS框架配合工作，甚至可以不用框架。

## 2、使用入门
  ### （1）文件引用
    <script src="gojs/go-debug_ok.js"></script>
    可以用cdn上面的最新版本，也可以引用本地down下来的文件。如果是开发，可以引用debug版本的js，正式运行的时候引用正式的js，这个无需多讲。
  ### （2）创建画布
    随便定义一个html元素，作为我们的画布
    <div id="myDiagramDiv" style="margin:auto;width:300px; height:300px; background-color:#ddd;"></div>
  ### 然后使用gojs的api初始化画布
    //创建画布
        var objGo = go.GraphObject.make;
        var myDiagram = objGo(go.Diagram, "myDiagramDiv",
            {
                //模型图的中心位置所在坐标
                initialContentAlignment: go.Spot.Center,
                
                //允许用户操作图表的时候使用Ctrl-Z撤销和Ctrl-Y重做快捷键
                "undoManager.isEnabled": true,
                
                //不运行用户改变图表的规模
                allowZoom: false,

                //画布上面是否出现网格
                "grid.visible": true,

                //允许在画布上面双击的时候创建节点
                "clickCreatingTool.archetypeNodeData": { text: "Node" },

                //允许使用ctrl+c、ctrl+v复制粘贴
                "commandHandler.copiesTree": true,  

                //允许使用delete键删除节点
                "commandHandler.deletesTree": true, 

                // dragging for both move and copy
                "draggingTool.dragsTree": true,  
            });   
    ## 官方示例用的$符号作为变量，博主觉得$符号太敏感，还是换个名字吧
  ### （3）创建模型数据（Model）
    接着上面的代码，我们增加如下几行:
      var myModel = objGo(go.Model);//创建Model对象
        // model中的数据每一个js对象都代表着一个相应的模型图中的元素
        myModel.nodeDataArray = [
            { key: "工厂" },
            { key: "车间" },
            { key: "工人" },
            { key: "岗位" },
        ];
        myDiagram.model = myModel; //将模型数据绑定到画布图上
  ### （4）创建节点（Node）
    上面有了画布和节点数据，只是有了一个雏形，但是还没有任何的图形化效果。我们加入一些效果试试

    在gojs里面给我们提供了几种模型节点的可选项：

    Shape:形状——Rectangle（矩形）、RoundedRectangle（圆角矩形），Ellipse（椭圆形），Triangle（三角形），Diamond（菱形），Circle（圆形）等
    TextBlock:文本域（可编辑）
    Picture:图片
    Panel:容器来保存其他Node的集合 
    默认的节点模型代码只是由一个TextBlock组件构建成
    我们增加如下一段代码：
       // 定义一个简单的节点模板
        myDiagram.nodeTemplate =
            objGo(go.Node, "Horizontal",//横向布局的面板
                // 节点淡蓝色背景
                { background: "#44CCFF" },
                objGo(go.Shape,
                    "RoundedRectangle", //定义形状，这是圆角矩形
                    { /* Shape的参数。宽高颜色等等*/figure: "Club", width: 40, height: 60, margin: 4, fill: 'red' },
                    // 绑定 Shape.figure属性为Node.data.fig的值，Model对象可以通过Node.data.fig 获取和设置Shape.figure（修改形状）
                    new go.Binding("figure", "fig"), new go.Binding('fill', 'fill2')),
                objGo(go.TextBlock,
                    "Default Text",  // 默认文本
                    // 设置字体大小颜色以及边距
                    { margin: 12, stroke: "white", font: "bold 16px sans-serif" },
                    //绑定TextBlock.text 属性为Node.data.name的值，Model对象可以通过Node.data.name获取和设置TextBlock.text
                    new go.Binding("text", "name"))
            );

        var myModel = objGo(go.Model);//创建Model对象
        // model中的数据每一个js对象都代表着一个相应的模型图中的元素
        myModel.nodeDataArray = [
            { name: "工厂", fig: 'YinYang', fill2: 'blue' },
            { name: "车间", fig: 'Peace', fill2: 'red' },
            { name: "工人", fig: 'NotAllowed', fill2: 'green' },
            { name: "岗位", fig: 'Fragile', fill2: 'yellow' },
        ];
        myDiagram.model = myModel; //将模型数据绑定到画布图上

