subtask_mst110cr_navigate_anywhere_deg
===================================

概要
-----------
共通制御信号対応クローラダンプMST110CRをナビゲーション操作するSubtask Nodeの1つ。
ダンプの目標位置姿勢を指定し、それに向かってナビゲーションを行う。
subtask_mst110cr_navigate_anywhereと機能は同じであるが、姿勢はdegreeを用いて指定する点が異なる。

対応表
-----------

.. image:: ../images/対応表_simok_actok.png
   :alt: 対応表
   :width: 800px
   :align: center  

.. raw:: html

   <br><br>

使用方法
-----------
- **model_name** : "mst110cr"と指定
- **record_name** : 接続するSubtask Nodeの仕様に合わせたパラメータデータのrecord_nameの値を指定
- **subtask_node** :  "subtask_mst110cr_navigate_anywhere_deg"と指定。

.. image:: ../images/SubtaskMst110crNavigateAnywhereDeg.png
   :alt: SubtaskMst110crNavigateAnywhereDeg
   :width: 400px
   :align: center  
  
.. raw:: html

.. raw:: html

   <br><br>

パラメータデータの仕様
-----------

Map座標基準での目標位置姿勢を指定する。
姿勢はmap座標からみたbase_link座標の相対位置姿勢であり、位置はxyzのm基準、姿勢はdegreeで指定する。

.. image:: ../images/DB_SubtaskNavigateAnywhereDeg.png
   :alt: DB_SubtaskNavigateAnywhereDeg
   :width: 300px
   :align: center  

※_id, model_name. description, record_name等の共通仕様は除外

サンプル
-----------

**動作** : Map座標基準のx軸方向1m地点へ移動。目標地点ではbase_linkとmapの姿勢を合わせる。

.. image:: ../images/Sample_SubtaskMst110crNavigateAnywhereDeg.svg
   :alt: Sample_SubtaskMst110crNavigateAnywhereDeg
   :width: 400px
   :align: center  
