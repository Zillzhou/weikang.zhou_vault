# RDS 项目开发案例

## RDS 项目开发案例
## 1 项目开发

作为开发者，如果收到一个名叫“demo 项目”的定制项目需求，首先通过阅读需求文档，并和产品经理沟通，了解项目具体需求。

## 1.1 项目需求

## “demo 项目”的项目需求描述如下：

1657183120098-caf3c3f3-a5bd-4c66-b449-1d227768a4b9.png 

## 图1
1657183135347-ae956c64-a8be-4874-864b-4e02a1daa782.png

## 图2

 在该项目的车间中，存在4个库区：PT 发运区，SD349 线边库，SD336 线边库，FD 线边库。其中 PT 发运区有6个库位，每个库位上都安装有光电检测设备，可自动检测对应库位是否存在物料；另外3个线边库，每个线边库都有4个空库位可用于存放物料。

 该车间使用一台叉车进行物料搬运，物料运输路线如图2所示，物料在 PT 发运区，由人工填充进发运区的库位，然后人工 PDA 下单，选择将物料从 PT 发运区运输到另外3个线边库区。需要补充的是，PT 发运区的6个库位，每2个库位绑定一个线边库，即这2个库位上的物料，只能运输到它们绑定的那个线边库的库位上，不能运输至另外2个线边库。如果线边库的库位被叉车运输来的物料占用，将有工人将物料转运到生产线消耗掉。

 以上是“demo 项目”的项目需求描述，接下来接入实战。

## 1.2 地图及库位搭建

在开发阶段，开发者大部分时候没有车间实际实施地图，此时，开发者需要根据需求在 Roboshop 中进行模拟地图的搭建，方便进行功能测试。Roboshop 地图及库位如何搭建，这里不做赘述，详情请查阅 Roboshop  相关使用文档。
## 项目中的库位定义如下：
## 库位定义表

## 库区名

## 库位ID

## 库区编号

## 标签

## 区域

## PT发运区

## PT-FD-1

## PT

## FD

## forkArea

## PT发运区

## PT-FD-2

## PT

## FD

## forkArea

## PT发运区

## PT-SD336-1

## PT

## SD336

## forkArea

## PT发运区

## PT-SD336-2

## PT

## SD336

## forkArea

## PT发运区

## PT-SD349-1

## PT

## SD349

## forkArea

## PT发运区

## PT-SD349-2

## PT

## SD349

## forkArea

## FD线边库

## FD-1

## FD

## forkArea

## FD线边库

## FD-2

## FD

## forkArea

## FD线边库

## FD-3

## FD

## forkArea

## FD线边库

## FD-4

## FD

## forkArea

## SD336线边库

## SD336-1

## SD336

## forkArea

## SD336线边库

## SD336-2

## SD336

## forkArea

## SD336线边库

## SD336-3

## SD336

## forkArea

## SD336线边库

## SD336-4

## SD336

## forkArea

## SD349线边库

## SD349-1

## SD349

## forkArea

## SD349线边库

## SD349-2

## SD349

## forkArea

## SD349线边库

## SD349-3

## SD349

## forkArea

## SD349线边库

## SD349-4

## SD349

## forkArea
## 搭建的模拟地图如下：
1657183219347-e6fb6fea-c035-442d-86a1-f4bbf3e54595.png 

其中地图的区域属性如“库位定义表”中所示，定义为 forkArea。
1657183303240-de0e6860-3af0-4b13-b9b4-98c477c0c628.png

搭建好模拟地图后，点击“推送场景”将该地图推送至 RDS。
1657183478409-e3d62587-8a92-4e81-953d-39d7cd755a77.png

打开浏览器，地址栏输入 http://localhost:8080，登录 RDS 管理后台。点击“运行监控”菜单，在右侧的监控页面中可以看到 Roboshop 推送的模拟地图。
1689817459721-d77a33f3-7f0d-4d37-93d7-c63f715cdeaa.png

点击“机器人”菜单，可以看到该模拟地图上添加的模拟机器人 sim_01。
1689817545698-bad9546b-6467-44dc-923c-2f8986f7debe.png

如上图，由于刚完成场景推送操作，模拟机器人 sim_01 的状态为不接单。可在列表中勾选 sim_01 机器人，再点击“抢占控制权”按钮让 sim_01 机器人变为“可接单”状态。
1689817564622-935a1850-6581-457d-af3f-dc6697cb772d.png

点击“库位”菜单，可看到在 Roboshop 中定义的所有库位 ID，库区名，及区域名。之前在 Roboshop 点击“推送场景”的按钮，在 Roboshop 中定义的库位属性，被同步推送到了 RDS，在 RDS 管理后台可以查看到。

1689817907665-30cbd704-8f82-4b85-9de3-4fca5b066875.png 

 这部分在 Roboshop 中定义并同步到 RDS 的库位称为“物理库位”，另外，在“库位”页面可以手动添加库位，只要点击“添加”按钮，完成库位属性配置，点击保存即可。在 RDS 管理后台手动添加的库位，我们称它为“逻辑库位”。
1689817934359-15b1043c-2887-4d83-9b8a-dec13689c305.png

至此，demo 项目的模拟地图搭建和库位定义同步都已经完成，RDS 管理后台中都能够查到到对应数据，接下来进入业务逻辑的开发。

## 1.3 业务开发

项目需求中，物料是从 PT 发运区运送到3个线边库，具体运送到3个线边库中的哪一个库位，需要客户在手持端菜单中选择，作为此次物料运输的终点站。
接下来进入手持端新增选择菜单的过程，只有手持端配置好选择菜单，客户才可以选择物料放置的终点站。
从制品库下载 RDS 手持端软件，启动手持端软件，登录。

1657183582869-e6a06ad1-bb8e-4e79-80b8-b236ef773869.png

 从上图可见，首次登录手持端，首页的“发起任务”页面没有可操作的菜单数据，发起物料运输的菜单需要在业务配置中添加，打开系统【设置】界面。

1690687208811-f9503c43-11c5-4d0f-88ab-7b87129e3b1c.png 

 添加手持端菜单配置，新增一个名叫“运输物料”的菜单，参数如红框中所示，保存配置。手持端具体菜单参数定义，参见 RDS 手持端说明文档。 
1690687257483-335668f7-dfbc-4d53-bec4-c5a54a53540b.png

【 设置】界面编辑完成，点击保存按钮后，手持端菜单配置刷新生效。部分系统级别参数需要重启 RDS 生效，重启 RDS 方法是点击“升级/备份”页的“重启设备”按钮。 
1657183658216-a72f2fd1-32c5-4c58-ae05-7d1fbf6d9d31.png

需要补充的是，以上步骤是通过打开系统目录中的application-biz.yml 配置文件修改手持端配置参数，除此以外，RDS 系统左侧菜单项中的“设置”界面呈现了该配置文件的内容，可以直接在 RDS “设置”界面修改对应参数，修改后，点击保存即可。
菜单配置参数中，dynamicOptionsSource: getForkSiteList 定义了一个名叫 getForkSiteList 的脚本方法，用于向手持端返回库位的列表，这个方法需要在脚本中实现。
## rdscore:
  baseUrl: http://127.0.0.1:8088

## operator:
## orders:
## - menuId: moveMat
## label: 运输物料
### menuItemBackground: #ffff33
### menuItemTextColor: black
### route: handleOperatorOrder
### robotTaskDef: moveMat
## disabled: false
## tip: 运输物料
### confirmMessage: 确定运输物料吗？
## params:
## - name: toSiteId
## input: select
## label: 放置物料的库位
          dynamicOptionsSource: getForkSiteList
## lazyGetSource: true

开始进行脚本编写，打开 RDS 管理后台的“脚本操作”页面，点击“新增”按钮。
1689818114938-b06a96ca-d885-4381-9a13-06d9b411032b.png

创建脚本的名称输入框中输入脚本名 boot，点击保存。
1689818170739-9ecb138e-77a0-4bb4-ae65-1005c4c48e7f.png 

1689818190689-3aa18283-1c48-4d5d-aa6e-23e3679669c3.png

在右侧空白处增加 js 脚本代码，点击”保存“按钮，在确保已经完成脚本编写后，点击”启动“按钮，该脚本即成功加载到 RDS 系统程序中。
1689818275010-0fe1e9fa-d04d-4b5f-92c5-516be8809029.png

此时，刷新手持端首页，可看到新增的“运输物料”的菜单。

1657183734784-4c029f05-4596-4e82-a271-279f73855d5e.png

## 点击“运输物料”的菜单后，效果如下：

1657183775287-3533963d-36fa-42d9-9f69-d0c79bc0ef7a.png

上图的下拉菜单，就是通过脚本方法 “getForkSiteList“ 返回的数据，客户可以从下拉菜单中选择一个线边库的库位作为运输物料的终点站。至此，运输物料根据客户的选择可以确定终点站，然后点击”下单“按钮，就可以发起物料搬运，但是到现在客户只选择终点站，还需要确认运输物料的起点站。
## 起点站如何确定？
 因为物料都是人工运进 PT 发运区，所以起点站必定是 PT 发运区中存在物料的库位，具体是哪一个库位需要根据库区绑定关系和光电状态来确定。
 demo 项目需求描述中讲到，3个线边库都会和 PT 发运区的2个库位绑定，这2个 PT 发运区库位的物料只能运输到绑定关系的线边库。用户选择了一个线边库的库位作为终点站，通过绑定关系，即可反推出这个终点站对应的PT发运区具体的2个库位，然后再根据这2个库位的光电检测状态，就能知道这两个库位中是否有物料，有物料的库位就可以作为物料搬运的起点站。
既然 PT 发运区和3个线边库存在绑定关系，这个绑定关系必定要配置进 RDS 系统，可以通过库位的”设置标签“功能实现。
 如下图，每一个 PT 发运区的库位都在”标签“中配置其对应的线边库库区名。比如图中，PT 发运区的 PT-FD-1 库位的标签为 FD，代表 PT-FD-1 库位绑定了 FD 线边区。
1689818321292-efe40c69-fb8f-4d3f-b460-135240fca455.png 

1689818389528-2123f0a4-12ca-4d85-a454-b3ab992d6f9b.png

PT 发运区和线边库的绑定关系已经完成设置，接下来，就需要在 js 脚本中实现手持端”运输物料“菜单的”下单“按钮的功能，以启动运输物料的任务。

1657183840199-80c34a97-6edd-4efc-89bb-a9d445811e55.png

js 脚本中增加代码。
## boot 函数中注册下单响应方法：
## // 注册下单按钮的响应方法
jj.registerHandler("POST", "/script-api/handleOperatorOrder", "handleOperatorOrder", false)

## 增加下单响应方法实现：
## // 下单按钮的响应方法
function handleOperatorOrder(param) {
    let paramObject = JSON.parse(param);
    let robotTaskDef = paramObject["robotTaskDef"];
    let params = paramObject["params"];
    let response = new ScriptRepsonseEntity();
    let operatorResponse = new OperatorRepsonseEntity();

## // 放置物料的终点库位ID
    let targetSiteId = params[0]["value"];
### // 手持端客户未选择库位，向手持端返回错误信息
    if(targetSiteId == "" || targetSiteId == null) {
        operatorResponse.code = 400;
        operatorResponse.msg = "请选择放置物料的库位";
        response.body = JSON.stringify(operatorResponse);
        return response;
## }
## let condition = {
### siteIds: [targetSiteId]
## }
## // 根据终点库位id查询库位具体信息
    let siteJson = jj.findSitesByCondition(JSON.stringify(condition), "siteId asc");
    let targetSiteInfo = JSON.parse(siteJson);
## // 终点线边库的库区名
    let siteGroup = targetSiteInfo[0]["groupName"];

    // 根据线边库的库区名，根据绑定关系，查询绑定的发运区被占用的库位列表
    // PT发运区的占用状态(即是否有物料)同步自rdscore
## condition = {
        tags: [siteGroup],
        filled: true,
## locked: false
## }
    let matSiteJson = jj.findSitesByCondition(JSON.stringify(condition), "siteId asc");
    jj.getLogger().info("matSiteJson:" + matSiteJson)
    // 如果查询到绑定的PT发运区库位不存在物料，则向手持端报错
### if(matSiteJson == "null") {
        operatorResponse.code = 400;
        operatorResponse.msg = "发运区没有可搬运的物料";
        response.body = JSON.stringify(operatorResponse);
        return response;
## }
    let matSiteList = JSON.parse(matSiteJson);

## let inputParams = {
## // 起点站
        fromSiteId: matSiteList[0]["siteId"],
## // 终点站
### toSiteId: targetSiteId
    };
## let taskParam = {
        taskLabel: "moveMat",
        inputParams: JSON.stringify(inputParams)
    };
## // 启动运输物料的任务
    jj.createWindTask(JSON.stringify(taskParam));   

    response.body = JSON.stringify(operatorResponse);
    return response;
## }

 以上代码中，PT 发运区的占用状态是由光电自动检测，这个要在 Roboshop 中配置 PT 发运区的库位检测功能，然后自动更新到 RDS 管理后台。
 而要实现自动更新到 RDS 管理后台，application-biz.yml 配置文件中需要增加PT发运区库位更新配置，也可以在管理后台的”设置“菜单中配置，如下图所示。
1689818443552-5f26b950-119b-4c16-983a-f958e76ed4c1.png

上一步骤中，下单响应方法中根据客户选择的终点库位，启动天风任务，实际触发这部分功能的代码是下面这一段。
## let taskParam = {
  taskLabel: "moveMat",
  inputParams: JSON.stringify(inputParams)
};

 其中 moveMat 是实现”搬运物料“功能的天风任务的名称，但是这个天风任务我们还没有定义，接下来我就要进行天风任务的编辑设置，用来实现物料从 PT 发运区运输到线边库。
进入”天风任务“页面。
1689818472744-e7452475-4d1f-42a0-b1c2-a6da18781544.png

点击”定义新任务“按钮，为任务命名为 moveMat，点击确定。
1689818504872-c479c1f9-51e4-4c21-a581-6c16d2efecf4.png 

1689818521431-cf33a70d-1c92-4a87-8b5f-e477fe8c2948.png

点击”编辑“，进入天风任务编辑页面。
1670323451157-0b3798bb-5074-44ce-8492-4bad77d0d344.png

然后根据需求拖动块到编辑器中，实现从起点站到终点站的搬运物料的功能，点击保存，天风任务创建完毕。
1670323393844-c605fab5-daaf-4885-98b9-3a5776cbfe63.png 

至此，项目的业务开发已经完毕，接着，就可以尝试进行手持端下单，进行功能测试了。

## 2 功能测试

打开手持端。

1657184007275-a3d6ef99-12c7-43a2-91b9-2c3ebda0e650.png 

1657184020346-b724bd5d-f411-4191-9538-9a43ec0e1315.png 

假设选择 SD349-1 作为物料运输的终点站，点击下单按钮。

1657184056638-eda59f91-52ef-464f-b1d0-9fbe68b3b990.png

点了下单之后，可以看到脚本代码返回了错误信息提示，如下图，显示”发运区没有可搬运的物料“，这是因为PT 发运区的库位中没有被占用的库位，不满足物料运输的条件。
1657184092339-0d23aae5-3850-4cd3-8c72-916ff9b067e9.png

既然 PT 发运区没有被占用的状态，为了接下来顺利完成功能测试，我们可以将 PT 发运区中绑定 SD349-1 库位的2个库位 PT-SD349-1 或者 PT-SD349-2 手动置为占用状态。
勾选 PT-SD349-1 和 PT-SD349-2 库位。点击”占用“按钮，即可将这两个库位设置为占用状态。
1689819178144-9fe5dc93-7808-4f70-96d0-7eea78fbd4c8.png 

1689819192949-b8cf387c-17c4-4c08-a5b6-a66305e5fbff.png

这样 PT 发运区的 PT-SD349-1 和 PT-SD349-2 库位已经有物料，接下来重新在手持端下单，再次选择 SD349-1 作为放置物料的终点站。

1657184157841-f8f67391-0aa1-4d89-a006-a79e0215a287.png 

1657184173695-fd590cfd-f212-43a7-884b-5839f7d9b369.png

这次，手持端显示了”下单成功“的信息。我们进入 RDS 管理后台，打开运行监控页面。如下图，在运行监控的模拟场景中，agv 模拟小车从停靠点 PP51 出发，运动到 PT-SD349-1 库位装载物料，装载物料后 agv 运动到 SD349-1 库位卸载物料。至此，小车运输物料的任务完成，完成后小车自行回到停靠点 PP51。
1689819447529-10ecd3f3-3c66-4ba8-a94b-52cb60a6604b.png 

1689819485980-4cec18fd-8c36-46ce-b64e-94669817329c.png 

1689819627486-f50a3fc1-6693-4ccf-8236-197bc4664a80.png

可以在”任务管理“页面查看 agv 运行的整个过程分解。点击”监控“，打开监控详情。
1689819764797-e1ac276b-f1ae-4ff9-8802-9172c112527d.png 

1689819882698-ee8a44b5-3c1b-4bde-9df3-7c4793a2ba15.png 

1689819904800-581c23cf-35b3-4d89-aac2-b51bb1c00e9f.png 

 此次运输物料搬运涉及的两个库位，PT-SD349-1 和 SD349-1 都在监控详情中正确显示出来，这样基本可以确认整个脚本开发和天风任务设置没有问题。
接下来，我们可以检查下整个运输物料的任务完成后，涉及的库位状态是否跟着更新。
1689819935816-fb8640ff-a68d-405a-bd83-4beabebc927f.png 

上图中，起点站 PT-SD349-1 的占用状态，从下单前的被占用，变成了运输后的无物料（未占用），符合业务逻辑。因为 PT-SD349-1 上的物料已经被运走，库位从有料变成无料状态，这是符合业务的。
1689819966526-d794a404-12e7-456a-951b-8e59747b4c20.png 

同样，既然货物已经从 PT-SD349-1 运到了 SD349-1 库位，那 SD349-1 库位应该是被物料占用的状态，如上图，确实如此。
至此，功能测试结束。
这里的功能测试，我们通过手动更新PT发运区的占用状态来表示 PT 发运区有物料，当然，我们也可以通过Modbus slave 进行完整的模拟测试，因为 PT 发运区的库位在 Roboshop 配置了库位检测，可以通过Roboshop 监听 Modbus slave 的地址状态，来实时更新 RDS 中 PT 发运区库位占用状态。

## 附录

## demo 项目完整的脚本代码

## // 区域名称
### const AREA_NAME = "forkArea"
## function boot() {
## // 注册下单按钮的响应方法
  jj.registerHandler("POST", "/script-api/handleOperatorOrder", "handleOperatorOrder", false)
## // 注册查询区域内所有库位列表的方法
  jj.registerHandler("POST", "/script-api/l2-operator/getForkSiteList", "getForkSiteList", false);
## }
## // 该方法实现查询区域内所有库位列表
function getForkSiteList(param) {
## // 查询区域内的库位列表
## let condition = {
## area: AREA_NAME
## }
  let siteListJson = jj.findSitesByCondition(JSON.stringify(condition), "siteId asc");
  let siteList = JSON.parse(siteListJson);
  let siteOptionList = [];
## // 组装成手持端显示的参数结构
  for (let i = 0; i < siteList.length; i++) {
    let siteOption = new SelectOption();
    siteOption.label = siteList[i]["siteId"];
    siteOption.value = siteList[i]["siteId"];
    siteOptionList.push(siteOption);
## }
  let response = new ScriptRepsonseEntity();
  response.body = JSON.stringify(siteOptionList);
## // 返回库位列表结果给手持端
  return response;
## }
## // 下单按钮的响应方法
function handleOperatorOrder(param) {
  let paramObject = JSON.parse(param);
  let robotTaskDef = paramObject["robotTaskDef"];
  let params = paramObject["params"];
  let response = new ScriptRepsonseEntity();
  let operatorResponse = new OperatorRepsonseEntity();

## // 放置物料的终点库位ID
  let targetSiteId = params[0]["value"];
### // 手持端客户未选择库位，向手持端返回错误信息
  if(targetSiteId == "" || targetSiteId == null) {
    operatorResponse.code = 400;
    operatorResponse.msg = "请选择放置物料的库位";
    response.body = JSON.stringify(operatorResponse);
    return response;
## }
## let condition = {
### siteIds: [targetSiteId]
## }
## // 根据终点库位id查询库位具体信息
  let siteJson = jj.findSitesByCondition(JSON.stringify(condition), "siteId asc");
  let targetSiteInfo = JSON.parse(siteJson);
## // 终点线边库的库区名
  let siteGroup = targetSiteInfo[0]["groupName"];

  // 根据线边库的库区名，根据绑定关系，查询绑定的发运区被占用的库位列表
  // PT发运区的占用状态(即是否有物料)同步自rdscore
## condition = {
    tags: [siteGroup],
    filled: true,
## locked: false
## }
  let matSiteJson = jj.findSitesByCondition(JSON.stringify(condition), "siteId asc");
  jj.getLogger().info("matSiteJson:" + matSiteJson)
  // 如果查询到绑定的PT发运区库位不存在物料，则向手持端报错
### if(matSiteJson == "null") {
    operatorResponse.code = 400;
    operatorResponse.msg = "发运区没有可搬运的物料";
    response.body = JSON.stringify(operatorResponse);
    return response;
## }
  let matSiteList = JSON.parse(matSiteJson);

## let inputParams = {
## // 起点站
    fromSiteId: matSiteList[0]["siteId"],
## // 终点站
### toSiteId: targetSiteId
  };
## let taskParam = {
    taskLabel: "moveMat",
    inputParams: JSON.stringify(inputParams)
  };
## // 启动运输物料的任务
  jj.newThreadToSetOrder(JSON.stringify(taskParam));   

  response.body = JSON.stringify(operatorResponse);
  return response;
## }

## // 手持端响应实体类
class OperatorRepsonseEntity {
## constructor() {
    this.code = 200;
    this.msg = "OK";
## }
## }
## // 服务端响应封装类
### class ScriptRepsonseEntity {
## constructor() {
    this.code = 200;
    this.body = "OK";
## }
## }
## // 手持端下拉菜单结构类
### class SelectOption {
## constructor() {
    this.value = "";
    this.label = "";
## }
## }

### 【设置】界面参数（application-biz.yml）

## rdscore:
  baseUrl: http://127.0.0.1:8088
## updateSitesBy: NONE
## updateSitesGroup:
## - cylinderGroup
## - PT

## operator:
## orders:
## - menuId: moveMat
## label: 运输物料
### menuItemBackground: #ffff33
### menuItemTextColor: black
### route: handleOperatorOrder
### robotTaskDef: moveMat
## disabled: false
## tip: 运输物料
### confirmMessage: 确定运输物料吗？
## params:
## - name: toSiteId
## input: select
## label: 放置物料的库位
          dynamicOptionsSource: getForkSiteList
## lazyGetSource: true
## demo 地图

## demo.zip
