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

   .. raw:: html

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
- :doc:`LeafNodeIc120 <CustomNodes/LeafNodeIc120>`
- :doc:`LeafNodeZx200 <CustomNodes/LeafNodeZx200>`
- :doc:`LeafNodeMst110cr <CustomNodes/LeafNodeMst110cr>`


Subatask Nodes
-----------
- :doc:`subtask_ic120_navigate_anywhere <CustomNodes/SubtaskIc120NavigateAnywhere>`
- :doc:`subtask_ic120_navigate_anywhere_deg <CustomNodes/SubtaskIc120NavigateAnywhereDeg>`
- :doc:`subtask_ic120_follow_waypoints <CustomNodes/SubtaskIc120FollowWaypoints>`
- :doc:`subtask_ic120_follow_waypoints_deg <CustomNodes/SubtaskIc120FollowWaypointsDeg>`
- :doc:`subtask_ic120_navigate_through_poses <CustomNodes/SubtaskIc120NavigateThroughPoses>`
- :doc:`subtask_ic120_navigate_through_poses_deg <CustomNodes/SubtaskIc120NavigateThroughPosesDeg>`
- :doc:`subtask_ic120_release_soil <CustomNodes/SubtaskIc120ReleaseSoil>`
- :doc:`subtask_zx200_change_pose <CustomNodes/SubtaskZx200ChangePose>`
- :doc:`subtask_zx200_excavate_simple <CustomNodes/SubtaskZx200ExcavateSimple>`
- :doc:`subtask_zx200_excavate_simple_plan <CustomNodes/SubtaskZx200ExcavateSimplePlan>`
- :doc:`subtask_zx200_release_simple <CustomNodes/SubtaskZx200ReleaseSimple>`
- :doc:`subtask_zx200_navigate_anywhere <CustomNodes/SubtaskZx200NavigateAnywhere>`
- :doc:`subtask_zx200_navigate_anywhere_deg <CustomNodes/SubtaskZx200NavigateAnywhereDeg>`
- :doc:`subtask_zx200_follow_waypoints <CustomNodes/SubtaskZx200FollowWaypoints>`
- :doc:`subtask_zx200_follow_waypoints_deg <CustomNodes/SubtaskZx200FollowWaypointsDeg>`
- :doc:`subtask_mst110cr_navigate_anywhere <CustomNodes/SubtaskMst110crNavigateAnywhere>`
- :doc:`subtask_mst110cr_navigate_anywhere_deg <CustomNodes/SubtaskMst110crNavigateAnywhereDeg>`
- :doc:`subtask_mst110cr_navigate_through_poses <CustomNodes/SubtaskMst110crNavigateThroughPoses>`
- :doc:`subtask_mst110cr_navigate_through_poses_deg <CustomNodes/SubtaskMst110crNavigateThroughPosesDeg>`
- :doc:`subtask_mst110cr_release_soil <CustomNodes/SubtaskMst110crReleaseSoil>`





Other Custom Nodes
========================================
Other Custom Nodesには以下のBTノードが存在します。
これらのノードはLeaf Node・Subtask Node等の末端のノードの実行順序を決定したり、Blackboardへの値の読み書きを行うノードが含まれています。


Other Custom Nodes
-----------
- :doc:`BlackboadrValueReaderMongo <CustomNodes/BlackboadrValueReaderMongo>`
- :doc:`ConditionalExpression <CustomNodes/ConditionalExpression>`
- :doc:`MongoValueWriter <CustomNodes/MongoValueWriter>`
- :doc:`SetLocalBlackboard <CustomNodes/SetLocalBlackboard>`
- :doc:`KeepRunningUntilFlgup <CustomNodes/KeepRunningUntilFlgup>`




