subtask_zx200_follow_waypoints_deg
===================================

概要
-----------
共通制御信号対応バックホウZX200をナビゲーション操作するSubtask Nodeの1つ。
バックホウの移動経路をNavigate Through Posesに従って指定する。
Navigate Through Posesではsubtask_zx200_follow_waypoints同様に
経路上の複数点に沿ったナビゲーションを行うものの、経路上の経由地点では位置合わせのみを
行う。
subtask_zx200_navigate_through_posesと機能は同じであるが、姿勢はdegreeを用いて指定する点が異なる。

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
- **subtask_node** :  "subtask_zx200_nvigate_through_poses_deg"と指定。

.. image:: ../images/SubtaskZx200NavigateTheoughPosesDeg.png
   :alt: SubtaskZx200NavigateTheoughPosesDeg
   :width: 400px
   :align: center  
  
.. raw:: html

.. raw:: html

   <br><br>

パラメータデータの仕様
-----------

各配列の要素番号NはN個目のウェイポイントの値として指定する。
姿勢はmap座標からみたbase_link座標の相対位置姿勢であり、位置はxyzのm基準、姿勢はdegreeで指定する。

.. image:: ../images/DB_SubtaskNavigateThroughPosesDeg.png
   :alt: DB_SubtaskNavigateThroughPosesDeg
   :width: 300px
   :align: center  

※_id, model_name. description, record_name等の共通仕様は除外

サンプル
-----------

**動作** : Map座標基準のx軸方向1m, 2m, 3m地点を経由して移動。各地点ではbase_linkとmapの姿勢を合わせる。

.. image:: ../images/Sample_SubtaskZx200NavigateThroughPosesDeg.svg
   :alt: Sample_SubtaskZx200NavigateThroughPosesDeg
   :width: 400px
   :align: center  
