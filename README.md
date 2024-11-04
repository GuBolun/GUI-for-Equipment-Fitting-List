# GUI-for-EFL-Equipment-Fitting-List
quickyly calculate and update product accessory prices through SQLITE, and be friendly to EXCEL

# 界面说明：
### 界面上半部分为阀门本身参数，下半部分为其拥有的配件情况。另外，阀门本身参数中，'毛坯重量'，'毛坯单价'，'加工费'，'成本'受'货源'影响，且成本自动计算生成无法修改。
  - 界面可伸缩，但只有配件明细部分会展示更多内容，字体和按钮大小等不会变化。
  - 几乎全局支持鼠标操作和键盘操作。

# 操作说明：
  - 所有输入框均可输入内容，类型会自动检查。'更新日期'会自动更新，同时也支持手动修改。'成本'自动计算且无法修改，如有差错可通过'其他费用'修正。
## 查询阀门（查询支持两种操作。不支持同时对货源的查询。）
  - 1.点击'型号'按钮，从各选项框中输入或依次选择型号参数，点击'选择型号'进行查询。
  - 2.直接在'型号'输入框中输入，会自动根据输入匹配现有型号，可直接鼠标点击匹配到的结果进行查询，也可以按键盘上下键进行选择并按回车进行查询。
  - 如果查询型号不存在，则会提示是否创建该型号。否则自动显示该型号成本最低的货源情况，通过'货源'的下拉菜单选择并查看相应货源（输入'货源'无法查询）。
## 修改阀门参数
  - 完成任意编辑后，点击'修改'按钮即可保存修改。
  - 如需新增货源，可以1.直接在'货源'输入框中输入并点击修改进行保存。2.点击'货源'按钮，像excel一样进行编辑（点击'+'或'-'新增或删除列），关闭窗口会提示是否保存修改。
  - **货源单价与config.xlsx实现了双向联动。
## 添加配件（也可以修改价格或数量）
  - 输入'配件名称'，'配件价格'和'配件数量'，点击'添加配件'按钮，将新建配件并将配件添加到当前'型号'中。
  - 若不填写'配件数量'，则仅对配件本身做修改（新建或修改价格）
  - 在输入'配件名称'时同样会自动匹配现有结果，选择后自动显示'配件价格'（当然如果不输入'配件价格'会默认使用现有的价格或'0'。）；并且选择后会将配件全称拷贝到粘贴板。
  - **config.xlsx中的材质-散件在软件中实现了同步修改，但价格未与excel联动。另外还有石墨圈（同材质相同斤价），螺帽（同规格同材质相同单价），铜螺母（相同斤价）实现了批量修改。
## 删除配件
  - 1.点击配件明细表中的任意一项，其名称、数量和单价会自动填充到右侧的配件输入框中，点击'删除配件'，将从该型号中删除对应的配件。
  - 2.输入'配件名称'，'配件价格'和'配件数量'，点击'删除配件'按钮，会与数据库中的数据进行匹配，如果相同则从该型号中删除对应的配件。
## 其他菜单栏操作
  - 文件-清理：删除未填写任何有效信息的非唯一型号货源，删除无任何有效信息的型号，删除没有分配给任何型号的配件（除用于材质批量修改的保留配件）。
  - 文件-退出：与直接点击右上角的'X'逻辑一致。
  - 查看-查看配件：提供了一个按配件名模糊搜索，并展示价格及所包含的型号列表。主要是为了测试和前期添加数据库阶段方便。点击选中能自动将型号列表拷贝到粘贴板。
  - 删除-删除阀门：输入指定'型号'以删除阀门，但不会删除其曾相关的配件。
  - 删除-删除配件：输入指定'配件名称'以删除配件。若配件仍存在于一些型号的阀门当中，则会进行提示，可以修改该配件名称或者仍然删除（删除配件并从其相关型号中删除）。
  - 修改-修改阀杆斤价：输入新的阀杆斤价以修改。也可以通过config.xlsx文件直接修改
  - 导入/导出-导入配件价格：可以在该excel表中按照'名称-规格-材质-单价'的顺序填写（批量复制）配件，并由此按钮批量修改配件价格。
  - 导入/导出-导出所有数据：将所有数据按照'配件明细汇总_.xlsx'模板的样式在当前目录下输出'配件明细汇总导出.xlsx'文件，如果'配件明细汇总导出.xlsx'已存在，则在'配件明细汇总导出.xlsx'的基础上修改/新增（为了保留后续新增的备注）。
  - 其他-说明：一些补充说明
  - 其他-关于我们：（显得很专业）

# 文件说明：
## main.py
  - python源码文件
## main.exe
  - 打包好的可执行文件。
## config.xlsx
  - 一些可供修改的配置文件，包括飞、锻的价格、阀杆斤价、货源毛坯单价和一个材质批量修改配置；所有内容可竖向添加，数据库同步修改。
## main.db
  - 数据库文件，存储阀门，配件，阀门-配件关系，阀门-货源四张表。
  -- valves (name TEXT PRIMARY KEY, date DATE, weight REAL, hnumber TEXT, rodd TEXT, rodl TEXT , diff REAL, remark TEXT)
  -- items (name TEXT PRIMARY KEY, cost REAL)
  -- relationships (vname TEXT, iname TEXT, count INTEGER, PRIMARY KEY (vname,iname))
  -- re_supplies (name TEXT, supply TEXT, cost REAL, bweight REAL, bwcost REAL, process REAL, PRIMARY KEY (name,supply))
## 配件价格.xlsx
  - 可以批量修改配件价格的excel文件，通过点击菜单栏'导入/导出-导入配件价格'实现。
## 配件明细汇总_.xlsx
  - 配件明细表模板，导出数据时根据该模板导出。
## README.md
  - 本文件
