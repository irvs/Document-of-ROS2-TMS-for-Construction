カスタムノードの概要
===================================

本章ではROS2-TMS for Constructionで独自に用意したBTノードについて紹介する。これらのノードにはLeaf NodeとSubtask Nodeという2種類のノードが大きく存在する。
Leaf NodeとSubtask Nodeの関係図を以下に示す。


1. Leaf Nodeにはmodel_name・record_name・subtask_nodeという3種類の入力ポートが存在する。このうち、model_nameとrecord_nameでデータベース上のパラメータデータを指定する。
2. Leaf NodeはROS2 Actionを介してsubtask_nameで指定したSubtask Nodeへと接続する。
3. Leaf NodeはROS2 Actionのgoalメッセージとしてmodel_nameとrecord_nameで指定したキー値をSubtask Nodeへと送る。
4. Subtask NodeはSubtask Nodeは2種類のキー値を受け取るとこれらをもとにデータベースを検索し、該当するものが見つかればデータを読み出してくる。
5. Subtask Nodeは読み出してきた値をもとに建設機械へ動作指令を送る。

なお、Subtask Nodeとは上記の通り、一部のパラメータを未定として実装した建機操作用のノードである。例えばバックホウの目標姿勢がパラメータ化されており、Subtask Nodeはこの値で指定された
姿勢へとモーションプラニングを行う機能を持っているとする。すると上記の図の場合、パラメータデータとしてデータベースから目標姿勢情報を読み出してきて、Subtask Nodeはこの値をもとに
建設機械をその姿勢へ移動させる。


なお、Leaf NodeはBTノードである一方でSubtask NodeはBTノードではなく、単なるROS2ノードである点に注意する。
このため、Groot上でタスクツリーを構築する際には末端がLeaf Nodeとなる。
LeafノードとSubtaskノードの一覧は以下に示すとおりである。

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


また、Leaf NodeとSubtask Node以外にも以下のCustom Nodeが存在する。


Other Custom Nodes
-----------
- :doc:`BlackboadrValueReaderMongo <CustomNodes/BlackboadrValueReaderMongo>`
- :doc:`ConditionalExpression <CustomNodes/ConditionalExpression>`
- :doc:`MongoValueWriter <CustomNodes/MongoValueWriter>`
- :doc:`SetLocalBlackboard <CustomNodes/SetLocalBlackboard>`
- :doc:`KeepRunningUntilFlgup <CustomNodes/KeepRunningUntilFlgup>`




