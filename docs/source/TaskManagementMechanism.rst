タスク管理機構の概要
===================================

タスク管理機構とはOPERAと連携し、Behavior Treeを使用してユーザから与えられたタスク（施工シナリオ）をもとに建設機械を操作するための仕組みです。
タスク管理機構では、データベースに格納された情報を任意のタイミング読み出してきて、これらの値をもとに動作します。このため、以下のデータを事前にデータベースに格納しておく必要があります。
データベースの仕様については :doc:`こちら <DataBase>` に記載したとおりです。

- **タスクデータ** : 
  
  施工シナリオに沿った建設機械の一連の動作データ
- **静的パラメータデータ** : 
  
  タスク実行開始時に読み出される、タスクデータの一部を保管するデータ
- **動的パラメータデータ** : 
  
  タスク実行中に読み出される、タスクデータの一部を保管するデータ

パラメータデータは、例えばダンプのナビゲーションの目標位置姿勢です。タスクデータでは当初はこのデータが未定の状態となっており、
代わりにデータベース上の値を検索するためのキーが与えられています。このキーをもとにパラメータデータを読み出してきて、
タスクデータを補完することで一連の施工シナリオを作り出します。パラメータデータにはタスク実行開始時にのみ読み出される静的なものと、
タスク実行中に読み出される動的なものの2種類が存在し、前者を静的パラメータデータ、後者を動的パラメータデータと呼びます。
以下では、タスク実行に必要となるデータが事前にデータベースに格納されていることを前提として、タスク管理機構における処理の流れを以下に示します。

.. image:: images/TaskManagementMechanism_Architecture.svg
   :alt: システム構造
   :width: 600px
   :align: center  
  
.. raw:: html

   <br><br>

1. ユーザからの実行指令がタスク管理機構へと送られてくると、タスクスケジューラ (Behavior Tree)がデータベースから施工の遂行に必要なタスクデータ
   及び静的パラメータデータを読み出します。実行するタスクを検索するための情報はユーザからの実行指令に含まれます。 (TMS_UR→ TMS_TS → TMS_DB → TMS_TS)
2. タスクスケジューラが読み出した値をもとに、タスクを実行し、操作指令を建設機械へと送ります。(TMS_TS → TMS_IF_for_OPERA → TMS_RP → TMS_RC → Mahcinery)
3. 建設機械は送られてきた指令をもとに動作し、建設機械の位置姿勢といった情報や与えられた操作指令の実行状況をタスクスケジューラへと逐次返却します。(Machinery → TMS_RC → TMS_RP → TMS_IF_for OPERA → TMS_TS)

加えて上記の流れでタスク実行がなされている間、以下の流れに従って環境側に配置されたセンサからの情報をデータベースを介してタスクスケジューラへと伝え、タスク実行に反映させます。

1. 建設現場など環境側に配置されたセンサからの情報を収集します。(Sensor → センシングPC)
2. 収集した情報を処理し、メタデータを付与して ROS2-TMS for Construction へと送ります。 (センシング PC → TMS_SP)
3. ROS2-TMS for Construction は値を受け取ると、メタデータをもとにデータベース上の該当データを見つけ、受け取ったデータで上書きします。(TMS_SP → TMS_DB)
4. タスクスケジューラはタスクの実行中にデータベース上から値を見つけ、実行中のタスク列に反映させます。(TMS TS → TMS DB → TMS TS)
5. 反映したタスク列に基づき、建設機械へと動作指令を送ったのちにそのフィードバックとして建設機械から状態信号を受け取ります (TMS_TS → TMS_IF_for_OPERA → TMS_RP → TMS_RC → Machinery → TMS_RC → TMS_RP →TMS_IF_for_OPERA → TMS_TS)

このように、タスク管理機構においては建設機械自体に取り付けられたセンサと環境側に配置したセンサからの2つの情報をもとに建設機械を操作することが可能となっています。


タスク管理機構の使用方法
===================================

.. _task-execusion:

タスク実行
-----------------------------------

1. 以下のコマンドを実行し、MongoDBを起動します。::
   
      sudo systemctl start mongod
   
2. 以下のコマンドを実行し、MongoDB Compassを起動します。::
   
      mongodb-compass
   
3. ポップアップするMongodbの画面にて、URIで"mongodb://localhost:27017/"と指定されていることを確認し、"Connect"ボタンをクリックすると表示される画面にて、rostmsdbをクリックして以下の画面が開けばデータベースの設定が完了しています。
   なお、データの個数などはここでは気にせず、単にrostmsdbデータベース下のコレクションが画像のものと同様に生成されていれば問題ありません。

   .. image:: images/db_setup.png
      :alt: db_setup
      :width: 400px
      :align: center  

   .. raw:: html

      <br><br>
   
   もしデータベースが上記の表示にならない場合、データベースの構築ができておらず、このままでは正常にタスクを起動できません。
   この場合には以下のコマンドを実行してください。::
       
      cd ~/ros2-tms-for-construction_ws/src/ros2_tms_for_construction/demo
      unzip rostmsdb_collections.zip
      mongorestore dump

4. データベースが正常に構築できていることを確認したら以下のコマンドを実行し、タスク管理機構を起動します。なお、タスクIDはデータベースの~/rostmsdb/task以下に格納してある所望のタスクデータのものを指定します。::

      cd ~/ros2-tms-for-construction_ws
      source install/setup.bash
      ros2 launch tms_ts_launch tms_ts_construction.launch.py task_id:=[タスクID]
    
    
    すると以下に示すGUIボタンが表示されます。GUIボタンには2種類のボタンが搭載されており、緑の部分をクリックするとサンプルタスクが実行されます。また、赤色の領域をクリックすると実行中のタスクを緊急停止することが可能です。


   .. image:: images/gui_button.png
    :alt: gui_button
    :width: 300px
    :align: center  

   .. raw:: html

      <br><br>

.. note::
   
   なお、現在のタスク管理機構は連続したタスク実行に対応しておりません。このため、一度タスクを実行したあとは再度launch.pyファイルを立ち上げ直す必要があります。
   

タスクの可視化
-----------------------------------
    
BehaviorTree.CPPではGrootと呼ばれるGUIアプリケーションが存在します。Grootの機能は以下に示すとおりです。

- **Editor** : タスクを設計・編集する機能
- **Monitor** : 実行中のタスクを可視化する機能

本章では、Grootを使用したタスクの可視化について紹介します。実行手順は以下に示すとおりです。

1. Behavior Treeがタスクを実行している間、Grootと呼ばれるアプリケーションを使用してタスクが実行されていく様子を可視化することができます。GrootはROS2上で実装されており、以下のコマンドで起動します。::

      cd ~/ros2-tms-for-construction_ws
      ros2 run groot Groot

.. note::

   Monitor機能はBehavior Treeがタスクを実行している間のみ実行可能です。

2. Grootを起動すると、以下の画面が表示されます。可視化機能の起動には"Monitor"と記載されている部分をクリックします。
   
     
  .. image:: images/groot_menu.png
   :alt: groot_menu
   :width: 300px
   :align: center  

.. raw:: html

   <br><br>

3. すると以下の画面が表示されるので、以下に示すとおりIPアドレスとポートを指定し、"Connect"ボタンをクリックします。

   - Sensing IP : localhost
   - Publisher Port : 1666
   - Server Port : 1667

  .. image:: images/groot_monitor_menu.png
   :alt: groot_monitor_menu
   :width: 300px
   :align: center  

.. raw:: html

   <br><br>
   
.. note::

   Publisher Port, Server Portの値は起動するtms_ts_managerによって異なるため注意してください。tms_ts_construction.launch.pyでタスクを実行した場合、`こちら <https://github.com/irvs/ros2_tms_for_construction/blob/main/tms_ts/tms_ts_manager/src/task_schedular_manager.cpp#L74>`_
   の部分でPublisher PortとServer Portを指定しており、上記のSensing IP, Publisher Port, Server PortでGrootとBehavior Treeを接続可能です。

4. すると以下に示すとおり、実行中のタスクの様子が可視化されます。緑色は実行後、橙色は実行中、青色は未実行を示しています。(画像のツリーはサンプルタスク(task_id: 1)のものです)

  .. image:: images/monitering_task.png
   :alt: groot_monitor_menu
   :width: 600px
   :align: center  

.. raw:: html

   <br><br>

.. note::

   タスクが可視化されず、以下のポップアップウィンドウが表示される場合があります。これはBehavior Treeがタスクを実行されていない若しくは実行終了していることを示しています。
   このような場合、 `こちら <task-execusion_>`_  の手順にしたがって再度タスクを実行し、必ずBehavior Treeがタスクを実行している間に"Connect"ボタンを押してください。

     .. image:: images/groot_monitoring_warn.png
      :alt: groot_monitoring_warn
      :width: 300px
      :align: center  

   .. raw:: html

.. _task-creation:

タスクの作成
-----------------------------------

本章では、Grootを使用したタスク作成の手順について説明します。

1. 以下のコマンドを実行し、Grootを起動する。::

      cd ~/ros2-tms-for-construction_ws
      ros2 run groot Groot

2. Grootを起動すると、以下の画面が表示されます。タスク作成機能の起動には"Editor"と記載されている部分をクリックします。
 
  .. image:: images/groot_menu.png
   :alt: groot_menu
   :width: 300px
   :align: center  

.. raw:: html

3. そして表示された画面上で以下の手順でカスタムノードを追加します。ここでいうカスタムノードとはROS2-TMS for Construction
   で独自に用意したBehavior Treeノード(以降、BTノードと呼ぶ)のことを指しています。

     .. image:: images/groot_add_custom_nodes.png
      :alt: groot_add_custom_nodes
      :width: 500px
      :align: center  

   .. raw:: html

   すると、以下に示すように新たに複数のノードが追加される。

     .. image:: images/custom_nodes.png
      :alt: custom_nodes
      :width: 500px
      :align: center  

   .. raw:: html

4. この状態で任意のタスクを構築しています。なお、Leaf Nodeの概要はこちらに、その他のカスタムノード及びBehavior Treeの標準ノードの
   概要は :doc:`こちら <CustomNodes>` で説明したとおりです。Behavior Treeではこれらのノードを木構造に接続することでタスクを構築します。作成したタスクの例は以下のとおりです。

     .. image:: images/sample_task.png
      :alt: sample_task
   .. raw:: html

.. note::

   タスクを構築する際には、タスクを構成するSubtask Nodeで使用するパラメータデータをデータベースの~/rostmsdb/parameter下
   に置いておく必要があります。データベースの仕様及びLeaf Nodeで指定するパラメータの仕様についてはこちらをご覧ください。

1. Groot上でタスクを作成したあとは以下の手順に沿って、src/ros2_tms_for_construction/tms_ts/tms_ts_manager/configディレクトリ下にタスク列のxmlファイル
を出力します。

     .. image:: images/save_task.png
      :alt: save_task
      :width: 300px
      :align: center  

   .. raw:: html

1. xml形式のタスク列を出力した後は、以下のコマンドを実行しデータベース上にタスクデータとして登録します。::
      
      cd ~/ros2-tms-for-construction_ws
      colcon build --packages-select tms_ts_manager
      ros2 run tms_ts_manager task_generator.py --ros-args -p bt_tree_xml_file_name:=[configディレクトリ以下のパス]

2. するとデータベース上の~/rostmsdb/task下に新たなタスクデータが作成され、 `こちら <task-execusion_>`_  の手順にそってBehavior Treeから実行できるようになります。


