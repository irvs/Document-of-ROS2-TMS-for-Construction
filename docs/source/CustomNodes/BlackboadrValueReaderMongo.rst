BlackboadrValueReaderMongo
===================================

概要
-----------
MongoDB上のGlobal Blackboardから値を読み出してきて、Local Blackboardに格納するBTノードである。
従来、別々のタスク間で情報共有するためのメモリ領域は存在せず、本BTノードの実装を行った。

使用方法
-----------
- **model_name** : "ic120"と指定
- **record_name** : 接続するSubtask Nodeの仕様に合わせたパラメータデータのrecord_nameの値を指定
- **subtask_node** :  "subtask_ic120_follow_waypoints"と指定。

.. image:: ../images/SubtaskIc120FollowWaypoints.png
   :alt: SubtaskIc120FollowWaypoints
   :width: 400px
   :align: center  

.. raw:: html

   <br><br>


パラメータデータの仕様
--------------------

各配列の要素番号NはN個目のウェイポイントの値として指定

.. image:: ../images/DB_SubtaskFollowWaypoints.png
   :alt: DB_SubtaskFollowWaypoints
   :width: 300px
   :align: center  

※_id, model_name. description, record_name等の共通仕様は除外

サンプル
-----------

**動作** : Map座標基準のx軸方向1m, 2m, 3m地点を経由して移動。各地点ではbase_linkとmapの姿勢を合わせる。

.. image:: ../images/Sample_SubtaskIc120NavigateAnywhere.svg
   :alt: Sample_SubtaskIc120NavigateAnywhere
   :width: 400px
   :align: center  
