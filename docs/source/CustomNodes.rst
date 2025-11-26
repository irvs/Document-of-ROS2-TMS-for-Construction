カスタムノードの概要
===================================

本章ではROS2-TMS for Constructionで独自に用意したノードについて紹介します。
これらのノードには大きくLeaf Node、Subtask Node、Other Custom Nodesの3種類が存在します。
以下ではLeaf Node及びSubtask Nodeと、Other Custom Nodesに章分けして説明します。

Leaf Node・Subtask Node
====================================

建設機械を動作させる目的で使用するノードがLeaf NodeとSubtask Nodeであり、Behavior Treeの末端に位置するノードです、
これらのノードの主な関係は以下に示すとおりです。

     .. image:: images/LeafNode_and_SubtaskNode.png
      :alt: custom_nodes
      :width: 500px
      :align: center  


1. Leaf Nodeにはmodel_name・record_name・subtask_nodeという3種類の入力ポートが存在します。このうち、model_nameとrecord_nameでデータベース上のパラメータデータを指定します。
2. Leaf NodeはROS2 Actionを介してsubtask_nameで指定したSubtask Nodeへと接続します。
3. Leaf NodeはROS2 Actionのgoalメッセージとしてmodel_nameとrecord_nameで指定したキー値をSubtask Nodeへと送ります。
4. Subtask NodeはSubtask Nodeは2種類のキー値を受け取るとこれらをもとにデータベースを検索し、該当するものが見つかればデータを読み出してきます。
5. Subtask Nodeは読み出してきた値をもとに建設機械へ動作指令を送ります。

なお、Subtask Nodeとは上記の通り、一部のパラメータを未定として実装した建機操作用のノードです。例えばバックホウの目標姿勢がパラメータ化されており、Subtask Nodeはこの値で指定された
姿勢へとモーションプラニングを行う機能を持っているとします。すると上図の場合、パラメータデータとしてデータベースから目標姿勢情報を読み出してきて、Subtask Nodeはこの値をもとに
建設機械をその姿勢へ移動させます。なお、LeafノードとSubtaskノードの一覧は以下に示すとおりです。

.. note::

   なお、Leaf NodeはBTノードである一方でSubtask NodeはBTノードではなく、単なるROS2ノードです。
   このため、GrootにおけるBehavior Tree構築時には、末端のノードはLeaf Nodeになり、Leaf Nodeに接続するSubtask Nodeは
   Leaf Node上の入力ポート上で指定します。


Leaf Nodes
-----------
.. toctree::
   :maxdepth: 1

   CustomNodes/LeafNodeIc120
   CustomNodes/LeafNodeZx200
   CustomNodes/LeafNodeMst110cr


Subatask Nodes
----------------
.. toctree::
   :maxdepth: 1

   CustomNodes/SubtaskIc120NavigateAnywhere
   CustomNodes/SubtaskIc120FollowWaypoints
   CustomNodes/SubtaskIc120NavigateThroughPoses
   CustomNodes/SubtaskIc120ReleaseSoil
   CustomNodes/SubtaskZx200ChangePose
   CustomNodes/SubtaskZx200ExcavateSimple
   CustomNodes/SubtaskZx200ExcavateSimplePlan
   CustomNodes/SubtaskZx200ReleaseSimple
   CustomNodes/SubtaskZx200NavigateAnywhere
   CustomNodes/SubtaskZx200FollowWaypoints
   CustomNodes/SubtaskZx200NavigateThroughPoses
   CustomNodes/SubtaskMst110crNavigateAnywhere
   CustomNodes/SubtaskMst110crFollowWaypoints
   CustomNodes/SubtaskMst110crNavigateThroughPoses
   CustomNodes/SubtaskMst110crReleaseSoil





Other Custom Nodes
========================================
Other Custom Nodesには以下のBTノードが存在します。
これらのノードはLeaf Node・Subtask Node等の末端のノードの実行順序を決定したり、Blackboardへの値の読み書きを行うノードが含まれています。

.. toctree::
   :maxdepth: 1

   CustomNodes/BlackboadrValueReaderMongo
   CustomNodes/ConditionalExpression
   CustomNodes/MongoValueWriter
   CustomNodes/SetLocalBlackboard
   CustomNodes/KeepRunningUntilFlgup




