Blackboardの概要
===================================

ROS2-TMS for Constructionのタスク管理機構を使用してタスクの実行を行う場合、"Local Blackboard"と"Global Blackboard"と
いう2種類の記憶領域が存在する。前者のLocal BlackboardはBehaviorTree.CPPに標準で用意されたものであり、詳細は
 `こちら <https://www.behaviortree.dev/docs/3.8/tutorial-basics/tutorial_02_basic_ports>`_ を
参照されたい。

Global Blackboardについて
===================================

本章ではGlobal Blackboadに焦点を当て、説明する。
Global BlackboardはBehaviorTree.CPPにはなく、タスク管理機構側で独自に実装したメモリ領域である。


実装背景
-----------------------------------

現在のROS2-TMS for Constructionでは2種類のタスク実装が可能である。

- 中央集権システム：自律化施工で使用する複数台の建設機械を1つのタスクで管理し、施工を実現する
- 分散システム型：自律化施工で使用する複数台の建設機械について、個々の建設機械ごとにタスクを実装し、これらが対応な立場で必要に応じて通信し合うことで施工を実現する

中央集権システムでは1つのタスク内で自律化施工を実装するため、ノード間の連携のみ可能であれば問題なかった。
この機能はBehaviorTree.CPPに標準で実装されているLocal Blackboardで実現できていた。
その一方で、分散型システムの場合には異なるタスク間で連携する必要がある。
異なるタスク間で情報共有可能な仕組みはBehaviorTree.CPPにまだなく、今回我々はGlobal Blackboardとして新たに実装した。
Global Blackboardの概要図を以下に示す。

.. image:: images/global_blackboard_image.png
   :alt: Global Blackboardのイメージ図
   :width: 800px
   :align: center  
  
.. raw:: html

   <br><br>

1. Task 1はこちらのノードを介してGlobal Blackboard上に値の書き込みを行う。
2. Task 2はこちらのノードを介してGlobal Blacoboardから値の読み出しを行う。
3. Global Blackboaarから読み出してきた値はTask 2のLocal Blackboard上に保存される。

Local Blackboard上に保管した値は必要に応じてTask 2内のノードから利用することが可能である。

使用方法
-----------------------------------

サンプルタスクを使用して使用方法を説明する。実行手順は以下に示すとおりである。

1. まずはじめにタスクの実装に応じて、必要となるGlobal Blackboadr上のパラメータを~/rostmsdb/parameter下に作成する。
   今回のサンプルタスクの実装にあたっては以下を用意した。

   .. image:: images/global_blackboard_imp.png
   :alt: Global Blackboardの実装例
   :width: 800px
   :align: center  
  
.. raw:: html

   <br><br>

2. 次に、Global Blackboardに対応したタスク実装を行う。今回は以下の2つのタスクをサンプルタスクとして実装した。

   タスク5の以下のノードでは、Global Blackboardへの値の書き込みを行う。
   MongoValueWriterはinput_name, mongo_param_name, mondo_record_nameの3種類の入力ポートが存在し、
   monog_record_nameで指定したキーから~/rostmsdb/parameter下の該当するデータを発見し、
   mongo_param_nameで指定したデータの中身をinput_valueで指定した値で上書きしている。
   実際にノードの実行前後では以下のようにGlobal Blackboard上の値が変化する。

   .. image:: images/MongoValueWriter_imp.png
   :alt: MongoVFalueWriterの実装例
   :width: 800px
   :align: center  
  
.. raw:: html

   <br><br>

   また、Global Blackboardからの値の読み出しを行うのはタスク6の以下のノードである。
   BlackboadrValueReaderMongoにはmonogo_param_name, mongo_record_name, output_portの3種類のポートが存在し、
   monog_record_nameで指定したキーから~/rostmsdb/parameter下の該当するデータを発見し、
   mongo_param_nameで指定したデータの中身をGlobal Blackboardから読み出してきて、
   output_portで指定したキーを持つLocal Blackboard上の値を読み出してきた値で上書きする。
   その流れを以下に示す。

   .. image:: images/MongoValueWriter_imp.png
   :alt: MongoVFalueWriterの実装例
   :width: 800px
   :align: center  
  
.. raw:: html

3. そして2つのタスクを同時に実行する。サンプルタスクの場合、以下の手順で2種類のタスクを同時に実行することが可能である。::
   
   sudo systemctl start mongod
   cd ros2-tms-for-construction_ws
   source install/setup.bash
   ros2 launch tms_ts_launch tms_ts_sample_gb.launch.py task_id_1:=5 task_id_2:=6

4. 上記のコマンドを実行すると、Global Blackboard上の値が更新されるのと同時にtask_id:6のLocal Blackboard上のパラメータの値がログ出力され、
   Global Blackboardを介してタスク間で情報のやりとりができていることを確認できる。

   







