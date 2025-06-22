subtask_ic120_release_soil
===================================

概要
-----------
共通制御信号対応クローラダンプIC120を操作するSubtask Nodeの1つ。
ベッセルの目標放土角度を指定し、放土動作を行う。

対応表
-----------

.. image:: ../images/対応表_simnotok_actok.png
   :alt: 対応表
   :width: 800px
   :align: center  

.. raw:: html

   <br><br>

使用方法
-----------
- **model_name** : "ic120"と指定
- **record_name** : 接続するSubtask Nodeの仕様に合わせたパラメータデータのrecord_nameの値を指定
- **subtask_node** :  "subtask_ic120_release_soil"と指定。

.. image:: ../images/SubtaskIc120ReleaseSoil.png
   :alt: SubtaskIc120ReleaseSoil
   :width: 400px
   :align: center  
  
.. raw:: html

.. raw:: html

   <br><br>

パラメータデータの仕様
-----------

Map座標基準での目標位置姿勢を指定

.. image:: ../images/DB_SubtaskReleaseSoil.png
   :alt: DB_SubtaskReleaseSoil
   :width: 300px
   :align: center  

※_id, model_name. description, record_name等の共通仕様は除外

サンプル
-----------

**動作** : Map座標基準のx軸方向1m地点へ移動。目標地点ではbase_linkとmapの姿勢を合わせる。

.. image:: ../images/Sample_SubtaskIc120ReleaseSoil.svg
   :alt: Sample_SubtaskIc120ReleaseSoil
   :width: 400px
   :align: center  
