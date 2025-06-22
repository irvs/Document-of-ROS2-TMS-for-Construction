subtask_zx200_change_pose
===================================

概要
-----------
共通制御信号対応バックホウZX200をマニピュレーション操作するSubtask Nodeの1つ。
バックホウの目標姿勢を指定し、現在姿勢からマニピュレーション操作を行う。

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
- **model_name** : "zx200"と指定
- **record_name** : 接続するSubtask Nodeの仕様に合わせたパラメータデータのrecord_nameの値を指定
- **subtask_node** :  "subtask_zx200_change_pose"と指定。

.. image:: ../images/SubtaskZx200ChangePose.png
   :alt: SubtaskZx200ChangePose
   :width: 400px
   :align: center  
  
.. raw:: html

.. raw:: html

   <br><br>

パラメータデータの仕様
-----------

ZX200を構成する各関節の角度をラジアン指定。

.. image:: ../images/DB_SubtaskChangePose.png
   :alt: DB_SubtaskChangePose
   :width: 400px
   :align: center

.. raw:: html

   <br><br>  

※_id, model_name. description, record_name等の共通仕様は除外。詳しくは :doc:`こちら <../DataBase>` へ。

サンプル
-----------

**動作** : 目標姿勢（swing_joint：-1.571[rad],boom_joint：-1.047[rad], arm_joint：2.443[rad], bucket_joint：-0.524[rad], bucket_end_joint：0[rad]）に向けて移動 

.. image:: ../images/Sample_SubtaskZx200ChangePose.svg
   :alt: Sample_SubtaskZx200ChangePose
   :width: 600px
   :align: center  
